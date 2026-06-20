# Splunk-SOC-Lab-02-Apache-Web-Log-Analysis

## Overview

This lab demonstrates how to analyze Apache HTTP access logs using Splunk Search Processing Language (SPL).

The objective is to establish a baseline of normal web traffic while identifying unusual patterns that could indicate reconnaissance activity, web scanning, or attempted attacks against a public-facing web server.

---

# Lab Environment

- Windows 10/11 Host
- Splunk Enterprise
- Search & Reporting App
- Apache Access Log Dataset

---

# Scenario

The SOC team has successfully onboarded Apache web server logs into Splunk.

Management has requested a security review of the web traffic to identify suspicious activity and establish a baseline before creating security detections.

As a SOC Analyst, your task is to analyze the logs using SPL and identify noteworthy findings.

---

# Objectives

- Analyze Apache access logs
- Learn basic SPL queries
- Identify top client IP addresses
- Identify frequently requested URLs
- Analyze HTTP methods
- Analyze HTTP status codes
- Visualize web traffic over time
- Hunt for suspicious requests

---

# MITRE ATT&CK Mapping

| Technique | Description |
|------------|------------|
| T1595 | Active Scanning |
| T1190 | Exploit Public-Facing Application |
| T1071.001 | Application Layer Protocol: Web Protocols |

---

# Severity

**Low**

This lab focuses on baseline traffic analysis and threat hunting.

---

# Detection Logic

## Potential Web Reconnaissance

```spl
index=main
| top clientip
```

---

## Frequent 404 Errors

```spl
index=main
status=404
| stats count by clientip
```

---

## Potential Login Probing

```spl
index=main
"/login"
```

---

## Potential Admin Page Access

```spl
index=main
"/admin"
```

---

# False Positives

- Search engine crawlers
- Internal vulnerability scanners
- Health monitoring systems
- Legitimate users requesting unavailable pages
- Automated website indexing services

---

# Recommended Containment

Review the source IP address, validate the request pattern, enable Web Application Firewall (WAF) protections if necessary, and block malicious IPs after confirming suspicious activity.

---

# Step 1: Count Total Events

Run:

```spl
index=main
| stats count
```

---

# Step 2: View Sample Events

Run:

```spl
index=main
| head 20
```

Observe the Apache log structure and available fields.

---

# Step 3: Identify Top Client IP Addresses

Run:

```spl
index=main
| top clientip
```

If the `clientip` field is unavailable:

```spl
index=main
| top host
```

---

# Step 4: Identify Top Requested URLs

Run:

```spl
index=main
| top uri_path
```

If the `uri_path` field is unavailable:

```spl
index=main
| top uri
```

---

# Step 5: Analyze HTTP Methods

Run:

```spl
index=main
| top method
```

Review the HTTP methods observed within the dataset.

---

# Step 6: Analyze HTTP Status Codes

Run:

```spl
index=main
| top status
```

Review common response codes including:

- 200
- 301
- 302
- 403
- 404
- 500

---

# Step 7: Visualize Traffic Over Time

Run:

```spl
index=main
| timechart count
```

Display the results as a line chart to observe traffic patterns.

---

# Step 8: Investigate 404 Errors

Run:

```spl
index=main
status=404
```

Count the events:

```spl
index=main
status=404
| stats count
```

---

# Step 9: Investigate 500 Internal Server Errors

Run:

```spl
index=main
status=500
```

Review any returned events.

---

# Step 10: Hunt for Sensitive URL Access

Run:

```spl
index=main
("/admin" OR "/login" OR "/config" OR "/backup")
```

Review any matching events for suspicious activity.

---

# Skills Demonstrated

- Splunk Enterprise
- Splunk Search Processing Language (SPL)
- SIEM Investigation
- Apache Log Analysis
- Web Traffic Analysis
- Threat Hunting
- Security Monitoring
- Baseline Analysis

---

# Lessons Learned

This lab improves understanding of:

- Apache access log analysis
- SPL fundamentals
- HTTP traffic analysis
- Threat hunting methodology
- Baseline establishment
- Security monitoring
- SOC investigation workflow
- Detection development fundamentals
