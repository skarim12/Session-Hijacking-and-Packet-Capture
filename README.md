# 🛡️ Packet Capture and Session Hijacking

> A cybersecurity lab demonstrating packet sniffing, ARP poisoning, Man-in-the-Middle (MITM) attacks, session hijacking, HTTPS interception, and denial of service using Wireshark, Ettercap, Burp Suite, arpspoof, and iptables.

---

## 📖 Overview

In this lab, I explored how an attacker can intercept, monitor, modify, and manipulate network traffic after gaining a Man-in-the-Middle (MITM) position on a local network.

Beginning with passive packet capture in Wireshark, I progressed to active sniffing using ARP poisoning before using Burp Suite to intercept HTTP and HTTPS traffic, hijack authenticated sessions, modify requests, and perform a denial-of-service attack.

This lab demonstrates why network segmentation, trusted certificate authorities, encrypted communications, and secure session management are essential for protecting users from MITM attacks.

---

# 🧰 Technologies Used

- Wireshark
- Ettercap
- Burp Suite
- arpspoof
- iptables
- Firefox
- Linux
- HTTP / HTTPS
- ARP
- TCP/IP

---

# 🎯 Objectives

- Capture network traffic with Wireshark
- Analyze ICMP and HTTP packets
- Perform ARP poisoning
- Conduct Man-in-the-Middle attacks
- Intercept HTTP traffic
- Hijack authenticated web sessions
- Modify HTTP requests using Burp Suite
- Intercept HTTPS traffic
- Understand certificate trust
- Perform a simple DoS attack
- Analyze captured packets throughout the attack

---

# 🔬 Lab Workflow

## 1️⃣ Passive Packet Capture

The lab began with Wireshark to observe normal network communication.

Captured traffic included:

- ICMP
- ARP
- RARP
- mDNS
- Browser traffic

An ICMP filter was applied to observe ping traffic between hosts.

**Skills Demonstrated**

- Packet capture
- Protocol analysis
- Traffic filtering
- Packet inspection

---

## 2️⃣ ARP Poisoning

Ettercap was used to poison ARP tables between two systems.

By convincing each host that the attacker's MAC address belonged to the other device, all traffic flowed through the attacker.

This established a successful Man-in-the-Middle position.

**Concepts Learned**

- ARP spoofing
- MAC address manipulation
- MITM positioning
- Active sniffing

---

## 3️⃣ HTTP Session Hijacking

Once traffic was flowing through the attacker, Burp Suite was configured as a transparent proxy.

Configuration included:

- Listening on all interfaces
- Invisible Proxying
- iptables NAT redirection
- HTTP interception

After the victim logged into the web application, Burp captured the authenticated requests.

Using the captured session cookie, I modified POST requests inside Burp Repeater and changed application data without logging in myself.

### Demonstrated

- Session cookies
- HTTP interception
- Request replay
- POST modification
- Session hijacking

---

## 4️⃣ HTTPS Interception

The attack was repeated against HTTPS traffic.

Using:

- arpspoof
- IP forwarding
- iptables
- Burp Suite

HTTPS traffic was redirected through Burp Suite.

Initially, Firefox displayed a certificate warning because the PortSwigger CA was untrusted.

After importing Burp Suite's CA certificate into Firefox, the browser trusted the connection and HTTPS traffic became fully visible inside Burp.

### Demonstrated

- HTTPS interception
- TLS certificate validation
- Certificate Authority trust
- SSL inspection
- Secure traffic decryption

---

## 5️⃣ Modifying HTTPS Requests

Captured HTTPS requests were edited inside Burp Repeater.

Instead of modifying a POST request, a PUT request was edited.

The request was altered to mark a different task as completed.

Refreshing the webpage confirmed that the server accepted the modified request.

This demonstrated that possessing a valid session cookie allows an attacker to perform actions as the authenticated user.

---

## 6️⃣ Denial of Service

Burp Suite was then used to interrupt communication between the client and server.

By intercepting requests and dropping TCP packets before forwarding them, the server never received the client's requests.

Wireshark confirmed the disruption by showing TCP Keep-Alive packets while the application eventually timed out.

### Demonstrated

- Request interception
- Packet dropping
- TCP analysis
- Availability attacks

---

# 📊 Key Concepts

| Concept | Description |
|----------|-------------|
| Passive Sniffing | Observing network traffic without modifying it |
| Active Sniffing | Manipulating network traffic to intercept communications |
| ARP Poisoning | Redirecting traffic by spoofing ARP responses |
| MITM | Positioning between two communicating hosts |
| Session Hijacking | Using stolen session cookies to impersonate users |
| Burp Suite | Intercepting and modifying HTTP/HTTPS requests |
| HTTPS Interception | Decrypting encrypted traffic using a trusted CA |
| DoS | Preventing communication between client and server |

---

# 🛡️ Security Lessons

This lab highlighted several important security principles:

- Encryption is only trustworthy when certificates are trusted.
- Session cookies effectively become authentication tokens.
- ARP poisoning can redirect all LAN traffic through an attacker.
- HTTPS alone cannot stop MITM attacks if users trust malicious certificates.
- Packet captures reveal valuable information about network behavior.
- Burp Suite demonstrates how requests can be intercepted and modified in transit.
- Denial-of-service attacks can be performed without compromising the target server itself.

---

# 📚 Skills Demonstrated

- Network Traffic Analysis
- Wireshark Packet Inspection
- ARP Poisoning
- Packet Sniffing
- MITM Attacks
- HTTP Analysis
- HTTPS Analysis
- Session Hijacking
- Burp Suite Proxying
- Request Manipulation
- Firewall Rule Configuration
- iptables NAT
- Linux Networking
- TCP/IP Analysis
- SSL/TLS Inspection

---

# 📸 Lab Screenshots

This repository contains screenshots demonstrating:

- Wireshark packet captures
- ICMP traffic
- ARP poisoning
- Ettercap configuration
- Burp Suite Proxy
- HTTP History
- Burp Repeater
- HTTPS interception
- Certificate installation
- Session hijacking
- PUT request modification
- DoS attack
- TCP Keep-Alive packets

---

# ✅ Conclusion

This lab demonstrated how a Man-in-the-Middle attacker can escalate from simply observing network traffic to fully manipulating authenticated web sessions. By combining ARP poisoning, Burp Suite, and packet analysis, I intercepted both HTTP and HTTPS communications, modified application requests, hijacked user sessions, and disrupted connectivity through a denial-of-service attack.

One of the biggest takeaways was that HTTPS only provides protection when users trust legitimate certificate authorities. Once a malicious certificate is accepted, encrypted traffic can still be decrypted, modified, and forwarded without the victim realizing it. Overall, this lab reinforced the importance of secure certificate validation, protected session management, and network defenses against MITM attacks.

---

## 👨‍💻 Author

**Saminur Karim**

Cybersecurity Student | Future SOC Analyst

**Skills:** Wireshark • Burp Suite • Ettercap • Packet Analysis • Network Security • MITM • Session Hijacking • TCP/IP • Linux • HTTPS • Incident Analysis
