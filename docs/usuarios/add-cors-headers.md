id: add-cors-headers
title: CORS
sidebar_label: CORS
sidebar_position: 3
---

Agrega cabeceras CORS por defecto a las respuestas: `Access-Control-Allow-Origin`, `-Methods`, `-Headers` y `Content-Type: application/json` si faltara.

- Requerido: no (flag booleano)
- Efecto: se aplica tanto en respuestas proxied como en respuestas mock.

Ejemplo
- ```bash
  proxxyy -c -t 'https://api.example.com' -u 'http://localhost:6969'
  ```
