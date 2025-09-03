---
title: 'Internship at ERIN Technologies (Summer 2025)'
date: 2025-08-20
permalink: /posts/2025/8/erininternship/
tags:
  - academic
---

**Email Platform Modernization**

This summer I spent 12 weeks as a Software Engineering Intern at ERIN Technologies, a referral-driven recruiting startup in Pittsburgh. My work centered on redesigning and implementing the company’s email notification system. What began as a fragmented collection of AWS Lambda functions turned into a consolidated, event-driven service capable of handling translation into eighteen languages, supporting multiple companies with custom branding, and providing much clearer visibility into delivery and engagement.

---

### Background

At the start of my internship, the email infrastructure was spread across roughly forty-five Lambda functions. Each generated HTML directly in code, often duplicating nearly identical logic with only minor variations. Some functions were triggered by DynamoDB streams, others by CRON schedules, and others by direct invocations through API Gateway.  

This setup worked, but it was brittle. Updating content required searching through code, editing inline HTML, and redeploying Lambdas. Internationalization was almost impossible to manage at scale. White-label customers required custom colors and logos, which often meant cloning functions and repeating work. Logging and analytics were limited, making it difficult to answer even basic support questions like “Did this candidate receive their referral notification?”  

My internship project aimed to overhaul this system by introducing a cleaner architecture with the following goals:

- **Simplify template management** by moving all email content into SendGrid dynamic templates.  
- **Support eighteen languages** with a scalable translation system.  
- **Consolidate redundant functions** into logical categories rather than one-off scripts.  
- **Improve visibility** through delivery logging and reporting.  
- **Introduce configuration controls** so individual companies could enable, disable, or customize their notifications without code changes.  
- **Lay the groundwork** for adding push and web notifications in the future.

---

### Approach and Design

I designed the new platform as an **event-driven pipeline** using AWS SNS and SQS. Instead of rendering HTML in code, upstream triggers now publish standardized events to an email topic. Each message includes the target recipients, the type of email, the originating company, the language code, and a dictionary of variables to be injected into the template.  

Downstream handlers consume these messages, fetch the correct SendGrid template, merge in the variables, and send the email. This design made the pipeline modular and easy to extend.  

To support translation and branding, I introduced two DynamoDB tables:

- **EmailContentTable** stores translated strings, keyed by company, language, and content key.  
- **EmailConfigTable** stores per-company mappings from event type to template ID, along with enable/disable flags and branding options.  

I also built a webhook consumer Lambda that subscribes to SendGrid events (delivered, dropped, bounced, blocked, opened). These logs are written back into DynamoDB so support staff and administrators have visibility into every notification.

---

### Example Message Payload

```python
{
  "to": ["recipient@example.com"],
  "event_id": "welcome_email",
  "company_id": "f683e9ae-99a7-4fba-84ad-07547b9491d6",
  "language_code": "en",
  "variables": {
    "job_name": "Software Engineer",
    "referrer_name": "Erin McSloth"
  }
}
```

This envelope standardizes all email events, making the pipeline predictable and easier to maintain.

Some design elements are shown below:

<p align="center">
  <img src="/assets/images/erin.png" alt="System Architecture Diagram" width="50%"/>
</p>

Please note that this diagram is only a snippet of the overall system since I cannot share full details of ERIN’s internal architecture. If you are interested in learning more about my project, feel free to reach out to me directly via email.

### Results and Deliverables

By the end of the summer, the system had transformed from dozens of brittle Lambdas into a cohesive notification service. Key outcomes included:

- **Code consolidation:** many redundant functions collapsed into just a handful of category-based handlers.  
- **Template clarity:** all email content lives in SendGrid, editable without touching Lambda code.  
- **Internationalization:** full support for eighteen languages, using variable substitution and translation keys.  
- **Visibility:** complete delivery logs, status tracking, and analytics hooks.  
- **Scalability:** SNS/SQS queueing provides natural buffering and throttling for bulk sends like job alerts.  
- **Flexibility:** per-company configuration makes it possible to adjust branding, enable/disable notifications, or swap templates without engineering involvement.  

---

### Reflections

From a professional standpoint, this project was a chance to **own a system from design to implementation**. I worked in TypeScript with AWS SAM for deployment, modeled DynamoDB schemas, wrote queue-driven Lambdas, and tested integrations with SendGrid. The experience gave me confidence not just in writing code, but in designing event architectures that need to scale and remain maintainable.  

What made the experience meaningful was the team. My mentor took time to walk me through AWS best practices and challenged me to think about the long-term consequences of design decisions. Design and support colleagues provided valuable feedback on how email visibility impacted their daily work. I’m grateful for the openness of the ERIN team, who made me feel like my work mattered to the product and to the customers using it.  

On a personal note, I came away from this internship with a deeper appreciation for the balance between flexibility and simplicity in software design. Building a platform that can support many companies, languages, and notification types without becoming overly complex required deliberate trade-offs. That lesson—thinking in terms of both developer experience and end-user reliability—will stay with me in future projects.  

---

### Internship Details

- **Duration:** 12 weeks, Summer 2025  
- **Role:** Software Engineering Intern  
- **Team:** Myself, a staff engineer mentor, and regular input from product and support  
- **Tech Stack:** AWS Lambda, SNS, SQS, DynamoDB, TypeScript, AWS SAM, SendGrid  
- **Skills:** System design, serverless architecture, internationalization, logging and observability, cross-team collaboration  


