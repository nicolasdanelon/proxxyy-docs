---
id: save-request-directory
title: Guardar respuestas y generar mocks
sidebar_label: Guardar respuestas
sidebar_position: 6
slug: /usuarios/save-request-directory
---

Guarda respuestas en archivos y genera/actualiza un TOML de mocks (`mocked-request.toml`) en el directorio indicado.

- Requerido: no
- Archivos generados: JSON (embellecido si es válido) con timestamp en el nombre; TOML acumulado con entradas `[[mocks]]` para cada captura.

Ejemplo
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -s './saved_requests'
```

Notas
- El TOML generado guarda `path` incluyendo query, lo que puede diferir del matching actual (que no usa query).
- Se crean los directorios si no existen. Los nombres de archivo se sanitizan.
