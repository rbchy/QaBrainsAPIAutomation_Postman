# QaBrainsAPIAutomation_Postman

> A production-grade API test automation framework built with **Postman** and **Newman** to validate the complete RESTful service layer of the **QaBrains** platform. This project demonstrates dual expertise in **software engineering** and **API quality assurance** — implementing structured collection architecture, dynamic request chaining, data-driven validation, and CI/CD-ready execution pipelines that mirror enterprise delivery standards.

---

## 🎯 Project Mission

The QaBrains platform exposes a suite of RESTful APIs powering its authentication, content management, and user interaction workflows. This framework establishes a **living contract test suite** around those services — ensuring every endpoint behaves as specified across environments, data conditions, and user roles.

This is not a collection of ad-hoc Postman requests. It is a **structured automation framework** where collections are modular, environments are portable, test logic is scripted, and execution is fully automatable from the command line — with or without the Postman GUI.

---

## 🚀 Key Framework Highlights

### Modular Collection Architecture
API requests are organized into **feature-scoped folders** within a single master collection. Each folder maps to a distinct business domain (Auth, Users, Courses, etc.), making it trivial to run isolated suites or the full regression pack.

### Dynamic Request Chaining
Environment and collection variables are used to **chain dependent requests** — extracting tokens, IDs, and response values from upstream calls and injecting them into downstream requests automatically. No manual copy-paste between tests.

### Data-Driven Testing
External JSON data files power **parametric test execution**, enabling a single request definition to validate dozens of input combinations — valid payloads, boundary values, and malformed inputs — without duplicating collection logic.

### Pre-Request & Test Script Layer
JavaScript-powered **Pre-Request Scripts** handle token injection, timestamp generation, and request signing before each call. **Test Scripts** execute immediately after each response, running assertions against status codes, response bodies, headers, and JSON schemas.

### Environment-Based Configuration
All environment-specific values — base URLs, credentials, API keys, and timeouts — are externalized into Postman **Environment files**. Switching from Development to Staging to Production requires selecting a single environment file. Zero code changes needed.

### Newman CLI Integration
The entire collection executes headlessly via **Newman**, Postman's official Node.js CLI runner. This makes the framework fully integrable into CI/CD pipelines — GitHub Actions, Jenkins, GitLab CI — with a single command.

### Structured Reporting
Newman generates **HTML reports** after each run, providing a visual summary of passed, failed, and skipped assertions, response times, and request-level details — suitable for attaching to sprint reports or defect tickets.

---

## 🛠 Tech Stack & Tools

| Component | Technology | Version | Purpose |
| :--- | :--- | :--- | :--- |
| **API Testing Tool** | Postman | Latest | Collection authoring, manual execution, and debugging |
| **CLI Runner** | Newman | 6.x | Headless, CI/CD-compatible collection execution |
| **Scripting Language** | JavaScript (ES6+) | — | Pre-request scripts, test assertions, and variable logic |
| **Reporting** | Newman HTML Reporter | Latest | Visual HTML execution dashboards |
| **Data Layer** | JSON | — | External data files for data-driven test iterations |
| **Runtime** | Node.js | 18+ | Newman runtime and reporter dependencies |
| **CI/CD** | GitHub Actions / Jenkins | — | Pipeline integration for automated scheduled runs |
| **Version Control** | Git / GitHub | — | Collection versioning and team collaboration |

---

## 📁 Project Structure

```text
QaBrainsAPIAutomation_Postman/
│
├── collections/
│   └── QaBrains_API_Collection.json     # Master Postman collection (all modules)
│
├── environments/
│   ├── QaBrains_Dev.postman_environment.json    # Development environment variables
│   ├── QaBrains_Test.postman_environment.json   # Test / Staging environment variables
│   └── QaBrains_Prod.postman_environment.json   # Production (read-only) environment
│
├── test-data/
│   ├── valid_users.json                 # Valid user payloads for positive tests
│   ├── invalid_users.json              # Malformed / boundary payloads for negative tests
│   ├── course_payloads.json            # Course creation and update data sets
│   └── auth_tokens.json               # Pre-seeded token data for chained flows
│
├── scripts/
│   ├── pre_request_auth.js             # Reusable pre-request token injection script
│   ├── response_schema_validator.js    # JSON schema validation helper
│   └── dynamic_variable_extractor.js  # Chaining helper — extracts and sets env vars
│
├── reports/
│   └── newman-report.html              # Latest HTML execution report (auto-generated)
│
├── .github/
│   └── workflows/
│       └── api-tests.yml               # GitHub Actions CI/CD pipeline definition
│
├── package.json                        # Node.js dependencies (Newman + reporters)
└── README.md                           # Project documentation
```

