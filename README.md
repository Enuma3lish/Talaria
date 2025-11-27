# Talaria

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)]()
[![C++ Standard](https://img.shields.io/badge/C%2B%2B-17-blue.svg)](https://en.wikipedia.org/wiki/C%2B%2B17)
[![Python](https://img.shields.io/badge/python-3.12-yellow.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

**Talaria** is a hybrid high-frequency trading (HFT) execution gateway designed for reliability and speed. It leverages the raw performance of **C++17** for market data streaming and order management, while exposing a flexible **Python 3.12** interface for strategy integration.

Named after the winged sandals of Hermes, Talaria minimizes the "tick-to-trade" latency by decoupling price ingestion from strategy logic.

## 🚀 Architecture

Talaria operates on a split-architecture model:

* **The Engine (C++17):** Handles WebSocket streams, maintains a local order book in shared memory, and manages thread-safe order execution queues. Built with MSVC and optimized for Windows environments.
* **The Bridge (pybind11):** Provides zero-copy bindings, allowing Python to access C++ memory structures instantly without serialization overhead.
* **The Pilot (Python 3.12):** An async implementation that consumes external signals (TradingView, Telegram, ML Models) and triggers execution via the C++ engine.

```mermaid
graph LR
    A[Ext. Signal] -->|Python Async| B(Talaria Python Wrapper)
    B <-->|pybind11 / Zero Copy| C{Talaria C++ Engine}
    C <-->|WebSocket| D[Exchange Market Data]
    C -->|REST/WS| E[Exchange Order Entry]
