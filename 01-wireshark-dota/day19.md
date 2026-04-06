# Date 06/04/2026

**Objective:** Incident Timeline - Simulated Attack
 
**Scenario:** Attacker VM (192.168.100.20) against Target VM (192.168.100.10)

| Time | Event | Evidence Source |
|------|-------|-----------------|
| 17:27 | Port 21, 80, 443 scan detected | Wireshark: SYN packets to multiple ports |
| 17:27-17:34 | FTP brute force started | /var/log/auth.log: repeated failures from 192.168.100.20 |
| 17:30-17:39 | Successful login (if happened) | /var/log/auth.log: "session opened" |
| 17:27-17:34| C2 beacon (simulated) | Wireshark: "HELLO C2 SERVER" to port 4444 |

**What I Learned:**
- Attacks follow phases: recon → exploit → persistence
- Logs + network capture together tell the full story
- Timestamps are critical for incident response
<img width="1733" height="258" alt="Screenshot 2026-04-06 201540" src="https://github.com/user-attachments/assets/2cff5948-906a-4c50-9dc4-a24eeec92e01" />
<img width="1858" height="612" alt="Screenshot 2026-04-06 201853" src="https://github.com/user-attachments/assets/c846bc23-3c8d-45df-8f36-ff8eaa3fa03a" />
<img width="1769" height="216" alt="Screenshot 2026-04-06 202101" src="https://github.com/user-attachments/assets/5b45c3e8-9b39-48d5-bdc4-a8dcdb698808" />
<img width="1785" height="391" alt="Screenshot 2026-04-06 202928" src="https://github.com/user-attachments/assets/7bae9e80-bf5a-4dbd-a919-3f76d1f6de8a" />
<img width="1864" height="662" alt="Screenshot 2026-04-06 204743" src="https://github.com/user-attachments/assets/6305f2ed-bf27-4fa4-aa54-37bb7ce6c28f" />
