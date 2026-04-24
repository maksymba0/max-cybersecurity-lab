# Date: 24/04/2026

## Detecting Lateral Movement: The 3 AM SSH Connection

### Scenario

You're a SOC analyst reviewing internal network logs. Your SIEM shows an alert:

> *Server A (10.0.10.25) established an SSH connection to Server B (10.0.10.45) at 3:14 AM.*

Neither server is used by human operators at that hour. No scheduled maintenance was planned.

---

### What Is Lateral Movement?

Once an attacker compromises one machine, they "move laterally" across the network — hopping from server to server — to reach high-value targets (databases, domain controllers, backups).

Lateral movement often uses legitimate tools (SSH, RDP, SMB) to blend in with normal traffic.

---

### Why 3 AM SSH Is Suspicious

| Normal | Suspicious |
|--------|------------|
| Business hours (9 AM – 5 PM) | 3 AM (off-hours) |
| Known user or service account | Unknown or generic account |
| Recurring pattern | One-time connection |
| Documented maintenance | No change request or ticket |

If there's no business justification, an unexpected internal SSH connection is a red flag.

---

### Investigation Steps

1. **Verify scheduled tasks** — Was there a cron job or systemd timer on Server A that triggered SSH at 3 AM?

2. **Check authentication logs** — Who initiated the connection? Was it a user account or a service account?
```bash
   sudo journalctl -u ssh --since "03:00" --until "04:00" | grep "Accepted"
```  
3. **Check command history** — What commands were run on Server B after the connection?
```bash
sudo cat /home/user/.bash_history
```
4. **Check for persistence** — Did the attacker add SSH keys or cron jobs?
```bash
sudo cat ~/.ssh/authorized_keys
sudo crontab -l
```
5. **Correlate with other alerts** — Did Server B connect to other internal hosts afterward?
```# Check SSH logs for a specific time window
sudo journalctl -u ssh --since "03:00" --until "04:00"

# Check for unexpected authorized_keys
sudo cat ~/.ssh/authorized_keys

# Check for cron jobs running as user
sudo crontab -l

# List active SSH sessions (if still ongoing)
ss -tulpn | grep :22
```

**What I learned**:
- Lateral movement -	Attackers move from one compromised host to another using legitimate tools (SSH, RDP, SMB)
- Baselining	- You must know what's normal (timing, accounts, frequency) to spot what's abnormal
- Off-hours activity	- 3 AM connections with no maintenance ticket = investigate immediately
- Detection	Monitor - internal SSH, RDP, and SMB traffic — not just perimeter logs
- Response -	Isolate both hosts, check for persistence, and trace the attack path
