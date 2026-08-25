# Networking Basics for Security Analysts

## The OSI Model (7 layers)

| Layer | Name | Examples / Relevance to security |
|---|---|---|
| 7 | Application | HTTP, DNS, SMTP — where most web app attacks (XSS, SQLi) live |
| 6 | Presentation | Encryption/encoding (TLS handshake details, character sets) |
| 5 | Session | Session establishment/teardown — session hijacking targets this |
| 4 | Transport | TCP/UDP — port scanning, SYN floods, segmentation |
| 3 | Network | IP, ICMP, routing — spoofing, traceroute, subnetting |
| 2 | Data Link | MAC addresses, switches — ARP spoofing, VLAN hopping |
| 1 | Physical | Cabling, radio — physical access, Wi-Fi sniffing |

TCP/IP model collapses this into 4 layers: Application, Transport, Internet, Network Access.

## TCP vs UDP

- **TCP**: connection-oriented, 3-way handshake (SYN → SYN/ACK → ACK), reliable/ordered delivery, used by HTTP(S), SSH, SMTP.
- **UDP**: connectionless, no handshake, faster but unreliable, used by DNS, DHCP, VoIP, streaming.

## The TCP 3-way handshake (know this cold — it underlies SYN scans and floods)

1. Client → Server: `SYN`
2. Server → Client: `SYN, ACK`
3. Client → Server: `ACK`

A **SYN flood** DoS attack sends step 1 repeatedly without completing step 3, exhausting the server's half-open connection table.

## IP addressing & subnetting quick reference

- IPv4 private ranges (RFC1918): `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`
- `/24` = 256 addresses (254 usable), `/16` = 65,536, `/8` = ~16.7M
- CIDR notation: the number after `/` is the count of network bits (fixed bits); remaining bits are host bits.
- `169.254.0.0/16` = APIPA (link-local, DHCP failure indicator worth flagging)

## Common network devices and their security relevance

- **Router**: routes between networks, first point for ACL/firewall rules
- **Switch**: forwards by MAC (Layer 2); vulnerable to ARP spoofing/CAM table overflow
- **Firewall**: enforces allow/deny rules by IP/port/protocol (stateless) or connection state (stateful) or app-layer (NGFW/WAF)
- **IDS vs IPS**: IDS *detects and alerts* (passive/out-of-band); IPS *detects and blocks* (inline, active)
- **Proxy**: forward proxy hides clients from servers; reverse proxy hides servers from clients (also load balances, terminates TLS)

## DNS essentials

- Resolution order: browser cache → OS cache → hosts file → recursive resolver → root → TLD → authoritative
- Record types to know: `A` (IPv4), `AAAA` (IPv6), `MX` (mail), `TXT` (often SPF/DKIM/DMARC), `CNAME` (alias), `NS` (nameserver), `PTR` (reverse lookup)
- Security relevance: DNS tunneling (data exfil via TXT/queries), DNS spoofing/cache poisoning, fast-flux DNS (malware C2 evasion), typosquatting domains

## VPNs & tunneling

- IPsec (site-to-site, network layer) vs SSL/TLS VPN (client, often app/session layer)
- Split tunneling risk: only some traffic goes through VPN, rest goes direct — can bypass corporate monitoring
