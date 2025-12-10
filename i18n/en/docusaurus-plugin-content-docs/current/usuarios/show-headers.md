---
id: show-headers
title: Show headers in logs
sidebar_label: Show headers
sidebar_position: 7
---

Shows incoming request headers in logs (hidden by default).

- Required: no (boolean)
- Useful for debugging; avoid exposing sensitive data when enabling.

Example
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -h
```

