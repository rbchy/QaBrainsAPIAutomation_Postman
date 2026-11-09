<div align="center">

# 🔌 QaBrains API Automation — Postman / Newman

### Postman collection for the QA Brains practice site — 130 scripted assertions, runnable headless with Newman

[![Typing SVG](https://readme-typing-svg.demolab.com/?font=Fira+Code&size=18&pause=1500&color=FFC107&center=true&vCenter=true&width=900&repeat=true&lines=Postman+%C2%B7+Newman+%C2%B7+JavaScript+test+scripts;Login+%C2%B7+Registration+%C2%B7+Forgot+Password+%C2%B7+E-Commerce;130+pm.test+assertions%3A+positive%2C+negative%2C+extended)](https://git.io/typing-svg)

<!-- ============ STATUS BAR ============ -->
[![Tests](https://github.com/rbchy/QaBrainsAPIAutomation_Postman/actions/workflows/tests.yml/badge.svg)](https://github.com/rbchy/QaBrainsAPIAutomation_Postman/actions/workflows/tests.yml)
![CI Assertions](https://img.shields.io/badge/CI%20assertions-40%20%2F%2040%20passing-22c55e?style=for-the-badge)
![Assertions](https://img.shields.io/badge/pm.test%20Assertions-130-155e75?style=for-the-badge)
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
| 🧪 **Assertions** | 130 `pm.test` blocks · 40 run in CI |
| ✅ **CI** | Newman on every push · 40 / 40 passing |
| 🔁 **Data-driven** | Registration reads `pm.iterationData` |
| ▶️ **Runner** | Postman Collection Runner · Newman (`npm test`) |

</td>
</tr>
</table>

> ℹ️ **About the practice site:** practice.qabrains.com has **no REST API** today — every `/api/...` path returns a 404 page. So the collection has two folders: **Practice Site Pages** (endpoints that exist, run in CI) and **Auth API** (requests and scripts kept ready for a real API, not run in CI).

---

## 🌐 Requests and Coverage

### 📄 Practice Site Pages — runs in CI

| # | Request | Method & endpoint | Assertions | What the script checks |
|---|---|---|---:|---|
| 1 | 🔑 **Forgot Password** | `POST {{baseUrl}}/forgot-password` | 20 | TC-01 – TC-20: status, timing, body, headers, injection and leakage checks on an HTML answer |
| 2 | 🛒 **E-Commerce Site** | `POST {{baseUrl}}/ecommerce-site` | 20 | TC-01 – TC-20: positive login indicators, negative cases, SQL / XSS reflection, data leakage |
| | **CI total** | | **40** | ![passed](https://img.shields.io/badge/-40%20%2F%2040%20passed-22c55e?style=flat-square) |

### 🔐 Auth API — ready, not run (no API on the practice site yet)

| # | Request | Method & endpoint | Assertions | What the script checks |
|---|---|---|---:|---|
| 3 | **LOGIN API (Save Token)** | `POST {{baseUrl}}/api/login` | 30 | Status, token present and saved to the environment, no validation errors, no leakage |
| 4 | **Registration** | `POST {{baseUrl}}/api/register` | 30 | TC01–TC10 positive · TC11–TC20 negative · TC21–TC30 extended; outcome from iteration data |
| 5 | **Update User (PUT)** | `PUT {{baseUrl}}/api/users/{{user_id}}` | 30 | 2xx, JSON object, id kept, name and e-mail echoed, no error keys |
| | **Collection total** | | **130** | |

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

## 🧹 Maintenance Log

| Change | Why |
|---|---|
| `const responseBody` → `bodyText` in two scripts | Postman already defines a global `responseBody`; the redeclaration threw a `SyntaxError`, so **none** of those 40 assertions had ever run |
| *LOGIN API* now `POST`s `{{email}}` / `{{password}}` | It sent `PUT {"name": "Updated Name"}`, which belonged to the update script |
| *New Request* → **Update User (PUT)** with the body its script checks | Request and script now match |
| Empty *Untitled Request* removed | Clutter in runner output |
| `{{baseUrl}}` everywhere, environment file added | No hard-coded URLs; variables now have values |
| Personal e-mail in Forgot Password → `{{email}}` (public practice account) | No personal data in the collection |
| Two folders; CI runs **Practice Site Pages** | The practice site returns 404 for every `/api/...` path |

---

## ⚙️ Running the Collection

**Prerequisites:** Postman (desktop) or Node.js 18+ for Newman.

<details open>
<summary><strong>Postman GUI</strong></summary>

<br>

1. **Import** → `QAbrainsAutomation_API.postman_collection.json` and `environments/QaBrains_Practice.postman_environment.json`
2. Select the **QaBrains Practice** environment
3. **Run** the *Practice Site Pages* folder (or the whole collection)

</details>

<details>
<summary><strong>Newman (headless)</strong></summary>

<br>

```bash
npm install          # newman + htmlextra from package.json
npm test             # Practice Site Pages → reports/newman-report.html and newman-junit.xml
npm run test:all     # every request, including the Auth API folder
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
| Data | `pm.iterationData`, environment file | Data-driven registration cases, practice account |
| CI | GitHub Actions | Newman on every push, HTML + JUnit reports as artifacts |

---

## 📁 Repository Contents

```
QaBrainsAPIAutomation_Postman/
├── QAbrainsAutomation_API.postman_collection.json   ← 2 folders, 5 requests, 130 assertions
├── environments/QaBrains_Practice.postman_environment.json
├── package.json                                     ← newman, htmlextra, npm test
├── .github/workflows/tests.yml                      ← Newman on every push
└── README.md
```

---

## 📈 Roadmap

- [x] Environment file, `{{baseUrl}}`, fixed login and update requests
- [x] `package.json` and GitHub Actions running Newman on every push
- [ ] Point the Auth API folder at a real practice API and add it to CI
- [ ] Registration data file (`tc_id`, `expected_result`) for the 30 data-driven cases
- [ ] Publish the htmlextra report to GitHub Pages

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
