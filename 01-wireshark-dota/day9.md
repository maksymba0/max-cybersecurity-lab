# Date: 24/03/2026
**Objective:** HTTP, HTTPS traffic

**Steps:**

- Capturing packets and DNS queries
- Entering `http://dota2.com`, being redirected to HTTPS (302 Found)
- Full request and response visible in plain text:
  - Browser: Chrome on Windows
  - Redirect location: https://www.dota2.com

**Results:**
- HTTP exposes everything, so never send passwords or personal data over HTTP
- HTTPS protects content but still reveals which sites you visit
- Most modern sites force HTTPS redirects to protect users

Why it is useful: Using HTTPS protocol to transfer sensitive data. I will be careful with HTTP because it can be hacked and user's sensitive data can be leaked (login, password).

<img width="1281" height="984" alt="Screenshot 2026-03-24 162212" src="https://github.com/user-attachments/assets/8586b07b-2258-49de-b4f9-0e5fe75cd420" />
<img width="1279" height="1016" alt="Screenshot 2026-03-24 172601" src="https://github.com/user-attachments/assets/349ae3f3-9e47-4cc0-bafc-f2ba5a04895e" />
