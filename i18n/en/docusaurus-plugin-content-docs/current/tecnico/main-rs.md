---
id: main-rs
title: main.rs walkthrough
description: Flow, structures, and design choices of the proxy server.
---

Overview
- Async binary using `warp` as HTTP server and `reqwest` as client.
- CLI by `clap`. Logging via `env_logger`/`log`.
- Supports mocks via TOML, optional CORS, extra headers, and response saving.

Structures and config
- `Config` flags: `--target-url`, `--api-url`, `--add-cors-headers`, `--extra-header`, `--mock-config`, `--save-request-directory`, `--show-headers`, `--show-body` (`main.rs:20`).
- `Mock` and `MockFile` model the TOML file with `[[mocks]]` entries (`main.rs:104`).
- Utility filters inject state: `with_config`, `with_mocks`, `with_client` (`main.rs:110`, `main.rs:115`, `main.rs:122`).

Conditional body loading for mocks
- `load_body_content` reads from disk only if `body` ends with `.json/.txt/.html`; otherwise returns the literal (`main.rs:128`).

Server bootstrap
- Initializes logging, parses CLI, loads mocks when `--mock-config` is provided (`main.rs:155`, `main.rs:165`).
- Parses `--api-url` to resolve `host`/`port` and binds the socket (`main.rs:155`).
- Builds the `route` capturing method, headers, full path, raw query, bytes body, config, mocks, and client; then dispatches to `proxy_handler` (`main.rs:221`).

Main handler: proxy_handler
- Parameters: method, headers, full path, query, body, config, mocks, client (`main.rs:231`).
- Logging: composes a `path?query` string only for logs.

Mock resolution
- If `mocks` exists, finds the first match by `method` and `path` (case-insensitive). Note: it compares only `full_path.as_str()` (no query) (`main.rs:231`).
  - On match: builds response with status, mock headers, optional CORS, and CLI extra headers; body via `load_body_content`.
  - If `--save-request-directory` is enabled, persists the mock response.

Forwarding (no mock)
- Concatenates `--target-url` (trimmed) + path and appends query if present. Copies all incoming headers except `host`. Sends with `reqwest` preserving the body.
- Network/body errors return `502 Bad Gateway` with a text description.
- Logs status/headers/body: hidden by default; `--show-headers` and `--show-body` enable pretty-printed output for JSON.

Response post-processing
- Applies `--extra-header` entries (validated `Name: Value`).
- If `--add-cors-headers`, adds `Access-Control-Allow-*` and sets `Content-Type: application/json` if missing.
- Rebuilds the final response with status, headers, and body.

Persistence and mock generation
- `save_response_to_file` ensures directory, generates a sanitized filename with timestamp, writes beautified JSON, and appends to `mocked-request.toml` (`main.rs:482`).

Important nuance
- Mock matching uses only the path without query, but the generated TOML stores the path including the query. Consider aligning them by adjusting either the saved `path` or the matching logic.

