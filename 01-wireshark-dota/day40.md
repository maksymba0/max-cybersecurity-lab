# Incident Investigation Report: Unauthorized Root Access

**Date:** 2026-05-20  
**Analyst:** maksymba0  
**Severity:** Critical

---

## Executive Summary

An unauthorized user gained root access to the target system via SSH brute force. The attacker created a file indicating successful compromise. The incident was detected through an anomalous file on the desktop.

---

## Detection

**Initial indicator:** Unknown folder `leaked` on `/home/maxubuntu/Desktop/` containing `readthis.txt` with text: *"you have been pwned"*
<img width="1358" height="707" alt="Screenshot 2026-05-20 223210" src="https://github.com/user-attachments/assets/c1e97490-a41d-4d11-9a3c-b2958caed2f6" />

**Immediate actions:**
1. Network isolation — `sudo ip link set enp0s3 down` and `enp0s8 down`
2. Evidence preservation — copied SSH logs, auth logs, and command history
<img width="816" height="136" alt="Screenshot 2026-05-20 223403" src="https://github.com/user-attachments/assets/054edbc4-d010-4985-a2e0-9f5b4f1092d8" />

---

## Timeline of Events

| Time (UTC) | Event | Evidence |
|------------|-------|----------|
| 19:09 | Nmap scan detected | SSH logs show connection attempts |
| 20:18 | Hydra brute force | Failed password entries in auth.log |
| 20:19 | Successful root login | `Accepted password for root from 192.168.100.20` |
| --:-- | Attacker created file | `/root/.bash_history` shows `mkdir` and `echo` commands |
| --:-- | Incident detected | User found `readthis.txt` on desktop |

<img width="652" height="552" alt="Screenshot 2026-05-20 224814" src="https://github.com/user-attachments/assets/02991850-5062-4e94-8665-bf12bccafe42" />

<img width="1241" height="472" alt="Screenshot 2026-05-20 224717" src="https://github.com/user-attachments/assets/414e600a-dde2-4da6-90ae-0c71ed126fae" />

---

## Indicators of Compromise (IoCs)

| Type | Value |
|------|-------|
| Attacker IP | 192.168.100.20 |
| Compromised account | root |
| Weak password | ubuntumax |
| File created | `/home/maxubuntu/Desktop/leaked/readthis.txt` |
| Attacker commands | `mkdir`, `echo`, `ls` (from .bash_history) |

---

## Evidence Collected

| Artifact | Path | What It Shows |
|----------|------|----------------|
| SSH logs | `journalctl -u ssh` | Failed and successful logins |
| Auth logs | `/var/log/auth.log` | Authentication attempts |
| Bash history | `/root/.bash_history` | Attacker's commands |
| Intrusion file | `~/Desktop/leaked/readthis.txt` | Proof of compromise |

---
