id: extra-header
title: Cabeceras extra
sidebar_label: Cabeceras extra
sidebar_position: 4
---

Agrega cabeceras personalizadas a la respuesta. Puede repetirse.

- Requerido: no
- Formato: `Nombre: Valor` (con dos puntos). Entradas inválidas se ignoran y se registran con warning.

Ejemplos
- ```bash
  proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' \
    -e 'x-proxy-bob: yes' -e 'x-proxy-alice: no'
  ```
