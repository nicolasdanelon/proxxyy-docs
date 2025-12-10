---
id: mock-config
title: Mocks desde archivo TOML
sidebar_label: Mocks (archivo TOML)
sidebar_position: 5
slug: /usuarios/mock-config
---

Ruta a un archivo TOML con definiciones de mocks. Si hay match, la respuesta no se reenvía: se devuelve el mock.

- Requerido: no
- Estructura: `[[mocks]]` con `method`, `path`, `status` (opcional, default 200), `body` y `headers` opcionales.
- Carga de `body` desde archivo: si termina en `.json`, `.txt` o `.html`, se intenta leer el contenido de ese archivo; si falla o no coincide la extensión, se usa el texto literal.

Ejemplo de archivo
```toml
[[mocks]]
method = "GET"
path = "/test"
status = 200
body = "Hello from test!"

[mocks.headers]
Content-Type = "text/plain"
```

Ejemplo de uso
```bash
proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -m './mocks.toml'
```

Nota importante
- El matching actual usa solo el `path` (sin query). Si tus mocks incluyen query en `path`, podrían no coincidir a menos que ajustes el archivo o la lógica.
