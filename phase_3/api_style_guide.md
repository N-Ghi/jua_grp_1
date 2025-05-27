
# JuaJobs API Style Guide

---

## 1. Naming Conventions for Resources, Endpoints, and Parameters

### Resource Naming Standards

- **Resources (Collections):** `/users`, `/jobs`, `/applications`, `/reviews`  
- **Individual Resources:** `/users/{user_id}`, `/jobs/{job_id}`  
- **Nested Resources:** `/jobs/{job_id}/applications`  
- **Multi-word Resources:** `/job-categories`, `/skill-tags`, `/user-profiles`  

### Endpoint Naming Conventions

- Always use plural nouns for collections  
- Use kebab-case for multi-word resources  
- Avoid verbs in URLs (use HTTP methods instead)  
- Maintain hierarchy for related resources  

### Parameter Naming Standards

| **Parameter Type** | **Convention** | **Examples** |
|--------------------|----------------|--------------|
| Path Parameters    | snake_case with _id | `{user_id}`, `{job_id}`, `{application_id}` |
| Query Parameters   | snake_case     | `sort_by`, `created_at`, `hourly_rate` |
| Request Body Fields| snake_case     | `first_name`, `job_title`, `cover_letter` |
| Response Fields    | snake_case     | `user_id`, `created_at`, `is_active` |

### Standard Query Parameter Patterns

#### Pagination

```
?page=1&limit=20&offset=0
```

#### Sorting

```
?sort_by=created_at&order=desc
?sort_by=title&order=asc
```

#### Filtering

```
?status=active&category=web-development
?min_budget=5000&max_budget=50000
?location=nairobi&skill=react
```

#### Search

```
?search=frontend+developer
?q=react+javascript
```

#### Field Selection

```
?fields=id,title,budget,deadline
?include=user,applications
```

---

## 2. Formatting Standards for Dates, Currencies, and Localized Data

### Date and Time Formatting

```json
{
  "created_at": "2024-05-27T14:30:00Z",
  "updated_at": "2024-05-27T14:30:00Z",
  "deadline": "2024-06-15T23:59:59Z",
  "birth_date": "1990-05-27",
  "local_time": "2024-05-27T16:30:00+03:00"
}
```

**Date Standards:**  
- Always use ISO 8601 format  
- Store all timestamps in UTC (use Z suffix)  
- Date-only fields: `YYYY-MM-DD` format  
- Include timezone when local time matters  

### Currency Formatting

```json
{
  "budget": {
    "amount": 150000,
    "currency": "RWF",
    "display": "RWF 150,000"
  },
  "hourly_rate": {
    "amount": 2500, 
    "currency": "RWF",
    "display": "RWF 2500/hour"
  },
  "salary_range": {
    "min_amount": 500000,
    "max_amount": 1000000,
    "currency": "RWF",
    "display": "RWF 500,000 - 1,000,000"
  }
}
```

**Currency Standards:**

- Store amounts as integers in smallest currency unit (francs, cents)  
- Use ISO 4217 codes (RWF, USD, EUR)  
- Always include currency code with amounts  
- Provide display strings for UI consumption  
- Support multiple currencies for international expansion  

### Localized Data Formatting

```json
{
  "user": {
    "location": {
      "country": "RWA",
      "country_name": "Rwanda",
      "city": "Kigali",
      "region": "Kigali City",
      "timezone": "Africa/Kigali"
    },
    "language": "rw-RW",
    "preferred_language": "en-RW",
    "phone": "+250701234567",
    "locale": "en_RW"
  }
}
```

**Localization Standards:**

- Country codes: ISO 3166-1 alpha-2 (KE, UG, TZ)  
- Language codes: IETF language tags (en-KE, sw-KE)  
- Phone numbers: E.164 format starting with +  
- Timezones: IANA timezone database names  
- Locale codes: Language_Country format for formatting rules  

---

## 3. Documentation Templates for Consistency

### Endpoint Documentation Template

```markdown
### [HTTP METHOD] [ENDPOINT URL]

**Description:** Brief description of what this endpoint does

**Authentication:** Required/Optional (specify token type)

**Parameters:**
- **Path Parameters:**
  - `param_name` (type, required/optional): Description
- **Query Parameters:**  
  - `param_name` (type, required/optional): Description
- **Request Body:** (for POST/PUT/PATCH)

**Request Example:**
```json
{
  "field_name": "value",
  "another_field": 123
}
```

**Responses:**  
200 OK: Success description  
400 Bad Request: Validation errors  
401 Unauthorized: Authentication required  
404 Not Found: Resource not found  

**Success Response Example:**

```json
{
  "success": true,
  "data": {
    // Response data structure
  },
  "meta": {
    "request_id": "req_abc123",
    "timestamp": "2024-05-27T14:30:00Z"
  }
}
```

**cURL Example:**

```bash
curl -X POST "https://api.juajobs.com/v1/endpoint" \
  -H "Authorization: Bearer token" \
  -H "Content-Type: application/json" \
  -d '{"field": "value"}'
