# 04. SSH and Remote Server Access

Once you create a Cloud Virtual Machine (like an Ubuntu server on DigitalOcean or AWS), there is no monitor, keyboard, or mouse plugged into it in the datacenter.

How do you connect to it and run commands from your own laptop? The answer is **SSH (Secure Shell)**.

---

## 1. What is SSH (Secure Shell)?

**SSH** is a cryptographic network protocol running over **Port 22** that allows you to securely access, control, and execute terminal commands on a remote computer over an unsecure network (the Internet).

```
+-------------------+                                      +-------------------+
|  Developer Laptop | ──────────[ Encrypted SSH Tunnel ]────────► |  Cloud Linux VM   |
|  (SSH Client)     | ◄─────────[      Port 22         ]───────── |  (SSH Daemon)     |
+-------------------+                                      +-------------------+
```

### Key Security Features:
1. **End-to-End Encryption:** Everything you type (commands, environment variables, code) is encrypted before it leaves your laptop.
2. **Data Integrity:** Prevents third parties from tampering with commands in transit.
3. **Authentication:** Guarantees that only authorized users can access the server.

---

## 2. Authentication Method 1: Password-Based Login

In password authentication, the server asks you for a password when you connect:

```bash
ssh root@146.190.221.189
# Prompt: root@146.190.221.189's password: [ ********** ]
```

### Why Password Login is Risky in Production:
* **Brute-Force Attacks:** Hackers run automated bot scripts scanning millions of public IPs on port 22 trying common passwords (e.g., `admin123`, `password`, `root`).
* **Vulnerable to Human Error:** People pick weak passwords or reuse them across websites.

---

## 3. Authentication Method 2: SSH Keypairs (The Gold Standard)

The modern, professional way to access servers is using **SSH Keypairs** based on **Asymmetric Cryptography**.

### The Lock and Key Analogy

```
+-------------------------------------------------------------------------+
|                       THE ASYMMETRIC KEY ANALOGY                        |
|                                                                         |
|  1. Public Key (`id_ed25519.pub`):                                      |
|     • Think of this as the "LOCK".                                      |
|     • You can make copies of this lock and put it on any server.        |
|     • It is completely safe to share with anyone publicly.              |
|                                                                         |
|  2. Private Key (`id_ed25519`):                                         |
|     • Think of this as the "KEY" that unlocks that lock.                |
|     • It NEVER leaves your personal laptop.                             |
|     • NEVER share it with anyone or commit it to GitHub.                |
+-------------------------------------------------------------------------+
```

### How the Challenge-Response Handshake Works

```
Developer Laptop                                      Cloud Server
(Holds Private Key)                                (Holds Public Key in
                                                    ~/.ssh/authorized_keys)
        │                                                     │
        │ 1. "Hey Server, I want to log in as user root"      │
        ├────────────────────────────────────────────────────►│
        │                                                     │
        │ 2. Server creates a challenge (puzzle) and locks it │
        │    with your Public Key                             │
        │ ◄───────────────────────────────────────────────────┤
        │                                                     │
        │ 3. Your Laptop solves/unlocks the puzzle using your │
        │    Private Key and sends the answer back            │
        ├────────────────────────────────────────────────────►│
        │                                                     │
        │ 4. Server verifies the answer.                      │
        │    Access Granted! Welcome to Ubuntu!               │
        │ ◄───────────────────────────────────────────────────┤
```

---

## 4. Key Algorithms: RSA vs. Ed25519

When generating an SSH key, you can choose the mathematical algorithm:

```
+-------------------------------------------------------------------------+
|  1. Ed25519 (RECOMMENDED)                                               |
|     • Based on Elliptic Curve Cryptography (Curve25519).                |
|     • Shorter key size (68 chars), ultra-fast, and highly secure.       |
|                                                                         |
|  2. RSA (Traditional / Legacy)                                          |
|     • Based on large prime number factorization.                        |
|     • Requires at least 2048-bit or 4096-bit length to remain secure.   |
+-------------------------------------------------------------------------+
```

---

## 5. Step-by-Step Hands-On Guide

### Step 1: Generate an SSH Keypair on Your Computer
Open your terminal (PowerShell, macOS Terminal, or Linux Bash):

```bash
# Generate a modern Ed25519 key:
ssh-keygen -t ed25519 -C "your_email@example.com"
```

* When prompted for file path: Press **Enter** to use default (`~/.ssh/id_ed25519`).
* When prompted for passphrase: You can set an optional password to protect your private key.

### Step 2: Inspect Your Generated Keys
Your keys are stored inside the hidden `.ssh` directory in your user home:

* **Private Key:** `~/.ssh/id_ed25519` *(Keep Secret!)*
* **Public Key:** `~/.ssh/id_ed25519.pub` *(Copy this to server)*

To view your public key:
```bash
cat ~/.ssh/id_ed25519.pub
```

### Step 3: Add Public Key to Your Cloud VM
When creating your VM in DigitalOcean / AWS, paste your public key into the **SSH Keys** section.

Or manually append it on the server inside:
```bash
~/.ssh/authorized_keys
```

### Step 4: Connect to Your Remote Server
```bash
ssh -i ~/.ssh/id_ed25519 root@<YOUR_SERVER_PUBLIC_IP>

# Example:
ssh root@146.190.221.189
```

---

## 6. Server Hardening Checklist (Production Best Practices)

Once your SSH key authentication is working:

1. **Disable Password Authentication:**
   Edit `/etc/ssh/sshd_config` on the server:
   ```ini
   PasswordAuthentication no
   ```
2. **Restart SSH Service:**
   ```bash
   sudo systemctl restart ssh
   ```
3. **Use a Firewall (UFW):**
   ```bash
   sudo ufw allow 22/tcp
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   sudo ufw enable
   ```

---

## 7. Summary: Key Takeaways

1. **SSH (Port 22)** is the primary method for managing remote cloud servers.
2. **Password authentication** is vulnerable to automated brute-force attacks.
3. **SSH Keypairs** use asymmetric cryptography:
   - **Public Key (`.pub`)** is placed on the server.
   - **Private Key** stays secret on your personal computer.
4. **Ed25519** is the modern standard algorithm recommended for all new keys.
