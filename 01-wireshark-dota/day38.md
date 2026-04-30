# Date: 30/04/2026

## Theory: MITRE ATT&CK & Linux Forensic Artifacts

### Part 1: MITRE ATT&CK Framework

**What Is It?**  
A knowledge base of attacker behaviors mapped to **tactics** (why) and **techniques** (how). Every SOC uses it to standardize detection and reporting.

**Core Structure:**

| Level | Example |
|-------|---------|
| Tactic (WHY) | TA0003 — Persistence |
| Technique (HOW) | T1098 — Account Manipulation (add SSH key) |
| Procedure (SPECIFIC) | `echo "ssh-rsa ..." >> ~/.ssh/authorized_keys` |

**Tactics You Already Encountered:**

| Tactic | ID | Your Lab Experience |
|--------|-----|---------------------|
| Reconnaissance | TA0043 | nmap scans, ping sweeps |
| Initial Access | TA0001 | SSH brute force (Hydra) |
| Execution | TA0002 | Running commands after SSH login |
| Persistence | TA0003 | Adding SSH keys, cron jobs |
| Lateral Movement | TA0008 | SSH from one VM to another |
| Exfiltration | TA0010 | C2 beacon simulation |

**How SOC Analysts Use MITRE:**

1. **Detect** — Map alert to technique (e.g., "SSH bruteforce = T1110")
2. **Investigate** — Look for related techniques (T1078, T1059)
3. **Report** — Use MITRE IDs in findings (managers love standardized language)

---

### Part 2: Linux Forensic Artifacts (What Attackers Leave Behind)

When a Linux host is compromised, attackers always leave traces. Here's where to look:

#### 1. SSH Keys (Persistence)

**Path:** `~/.ssh/authorized_keys`

**Check command:**
```bash
cat ~/.ssh/authorized_keys
sudo find /home -name "authorized_keys" -exec cat {} \;
```
### 2. Cron Jobs (Scheduled Persistence)

**Check commands**:
```bash
crontab -l                      # current user
sudo crontab -l                 # root
cat /etc/crontab                # system cron
ls -la /etc/cron.hourly/        # hourly scripts
```
**Why it matters**: Attackers schedule malicious scripts to run every hour, day, or week.
### 3. Command History

**Path**: ~/.bash_history
**Check command**:
cat ~/.bash_history | grep -E "wget|curl|chmod|nc|python|/tmp"

**Why it matters**: Attackers often don't wipe history. You can see exactly what they downloaded and executed.

### 4. Authentication Logs
**Path**: /var/log/auth.log
**Check commands**:
```bash
bash
sudo journalctl -u ssh | grep "Accepted"
sudo strings /var/log/auth.log | grep "Failed password"
```
**Why it matters**: Shows who logged in, from where, and when.

### 5. System Logs
**Path**: /var/log/syslog

Why it matters: Contains system-wide events — service starts, network changes, process launches.

Persistence Detection Workflow
After containing an incident (block IP, change passwords), always check:

| Priority | Artifact | Command |
|----------|----------|---------|
| 1 | SSH keys | `cat ~/.ssh/authorized_keys` |
| 2 | Cron jobs | `crontab -l` |
| 3 | Command history | `cat ~/.bash_history` |
| 4 | Auth logs | `sudo journalctl -u ssh \| grep "Accepted"` |

**What I Learned**:
 
MITRE ATT&CK	-Common language for describing attacker behavior
TA0003 (Persistence) -	Attackers want to maintain access — via SSH keys or cron
SSH keys -	Always check authorized_keys after a compromise
Cron jobs	- Attackers schedule backdoors to survive reboots
Forensic artifacts -	Attackers leave traces — you just need to know where to look

