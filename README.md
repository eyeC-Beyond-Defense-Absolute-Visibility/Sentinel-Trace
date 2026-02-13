---

## 🛰️ Sentinel Trace v2.0 — Correlation & Context (Work in Progress)

**The Goal:** Moving from isolated signals to **behavioral chains**. 
Instead of just seeing a process, we link it to network activity and MITRE ATT&CK techniques.

### 🔭 New Capabilities
- **Network Observability:** Monitoring TCP/UDP connections in real-time via Tetragon.
- **Signal Correlation:** Linking a `process_exec` (Shell) to a `socket_connect` (Reverse Shell).
- **Multi-Node Lab:**
  - 🛡️ **Debian 12** (Target): Protected by Sentinel Trace.
  - 🛡️ **Metasploitable3** (Vulnerable Target): Expanded monitoring.
  - 💀 **Kali Linux** (Attacker): Simulation of RCE and Privilege Escalation.

### 🛡️ Planned Scenario: The Reverse Shell Chain
Sentinel Trace v2.0 will detect the following sequence as a single high-priority alert:
1. `Nginx` (UID 33) spawns `/bin/sh` (**Process Signal**)
2. `/bin/sh` initiates an outbound connection to a non-standard port 4444 (**Network Signal**)
3. **Reasoning:** Unauthorized Outbound Shell (MITRE T1059.004).

### 🛠️ Infrastructure Evolution
- Integration of **Grafana Loki** or **ELK** to centralize and visualize eBPF signals.
- First automated enforcement rules (**Sigkill** on confirmed C2 connections).
