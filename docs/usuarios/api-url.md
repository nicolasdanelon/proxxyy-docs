id: api-url
title: URL del servidor (escucha)
sidebar_label: URL del servidor
sidebar_position: 2
---

Define la URL donde proxxyy escucha (host y puerto).

- Requerido: sí
- Valor: URL absoluta con esquema http(s). Se usa para resolver `host`/`port` del servidor.

Ejemplos
- ```bash
  proxxyy -t 'https://api.example.com' -u 'http://localhost:6969'
  ```
- Si usas `localhost`, internamente se enlaza a `127.0.0.1` con el puerto indicado.
