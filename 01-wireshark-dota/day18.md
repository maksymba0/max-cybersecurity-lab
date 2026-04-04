# Date: 04/04/2026

**Objective:** Review logs from previous labs

**FTP Brute Force (from /var/log/syslog):**
- 7-8 failed attempts from 192.168.100.20
- 1 successful login for testuser 
  
**Source:** `/var/log/auth.log`

**Evidence Found:**
- Multiple authentication failures for user `testuser`
- Source IP: `192.168.100.20` (Attacker VM)
- Timestamps show clusters of failures (8 attempts, pause, 8 attempts)
- This pattern matches automated brute force tool (Hydra)

**What I Learned:**
- Logs capture every authentication attempt
- Failed + success = brute force detected
- Even simple services leave forensic evidence 

**Why This Matters:**
- Logs are **primary evidence** in security incidents
- Failed logins + volume + source IP = brute force attack
- Analysts use this data to block IPs, identify compromised accounts
  
<img width="1735" height="918" alt="Screenshot 2026-04-04 202808" src="https://github.com/user-attachments/assets/8ed47457-db2a-49a8-83e2-2c8355118840" />
<img width="1291" height="848" alt="Screenshot 2026-04-04 205224" src="https://github.com/user-attachments/assets/b6c03158-7634-4e22-be62-532d9bbf36a3" />
