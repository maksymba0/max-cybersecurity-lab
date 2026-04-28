# Date: 28/04/2026

## Lateral Movement Detection: Internal IP Scanner

### Overview

I built and deployed a bash script that monitors SSH logs for **internal IPs** (192.168.x.x, 10.x.x.x) with excessive failed login attempts. The script runs hourly via cron and logs alerts to a dedicated file.

This detects **lateral movement** — an attacker trying to move from one compromised internal host to another.

---

### The Script

**Location:** `~/Desktop/scripts/check_internal.sh`

```bash
#!/bin/bash

# Create log directory if missing
sudo mkdir -p /var/log/scans

LOG_FILE="/var/log/scans/internal_alerts.log"
IP_LIMIT=5

echo "======== INTERNAL IP SCAN REPORT FROM $(date) ========" >> "$LOG_FILE"
sudo journalctl -u ssh --since "24 hours ago" | grep -i "failed password" | awk '{print $11}' | sort | uniq -c | sort -rn | while read count ip; do
    if [ "$count" -gt "$IP_LIMIT" ]; then
        echo "⚠️  $ip had $count failed attempts today" >> "$LOG_FILE"
    fi
done
echo "=====================================================" >> "$LOG_FILE"
```

<img width="1026" height="369" alt="Screenshot 2026-04-28 123420" src="https://github.com/user-attachments/assets/60581fce-52d4-4ff4-8050-bf78ca68c3d4" />
<img width="999" height="160" alt="Screenshot 2026-04-28 123326" src="https://github.com/user-attachments/assets/158649da-cc46-4f2f-9e41-1f35dc310866" />
<img width="1002" height="333" alt="Screenshot 2026-04-28 123309" src="https://github.com/user-attachments/assets/ac8ce63d-d1bb-4ef2-a452-2631681807ea" />

**What I learned**:
- Lateral movement -	Attackers move from one internal host to another using SSH, RDP, or SMB
- Internal IP ranges	- 192.168.x.x, 10.x.x.x — monitoring these catches internal scans
- while read count ip	- Reads two variables from uniq -c output (count and value)
- Cron automation	- Scripts run without manual intervention — essential for detection
- Log directories	- Bash creates files automatically, but not parent directories
- Threshold tuning	- 5 failures in 24 hours is reasonable for a lab; adjust for production
