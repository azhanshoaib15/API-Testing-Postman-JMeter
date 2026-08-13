# Functional API Test Report — JSONPlaceholder REST API

**Project:** Assignment No. 4 – API Testing  
**Prepared for:** QA Internship Program  
**Prepared by:** Azhan Shoaib  
**API Under Test:** JSONPlaceholder (`https://jsonplaceholder.typicode.com`)  
**Testing Tool:** Postman (Collection Runner & Environment Manager)  
**Execution Status:** ✅ **PASSED (100% Pass Rate — 32/32 Assertions Passed)**  

---

## 1. Executive Summary

This report documents the functional test automation suite developed for the **JSONPlaceholder REST API**. The objective was to thoroughly validate API contract compliance, HTTP status codes, response payloads, JSON schemas, header metadata, error handling, and performance SLAs across primary HTTP methods (**GET**, **POST**, **PUT**, **PATCH**, **DELETE**).

All test scripts were written in JavaScript using Postman's `pm.*` test sandbox. The suite was executed both within the Postman Collection Runner and via the Newman CLI runner, completing **10 requests** and **32 automated assertions** with zero failures.

---

## 2. Test Environment & Configuration

| Parameter | Value / Configuration |
| :--- | :--- |
| **Base URL** | `https://jsonplaceholder.typicode.com` |
| **Authentication** | None (Public REST Mock API) |
| **Payload Format** | `application/json; charset=UTF-8` |
| **Test Collection** | `postman/JSONPlaceholder_API_Testing.postman_collection.json` |
| **Environment File** | `postman/JSONPlaceholder_Environment.postman_environment.json` |
| **Execution Tool** | Postman Runner / Newman v6+ on Node.js v24 |

### Environment Variables Used

- `baseUrl`: The root host URL (`https://jsonplaceholder.typicode.com`).
- `samplePostId`: Default test post ID (`1`).
- `sampleUserId`: Default test user ID (`1`).
- `nonExistentPostId`: Boundary value for negative testing (`99999`).
- `createdPostId`: Dynamically populated during POST execution (`101`).

---

## 3. HTTP Methods & Status Codes Overview

| HTTP Method | Purpose in REST Architecture | Expected Status Code | Meaning |
| :--- | :--- | :--- | :--- |
| **GET** | Retrieve resource representation without side effects (Idempotent). | `200 OK` / `404 Not Found` | Request succeeded / Resource not found |
| **POST** | Submit new entity data to create a new resource on the server. | `201 Created` | Resource successfully created |
| **PUT** | Replace the complete representation of an existing resource. | `200 OK` | Resource completely replaced/updated |
| **PATCH** | Apply partial modifications to an existing resource. | `200 OK` | Specified fields updated |
| **DELETE** | Remove the target resource from the server. | `200 OK` / `204 No Content` | Resource deleted |

---

## 4. Test Matrix & Detailed Assertions Breakdown

### Module 1: GET Endpoints Validation

#### TC-01: `GET /posts` — Retrieve All Posts
- **Objective:** Verify that the API returns the complete collection of posts with proper array schema and data types.
- **Assertions:**
  1. Status code is `200 OK`.
  2. Response time SLA is `< 2500ms`.
  3. `Content-Type` header includes `application/json`.
  4. Response is an array containing exactly `100` posts.
  5. Schema check on first record: `userId` (number), `id` (number), `title` (string), `body` (string).
- **Result:** ✅ PASS

#### TC-02: `GET /posts/1` — Retrieve Single Post by ID
- **Objective:** Validate retrieval of a specific post resource by primary key (`id=1`).
- **Assertions:**
  1. Status code is `200 OK`.
  2. Response time is acceptable (`< 2500ms`).
  3. `Content-Type` header is JSON.
  4. Exact match: `post.id === 1`.
  5. Field integrity: `userId` is a positive number, `title` and `body` are non-empty strings.
- **Result:** ✅ PASS

#### TC-03: `GET /posts/1/comments` — Retrieve Relational Comments
- **Objective:** Verify relational query endpoint retrieving comments belonging to post ID 1.
- **Assertions:**
  1. Status code is `200 OK`.
  2. Response is a non-empty array of comment objects (`postId`, `id`, `name`, `email`, `body`).
  3. Regex validation: Email field matches RFC-compliant email pattern (`/^[^\s@]+@[^\s@]+\.[^\s@]+$/`).
- **Result:** ✅ PASS

#### TC-04: `GET /posts?userId=1` — Query Parameter Filtering
- **Objective:** Verify backend filtering by query parameter.
- **Assertions:**
  1. Status code is `200 OK`.
  2. Every returned item in the array has `userId === 1`.
- **Result:** ✅ PASS

#### TC-05: `GET /posts/99999` — Negative Test (Non-Existent Resource)
- **Objective:** Verify appropriate error handling for non-existent resource IDs.
- **Assertions:**
  1. Status code is `404 Not Found`.
  2. Response body is an empty JSON object `{}`.
- **Result:** ✅ PASS

---

### Module 2: POST Endpoints Validation

