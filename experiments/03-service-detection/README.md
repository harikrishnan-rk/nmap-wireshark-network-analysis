# 🔎 Experiment 3 — Nmap Service & Version Detection

## 📌 Objective

The objective of this experiment was to use **Nmap service and version detection (`-sV`)** to identify services running on open TCP ports and observe how Windows Firewall configuration affects the detection process using **Wireshark**.

This experiment builds on **Experiment 2 — Port Scanning**.

---

## 🖥️ Lab Environment

| Component          | Details                     |
| ------------------ | --------------------------- |
| 🐉 Kali Linux      | `192.168.100.10`            |
| 🪟 Windows 11      | `192.168.100.20`            |
| 🌐 Network         | VirtualBox Internal Network |
| 🛡️ Firewall       | Windows Defender Firewall   |
| 🔍 Tool            | Nmap 7.99                   |
| 📡 Packet Analysis | Wireshark                   |

---

## 🎯 Ports Investigated

Experiment 2 identified:

```text
135/tcp
139/tcp
445/tcp
```

These ports were investigated using Nmap service detection.

---

## 🧪 Methodology

### 1️⃣ Firewall ON

The initial service-detection scan was performed with Windows Defender Firewall enabled:

```bash
nmap -sV -p 135,139,445 192.168.100.20
```

Nmap reported the ports as:

```text
135/tcp filtered msrpc
139/tcp filtered netbios-ssn
445/tcp filtered microsoft-ds
```

Wireshark showed TCP SYN probes from Kali without usable responses from the Windows target.

### 2️⃣ Firewall OFF

As a controlled comparison, Windows Defender Firewall was temporarily disabled.

The same scan was then performed:

```bash
nmap -sV -p 135,139,445 192.168.100.20
```

Nmap reported:

```text
135/tcp open msrpc        Microsoft Windows RPC
139/tcp open netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open microsoft-ds?
```

Nmap also identified:

```text
OS: Windows
```

The firewall was restored to its original enabled state after the test.

---

## 📡 Wireshark Analysis

### Firewall ON

The capture showed:

```text
192.168.100.10 → 192.168.100.20
```

with TCP SYN probes directed at ports:

```text
135
139
445
```

No usable response was observed for the probes, corresponding with Nmap's `filtered` results.

### Firewall OFF

With the firewall disabled, the capture showed successful TCP communication, including:

```text
SYN
SYN/ACK
ACK
```

followed by additional service-detection traffic.

This allowed Nmap to obtain responses and identify the services.

---

## 🔍 Findings

The experiment demonstrated that:

1. Nmap `-sV` performs additional probes to identify services.
2. With the firewall enabled, the investigated ports were filtered.
3. With the firewall disabled, the ports became reachable.
4. Port `135` was identified as **Microsoft Windows RPC**.
5. Port `139` was identified as **Microsoft Windows netbios-ssn**.
6. Port `445` was identified as **microsoft-ds**, with Nmap showing lower confidence using `?`.
7. Wireshark provided packet-level evidence supporting the difference between the two firewall states.

The controlled firewall comparison demonstrates that filtering can prevent Nmap from obtaining the responses required for service detection.

---

## 📁 [Evidence](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/experiments/03-service-detection/evidence)

### Firewall ON

```text
03-exp3-wireshark.pcapng
```

Supporting screenshot:

```text
01-nmap-scan.png
02-wireshark-capture.png
```

### Firewall OFF

```text
05-exp3-nmap-scan-firewall-off.pcapng
```

Supporting screenshot:

```text
04-nmap-scan-firewall-off.png
```

---

## ✅ Conclusion

Nmap `-sV` was used to identify services associated with the Windows target's TCP ports.

The firewall comparison demonstrated that service detection depends on receiving useful responses from the target. With the firewall enabled, the ports were filtered; after temporarily disabling the firewall, Nmap received responses and successfully identified the services.

The experiment therefore connected **Nmap service detection, Windows Firewall behavior, and packet-level evidence in Wireshark**.

---

## ⚠️ Lab Disclaimer

This experiment was performed in an isolated virtual lab environment for learning and portfolio development.

No production systems were involved.
