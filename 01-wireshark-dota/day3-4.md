# Date: 21/03/2026

**Objective:** Analyze packet structure, length, data, and filtering.

**Steps:**

- Captured Dota 2 traffic (bot lobby)
- Filtered for Valve server IP: 155.133.230.98
- Observed raw data (Follow UDP Stream — encrypted, as expected)
- Analyzed packet lengths:
 -- Normal gameplay: 200–300 bytes
 -- Chat messages: ~450 bytes (filter: ip.src == MY_IP and frame.len > 400)
- Confirmed each long message = one UDP packet (~453 bytes)

**Results:**

- Chat messages produce larger packets (~450 bytes)
- Dota 2 traffic is encrypted (Valve's GameNetworkingSockets)
- Packet size patterns help detect anomalies

**Screenshots:**

<img width="769" height="164" alt="Screenshot 2026-03-21 214111" src="https://github.com/user-attachments/assets/c8e4d36c-2917-4867-aea9-c85ad13c1534" /> 
<img width="992" height="272" alt="Screenshot 2026-03-21 214131" src="https://github.com/user-attachments/assets/e2b55113-aa08-46a8-967e-643badf031e8" />
<img width="1009" height="617" alt="Screenshot 2026-03-21 205657" src="https://github.com/user-attachments/assets/d41c8bd5-7c6c-4537-b8c0-980f082247b6" /> 
<img width="1918" height="782" alt="Screenshot 2026-03-21 205515" src="https://github.com/user-attachments/assets/0e3c8def-5156-496d-a9f9-79dcfdddd9d9" />  
<img width="1277" height="1017" alt="Screenshot 2026-03-21 205431" src="https://github.com/user-attachments/assets/019bb59e-5475-40a5-8b1d-e6caa3c28771" />
