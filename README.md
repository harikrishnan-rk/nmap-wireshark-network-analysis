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

## [01 — Basic Host Discovery](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/experiments/01-basic-host-discovery)

The first experiment focuses on identifying an active host on the lab network.

---

## [02 — Port Scanning](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/experiments/02-port-scanning)

The second experiment examines TCP ports exposed by the Windows 11 target.

---

## [03 — Service & Version Detection](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/experiments/03-service-detection)

The third experiment builds on port scanning by examining the services associated with the discovered ports.

---

# 📡 Nmap + Wireshark Analysis

A key part of this project is comparing two views of the same network activity:

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

Nmap provides the scan results.

Wireshark provides packet-level visibility into the traffic generated during the scan.

The firewall comparison also demonstrated how filtering can change the packet responses visible to the scanner and therefore change Nmap's interpretation of port state and service availability.

---

# 🤝 Collaboration

The project was completed using individual lab environments.

The three participants are:

* 👨‍💻 [Hari Krishnan R K](https://github.com/harikrishnan-rk/)
* 👨‍💻 [Manu P Nair](https://github.com/manunair16/)
* 👨‍💻 [Varun M Nair](https://github.com/varunmnair95/)

Each participant maintains a separate repository and lab setup.

Collaboration included:

* Sharing approaches
* Discussing observations
* Comparing results
* Reviewing documentation

The implementation and evidence in this repository represent **my own lab work**.

### Participant Repositories

* [Hari Krishnan R K — Nmap & Wireshark Network Analysis](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis)
* [Manu P Nair — Nmap & Wireshark Network Analysis](https://github.com/manunair16/nmap-wireshark-network-analysis)
* [Varun M Nair — Nmap & Wireshark Network Analysis](https://github.com/varunmnair95/nmap-wireshark-network-analysis)

---

# 📸 Evidence

Evidence includes:

* Network configuration
* Connectivity verification
* Target listening ports
* Nmap scan results
* Firewall status and controlled comparison
* Wireshark packet captures
* `.pcapng` files
* Experiment observations

The packet captures are retained as supporting evidence so the traffic can be reviewed independently of the screenshots.

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
└── collaboration-comparison-and-network-analysis/
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

**Nmap & Wireshark Network Analysis — Completed**

The lab covers:

**Baseline → Host Discovery → Port Scanning → Service Detection → Packet Analysis**

The project is maintained as a practical learning and portfolio exercise.

---

## ⚠️ Disclaimer

This project was performed in an isolated virtual lab using systems controlled by the participants.

Nmap scanning and packet analysis should only be performed against systems where you have permission to test.

This project is intended for learning, practice, and portfolio development.


