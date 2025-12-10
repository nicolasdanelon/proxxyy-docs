---
id: add-cors-headers
title: CORS
sidebar_label: CORS
sidebar_position: 3
---

Adds default CORS headers to responses: `Access-Control-Allow-Origin`, `-Methods`, `-Headers`, and `Content-Type: application/json` if missing.

- Required: no (boolean flag)
- Applies to both proxied and mock responses.

Example
- ```bash
  proxxyy -c -t 'https://api.example.com' -u 'http://localhost:6969'
  ```

