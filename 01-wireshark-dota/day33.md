# Date: 22/04/2026

## SOC Interview Question: 500 Failed SSH Logins → Success

### Question

> *"You see 500 failed SSH logins in 5 minutes from the same IP, then a successful login. What do you do?"*

### My Answer

1. **Contain** — Block the IP at the firewall immediately. Isolate the host if critical.
2. **Investigate** — Check logs (journalctl, auth.log, SIEM). Look for commands run, files accessed, persistence installed.
3. **Remediate** — Change password, remove attacker's SSH keys, check for lateral movement.
4. **Prevent** — Disable root SSH login, enforce key-only auth, install fail2ban.

### What I Learned

- Speed matters — block first, ask questions later
- After a compromise, assume the attacker left backdoors (cron, SSH keys, services)
- Prevention (fail2ban, key auth) stops brute force before it succeeds
