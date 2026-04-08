# Date: 08/04/2026

## Objective
Analyze public PCAP from malware-traffic-analysis.net (2023-06-05) to identify signs of Formbook (XLoader) infection.

## Findings

- **Victim IP:** 10.6.5.101
- **Malware family:** Formbook (XLoader)
- **First suspicious DNS query:** `www.guninfo.guru`
- **Malware domains observed:**  
  `hfaer4.xyz`, `cyg8wm3zfb.xyz`, `6o20r.beauty`, `mimi2023.monster`
- **C2 protocol:** HTTP (port 80) with GET and POST requests to `/he2a/`
- **No file download observed** in this capture (exfiltration likely occurred via POST)

## What I Learned

- Random-looking `.xyz`, `.beauty`, `.monster` domains are often malicious
- DNS filtering can block many malware families
- HTTP traffic reveals malware C2 communication even without file download
- Formbook uses a single URI path (`/he2a/`) for both beaconing (GET) and data exfiltration (POST)

## Useful Wireshark Filters

| Filter | Purpose |
|--------|---------|
| `dns and ip.src == 10.6.5.101` | See all domains the victim queried |
| `http and ip.src == 10.6.5.101` | View all HTTP traffic from the victim |
| `http.request.method == POST` | Find potential data exfiltration |
| `frame.len > 5000` | Detect large transfers (files, exfil) |
| `http.request.uri contains "/he2a/"` | Identify Formbook C2 traffic |

## Indicators of Compromise (IoCs)

| Type | Value |
|------|-------|
| Victim IP | `10.6.5.101` |
| Domains | `www.guninfo.guru`, `www.hfaer4.xyz`, `www.cyg8wm3zfb.xyz`, `mimi2023.monster`, `www.6o20r.beauty` |
| C2 URI | `/he2a/` |
| C2 Port | `80` (HTTP) |

## References

- [Malware Traffic Analysis - 30 Days of Formbook: Day 1](https://www.malware-traffic-analysis.net/2023/06/05/)
