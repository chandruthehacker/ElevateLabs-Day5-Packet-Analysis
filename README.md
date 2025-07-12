# Task 4 – Wireshark Network Traffic Analysis 📡

> **Internship:** Elevate Labs Cybersecurity  
> **Date:** 30-06-2025   
> **Environment:** Kubuntu 22.04 LTS (VMware Workstation Pro) | Wireshark 4.x

---

## 🎯 Objective
Capture live network traffic, isolate four fundamental protocols (ICMPv6, DNS, HTTP, TCP), and document the analysis process.

---

## 🖥️ Lab Setup
| Component | Details |
|-----------|---------|
| **Host OS** | Windows 11 Pro |
| **Guest OS** | Kubuntu 22.04 (64‑bit) in VMware |
| **Wireshark** | 4.x (installed via `apt`) |
| **Network** | TP-Link WiFi Adaptor |
| **Traffic Generator** | `ping google.com`, Firefox browsing to `https://example.com` |

---

## 🔄 Methodology
1. **Update & Install Wireshark**  
   ```bash
   sudo apt update && sudo apt install wireshark -y
   sudo usermod -aG wireshark $USER && reboot
   ```

2. **Start Capture – Interface (TP-Link WiFi Adaptor).**

3. **Generate Traffic – ping google.com (ICMPv6) & web browsing (HTTP over TCP + implicit DNS).**

4. **Stop Capture after ~10 s.**

5. **Apply Display Filters:**
   ```bash
   icmpv6           # Echo request/reply
   dns              # DNS queries & responses
   http             # HTTP requests & responses
   tcp              # TCP segments (handshakes, ACKs)
   ```
6. **Save pcap‑ng files per‑protocol (icmp.pcap, dns.pcap, http.pcap, tcp.pcap).**

---

## 🗂️ Repository Contents
- [tcp.pcap](Captures/tcp.pcap)  
- [dns.pcap](Captures/dns.pcap)  
- [http.pcap](Captures/http.pcap)  
- [icmp-tcp-http-dns.pcap](Captures/icmp-tcp-http-dns.pcap)  
- [screenshots/](Screenshots/) (PNG images referenced below)

---

## 🔍 Protocol Analysis & Sample Packets

### ICMPv6

```txt
Frame 1: 150 bytes on wire (1200 bits)
Ethernet II, Src: 40:ae:30:16:61:2f, Dst: 33:33:00:00:00:16
Internet Protocol Version 6, Src: fe80::376e:bd83:18bf:29ad, Dst: ff02::16
Internet Control Message Protocol v6
    Type: 143 (Multicast Listener Report Message v2)
```
**Insight: Confirms IPv6 connectivity using ping (ICMPv6 echo request).**

### DNS
```txt
Frame 4: 85 bytes on wire
Ethernet II, Src: 40:ae:30:16:61:2f, Dst: ba:b8:e7:6b:dc:84
IP: 192.168.203.196 → 192.168.203.36
UDP, Src Port: 55046 → 53
Domain Name System (query)
    Transaction ID: 0xb706
    Query: AAAA mobile-gtalk.l.google.com
```
**Insight: Demonstrates DNS resolution attempt for google.com.**

### HTTP over TLS (TLS 1.3)
```txt
Frame 21: 799 bytes on wire
TLSv1.3 Record Layer: Handshake Protocol: Client Hello
    Server Name Indication: public.sqrx.com
```
**Insight: Plain HTTP GET request showing application layer headers.**


### TCP (3-Way Handshake)
```txt
Frame 13: SYN
    Src: 55190 → Dst: 443 (HTTPS)
    Flags: SYN, Seq=0

Frame 18: SYN-ACK
    Src: 443 → Dst: 55190
    Flags: SYN, ACK, Seq=0, Ack=1

Frame 19: ACK
    Src: 55190 → Dst: 443
    Flags: ACK, Seq=1, Ack=1
```

---


## 🔍 Protocol Analysis Summary

### ICMPv6
- Captured packets during `ping google.com`. Confirms IPv6 connectivity with echo requests/replies.

### DNS
- Shows resolution process for domains like `google.com`. Queries and responses observed using UDP.

### HTTP
- Captured plain-text GET request headers while browsing `example.com`.

### TCP
- Three-way handshake packets show connection establishment (SYN, SYN-ACK, ACK) for TCP sessions.

---


## 📈 Key Findings
- All target protocols successfully captured and filtered.
- Insight into how packets appear at different OSI layers.
- Able to verify both connection setup and application data exchange.

---

## 🧠 Learnings
| Topic            | Insight                                              |
|------------------|------------------------------------------------------|
| Protocol Isolation | Filters help pinpoint specific communication types |
| Troubleshooting   | Packet visibility helps trace networking issues     |
| Tools             | Wireshark is essential for real-time traffic analysis |


---

> Prepared by **@chandruthehacker** for Elevate Labs Day 5
