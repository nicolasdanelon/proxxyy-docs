---
id: show-body
title: Mostrar cuerpo en logs
sidebar_label: Mostrar cuerpo
sidebar_position: 8
slug: /usuarios/show-body
---

Muestra el cuerpo de la respuesta en los logs. Si es JSON válido, se embellece (pretty-print).

- Requerido: no (flag booleano)
- Por defecto se oculta para proteger datos sensibles.

Ejemplo
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -b
```
