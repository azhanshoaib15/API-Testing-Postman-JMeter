# Assignment No. 4 – API Testing (Postman & Apache JMeter)

**Prepared for:** QA Internship Program  
**Prepared by:** Azhan Shoaib  
**Target API:** JSONPlaceholder REST API (`https://jsonplaceholder.typicode.com`)  


---

## 📌 Project Overview

This repository contains my submission for **Assignment 4 – API Testing**. The goal was to perform end-to-end functional API validation using **Postman** and basic performance load testing using **Apache JMeter** against the free public **JSONPlaceholder** REST API.

### What was completed:
- **Postman Functional Testing:** Built a structured collection covering GET, POST, PUT, PATCH, and DELETE requests. Added 32 automated JavaScript assertions to validate status codes, response time limits, headers, JSON schemas, and field values.
- **Apache JMeter Load Testing:** Built a load test plan with 10 virtual users, 5-second ramp-up, and 5 loops (250 total requests). Monitored response times, throughput, and error rates using View Results Tree, Summary Report, and Aggregate Report listeners.
- **Evidence & Reports:** Generated comprehensive test execution reports and captured 16 proof screenshots.

---

## 💡 Practical QA Notes on JSONPlaceholder

- **Mock API Behavior:** JSONPlaceholder is a free mock REST API built for testing and prototyping. It validates incoming request structures and returns standard HTTP responses (`200 OK`, `201 Created`, `404 Not Found`). However, it does not permanently save changes to a live database. For example, any POST request returns a generated `id: 101` with your data echoed back, and a DELETE request returns `200 OK` with `{}` without permanently removing the post. This is normal mock API behavior.
- **Workload Calibration:** A workload of 10 virtual users over 5 loops was selected to verify concurrent user orchestration without overloading a shared public educational server.
- **Response Time SLAs:** Assertion thresholds are set with realistic bounds (< 2500ms) to detect genuine server delays while preventing false alarms caused by everyday internet latency variations.

---

## 📂 Repository Structure

```
Api_Testing/
├── postman/
│   ├── JSONPlaceholder_API_Testing.postman_collection.json  # Postman Test Collection (v2.1)
│   └── JSONPlaceholder_Environment.postman_environment.json   # Environment Variables (baseUrl, IDs)
├── jmeter/
│   ├── JSONPlaceholder_Load_Test.jmx                        # Apache JMeter Load Test Plan (.jmx)
│   └── results/                                            # JMeter Result Logs (.jtl)
│       ├── summary_report.jtl
│       ├── results_tree.jtl
│       └── aggregate_report.jtl
├── reports/
│   ├── API-Testing-Assignment-4-Report.docx                 # Master Assignment Word Report (.docx)
│   ├── API_Functional_Test_Report.md                        # Functional QA Test Report (Markdown)
│   ├── Postman_Test_Execution_Evidence.md                   # Postman Screenshots & Evidence (Markdown)
│   ├── Postman_API_Testing_Report.docx                      # Postman Word Report (.docx)
│   ├── JMeter_Performance_Report.md                         # JMeter Performance Report (Markdown)
│   └── JMeter_Performance_Testing_Report.docx               # JMeter Word Report (.docx)
├── screenshots/                                             # 16 Proof Screenshots
│   ├── 01_postman_collection_runner.png
│   ├── 02_postman_collection_runner.png
│   ├── 02_postman_get_posts_tests.png
│   ├── 03_postman_collection_runner.png
│   ├── 03_postman_get_single_post.png
│   ├── 03_postman_get_single_post_body.png
│   ├── 04_postman_post_create_post_body.png
│   ├── 04_postman_post_create_post_test.png
│   ├── 05_postman_put_update_post_body.png
│   ├── 05_postman_put_update_post_test.png
│   ├── 06_postman_delete_post.png
│   ├── 07_jmeter_test_plan_tree.png
│   ├── 08_jmeter_thread_group_config.png
│   ├── 09_jmeter_view_results_tree.png
│   ├── 10_jmeter_summary_report.png
│   └── 11_jmeter_aggregate_report.png
├── .gitignore                                               # Git ignore rules
└── README.md                                                # Master Project Documentation
```

