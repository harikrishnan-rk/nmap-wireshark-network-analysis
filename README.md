# 🔎 Nmap & Wireshark Network Analysis

![Nmap](https://img.shields.io/badge/Nmap-Network%20Scanning-2E8B57)
![Wireshark](https://img.shields.io/badge/Wireshark-Packet%20Analysis-1679A7)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-Lab-557C94)
![Windows 11](https://img.shields.io/badge/Windows%2011-Test%20Host-0078D4)
![VirtualBox](https://img.shields.io/badge/VirtualBox-Virtual%20Lab-183A61)

## 🎯 Project Overview

This project is a hands-on network analysis lab using **Nmap** and **Wireshark**.

The purpose is to understand how network discovery and scanning work, then compare the results of Nmap with the actual network traffic captured in Wireshark.

The project focuses on:

* Network discovery
* Port scanning
* Service detection
* Packet capture
* Traffic analysis
* Comparing tool output with packet-level evidence
* Technical documentation

The work was completed in an isolated virtual lab environment.

---

## 🖥️ Lab Environment

| Component                     | Details                  |
| ----------------------------- | ------------------------ |
| 🐧 Attacker / Analysis System | Kali Linux               |
| 🪟 Target System              | Windows 11               |
| 💻 Virtualization             | VirtualBox               |
| 🌐 Network                    | Isolated virtual network |
| 🔍 Scanning Tool              | Nmap                     |
| 📡 Packet Analysis            | Wireshark                |

### 🌐 Network Configuration

The lab uses a direct virtual network between Kali Linux and Windows 11.

The lab addressing is documented in the baseline evidence.

| System        | IP Address       |Role       |
| ------------- | ---------------- |---------------- |
| 🐧 Kali Linux | 192.168.100.10 |Analysis/ Scanning| 
| 🪟 Windows 11 | 192.168.100.20 |Target       |

The environment is intended only for controlled testing between the two lab systems.

---

## 🏗️ Baseline & Network Topology

Before running the experiments, the network configuration and connectivity between Kali Linux and Windows 11 were verified.

Baseline evidence includes:

1. VirtualBox network configuration
2. Kali Linux network configuration and connectivity
3. Windows 11 network configuration and connectivity
4. Windows 11 listening ports
5. Final lab network topology

📁 [**Baseline evidence**](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/baseline-and-topology)

### 🗺️ Lab Topology

![Hari Lab Network Topology](baseline-and-topology/05-hari-lab-topology.png)

---

# 🔬 Experiments

The project is divided into three progressive experiments.

## 01 — Basic Host Discovery

The first experiment focuses on identifying an active host on the lab network.

**Objective:**

* Discover the target system
* Confirm network reachability
* Observe the traffic generated during discovery

📁 **Experiment:**
[`experiments/01-basic-host-discovery/`](experiments/01-basic-host-discovery/)

---

## 02 — Port Scanning

The second experiment examines the ports exposed by the Windows 11 target.

**Objective:**

* Identify open ports
* Understand the purpose of port scanning
* Capture and examine the related network traffic

📁 **Experiment:**
[`experiments/02-port-scanning/`](experiments/02-port-scanning/)

---

## 03 — Service Detection

The third experiment builds on port scanning by examining the services associated with discovered ports.

**Objective:**

* Identify services running on discovered ports
* Compare Nmap results with captured traffic
* Understand how service detection generates network activity

📁 **Experiment:**
[`experiments/03-service-detection/`](experiments/03-service-detection/)

---

# 📡 Nmap + Wireshark Analysis

A key part of this project is comparing two different views of the same network activity:

```text
Nmap
  ↓
Scan / Discovery
  ↓
Network Traffic
  ↓
Wireshark Capture
  ↓
Packet Analysis
  ↓
Compare Results
```

**Nmap** provides the scan results.

**Wireshark** provides packet-level visibility into the traffic generated during the scan.

Using both tools provides a better understanding of what happens on the network instead of relying only on the final Nmap output.

---

# 🤝 Collaboration

The project was completed using **individual lab environments**.

The three participants are:

* 👨‍💻 [Hari Krishnan R K](https://github.com/harikrishnan-rk)
* 👨‍💻 [Manu P Nair](https://github.com/manunair16)
* 👨‍💻 [Varun M Nair](https://github.com/varunmnair95)

Each participant maintains a separate repository and lab setup.

Collaboration may include:

* Sharing approaches
* Discussing observations
* Comparing results
* Reviewing documentation

The implementation and evidence in this repository represent **my own lab work**.

---

# 📸 Evidence

The repository contains screenshots and supporting evidence for the lab setup and experiments.

Evidence is used to show:

* Network configuration
* Connectivity
* Target listening ports
* Nmap activity
* Wireshark packet captures
* Experiment observations

The intention is to document the actual work performed rather than only describe the theory.

---

# 📂 Repository Structure

```text
nmap-wireshark-network-analysis/
│
├── README.md
│
├── baseline-and-topology/
│   ├── 01-virtualbox-network-configuration-for-kali-and-win11.png
│   ├── 02-kali-network-configuration-and-connectivity.png
│   ├── 03-win11-network-configuration-and-connectivity.png
│   ├── 04-listening-ports-win11.png
│   └── 05-hari-lab-topology.png
│
├── experiments/
│   ├── 01-basic-host-discovery/
│   ├── 02-port-scanning/
│   └── 03-service-detection/
│
└── collaboration/
    └── .gitkeep
```

---

# 🎓 Skills Demonstrated

| Area                 | Skills                                              |
| -------------------- | --------------------------------------------------- |
| 🌐 Networking        | IP addressing, connectivity, ports, network traffic |
| 🔎 Network Discovery | Host discovery and port scanning                    |
| 🛠️ Nmap             | Network scanning and service detection              |
| 📡 Wireshark         | Packet capture and traffic analysis                 |
| 🧪 Lab Work          | Virtual networking and controlled testing           |
| 📝 Documentation     | Evidence collection and technical reporting         |
| 🤝 Collaboration     | Comparing approaches and findings                   |

---

# 📌 Project Status

**Project 1 — Nmap & Wireshark Network Analysis**

The lab covers:

**Baseline → Host Discovery → Port Scanning → Service Detection → Packet Analysis**

The project is maintained as a practical learning and portfolio exercise.

---

## ⚠️ Disclaimer

This project was performed in an isolated virtual lab using systems controlled by the participants.

Nmap scanning and packet analysis should only be performed against systems where you have permission to test.

This project is intended for **learning, practice, and portfolio development**.

