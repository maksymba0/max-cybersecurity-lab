# Date: 23/04/2026

## Hunting Low-and-Slow Attacks: Pattern Recognition

### Scenario

A SOC analyst notices an odd pattern over 2 weeks:

- Every Tuesday at 3:14 AM
- A single failed SSH login for user `support`
- Different IP address each time
- No successes
- No other activity

Most alerts would ignore this (low volume). But the **pattern** reveals a possible low-and-slow attack.

---

### Why This Is Suspicious

| Normal activity | This pattern |
|----------------|--------------|
| Random timing | Fixed day and time |
| Few failures | Exactly one failure per week |
| Same IP if repeated | Different IP each time |

**Interpretation:** An attacker is probing for a valid account without triggering volume-based alerts.

---

### Investigation Steps

1. **Check the IPs** — are they from the same region, ISP, or ASN?
2. **Check ASN** — do they share a common hosting provider?
3. **Check user `support`** — does it exist? Does it have a weak password?
4. **Check login history** — any successful logins for `support` in the last 30 days?

---

### What Is an ASN?

**ASN** = **Autonomous System Number** — a unique identifier for a network under one administrative control.

Example: `AS15169` is Google.

Two IPs with the same ASN may belong to the same organization (e.g., same cloud provider, same attacker infrastructure).

#### How to Check ASN (Linux)

```bash
whois 203.0.113.89 | grep -i "origin"
```
