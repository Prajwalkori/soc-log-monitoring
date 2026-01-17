# SOC Log Monitoring and Threat Detection Dashboard

A Splunk-based SOC dashboard designed to detect common attack behaviors such as authentication abuse, web reconnaissance, and abnormal traffic patterns using centralized log data.

This project focuses on **visibility and detection logic**, not alerting automation.

---

## 📌 Objective

To build a clean, analyst-friendly dashboard that highlights:
- SSH brute force attempts
- Web directory enumeration (404 abuse)
- High request volume by source IP
- Reconnaissance via robots.txt access

The dashboard simulates real SOC monitoring scenarios using sample log data.

---

## 🧱 Data Source

- Index: `soc_logs`
- Log types:
  - Linux authentication logs
  - Web access logs
- Sample data derived from publicly available GitHub datasets

---

## 📊 Dashboard Panels

### 1. SSH Brute Force — Top Attacking IPs
**Purpose:**  
Identify source IPs generating repeated SSH authentication failures.

**Logic:**
```spl
index=soc_logs "authentication failure"
| stats count as attempts by src_ip
| sort - attempts
