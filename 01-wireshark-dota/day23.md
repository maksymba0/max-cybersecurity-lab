# Date: 10/04/2026

## SIEM Scenario Analysis (Security Information and Event Management)

*Situation*: "Multiple failed logins on server 10.0.10.25, followed by successful login from new IP"

**Alert:** Multiple failed logins → successful login on 10.0.10.25 from new IP

**My analysis:**

1. **First log to check:** Linux auth.log (server is Linux)
2. **What to look for:** username, source IP, timestamp, authentication method
3. **3 AM + foreign IP:** Highly suspicious — possible brute force success or credential compromise
4. **Exceptions:** Employee on authorized business trip (but would not follow failed attempts)

**What I Learned:**
- SIEM correlates events across systems
- Not every alert is a true positive, but this one needs immediate investigation
- Context matters: business trips, VPN usage, time zones
- Anti-cheat and SIEM share the same core idea: detect anomalies