#### TC-06: `POST /posts` — Create New Post Entity
- **Objective:** Validate resource creation and server ID generation.
- **Request Body:**
  ```json
  {
    "title": "Automated API Testing with Postman",
    "body": "Validating REST endpoints, headers, status codes, and JSON schemas.",
    "userId": 1
  }
  ```
- **Assertions:**
  1. Status code is `201 Created`.
  2. Response time SLA is `< 2500ms`.
  3. Server assigns new ID (`id === 101`).
  4. Echoed fields match submitted `title`, `body`, and `userId`.
  5. Dynamically saves `responseData.id` to environment variable `createdPostId`.
  6. `Content-Type` header contains `application/json`.
- **Result:** ✅ PASS

#### TC-07: `POST /posts` — Dynamic Timestamp Payload
- **Objective:** Validate pre-request script execution and dynamic payload generation.
- **Pre-request Logic:** Generates unique epoch timestamp: `pm.variables.set("dynamicTitle", "Performance Test Post - " + Date.now())`.
- **Assertions:**
  1. Status code is `201 Created`.
  2. Response echoes exact dynamic timestamp title and body.
- **Result:** ✅ PASS

---

### Module 3: PUT Endpoints Validation

#### TC-08: `PUT /posts/1` — Full Update of Existing Resource
- **Objective:** Verify complete replacement of post resource representation.
- **Request Body:**
  ```json
  {
    "id": 1,
    "title": "Updated Post Title - QA Regression Suite",
    "body": "This post content has been modified during automated test execution.",
    "userId": 1
  }
  ```
- **Assertions:**
  1. Status code is `200 OK`.
  2. Response time is within SLA (`< 2500ms`).
  3. Resource `id` remains `1`.
  4. Fields `title` and `body` reflect updated values.
  5. `Content-Type` is JSON.
- **Result:** ✅ PASS

---

### Module 4: PATCH Endpoints Validation

#### TC-09: `PATCH /posts/1` — Partial Resource Modification
- **Objective:** Verify partial resource updates where only specific keys are modified.
- **Request Body:**
  ```json
  {
    "title": "Patched Title via QA Automation"
  }
  ```
- **Assertions:**
  1. Status code is `200 OK`.
  2. `title` is updated to the patched value.
  3. Other attributes (`userId`, `body`, `id`) remain intact and populated.
- **Result:** ✅ PASS

---

### Module 5: DELETE Endpoints Validation

#### TC-10: `DELETE /posts/1` — Resource Deletion
- **Objective:** Validate resource removal request handling.
- **Assertions:**
  1. Status code is `200 OK` (or `204 No Content`).
  2. Response time is within SLA (`< 2500ms`).
  3. Response payload is an empty JSON object `{}`.
- **Result:** ✅ PASS

---

## 5. Postman Test Scripts Example Reference

Below are representative code snippets from the automated test suite demonstrating standard QA validation practices:

### Status Code & Response Time Validation:
```javascript
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

pm.test("Response time is within SLA (< 2500ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(2500);
});
```

### JSON Schema & Field Type Validation:
```javascript
pm.test("Validate post schema and data types", function () {
    const post = pm.response.json();
    pm.expect(post).to.have.property("id").that.is.a("number");
    pm.expect(post).to.have.property("userId").that.is.a("number");
    pm.expect(post).to.have.property("title").that.is.a("string").and.not.empty;
    pm.expect(post).to.have.property("body").that.is.a("string").and.not.empty;
});
```

### Dynamic Variable Chaining:
```javascript
pm.test("Capture newly created post ID for chaining", function () {
    const responseData = pm.response.json();
    pm.expect(responseData.id).to.eql(101);
    pm.environment.set("createdPostId", responseData.id);
});
```

---

## 6. Execution Results Summary

```
┌─────────────────────────┬────────────────────┬────────────────────┐
│ Metric                  │ Executed           │ Failed             │
├─────────────────────────┼────────────────────┼────────────────────┤
│ Total Iterations        │ 1                  │ 0                  │
│ Total Requests          │ 10                 │ 0                  │
│ Test Scripts Executed   │ 10                 │ 0                  │
│ Pre-request Scripts     │ 1                  │ 0                  │
│ Total Assertions        │ 32                 │ 0                  │
│ Pass Rate               │ 100.0%             │ 0.0%               │
├─────────────────────────┴────────────────────┴────────────────────┤
│ Total Run Duration: 3.7 seconds                                   │
│ Total Data Received: 30.21 kB                                     │
│ Average Response Time: 273 ms (Min: 98 ms, Max: 740 ms)           │
└───────────────────────────────────────────────────────────────────┘
```

---

## 7. QA Findings & Observations

1. **API Consistency:** JSONPlaceholder provides consistent status codes (`200 OK`, `201 Created`, `404 Not Found`).
2. **Mock Persistence:** Because JSONPlaceholder is a mock API, `POST`, `PUT`, and `DELETE` requests simulate state changes and return proper HTTP responses without permanently modifying the backend database.
3. **Response Headers:** Responses include standard security and caching headers (`access-control-allow-credentials: true`, `x-content-type-options: nosniff`, `Cache-Control: no-cache`).
4. **Latency:** Average endpoint latency was **273ms**, well within acceptable web API SLA targets.
