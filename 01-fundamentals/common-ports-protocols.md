# Common Ports & Protocols Reference

| Port | Protocol | Service | Notes |
|---|---|---|---|
| 20/21 | TCP | FTP (data/control) | Unencrypted — prefer SFTP/FTPS |
| 22 | TCP | SSH | Remote admin, tunneling, SFTP |
| 23 | TCP | Telnet | Unencrypted — flag if seen internally |
| 25 | TCP | SMTP | Mail relay — watch for open relays |
| 53 | TCP/UDP | DNS | UDP for queries, TCP for zone transfers/large responses |
| 67/68 | UDP | DHCP | Server/client |
| 69 | UDP | TFTP | No auth — often used in network device provisioning |
| 80 | TCP | HTTP | Unencrypted web |
| 88 | TCP/UDP | Kerberos | AD authentication — key target in AD attacks (Kerberoasting, Golden Ticket) |
| 110 | TCP | POP3 | Mail retrieval |
| 111 | TCP/UDP | RPCbind | Legacy, often flagged in scans |
| 123 | UDP | NTP | Time sync — can be abused for DDoS amplification |
| 135 | TCP | MS RPC | Windows RPC endpoint mapper |
| 137-139 | TCP/UDP | NetBIOS | Legacy Windows networking |
| 143 | TCP | IMAP | Mail retrieval |
| 161/162 | UDP | SNMP | Device monitoring — default community strings are a common finding |
| 389 | TCP/UDP | LDAP | Directory services |
| 443 | TCP | HTTPS | TLS-encrypted web |
| 445 | TCP | SMB | File sharing — EternalBlue (MS17-010), ransomware propagation vector |
| 464 | TCP/UDP | Kerberos password change | |
| 514 | UDP | Syslog | Centralized logging |
| 587 | TCP | SMTP (submission) | Authenticated mail submission, often with STARTTLS |
| 636 | TCP | LDAPS | LDAP over TLS |
| 993 | TCP | IMAPS | IMAP over TLS |
| 995 | TCP | POP3S | POP3 over TLS |
| 1433 | TCP | MS SQL Server | |
| 1521 | TCP | Oracle DB | |
| 3306 | TCP | MySQL | |
| 3389 | TCP | RDP | Frequent brute-force/ransomware entry point — should rarely be internet-facing |
| 5060/5061 | TCP/UDP | SIP | VoIP signaling |
| 5432 | TCP | PostgreSQL | |
| 5900 | TCP | VNC | Remote desktop — often weak/no auth |
| 8080/8443 | TCP | HTTP(S) alt | Common proxy/admin panel ports |
| 9200 | TCP | Elasticsearch | Frequently exposed without auth — common misconfig finding |

## Well-known port ranges

- **0–1023**: Well-known ports (require root/admin to bind)
- **1024–49151**: Registered ports
- **49152–65535**: Dynamic/ephemeral ports (client-side connections)

## Quick triage tips

- Unexpected 445/3389/5900 open to the internet → high-priority finding
- SNMP with default community string `public`/`private` → credential/config finding
- Plaintext protocols (21, 23, 80, 110, 143) carrying auth → recommend encrypted equivalents
- High ephemeral-port outbound connections to a single external IP over time → possible C2 beaconing, check interval regularity
