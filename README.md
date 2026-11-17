# API Testing Portfolio — Postman

This repository showcases automated REST API test suites built in **Postman**, covering end-to-end CRUD workflows, data-driven testing, dynamic variable chaining, authentication checks, and negative/boundary test scenarios.

---

## Collections Overview

| # | Collection | Target API | Key Testing Techniques |
| :- | :--- | :--- | :--- |
| 1 | **`Simple Grocery Store API`** | E-commerce REST API (`{{baseURL}}`) | End-to-End CRUD flow, Bearer Token auth, Negative & Boundary testing (`400`, `401`, `404`), dynamic ID chaining |
| 2 | **`Trello API`** | Trello REST API (`https://api.trello.com`) | Stateful workflow automation (Board → Lists → Card → Move → Delete), execution flow control (`setNextRequest`), cleanup verification |
| 3 | **`Order Management`** | HTTPBin (`httpbin.org`) | Data-Driven Testing (`pm.iterationData`), pre-request JSON serialization, conditional assertions (`isValid`), dynamic status validation |

---

## Detailed Breakdown

### 1. Simple Grocery Store API (`Simple Grocery Store API.postman_collection.json`)
A comprehensive test suite organized into **Happy Path**, **Missing Authentication**, and **Invalid Inputs** folders:
* **End-to-End Shopping Flow:** Checks API status, filters available products (`category=coffee`), dynamically captures `productId`, creates a cart (`cartId`), adds/updates/replaces/deletes cart items, registers an API client, places an order (`orderId`), updates order details, and verifies deletion with a `404 Not Found` check.
* **Authentication & Authorization (`401 Unauthorized`):** Validates error responses (`Missing Authorization header.` and `Invalid bearer token.`) when accessing protected `/orders` endpoints without valid credentials.
* **Boundary & Negative Testing (`400 Bad Request`):** Tests invalid product categories and out-of-bounds pagination parameters (`results=0`, `results=-1`, `results=21`).

### 2. Trello API (`Trello API.postman_collection.json`)
An automated workflow validating board and task lifecycle management:
* **Lifecycle Automation:** Retrieves existing boards, dynamically increments board naming (`Learning Postman {{boardNumber}}`), creates `TODO` and `DONE` lists, creates a card inside `TODO`, moves it to `DONE` via `PUT`, deletes the board, and confirms deletion (`404`).
* **Custom Execution Flow:** Uses `pm.execution.setNextRequest()` to orchestrate request sequencing during Collection Runner execution.
* **Deep Payload Assertions:** Verifies nested response properties such as board privacy settings (`permissionLevel: private`) and disabled calendar views.

### 3. Order Management (`Order management.postman_collection.json`)
A data-driven collection designed to run with external CSV/JSON test data files via Postman Collection Runner:
* **Dynamic UUID Generation:** Fetches a unique `referenceId` from `/uuid` and stores it globally for downstream requests.
* **Pre-Request Scripting & Conditional Logic:** Stringifies nested `metaInfo` objects before sending `POST` payloads and branches test assertions dynamically based on the `isValid` iteration flag.
* **Parameterized Status Validation:** Dynamically asserts expected HTTP status codes via `/status/{{status}}`.

---

## How to Run Locally

1. **Clone or download** this repository.
2. Open **Postman** and click **Import** (top-left) to load any `.postman_collection.json` file.
3. **Configure Variables** (Environment or Collection Variables):
   * **Simple Grocery Store API:** Set `baseURL` (e.g., `https://simple-grocery-store-api.glitch.me` or `https://simple-grocery-store-api.click`) and run `Register API client` to generate your `accessToken`.
   * **Trello API:** Replace `trelloKey` and `trelloToken` with your own Trello developer credentials, and set `username` to your Trello handle.
   * **Order Management:** Run via **Collection Runner** with a JSON/CSV data file containing `customerId`, `productId`, `metaInfo`, `isValid`, `status`, and `expectedStatus`.
4. Click **Run collection** to execute all automated assertions.
