# Date: 17/04/2026

## SSH Troubleshooting & Theory

### Hands-On: Diagnosing SSH Connection Issues

**Setup:**
- Attacker VM: `192.168.100.20`
- Target VM: `192.168.100.10`
- Both on same internal network (`192.168.100.0/24`)

**Problem:**  
`ssh root@192.168.100.10` hung with no output. Wireshark on Target showed TCP retransmissions of SYN packets from Attacker.

**Diagnosis:**
- SYN packets sent → no SYN-ACK received → connection not established
- Possible causes: firewall block, SSH not listening, wrong interface

**Solution:**  
Verified SSH was running on Target, then used `ssh -b 192.168.100.20 root@192.168.100.10` to bind to the correct interface.

<img width="1855" height="557" alt="Screenshot 2026-04-17 195548" src="https://github.com/user-attachments/assets/001ecdee-0306-4218-8a6a-9b8f5e7491f7" />
**Result**:  
Connection established. Password prompt appeared. Returned `Permission denied` error.
<img width="724" height="108" alt="Screenshot 2026-04-17 195618" src="https://github.com/user-attachments/assets/3a8bceba-1a61-4cce-b03b-c64e7f08f308"/>
<img width="1517" height="101" alt="Screenshot 2026-04-17 195630" src="https://github.com/user-attachments/assets/f10ce2c0-cd61-4edc-a682-f6d4bf49b062" />

### SSH Theory 

#### What is SSH?

SSH (Secure Shell) is a protocol for encrypted remote access to a computer over a network.

| Protocol | Port | Encryption | Use |
|----------|------|------------|-----|
| SSH | 22 | ✅ Yes | Secure remote administration |
| Telnet | 23 | ❌ No | Obsolete, insecure |
| FTP | 21 | ❌ No | File transfer (insecure) |

#### How SSH Works (Simplified)

1. **TCP handshake** — SYN → SYN-ACK → ACK
2. **Encryption negotiation** — client and server agree on cipher
3. **Authentication** — password or SSH key
4. **Session** — encrypted commands

#### Why SSH is Secure

| Threat | Protection |
|--------|------------|
| Password sniffing | Encryption (no plain text) |
| Man-in-the-middle | Server fingerprint verification |
| Brute force | Can be disabled or rate-limited |

#### Common SSH Hardening Settings

| Setting | Value | Effect |
|---------|-------|--------|
| `PermitRootLogin` | `no` | Root cannot log in via SSH |
| `PasswordAuthentication` | `no` | Only SSH keys allowed |
| `AllowUsers` | `user1 user2` | Limit who can connect |
| `Port` | `2222` | Change from default (obscurity) |

#### What `Permission denied` Means

| Error | Likely Cause |
|-------|--------------|
| `Permission denied (publickey)` | Password login disabled, need SSH key |
| `Permission denied (password)` | Wrong password or user doesn't exist |
| `Connection refused` | SSH server not running |
| `No route to host` | Network issue or firewall block |

#### Detecting SSH Brute Force in Logs

Log file: `/var/log/auth.log`
<img width="1568" height="134" alt="Screenshot 2026-04-17 195811" src="https://github.com/user-attachments/assets/3890b576-a67e-45ea-8e32-28bfa53a4a1c" />

**What I Learned**:
- SSH uses encryption — passwords are never visible in Wireshark
- TCP retransmissions indicate a connection problem (not a security issue)
- Permission denied means wrong password or login disabled
- Root password login is disabled by default on Ubuntu (prohibit-password)
- auth.log is the primary source for detecting SSH brute force attacks
- Hardening SSH (disabling root login, using keys) prevents many attacks
