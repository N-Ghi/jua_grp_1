# API Endpoint Design

## 1. Comprehensive Endpoint List Following RESTful Conventions

We have designed our API using REST (Representational State Transfer), a standard architectural style for scalable and predictable web APIs.

### Key Concepts

- **Resources** are represented by URLs (e.g., users, jobs).
- **Actions** (Create, Read, Update, Delete) are mapped to HTTP methods.

### Key Resources

We have the following core resources in JuaJobs:

- **Users** – Profiles of workers and clients  
- **Jobs** – Gigs posted by clients  
- **Applications** – Submissions by workers to gigs  
- **Reviews** – Feedback left for users  
- **Authentication** – Account management and security  

---

### USERS

| Method | Endpoint           | Description                              |
|--------|--------------------|------------------------------------------|
| GET    | /users             | Fetch all users (admin or freelancer list) |
| POST   | /users             | Register a new user                      |
| GET    | /users/{id}        | View a user’s profile                    |
| PUT    | /users/{id}        | Replace all fields in a profile          |
| PATCH  | /users/{id}        | Update part of a user’s profile          |
| DELETE | /users/{id}        | Delete a user account                    |

---

### JOBS

| Method | Endpoint           | Description                              |
|--------|--------------------|------------------------------------------|
| GET    | /jobs              | List all job listings                    |
| POST   | /jobs              | Post a new gig                           |
| GET    | /jobs/{id}         | View details of a specific job           |
| PUT    | /jobs/{id}         | Replace all fields in a job              |
| PATCH  | /jobs/{id}         | Update selected fields (e.g., budget)    |
| DELETE | /jobs/{id}         | Delete a job listing                     |

---

### APPLICATIONS

Applications are job-specific, so we’ve used nested routes for clarity.

| Method | Endpoint                              | Description                          |
|--------|----------------------------------------|--------------------------------------|
| POST   | /jobs/{job_id}/applications           | Apply for a job                      |
| GET    | /jobs/{job_id}/applications           | View all applications for a job      |
| GET    | /applications/{id}                    | View a specific application          |
| PATCH  | /applications/{id}                    | Update application status            |
| DELETE | /applications/{id}                    | Withdraw an application              |

---

### REVIEWS

Reviews are tied to users:

| Method | Endpoint                         | Description                       |
|--------|----------------------------------|-----------------------------------|
| POST   | /users/{user_id}/reviews         | Submit a review for a user        |
| GET    | /users/{user_id}/reviews         | Get all reviews for a user        |
| PATCH  | /reviews/{id}                    | Edit a review                     |
| DELETE | /reviews/{id}                    | Delete a review                   |

---

### AUTHENTICATION

| Method | Endpoint             | Description                                 |
|--------|----------------------|---------------------------------------------|
| POST   | /auth/register       | Register a new account                      |
| POST   | /auth/login          | Log in and receive a token                  |
| POST   | /auth/logout         | Invalidate the current session/token        |
| POST   | /auth/refresh        | Get a new access token using refresh token  |

---

## 2. HTTP Methods with Justification

We have carefully mapped HTTP methods to their proper use cases:

| Method | Purpose              | Example Endpoint         | Justification                        |
|--------|----------------------|---------------------------|--------------------------------------|
| GET    | Retrieve data        | `/jobs`, `/users/5`       | Safe, read-only                      |
| POST   | Create resource      | `/jobs`, `/applications`  | Used for submitting new data         |
| PUT    | Replace resource     | `/users/5`                | Full update of a resource            |
| PATCH  | Modify resource      | `/jobs/5`                 | Partial update                       |
| DELETE | Remove resource      | `/applications/5`         | Remove unwanted or expired data      |

---

## 3. URL Pattern Design & Hierarchy

We have established consistent and semantic URL design for clarity and scalability:

- **Plural nouns** for collections: `/users`, `/jobs`, `/applications`
- **Nested routes** to reflect relationships:

```http
/jobs/{job_id}/applications
````

> Represents all applications for a specific job.

* **Avoided action-based routes** (e.g., `/getUser`). Instead, we rely on HTTP methods:

| Action         | Correct Usage       |
| -------------- | ------------------- |
| Get user       | `GET /users`        |
| Create user    | `POST /users`       |
| Update profile | `PATCH /users/{id}` |

---

## 4. Consistent Naming Conventions

We have adopted the following naming practices across all endpoints and query patterns:

* ✅ Use **kebab-case** for multi-word routes: `/job-categories`, `/user-profiles`
* ✅ Always use **plural nouns** for resource collections: `/users`, not `/user`
* ✅ Maintain consistent query parameter syntax:

```http
?sort_by=created_at&order=desc
```