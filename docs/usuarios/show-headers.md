---
id: show-headers
title: Mostrar cabeceras en logs
sidebar_label: Mostrar cabeceras
sidebar_position: 7
slug: /usuarios/show-headers
---

Muestra las cabeceras de la solicitud entrante en los logs (por defecto están ocultas).

- Requerido: no (flag booleano)
- Útil para depurar, pero evita exponer datos sensibles al habilitarlo.

Ejemplo
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -h
```
