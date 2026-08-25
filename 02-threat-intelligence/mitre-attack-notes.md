# MITRE ATT&CK Notes

MITRE ATT&CK is a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. Used for threat modeling, detection engineering, red/blue team exercises, and mapping incidents to known behavior.

## Structure

- **Tactic** — the *why* (adversary's goal), e.g. "Initial Access", "Persistence"
- **Technique / Sub-technique** — the *how* (specific method used to achieve the tactic)
- **Procedure** — the specific implementation by a specific threat actor/malware

## Enterprise Matrix — Tactics (in typical attack order)

1. **Reconnaissance** — gathering info to plan future operations
2. **Resource Development** — establishing resources (infra, accounts, capabilities)
3. **Initial Access** — getting a foothold (phishing, exploit public-facing app, valid accounts)
4. **Execution** — running attacker code (PowerShell, scripting, scheduled task)
5. **Persistence** — maintaining foothold across reboots/credential changes (registry run keys, services, scheduled tasks)
6. **Privilege Escalation** — gaining higher-level permissions
7. **Defense Evasion** — avoiding detection (obfuscation, disabling security tools, living-off-the-land)
8. **Credential Access** — stealing credentials (Kerberoasting, credential dumping, keylogging)
9. **Discovery** — learning about the environment (network/account/system discovery)
10. **Lateral Movement** — moving through the environment (RDP, SMB, pass-the-hash)
11. **Collection** — gathering data of interest before exfiltration
12. **Command and Control (C2)** — communicating with compromised systems
13. **Exfiltration** — stealing data out of the network
14. **Impact** — disrupt, destroy, or manipulate systems/data (ransomware, wipers, defacement)

## Why analysts use it

- **Mapping alerts to TTPs** gives context beyond a single IOC — e.g., "T1059.001 PowerShell" tells you *how*, not just *that something happened*.
- **Gap analysis** — plot which techniques your detections cover vs. don't (ATT&CK Navigator heatmaps).
- **Threat actor profiles** — groups (e.g., APT29, FIN7) have documented technique sets; useful for hunting hypotheses.
- **Purple teaming** — red team emulates specific technique IDs, blue team validates detection coverage.

## Living-off-the-land (LOLBins) — high value to remember

Attackers abuse legitimate, signed system binaries to evade detection: `powershell.exe`, `certutil.exe` (download files), `mshta.exe`, `rundll32.exe`, `wmic.exe`, `regsvr32.exe`. Flag unusual parent/child process relationships involving these (e.g., `winword.exe` spawning `powershell.exe`).

## IOC vs IOA

- **IOC (Indicator of Compromise)** — evidence something already happened: file hash, malicious IP, domain, registry key
- **IOA (Indicator of Attack)** — behavioral pattern indicating intent, detectable *during* the attack, more resilient to change than static IOCs

## Threat intel sources worth knowing

- MITRE ATT&CK (attack.mitre.org) / ATT&CK Navigator
- CISA advisories & Known Exploited Vulnerabilities (KEV) catalog
- Vendor threat reports (Mandiant, CrowdStrike, Microsoft, Talos)
- OSINT: VirusTotal, AbuseIPDB, Shodan, urlscan.io
- ISACs (Information Sharing and Analysis Centers) for sector-specific intel
- STIX/TAXII for structured, machine-readable threat intel sharing
