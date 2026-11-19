# Packet Capture and Session Hijacking

## Overview
This lab demonstrates how an attacker positioned on a LAN can move from passive packet capture to active session hijacking and denial-of-service, using Ettercap, Wireshark, and Burp Suite. The lab covers ARP poisoning for MITM positioning, HTTP/HTTPS session hijacking through request modification, HTTPS interception via a trusted CA certificate, and a DoS attack carried out through Burp Suite's Intercept feature.

## Environment
- **AttackLinux01** — attacker machine running Ettercap, arpspoof, Wireshark, and Burp Suite
- **vWorkstation** — victim machine, used to access a student to-do list website
- Target network addresses referenced in this lab: `172.30.0.1` (gateway), `172.30.0.2` (victim), `172.30.0.3` (Burp Suite listener)

## Tools Used
- **Wireshark** — passive packet capture and traffic analysis
- **Ettercap** — ARP poisoning for active sniffing and MITM positioning
- **arpspoof** — CLI-based ARP poisoning as an alternative to Ettercap
- **Burp Suite** — proxying, request interception, repeater, and DoS simulation
- **iptables** — firewall rules to redirect traffic into Burp Suite

## Lab Steps

### 1. Passive Packet Capture
Used Wireshark to monitor traffic on the correct network adapter, observing ARP, RARP, MDNS, BROWSER, and ICMP packets between AttackLinux01 and vWorkstation.

### 2. ARP Poisoning
Used Ettercap to poison the ARP cache of the target and gateway, redirecting vWorkstation's traffic through AttackLinux01. Confirmed the attack by capturing HTTP GET and 200 OK packets in Wireshark after generating traffic from vWorkstation.

### 3. HTTP Session Hijacking with Burp Suite
Configured Burp Suite to listen on all interfaces with invisible proxying enabled, and added an iptables rule to redirect port 80 traffic into Burp:

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to 172.30.0.3:8080
```

Logged into the to-do list site from vWorkstation, captured the POST request for a new item in Burp's HTTP history, and used Repeater to resend the request with modified content ("A sneaky item"), demonstrating that a valid session cookie is enough to act as the user.

### 4. HTTPS Session Hijacking
Used `arpspoof` for ARP poisoning instead of Ettercap:

```bash
sudo sysctl -w net.ipv4.ip_forward=1
sudo arpspoof -i eth1 -r -t 172.30.0.2 172.30.0.1
```

Added a second iptables rule to redirect HTTPS traffic:

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j DNAT --to 172.30.0.3:8443
```

Visited the HTTPS to-do list site and confirmed the PortSwigger CA was untrusted, as expected for an unapproved interception certificate. Captured and modified a PUT request in Burp Suite to change an item's `is_finished` status, confirming the change on vWorkstation after a refresh.

### 5. DoS Attack with Burp Suite
Enabled Intercept under Proxy in Burp Suite and dropped requests before they reached the server. This caused the client to hang waiting on a response that would never arrive. Wireshark confirmed this through TCP Keep-Alive packets showing an incomplete conversation between client and server.

### 6. Installing a Trusted CA to Bypass SSL Warnings
Downloaded the Burp Suite CA certificate from the Burp listener and imported it into Firefox as a trusted authority. Revisited the HTTPS site and confirmed the padlock icon returned with no SSL warning, even though Burp Suite was still intercepting and able to decrypt the traffic. Reattempted the DoS attack over HTTPS and confirmed it worked the same way as over HTTP.

## Key Takeaways
- ARP poisoning is the foundation that makes MITM positioning possible on a LAN.
- Session cookies alone don't verify who is making a request — anyone holding the cookie can act as the user.
- HTTPS only protects a connection if the certificate authority behind it is actually trustworthy. Once a malicious CA is trusted, encryption no longer stops interception, it just removes the warning that would normally reveal it.
- A DoS attack doesn't require overwhelming a server with traffic. Controlling the path between client and server is enough to deny service by simply withholding requests.

## Author
Saminur Karim
