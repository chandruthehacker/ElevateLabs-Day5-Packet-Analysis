# Day 5 – Wireshark Network Traffic Analysis 📡

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

### ICMPv6 (Ping)

```txt
Frame 12: 118 bytes on wire (944 bits)
Ethernet II, Src: 08:00:27:12:34:56, Dst: 33:33:00:00:00:01
Internet Protocol Version 6, Src: fe80::1, Dst: ff02::1
Internet Control Message Protocol v6
    Type: 128 (Echo request)
    Code: 0
    Identifier: 0x1a2b, Sequence: 1
```
**Insight: Confirms IPv6 connectivity using ping (ICMPv6 echo request).**

### DNS
```txt
Frame 28: 90 bytes on wire (720 bits)
Ethernet II
Internet Protocol Version 4
User Datagram Protocol, Src Port: 48653 → Dst Port: 53
Domain Name System (query)
    Transaction ID: 0x1c77
    Questions: 1
        Name: google.com
        Type: A, Class: IN
```
**Insight: Demonstrates DNS resolution attempt for google.com.**

### HTTP
```txt
Frame 60: 518 bytes on wire (4,144 bits)
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol, Src Port: 443 → 49216
Hypertext Transfer Protocol
    GET / HTTP/1.1
    Host: example.com
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0)
```
**Insight: Plain HTTP GET request showing application layer headers.**


TCP (3-Way Handshake)
```txt
Frame 41: 66 bytes on wire (528 bits)
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
    Src Port: 49216 → Dst Port: 443
    Flags: SYN

Frame 42: 66 bytes on wire (528 bits)
Transmission Control Protocol
    Src Port: 443 → Dst Port: 49216
    Flags: SYN, ACK

Frame 43: 54 bytes on wire (432 bits)
Transmission Control Protocol
    Src Port: 49216 → Dst Port: 443
    Flags: ACK
```

---


## 🔍 Protocol Analysis Summary

### ICMPv6
Captured packets during `ping google.com`. Confirms IPv6 connectivity with echo requests/replies.

### DNS
Shows resolution process for domains like `google.com`. Queries and responses observed using UDP.

### HTTP
Captured plain-text GET request headers while browsing `example.com`.

### TCP
Three-way handshake packets show connection establishment (SYN, SYN-ACK, ACK) for TCP sessions.

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
