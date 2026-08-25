# Core Security Concepts

## CIA Triad

- **Confidentiality** — only authorized parties can access data (encryption, access control, classification)
- **Integrity** — data hasn't been altered improperly (hashing, checksums, digital signatures, version control)
- **Availability** — systems/data are accessible when needed (redundancy, backups, DDoS protection, patching)

Extended models add **Authenticity**, **Non-repudiation**, and **Accountability (AAA)**.

## AAA framework

- **Authentication** — proving identity (password, MFA, certificates, biometrics)
- **Authorization** — what an authenticated identity is allowed to do (RBAC, ABAC, least privilege)
- **Accounting** — logging what was done, by whom, when (audit trails)

## Defense in depth

Layered controls so a single failure doesn't lead to compromise: perimeter (firewall) → network (segmentation, IDS/IPS) → host (EDR, hardening) → application (input validation, WAF) → data (encryption, DLP) → people/process (training, policy).

## Zero Trust

"Never trust, always verify" — no implicit trust based on network location. Core tenets: verify explicitly, use least-privilege access, assume breach (segment, monitor, encrypt everywhere).

## Common attack types (know the mechanism, not just the buzzword)

- **Phishing / spear phishing / whaling** — social engineering via email; whaling targets executives specifically
- **Man-in-the-Middle (MitM)** — attacker intercepts/alters traffic between two parties (ARP spoofing, rogue Wi-Fi AP, SSL stripping)
- **SQL Injection** — unsanitized input alters a backend SQL query (`' OR '1'='1`)
- **Cross-Site Scripting (XSS)** — injected script runs in victim's browser (stored, reflected, DOM-based)
- **Cross-Site Request Forgery (CSRF)** — victim's browser is tricked into making an authenticated request
- **Privilege escalation** — vertical (user → admin) or horizontal (user A → user B's access)
- **Lateral movement** — attacker pivots from initial foothold to other hosts (pass-the-hash, RDP, PsExec, WMI)
- **Ransomware** — encrypts data and demands payment; modern variants also exfiltrate first (double extortion)
- **Supply chain attack** — compromise a trusted vendor/update mechanism to reach many downstream targets
- **Credential stuffing vs brute force** — stuffing reuses breached credential pairs across sites; brute force tries many passwords against one account

## The Cyber Kill Chain (Lockheed Martin)

1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control (C2)
7. Actions on Objectives

Compare with **MITRE ATT&CK** (see `02-threat-intelligence/`), which is more granular and maps real-world TTPs rather than a linear chain.

## Encryption basics

- **Symmetric** (AES) — same key encrypts/decrypts, fast, used for bulk data
- **Asymmetric** (RSA, ECC) — public/private key pair, used for key exchange and signatures
- **Hashing** (SHA-256) — one-way, used for integrity checks, password storage (with salt), not encryption
- **TLS handshake (simplified)**: client hello → server hello + certificate → key exchange → symmetric session key established → encrypted application data

## Risk terminology

- **Vulnerability** — a weakness that could be exploited
- **Threat** — something/someone that could exploit a vulnerability
- **Risk** — likelihood × impact of a threat exploiting a vulnerability
- **Exposure** — instance of being susceptible to loss from a threat
