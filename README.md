# Mastodon Random Cafe ☕️🌍

[![Build & Test](https://github.com/seungjin/mstd-random-cafe/actions/workflows/build.yml/badge.svg)](https://github.com/seungjin/mstd-random-cafe/actions/workflows/build.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

A sophisticated Mastodon bot that discovers and shares charming cafes from around the world. It picks a random global city, finds a highly-rated cafe nearby via Google Places, generates accessible AI descriptions for images using Google Gemini, and posts a beautifully formatted status to Mastodon.

Built as a modern **WASI P2 (WebAssembly System Interface Preview 2)** component, it demonstrates the power of sandboxed, cross-platform WebAssembly for cloud-native automation.

## 🚀 Quick Start

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/seungjin/mstd-random-cafe.git
    cd mstd-random-cafe
    ```

2.  **Configure your environment:**
    ```bash
    cp sample.env .env
    # Edit .env with your API keys and Mastodon details
    ```

3.  **Run the bot:**
    ```bash
    just run
    ```

## 🛠 Features

-   **Global Reach:** Selects from 10,000+ cities in `src/geopoints.csv`, with configurable weighting for specific countries.
-   **Smart Filtering:** Only selects cafes with a rating of 3.0+ and at least 100 reviews.
-   **AI-Powered Accessibility:** Automatically generates alt-text for cafe photos using the `gemini-1.5-flash` model.
-   **WASI P2 Architecture:** Fully compliant with the Component Model, utilizing `wasi-http` for all network requests.
-   **Multi-Platform Deployment:** Runs anywhere Wasmtime is available, or as a tiny OCI container.

## ⚙️ Configuration

The bot uses environment variables for configuration. Use the `sample.env` file as a template.

| Variable | Required | Description |
| :--- | :--- | :--- |
| `MSTDN_URI` | **Yes** | The domain of your Mastodon instance (e.g., `mastodon.social`). |
| `MSTDN_ACCESS_TOKEN` | **Yes** | Access token for your Mastodon bot account. |
| `GOOGLE_API_KEY` | **Yes** | Google Cloud API key with Places and Gemini API access. |
| `GEMINI_API_KEY` | No | Separate key for Gemini if different from `GOOGLE_API_KEY`. |
| `GEMINI_API_KEY_API_URI` | No | Custom API endpoint for Gemini. |
| `WEIGHTED_COUNTRIES` | No | Comma-separated ISO2 codes to weight (Default: `DE,GB,FR,ES,IT,TW,TH,VN,MX,PT,KR`). |
| `EXCLUDED_PLACE_TYPE` | No | Comma-separated Google Places types to exclude from selection (Default: `hotel,lodge,gas_station,convenience_store,restaurant,bar`). |

## 💻 Development

The project uses `just` as a task runner:

-   `just build`: Build the WASM component (`debug`).
-   `just build-release`: Build the WASM component (`release`).
-   `just run`: Build and run the component using `wasmtime`.
-   `just clean`: Remove build artifacts.

For linting and testing:
```bash
cargo clippy
cargo fmt
cargo test
```

### Prerequisites
-   **Rust**: Latest stable version.
-   **cargo-component**: `cargo install cargo-component`
-   **Wasmtime**: `brew install wasmtime` (or similar)
-   **Just**: `brew install just` (or similar)

## 📦 Deployment

### Container (OCI)
Build a minimal, secure container image:
```bash
docker build -t mstd-random-cafe .
```

### Systemd
Automate the bot on a schedule (e.g., hourly) using the provided units:
1.  Customize `mstd-random-cafe.service` with your project path.
2.  `sudo cp mstd-random-cafe.* /etc/systemd/system/`
3.  `sudo systemctl enable --now mstd-random-cafe.timer`

## 📄 License

This project is dual-licensed under the **MIT License** and the **Apache License (Version 2.0)**.

Copyright (c) 2026 Seungjin Kim
