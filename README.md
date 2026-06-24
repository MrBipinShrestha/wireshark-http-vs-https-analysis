# Wireshark HTTP vs HTTPS Analysis

**Network security lab demonstrating plaintext HTTP traffic exposure vs TLS-encrypted HTTPS protection using Wireshark packet analysis.**

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-Kali%20Linux-blue.svg)
![Tool](https://img.shields.io/badge/tool-Wireshark-blue.svg)
![Relevant](https://img.shields.io/badge/for-SOC%20%26%20Network%20Analysts-brightgreen.svg)

---

## 🎯 Overview

This lab demonstrates **how HTTPS encryption protects sensitive data** compared to unencrypted HTTP. By analyzing actual network traffic, security professionals learn:

- How HTTP exposes credentials and sensitive data
- Why HTTPS encryption is critical
- How TLS/SSL protects confidentiality
- Network-level traffic analysis techniques
- Security implications for organizations
- Practical encryption implementation

**Target Audience:** SOC analysts, network administrators, security engineers, incident responders, students.

---

## 🔬 Lab Objectives

After completing this lab, you will understand:

✅ HTTP protocol and plaintext data exposure  
✅ TLS/SSL encryption mechanisms  
✅ Packet capture using Wireshark  
✅ Difference between encrypted and plaintext traffic  
✅ Why HTTPS is essential for security  
✅ Real-world implications of unencrypted protocols  
✅ Network security best practices  

---

## 📋 Lab Environment

### Requirements

**Hardware:**
- Virtual Machine with 1GB+ RAM
- Network interface with internet access
- 500MB free disk space

**Software:**
- Kali Linux or Ubuntu
- Wireshark (network protocol analyzer)
- Web browser (Firefox, Chromium)
- Internet connection
- Optional: neverssl.com (HTTP-only test site)

**Knowledge:**
- Basic networking concepts
- TCP/IP fundamentals
- Web browser basics
- Packet structure understanding

---

## 🚀 Quick Start (30 Minutes)

### Step 1: Install Wireshark
```bash
# Install Wireshark
sudo apt-get install wireshark -y

# Add current user to wireshark group (avoid sudo)
sudo usermod -a -G wireshark $(whoami)

# Verify installation
wireshark --version
```

### Step 2: Capture HTTP Traffic
```bash
# Start Wireshark
wireshark &

# Select your network interface (eth0, wlan0, etc.)
# Start capture (green shark fin button)

# Open browser and visit (in NEW TERMINAL):
# HTTP (unencrypted): http://neverssl.com
```

### Step 3: Analyze HTTP Plaintext
```
In Wireshark:
1. Filter for HTTP traffic:
   http

2. Find HTTP GET request
   Look for: "GET / HTTP/1.1"

3. Expand HTTP layer
   View plaintext data:
   - Host header (which website)
   - User-Agent (browser info)
   - All headers visible in plaintext

4. Expand TCP layer
   Packet data is completely readable
```

### Step 4: Capture HTTPS Traffic
```bash
# Stop HTTP capture
# Clear capture (trash button)
# Start new capture

# Visit HTTPS site:
# https://example.com
```

### Step 5: Analyze HTTPS Encryption
```
In Wireshark:
1. Filter for TLS traffic:
   tls

2. Find TLS Handshake
   Look for: "Client Hello"

3. Examine TLS Record
   Note: "Encrypted Application Data"
   No plaintext visible!

4. Try to read application data
   Result: Encrypted and unreadable
   WITHOUT encryption key
```

---

## 📊 Lab Components

### Phase 1: HTTP Traffic Capture
- Network interface selection
- HTTP traffic generation
- Packet capture confirmation

### Phase 2: HTTP Analysis
- Plaintext header exposure
- URL visibility
- User-Agent exposure
- HTTP request structure

### Phase 3: TLS Handshake Analysis
- Client Hello
- Server Hello
- Certificate exchange
- Key agreement

### Phase 4: HTTPS Traffic Analysis
- Encrypted application data
- No plaintext exposure
- Certificate verification
- Security indicators

### Phase 5: Comparative Analysis
- HTTP vs HTTPS comparison
- Security implications
- Real-world applications
- Best practices

---

## 🔍 Traffic Comparison

### HTTP (Unencrypted) - What Attackers See

```
GET / HTTP/1.1
Host: neverssl.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64)...
Accept: text/html,application/xhtml+xml...
Accept-Language: en-US,en;q=0.9
Connection: upgrade
Upgrade-Insecure-Requests: 1

[Complete plaintext response with HTML, forms, etc.]
```

**Exposed Information:**
- ✗ Website being visited
- ✗ Browser type and version
- ✗ Operating system
- ✗ Language preferences
- ✗ Form data (if POSTed)
- ✗ Cookies (if sent)
- ✗ Response content (HTML, images)

### HTTPS (TLS Encrypted) - What Attackers See

```
TLSv1.3 Record Layer: Handshake Protocol: Client Hello
    Content Type: Handshake (22)
    Version: TLS 1.2 (0x0303)
    Length: 512
    Handshake Protocol: Client Hello
        Version: TLS 1.2 (0x0303)
        Random: [encrypted bytes...]
        Session ID Length: 0
        Cipher Suites Length: 50
        Cipher Suites: [encrypted...]

[Encrypted Application Data - UNREADABLE]
```

**Protected Information:**
- ✓ Encryption protects all data
- ✓ No plaintext headers visible
- ✓ Packet contents encrypted
- ✓ Only metadata visible (packet sizes, timing)

---

## 📈 Key Findings from Lab

### Security Impact Analysis

**HTTP Vulnerability:**
```
Attacker Position: Network (same WiFi, ISP, etc.)

Capabilities:
✗ Read all URLs visited
✗ Capture login credentials (if sent as plaintext)
✗ Steal form data submissions
✗ Modify responses (MITM attack)
✗ Inject malicious content
✗ Track user behavior
✗ Steal cookies and sessions
```

**HTTPS Protection:**
```
Attacker Position: Network (same WiFi, ISP, etc.)

Capabilities:
✓ See encrypted data (unreadable)
✓ See packet size and timing
✓ See destination IP/port
✓ Cannot read content
✓ Cannot modify traffic (detected by TLS)
✓ Cannot forge responses
✓ Cannot steal credentials
```

### Real-World Attack Scenarios

**HTTP Without Protection:**
```
Scenario: User on public WiFi at coffee shop
1. Attacker captures unencrypted HTTP traffic
2. Attacker sees user login to email
3. Attacker captures credentials
4. Attacker logs into user's email
5. Attacker resets passwords, accesses accounts

Result: Complete account compromise
```

**HTTPS With Protection:**
```
Scenario: User on same public WiFi
1. Attacker captures encrypted HTTPS traffic
2. Attacker sees only encrypted packets
3. Attacker cannot decrypt without key
4. Attacker cannot read credentials
5. Login attempt fails (encrypted)

Result: User account remains secure
```

---

## 🛡️ Security Best Practices

### For Users
```
✓ Always check for HTTPS (padlock icon)
✓ Never enter credentials on HTTP sites
✓ Use VPN on public WiFi
✓ Check certificate validity
✓ Don't bypass certificate warnings
✓ Use browsers' security features
```

### For Organizations
```
✓ Enforce HTTPS everywhere
✓ Use modern TLS versions (1.2+)
✓ Strong cipher suites
✓ Valid, trusted certificates
✓ HSTS header configuration
✓ Regular security audits
✓ Train employees about HTTPS
```

### For Developers
```
✓ Use HTTPS for all connections
✓ Implement HSTS (HTTP Strict-Transport-Security)
✓ Redirect HTTP to HTTPS
✓ Use strong TLS configuration
✓ Keep certificates updated
✓ Use secure cookie flags
✓ Regular security testing
```

---

## 🧪 Lab Exercises

### Exercise 1: HTTP Traffic Capture
**Objective:** Capture plaintext HTTP traffic  
**Duration:** 15 minutes  
**Difficulty:** Beginner  
See: `01-http-capture.md`

### Exercise 2: Plaintext Data Extraction
**Objective:** Identify exposed data in HTTP packets  
**Duration:** 20 minutes  
**Difficulty:** Beginner  
See: `02-plaintext-analysis.md`

### Exercise 3: TLS Handshake Analysis
**Objective:** Understand HTTPS encryption setup  
**Duration:** 20 minutes  
**Difficulty:** Intermediate  
See: `03-tls-handshake.md`

### Exercise 4: Encrypted Traffic Analysis
**Objective:** Analyze encrypted HTTPS traffic  
**Duration:** 20 minutes  
**Difficulty:** Intermediate  
See: `04-encrypted-analysis.md`

### Exercise 5: Comparative Security Analysis
**Objective:** Compare HTTP vs HTTPS security  
**Duration:** 30 minutes  
**Difficulty:** Advanced  
See: `05-security-comparison.md`

---

## 📁 Repository Structure

```
wireshark-http-vs-https-analysis/
├── README.md (this file)
├── LICENSE
│
├── 01-lab-setup/
│   ├── wireshark-installation.md
│   ├── interface-selection.md
│   └── browser-configuration.md
│
├── 02-http-analysis/
│   ├── http-protocol-overview.md
│   ├── plaintext-exposure.md
│   ├── header-analysis.md
│   └── vulnerability-demo.md
│
├── 03-https-analysis/
│   ├── tls-ssl-overview.md
│   ├── encryption-mechanisms.md
│   ├── certificate-analysis.md
│   └── secure-communication.md
│
├── 04-exercises/
│   ├── 01-http-capture.md
│   ├── 02-plaintext-analysis.md
│   ├── 03-tls-handshake.md
│   ├── 04-encrypted-analysis.md
│   └── 05-security-comparison.md
│
├── 05-practical-guides/
│   ├── wireshark-filters-reference.md
│   ├── http-header-meanings.md
│   ├── tls-record-structure.md
│   └── certificate-verification.md
│
├── 06-resources/
│   ├── networking-fundamentals.md
│   ├── encryption-explained.md
│   ├── https-best-practices.md
│   └── troubleshooting.md
│
├── pcap-files/
│   ├── http-traffic.pcapng
│   └── https-traffic.pcapng
│
└── screenshots/
    ├── http-plaintext-headers.png
    ├── http-exposed-data.png
    ├── tls-handshake.png
    ├── https-encrypted-traffic.png
    └── wireshark-comparison.png
```

---

## 💻 Wireshark Reference

### Installation
```bash
# Install Wireshark
sudo apt-get install wireshark

# Add user to wireshark group
sudo usermod -a -G wireshark $USER
```

### Common Filters

```bash
# Filter for HTTP traffic
http

# Filter for HTTPS/TLS traffic
tls

# Filter for specific domain
ip.dst == 93.184.216.34  (example.com IP)

# Filter for packets containing specific string
frame contains "password"

# Filter for DNS queries
dns

# Filter for traffic to/from specific IP
ip.addr == 192.168.1.100
```

### Useful Display Options

```bash
# Show HTTP methods and URLs
http.request.method

# Show response codes
http.response.code

# Show TLS versions
tls.version

# Show certificates
tls.handshake.certificate

# Expand dissectors
Right-click → Follow → HTTP Stream (read complete conversation)
```

---

## 📊 Statistics & Context

**HTTPS Adoption:**
- **96%** of major websites use HTTPS (2024)
- **78%** of traffic is encrypted globally
- **99%+** of browsers support HTTPS
- Legacy HTTP usage: **4%** and declining

**Why HTTPS Matters:**
- **Confidentiality** — Data cannot be read by eavesdroppers
- **Integrity** — Data cannot be modified in transit
- **Authentication** — Verifies server identity
- **Compliance** — Required by regulations (GDPR, PCI-DSS, HIPAA)
- **Trust** — Browser warnings help users avoid fake sites

**This Lab Teaches:**
- Real security impact of encryption
- Practical packet analysis
- Why technical controls matter
- Network security fundamentals

---

## 🛠️ Tools & Resources

### Wireshark
```
Official: https://www.wireshark.org/
Documentation: https://wiki.wireshark.org/
Download: https://www.wireshark.org/download/
License: GPL v2
```

### Test Sites (HTTP & HTTPS)
```
HTTP (unencrypted): http://neverssl.com
HTTPS (encrypted): https://example.com, https://google.com
```

### Related Tools
```
tcpdump — Command-line packet capture
tshark — Wireshark command-line version
mitmproxy — Man-in-the-middle proxy
Burp Suite — Web security testing
```

---

## 📚 Learning Resources

### Official Documentation
- **Wireshark User Guide:** https://www.wireshark.org/docs/wsug_html_chunked/
- **HTTP Protocol:** https://tools.ietf.org/html/rfc7230
- **TLS 1.3:** https://tools.ietf.org/html/rfc8446

### Recommended Reading
- **OWASP Network Security:** https://owasp.org/
- **NIST Encryption Guidelines:** https://pages.nist.gov/
- **Browser Security:** https://hstspreload.org/

### Related Projects
- Network Anomaly Detection (IDS using ML)
- Social Engineering Awareness Lab (Phishing)
- ClamAV Malware Detection Lab (Endpoint security)

---

## ❓ FAQ

**Q: Why do we need HTTPS if we just encrypt one layer?**  
A: Transport layer encryption protects in transit. Combined with application-level security, it provides defense in depth.

**Q: Can someone still see my traffic if I use HTTPS?**  
A: They can see packet size, timing, and destination. But not content. This is acceptable for most use cases.

**Q: Why isn't everything using HTTPS?**  
A: Most things do (96% of major sites). Legacy systems and IoT devices sometimes use HTTP.

**Q: Can HTTPS be cracked?**  
A: Breaking modern encryption (AES-256, TLS 1.3) would take longer than the universe's age with current technology.

**Q: What if the certificate is self-signed?**  
A: Browser warns users. Acceptable for internal systems but users should verify before trusting.

**Q: Does HTTPS protect against all attacks?**  
A: No. It protects confidentiality in transit. Other attacks (malware, phishing, weak passwords) are not prevented by HTTPS alone.

---

## 📄 License

MIT License — Educational use only.

See `LICENSE` file for full details.

---

## 👤 Author

**Repository Created By:** Bipin Shrestha  
**Focus Area:** Network Security & Traffic Analysis  
**Last Updated:** June 2024  
**Maintained For:** SOC analysts, network engineers, students

---

## 🔗 Related Projects

⭐ **[Network-Anomaly-Detection-UNSW-NB15](https://github.com/MrBipinShrestha/network-anomaly-detection-unsw-nb15)**  
Machine learning-based IDS with network traffic analysis

⭐ **[Social-Engineering-Awareness-Lab](https://github.com/MrBipinShrestha/social-engineering-awareness-lab)**  
Credential harvesting and phishing attack simulation using SET

⭐ **[ClamAV-Malware-Detection-Lab](https://github.com/MrBipinShrestha/clamav-malware-detection-lab)**  
Signature-based malware detection and removal on Linux

---

## ⭐ Don't Forget to Star!

If this lab helped you understand network security and encryption, **please star this repository** ⭐

Your star helps others discover this educational resource.

---

**Securing networks, one packet at a time.** 🔒

