# 🛰️ Sentinel Trace (v1.0 — Genesis)

> **"Signals are cheap. Decisions are earned."** > *Behavioral Detection Lab powered by eBPF & Tetragon.*

---

## 🌐 Overview
**Sentinel Trace** is a Blue Team-focused security lab dedicated to **behavioral detection** using low-level system signals. 

While most security tools focus on signatures (what a file *looks* like), Sentinel Trace focuses on **contextual reasoning** (what a process *does*). It observes, contextualizes, and explains suspicious behaviors at the kernel level without modifying applications.

## 🧠 Detection Philosophy: "Reasoning-First"
We move away from noisy, signal-only alerts to focus on **explainable security**:
- ✅ **Context over Isolation:** A shell is not a threat; a shell spawned by `nginx` is.
- ✅ **Kernel-Level Truth:** Using eBPF for tamper-proof telemetry.
- ✅ **Zero Instrumentation:** Security visibility with zero impact on application code.
- ✅ **Evidence-Based:** Every alert must be backed by a clear parent/child process lineage.

---

## 🏗️ Architecture & Integration
Sentinel Trace acts as the **Thinking Brain**, designed to feed high-confidence decisions into the **Sovereign Shield** (the Enforcement Plane).

```mermaid
graph TD
    A[Activity Source: Web/User] --> B[Linux Kernel]
    B -->|eBPF Hooks| C[Sentinel Trace]
    C -->|Context Enrichment| D{Reasoning Engine}
    D -->|Suspicious| E[Decision: ALERT]
    D -->|Normal| F[Decision: IGNORE]
    E -.-> G[Sovereign Shield: ENFORCE]
```

### 🧪 MVP (Minimum Viable Product) Scenario: Suspicious Shell Spawning (***The Question: Why is a public-facing HTTP service spawning a shell?***)

🔎 Observed Signals
- Event: ``process_exec``
- Parent: ``/usr/sbin/nginx``
- Child: ``/bin/sh``
- Context: UID: 33 (www-data) | Namespace: Runtime

🧠 Sentinel Decision
Status: ❌ SUSPICIOUS Reasoning: An HTTP service (Nginx) has no operational justification to spawn an interactive shell. This pattern is synonymous with Remote Code Execution (RCE) or Web Shell exploitation.

🚀 Quick Start (Standalone Mode)
1. Verify Kernel Support
```
sudo bpftool feature probe kernel | grep "eBPF"
```

3. Install Tetragon Daemon
```
curl -fL -o tetragon.tar.gz [https://github.com/cilium/tetragon/releases/download/v1.6.0/tetragon-v1.6.0-amd64.tar.gz](https://github.com/cilium/tetragon/releases/download/v1.6.0/tetragon-v1.6.0-amd64.tar.gz)
tar -xzf tetragon.tar.gz && cd tetragon-v1.6.0-amd64
sudo ./install.sh
sudo systemctl enable --now tetragon
```

5. Observe the Stream

```
sudo tetra getevents
```

🧭 Roadmap
- [x] v1.0 Genesis: Single-signal detection & Manual reasoning.
- [ ] v2.0 Correlation: Multi-signal (Process + Network) correlation.
- [ ] v2.5 MITRE Mapping: Direct mapping to ATT&CK techniques.
- [ ] v3.0 Sovereign Bridge: Automated enforcement via Sovereign Shield.


### ⚙️ System Validation
#### ⚙️ Engine & Service Configuration
![Detection Log](assets/img/v1/system_status.png)

#### 🛡️ TracingPolicy Injection & Kernel Validation
![Detection Log](assets/img/v1/policy_load.png)

#### 🛡️ TracingPolicy Deployment
![Detection Log](assets/img/v1/attack_sim.png)

#### 🚨 High-Fidelity Detection Log (The Proof)
![Detection Log](assets/img/v1/detection_log.png)


## Hou to check logs
```
# To follow real-time alerts for the Nginx user:
sudo tetra getevents --output json | jq -ce 'select(.process_exec.process.uid == 33)'
```

## ⚙️ System Integration & Persistence

To ensure continuous runtime monitoring, **Sentinel Trace** is deployed as a `systemd` service. This configuration guarantees that the eBPF hooks are re-attached automatically upon system reboot and provides resilience against service termination.

### Scalable Security Model
The service is configured to automatically load all policies located in `/etc/tetragon/tracingpolicies/`. This creates an **evolvable security system**: to update the protection or add new detection rules, you simply drop a new `.yaml` file into the directory. The kernel hooks are updated without any service interruption or application downtime.

### 🚀 Quick Start (Automated Install)

Clone the repository and run the install script to deploy the engine and policies:
```
git clone [https://github.com/OBadolo/sentinel-trace.git](https://github.com/OBadolo/sentinel-trace.git)
cd sentinel-trace
chmod +x install.sh
sudo ./install.sh
```


This project is part of the Sovereign Shield Ecosystem.


# 🛰️ Sentinel Trace v2.0 — Correlation: Multi-signal (Work in Progress)
**Status: In Development (feature/v2-correlation branch)**

### Objectives
- Link process execution to network activity.
- Detect "Inbound Shell" vs "Outbound Connection" (Reverse Shell pattern).
- Isolate suspicious web traffic from `www-data`.

### New Policies
- `network-observability.yaml`: Monitors `__sys_connect` to capture destination IPs.

### Proof of Concept (v2)
```json
// here goes the JSON obtained with curl test
```


👤 Author
O’djuma Badolo Cybersecurity • Cloud • DevSecOps
