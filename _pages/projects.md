---
permalink: /projects/
title: "Projects"
author_profile: true
---
### [Email Notification Service (ERIN)](https://yswcyswc.github.io/posts/2025/8/erininternship/)
**Node.js, AWS Lambda, DynamoDB, SendGrid**

Developed a serverless email notification system for a referral platform using event-driven AWS Lambda processors. Refactored legacy email workflows into modular processors with SendGrid templates, dynamic content rendering, and multi-language support.

### [Scalable Robot–Target Interception Planner](https://github.com/yswcyswc/Scalable-Robot-Target-Interception-Planner)
**C++**

Designed a robot planning system to intercept a moving target on large weighted 2D costmaps while performing collision checking and trajectory alignment. Scaled planning to maps with ~4.2M cells and ~5.3K time steps while reducing memory usage by ~55% and planning time by ~60–70% relative to a baseline time-augmented A* approach.

---

### [Order Management System](https://github.com/yswcyswc/Order-Management-System)
**Ruby on Rails, React**

Developed a full-stack order management application with a Rails backend and React frontend supporting authentication, role-based authorization, and a relational database schema. Built RESTful APIs and optimized ActiveRecord queries with `fastjsonapi` serialization for efficient client–server data exchange.

---

### [High-DoF Robotic Arm Motion Planning](https://github.com/yswcyswc/Sampling-Based-Planning-in-Manipulator-Configuration-Space)
**C++**

Implemented sampling-based motion planning algorithms (**RRT-Connect and PRM**) to generate collision-free trajectories for a high-degree-of-freedom planar robotic arm. Benchmarked planning time, path cost, and success rate across randomized start–goal configurations under a **<5 second planning constraint**.

---

### [Dynamic Memory Allocator](https://yswcyswc.github.io/posts/2024/8/icsproject/)
**C**

Implemented a dynamic memory allocator supporting `malloc`, `free`, `realloc`, and `calloc`. Improved memory utilization by ~17% using segregated free lists and more efficient block management to reduce fragmentation.

---

### [MazeRunner (Procedural Maze Game)](https://github.com/yswcyswc/15112TermProject)
**Python**

Built an interactive maze exploration game using an MVC-style architecture with `cmu_112_graphics`. Implemented procedural maze generation using DFS backtracking and Prim’s algorithm to create randomized maze layouts with different difficulty levels.