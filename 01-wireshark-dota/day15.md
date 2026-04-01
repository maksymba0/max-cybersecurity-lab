# Date: 01/04/2026

**Objective:** Analyze a port scan capture to understand TCP handshake patterns.

**Evidence:**
- Wireshark capture of `nmap -A` from Attacker (192.168.100.20) to Target (192.168.100.10)
- Filter used: `tcp.flags.syn == 1`

**Observation of Open Port (80):**

| # | Time | Source | Destination | Flags | Details |
|---|------|--------|-------------|-------|---------|
| 1 | 0.000000000 | 192.168.100.20 | 192.168.100.10 | SYN | Attacker initiates connection (port 80) — Seq=0, Win=64240 |
| 2 | 0.000060846 | 192.168.100.10 | 192.168.100.20 | SYN-ACK | Target responds — port is OPEN. Seq=0, Ack=1, Win=65160 |

The extremely short time between packets (0.00006 seconds) indicates both VMs are on the same virtual network with low latency.

**For closed ports**, the capture showed:
- SYN from Attacker → RST-ACK from Target (no SYN-ACK)
- Target immediately rejects connection attempts on ports that are not listening

**What I Learned:**
- SYN → SYN-ACK = open port (e.g., port 80 with nginx)
- SYN → RST-ACK = closed port (no service listening)
- Port scans leave clear, identifiable patterns in network captures

**Screenshots:**
<img width="1580" height="856" alt="Screenshot 2026-04-01 234639" src="https://github.com/user-attachments/assets/09880cf8-421a-429a-ad43-e43324a5ac47" />
<img width="842" height="384" alt="Screenshot 2026-04-01 235515" src="https://github.com/user-attachments/assets/8e4efb28-ca12-4eff-ac01-757d16d06e94" />


**Why This Matters:**
- Reconnaissance like this often precedes real attacks
- Knowing what normal vs scan traffic looks like helps in detection
- Analysts use these patterns to write detection rules and understand attacker behavior
