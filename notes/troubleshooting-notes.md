# Troubleshooting Notes

## Overview

This document records issues encountered during the Apache Web Log Analysis Lab.

---

# Issue 1

## Problem

No results returned for:

```spl
index=main
status=500
```

## Cause

The dataset contains no HTTP 500 Internal Server Error responses.

## Resolution

This is a valid finding and indicates that no internal server errors were observed during the captured period.

---

# Issue 2

## Problem

No results returned for:

```spl
index=main
| top clientip
```

## Cause

The `clientip` field may not have been extracted automatically.

## Resolution

Verify extracted fields or use:

```spl
index=main
| top host
```

Alternatively, create custom field extractions.

---

# Issue 3

## Problem

No results returned for:

```spl
index=main
| top uri_path
```

## Cause

The `uri_path` field may not exist in the imported dataset.

## Resolution

Use:

```spl
index=main
| top uri
```

or inspect available fields in Splunk.

---

# Issue 4

## Problem

No results returned for:

```spl
index=main
| top method
```

## Cause

HTTP methods were not extracted into separate fields.

## Resolution

Review the raw logs or create field extractions for the request line.

---

# General Troubleshooting

Verify total events:

```spl
index=main
| stats count
```

View raw events:

```spl
index=main
```

Check available fields:

```spl
index=main
| head 1
```

Verify sourcetype:

```spl
index=main
| stats count by sourcetype
```

Verify source:

```spl
index=main
| stats count by source
```

---

# Lessons Learned

- Field extraction depends on the sourcetype.
- Not every dataset contains HTTP 500 errors.
- Missing fields can often be resolved with custom extractions.
- Always verify available fields before writing SPL queries.
- Understanding the dataset structure is essential for effective threat hunting.
