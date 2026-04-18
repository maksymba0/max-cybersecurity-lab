# Date: 18/04/2026

## SOC Investigation: SSH Brute Force Detection

### Scenario

As a SOC analyst, I investigated an alert: *"Multiple failed logins → success from the same IP on a Linux server."*

I used live SSH attempts from Attacker to Target, then analyzed logs on Target.
<img width="963" height="220" alt="Screenshot 2026-04-18 210200" src="https://github.com/user-attachments/assets/f47b218c-2558-487f-bd9c-b575feb8e767" />

---

### Commands Used

| Purpose | Command |
|---------|---------|
| Check for failed login attempts | `sudo strings /var/log/auth.log \| grep -i "failed"` |
| Force SSH to use specific interface | `ssh -b 192.168.100.20 root@192.168.100.10` |
| Count failures from a specific IP | `sudo strings /var/log/auth.log \| grep -i "failed" \| grep "192.168.100.20" \| wc -l` |
| View SSH logs from last hour (systemd) | `sudo journalctl -u ssh --since "1 hour ago"` |

<img width="1591" height="406" alt="Screenshot 2026-04-18 210531" src="https://github.com/user-attachments/assets/736e82c6-1649-4595-970a-79004563d6e4" />
<img width="1594" height="220" alt="Screenshot 2026-04-18 211313" src="https://github.com/user-attachments/assets/14b7c9b0-2abe-4109-8e31-4ea236e92785" />

---

### What I Observed

- Failed SSH attempts were logged in `/var/log/auth.log`
- Each entry included: timestamp, user (`root`), source IP, and port
- Root password login was disabled (expected on Ubuntu) — no successful login occurred
- `journalctl -u ssh` showed only SSH service logs, which is cleaner than grepping auth.log

---

### Key Takeaways

| Topic | What I Learned |
|-------|----------------|
| **Log source** | SSH failures are in `/var/log/auth.log` |
| **Unit filtering** | `journalctl -u ssh` filters logs by service unit |
| **Counting failures** | `grep` + `wc -l` gives attack volume |
| **SSH binding** | `-b` option binds to a specific interface IP |
| **Response priority** | Contain (block IP) → Investigate → Remediate |

---

### What Is a "Unit" in Linux?

A **unit** is a resource managed by `systemd` (service, timer, socket, etc.).  
`-u ssh` refers to `ssh.service` — the SSH daemon.

Example:  
`sudo journalctl -u ssh` → shows logs only for SSH, not the whole system.

---

### Detection Rule (Mental Model)
```
IF (failed_logins > 10 in 60 seconds from same IP) AND (no success)
THEN alert: "Possible brute force in progress"

IF (failed_logins > 3) AND (success within 30 seconds)
THEN alert: "CRITICAL: Account compromised"
```
---

### What I Learned

- `journalctl -u` is faster and cleaner than grepping raw log files
- Counting failures helps assess attack severity
- Root password login is disabled by default on Ubuntu (`prohibit-password`)
- In a real incident, the first step is to block the attacking IP

---

### References

- `man journalctl` — systemd log viewer
- `man ssh` — OpenSSH client
- `/var/log/auth.log` — authentication logs
