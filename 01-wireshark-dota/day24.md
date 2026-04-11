# Date: 11/04/2026

## SIEM Alert Analysis #2

**Alert:** Multiple failed logins → successful login on admin account

**Log snippet:**
```
2026-04-11 02:34:12 src_ip=45.33.22.11 user=admin status=failed
2026-04-11 02:34:15 src_ip=45.33.22.11 user=admin status=failed
2026-04-11 02:34:18 src_ip=45.33.22.11 user=admin status=success
```
**My analysis:**

1. **What happened?**  
   Three login attempts in 6 seconds, two failures then success. The time (2 AM) is unusual for a human user.

2. **Is it suspicious?**  
   Yes. The source IP is external. Direct logins to an admin account from possibly outside the corporate network at 2 AM is a red flag.

3. **What would I check next?**  
   - Search SIEM for other activity from `45.33.22.11`  
   - Check IP reputation on VirusTotal / AbuseIPDB  
   - Verify with the user if they were working at that time  
   - If malicious, block the IP and reset the account credentials

**What I learned:**
- Not every alert is a true positive, but context matters (time, IP reputation, user behavior)
- SOC analysts don't just look at logs — they investigate, validate, and escalate
- External IP + admin account + odd time = high priority investigation
