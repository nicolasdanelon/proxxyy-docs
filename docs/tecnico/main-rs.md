---
id: main-rs
title: Explicación de main.rs
description: Flujo, estructuras y decisiones de diseño del servidor proxy.
---

Descripción general
- Binario asincrónico con `warp` como servidor HTTP y `reqwest` como cliente para reenviar.
- CLI con `clap` para parámetros. Logging con `env_logger`/`log`.
- Soporta mocks via TOML, CORS opcional, cabeceras extra y guardado de respuestas.

Estructuras y configuración
- `Config` define los flags: `--target-url`, `--api-url`, `--add-cors-headers`, `--extra-header`, `--mock-config`, `--save-request-directory`, `--show-headers`, `--show-body` (`main.rs:20`).
- `Mock` y `MockFile` modelan el archivo TOML con `[[mocks]]` y sus campos (`main.rs:104`).
- Filtros utilitarios inyectan estado en filtros de warp: `with_config`, `with_mocks`, `with_client` (`main.rs:110`, `main.rs:115`, `main.rs:122`).

Carga condicional del cuerpo de mocks
- `load_body_content` lee el contenido desde disco solo si `body` termina en `.json/.txt/.html`; si falla, devuelve el literal (`main.rs:128`).

Arranque del servidor
- `main` inicializa logging (`info` por defecto), parsea CLI y carga mocks si `--mock-config` está presente, registrando errores de E/S/TOML (`main.rs:155`, `main.rs:165`).
- Resuelve `--api-url` para obtener `host`/`port` y compone el `SocketAddr` de escucha (`main.rs:155`).
- Construye el filtro `route` capturando: método, cabeceras, path, query raw, cuerpo como bytes, config, mocks y cliente, y delega a `proxy_handler` (`main.rs:221`).

Manejador principal: proxy_handler
- Firma y parámetros: método, cabeceras, path completo, query, body, config, mocks y cliente HTTP (`main.rs:231`).
- Logging: arma una URL completa solo para log (path + `?query` si aplica) y marca el método y ruta.

Resolución de mocks
- Si `mocks` existe, busca la primera coincidencia por `method` y `path` ignorando mayúsculas/minúsculas. Importante: la comparación usa solo `full_path.as_str()` (sin query) (`main.rs:231`).
  - Si coincide: construye respuesta con `status`, cabeceras del mock, aplica CORS si se pidió, agrega cabeceras extra de CLI y compone el cuerpo con `load_body_content`.
  - Si `--save-request-directory` está activo, persiste la respuesta mock.

Reenvío cuando no hay mock
- Concatena `--target-url` (sin barra final) + path y anexa la query si existe. Copia todas las cabeceras de entrada salvo `host` y reenvía con `reqwest` preservando el cuerpo.
- Si hay error de red o lectura de cuerpo, responde `502 Bad Gateway` con texto descriptivo.
- Registra estado/cabeceras/cuerpo: por defecto oculta cabeceras y cuerpo; `--show-headers` y `--show-body` habilitan su registro con pretty-print si es JSON.

Post-procesado de respuesta
- Aplica cabeceras extra (`--extra-header`) validando formato `Nombre: Valor`.
- Si `--add-cors-headers` está activo, agrega `Access-Control-Allow-*` y `Content-Type: application/json` si faltara.
- Reconstruye la respuesta final con estado, cabeceras acumuladas y cuerpo.

Persistencia de respuestas y generación de mocks
- `save_response_to_file` crea el directorio si no existe, genera un nombre seguro a partir de path+query y escribe un JSON embellecido con timestamp.
- Crea/actualiza `mocked-request.toml` en el directorio con entradas `[[mocks]]` que incluyen `method`, `path` (aquí incluye query), `status` y `body` apuntando al JSON generado (`main.rs:482`).

Detalles y matices importantes
- Coincidencia de mocks: la búsqueda usa solo el path sin query, mientras que el generador de TOML guarda `path` con query. Esto puede provocar que un mock guardado con query no coincida luego en ejecución. Opciones:
  - Guardar mocks sin query en `path`, o
  - Ajustar la lógica de matching para comparar contra `path + '?' + query`.
- Cabeceras extra: el formato debe ser exacto `Clave: Valor`; entradas inválidas se registran con `warn` y se omiten.
- Seguridad/privacidad: por defecto no se logean cabeceras/cuerpos; habilítalos explícitamente si lo necesitas (`--show-headers`, `--show-body`).

Extensiones posibles
- Soporte de alias para `--mock-config` (p. ej. `--mock-file`) para alinear CLI con documentación histórica.
- Coincidencia de mocks por patrón o incluyendo query.
- Control fino de CORS y filtrado de cabeceras a propagar.