---

## 🌐 API Modules Under Test

| Module | Endpoint Group | Scenarios Covered |
| :--- | :--- | :--- |
| **Authentication** | `/auth/login`, `/auth/logout`, `/auth/refresh` | Valid login, invalid credentials, token expiry, refresh flow |
| **User Management** | `/users`, `/users/{id}` | Create, read, update, delete user records |
| **Course Catalog** | `/courses`, `/courses/{id}` | List, create, update, and delete course entries |
| **Enrollment** | `/enrollments`, `/enrollments/{id}` | Enroll user, verify enrollment, cancel enrollment |
| **Search** | `/search?q=` | Valid queries, empty queries, special characters |
| **Error Handling** | All endpoints | 400, 401, 403, 404, 422, 500 response validation |

---

## 🎯 Test Coverage Matrix

### 🔐 Authentication APIs
- Token generation on valid credentials
- Authorization header validation across protected endpoints
- Session expiry and token refresh behavior
- Unauthorized access rejection (401 / 403)

### 📦 CRUD Operations
- `POST` — Resource creation with valid payload; assert `201 Created`
- `GET` — Single and collection retrieval; assert response structure
- `PUT` / `PATCH` — Full and partial updates; assert field-level changes
- `DELETE` — Soft and hard delete; assert `204 No Content` or `200 OK`

### 🔎 Response Validation
- HTTP status code assertions on every request
- JSON schema validation using `tv4` or `ajv` in Test Scripts
- Key/value assertions on critical response fields
- Response time assertions (SLA compliance)
- Header validation (`Content-Type`, `Authorization`, `Cache-Control`)

### ⚠️ Negative Testing
- Invalid request bodies (missing required fields, wrong data types)
- Unauthorized requests (missing or expired token)
- Non-existent resource access (`404 Not Found`)
- Duplicate resource creation conflict (`409 Conflict`)
- SQL injection and XSS payload handling

### 🔄 End-to-End API Flows
- **Login → Get Profile → Update Profile → Logout** flow
- **Create Course → Enroll User → Verify Enrollment → Delete Course** flow
- **Register User → Verify Email → Login → Access Protected Resource** flow

---

## ⚙️ Setup & Installation

### Prerequisites

Ensure the following are available on your system:

