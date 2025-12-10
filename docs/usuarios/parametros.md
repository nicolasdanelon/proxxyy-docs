---
id: parametros
title: Parámetros y uso
description: Opciones de línea de comandos de proxxyy y ejemplos prácticos.
---

Esta guía explica cómo ejecutar proxxyy y documenta todos los parámetros disponibles.

Nota: Los nombres largos de flags reflejan exactamente lo que implementa el binario (main.rs). Los atajos cortos están entre paréntesis.

Parámetros principales
- `--target-url` (`-t`): URL de destino a la que se reenvían las solicitudes.
  - Ej.: `-t 'https://api.example.com/api'`
- `--api-url` (`-u`): URL donde escucha el proxy (host y puerto).
  - Ej.: `-u 'http://localhost:6969'`

CORS y cabeceras
- `--add-cors-headers` (`-c`): Agrega cabeceras CORS por defecto a la respuesta.
- `--extra-header` (`-e`) repetible: Cabeceras extra para incluir en la respuesta, formato `Nombre: Valor`.
  - Ej.: `-e 'x-proxy-bob: yes' -e 'x-proxy-alice: no'`

Mocks
- `--mock-config` (`-m`): Ruta a un archivo TOML con mocks. Cada entrada se define como `[[mocks]]` con `method`, `path`, `status`, `body` y cabeceras opcionales.
  - Ej.: `-m './mocks.toml'`
  - Si `body` termina en `.json`, `.txt` o `.html`, proxxyy intenta leer ese archivo y usar su contenido como cuerpo de la respuesta; de lo contrario usa el texto literal.

Guardado de solicitudes y respuestas
- `--save-request-directory` (`-s`): Directorio donde se guardan respuestas en archivos y se mantiene un TOML acumulado de mocks (`mocked-request.toml`).
  - Se crean archivos JSON con marca de tiempo y un TOML con entradas `[[mocks]]` para cada solicitud capturada.

Logging
- `--show-headers` (`-h`): Muestra cabeceras de la solicitud en logs. Por defecto están ocultas.
- `--show-body` (`-b`): Muestra el cuerpo de la respuesta en logs (se intenta embellecer si es JSON). Por defecto está oculto.
- Ajusta el nivel con `RUST_LOG`, por ejemplo: `RUST_LOG=info`.

Ejemplos
- Mínimo:
  - ```bash
    proxxyy -t 'https://api.example.com/api' -u 'http://localhost:6969'
    ```
- Con CORS + cabeceras extra + mocks:
  - ```bash
    proxxyy -c \
      -t 'https://api.example.com/api' \
      -u 'http://localhost:6969' \
      -e 'x-proxy-bob: yes' \
      -e 'x-proxy-alice: no' \
      -m './mocks.toml'
    ```
- Guardando respuestas para generar mocks:
  - ```bash
    proxxyy -t 'https://api.example.com' -u 'http://localhost:6969' -s './saved_requests'
    ```

Formato de mocks (TOML)
```toml
[[mocks]]
method = "GET"
path = "/test"
status = 200
body = "Hello from test!"

[mocks.headers]
Content-Type = "text/plain"
```

Sugerencias
- Si referencias archivos en `body` (p. ej. `data.json`), asegúrate de que la ruta sea válida según tu directorio de ejecución.
- Cuando `--save-request-directory` está activo, se guardan JSON embellecidos y se agrega/actualiza `mocked-request.toml` con nuevas entradas.

