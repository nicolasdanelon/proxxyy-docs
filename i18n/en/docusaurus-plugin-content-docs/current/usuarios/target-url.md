---
id: target-url
title: Target URL
sidebar_label: Target URL
sidebar_position: 1
---

Defines the base URL where incoming requests are forwarded.

- Required: yes
- Value: absolute URL (trailing slash not required)

Examples
- ```bash
  proxxyy -t 'https://api.example.com/api' -u 'http://localhost:6969'
  ```
- If the client request has a query string, proxxyy preserves it when forwarding.

