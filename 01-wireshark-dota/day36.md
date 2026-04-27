**Date:** 2026-04-28  

# IOC Report: Suspicious Internal Scan (SCENARIO)
**Severity:** High

---

## Summary

Internal IP `192.168.1.100` was observed scanning 15 internal servers at 2 AM, followed by an SSH connection to a database server at 3 AM. No scheduled maintenance or legitimate activity was documented for this time.

---

## Impact

- **Lateral movement** — attacker (or compromised host) is probing the internal network
- **Data at risk** — the database server accessed via SSH may contain sensitive information
- **Potential for further compromise** — attacker could steal credentials, install backdoors, or deploy ransomware

---

## Recommended Actions

1. **Contain** — Immediately block IP `192.168.1.100` at the firewall and isolate the host from the network
```bash
# Block IP at firewall (on Linux gateway or target host)
sudo ufw deny from 192.168.1.100

# Isolate host from network (disable interface)
sudo ip link set enp0s3 down

# Or kill active SSH sessions from that IP
sudo pkill -kill -t $(who | grep 192.168.1.100 | awk '{print $2}')
```
2. **Investigate** — Check logs on the database server for commands run, files accessed, or persistence installed (e.g., SSH keys, cron jobs)
```bash
# Check SSH logins from the suspicious IP
sudo journalctl -u ssh | grep 192.168.1.100

# Check commands run after login (if user had bash history)
sudo cat /home/*/.bash_history | grep -v "ls" | sort | uniq

# Check for installed SSH keys (persistence)
sudo cat /home/*/.ssh/authorized_keys

# Check for cron jobs (backdoor)
sudo crontab -l
sudo cat /etc/crontab
```
3. **Remediate** — Scan compromised host for malware, change passwords for any accounts used, and review firewall rules to restrict internal SSH
```bash
# Scan for rootkits (install first: sudo apt install chkrootkit)
sudo chkrootkit

# Check for listening backdoors
sudo ss -tulpn | grep LISTEN

# Disable root SSH login permanently
sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config
sudo systemctl restart ssh

# Limit SSH to specific users only
echo "AllowUsers admin user1" | sudo tee -a /etc/ssh/sshd_config
```

---

## Indicators of Compromise (IoCs)

| Type | Value |
|------|-------|
| Source IP | 192.168.1.100 |
| Time | 2:00 AM (scan), 3:00 AM (SSH to DB) |
| Target | Multiple internal servers, then database server |

---

## What I Learned

- Internal scans are **not** false positives — they indicate lateral movement
- SSH from one internal server to another at odd hours is a red flag
- A good IOC report is short, clear, and actionable: **summary → impact → actions**
