# Date: 29/03/2026

**Objective**:
Fix a "Network is unreachable" error on Ubuntu and verify connectivity using DNS tools (dig/resolvectl)

**Observation**:
The system was confused by two network interfaces. We used sudo ip addr flush to clear stale settings and updated Netplan to prioritize enp0s8 (Internet) over enp0s3 (Local Lab) using a lower route-metric. This restored the "Default Gateway," allowing traffic to reach the web.

**Why This Matters**:
- Subnet Masks (/24): Define the "size" of your local network (256 IPs).
- resolvectl: Confirmed Ubuntu’s DNS "middleman" was active.
- dig: Verified the "Global Phonebook" by looking up Valve and Dota2 IP addresses.

**Priority**: Without a clear metric, a computer doesn't know which "door" leads to the internet.

<img width="1089" height="291" alt="Screenshot 2026-03-29 220217" src="https://github.com/user-attachments/assets/64fb47d3-193f-4a88-b593-964eea11e7e9" />
<img width="1076" height="597" alt="Screenshot 2026-03-29 220108" src="https://github.com/user-attachments/assets/8d3bb8df-4702-47dd-9d2c-c87a7579c828" />
