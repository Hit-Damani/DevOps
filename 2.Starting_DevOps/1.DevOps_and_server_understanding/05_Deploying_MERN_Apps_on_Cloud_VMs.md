# 05. Deploying a Full MERN App on a Cloud VM

Now that we understand networking, DNS, VMs, and SSH, let's put it all together into a complete, step-by-step production blueprint for deploying a **MERN (MongoDB, Express, React, Node.js)** application on a cloud server (e.g., DigitalOcean Droplet or AWS EC2).

---

## 1. Production Architecture Overview

Here is the complete architectural flow of a professional MERN stack deployment on a single cloud VM:

```
+-------------------------------------------------------------------------+
|                               INTERNET                                  |
|                 Users visiting: https://myapp.com                       |
+------------------------------------+------------------------------------+
                                     │
                                     ▼ (DNS A-Record lookup)
                    Public IP: 146.190.221.189
                                     │
+------------------------------------▼------------------------------------+
|                         CLOUD VIRTUAL MACHINE                           |
|                                                                         |
|  +-------------------------------------------------------------------+  |
|  |                     NGINX REVERSE PROXY                           |  |
|  |             (Listens on Port 80 HTTP / Port 443 HTTPS)            |  |
|  |                  Secured with SSL / TLS Cert                      |  |
|  +------------------+------------------------------+-----------------+  |
|                     │                              │                    |
|      Requests to /api/*              Static Frontend Requests           |
|                     ▼                              ▼                    |
|  +---------------------------+       +-------------------------------+  |
|  |     Node.js / Express     |       |   React Production Build      |  |
|  |      Backend Server       |       |   (HTML / CSS / JS bundle     |  |
|  |   [Managed by PM2 :5000]  |       |    served directly by Nginx)  |  |
|  +--------------+------------+       +-------------------------------+  |
|                 │                                                       |
|                 ▼ (Internal TCP: 27017)                                 |
|  +---------------------------+                                          |
|  |    MongoDB Database       |                                          |
|  |   (Stores collections)    |                                          |
|  +---------------------------+                                          |
+-------------------------------------------------------------------------+
```

---

## 2. Step 1: Provisioning Your Cloud VM

1. Log in to your cloud provider (e.g., [DigitalOcean](https://www.digitalocean.com) or [AWS](https://aws.amazon.com)).
2. Create a new instance:
   * **OS:** Ubuntu 22.04 LTS or 24.04 LTS.
   * **Plan:** Basic ($4 - $6/month).
   * **Authentication:** Select your **SSH Public Key**.
3. Once created, copy the **Public IP Address** assigned to your VM (e.g., `146.190.221.189`).

---

## 3. Step 2: Connect to Your VM via SSH

Open your local terminal and connect:

```bash
ssh root@<YOUR_VM_PUBLIC_IP>
```

Update the server packages:
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 4. Step 3: Install Node.js, Git, and Build Tools

Install Node.js (v20 LTS):

```bash
# Download and install NodeSource setup script:
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Install Node.js & Git:
sudo apt install -y nodejs git build-essential
```

Verify installation:
```bash
node -v
npm -v
git --version
```

---

## 5. Step 4: Clone Your MERN Application

Clone your GitHub repository onto the server:

```bash
# Navigate to the web directory:
cd /var/www

# Clone your project:
git clone https://github.com/yourusername/mern-app.git
cd mern-app
```

---

## 6. Step 5: Keep the Backend Running 24/7 with PM2

When you run `node index.js`, the process dies as soon as you close your SSH terminal window.

To keep your backend running permanently and restart automatically after system reboots, use **PM2 (Production Process Manager)**:

```bash
# Install PM2 globally:
sudo npm install -g pm2

# Navigate to backend directory:
cd /var/www/mern-app/backend
npm install

# Start the application with PM2:
pm2 start server.js --name "mern-backend"

# Configure PM2 to start on server reboot:
pm2 startup
pm2 save
```

### Useful PM2 Commands:
```bash
pm2 status        # View all running apps
pm2 logs          # View live real-time console logs
pm2 restart all   # Restart all backend processes
pm2 stop all      # Stop processes
```

---

## 7. Step 6: Build the React Frontend

```bash
cd /var/www/mern-app/frontend
npm install
npm run build
```

This compiles your React app into a static folder (`dist/` or `build/`).

---

## 8. Step 7: Configure Nginx Reverse Proxy

Install Nginx:
```bash
sudo apt install -y nginx
```

Create a new Nginx server configuration:
```bash
sudo nano /etc/nginx/sites-available/mern-app
```

Paste the following configuration:

```nginx
server {
    listen 80;
    server_name myapp.com www.myapp.com;

    # 1. Serve Static React Frontend:
    root /var/www/mern-app/frontend/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 2. Reverse Proxy API Requests to Node.js Backend:
    location /api/ {
        proxy_pass http://127.0.0.1:5000/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable the site and restart Nginx:
```bash
sudo ln -s /etc/nginx/sites-available/mern-app /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

## 9. Step 8: Secure with Free SSL / HTTPS (Let's Encrypt)

Install Certbot to obtain free automatic SSL certificates:

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d myapp.com -d www.myapp.com
```

Certbot automatically configures HTTPS (Port 443) and sets up automatic certificate renewal!

---

## 10. Summary Checklist for MERN Deployment

```
+------------------------------------------------------------------------+
|                      MERN DEPLOYMENT CHECKLIST                         |
+------------------------------------------------------------------------+
|  [x] Provision Cloud VM with Public IP (Ubuntu)                        |
|  [x] Set up SSH Keypair authentication & disable password login        |
|  [x] Install Node.js, Git, and project dependencies                    |
|  [x] Use PM2 to run Node.js backend continuously in background         |
|  [x] Build production React frontend bundle                            |
|  [x] Configure Nginx as Reverse Proxy & Static File Server             |
|  [x] Point Custom Domain via DNS A Records                             |
|  [x] Generate free SSL Certificate with Certbot (HTTPS)                |
+------------------------------------------------------------------------+
```
