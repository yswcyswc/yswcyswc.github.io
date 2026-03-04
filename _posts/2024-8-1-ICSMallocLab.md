---
title: "A Systems-Level Walkthrough of My 15-213 malloc Lab Allocator"
subtitle: "Struct-based blocks, footer elision, mini blocks, and segregated free lists"
date: 2024-08-1
permalink: /posts/2024/8/icsproject/
tags:
  - academic
---

### Academic Integrity Note (Read First)

This post describes *design decisions and allocator mechanics* from my 15-213 (Introduction to Computer Systems) malloc lab.  
By course academic integrity policy, I cannot share the full project source, benchmarks, traces, or any submission-ready implementation.  

I will only include **non-submission snippets**: tiny excerpts that illustrate concepts (bit packing, invariants, data layout) without reconstructing a complete allocator.

---

## What this project is

The malloc lab is about implementing a user-space dynamic memory allocator providing:

- `malloc(size_t)`
- `free(void*)`
- `realloc(void*, size_t)`
- `calloc(size_t, size_t)`

The allocator manages a simulated heap grown via `mem_sbrk`, and must satisfy:

- **Correctness**: return aligned payload pointers; no overlap; safe coalescing; valid block metadata.
- **Performance**: good **throughput** (ops/sec) and **utilization** (payload / heap footprint).

This lab forces you to reason about low-level representation: every byte matters, and every invariant needs to be enforceable.

---

## Goals and constraints I optimized for

My allocator design was guided by four constraints:

1. **16-byte alignment** on a 64-bit system.
2. Reduce metadata overhead to improve **utilization**.
3. Avoid linear scans of the whole heap during allocation to improve **throughput**.
4. Preserve coalescing capability even when removing some footers.

That led to a hybrid design:

- **Implicit heap order** (walk blocks by size in memory)
- **Explicit segregated free lists** (fast search for free space)
- **Footer elision** for allocated blocks (and some small blocks) to save space
- **Mini blocks** to handle tiny allocations efficiently

---

## Block model: what “a block” means

A heap block is the allocator’s unit of management. It includes:

- a **header** (always present)
- a **payload** (for allocated blocks)
- and for free blocks: list pointers + often a **footer**

The core trick is: a block carries enough information to locate the next block (`block + size`), and often enough information to locate the previous block (via a footer *or* via encoded flags).

### Header format (64-bit word)

I used a single 64-bit `header` word that contains:

- upper bits: block **size** (multiple of 16)
- lower 4 bits: flags (since alignment keeps them otherwise unused)

Flags I used:

- `alloc` bit: whether current block is allocated
- `prev_alloc` bit: whether previous block is allocated
- `prev_mini` bit: whether previous block is a “mini block” (special-case navigation)

Conceptually:

```
| size ... | prev_mini | prev_alloc | alloc |
```

This is the key to removing footers from some blocks while still supporting coalescing and `find_prev`.

---

## Why remove footers

Classic boundary-tag allocators put both header and footer in **every** block so you can jump backwards:

- next block address: `bp + size(bp)`
- previous block address: `bp - size(prev_footer)`

But footers cost space. Footers on **allocated** blocks are particularly wasteful because allocated blocks do not need to be in a free list, and their payload is what utilization measures.

So I removed footers from:

- allocated blocks
- some small blocks (mini policy)

This improves utilization but makes backward navigation harder.

To preserve correctness, I stored enough “previous block info” in the *current header*:
- `prev_alloc`: tells whether coalescing with prev is allowed
- `prev_mini`: tells how to compute prev address when the previous block is a mini block (which does not have a footer)

---

## Mini blocks: why they exist

Tiny allocations are common. If the minimum free block size is too big, small allocations waste memory (internal fragmentation). But if free blocks are too small, you cannot store the metadata required for an explicit free list (prev/next pointers).

So I used a “mini block” policy:

- **mini free blocks** have only:
  - header + next pointer
  - total size: **16 bytes**
- they do **not** store a `prev` pointer (no room)
- they also do **not** store a footer

This creates a tradeoff:

- Pro: supports tiny blocks without forcing large minimum sizes
- Con: removing a mini block from a free list is harder because you cannot directly go from node to its predecessor

I handled that by special-casing removal for mini blocks:
- to remove a mini block, traverse the bucket list to find its predecessor.

That makes mini removals slightly more expensive, but mini blocks are relatively constrained in size and bucket locality, so the cost stays manageable.

---

## Data structures: implicit heap + segregated free lists

### Implicit heap order

The heap is still laid out sequentially, so `find_next` is trivial:

- `next = (char*)block + block_size(block)`

That property is critical for:
- heap traversal in the heap checker
- writing the epilogue
- some forms of debugging output

### Segregated free list

Instead of one global explicit free list, I used **multiple buckets** by size range.

Benefits:
- allocation search time improves because you only traverse plausible candidates
- helps avoid scanning tiny blocks when requesting large blocks

I used an array of heads:

```c
static block_t *seg_group_list[10];
```

Each bucket holds free blocks in that approximate size class.

I computed bucket index roughly via repeated halving until size is small enough (log-ish behavior), with a cap into the largest bucket.

The list insertion policy was **LIFO** (add at head):
- fast `O(1)` insertion
- better temporal locality (recent frees get reused quickly)

---

## Allocation path: malloc

The high-level plan:

1. Normalize requested size into an aligned block size:
   - add header overhead
   - round up to 16 bytes
   - enforce minimum block size

2. Find a free block in the segregated lists.

