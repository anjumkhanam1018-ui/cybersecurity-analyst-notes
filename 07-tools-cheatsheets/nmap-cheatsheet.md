# Nmap Cheatsheet

## Scan types

```
nmap -sS <target>        # SYN scan (stealthy, needs root, default for privileged users)
nmap -sT <target>        # TCP connect scan (full handshake, no root needed)
nmap -sU <target>        # UDP scan (slow, often needed for DNS/SNMP/DHCP services)
nmap -sV <target>        # Service/version detection
nmap -O <target>         # OS detection
nmap -sn <target>        # Host discovery only (ping scan, no port scan)
nmap -sC <target>        # Run default NSE scripts
nmap -A <target>         # Aggressive: OS detect + version + scripts + traceroute
```

## Port selection

```
nmap -p 80,443 <target>       # Specific ports
nmap -p 1-1000 <target>       # Range
nmap -p- <target>             # All 65535 ports
nmap -F <target>              # Fast scan (top 100 ports)
```

## Timing & performance

```
nmap -T0 .. -T5 <target>      # Paranoid (0) to Insane (5); T3 is default, T4 common for faster scans
nmap --min-rate 1000 <target> # Minimum packets per second
```

## Output

```
nmap -oN scan.txt <target>    # Normal output
nmap -oX scan.xml <target>    # XML output
nmap -oA basename <target>    # All formats at once
```

## NSE scripts (Nmap Scripting Engine)

```
nmap --script vuln <target>        # Vulnerability-focused scripts
nmap --script default <target>     # Default safe scripts
nmap --script=smb-vuln* <target>   # Wildcard match by category
nmap --script-help <script-name>   # Learn what a script does before running it
```

## Practical examples

```
# Quick host discovery on a subnet
nmap -sn 10.0.0.0/24

# Full TCP port scan with version detection, output to all formats
nmap -sS -sV -p- -oA fullscan 10.0.0.5

# Check a host for common SMB vulnerabilities (e.g., EternalBlue)
nmap --script smb-vuln-ms17-010 -p 445 10.0.0.5
```

## Notes

- Always confirm you have authorization before scanning any network you don't own/manage.
- `-sS` requires raw socket privileges (root/admin); falls back to `-sT` otherwise.
- Firewalls/IDS may drop or rate-limit scans — a "filtered" state means no response, not necessarily closed.
