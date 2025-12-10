---
id: save-request-directory
title: Save responses and generate mocks
sidebar_label: Save responses
sidebar_position: 6
---

Saves responses to files and creates/updates a TOML (`mocked-request.toml`) in the given directory.

- Required: no
- Generated files: beautified JSON with a timestamp in the filename; an accumulating TOML with `[[mocks]]` entries for each capture.

Example
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -s './saved_requests'
```

Notes
- The generated TOML stores `path` including the query, which can differ from current matching (which ignores the query).
- Directories are created if missing. Filenames are sanitized.

