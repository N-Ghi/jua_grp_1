# JuaJobs API Resource Relationships Analysis

---

## 1. Resource Relationship Mapping with Cardinality

### Users ↔ Job Postings

- **Relationship:** One-to-Many (Client creates Job Postings)  
- **Cardinality:** 1:N  
- **Description:** A client (user with `role='client'`) can create multiple job postings, but each job posting belongs to exactly one client  
- **Foreign Key:** `job_postings.client_id → users.user_id`  
- **Referential Integrity:** `CASCADE` on delete (when client deleted, their job postings are also deleted)  

### Users ↔ Worker Profiles

- **Relationship:** One-to-One (Worker has Profile)  
- **Cardinality:** 1:1  
- **Description:** Each worker (user with `role='worker'`) has exactly one worker profile  
- **Foreign Key:** `worker_profiles.worker_id → users.user_id`  
- **Referential Integrity:** `CASCADE` on delete  

### Users ↔ Applications

- **Relationship:** One-to-Many (Worker submits Applications)  
- **Cardinality:** 1:N  
- **Description:** A worker can submit multiple applications, but each application belongs to one worker  
- **Foreign Key:** `applications.worker_id → users.user_id`  
- **Referential Integrity:** `CASCADE` on delete  

### Job Postings ↔ Applications

- **Relationship:** One-to-Many (Job receives Applications)  
- **Cardinality:** 1:N  
- **Description:** Each job posting can receive multiple applications, but each application is for one specific job  
- **Foreign Key:** `applications.job_id → job_postings.job_id`  
- **Referential Integrity:** `CASCADE` on delete  

### Applications ↔ Job Completions

- **Relationship:** One-to-One (Application leads to Completion)  
- **Cardinality:** 1:0..1  
- **Description:** An accepted application can result in one job completion (optional)  
- **Foreign Key:** `job_completions.application_id → applications.application_id`  
- **Referential Integrity:** `SET NULL` on delete  

### Users ↔ Reviews (Bidirectional)

- **Relationship:** Many-to-Many (Users give and receive Reviews)  
- **Cardinality:** M:N  
- **Description:** Users can give multiple reviews and receive multiple reviews  
- **Foreign Keys:**  
  - `reviews.reviewer_id → users.user_id`  
  - `reviews.reviewee_id → users.user_id`  
- **Referential Integrity:** `CASCADE` on delete for reviewer, `SET NULL` for reviewee  

### Users ↔ Payments (Bidirectional)

- **Relationship:** Many-to-Many (Users pay and receive payments)  
- **Cardinality:** M:N  
- **Description:** Users can make multiple payments and receive multiple payments  
- **Foreign Keys:**  
  - `payments.payer_id → users.user_id`  
  - `payments.payee_id → users.user_id`  
- **Referential Integrity:** `RESTRICT` on delete (prevent deletion of users with payment history)  

### Job Postings ↔ Payments

- **Relationship:** One-to-Many (Job triggers Payments)  
- **Cardinality:** 1:N  
- **Description:** A job can have multiple payments (deposits, final payments, refunds)  
- **Foreign Key:** `payments.job_id → job_postings.job_id`  
- **Referential Integrity:** `CASCADE` on delete  

### Users ↔ Notifications

- **Relationship:** One-to-Many (User receives Notifications)  
- **Cardinality:** 1:N  
- **Description:** A user can receive multiple notifications  
- **Foreign Key:** `notifications.user_id → users.user_id`  
- **Referential Integrity:** `CASCADE` on delete  

### Categories ↔ Job Postings

- **Relationship:** One-to-Many (Category contains Job Postings)  
- **Cardinality:** 1:N  
- **Description:** Each job posting belongs to one category, but categories can contain multiple jobs  
- **Foreign Key:** `job_postings.category_id → categories.category_id`  
- **Referential Integrity:** `RESTRICT` on delete  

### Categories ↔ Categories (Self-Referential)

- **Relationship:** One-to-Many (Parent-Child Categories)  
- **Cardinality:** 1:N  
- **Description:** Categories can have subcategories (hierarchical structure)  
- **Foreign Key:** `categories.parent_category_id → categories.category_id`  
- **Referential Integrity:** `SET NULL` on delete  

---

## 2. Entity Relationship Diagram

