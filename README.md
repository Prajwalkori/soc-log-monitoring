# SOC Log Monitoring Dashboard (Splunk)
A Splunk-based SOC monitoring dashboard focused on **log visibility and analyst-driven investigation**.  
The project demonstrates how centralized logs can be used to identify suspicious patterns related to authentication abuse, web reconnaissance, and abnormal traffic behavior.
This project intentionally focuses on **monitoring and visualization**, not alerting or automated response.
---
## 📷 Dashboard Preview

![SOC Log Monitoring Dashboard](screenshots/overiview.png)
--
## 🎯 Objective
To design a clean, SOC-style dashboard that helps an analyst quickly answer:
- Who is attempting repeated authentication failures?
- Who is probing the web application for hidden paths?
- Which IPs are generating unusually high traffic?
- Which sources are performing early reconnaissance activity?
The goal is **situational awareness**, not automated threat blocking.
---
## 🧱 Data Sources
- **Splunk Index:** `soc_logs`
- **Log Types Used:**
  - Linux authentication logs (SSH / PAM failures)
  - Web server access logs
- **Data Origin:**  
  Publicly available sample log datasets sourced from GitHub repositories
---
## 📊 Dashboard Panels
### 1. SSH Brute Force — Top Attacking IPs
**Purpose:**  
Provide visibility into repeated SSH authentication failures from the same source IPs.
**SPL Logic:**
```spl
index=soc_logs "authentication failure"
| stats count as attempts by rhost
| sort - attempts
```
**What it shows:**
Source IPs with high volumes of failed SSH login attempts, supporting investigation of potential brute-force or credential-guessing behavior.
### 2. Web Directory Enumeration (404 Abuse)
**Purpose:**
Identify potential directory scanning or forced browsing activity against the web application.
**SPL Logic:**
```spl
index=soc_logs status=404
| stats count as hits by clientip
| sort - hits
```
**What it shows:**
Clients generating large numbers of 404 responses, commonly associated with automated scanners.
### 3. High Request Volume by Source IP
**Purpose:**
Highlight source IPs generating unusually high volumes of HTTP requests.
**SPL Logic:**
```spl
index=soc_logs
| stats count as requests by clientip
| sort - requests
```
**What it shows:**
Traffic patterns that may indicate bots, aggressive crawlers, or abnormal usage.
### 4. robots.txt Recon Activity
**Purpose:**
Monitor access to robots.txt, which is often requested during early reconnaissance.
**SPL Logic:**
```spl
index=soc_logs uri_path="/robots.txt"
| stats count as hits by clientip
| sort - hits
```
**What it shows:**
IP addresses performing reconnaissance that may precede more targeted attacks.
---
## 🎨 Dashboard Design Choices
- **Dark theme** aligned with SOC monitoring environments
- **Bar and area charts** for fast comparison
- **Ranking-based views (Top IPs)** for prioritization
- **No alerting or automation** to keep focus on analyst review
- **Report-backed panels** for stability and consistency
## ⚠️ Scope and Limitations
- This project provides **visibility only**
- No alerting, correlation, or automated response implemented
- Thresholds are illustrative, not enforced
- Uses sample data rather than live production traffic
- Not intended to represent a fully operational SOC deployment
## 🚀 Future Enhancements
- Add scheduled alerts with tuned thresholds
- Correlate authentication and web activity into incidents
- Map detections to MITRE ATT&CK techniques
- Enrich IPs with geo, ASN, and reputation data
- Integrate with WAF or firewall controls
## 📷 Screenshots
Dashboard and panel screenshots are available in the `screenshots/` directory.
## 👤 Author
Built as a SOC analyst portfolio project to demonstrate:
- Log analysis
- SPL querying
- Security-focused dashboard design
- Analyst-oriented visibility using Splunk
