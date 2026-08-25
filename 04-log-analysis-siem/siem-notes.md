# Log Analysis & SIEM Notes

## What a SIEM does

Aggregates logs from many sources, normalizes them into a common schema, correlates events across sources, and alerts on defined rules/analytics. Core value is turning scattered raw logs into a searchable, correlated timeline.

## Key log sources to know

- **Windows Event Logs** — Security, System, Application, PowerShell Operational, Sysmon
- **Linux** — `/var/log/auth.log` (or `secure`), `syslog`, `audit.log` (auditd)
- **Firewall/network** — allow/deny events, NAT translations, VPN logs
- **DNS logs** — queries can reveal C2 beaconing, DGA domains, tunneling
- **Proxy/web logs** — user browsing, blocked categories, data volumes
- **EDR/AV** — process creation, file/registry changes, detections
- **Cloud (AWS CloudTrail / Azure Activity Log / GCP Audit Logs)** — API calls, IAM changes, resource creation
- **Identity provider (AD, Okta, Azure AD)** — auth events, MFA challenges, conditional access decisions

## High-value Windows Event IDs

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon (brute force indicator if repeated) |
| 4634 | Logoff |
| 4648 | Logon using explicit credentials (RunAs, lateral movement indicator) |
| 4672 | Special privileges assigned (admin-equivalent logon) |
| 4698 | Scheduled task created (persistence) |
| 4720 | User account created |
| 4726 | User account deleted |
| 4732 | Member added to security-enabled local group |
| 4768/4769 | Kerberos TGT/service ticket requested (Kerberoasting hunting) |
| 4771 | Kerberos pre-auth failed |
| 5140 | Network share accessed |
| 1102 | Audit log cleared (huge red flag — attacker covering tracks) |

## Sysmon event IDs worth alerting on

- **1** — Process creation (with command line — critical for detection)
- **3** — Network connection
- **7** — Image loaded (DLL loading, can catch DLL sideloading/injection)
- **8** — CreateRemoteThread (process injection indicator)
- **11** — File created
- **12/13/14** — Registry event (persistence via Run keys, etc.)
- **22** — DNS query

## Log analysis workflow

1. Establish a baseline of "normal" for the environment before you can spot "abnormal"
2. Pivot from a single alert outward: same host, same user, same source IP, same time window
3. Correlate across log sources — a single log rarely tells the whole story
4. Watch for time gaps or log volume drops (possible log tampering/service stopped)
5. Normalize timestamps to a single timezone (UTC recommended) before building a timeline

## Common query building blocks (concept, adapt syntax to your SIEM: Splunk SPL, KQL, Elastic DSL)

- Filter by time range → filter by source/index → filter by field values → aggregate (count, stats, distinct) → sort → visualize
- Start broad, narrow with `WHERE`/`search` filters, then pivot to `stats`/`summarize` for patterns
- Use `first-time-seen` style hunts: assets, users, or processes appearing for the first time are high-signal

## False positive management

- Document every tuning decision and *why* (don't just silence — understand root cause)
- Prefer narrowing scope (specific host/user exclusion) over disabling a whole rule
- Revisit suppressed/tuned alerts periodically — environments change
