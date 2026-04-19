# Date: 19/04/2026

## SOC Investigation: Live SSH Brute Force Detection

### Scenario

I simulated a small SSH brute force attack from Attacker VM to Target VM, then investigated using only system logs — no Wireshark, no network captures. This mimics a real SOC analyst reviewing logs after an alert.

---

### Steps Performed

| Step | Action | Command |
|------|--------|---------|
| 1 | Monitor SSH logs in real time (Target) | `sudo journalctl -u ssh -f` |
| 2 | Simulate failed SSH login attempts (Attacker) | `ssh -b 192.168.100.20 root@192.168.100.10` (wrong password, repeated 3 times) |
| 3 | Observe live log entries | `Failed password for root from 192.168.100.20` |
| 4 | Count total failures from attacker IP | `sudo journalctl -u ssh \| grep "Failed password" \| grep "192.168.100.20" \| wc -l` |

<img width="1590" height="585" alt="Screenshot 2026-04-19 201932" src="https://github.com/user-attachments/assets/0e7dd8a1-5977-4edf-87fa-e8d8dc1d31c7" />

---

### Observations

- Each failed login attempt was logged immediately in `journalctl`
- Connection was closed after each failure (root password login disabled — secure default)
- The logs included: timestamp, user (`root`), source IP (`192.168.100.20`), and source port
 
