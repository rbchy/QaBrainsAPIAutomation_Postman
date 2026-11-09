<div align="center">

# 🔌 QaBrains API Automation — Postman / Newman

### Postman collection for the QA Brains practice site — 130 scripted assertions, runnable headless with Newman

[![Typing SVG](https://readme-typing-svg.demolab.com/?font=Fira+Code&size=18&pause=1500&color=FFC107&center=true&vCenter=true&width=900&repeat=true&lines=Postman+%C2%B7+Newman+%C2%B7+JavaScript+test+scripts;Login+%C2%B7+Registration+%C2%B7+Forgot+Password+%C2%B7+E-Commerce;130+pm.test+assertions%3A+positive%2C+negative%2C+extended)](https://git.io/typing-svg)

<!-- ============ STATUS BAR ============ -->
![Status](https://img.shields.io/badge/Status-In%20Progress-f59e0b?style=for-the-badge&logo=checkmarx&logoColor=white)
![Assertions](https://img.shields.io/badge/pm.test%20Assertions-130-155e75?style=for-the-badge)
![Requests](https://img.shields.io/badge/Requests-5%20scripted-0d9488?style=for-the-badge)
![Target](https://img.shields.io/badge/Target-practice.qabrains.com-1e3a8a?style=for-the-badge&logo=googlechrome&logoColor=white)

![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Newman](https://img.shields.io/badge/Newman-CLI-FF6C37?style=for-the-badge&logo=npm&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)

![Last Commit](https://img.shields.io/github/last-commit/rbchy/QaBrainsAPIAutomation_Postman?style=flat-square&color=0d9488&label=Last%20commit)
![Repo Size](https://img.shields.io/github/repo-size/rbchy/QaBrainsAPIAutomation_Postman?style=flat-square&color=155e75&label=Repo%20size)

</div>

---

## 📌 At a Glance

<table>
<tr>
<td width="50%" valign="top">

**🙋 What this is**

A **Postman collection** that exercises the authentication, registration, password-reset and e-commerce endpoints of the [QA Brains practice site](https://practice.qabrains.com). Each request carries a JavaScript test script with **positive, negative and extended checks** — status codes, response times, content types, body structure, token fields, error handling and security headers.

Scripts are written to survive a **JSON or an HTML** response, because several practice endpoints answer with a page rather than JSON — the tests say so instead of crashing.

> *A test that doesn't fail when it should is worse than no test at all.*

</td>
<td width="50%" valign="top">

**📍 Quick facts**

| | |
|---|---|
| 📦 **Collection** | `QAbrainsAutomation_API.postman_collection.json` |
| 🎯 **Target** | `https://practice.qabrains.com` |
| 🧪 **Assertions** | 130 `pm.test` blocks |
| 🔁 **Data-driven** | Registration reads `pm.iterationData` |
| ▶️ **Runner** | Postman Collection Runner · Newman |

</td>
</tr>
</table>

> 🚧 **Repository status:** this repository holds the collection itself. The environment file, iteration-data file, `package.json` and CI workflow are not committed yet — see the roadmap.

---

## 🌐 Requests and Coverage

| # | Request | Method & endpoint | Assertions | What the script checks |
|---|---|---|---:|---|
| 1 | 🔐 **LOGIN API (Save Token)** | `PUT /api/login` | 30 | Status, response time, JSON-or-HTML body, token field when JSON, error handling |
| 2 | 🔐 **Login (GET check)** — named *New Request* | `GET /api/login` | 30 | Positive (P-xx) and negative checks, safe JSON parsing, Content-Type |
| 3 | 📝 **Registration** | `POST /api/register` | 30 | TC01–TC10 positive · TC11–TC20 negative · TC21–TC30 extended; outcome taken from iteration data |
| 4 | 🔑 **Forgot Password** | `POST /forgot-password` | 20 | TC-01 – TC-20: status, timing, body, headers, JSON-or-HTML handling |
| 5 | 🛒 **E-Commerce Site** | `POST /ecommerce-site` | 20 | Status, timing, body and header checks for the shop login |
| | **Total** | | **130** | |

<details>
<summary><strong>🧩 Patterns used in the test scripts</strong></summary>

<br>

```javascript
// Safe parse: never crash on an HTML or empty response
let jsonBody = null, isJSON = false;
try { jsonBody = pm.response.json(); isJSON = true; } catch (e) { isJSON = false; }

pm.test("✅ [P-01] Status code is 2xx (success)", function () {
    pm.expect(pm.response.code).to.be.oneOf([200, 201, 202, 204]);
});

pm.test("[POSITIVE-05] If JSON, token/auth key should exist", function () {
    if (isJSON) {
        const token = jsonBody.token || jsonBody.access_token || jsonBody.auth_token || jsonBody.jwt;
        pm.expect(token, "Expected token-like field in JSON response").to.exist;
    }
});
```

- **Numbered test IDs** (`P-01`, `POSITIVE-05`, `TC-17` …) so a failure in a Newman report maps straight back to a test case
- **Iteration data** — Registration reads `tc_id` and `expected_result` from `pm.iterationData`, so one request can run a whole positive/negative data set
- **Response-type awareness** — checks branch on JSON vs. HTML instead of assuming one

</details>

---

## 🔍 Review Notes (open items)

| # | Item | Effect |
|---|---|---|
| 1 | Registration and E-Commerce use `{{name}}`, `{{email}}`, `{{password}}` … but no environment or data file is committed | Those requests need values supplied before they can run |
| 2 | *LOGIN API (Save Token)* sends `PUT` with `{"name": "Updated Name"}`; its script header describes `POST` with email and password | Request and script disagree; login is not really exercised |
| 3 | `/forgot-password` and `/ecommerce-site` are page URLs that return HTML | Scripts handle it, but these are page checks rather than API contract tests |
| 4 | An empty *Untitled Request* (`DELETE`, no URL) and a default-named *New Request* | Cosmetic; clutters runner output |
| 5 | Forgot Password body uses a personal e-mail address | Replace with a variable or a practice account |

---

## ⚙️ Running the Collection

**Prerequisites:** Postman (desktop) or Node.js 18+ for Newman.

<details open>
<summary><strong>Postman GUI</strong></summary>

<br>

1. **Import** → `QAbrainsAutomation_API.postman_collection.json`
2. Create an environment with `name`, `Select Country`, `Account Type`, `email`, `password`, `confirm password`
3. **Run collection** → optionally attach a CSV/JSON data file for Registration (`tc_id`, `expected_result`, plus the fields above)

</details>

<details>
<summary><strong>Newman (headless)</strong></summary>

<br>

```bash
npm install -g newman newman-reporter-htmlextra

# whole collection, with variables on the command line
newman run QAbrainsAutomation_API.postman_collection.json \
  --env-var "email=test@qabrains.com" --env-var "password=Password123"

# one request
newman run QAbrainsAutomation_API.postman_collection.json --folder "Forgot Password"

# data-driven registration + HTML report
newman run QAbrainsAutomation_API.postman_collection.json \
  --folder "Registration" -d registration-data.csv \
  -r cli,htmlextra --reporter-htmlextra-export reports/newman-report.html
```

</details>

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|---|---|---|
| API client | Postman | Authoring, manual and exploratory runs |
| Test scripts | JavaScript (`pm.test`, Chai `pm.expect`) | Assertions after each response |
| CLI runner | Newman | Headless, CI-ready execution |
| Reporting | Newman CLI / htmlextra | Run summaries and HTML reports |
| Data | `pm.iterationData` | Data-driven registration cases |

---

## 📁 Repository Contents

```
QaBrainsAPIAutomation_Postman/
├── QAbrainsAutomation_API.postman_collection.json   ← 6 requests, 130 assertions
└── README.md
```

---

## 📈 Roadmap

- [ ] Commit an environment file (`baseUrl`, practice credentials) and a registration data file
- [ ] Fix the login request: `POST /api/login` with email and password, save the token to a variable
- [ ] Remove the empty request and rename *New Request*
- [ ] Use `{{baseUrl}}` instead of hard-coded URLs
- [ ] `package.json` with Newman + htmlextra
- [ ] GitHub Actions workflow running Newman on every push, with the HTML report as an artifact

---

## 🔗 Related Projects

| Project | Stack |
|---|---|
| 🔌 [API — Rest Assured](https://github.com/rbchy/QaBrainsAPIAutomationRestAssured) | Java · Rest Assured · Cucumber · Selenium — CI green |
| ⭐ [RbcTcsWorld E-Commerce Suite](https://github.com/rbchy/RbcTcsWorld_ECommerceSuite) | Spring Boot · REST Assured · contract tests · CI quality gates |
| 🔬 [QA Brains — Java + Playwright](https://github.com/rbchy/QaBrainsAutomationJavaPlaywright) | Java 17 · Playwright · Cucumber JVM · TestNG |
| 🧪 [ClinTrial-Connect](https://github.com/rbchy/RbcTcsWorld_ClinTrial-Connect) | Spring Boot · JUnit 5 · Cucumber · JMeter |

<sub>All projects: [github.com/rbchy](https://github.com/rbchy)</sub>

---

## 👤 Author

<div align="center">

**Ranajit Baran Chowdhury (RB Chowdhury)**
QA Automation Engineer / SDET · West Norriton, Pennsylvania, USA

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rbchy/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/rbchy)
[![Website](https://img.shields.io/badge/Website-rbchy.github.io%2Frbchy-0d9488?style=for-the-badge&logo=githubpages&logoColor=white)](https://rbchy.github.io/rbchy/)
[![Portfolio](https://img.shields.io/badge/Portfolio-Website-blueviolet?style=for-the-badge&logo=wix&logoColor=white)](https://rbc6543.wixsite.com/rbc-portfolio)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:chyranajit@gmail.com)

<sub>Provided for educational and professional QA automation purposes. QA Brains is a public practice site.</sub>

⭐ *If this collection helps you, a star on the repo is appreciated.*

</div>