3. If none exists, extend the heap via `mem_sbrk`.

4. Remove the chosen free block from its bucket list.

5. Mark allocated, update neighbor header flags.

6. Split if the remainder is large enough to form a valid free block.

### Size normalization

The key idea is making sure every payload pointer returned by `malloc` is aligned, and that every block respects the minimum metadata requirements.

Pseudo-snippet:

```c
asize = round_up(requested + header_size, 16);
asize = max(asize, min_block_size);
```

---

## Finding a fit: best-fit with a throughput cap

This is the most “performance knob” part of the allocator.

- **First-fit** is fast but can fragment.
- **Best-fit** can reduce fragmentation but is slow if you scan too much.

I used a hybrid:

- search within starting bucket and above
- keep track of the best candidate found so far
- stop scanning after `hardlimit` candidates examined

This is essentially “bounded best-fit”.

Design rationale:
- best-fit *tends* to improve utilization by reducing leftover slack
- but scanning too much kills throughput
- the cap keeps worst-case behavior under control

This is explicitly tunable:
- small `hardlimit` favors speed
- larger `hardlimit` favors utilization

---

## Splitting: controlling internal fragmentation

After finding a block, if it is larger than needed, splitting prevents wasting the tail as internal fragmentation.

Mechanics:

- write allocated header for the front portion
- create a new free block in the remainder
- write the remainder’s footer if policy requires it
- add remainder to segregated list
- update the next block’s `prev_alloc` and `prev_mini` bits

The subtlety is correctness across all cases:
- remainder must be >= minimum free block size
- flags must match reality (especially because some blocks lack footers)

Splitting is where many allocators break invariants if they forget to update the header bits of the block that follows the split.

---

## Free path: free + coalesce

### Free

To free:

1. Convert payload pointer to block header pointer.
2. Mark header as free.
3. Write a footer if this block is not a mini-block policy case.
4. Coalesce with adjacent free blocks.
5. Insert final coalesced block into appropriate segregated bucket.
6. Update the next block’s header flags (`prev_alloc`, `prev_mini`).

### Coalescing

Coalescing reduces external fragmentation by merging adjacent free blocks into a larger free block.

Classic 4 cases:

1. prev allocated, next allocated: no merge
2. prev allocated, next free: merge with next
3. prev free, next allocated: merge with prev
4. prev free, next free: merge with both

In my design, case detection uses:

- next allocation bit from next header
- previous allocation bit stored in current header (`prev_alloc`)
- previous mini bit stored in current header (`prev_mini`) to compute prev location

Critical operations during coalescing:

- remove any neighbor free blocks being merged from their segregated lists
- compute new total size
- write new header (and footer if needed)
- update next block’s header flags to reflect the new previous-block status

This “update-next” step is non-negotiable because in a footerless design, the next block relies on the encoded `prev_alloc` / `prev_mini` bits for future correctness.

---

## Heap extension: extend_heap

When no free block can satisfy a request:

- extend the heap by `max(asize, chunksize)` (chunked growth)
- create a new free block in the new region
- write a new epilogue at the end
- coalesce in case the previous terminal block was free

Chunking is a performance technique:
- reduces `sbrk` frequency
- amortizes extension overhead

---

## realloc and calloc: correctness-first approach

### realloc

I implemented realloc as:

1. if `ptr == NULL`: behave like malloc
2. if `size == 0`: free and return NULL
3. else:
   - allocate new block
   - `memcpy` min(old_payload_size, new_size)
   - free old block

This is not the most optimal realloc (in-place growth is possible), but it is reliable and passes functional correctness. It also demonstrates understanding of the semantics.

### calloc

calloc is:

- compute total bytes = elements * size
- check overflow
- malloc(total)
- memset to 0

Overflow checks matter because `elements * size` can wrap around and create a too-small allocation with dangerous writes.

---

## Debugging: what my heap checker enforces

The heap checker is where you show you understand allocator invariants.

My checker validates:

### Block-level invariants (implicit heap)

- prologue and epilogue correctness
- block sizes are multiples of 16
- payload pointers are aligned
- each block address is within heap bounds
- no consecutive free blocks remain uncoalesced
- header size field matches computed traversal size
- `prev_alloc` bit matches the actual allocation of the previous block (where applicable)

### Free-list invariants (segregated lists)

- number of free blocks in seg lists matches free blocks in heap traversal
- blocks are in the correct bucket by size class
- pointer integrity:
  - for non-mini blocks: `next->prev == current`
- no cycles (tortoise-hare)
- every free-list pointer lies inside heap bounds

This is the part recruiters care about, even more than the specific data structure: it shows discipline in systems correctness.

---

## The key design lesson: metadata is a budget

This lab forced a concrete mindset shift:

- Every convenience feature in an allocator (footers everywhere, doubly-linked lists everywhere) is paid for in bytes.
- Every byte in metadata is a byte not usable by payload.
- Utilization is not an abstract metric; it is literally a consequence of representation choices.

Removing footers improves utilization, but only if you redesign how you recover the necessary information for coalescing and navigation. That is why the `prev_alloc` and `prev_mini` bits exist: they are a compact way to retain safety without paying a full footer on every block.

---

## Summary (what this demonstrates)

This project demonstrates:

- understanding of heap layout, alignment, and pointer arithmetic
- bit packing and low-level representation choices
- explicit vs implicit free list tradeoffs
- fragmentation tradeoffs (internal vs external)
- algorithmic tradeoffs (best-fit vs first-fit vs bounded search)
- disciplined invariant checking and debugging tooling
