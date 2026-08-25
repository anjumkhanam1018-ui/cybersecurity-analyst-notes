# Incident Response Process Notes

Based on the NIST SP 800-61 lifecycle, the industry-standard reference.

## The 4 phases (NIST)

### 1. Preparation
- Have an IR plan, playbooks, and contact list *before* an incident happens
- Ensure logging/monitoring coverage, EDR deployed, backups tested
- Pre-stage tools: forensic imaging tools, jump bag, communication channels (out-of-band, in case email/chat is compromised)

### 2. Detection & Analysis
- Identify precursors (signs an incident *may* occur) vs indicators (signs one *is/has* occurring)
- Common detection sources: SIEM alerts, EDR, IDS/IPS, user reports, threat intel matches
- Triage and prioritize: scope, business impact, data sensitivity, containment urgency
- Document everything with timestamps as you go — this becomes the incident timeline

### 3. Containment, Eradication & Recovery
- **Containment** — stop the bleeding first. Short-term (isolate host, disable account, block IOC at firewall) vs long-term (patch, rebuild, rotate credentials broadly)
- **Eradication** — remove the root cause (malware, persistence mechanisms, attacker-created accounts)
- **Recovery** — restore systems to normal operation, verify with heightened monitoring before declaring "all clear"

### 4. Post-Incident Activity
- Lessons-learned / after-action review — what worked, what didn't, what to change
- Update playbooks and detection rules based on findings
- Calculate cost/impact for reporting to leadership

## Containment decision factors

- Potential damage/theft of resources vs. need to preserve evidence
- Service availability requirements (can we take this system offline?)
- Time/resources required for each containment strategy
- Effectiveness (partial containment vs. full)
- Duration of the solution (temporary vs. permanent fix)

## Chain of custody essentials (if evidence may go legal)

- Who collected it, when, where, how
- Every transfer of evidence logged (who received it, when, condition)
- Hashes taken at collection time and verified at every subsequent access
- Store evidence in a secured, access-controlled location

## Severity/priority framework (example — adapt to your org's matrix)

| Severity | Criteria | Example |
|---|---|---|
| Critical (P1) | Active data breach, ransomware spreading, critical system down | Domain controller compromised |
| High (P2) | Confirmed compromise, contained but not eradicated | Single workstation malware, contained |
| Medium (P3) | Suspicious activity requiring investigation | Unusual login pattern, no confirmed compromise |
| Low (P4) | Policy violation, informational | Blocked phishing email, no click |

## Communication during an incident

- Keep a single source of truth (incident channel/ticket)
- Use out-of-band communication if the primary comms system may be compromised
- Know your legal/regulatory notification requirements and timelines (varies by industry/jurisdiction/data type — confirm with legal/compliance, don't assume)
