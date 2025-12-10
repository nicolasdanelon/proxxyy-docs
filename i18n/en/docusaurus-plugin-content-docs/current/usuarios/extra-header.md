---
id: extra-header
title: Custom headers
sidebar_label: Custom headers
sidebar_position: 4
---

Adds custom headers to the response. Repeatable.

- Required: no
- Format: `Name: Value` (with colon). Invalid entries are ignored and logged as warnings.

Examples
- ```bash
  proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' \
    -e 'x-proxy-bob: yes' -e 'x-proxy-alice: no'
  ```

