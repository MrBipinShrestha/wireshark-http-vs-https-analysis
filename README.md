# 🔬 Network Protocol Analysis — HTTP vs HTTPS

![Wireshark](https://img.shields.io/badge/Wireshark-Packet_Analysis-blue?style=for-the-badge)
![MITRE ATT&CK](https://img.shields.io/badge/MITRE-T1040-red?style=for-the-badge)
![Focus](https://img.shields.io/badge/Focus-Network_Security-purple?style=for-the-badge)

> **Network security lab demonstrating plaintext credential exposure in HTTP vs encrypted traffic in HTTPS using live Wireshark packet capture and analysis.**
>
> This lab validates a core SOC assumption: unencrypted protocols are a critical detection and data-theft vector.

---

## 🧠 Security Engineering Objective

This lab answers a fundamental network security question:

> "What data is exposed to a network-level attacker intercepting HTTP vs HTTPS traffic?"

To validate this, I captured and analyzed live network traffic across both protocols and documented:

- Credential exposure in plaintext protocols
- TLS encryption effectiveness
- Detection opportunities for SOC analysts
- Network-level attacker capabilities

---

## 🏗️ Lab Architecture

```
┌─────────────┐     HTTP (port 80)      ┌──────────────┐
│   Browser   │ ──────────────────────► │  Web Server  │
│   Client    │ ◄────────────────────── │              │
└──────┬──────┘   Plaintext Response    └──────────────┘
       │
       │  Wireshark captures all traffic
       ▼
┌─────────────────────────────┐
│   Wireshark Packet Capture  │
│   - Protocol dissection     │
│   - Credential extraction   │
│   - Traffic analysis        │
└─────────────────────────────┘

┌─────────────┐    HTTPS (port 443)     ┌──────────────┐
│   Browser   │ ──────────────────────► │  Web Server  │
│   Client    │ ◄────────────────────── │  TLS 1.3     │
└─────────────┘   Encrypted Response    └──────────────┘
       │
       │  Wireshark captures encrypted blobs only
       ▼
┌─────────────────────────────┐
│   Wireshark Packet Capture  │
│   - TLS handshake visible   │
│   - Payload: encrypted      │
│   - No credential exposure  │
└─────────────────────────────┘
```

---

## 🚨 Key Findings

### HTTP — Plaintext Exposure

| Data Type | Exposed? | Risk |
|-----------|----------|------|
| Usernames | ✅ YES | Critical |
| Passwords | ✅ YES | Critical |
| Session tokens | ✅ YES | Critical |
| Form data | ✅ YES | High |
| Cookie values | ✅ YES | High |

**Wireshark capture shows:**
- Full HTTP GET/POST requests visible
- Credentials readable in packet payload
- No decryption required by attacker

---

### HTTPS — Encrypted Protection

| Data Type | Exposed? | Risk |
|-----------|----------|------|
| Usernames | ❌ NO | None |
| Passwords | ❌ NO | None |
| Session tokens | ❌ NO | None |
| Form data | ❌ NO | None |
| Cookie values | ❌ NO | None |

**Wireshark capture shows:**
- TLS ClientHello/ServerHello visible
- SNI (hostname) visible — metadata only
- Payload: encrypted, unreadable
- Certificate exchange visible

---

## 📊 MITRE ATT&CK Mapping

| Tactic | Technique | Relevance |
|--------|-----------|-----------|
| Credential Access | T1040 (Network Sniffing) | HTTP credential capture |
| Collection | T1557 (Adversary-in-the-Middle) | HTTP interception |
| Defense Evasion | T1573 (Encrypted Channel) | HTTPS protection |

---

## 🔍 SOC Detection Opportunities

### Detecting HTTP Credential Exposure
```
Alert: Unencrypted credential submission detected
Logic: HTTP POST containing password= or passwd= fields
Severity: HIGH
Action: Alert SOC + block/redirect to HTTPS
```

### Detecting Network Sniffing Attempts
```
Alert: Suspicious promiscuous mode detected
Rule: Wireshark/tcpdump process on non-analyst host
Severity: MEDIUM
Action: Investigate host for insider threat
```

### Detecting Protocol Downgrade Attacks
```
Alert: HTTPS to HTTP redirect detected
Logic: 301/302 redirect from https:// to http://
Severity: HIGH
Action: Investigate for MITM or misconfiguration
```

---

## 🧰 Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Packet capture and protocol analysis |
| HTTP server | Plaintext traffic generation |
| HTTPS server | Encrypted traffic generation |
| Browser | Client traffic generation |

---

## 📸 Evidence

Screenshots in `screenshots/` folder:
- HTTP packet showing plaintext credentials
- HTTPS packet showing encrypted payload
- Wireshark filter syntax for each protocol
- Side-by-side protocol comparison

---

## 📈 Key Takeaways

- HTTP transmits credentials in plaintext — visible to any network observer
- HTTPS encrypts all payload data — only metadata (SNI, IP, timing) is visible
- SOC analysts should alert on any authentication over HTTP
- Network segmentation + TLS enforcement are critical security controls

---

## 🔮 Extensions

- [ ] TLS certificate validation analysis
- [ ] SSL stripping attack simulation
- [ ] HSTS bypass techniques
- [ ] Certificate pinning bypass analysis
- [ ] Network-level DLP detection rules

---

## 📬 Contact

**GitHub:** [github.com/MrBipinShrestha](https://github.com/MrBipinShrestha)
**LinkedIn:** [linkedin.com/in/shresthabipin](https://www.linkedin.com/in/shresthabipin)
**Location:** Sydney, Australia