- **Postman** (latest desktop client) — [Download](https://www.postman.com/downloads/)
- **Node.js** v18+ — [Download](https://nodejs.org/)
- **npm** (bundled with Node.js)
- Access to QaBrains API endpoints (Dev or Test environment)

### Clone the Repository

```bash
git clone https://github.com/ranajitchowdhury/QaBrainsAPIAutomation_Postman.git
cd QaBrainsAPIAutomation_Postman
```

### Install Newman and Reporters

```bash
npm install
```

Or manually:

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

---

## ▶️ Running the Tests

### Option 1 — Postman GUI (Manual / Exploratory)

1. Open Postman
2. Click **Import** → select `collections/QaBrains_API_Collection.json`
3. Import the matching environment file from `environments/`
4. Select the target environment from the top-right dropdown
5. Open **Collection Runner** → select the collection → click **Run**

### Option 2 — Newman CLI (Headless / Automated)

**Run against the Test environment:**
```bash
newman run collections/QaBrains_API_Collection.json \
  -e environments/QaBrains_Test.postman_environment.json
```

**Run with data file for data-driven testing:**
```bash
newman run collections/QaBrains_API_Collection.json \
  -e environments/QaBrains_Test.postman_environment.json \
  -d test-data/valid_users.json
```

**Run a specific folder only:**
```bash
newman run collections/QaBrains_API_Collection.json \
  -e environments/QaBrains_Test.postman_environment.json \
  --folder "Authentication"
```

**Generate HTML Report:**
```bash
newman run collections/QaBrains_API_Collection.json \
  -e environments/QaBrains_Test.postman_environment.json \
  -r htmlextra \
  --reporter-htmlextra-export reports/newman-report.html
```

---

## 📊 Sample Test Script

The following is a representative Test Script from the **Login** request, demonstrating the assertion pattern used throughout the collection:

```javascript
// ── Status Code Assertion ──────────────────────────────────
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

// ── Response Time SLA ─────────────────────────────────────
pm.test("Response time is under 2000ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(2000);
});

// ── Content-Type Header ───────────────────────────────────
pm.test("Response is JSON", function () {
    pm.response.to.have.header("Content-Type", /application\/json/);
});

// ── Token Extraction and Chaining ─────────────────────────
const jsonData = pm.response.json();

pm.test("Response contains access token", function () {
    pm.expect(jsonData).to.have.property("token");
    pm.expect(jsonData.token).to.be.a("string").and.not.empty;
});

// Set token for downstream requests
pm.environment.set("auth_token", jsonData.token);
pm.environment.set("user_id", jsonData.user.id);

// ── JSON Schema Validation ────────────────────────────────
const schema = {
    type: "object",
    required: ["token", "user"],
    properties: {
        token: { type: "string" },
        user: {
            type: "object",
            required: ["id", "email", "role"],
            properties: {
                id:    { type: "integer" },
                email: { type: "string" },
                role:  { type: "string" }
            }
        }
    }
};

pm.test("Response schema is valid", function () {
    pm.response.to.have.jsonSchema(schema);
});
```

---

## 🔄 CI/CD Pipeline Integration

The framework ships with a **GitHub Actions** workflow that triggers on every push to `main` and on pull request creation:

```yaml
# .github/workflows/api-tests.yml
name: QaBrains API Test Suite

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  api-tests:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install Newman and reporters
        run: npm install

      - name: Execute API Test Suite
        run: |
          newman run collections/QaBrains_API_Collection.json \
            -e environments/QaBrains_Test.postman_environment.json \
            -r htmlextra \
            --reporter-htmlextra-export reports/newman-report.html

      - name: Upload Test Report
        uses: actions/upload-artifact@v3
        with:
          name: API-Test-Report
          path: reports/newman-report.html
```

---

## 📈 Roadmap — Future Enhancements

- [ ] **Allure Report Integration** — richer, interactive execution dashboards
- [ ] **Multi-Environment Matrix Execution** — run the same suite against Dev, Test, and Staging in a single pipeline job
- [ ] **Contract Testing** — integrate **Pact** for consumer-driven contract validation
- [ ] **Performance Baseline Assertions** — enforce per-endpoint response time SLAs
- [ ] **Mock Server Integration** — use Postman Mock Servers to enable testing in environments without live API access
- [ ] **Jenkins Pipeline** — add `Jenkinsfile` for teams on Jenkins-based CI infrastructure
- [ ] **Secrets Management** — integrate with GitHub Secrets / HashiCorp Vault for credential injection

---

## 🏗 Design Principles

### Contract-First Validation
Every endpoint is tested against both its **happy path** and its **error contract**. An API that returns the right data for valid inputs but returns a `500` instead of a `422` for malformed inputs is a broken API — this framework catches both.

### Zero Hardcoded Values
No credentials, base URLs, or environment-specific data live inside the collection. Every configurable value is externalized to an Environment file, making the collection **portable and environment-agnostic by design**.

### Assertion Depth
Shallow assertions (`status === 200`) are not sufficient for production-grade API testing. Every response is validated for status code, Content-Type header, response body structure, field-level data types, and where applicable, JSON schema conformance.

### Reusability Over Repetition
Pre-Request Scripts and helper scripts in the `scripts/` directory are authored once and referenced everywhere. Common logic — token injection, timestamp generation, schema validation — lives in a single place.

---

## 🤝 Contributing

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/add-enrollment-tests`
3. Commit your changes: `git commit -m "feat: add enrollment API negative test cases"`
4. Push to your branch: `git push origin feature/add-enrollment-tests`
5. Open a Pull Request against `main`

Please follow the existing folder structure and naming conventions.

---

## 👨‍💻 Author

**Ranajit B. Chowdhury**
Software Developer | QA Automation Engineer
API & UI Automation | Selenium | Playwright | Postman | Newman

> *"A test that doesn't fail when it should is worse than no test at all."*

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for full details.
