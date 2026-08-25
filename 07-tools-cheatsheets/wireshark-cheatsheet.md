# Wireshark Cheatsheet

## Display filter basics

```
ip.addr == 10.0.0.5              # Traffic to/from an IP
ip.src == 10.0.0.5                # Traffic from a source IP
ip.dst == 10.0.0.5                # Traffic to a destination IP
tcp.port == 443                   # TCP traffic on a port
tcp.flags.syn == 1 && tcp.flags.ack == 0   # SYN packets only (scan/connection attempts)
http.request                      # HTTP requests only
http.response.code == 200         # Specific HTTP status
dns.qry.name contains "example"   # DNS queries matching a string
tls.handshake.type == 1           # TLS Client Hello (see SNI in cleartext)
frame contains "password"         # Raw payload search (unencrypted traffic only)
```

## Combining filters

```
ip.addr == 10.0.0.5 && tcp.port == 80
!(ip.addr == 10.0.0.1)            # Exclude an IP
ip.addr == 10.0.0.5 || ip.addr == 10.0.0.6
```

## Useful workflows

- **Follow TCP/HTTP/TLS Stream** (right-click a packet) — reconstructs the full conversation, fastest way to read a session
- **Statistics → Conversations** — see all host pairs and traffic volume, good for spotting large/unusual transfers
- **Statistics → Protocol Hierarchy** — quick overview of what protocols are present in a capture
- **Statistics → Endpoints** — list of all hosts seen, sortable by bytes/packets
- **File → Export Objects → HTTP** — pull out files transferred over HTTP for further analysis

## Investigating specific scenarios

- **Suspected port scan**: filter `tcp.flags.syn==1 && tcp.flags.ack==0`, check Conversations for one source hitting many ports/hosts in a short window
- **Suspected data exfiltration**: Statistics → Conversations, sort by bytes, look for large outbound transfers to unfamiliar external IPs
- **Suspected C2 beaconing**: look for regular, periodic small requests to the same external host (check timestamps for consistent intervals)
- **Credential capture check (unencrypted only)**: `http.request.method == "POST"` combined with Follow HTTP Stream to inspect form data

## Command-line companion: tshark

```
tshark -r capture.pcap -Y "http.request" -T fields -e ip.src -e http.host -e http.request.uri
tshark -r capture.pcap -q -z conv,ip     # IP conversation summary
```

## Notes

- Capturing traffic you don't have authorization to capture may be illegal — only capture on networks/systems you own or are authorized to monitor.
- TLS-encrypted traffic hides payload content by design; you can still see SNI, cert details, and metadata (size/timing) without decryption.