---

## 📋 Completed Tasks Breakdown

| Task # | Assignment Requirement | What Was Done | Status |
| :---: | :--- | :--- | :---: |
| **1** | **Setup Postman Workspace** | Created a collection organized into folders (`01_GET`, `02_POST`, `03_PUT`, `04_PATCH`, `05_DELETE`) and configured an environment with `{{baseUrl}}`. | ✅ Done |
| **2** | **Test GET API Endpoints** | Tested `GET /posts` (list of 100 posts), `GET /posts/1` (single post), `GET /posts/1/comments`, `GET /posts?userId=1`, and `GET /posts/99999` (negative 404 test). | ✅ Done |
| **3** | **Test POST API Endpoints** | Tested `POST /posts` verifying `201 Created` status, ID generation (`id: 101`), payload echoing, and pre-request dynamic timestamps. | ✅ Done |
| **4** | **Test PUT API Endpoints** | Tested `PUT /posts/1` verifying full resource update, `200 OK` status, and updated title reflection. | ✅ Done |
| **5** | **Test DELETE API Endpoints** | Tested `DELETE /posts/1` verifying `200 OK` status and empty JSON object `{}`. | ✅ Done |
| **6** | **Write Postman Test Scripts** | Added 32 automated test assertions in JavaScript covering status codes, response time limits, headers, schemas, and exact field values. | ✅ Done |
| **7** | **Basic Load Testing with JMeter** | Created a `.jmx` test plan with 10 virtual users, 5s ramp-up, 5 loops (250 total samples), duration assertions (< 2000ms), and listeners. | ✅ Done |

---

## 🚀 Part 1: How to Run Postman Tests

### 1. Import Files into Postman
1. Open **Postman**.
2. Click **Import** (top-left) and select both files from the [`postman/`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/postman/) folder:
   - `JSONPlaceholder_API_Testing.postman_collection.json`
   - `JSONPlaceholder_Environment.postman_environment.json`
3. In the top-right environment dropdown, select **`JSONPlaceholder - Test Environment`**.

### 2. Endpoints & Test Cases in the Collection

| Method | Endpoint | Purpose | Expected Status |
| :--- | :--- | :--- | :---: |
| `GET` | `{{baseUrl}}/posts` | Retrieve all 100 posts; validate array schema & item types | `200 OK` |
| `GET` | `{{baseUrl}}/posts/{{samplePostId}}` | Retrieve single post; validate exact ID (1), title, body, userId | `200 OK` |
| `GET` | `{{baseUrl}}/posts/{{samplePostId}}/comments` | Retrieve nested comments; validate email regex format | `200 OK` |
| `GET` | `{{baseUrl}}/posts?userId={{sampleUserId}}` | Query filtering; verify all items match `userId === 1` | `200 OK` |
| `GET` | `{{baseUrl}}/posts/{{nonExistentPostId}}` | Negative test; verify non-existent resource error handling | `404 Not Found` |
| `POST` | `{{baseUrl}}/posts` | Create new post; verify ID `101` generation and payload echo | `201 Created` |
| `POST` | `{{baseUrl}}/posts` | Create post with dynamic pre-request timestamp payload | `201 Created` |
| `PUT` | `{{baseUrl}}/posts/{{samplePostId}}` | Full resource update; verify updated title and body | `200 OK` |
| `PATCH` | `{{baseUrl}}/posts/{{samplePostId}}` | Partial update; verify modified title while preserving other fields | `200 OK` |
| `DELETE`| `{{baseUrl}}/posts/{{samplePostId}}` | Delete post; verify HTTP status and empty response `{}` | `200 OK` |

### 3. Example Postman Automated Test Script

