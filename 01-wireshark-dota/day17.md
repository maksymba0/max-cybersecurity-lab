# 03/04/2026 - Malware Traffic Analysis

**Objective:** Analyze simulated malware network behavior

**Observation:**
- Attacker (192.168.100.20) initiated connection to Target (192.168.100.10:4444)
- Payload contained plain text: "HELLO C2 SERVER"
- No encryption so message fully readable in packet capture

**Why This Is Suspicious:**
- Outbound connection to non-standard port (4444)
- Small, irregular packet size (16 bytes) - possible beaconing
- Plain text command - easily detectable by IDS/IPS

**Detection Methods:**
- Signature-based: rule looking for "HELLO C2" string
- Behavioral: unusual connection from host to new IP on odd port
- Statistical: packet size and timing anomalies

**What I Learned:**
- Even simple malware leaves detectable network traces
- Encrypted C2 traffic is harder to detect but still leaves metadata (IPs, ports, timing)
- Analysts combine multiple detection methods to catch threats

<img width="1008" height="270" alt="Screenshot 2026-04-03 191845" src="https://github.com/user-attachments/assets/f0512a8d-2959-486f-8dda-9bd234717f1a" />
<img width="865" height="267" alt="Screenshot 2026-04-03 191852" src="https://github.com/user-attachments/assets/0d09e49c-6c90-49fd-a106-4b64527d93f6" /> 
<img width="1841" height="741" alt="Screenshot 2026-04-03 192124" src="https://github.com/user-attachments/assets/13f13ec8-ee7a-4c57-b6a9-061ac2bfdb16" />
