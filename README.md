# Zlija (زليجة) 🧱

[![Rust](https://img.shields.io/badge/rust-stable-brightgreen.svg)](https://www.rust-lang.org)
[![License: MIT OR Apache-2.0](https://img.shields.io/badge/License-MIT%20OR%20Apache--2.0-blue.svg)](LICENSE-MIT)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)]()

**Zlija** is a fast, lightweight, zero-dependency embedded NoSQL Key-Value store written in pure Rust. Optimized specifically for resource-constrained environments such as IoT devices and Edge Computing, Zlija delivers high throughput and crash resilience through a Bitcask-inspired append-only log structure.

---

## 💡 Why Zlija?

In embedded systems and edge environments, traditional databases often introduce heavy runtime overhead, complex SQL parsers, or risky external C library dependencies. **Zlija** solves this by providing:

* **Zero-Configuration Simplicity:** Plug-and-play local storage engine without complex server management.
* **Deterministic Write Performance:** Append-only log design ensures low latency writes with minimal write amplification.
* **Memory Safety & Portability:** Written in 100% pure, safe Rust—no `unsafe` code blocks, no C/C++ bindings.
* **Compact Binary Footprint:** Optimized record header (20 bytes fixed overhead) to preserve limited storage space.

---

## ✨ Features

* 🦀 **Pure Rust Implementation:** No C-dependencies or external bindings required.
* ⚡ **Bitcask Architecture:** $O(1)$ write operations and high-throughput point queries via in-memory indexing (KeyDir).
* 🛡️ **Data Integrity:** Built-in CRC32 checksums for every record to detect disk corruption or unexpected power outages.
* 📦 **Minimal Footprint:** Designed specifically for low-RAM and low-storage IoT and Edge environments.
* 📜 **Dual Licensed:** Open-source under MIT or Apache-2.0 at your option.

---

## 📐 Binary Storage Format

Every key-value entry is written sequentially to disk in an immutable append-only binary log format (`.zlij` file):

```text
+----------------+----------------+----------------+-----------------+----------+------------+
| Checksum (u32) | Timestamp(u64) | Key Size (u32) | Value Size(u32) |   Key    |   Value    |
|    4 Bytes     |    8 Bytes     |    4 Bytes     |     4 Bytes     | Variable |  Variable  |
+----------------+----------------+----------------+-----------------+----------+------------+
|<-------------------------- Header (20 Bytes) ------------------------>|<---- Payload ----->|