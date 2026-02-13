![Version](https://img.shields.io/badge/version-2.0--correlation-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Success_PoC-success?style=for-the-badge)

# 🛰️ Sentinel Trace

> **"Signals are cheap. Decisions are earned."**
> *Behavioral Detection Lab powered by eBPF & Tetragon.*

---

## 🌐 Overview
**Sentinel Trace** is a Blue Team-focused security lab dedicated to **behavioral detection** using low-level system signals. 

While most security tools focus on signatures, Sentinel Trace focuses on **contextual reasoning**. It observes, contextualizes, and explains suspicious behaviors at the kernel level.

### 🧭 Roadmap
- [x] **v1.0 Genesis**: Single-signal detection (Process Monitoring).
- [x] **v2.0 Correlation**: Multi-signal (Process + Network) correlation.
- [ ] **v2.5 MITRE Mapping**: Direct mapping to ATT&CK techniques (Work in Progress).
- [ ] **v3.0 Sovereign Bridge**: Automated enforcement (Work in Progress).

---

## 🧠 Detection Philosophy: "Reasoning-First"
- ✅ **Context over Isolation:** A shell is not a threat; a shell spawned by `nginx` is.
- ✅ **Kernel-Level Truth:** Using eBPF for tamper-proof telemetry.
- ✅ **Evidence-Based:** Every alert is backed by a clear parent/child process lineage.

---

## 🏗️ Architecture & Persistence
Sentinel Trace applies **Infrastructure as Code (IaC)** principles for resilience:

1. **Deployment Engine (`install.sh`)**: Standardizes environment and injects policies.
2. **Persistence Layer (`tetragon.service`)**: Ensures boot-time protection and auto-restart.

```text
┌──────────────────────────┐
│ install.sh               │  → Deployment Engine (IaC)
├──────────────────────────┤
│ tetragon.service         │  → Persistence Layer (Systemd)
└──────────────────────────┘