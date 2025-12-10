---
id: mock-config
title: Mocks from TOML file
sidebar_label: Mocks (TOML file)
sidebar_position: 5
---

Path to a TOML file with mock definitions. If a mock matches, the request is not forwarded.

- Required: no
- Structure: `[[mocks]]` with `method`, `path`, `status` (optional, default 200), `body`, and optional `headers`.
- Body loading from file: if it ends with `.json`, `.txt`, or `.html`, proxxyy attempts to read the file; otherwise uses the literal.

Example file
```toml
[[mocks]]
method = "GET"
path = "/test"
status = 200
body = "Hello from test!"

[mocks.headers]
Content-Type = "text/plain"
```

Example usage
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -m './mocks.toml'
```

Important note
- Matching currently uses only the path (without query). If your mocks include a query in `path`, they may not match unless you adjust the file or logic.

