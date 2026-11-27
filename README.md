# DNCC BlazeMeter Performance Test

![Apache JMeter](https://img.shields.io/badge/Tool-Apache%20JMeter-red)
![Test Type](https://img.shields.io/badge/Test-Baseline%20Performance-blue)
![Report](https://img.shields.io/badge/Report-HTML%20Dashboard-green)

A performance testing project for the **DNCC e-Revenue portal** (`erevenue.dncc.gov.bd`), carried out with **Apache JMeter**. This repository contains the raw test result file, the auto-generated **JMeter HTML Dashboard report**, screenshots of the report, and an analysis of the findings.

---

## 📑 Table of Contents

1. [Project Overview](#-project-overview)
2. [Objectives](#-objectives)
3. [Scope of Testing](#-scope-of-testing)
4. [Tools & Technologies](#-tools--technologies)
5. [Test Information](#-test-information)
6. [Test Scenario & Configuration](#-test-scenario--configuration)
7. [Understanding the Metrics](#-understanding-the-metrics)
8. [Report Sections Explained](#-report-sections-explained)
9. [Report Screenshots](#-report-screenshots)
10. [Test Results Summary](#-test-results-summary)
11. [Analysis & Observations](#-analysis--observations)
12. [Conclusion](#-conclusion)
13. [Recommendations](#-recommendations)
14. [Project Structure](#-project-structure)
15. [How to View the Report](#-how-to-view-the-report)
16. [How to Re-run the Test](#-how-to-re-run-the-test)
17. [Author](#-author)

---

## 📌 Project Overview

Performance testing checks how fast, stable and scalable a web application is when many users access it at the same time. In this project, **Apache JMeter** was used to replay a real user journey on the DNCC e-Revenue portal (**open login page → select user type → login → OTP verification**) and measure how fast each step responds. The scenario was recorded in the browser, converted into a JMeter test plan (`DNCC.jmx`), executed, and the results were saved in a `.jtl` file (`DNCC.jtl`).

From that file, JMeter generated an **HTML Dashboard Report** (`index.html`) showing response times, throughput, errors, APDEX score and various charts. This repository stores that report so anyone can open it and understand how the application performed under the tested load.

## 🎯 Objectives

- Measure the **response time** of the application under load
- Measure **throughput** (how many requests the server handles per second)
- Detect **errors and failed requests** and find their causes
- Evaluate user satisfaction using the **APDEX** score
- Find possible **bottlenecks** and slow pages/APIs
- Provide a documented performance baseline for future comparison

## 🔍 Scope of Testing

**Included**
- The user login flow: login page load, user-type selection, login submit and OTP verification (4 HTTP requests)
- Response time, throughput and error analysis for each request

**Not included**
- Security testing
- Functional/UI testing
- Multi-user load / stress / endurance testing (this run used a single user)
- Database or server-side internal monitoring

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Apache JMeter | Create and run the load test |
| JMeter HTML Dashboard | Generate the visual report from the `.jtl` file |
| SB Admin 2 / Bootstrap | Dashboard theme |
| Flot Charts | Charts in the report |
| HTML / CSS / JavaScript | Report pages |
| Git & GitHub | Version control and hosting |

## 🗂️ Test Information

| Item | Details |
|------|---------|
| Application tested | DNCC e-Revenue portal (`erevenue.dncc.gov.bd`) |
| Test plan | `DNCC.jmx` (JMeter 5.6.3) |
| Source file | `DNCC.jtl` |
| Test date | 27 November 2026 |
| Start time | 7:51 PM |
| End time | 7:52 PM |
| Duration | About 1 minute |
| Filter for display | None (all samples included) |
| Report type | Apache JMeter HTML Dashboard |

## ⚙️ Test Scenario & Configuration

| Parameter | Value |
|-----------|-------|
| Test plan file | `DNCC.jmx` (created with JMeter 5.6.3, recorded and converted via BlazeMeter converter) |
| Number of Threads (Users) | 1 |
| Ramp-up Period | 1 second |
| Loop Count | 1 |
| Base URL | `erevenue.dncc.gov.bd` (HTTPS) |
| Controller | Transaction Controller named **Test** (groups all 4 requests) |
| On sample error | Continue |
| Timer | Constant Timer of 4753 ms after the first POST (other timers disabled) |
| Assertions | None |
| Listener | View Results Tree |
| Config elements | HTTP Header Manager (Chrome 157 / Windows headers), HTTP Request Defaults, DNS Cache Manager, HTTP Cookie Manager, HTTP Cache Manager (all cleared each iteration) |

**Requests (samplers) in the test:**

| # | Request | Method | Path | Purpose |
|---|---------|--------|------|---------|
| 1 | Open login page | GET | `/web/userulocknew.aspx` | Loads the login entry page for the user |
| 2 | Select user type | POST | `/newv/Default` | Form postback after selecting the user type from the dropdown |
| 3 | Login | POST | `/newv/Default` | Submits the login form |
| 4 | OTP verification | POST | `/newv/otpverification` | Submits the one-time password to verify the login |

**Test flow:**
1. One virtual user is started (ramp-up of 1 second).
2. The user sends the 4 HTTP requests in order: login page → user type → login → OTP verification.
3. JMeter records response time, status, bytes and errors for every request.
4. Results are saved into `DNCC.jtl`.
5. The HTML dashboard is generated from the `.jtl` file.

## 📖 Understanding the Metrics

| Metric | Meaning |
|--------|---------|
| **Samples** | Total number of requests sent |
| **Average** | Mean response time of all requests |
| **Min / Max** | Fastest and slowest response time |
| **Median** | Middle response time (50% of requests were faster) |
| **90th / 95th / 99th pct** | 90%/95%/99% of requests completed within this time |
| **Error %** | Percentage of failed requests |
| **Throughput** | Requests handled per second (or minute) |
| **Received / Sent KB/s** | Network data received from / sent to the server |
| **APDEX** | User satisfaction score from 0 to 1 (closer to 1 is better) |

**APDEX rating guide**

| Score | Rating |
|-------|--------|
| 0.94 – 1.00 | Excellent |
| 0.85 – 0.93 | Good |
| 0.70 – 0.84 | Fair |
| 0.50 – 0.69 | Poor |
| Below 0.50 | Unacceptable |

## 🧭 Report Sections Explained

The dashboard (`index.html`) contains the following sections:

- **Test and Report Information** – source file, start time, end time and applied filter.
- **APDEX** – satisfaction score per request, based on tolerated and frustrated time thresholds.
- **Requests Summary** – pie chart showing the share of passed and failed requests.
- **Statistics** – table of samples, error %, average/min/max, percentiles and throughput for every request.
- **Errors** – types of errors that occurred and how many times.
- **Top 5 Errors by Sampler** – the requests that produced the most errors.
- **Charts → Over Time** – response times over time, active threads, latencies, bytes throughput.
- **Charts → Throughput** – hits per second, codes per second, transactions per second.
- **Charts → Response Times** – percentiles, response time distribution and overview.
- **Customs Graphs** – custom graph view over time.

## 📸 Report Screenshots

### 1. Dashboard Overview (Test Information)
![Dashboard Overview](screenshots/dashboard.png)

### 2. APDEX and Requests Summary
![APDEX and Requests Summary](screenshots/apdex-summary.png)

### 3. Statistics Table
![Statistics](screenshots/statistics.png)

### 4. Errors and Top 5 Errors by Sampler
![Errors](screenshots/errors.png)

### 5. Charts
![Response Times Over Time](screenshots/over-time.png)
![Throughput](screenshots/throughput.png)
![Response Times](screenshots/response-times.png)

> 📝 Place all screenshots inside the `screenshots/` folder with the file names above.

## 📊 Test Results Summary

> Fill in these values from the dashboard report.

| Metric | Result |
|--------|--------|
| Total Samples | - |
| Passed Requests | - |
| Failed Requests | - |
| Error % | - |
| Average Response Time | - ms |
| Median Response Time | - ms |
| 90th Percentile | - ms |
| 95th Percentile | - ms |
| 99th Percentile | - ms |
| Min / Max Response Time | - ms / - ms |
| Throughput | - req/sec |
| APDEX Score | - |

## 🔎 Analysis & Observations

> Write your own findings here after reviewing the report. Example points:

- **Response time:** Was the average response time acceptable (for example under 2–3 seconds)?
- **Errors:** Which requests failed, and what were the error types (e.g. 500, 503, timeout, connection refused)?
- **Throughput:** What was the request rate for this single-user run?
- **Slowest requests:** Which pages/APIs had the highest response time?
- **Login flow:** Which of the 4 steps (page load, user type, login, OTP) took the longest?

## ✅ Conclusion

> Summarize the result in a few lines, for example:
> The application handled the tested load with an error rate of **X%** and an average response time of **Y ms**. The APDEX score of **Z** indicates **(excellent / good / fair / poor)** user experience under this load.

## 💡 Recommendations

- Optimize the slowest requests identified in the Statistics table
- Investigate and fix the requests listed in *Top 5 Errors by Sampler*
- Use caching and compression for static content
- Review server capacity if errors increase with load
- Repeat the test with more virtual users (for example 10, 50, 100) and a longer duration for real load, stress and endurance testing

## 📁 Project Structure

```
DNCC-JMeter-Performance-Test/
├── index.html           # Main JMeter dashboard report
├── content/             # Report CSS, JS, graphs and chart pages
├── sbadmin2-1.0.7/      # Dashboard theme and libraries
├── DNCC.jmx             # JMeter test plan (remove credentials before uploading)
├── DNCC.jtl             # Raw JMeter results (optional)
├── screenshots/         # Screenshots of the report
└── README.md            # Project documentation
```

## 🌐 How to View the Report

**Option 1: Locally**
```bash
git clone https://github.com/<your-username>/DNCC-JMeter-Performance-Test.git
cd DNCC-JMeter-Performance-Test
```
Then open `index.html` in any web browser.

**Option 2: GitHub Pages**
Go to *Settings → Pages*, select the `main` branch and save. The report will be available at:
`https://<your-username>.github.io/DNCC-JMeter-Performance-Test/`

## ▶️ How to Re-run the Test

1. Install Java and Apache JMeter.
2. Run the test in non-GUI mode and save results:
   ```bash
   jmeter -n -t DNCC.jmx -l DNCC.jtl   # use a new .jtl file name or delete the old one first
   ```
3. Generate the HTML dashboard:
   ```bash
   jmeter -g DNCC.jtl -o report/
   ```
4. Open `report/index.html` in a browser.

## 👤 Author

**Your Name**
GitHub: [@your-username](https://github.com/your-username)
Email: your-email@example.com

---

⭐ If you find this project useful, feel free to star the repository.
