# Network Scanning and Packet Sniffing with Nmap and Scapy - Learning Assignment

## Objective
Learn how to perform host discovery, port scanning, and service enumeration using **Nmap**, and packet sniffing/analysis using **Scapy** on a lab network.

This assignment is designed for beginners in ethical hacking / network security. Practice on a safe lab environment (e.g., TryHackMe, HackTheBox, or your own VM network).

---

## Prerequisites
- Kali Linux (or any Linux with Nmap and Scapy installed)
- A lab network with at least 2–3 machines (router + workstations)
- Basic command-line knowledge

---

## Lab Network Used

Based on the lab session, the following IPs were observed:

- **Attacker machine (Kali):
- **Router / Gateway / DNS (dnsmasq):
- **Windows workstation (SMB, RPC):
- **Additional host (possible honeypot/tarpit):
---

## Part 1: Host Discovery with Nmap (20 points)

1. Discover live hosts:
   ```bash
   sudo nmap -sn 192.168.18.0/24
   ```
<img width="591" height="254" alt="image" src="https://github.com/user-attachments/assets/954e694f-9d90-45cc-a138-82b3658945be" />



2. List only IP addresses of live hosts:
   ```bash
   sudo nmap -sn 192.168.18.0/24 --open -oG - | awk '/Up$/{print $2}'
   ```
<img width="575" height="86" alt="image" src="https://github.com/user-attachments/assets/6b2bdb4c-483c-49f1-a705-2d713c4d6967" />

3. Explain:
   - `-sn` → Host discovery only (no port scan)
   - ARP ping → Fast and reliable on LAN

---

## Part 2: Port Scanning with Nmap (30 points)

### Scan the router (`192.168.18.1`) and Windows host (`192.168.18.18`)

1. Quick scan:
   ```bash
   nmap 192.168.18.1
   nmap 192.168.18.18
   ```
<img width="616" height="408" alt="image" src="https://github.com/user-attachments/assets/b255912d-3897-4f88-9997-306b7a47f528" />

2. Full port scan with service detection:
   ```bash
   sudo nmap -p- --open -sSV -Pn -T4 192.168.18.1
   sudo nmap -p- --open -sSV -Pn -T4 192.168.18.18
   ```
<img width="629" height="485" alt="image" src="https://github.com/user-attachments/assets/4dd396fb-b6ba-44d2-8087-71aeeab36cfc" />

3. Aggressive scan:
   ```bash
   sudo nmap -p- --open -sSV -sC -Pn -T4 192.168.18.1
   sudo nmap -p- --open -sSV -sC -Pn -T4 192.168.18.18
   ```
<img width="621" height="555" alt="image" src="https://github.com/user-attachments/assets/7186114c-6abb-4fbc-bee3-af7a112e06fe" />

4. SMB OS discovery (Windows target):
   ```bash
   sudo nmap -p 135,139,445 --script smb-os-discovery 192.168.204.91
   ```
<img width="605" height="172" alt="image" src="https://github.com/user-attachments/assets/de9ce4bd-f810-44c8-88b5-a7f8f48d0bc6" />

---

## Part 3: Packet Sniffing with Scapy 

### 1. Start Scapy
```bash
sudo scapy
```
<img width="516" height="246" alt="image" src="https://github.com/user-attachments/assets/6da53834-2239-49d5-8032-67439dd70bd6" />

### 2. Basic sniff:
```python
paro = sniff(timeout=20)
# In another terminal: ping google.com
paro.summary()
```
<img width="523" height="812" alt="image" src="https://github.com/user-attachments/assets/d6abafa6-08f5-4e5b-9043-75b72eeb1376" />

### 3. Sniff on interface (`eth0`):
```python
paro2 = sniff(iface="eth0", timeout=30)
# Open a website like http://192.168.18.1
paro2.summary()
```
<img width="523" height="494" alt="image" src="https://github.com/user-attachments/assets/a9f6e9e4-489d-4246-a470-fc1beccc7094" />

### 4. Filter ICMP packets:
```python
paro2 = sniff(iface="eth0", filter="icmp", count=10)
# ping 192.168.18.1
paro2.summary()
paro2[3].show()
```
<img width="530" height="741" alt="image" src="https://github.com/user-attachments/assets/17c07cce-074a-4d4a-a352-684ff9683457" />

### 5. Sniff HTTP traffic:
```python
packets = sniff(iface="eth0", filter="host 192.168.204.138 and port 80", timeout=30)
packets.summary()
```
<img width="508" height="808" alt="image" src="https://github.com/user-attachments/assets/40f698a6-d6e8-4385-9611-9c799923aac9" />

### 6. Save capture:
```python
wrpcap("http_capture_192.168.204.138.pcap", packets)
```

---

## ⚠️ Identified Vulnerabilities

### 1. Router (192.168.18.1)

| Issue | Description |
|-------|-------------|
| **Cleartext Login** | Admin credentials sent over HTTP (no HTTPS). |
| **UPnP Service Exposed** | Older UPnP versions have RCE vulnerabilities. |
| **Outdated dnsmasq** | Version shown is old and exploitable in some cases. |

---

### 2. Windows Machine (192.168.18.17)

| Issue | Description |
|-------|-------------|
| **SMB Exposed** | Attack surface for SMB enumeration & brute force. |
| **Weak SSL on uTorrent WebUI** | Self-signed + SHA1, insecure TLS. |
| **Old uTorrent Version** | File access vulnerabilities possible. |

---

## 🧬 Explanation of Packet Layers (From Scapy Output)

### Example: ICMP Echo Request  
From `paro3[3].show()`:

**Ethernet Layer**
- Source MAC: Kali machine  
- Destination MAC: Router  
- Type: IPv4 (0x0800)

**IP Layer**
- Source IP: 192.168.18.18  
- Destination IP: router or workstation  
- TTL: 64  
- Protocol: ICMP

**ICMP Layer**
- Type: 8 = Echo Request  
- Code: 0  
- Sequence number / ID used to match replies

---

### Example: Captured HTTP POST (Router Login)

**TCP Layer:**  
Flag **PA** (Push + ACK) → data is being delivered

**Raw Payload Layer:**  
Contains HTTP POST fields including username/password

**Vulnerability:**  
Credentials transmitted **unencrypted**