```javascript
// 1. Status Code Validation
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// 2. Response Time Limit Validation
pm.test("Response time is within acceptable SLA (< 2500ms)", function () {
    pm.expect(pm.response.responseTime).to.be.below(2500);
});

// 3. Schema and Field Validation
pm.test("Verify post structure and data types", function () {
    const post = pm.response.json();
    pm.expect(post.id).to.be.a("number").and.to.eql(1);
    pm.expect(post.userId).to.be.a("number");
    pm.expect(post.title).to.be.a("string").and.not.empty;
    pm.expect(post.body).to.be.a("string").and.not.empty;
});
```

### 4. Running the Collection Runner
1. Right-click **`JSONPlaceholder REST API - Functional Test Suite`** in Postman -> click **Run collection**.
2. Keep all 10 requests selected and click **Run**.
3. **Execution Summary:** 10 requests executed, 32 assertions evaluated, **32 Passed, 0 Failed (100% Pass Rate)**.

---

## ⚡ Part 2: How to Run Apache JMeter Load Tests

### 1. Test Plan Configuration (`JSONPlaceholder_Load_Test.jmx`)
- **Global Variables:** `BASE_URL = jsonplaceholder.typicode.com`, `PROTOCOL = https`, `PORT = 443`.
- **HTTP Header Manager:** `Content-Type: application/json; charset=UTF-8`.
- **Thread Group Workload:**
  - **Number of Threads (Virtual Users):** `10`
  - **Ramp-Up Period:** `5 seconds`
  - **Loop Count:** `5`
  - **Total Samples:** $10 \text{ users} \times 5 \text{ loops} \times 5 \text{ samplers} = 250 \text{ requests}$
- **HTTP Samplers:** `GET /posts`, `GET /posts/1`, `POST /posts`, `PUT /posts/1`, `DELETE /posts/1`.
- **Assertions:** Duration Assertion (< 2000ms), Response Code Assertions (200 / 201).
- **Listeners:** `View Results Tree`, `Summary Report`, `Aggregate Report`, `Response Time Graph`.

### 2. Running JMeter (GUI Mode)
1. Open Apache JMeter (`jmeter.bat` or `jmeter`).
2. Go to **File -> Open** -> select [`jmeter/JSONPlaceholder_Load_Test.jmx`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/jmeter/JSONPlaceholder_Load_Test.jmx).
3. Click the green **Start (▶️)** button (or press `Ctrl + R`).
4. Click on **View Results Tree** or **Summary Report** to review live metrics.

---

## 📊 Performance Load Test Results & Findings

Below is the summary table from our JMeter performance run with 10 virtual users:

| Sampler Label | Samples | Average (ms) | Min (ms) | Max (ms) | 90% Line (ms) | 95% Line (ms) | Throughput (req/s) | Error Rate |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **GET All Posts** | 50 | 353 ms | 123 ms | 1524 ms | 812 ms | 1258 ms | 1.71 | 0.00% |
| **GET Single Post** | 50 | 140 ms | 88 ms | 638 ms | 387 ms | 463 ms | 1.71 | 0.00% |
| **POST Create Post** | 50 | 456 ms | 301 ms | 1697 ms | 795 ms | 795 ms | 1.71 | 0.00% |
| **PUT Update Post** | 50 | 491 ms | 302 ms | 1718 ms | 851 ms | 954 ms | 1.71 | 0.00% |
| **DELETE Post** | 50 | 504 ms | 299 ms | 3166 ms | 826 ms | 826 ms | 1.71 | 0.00% |
| **OVERALL TOTAL** | **250** | **389 ms** | **88 ms** | **3166 ms** | **795 ms** | **826 ms** | **8.54** | **0.00%** |

### Key Takeaways:
1. **Average Response Time:** ~389 ms overall, comfortably meeting the performance SLA target (< 1000 ms).
2. **Percentiles:** 90% of requests finished in under **795 ms**, demonstrating stable performance under concurrency.
3. **Throughput:** Maintained an overall throughput of **8.54 requests/second**.
4. **Reliability:** **0.00% error rate** across all 250 requests with zero timeouts or failed assertions.

