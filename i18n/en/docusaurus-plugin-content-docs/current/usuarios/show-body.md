---
id: show-body
title: Show body in logs
sidebar_label: Show body
sidebar_position: 8
---

Shows the response body in logs. If it’s valid JSON, it is pretty-printed.

- Required: no (boolean)
- Hidden by default to protect sensitive data.

Example
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -b
```

