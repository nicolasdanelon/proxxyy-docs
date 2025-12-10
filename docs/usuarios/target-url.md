id: target-url
title: URL de destino
sidebar_label: URL de destino
sidebar_position: 1
---

Define la URL base de destino a la que se reenvían las solicitudes entrantes.

- Requerido: sí
- Valor: URL absoluta (sin barra final obligatoria; se normaliza internamente)

Ejemplos
- ```bash
  proxxyy -t 'https://api.example.com/api' -u 'http://localhost:6969'
  ```
- Con query en la solicitud de cliente, proxxyy la conserva al reenviar: `/users?page=1` → `https://api.example.com/api/users?page=1`.
