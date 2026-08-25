# Cybersecurity Analyst Notes

A personal knowledge base of notes, cheatsheets, templates, and writeups compiled while working and studying as a cybersecurity/SOC analyst. Organized by topic so any note can be found quickly during an investigation, a study session, or an interview prep sprint.

## Structure

| Folder | What's in it |
|---|---|
| [`01-fundamentals/`](01-fundamentals) | Networking basics, core security concepts, ports & protocols reference |
| [`02-threat-intelligence/`](02-threat-intelligence) | MITRE ATT&CK, IOCs, threat actor tracking, intel sources |
| [`03-incident-response/`](03-incident-response) | IR lifecycle notes and ready-to-use report templates |
| [`04-log-analysis-siem/`](04-log-analysis-siem) | Log sources, SIEM query notes, detection engineering basics |
| [`05-vulnerability-management/`](05-vulnerability-management) | Scanning, CVSS, patch/remediation workflow |
| [`06-malware-analysis/`](06-malware-analysis) | Static/dynamic analysis basics, sandboxing, safe-handling notes |
| [`07-tools-cheatsheets/`](07-tools-cheatsheets) | Quick-reference command cheatsheets (Nmap, Wireshark, Splunk, etc.) |
| [`08-certifications-study/`](08-certifications-study) | Study notes and progress tracker for certs (Security+, CySA+, etc.) |
| [`09-ctf-writeups/`](09-ctf-writeups) | CTF challenge writeups and lessons learned |
| [`daily-logs/`](daily-logs) | Daily/shift log template for SOC work |

## How this repo is used

- Each topic folder has its own notes in Markdown so they render cleanly on GitHub.
- `03-incident-response/templates/` and `daily-logs/template.md` are meant to be copied and filled in per real event — copies of filled-in logs stay local/private and are **not** committed here (see `.gitignore`).
- Notes are living documents — update them as tools, threats, and workflows change rather than starting new files.

## Disclaimer

These are personal study/reference notes only. Nothing here contains real incident data, credentials, client information, or anything proprietary — sanitize any real-world details before adding new notes.
