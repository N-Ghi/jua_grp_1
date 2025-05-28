# API Query Parameters & Filtering

## 1. Filtering, Sorting, and Pagination

We have implemented standard query parameters to handle large datasets such as thousands of jobs or users.

### Filtering

We allow narrowing down results using specific field-based filters:

```http
GET /jobs?location=nairobi&category=plumbing
````

* Multiple filters can be combined.

### Sorting

We support sorting results by date, price, rating, and more:

```http
GET /jobs?sort_by=created_at&order=desc
```

* `sort_by`: Field to sort by
* `order`: `asc` or `desc`

### Pagination

We break down results into pages to enhance performance:

```http
GET /jobs?page=2&limit=20
```

| Parameter | Meaning                      |
| --------- | ---------------------------- |
| `page`    | Which set of results to show |
| `limit`   | Number of items per page     |

**Best Practice:**

* Default: `limit=10`
* Max: `limit=100`

---

## 2. Field Selection (Sparse Fieldsets)

We allow clients to request only the fields they need:

```http
GET /users/5?fields=name,skills,rating
```

### Benefits

* Speeds up responses
* Saves bandwidth—especially useful for mobile devices

---

## 3. Complex Data Queries

### Full-text Search

We support full-text search across job titles and descriptions:

```http
GET /jobs?search=electrician
```

> Our backend implements smart search using tools like PostgreSQL `tsvector` or Elasticsearch.

### Geo-proximity Search

We enable job searches based on proximity to a user’s location:

```http
GET /jobs?near=1.9403,30.0821&radius=10
```

* `near`: Coordinates (`latitude,longitude`)
* `radius`: Distance in kilometers

---

## 4. Parameter Validation Rules & Error Handling

We validate all incoming parameters before executing business logic.

| Parameter | Validation Rule         | Example Error                       |
| --------- | ----------------------- | ----------------------------------- |
| `limit`   | Must be between 1–100   | `"Limit must be between 1 and 100"` |
| `order`   | Must be `asc` or `desc` | `"Order must be 'asc' or 'desc'"`   |
| `radius`  | Must be > 0             | `"Radius must be > 0"`              |

### Sample Error Response

```json
{
  "error": "InvalidParameter",
  "message": "The 'sort_by' field is not valid. Use 'created_at' or 'price'.",
  "timestamp": "2025-05-26T13:00:00Z"
}
```

> We provide clear error messages to help developers debug efficiently.

---

## Summary: Why This Matters

### Why RESTful Principles?

We follow RESTful standards because they are:

* ✅ Clear and standardized
* ✅ Easy for frontend and mobile teams to consume
* ✅ Scalable and maintainable

### Why Filtering & Query Flexibility?

* We support lean data transfers, crucial in low-bandwidth environments
* Our API is optimized for mobile-first experiences

### Why Good Error Handling?

* We reduce debugging time
* We improve the developer experience through structured error responses

---

## Tools We Use

* **Swagger / OpenAPI** – For interactive API design and documentation
* **Postman** – For testing endpoints and query parameters
* **Rate Limiting & Validation** – Essential for production-grade APIs

```