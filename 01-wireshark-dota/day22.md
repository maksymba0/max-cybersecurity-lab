# Date: 09/04/2026

**Objective**: Log Analysis Exercise

**Objective:** Check `/var/log/auth.log` for signs of brute force or unauthorized access.

**Commands used:**
```bash
sudo strings /var/log/auth.log | grep -i "failed"
sudo strings /var/log/auth.log | grep "Accepted password"
```

**Findings**:
- No SSH failed password attempts in the last 24 hours
- No successful logins from unknown IPs
- Only unrelated failures (Bluetooth service, GDM conversation)
-
- <img width="1813" height="845" alt="Screenshot 2026-04-09 214202" src="https://github.com/user-attachments/assets/5bfa4dad-459c-4119-baa9-11ee76bc285f" />