```

```

### API Response Template

```json
{
  "success": boolean,
  "data": {
    // Main response data or null on error
  },
  "meta": {
    "pagination": {
      "current_page": 1,
      "per_page": 20,
      "total": 150,
      "total_pages": 8,
      "has_next": true,
      "has_previous": false
    },
    "request_id": "req_abc123",
    "timestamp": "2024-05-27T14:30:00Z",
    "api_version": "v1"
  },
  "errors": [
    {
      "code": "ERROR_CODE",
      "message": "Human readable message",
      "field": "field_name",
      "details": {}
    }
  ]
}
```

### Error Response Template

```json
{
  "success": false,
  "data": null,
  "errors": [
    {
      "code": "VALIDATION_ERROR",
      "message": "The email field is required",
      "field": "email"
    },
    {
      "code": "INVALID_FORMAT",
      "message": "Phone number must be in E.164 format",
      "field": "phone"
    }
  ],
  "meta": {
    "request_id": "req_def456",
    "timestamp": "2024-05-27T14:30:00Z"
  }
}
```

### Resource Schema Template

```yaml
JobListing:
  type: object
  required:
    - title
    - description
    - budget
    - client_id
  properties:
    id:
      type: integer
      description: Unique job identifier
      example: 123
    title:
      type: string
      maxLength: 100
      description: Job title
      example: "Frontend Developer for E-commerce Site"
    description:
      type: string
      maxLength: 2000
      description: Detailed job description
    budget:
      type: object
      properties:
        amount:
          type: integer
          description: Budget amount in cents
        currency:
          type: string
          description: ISO 4217 currency code
        display:
          type: string
          description: Formatted display string
    status:
      type: string
      enum: [draft, active, paused, completed, cancelled]
      description: Current job status
    created_at:
      type: string
      format: date-time
      description: Job creation timestamp
      example: "2024-05-27T14:30:00Z"
```

---

## 4. Guidelines for Future API Extensions

### Versioning Strategy

- URL Versioning:  
  - `/v1/users`  
  - `/v1/jobs`  
  - `/v1/applications`  

### Backward Compatibility Rules

**Safe Changes (Non-Breaking)**  
✅ Can be added to existing version:  
- Adding new optional fields to responses  
- Adding new optional request parameters  
- Adding new endpoints  
- Adding new enum values (with sensible defaults)  
- Making required fields optional  
- Adding new HTTP methods to existing endpoints  

**Breaking Changes (Require New Version)**  
❌ Require version increment:  
- Removing fields from responses  
- Making optional fields required  
- Changing field data types  
- Renaming fields or endpoints  
- Removing endpoints  
- Changing URL structure  
- Removing enum values  

### Extension Patterns

**Adding New Fields Safely**

```json
// Version 1.0 Response
{
  "user": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com"
  }
}

// Version 1.1 Response (backward compatible)
{
  "user": {
    "id": 123,
    "name": "John Doe",
    "email": "john@example.com",
    "verification_status": "verified",
    "profile_completion": 85
  }
}
```

### Feature Flags for Gradual Rollouts

```http
GET /users/123?features=enhanced_profile,skill_matching
```

```json
{
  "success": true,
  "data": {
    // User data with enhanced features
  },
  "meta": {
    "active_features": ["enhanced_profile", "skill_matching"],
    "available_features": ["enhanced_profile", "skill_matching", "ai_recommendations"]
  }
}
```

### Deprecation Strategy

- Announce deprecation 6 months before removal  
- Add deprecation headers to responses:  
  - `Deprecation: true`  
  - `Sunset: 2025-06-01T00:00:00Z`  
- Support old version for minimum 12 months  
- Provide migration guide with examples  
- Offer migration tools when possible  

### API Evolution Guidelines

- Plan for growth: Design resources with future fields in mind  
- Use consistent patterns: Follow established conventions for new endpoints  
- Maintain documentation: Update docs with every change  
- Test compatibility: Ensure old clients continue working  
- Monitor usage: Track which endpoints/versions are being used  
- Communicate changes: Notify developers of upcoming changes  

### Future-Proofing Checklist

- Resource names are generic enough for expansion  
- Response structure allows for additional metadata  
- Error codes are specific and actionable  
- Pagination supports different strategies  
- Search supports multiple query types  
- Filtering supports complex conditions  
- Sorting supports multiple fields  
- Authentication supports multiple methods  

---
