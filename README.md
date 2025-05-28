# JuaJobs Activity Group 1
---

## Group Members and Roles

- **Belinda Belange Larose** : *Security Designer*

  As a team member of JuaJobs API Design project, I focused on key aspects of the API's foundation and security. In Resource Attribute Design, I helped create the detailed blueprints for important data like user profiles and job listings, making sure each piece of information had the right type and rules. As the Security Designer, I led the effort to build a safe way for users to log in using sessions and set up rules so people can only access what they need, like clients managing their own jobs. I also contributed to the Localization Strategy, thinking about how the API needs to handle different languages, money, and local rules to work well across various African markets.
- **Daniel Iryivuze** : *Endpoint Designer* & *User Experience Analyst*
- **Nagasaro Ghislaine** : *Documentation Specialist*
- **Nana Koramah Abeasi** : *API Architect*

  As part of the JuaJobs team, I contributed to the API architecture by expanding core resources with Payments, Notifications, and Support Tickets, and analyzing their ownership and lifecycle. I also defined HTTP status codes, error response formats, and versioning strategies to ensure clarity, consistency, and forward compatibility. Additionally, I proposed connectivity optimizations like batch operations, caching, and offline-first support to enhance performance in low-bandwidth African markets.

---

## 1. API Design Overview

### 1. Complete resource models with attributes and relationships
#### Resource Identification and Analysis
We expanded the JuaJobs resource model by adding Payments, Notifications, and Support Tickets to better support financial transactions, real-time updates, and issue resolution. We justified each based on business needs and mapped out how users interact with each resource over its lifecycle. This ensured that every resource has a clear owner and purpose within the platform’s ecosystem.
Link to document **[here](phase_1/resource_identification_&_analysis.md)**
### 2. Entity-relationship diagrams
### 3. Comprehensive endpoint catalog with HTTP methods
### 4. Authentication and authorization framework 

  We made a simple but safe way for users to log in using sessions stored in Redis. This helps us keep track of who is logged in and what they can do. We have three types of users (clients, workers, and admins) and each one can only do certain things in the app. For example, only clients can post jobs, and only workers can apply for jobs. We also made sure it works well even when the internet is slow. Link to document: For Authentication **[here](phase_4/authentication.md)** & For Authorization **[here](phase_4/authorization.md)**

### 5. Query parameter standards
### 6. Error handling strategy
We defined appropriate HTTP status codes for all major API responses and designed a standardized, user-friendly error format. Our error system distinguishes between validation, business logic, and system errors, making it easier for developers to troubleshoot. Each error message was written to be clear and actionable for API consumers.
Link to document **[here](phase_2/HTTP_status_codes.md)**
### 7. Versioning approach
We chose a URL-based versioning strategy (e.g., /api/v1/) to make API changes explicit and maintain clear structure over time. We documented how future versions would be rolled out and deprecated to preserve compatibility. Our lightweight change management framework helps plan for continuous improvement without breaking existing clients.
Link to document **[here](phase_2/versioning_strategy.md)**

---

## 2. OpenAPI/Swagger Specification File

This OpenAPI 3.0.3 specification defines the complete REST API for JuaJobs.The API provides comprehensive functionality for user management, job posting and discovery, application processing, and reviews.

The specification file includes 23 endpoints organized across five main resource categories: Users, Jobs, Applications, Reviews, and Authentication. It supports role-based access control with three user types (clients, freelancers, and administrators), implements *JWT-based authentication* with *Bearer tokens*, and provides detailed request/response schemas with validation rules.

Key features include **pagination** and **filtering** across list endpoints, comprehensive error handling with specific error codes and validation messages, and bidirectional relationships between core entities. The API is designed to support both *fixed-price* and *hourly-rate* projects, maintains user reputation through a 5-star rating/review system, and includes complete CRUD operations for all major resources.

The specification targets production deployment at ***api.juajobs.com/v1** with a staging environment available, and includes extensive documentation with request/response examples and detailed schema definitions for all data models.

**The Specification file can be found [here](phase_3/OpenAPI_file.yaml)**

_*Note that the deployment address used is fake_

---

## 3. Market Adaptation

### 1. Solutions for localization challenges
We thought about how our app needs to work in different countries in Africa. We are planning to support different languages like English, Swahili, and French, and also handle different money types and local dates and times. We also looked at specific things needed in countries like Kenya and Nigeria, and how to follow their local rules. Link to document **[here](phase_5/localization.md)**
### 2. Connectivity optimization strategies
We optimized the API for Africa’s low-bandwidth environments by proposing batch operations, caching strategies, and payload minimization techniques. We also accounted for mobile-first users with offline-first features that allow apps to sync when reconnected. These design decisions help ensure JuaJobs performs well even in areas with limited internet access.
Link to document **[here](phase_5/connectivity.md)**
### 3. Payment integration design
### 4. Mobile-first considerations

---

## 4. Presentation Material

- To Be Added

---
