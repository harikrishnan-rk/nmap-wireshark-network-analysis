# 🤝 Collaboration, Comparison & Network Analysis

## 🎯 Purpose

This document records **my individual network analysis and collaborative comparison** for the Nmap & Wireshark project.

The three participants worked independently in their own virtual environments while following the same overall project progression:

```text
Baseline
   ↓
Host Discovery
   ↓
Port Scanning
   ↓
Service Detection
   ↓
Wireshark Analysis
   ↓
Collaborative Comparison
```

The purpose of this analysis is to understand the network traffic behind Nmap results and compare observations between independent environments.

---

# 🖥️ My Lab Environment

| Component              | Details                  |
| ---------------------- | ------------------------ |
| 🐧 Analysis / Scanning | Kali Linux               |
| 🪟 Target              | Windows 11               |
| 💻 Virtualization      | VirtualBox               |
| 🌐 Network             | Isolated virtual network |
| 🔍 Scanner             | Nmap                     |
| 📡 Packet Analysis     | Wireshark                |
| Kali IP                | `192.168.100.10`         |
| Windows IP             | `192.168.100.20`         |

The baseline directory contains the network configuration, connectivity verification, listening-port information, and topology used for my experiments.

📁 [Baseline & Topology](https://github.com/harikrishnan-rk/nmap-wireshark-network-analysis/tree/main/baseline-and-topology)

---

# 🔬 Analysis Methodology

My approach was to correlate the Nmap result with the actual packets.

```text
Nmap Command
      ↓
Probe Generated
      ↓
Target Response
      ↓
Wireshark Capture
      ↓
Packet Interpretation
      ↓
Nmap Result
```

The main questions were:

* What protocol was used?
* What packet did Nmap send?
* What response came back?
* What TCP flags were present?
* Did the response explain the reported state?
* Could filtering or firewall behaviour explain the result?

---

# 01 — Host Discovery

Host discovery is the first step in determining whether the target is active.

One of the important observations when analysing Nmap on a local network is that **host discovery does not necessarily mean ICMP Echo**.

## 🌐 ARP vs ICMP

For a local IPv4 target, Nmap can use ARP:

```text
Kali → ARP Request → Windows
Kali ← ARP Reply  ← Windows
```

ARP is used to resolve an IPv4 address to a MAC address on the local Ethernet network.

ICMP Echo is different:

```text
Kali → ICMP Echo Request → Windows
Kali ← ICMP Echo Reply  ← Windows
```

ARP operates at Layer 2, while ICMP operates at Layer 3.

Nmap's documentation explicitly describes ARP as the normal discovery mechanism for IPv4 targets on local Ethernet networks because it is generally faster and more effective.

### Why this matters

If Wireshark shows ARP rather than ICMP during local host discovery, that is not evidence of an incorrect scan.

It can simply mean:

> Nmap is using the local-network discovery mechanism appropriate to the target.

---

# 🔎 Host Discovery Options

Several discovery techniques are available for comparison:

| Option               | Purpose               |
| -------------------- | --------------------- |
| `-sn`                | Host discovery only   |
| `-Pn`                | Skip host discovery   |
| `-PE`                | ICMP Echo             |
| `-PP`                | ICMP Timestamp        |
| `-PS`                | TCP SYN discovery     |
| `-PA`                | TCP ACK discovery     |
| `-PU`                | UDP discovery         |
| `--disable-arp-ping` | Disable ARP discovery |

For example:

```bash
nmap -sn 192.168.100.20
```

can be compared with:

```bash
nmap -sn -PE 192.168.100.20
```

and:

```bash
nmap -sn -PS80,443 192.168.100.20
```

These options should be treated as diagnostic/comparison techniques unless the specific command appears in the experiment evidence.

---

# 🧠 Why `-Pn` Can Change the Investigation

A host may be alive but fail to answer a particular discovery probe.

For example:

```text
Host is alive
      +
ICMP blocked
      ↓
ICMP discovery fails
```

Using:

```bash
nmap -Pn 192.168.100.20
```

tells Nmap to skip host discovery and treat the target as online.

This is useful when testing whether a discovery failure is being confused with a host being offline.

---

# 02 — Port Scanning

Port scanning determines which ports appear open, closed, or filtered.

The TCP SYN scan provides a straightforward packet-level model.

## 🔹 SYN Scan

```text
Kali → Windows : SYN
Windows → Kali : SYN/ACK
```

This commonly corresponds to an **open** TCP port.

For a closed port:

```text
Kali → Windows : SYN
Windows → Kali : RST
```

For filtering:

```text
Kali → Windows : SYN
Windows → Kali : no usable response
```

The last case should not be interpreted too aggressively.

A `filtered` result means Nmap cannot determine the port state because the expected evidence is unavailable. A firewall is a common reason, but the result does not prove the exact filtering mechanism.

---

# 🔹 SYN vs Connect

Two useful TCP scan types are:

```text
-sS → SYN scan
-sT → TCP Connect
```

### SYN scan

```text
SYN
 ↓
SYN/ACK
 ↓
No normal completed connection
```

### Connect scan

```text
SYN
 ↓
SYN/ACK
 ↓
ACK
 ↓
Connection established
```

The completed handshake is therefore visible in Wireshark when using a Connect scan.

This makes `-sS` and `-sT` useful for a direct packet-level comparison.

---

# 🔎 Port Selection

Useful port-selection options include:

| Option      | Purpose                 |
| ----------- | ----------------------- |
| `-p 80,443` | Selected ports          |
| `-p-`       | Full TCP port range     |
| `-F`        | Faster/common-port scan |

The reason for controlling the port range is analytical as well as practical.

A smaller, known port set makes it easier to correlate:

```text
Nmap Port
     ↕
Wireshark Destination Port
```

---

# 🔥 Firewall and Port States

Firewall state can change what Nmap sees.

A useful comparison is:

```text
Same target
     ↓
Same Nmap command
     ↓
Firewall configuration changed
     ↓
Repeat scan
     ↓
Compare captures
```

The packet capture can then be checked for:

* SYN packets
* SYN/ACK responses
* RST responses
* ICMP errors
* Missing responses
* Retransmissions

This is a better approach than assuming that a different Nmap result automatically means the service itself changed.

---

# 03 — Service Detection

After identifying accessible ports, service detection can provide additional information.

The main switch is:

```bash
nmap -sV 192.168.100.20
```

The important difference is that Nmap can send additional probes to identify the service and version.

Therefore:

```text
Port Scan
    ↓
Port identified
    ↓
-sV
    ↓
Additional probes
    ↓
Service response
```

Wireshark can show that service detection produces traffic beyond the initial port-state probe.

This is why service detection should be analysed as a separate experiment rather than treating it as simply another port scan.

---

# 📡 TCP State Analysis

A useful reference is:

| Result       | Typical packet evidence                        | Meaning                                                       |
| ------------ | ---------------------------------------------- | ------------------------------------------------------------- |
| `open`       | SYN/ACK                                        | A TCP service is accepting connections                        |
| `closed`     | RST                                            | Host is reachable but no service is accepting that connection |
| `filtered`   | Missing/blocked response or filtering evidence | Port state cannot be determined confidently                   |
| `unfiltered` | Response to ACK probe                          | Probe reached target; does not mean open                      |

The exact packet evidence should always be considered in the context of the scan technique.

---

# 🔐 ACK Scan

An ACK scan uses:

```bash
nmap -sA -p <PORT> 192.168.100.20
```

Its purpose is mainly to investigate **filtering**.

A common packet sequence is:

```text
Kali → TCP ACK → Windows
Kali ← TCP RST ← Windows
```

A response such as RST can support an `unfiltered` result.

However:

> **Unfiltered is not the same as open.**

The ACK scan does not reliably determine whether an application is listening on the port.

This is one of the most useful examples of why the scan type must be understood before interpreting the result.

---

# 🌐 UDP Analysis

UDP scanning is different because UDP has no TCP-style handshake.

Possible observations include:

### Open

```text
UDP Probe
   ↓
UDP Response
```

### Closed

```text
UDP Probe
   ↓
ICMP Port Unreachable
```

### Open|Filtered

```text
UDP Probe
   ↓
No Response
```

`open|filtered` is intentionally ambiguous.

It means Nmap could not determine whether the port is open but silent or filtered.

Therefore, an analyst should not translate:

```text
open|filtered
```

into:

```text
open
```

without additional evidence.

---

# 🧩 FIN, NULL & Xmas

These scans are useful for understanding TCP flag behaviour:

| Switch | Flags           |
| ------ | --------------- |
| `-sF`  | FIN             |
| `-sN`  | No TCP flags    |
| `-sX`  | FIN + PSH + URG |

The Wireshark investigation should verify the actual flags in the packet.

For example:

```text
Nmap -sF
     ↓
TCP packet
     ↓
FIN flag
     ↓
Target response
     ↓
Nmap result
```

The same approach can be used for NULL and Xmas scans.

These scans can be useful as a later experiment because their interpretation depends more heavily on target operating-system behaviour and filtering.

---

# 🦈 Wireshark Analysis

Useful filters for my lab include:

```text
ip.addr == 192.168.100.20
```

```text
arp
```

```text
icmp
```

```text
tcp
```

```text
tcp.port == 80
```

```text
tcp.flags.syn == 1
```

```text
tcp.flags.reset == 1
```

```text
udp
```

The analysis should not stop at the filter.

For an important packet, I should inspect:

* Source IP
* Destination IP
* Source port
* Destination port
* Protocol
* TCP flags
* Sequence numbers
* Acknowledgement numbers
* Response packet
* Timing

This converts a packet capture from a collection of packets into evidence.

---

# 📊 Cross-Lab Comparison

All three participants use the same basic methodology, but the environments are independent.

| Participant | Virtualization | Kali             | Windows          |
| ----------- | -------------- | ---------------- | ---------------- |
| Hari        | VirtualBox     | `192.168.100.10` | `192.168.100.20` |
| Manu        | VirtualBox     | `192.168.100.50` | `192.168.100.5`  |
| Varun       | UTM            | `192.168.128.3`  | `192.168.128.4`  |

The different subnets and virtualization platforms mean that packet counts, timestamps, and minor implementation details may differ.

The meaningful comparison is therefore:

### Methodology

Did all three perform the same type of experiment?

### Protocol behaviour

Did the expected ARP, ICMP, TCP, UDP, or service-level behaviour appear?

### Evidence

Does Wireshark support the Nmap result?

### Environment

Can a difference be explained by firewall state, services, network configuration, or virtualization?

---

# 👥 Other Participants

### 👨‍💻 Varun M Nair

Varun's implementation uses UTM and the `192.168.128.0/24` lab network.

📄 [Varun — Collaboration & Network Analysis](https://github.com/varunmnair95/nmap-wireshark-network-analysis)

### 👨‍💻 Manu P Nair

Manu's implementation uses VirtualBox and the `192.168.100.0/24` lab network.

📄 [Manu — Collaboration & Network Analysis](https://github.com/manunair16/nmap-wireshark-network-analysis)

Their documents contain their own detailed observations and evidence.

---

# 🧠 My Main Analytical Takeaways

The most important learning point is that Nmap gives a **conclusion based on network responses**, while Wireshark allows me to inspect those responses directly.

The useful mental model is:

```text
WHAT DID NMAP SEND?
        ↓
WHAT DID THE TARGET RETURN?
        ↓
WHAT DOES WIRESHARK SHOW?
        ↓
WHAT DID NMAP CONCLUDE?
        ↓
DOES THE EVIDENCE SUPPORT IT?
```

This prevents common mistakes such as:

* Treating ICMP as the only host-discovery method
* Assuming ARP and ICMP perform the same function
* Treating `filtered` as proof of a specific firewall
* Treating `unfiltered` as `open`
* Treating UDP `open|filtered` as `open`
* Assuming SYN and Connect scans generate identical traffic

---

# 🎓 Skills Demonstrated

* 🌐 Network discovery
* 🔎 Nmap scanning
* 🚪 TCP/UDP analysis
* 🛠️ Service detection
* 📡 Wireshark packet analysis
* 🧩 TCP flag interpretation
* 🔥 Firewall/filter investigation
* 📊 Cross-environment comparison
* 🧪 Controlled laboratory testing
* 📝 Evidence-based technical documentation

---

# 📌 Conclusion

My work demonstrates an end-to-end network-analysis process:

**Configure → Scan → Capture → Inspect → Correlate → Compare → Explain**

The collaboration adds value because the same methodology was applied independently in three environments.

The goal was not to produce identical packet captures.

The goal was to determine whether the **network behaviour, Nmap interpretation, and Wireshark evidence were logically consistent**.

---

## ⚠️ Disclaimer

All testing was performed in isolated laboratory environments controlled by the participants.

Nmap scanning should only be performed against systems where permission to test has been obtained.
