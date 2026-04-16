# Date: 16/04/2026

## SOC Analysis: SSH Brute Force Detection

### Scenario
A SIEM alert showed four logs from a Linux server (`web01`):
```
2026-04-16 20:45:01 web01 sshd[1234]: Failed password for root from 192.168.1.100 port 45678
2026-04-16 20:45:03 web01 sshd[1234]: Failed password for root from 192.168.1.100 port 45678
2026-04-16 20:45:05 web01 sshd[1234]: Failed password for root from 192.168.1.100 port 45678
2026-04-16 20:45:30 web01 sshd[1234]: Accepted password for root from 192.168.1.100 port 45678
```
### What Happened

- An attacker at IP `192.168.1.100` attempted to log in as `root` via SSH (port 22)
- Three failed attempts in quick succession (2-second gaps)
- On the fourth attempt, the password was accepted
- Total time from first failure to success: 29 seconds

**Conclusion:** Successful brute force attack on the root account.
---
### Is This Suspicious?

**Yes — 100%. Keep in mind:**  
- `root` is the superuser account; normal employees should never log in as root directly
- The timing pattern (fast failures, 2-second gaps) is consistent with automated brute force tools
- A legitimate user would have:
  - Used their own account (not root)
  - Taken more time between attempts
  - Succeeded on the first or second try

---

### Immediate Response (What a SOC Analyst Would Do)

| Priority | Action |
|----------|--------|
| 1 | **Contain** — Block IP `192.168.1.100` at the firewall immediately |
| 2 | **Investigate** — Check logs for post-exploitation activity (commands run, files accessed) |
| 3 | **Remediate** — Change root password, disable root SSH login |
| 4 | **Verify** — Check for persistence (cron jobs, SSH keys, backdoors) |

---

### Prevention: Hardening SSH

On my Target VM, I checked the SSH server configuration:

```bash
sudo cat /etc/ssh/sshd_config | grep PermitRootLogin
```
<img width="807" height="587" alt="Screenshot 2026-04-16 235140" src="https://github.com/user-attachments/assets/9e5f0d43-953e-450b-bd79-e6f90ca58f10" />

Recommended hardening for any SSH server:

|Setting|Value|Why|
|-------|---------|
|PermitRootLogin	|no or prohibit-password|	Prevent password brute force on root|
|PasswordAuthentication|	no	|Require SSH keys for all users|
|AllowUsers|	Specific usernames|	Limit who can log in|

**What I Learned**:
- SSH brute force attacks follow a clear pattern: fast failures → success
- Timing between attempts helps distinguish automated attacks from human mistakes
- Root account is a high-value target; direct root login should be disabled
- Prevention (hardening SSH) is more effective than detection
- A secure configuration would have stopped this attack entirely