[Entity Relationship Diagram.](phase_1/JuaJua_ERD.pdf)
**For a cleaner look, it only shows primary and foreign keys*

---

## 3. Parent-Child Relationships and Containment Hierarchies

### Primary Hierarchies

**Level 1: Users (Root Entity)**  
Users (Root)
├── Job Postings (Client-owned)
├── Worker Profiles (Worker-owned)
├── Applications (Worker-submitted)
├── Reviews (User-generated)
├── Payments (User transactions)
└── Notifications (User alerts)

**Level 2: Job-Centric Hierarchy**  

Job Postings
├── Applications (Multiple per job)
│ └── Job Completions (One per accepted application)
├── Reviews (Generated after completion)
└── Payments (Financial transactions)

**Level 3: Category Hierarchy**  
Categories
├── Subcategories (Self-referential hierarchy)
│ └── Further subcategories...
└── Job Postings (Classified by category)

**Level 4: System-Generated Resources**  
System Events
├── Notifications (Event-triggered alerts)
└── Payments (Transaction records)

### Containment Rules

- Job Postings are contained within Client Users and Categories  
- Worker Profiles are contained within Worker Users  
- Applications bridge Workers and Job Postings  
- Reviews are contained within completed Job Postings  
- Job Completions are contained within successful Applications  
- Payments are contained within Job Postings and linked to Users  
- Notifications are contained within Users and triggered by system events  
- Categories can contain Subcategories (hierarchical structure)  

---

## 4. Resource Dependencies and Referential Integrity Requirements

### Creation Dependencies (Required Order)

- Categories must exist before Job Postings (system-seeded data)  
- Users must exist before any user-owned resource  
- Job Postings require existing Client User and Category  
- Worker Profiles require existing Worker User  
- Applications require existing Worker and Job Posting  
- Job Completions require existing Application (accepted status)  
- Reviews require existing Job Completion  
- Payments require existing Job and Users (payer/payee)  
- Notifications can be created independently (system-generated)  

### Deletion Cascades

**User Deletion Impact:**  

├── If Client:
│ ├── Delete all Job Postings
│ ├── → Delete Applications (cascade)
│ ├── → Update Job Completions (set null)
│ ├── → Restrict if has payment history
│ └── → Delete notifications (cascade)
├── If Worker:
│ ├── Delete Worker Profile
│ ├── Delete Applications
│ ├── → Update Reviews (set reviewee null)
│ ├── → Restrict if has payment history
│ └── → Delete notifications (cascade)
└── All Reviews (as reviewer): Delete, (as reviewee): Set reviewee_id to NULL

**Category Deletion Impact:**  
├── Restrict if any Job Postings exist in category
├── Set parent_category_id to NULL for child categories
└── Admin must reassign jobs before deletion

**Job Posting Deletion Impact:**  
├── Delete all Applications (cascade)
├── Delete all Payments (cascade)
├── Delete all Reviews (cascade)
└── Delete Job Completion (cascade)

## Business Rule Constraints

- **User Role Validation:** Only clients can create job postings, only workers can have profiles  
- **Application Status Flow:** `draft → submitted → reviewed → accepted/rejected → completed`  
- **Review Eligibility:** Only after job completion with verified application  
- **Payment Flow:** Linked to job completion and user verification  
- **Notification Triggers:** System events create notifications automatically  
- **Category Hierarchy:** Prevent circular parent-child relationships  

## Data Integrity Rules

**Unique Constraints:**  

- One worker profile per worker user  
- One application per worker per job posting  
- One completion per application  
- Unique category names within same parent level  

**Check Constraints:**

- `budget_min ≤ budget_max` in job postings  
- Rating values between 1-5  
- Valid status transitions in applications  
- Payment amounts must be positive  
- Category hierarchy depth limits  

**Foreign Key Constraints:**  

- All foreign keys must reference existing records  
- Payment users must have different payer/payee IDs  
- Categories cannot reference themselves as parents  
- Prevent orphaned records through proper `CASCADE`/`RESTRICT`/`SET NULL` rules  

## Financial Integrity

- **Payment Immutability:** Payments cannot be deleted, only status updated  
- **Audit Trail:** All payment modifications logged with timestamps  
- **User Balance:** Calculated from payment history, not stored directly  
- **Refund Handling:** New payment records with negative amounts 

---
