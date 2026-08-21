# 🔬 Experiment 02 — TCP Port Scanning

## 📌 Objective

To identify TCP ports exposed by the Windows 11 target using Nmap and correlate the scan results with the TCP traffic captured in Wireshark.

The experiment also includes a controlled firewall comparison to investigate why the initial scan reported all ports as filtered.

---

## 🖥️ Lab Setup

| Component          | Details                     |
| ------------------ | --------------------------- |
| Attacker / Scanner | Kali Linux                  |
| Kali IP            | `192.168.100.10`            |
| Target             | Windows 11                  |
| Windows IP         | `192.168.100.20`            |
| Tools              | Nmap, Wireshark, PowerShell |

---

## 🛠️ Methodology

1. Start Wireshark and capture traffic on the lab network interface.
2. Run a default Nmap scan against the Windows 11 target.
3. Examine the TCP SYN probes and responses.
4. Verify the Windows Firewall state.
5. Confirm that the target network profile is `Public`.
6. Temporarily disable the Public firewall profile for controlled validation.
7. Repeat the same Nmap scan.
8. Compare the results and packet behaviour.
9. Restore the firewall configuration.

### Nmap Command

```bash
nmap 192.168.100.20
```

---

## 🔍 Initial Results — Firewall Enabled

The initial scan reported:

```text
1000 filtered tcp ports (no-response)
```

Wireshark showed TCP SYN probes from:

```text
192.168.100.10 → 192.168.100.20
```

No TCP response was observed from the Windows target.

The Windows Firewall profiles were enabled, and the active network profile was `Public`.

---

## 🔬 Controlled Validation

The Public firewall profile was temporarily disabled:

```powershell
Set-NetFirewallProfile -Profile Public -Enabled False
```

The same Nmap scan was then repeated.

The result changed to:

|    Port | State | Service      |
| ------: | ----- | ------------ |
| 135/tcp | open  | msrpc        |
| 139/tcp | open  | netbios-ssn  |
| 445/tcp | open  | microsoft-ds |

The remaining **997 ports were reported as closed with TCP resets**.

After validation, the firewall was restored:

```powershell
Set-NetFirewallProfile -Profile Public -Enabled True
```

---

## 📡 Packet Analysis

The captures demonstrated three TCP response patterns:

```text
Open:
SYN → SYN/ACK → RST

Closed:
SYN → RST

Filtered:
SYN → No response
```

With the firewall enabled, the scanned SYN probes received no TCP response.

With the firewall temporarily disabled, responses from the Windows host became visible, allowing Nmap to distinguish open and closed ports.

The controlled comparison provides evidence that firewall filtering caused the original no-response/filtered result in this lab.

---

## 📸 [Evidence](experiments/02-port-scanning/evidence)

### Initial scan

* `01-nmap-scan.png` — Initial Nmap result
* `02-wireshark-port-scanning.png` — Initial Wireshark capture
* `03-exp2-nmap-scan.pcapng` — Packet capture with firewall enabled
* `04-firewall-status.png` — Windows Firewall profile status

### Controlled validation

* `07-nmap-rescan.png` — Nmap result after temporarily disabling the Public firewall
* `09-exp2-nmap-rescan.pcapng` — Packet capture from the controlled rescan

The intermediate firewall-change and Wireshark screenshots are not included as final evidence because the PCAPNG files provide the detailed packet-level evidence.

---

## ✅ Conclusion

The experiment demonstrated TCP port scanning with Nmap and packet-level analysis using Wireshark.

The controlled firewall comparison showed how filtering can prevent TCP responses from reaching the scanner, causing Nmap to report ports as filtered.

The packet captures provided evidence connecting the observed TCP behaviour with the resulting Nmap port states.

