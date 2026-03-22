# Date: 22/03/2026

**Objective:** Statistics,  conversations, endpoints, I/O Graphs,

**Steps:** 

- Captured Dota 2 traffic (bot lobby)
- Verified IPs used in EndPoints:
    "Address": "155.133.238.178", Data Center South Africa **(Valve)**
    "Address": "155.133.227.35", Data Center Brazil **(Valve)**
    "Address": "151.106.18.227", Data Center Velia.net France
    "Address": "148.72.174.135", Data Center Velia.net France
    "Address": "146.66.155.69", Data Center Austria **(Valve)**
    "Address": "103.28.54.188", Data Center Hong Kong **(Valve)**
    "Address": "103.10.125.20", Data Center Australia **(Valve)**
    "Address": "103.10.124.116", Data Center Singapore **(Valve)**
    "Address": "74.201.228.148", Data Center Internap Holding
    "Address": "68.169.42.221", Westhost Inc Data Center
    "Address": "23.251.100.126", Miami ZenLayer Data Center
- Investigated traffic betweeen client and server. Server sends much more data to client, than client to server.
- Observed abnormal network activities & the packet traffic peak via I/O Graph during match in different in-game scenarios:
  - a) Player was idle (not performing any activities)
  - b) Player was active (performing different activites, including Player Hero movement, casting spells, sending chat messsages)
  
**Results:**

- Valve has several Data Centres throughout the world to manage player data faster.
- Most of the game server activities is done on the server side, client receives much more data than sends.
- Learned about I/O graph tool which is an easier visual representation of how often are packets or conversations are done.

**Why it is useful:**
- If I saw unexpected IPs (non-Valve, non-Steam), that would be suspicious, possibly a reason for investigation.
- Typically server sends more data than client, which is normal for client-server architecture but if client started sending massive and more data, I'd investigate possible data leak.
- I/O Graph spikes during activity are normal. A spike during idle time would be a red flag.
  
**Screenshots:** 
<img width="1911" height="789" alt="Screenshot 2026-03-22 212227" src="https://github.com/user-attachments/assets/df7f4ca7-37b9-4c09-bb8c-299742e2ba99" />
<img width="1919" height="769" alt="Screenshot 2026-03-22 212318" src="https://github.com/user-attachments/assets/daef5e7f-8308-4a8a-ad59-ae81ff24a977" />
 <img width="1062" height="906" alt="Screenshot 2026-03-22 210925" src="https://github.com/user-attachments/assets/e45a380d-53c5-4315-817f-4a2efbbeed61" />
