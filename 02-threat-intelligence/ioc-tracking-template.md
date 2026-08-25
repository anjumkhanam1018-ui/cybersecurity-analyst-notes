# IOC Tracking Log (Template)

Copy this table into a new note per investigation/campaign. Keep entries atomic (one IOC per row).

| Date Added | IOC Type | Value | Source | Related Case/Alert | Confidence | Status | Notes |
|---|---|---|---|---|---|---|---|
| YYYY-MM-DD | IP / Domain / Hash / URL / Email | | | | Low/Med/High | Active/Expired/False Positive | |

## IOC types cheat sheet

- **IP address** — check reputation (AbuseIPDB, VirusTotal), geolocation, ASN/hosting provider, whether it's a known VPN/Tor exit node/cloud provider (AWS/Azure ranges are often benign but abused)
- **Domain** — WHOIS age (newly registered domains are higher risk), DNS history, typosquat similarity to known brands
- **File hash** — always record all three if available: MD5, SHA1, SHA256 (SHA256 preferred for uniqueness)
- **Email address / header** — sender, reply-to mismatch, SPF/DKIM/DMARC results
- **URL** — full path matters (query strings can carry payloads/tracking), check via sandboxed detonation, not direct browsing

## Enrichment checklist before acting on an IOC

1. Cross-check against internal allowlists (avoid blocking legitimate infra)
2. Check multiple reputation sources — don't rely on a single vendor
3. Determine scope: how many internal hosts have touched this IOC?
4. Check first-seen/last-seen internally to establish a timeline
5. Document the pivot: what led you to this IOC, and what does it lead to next?
