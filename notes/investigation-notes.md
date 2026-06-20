# Investigation Notes

## Overview

This document records observations made during the analysis of Apache HTTP access logs using Splunk Search Processing Language (SPL).

The objective was to establish a baseline of normal web traffic and identify any suspicious patterns.

---

# Dataset

Apache HTTP Access Logs

Index:

```
main
```

---

# SPL Queries Executed

## Count Total Events

```spl
index=main
| stats count
```

---

## View Sample Events

```spl
index=main
| head 20
```

---

## Top Client IP Addresses

```spl
index=main
| top clientip
```

If unavailable:

```spl
index=main
| top host
```

---

## Top Requested URLs

```spl
index=main
| top uri_path
```

If unavailable:

```spl
index=main
| top uri
```

---

## HTTP Methods

```spl
index=main
| top method
```

---

## HTTP Status Codes

```spl
index=main
| top status
```

---

## Traffic Trend

```spl
index=main
| timechart count
```

---

## HTTP 404 Errors

```spl
index=main
status=404
```

---

## HTTP 500 Errors

```spl
index=main
status=500
```

---

## Sensitive URL Search

```spl
index=main
("/admin" OR "/login" OR "/config" OR "/backup")
```

---

# Findings

Record:

- Total events analyzed
- Top source IP
- Top requested URL
- Common HTTP methods
- Common HTTP status codes
- Number of 404 errors
- Number of 500 errors
- Suspicious URLs observed
- Unusual traffic spikes

---

# Analyst Summary

Summarize any suspicious behavior observed during the investigation and document whether further analysis is required.
