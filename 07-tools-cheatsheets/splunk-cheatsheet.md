# Splunk SPL Cheatsheet

## Basic search structure

```
index=main sourcetype=access_combined status=404
| stats count by clientip
| sort -count
```

Pipe (`|`) chains commands left to right: search → transform → format.

## Common commands

```
stats count by field            # Aggregate/count grouped by field
top field                       # Most common values of a field
rare field                      # Least common values (good for anomaly hunting)
table field1 field2             # Display only specified fields
fields + field1, field2         # Keep only specified fields
fields - field1                 # Remove a field
dedup field                     # Remove duplicate events by field
sort -count / sort +count       # Descending / ascending sort
eval newfield = expression      # Create/modify a field
where condition                 # Filter after stats/eval (search filters before)
timechart span=1h count by field   # Time-bucketed chart
transaction field maxspan=5m    # Group related events into a session/transaction
```

## Useful security search patterns

```
# Failed logon spike by user (Windows Security log)
index=wineventlog EventCode=4625
| stats count by Account_Name, src_ip
| where count > 10

# First-time-seen process (requires a lookup/summary of historical processes)
index=sysmon EventCode=1
| stats earliest(_time) as first_seen by Image
| where first_seen > relative_time(now(), "-1d")

# Beaconing detection — look for regular interval outbound connections
index=firewall action=allowed
| stats count, values(dest_port) by src_ip, dest_ip
| where count > 50

# DNS queries to newly seen domains
index=dns
| stats count by query
| sort -count
```

## Time modifiers

```
earliest=-24h latest=now
earliest=-7d@d latest=@d        # Full previous 7 days, day-aligned
```

## Field extraction & lookups

```
| rex field=_raw "user=(?<username>\w+)"     # Regex field extraction
| lookup threat_intel_iocs ip AS src_ip OUTPUT verdict   # Enrich with a lookup table
```

## Notes

- `search` filters events (before `|`); commands after `|` transform the already-filtered result set — filter as early as possible for performance.
- Use `index=` and `sourcetype=` filters first — they're the most efficient narrowing step before free-text search.
- Saved searches + alerting thresholds are how ad-hoc hunts become standing detections.
