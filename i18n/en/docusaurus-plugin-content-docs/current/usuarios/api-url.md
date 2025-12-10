---
id: api-url
title: Server URL (listen)
sidebar_label: Server URL
sidebar_position: 2
---

Defines the URL where proxxyy listens (host and port).

- Required: yes
- Value: absolute http(s) URL. Used to resolve host/port.

Examples
- ```bash
  proxxyy -t 'https://api.example.com' -u 'http://localhost:6969'
  ```
- If you use `localhost`, it binds to `127.0.0.1` with the given port.

