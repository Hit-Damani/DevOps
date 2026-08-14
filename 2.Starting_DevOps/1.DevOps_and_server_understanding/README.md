# Starting DevOps, Virtual Machines & Reverse Proxies Management

Welcome to the **DevOps & Cloud Deployment Guide**! This module covers everything a developer needs to know about transitioning from local development (`localhost`) to hosting production-grade full-stack web applications on cloud infrastructure.

---

## 📚 Table of Contents & Learning Modules

| Module | Title | Topics Covered |
| :--- | :--- | :--- |
| **[01](01_Networking_Foundations_Localhost_IPs_and_DNS.md)** | **[Networking Foundations: Localhost, IPs, and DNS](01_Networking_Foundations_Localhost_IPs_and_DNS.md)** | Why `localhost` isn't enough, Loopback Interface (`127.0.0.1`), IPv4 vs IPv6, Domain Names, and DNS Resolution |
| **[02](02_Local_Networks_and_Mild_Hosting.md)** | **[Local Networks and Mild Hosting](02_Local_Networks_and_Mild_Hosting.md)** | LAN vs WAN, Private IP ranges (`192.168.x.x`), Hosting apps over home Wi-Fi, and Network Address Translation (NAT) |
| **[03](03_Deployment_Options_and_Server_Architectures.md)** | **[Deployment Options and Server Architectures](03_Deployment_Options_and_Server_Architectures.md)** | Cloud VMs vs Bare-Metal vs Serverless vs Self-Hosting, Hypervisors, and Virtual Machine internal architecture |
| **[04](04_SSH_and_Remote_Server_Access.md)** | **[SSH and Remote Server Access](04_SSH_and_Remote_Server_Access.md)** | Secure Shell (Port 22), Password vs Keypair Authentication, Asymmetric Cryptography, Ed25519 vs RSA, and Server Hardening |
| **[05](05_Deploying_MERN_Apps_on_Cloud_VMs.md)** | **[Deploying a Full MERN App on a Cloud VM](05_Deploying_MERN_Apps_on_Cloud_VMs.md)** | Complete production setup: Cloud Public IP, PM2 process management, Nginx Reverse Proxy, and Let's Encrypt SSL/TLS |

---

## 🗺️ Visual Architecture Map

```
+-------------------------------------------------------------------------+
|                           THE DEPLOYMENT JOURNEY                        |
+-------------------------------------------------------------------------+
|                                                                         |
|  [ Level 1: Localhost ]                                                 |
|  http://localhost:3000 (Accessible only to your laptop)                 |
|                            │                                            |
|                            ▼                                            |
|  [ Level 2: Mild Hosting (LAN) ]                                        |
|  http://192.168.1.5:3000 (Accessible to devices on same Wi-Fi router)   |
|                            │                                            |
|                            ▼                                            |
|  [ Level 3: Cloud Virtual Machine (Public IP) ]                         |
|  http://146.190.221.189:3000 (Accessible to anyone on the Internet)    |
|                            │                                            |
|                            ▼                                            |
|  [ Level 4: Production Architecture (Domain + Nginx + SSL) ]            |
|  https://myapp.com (Secure, reverse-proxied, 24/7 PM2 uptime)          |
|                                                                         |
+-------------------------------------------------------------------------+
```

---

## 💡 Quick Tips for Beginners
1. Start with **Module 01** to build your networking fundamentals.
2. Practice **Module 02** with your own mobile phone on your home Wi-Fi.
3. Use **Module 04 & 05** when you are ready to launch your project on a cloud provider like DigitalOcean or AWS.
