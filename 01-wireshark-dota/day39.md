# Date: 03/05/2026

## Three Pillars of SOC Analysis: Detection, Hunting, Baseline

### 1. Detection Engineering (1 May)

**Concept:** Turn raw logs into alerts using thresholds + logic.

**Example:** Alert when root SSH failures exceed 3 in 1 minute.

```bash
COUNT=$(sudo journalctl -u ssh --since "1 minute ago" | grep "Failed password for root" | wc -l)
if [ "$COUNT" -gt 3 ]; then
    echo "ALERT: Root brute force detected!"
fi
```

**Per-IP version (more precise):**
```bash
sudo journalctl -u ssh --since "1 minute ago" | grep "Failed password" | awk '{print $11}' | sort | uniq -c | while read count ip; do
    if [ "$count" -gt 3 ]; then
        echo "ALERT: $ip had $count failures in 1 minute"
    fi
done
```

**Key takeaway:** Detection = threshold + command + action.

---

### 2. Threat Hunting (2 May)

**Concept:** Proactively search for suspicious patterns instead of waiting for alerts.

**Example:** Find all successful SSH logins from **external IPs** (not local) in the last 7 days.

```bash
sudo journalctl -u ssh --since "7 days ago" | grep "Accepted" | grep -v "192.168." | grep -v "10\."
```

**What it does:**
- `grep -v` excludes internal IP ranges (`192.168.x.x`, `10.x.x.x`)
- Only external (internet) IPs remain
- Any external login to a lab VM is suspicious

**Key takeaway:** Hunting is asking specific questions of your data.

---

### 3. Security Baseline (3 May)

**Concept:** Establish what "normal" looks like so you can spot "abnormal."

**Baseline for my lab:**

| Activity | Normal Pattern |
|----------|----------------|
| SSH logins | Only from `192.168.100.20` (Attacker VM) |
| Login time | 9:00 – 17:00 (working hours) |
| Failed attempts | 0–3 per day (typos) |
| Internal IP ranges | `192.168.x.x`, `10.x.x.x` |

**Why `10.x.x.x` is internal:** VirtualBox NAT Network often uses `10.0.2.0/24` or `10.0.3.0/24`. These are still local to your host, not the public internet.

**Example abnormal activity:** SSH login from `10.0.0.50` at 3 AM — investigate (even if password is correct).

**Key takeaway:** A baseline turns "weird" into "actionable."

---

## What I Learned

| Topic | Key Takeaway |
|-------|--------------|
| **Detection** | Automate thresholds with `if` or `while read` |
| **Hunting** | Use `grep -v` to exclude normal traffic (internal IPs) |
| **Baseline** | Define normal (IPs, hours, volume) to spot abnormal |
| `10.x.x.x` | Internal range (VirtualBox NAT), **not** public internet |
| `grep -v` | Exclude patterns — essential for hunting |

---

## Commands Reference

```bash
# Detection (root brute force)
COUNT=$(sudo journalctl -u ssh --since "1 minute ago" | grep "Failed password for root" | wc -l)
if [ "$COUNT" -gt 3 ]; then echo "ALERT"; fi

# Hunting (external SSH logins)
sudo journalctl -u ssh --since "7 days ago" | grep "Accepted" | grep -v "192.168." | grep -v "10\."

# Baseline check (logins outside working hours)
sudo journalctl -u ssh --since "today" | grep "Accepted" | grep -E "0[0-9]|1[0-9]|2[0-2]"
```
