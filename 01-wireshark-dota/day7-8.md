# Date: 23/03/2026

**Objective**: DNS, Dota DNS observations

**Steps:**
- Capturing packets and DNS queries
- TTL (Time-to-live) Observations:
  - steam-chat.com: 8 seconds
  - steamcommunity.com: 20 seconds  
  - p2p-waw1.discovery.steamserver.net: 13 seconds

**Results**:
- Identified a few DNS belonging to Steam/Dota.
- Lower TTL means more DNS lookups (useful for services that change IPs often (load balancing, fast failover))
- `api.steampowered.com` root returns 404 (nginx)
- APIs require specific paths to return data (https://api.steampowered.com/ISteamWebAPIUtil/GetServerInfo/v1/)

Why it is useful:
If I saw a sudden TTL drop for a stable (usually high TTL) domain, it could mean some DNS hijacking or fast-flux malware. Now I understand the DNS logic much better.

<img width="1919" height="299" alt="Screenshot 2026-03-23 184017" src="https://github.com/user-attachments/assets/8c881650-aac5-461e-9a75-6cc6ec97ff29" />
<img width="1919" height="1021" alt="Screenshot 2026-03-23 184950" src="https://github.com/user-attachments/assets/720b9118-bd92-46c0-a1e1-83724b82f228" />
<img width="1919" height="1019" alt="Screenshot 2026-03-23 185110" src="https://github.com/user-attachments/assets/bbe42e36-b6a4-4b20-a0aa-fdd570272c71" />
<img width="715" height="220" alt="Screenshot 2026-03-23 195406" src="https://github.com/user-attachments/assets/52ace8e9-8d44-4bb3-97ca-60d1fe6f4c4a" />
