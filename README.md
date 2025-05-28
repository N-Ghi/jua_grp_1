# JuaJobs Activity Group 1
---

## Group Members and Roles

- **Belinda Belange Larose** : *Security Designer*
- **Daniel Iryivuze** : *Endpoint Designer* & *User Experience Analyst*
- **Nagasaro Ghislaine** : *Documentation Specialist*
- **Nana Koramah Abeasi** : *API Architect*

---

## 1. API Design Overview

### 1. Complete resource models with attributes and relationships

summary
link to document

### 2. Entity-relationship diagrams

**Overview:**
The *ERD* defines the data structure for a JuaJobs, centered around the USERS entity, which includes both clients and workers. JOB_POSTINGS are created by clients and categorized under CATEGORIES, which support a hierarchical structure via parent_category_id. Workers have associated WORKER_PROFILES and can submit APPLICATIONS to job postings. Once a job is completed, it is recorded in JOB_COMPLETIONS, referencing both the job and the application. REVIEWS enable users to evaluate one another per job, distinguishing between reviewer and reviewee. PAYMENTS log transactions between users, tied to specific jobs. NOTIFICATIONS are user-specific and likely triggered by various events in the system. The schema establishes clear relationships between entities to support job creation, application, fulfillment, feedback, and compensation.

**Link to Diagram:** [ERD](phase_1/JuaJua_ERD.pdf)

### 3. Comprehensive endpoint catalog with HTTP methods

### 4. Authentication and authorization framework

### 5. Query parameter standards

### 6. Error handling strategy

### 7. Versioning approach

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
summary
link to document
### 2. Connectivity optimization strategies
### 3. Payment integration design
### 4. Mobile-first considerations

---

## 4. Presentation Material

- To Be Added

---
