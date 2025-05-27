# 📘 Developer Experience Documentation

---

## 1. Getting Started

**Base URL**  

```
https://api.juajobs.com/v1
```

**Required Headers**

```http
Content-Type: application/json
Authorization: Bearer <your_access_token>
```

**Recommended Tools**

- Postman or Insomnia for testing
- Curl or HTTPie for CLI
- Swagger UI for interactive API docs

---

## 2. Authentication Flows

### 🔐 Register

```http
POST /auth/register
```

**Request**

```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "name": "Jane Doe"
}
```

**Response**

```json
{
  "message": "Account created successfully",
  "token": "<access_token>",
  "refresh_token": "<refresh_token>"
}
```

### 🔐 Login

```http
POST /auth/login
```

**Request**

```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response**

```json
{
  "token": "<access_token>",
  "refresh_token": "<refresh_token>"
}
```

### 🔄 Refresh Token

```http
POST /auth/refresh
```

**Request**

```json
{
  "refresh_token": "<refresh_token>"
}
```

### 🚪 Logout

```http
POST /auth/logout
```

---

## 3. Interactive Examples

### 📄 List All Jobs

```bash
curl -X GET https://api.juajobs.com/v1/jobs \
  -H "Authorization: Bearer <access_token>"
```

### 📬 Apply to a Job

```bash
curl -X POST https://api.juajobs.com/v1/jobs/123/applications \
  -H "Authorization: Bearer <access_token>" \
  -d '{
    "cover_letter": "Excited to apply for this gig!",
    "portfolio_link": "https://portfolio.me"
  }'
```

### 🖼️ Edit User Profile Image

```bash
curl -X PATCH https://api.juajobs.com/v1/users/42 \
  -H "Authorization: Bearer <access_token>" \
  -d '{
    "profile_image": "https://cdn.example.com/user42.png"
  }'
```

---

## 4. Use-Case-Based Guides

### 🧑‍💼 Freelancer: Register & Apply

1. `POST /auth/register`
2. `POST /users` – Create profile
3. `GET /jobs` – Browse jobs
4. `POST /jobs/{id}/applications` – Apply
5. `GET /applications/{id}` – Check status

### 🧑‍💻 Client: Post Job & Hire

1. `POST /auth/register`
2. `POST /jobs` – Create job
3. `GET /jobs/{id}/applications` – Review
4. `PATCH /applications/{id}` – Accept applicant

### ⭐ Leave a Review

1. `POST /users/{user_id}/reviews`
2. `GET /users/{user_id}/reviews`

---