---

## 📸 Screenshots Evidence Mapping

All screenshots are stored in [`screenshots/`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/screenshots/):

| Screenshot File | Tool | What it Shows |
| :--- | :---: | :--- |
| `01_postman_collection_runner.png` | Postman | Collection Runner summary (Passed: 32 / Failed: 0) |
| `02_postman_collection_runner.png` | Postman | Collection Runner execution (PUT / PATCH tests) |
| `03_postman_collection_runner.png` | Postman | Collection Runner execution (Overview) |
| `02_postman_get_posts_tests.png` | Postman | `GET /posts` status 200 OK and 5/5 passing test results |
| `03_postman_get_single_post_body.png` | Postman | `GET /posts/1` response body showing single post |
| `03_postman_get_single_post.png` | Postman | `GET /posts/1` test results tab |
| `04_postman_post_create_post_body.png` | Postman | `POST /posts` request body and 201 Created response (id: 101) |
| `04_postman_post_create_post_test.png` | Postman | `POST /posts` test results tab |
| `05_postman_put_update_post_body.png` | Postman | `PUT /posts/1` request body and 200 OK updated response |
| `05_postman_put_update_post_test.png` | Postman | `PUT /posts/1` test results tab |
| `06_postman_delete_post.png` | Postman | `DELETE /posts/1` status 200 OK and empty `{}` body |
| `07_jmeter_test_plan_tree.png` | JMeter | Test Plan hierarchy showing Samplers, Assertions, Listeners |
| `08_jmeter_thread_group_config.png` | JMeter | Thread Group workload: 10 Users, 5s Ramp-up, 5 Loops |
| `09_jmeter_view_results_tree.png` | JMeter | View Results Tree showing green successful sample requests |
| `10_jmeter_summary_report.png` | JMeter | Summary Report table (Latency, Throughput, 0% Error) |
| `11_jmeter_aggregate_report.png` | JMeter | Aggregate Report table showing 90th/95th percentile lines |

---

## 📝 Submission Files Checklist

- [x] **Master Word Document (`.docx`):** [`reports/API-Testing-Assignment-4-Report.docx`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/API-Testing-Assignment-4-Report.docx)
- [x] **Postman Collection:** [`postman/JSONPlaceholder_API_Testing.postman_collection.json`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/postman/JSONPlaceholder_API_Testing.postman_collection.json)
- [x] **Postman Environment:** [`postman/JSONPlaceholder_Environment.postman_environment.json`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/postman/JSONPlaceholder_Environment.postman_environment.json)
- [x] **Postman Word Report (`.docx`):** [`reports/Postman_API_Testing_Report.docx`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/Postman_API_Testing_Report.docx)
- [x] **JMeter Test Plan (`.jmx`):** [`jmeter/JSONPlaceholder_Load_Test.jmx`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/jmeter/JSONPlaceholder_Load_Test.jmx)
- [x] **JMeter Word Report (`.docx`):** [`reports/JMeter_Performance_Testing_Report.docx`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/JMeter_Performance_Testing_Report.docx)
- [x] **JMeter Test Results (`.jtl` logs):** [`jmeter/results/summary_report.jtl`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/jmeter/results/summary_report.jtl)
- [x] **Functional QA Report (Markdown):** [`reports/API_Functional_Test_Report.md`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/API_Functional_Test_Report.md)
- [x] **Postman Evidence Report (Markdown):** [`reports/Postman_Test_Execution_Evidence.md`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/Postman_Test_Execution_Evidence.md)
- [x] **Performance QA Report (Markdown):** [`reports/JMeter_Performance_Report.md`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/reports/JMeter_Performance_Report.md)
- [x] **Screenshots Evidence:** All 16 named screenshots saved in [`screenshots/`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/screenshots/)
- [x] **Master README:** [`README.md`](file:///c:/Users/azhan/OneDrive/Desktop/Api_Testing/README.md)

