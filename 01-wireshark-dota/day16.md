# Date: 02/04/2026

**Objective:** Simulate and detect a brute force attack on FTP

**Setup:**
- Target VM (192.168.100.10) running vsftpd (very secured FTP daemon)
- Attacker VM (192.168.100.20) with Hydra

**Attack:**
- Hydra attempted 8 passwords against testuser
- Successful login with password: qwerty123456

**Detection:**
- Wireshark: multiple SYN packets to port 21 in rapid succession or failure
- Syslog: failed login attempts logged with "Login incorrect"
- Successful login logged with "Login succesful"
 
**What I Learned:**
- Brute force attacks are noisy so it is easy to detect in logs and network
- Rate-limiting and account lockouts are defenses
- Even simple services like FTP generate clear evidence

**Screenshots:**
<img width="1266" height="645" alt="Screenshot 2026-04-02 222527" src="https://github.com/user-attachments/assets/6a184cbe-1962-48c0-9c0b-b89e48db4d84" />
<img width="1176" height="500" alt="Screenshot 2026-04-02 222557" src="https://github.com/user-attachments/assets/10633774-e972-4898-a11a-9a0ca446dac6" />
<img width="1620" height="493" alt="Screenshot 2026-04-02 222737" src="https://github.com/user-attachments/assets/cef486ce-b7ec-45a6-8c3c-dced3995c38a" />
<img width="1289" height="601" alt="Screenshot 2026-04-02 223034" src="https://github.com/user-attachments/assets/16ba1f7d-b03e-42cf-a826-b8cacd5d6172" />
<img width="1624" height="482" alt="Screenshot 2026-04-02 223057" src="https://github.com/user-attachments/assets/c8ba2876-a84b-4365-b878-0e312f0c53e0" />
