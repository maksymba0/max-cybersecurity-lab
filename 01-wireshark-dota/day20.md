# Date: 07/04/2026

**Objective:** Analyze public PCAP for signs of malware infection (https://www.malware-traffic-analysis.net/2023/06/05/)

**Findings:**
- Victim IP: 10.6.5.101
- Malware family: Formbook (XLoader)
- First suspicious DNS query: www.guninfo.guru
- Malware domains observed: hfaer4.xyz, cyg8wm3zfb.xyz, 6o20r.beauty, mimi2023.monster
- Malware communicates over HTTP with random-looking domains
- No file download observed in this capture (likely staged via other means)

## What I Learned

- Random-looking `.xyz`, `.beauty`, `.monster` domains are often malicious
- DNS filtering can block many malware families
- HTTP traffic reveals malware C2 communication even without file download

### Useful Filters for Malware Analysis

| Filter | Purpose |
|--------|---------|
| `dns and ip.src == [victim_IP]` | See all domains the victim queried |
| `http and ip.src == [victim_IP]` | View all HTTP traffic from the victim |
| `http.request.method == POST` | Find potential data exfiltration |
| `frame.len > 5000` | Detect large transfers (files, exfil) |
| `tcp.flags.syn == 1 and tcp.flags.ack == 0` | Detect port scans |
| `tcp.port == 4444` | Check for non-standard C2 ports |

<img width="1866" height="401" alt="Screenshot 2026-04-07 155425" src="https://github.com/user-attachments/assets/f7805b6b-14cd-43fd-a5d8-89f629d82c2a" />
<img width="1802" height="543" alt="Screenshot 2026-04-07 160002" src="https://github.com/user-attachments/assets/1079dab5-5c57-4707-b203-d83c5438ecd6" />
<img width="1871" height="828" alt="Screenshot 2026-04-07 160528" src="https://github.com/user-attachments/assets/1a14f945-3c89-4d31-9e02-dae257883abd" />
<img width="1875" height="850" alt="Screenshot 2026-04-07 161344" src="https://github.com/user-attachments/assets/b1971d6f-0a61-495f-9797-89b23d9a0d28" />
<img width="1873" height="839" alt="Screenshot 2026-04-07 161449" src="https://github.com/user-attachments/assets/834cde03-7ce1-486c-bc8b-7c78a00aca59" />
