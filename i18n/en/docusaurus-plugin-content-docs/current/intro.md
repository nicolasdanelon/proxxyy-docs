---
sidebar_position: 1
---

# Install and build proxxyy

This guide shows how to download, build, and install the Rust proxy.

Requirements
- Rust and Cargo (recommended installer: `rustup`)
  - macOS/Linux: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
  - Windows: install Rust from https://www.rust-lang.org/tools/install
  - Verify: `rustc --version` and `cargo --version`

Download
- Clone the repository:
  - ```bash
    git clone https://github.com/nicolasdanelon/proxxyy.git
    cd proxxyy
    ```

Build
- Debug mode (fast for dev):
  - ```bash
    cargo build
    # binary at: target/debug/proxxyy
    ```
- Release mode (optimized):
  - ```bash
    cargo build --release
    # binary at: target/release/proxxyy
    ```
- Install to your PATH:
  - ```bash
    cargo install --path=.
    # to update later: cargo install --path=. --force
    ```

Quick run
- Minimal (adjust URLs to your case):
  - ```bash
    proxxyy -t 'https://api.example.com' -u 'http://localhost:6969'
    ```
- Run without installing:
  - ```bash
    cargo run -- -t 'https://api.example.com' -u 'http://localhost:6969'
    ```

Notes
- On macOS/Linux, ensure `~/.cargo/bin` is in your `PATH`.
- For usage examples and all flags, see the “User Guide”.

