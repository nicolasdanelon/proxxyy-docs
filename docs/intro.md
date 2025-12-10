---
sidebar_position: 1
---

# Instalar y compilar proxxyy

Esta guía explica cómo descargar, compilar e instalar el proxy escrito en Rust.

Requisitos
- Rust y Cargo (instalador recomendado: `rustup`)
  - macOS/Linux: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
  - Windows: instala Rust desde https://www.rust-lang.org/tools/install
  - Verifica: `rustc --version` y `cargo --version`

Descarga
- Clona el repositorio:
  - ```bash
    git clone https://github.com/nicolasdanelon/proxxyy.git
    cd proxxyy
    ```

Compilación
- Modo debug (rápido para desarrollo):
  - ```bash
    cargo build
    # binario en: target/debug/proxxyy
    ```
- Modo release (optimizado):
  - ```bash
    cargo build --release
    # binario en: target/release/proxxyy
    ```
- Instalar en tu PATH (útil para usar `proxxyy` desde cualquier directorio):
  - ```bash
    cargo install --path=.
    # para actualizar en el futuro: cargo install --path=. --force
    ```

Probar ejecución mínima
- Con parámetros mínimos (ajusta URLs según tu caso):
  - ```bash
    proxxyy -t 'https://api.example.com' -u 'http://localhost:6969'
    ```
- También puedes ejecutar sin instalar:
  - ```bash
    cargo run -- -t 'https://api.example.com' -u 'http://localhost:6969'
    ```

Notas
- En macOS/Linux, asegúrate de que `~/.cargo/bin` esté en tu `PATH` para usar el binario instalado.
- Si necesitas ejemplos de uso y parámetros disponibles, consulta la “Guía de Usuario” en el menú lateral.
