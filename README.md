# 🌍 Travel Memory - MERN Deployment on AWS

> A complete MERN stack application deployed on AWS EC2 with load balancing, reverse proxy, and Cloudflare CDN integration.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![AWS](https://img.shields.io/badge/AWS-EC2-orange)](https://aws.amazon.com)
[![Node.js](https://img.shields.io/badge/Node.js-v18+-green)](https://nodejs.org)
[![React](https://img.shields.io/badge/React-v18+-blue)](https://react.dev)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green)](https://mongodb.com)

**Live Application:** [https://rushi.online](https://rushi.online)

---

## 📖 Table of Contents

- [Project Overview](#-project-overview)
- [Architecture](#-architecture)
- [Technology Stack](#-technology-stack)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Deployment Guide](#-deployment-guide)
- [Screenshots](#-screenshots)
- [Security](#-security)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 📖 Project Overview

**Travel Memory** is a full-stack MERN application enabling users to create, view, and manage travel experiences with:

- ✅ Multi-instance scaling with AWS ALB
- ✅ Production-grade Nginx reverse proxy
- ✅ SSL/TLS via Cloudflare
- ✅ PM2 process management
- ✅ MongoDB Atlas cloud database
- ✅ Global CDN distribution
- ✅ High availability & fault tolerance

**Original Repository:** [TravelMemory](https://github.com/UnpredictablePrashant/TravelMemory)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│         User Browser (Global)                    │
└─────────────────┬───────────────────────────────┘
                  │
                  ▼
       ┌──────────────────────────┐
       │  Cloudflare (DNS+CDN)    │
       │  Full Strict SSL/TLS     │
       │  DDoS Protection         │
       └──────────────┬───────────┘
                      │
                      ▼
       ┌──────────────────────────────┐
       │ AWS Application Load Balancer│
       │ (Route 53 + ALB)             │
       └──────────┬────────────────────┘
                  │
        ┌─────────┴─────────┐
        │                   │
        ▼                   ▼
   ┌─────────────┐    ┌─────────────┐
   │EC2 Instance │    │EC2 Instance │
   │     1       │    │     2       │
   ├─────────────┤    ├─────────────┤
   │ Nginx Port80│    │ Nginx Port80│
   ├─────────────┤    ├─────────────┤
   │React Build  │    │React Build  │
   ├─────────────┤    ├─────────────┤
   │PM2 Port3000 │    │PM2 Port3000 │
   │Node Backend │    │Node Backend │
   └──────┬──────┘    └──────┬──────┘
          │                  │
          └────────┬─────────┘
                   ▼
        ┌──────────────────────┐
        │  MongoDB Atlas       │
        │  (Cloud Database)    │
        └──────────────────────┘
```

---

## 🛠️ Technology Stack

| Component | Technology | Version |
|-----------|-----------|----------|
| Frontend | React.js | v18+ |
| Backend | Node.js + Express | v18+ |
| Database | MongoDB Atlas | Cloud |
| Server | Ubuntu | 22.04 LTS |
| Web Server | Nginx | Latest |
| Process Manager | PM2 | Latest |
| Load Balancer | AWS ALB | - |
| DNS/CDN | Cloudflare | Pro/Business |
| SSL/TLS | Cloudflare Full Strict | - |

---

## ✅ Prerequisites

- AWS account with EC2 access
- Domain name (e.g., rushi.online)
- Cloudflare account (Free tier minimum)
- MongoDB Atlas cluster (Free tier available)
- SSH client (PuTTY/Terminal)
- Git installed locally
- Basic Linux & Nginx knowledge

---

## 🚀 Quick Start

### Clone Repository
```bash
git clone https://github.com/Rushiargade/TravelMemory-Deployment.git
cd TravelMemory-Deployment
```

### Manual Setup
Follow the [Deployment Guide](#-deployment-guide) section.

---

## 🎯 Deployment Guide

### 1. Backend Configuration

#### 1.1 Launch EC2 Instance
- **OS:** Ubuntu 22.04 LTS
- **Type:** t2.micro (or t2.small for better performance)
- **Security Group:** Open ports 22 (SSH), 80 (HTTP), 443 (HTTPS)
- **Storage:** 20GB minimum

#### 1.2 Connect via SSH
```bash
ssh -i your-key.pem ubuntu@your-ec2-ip
```

#### 1.3 Install Dependencies
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js & npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs npm git nginx

# Install PM2 globally
sudo npm install -g pm2
```

#### 1.4 Clone Backend Repository
```bash
git clone https://github.com/UnpredictablePrashant/TravelMemory.git
cd TravelMemory/backend
npm install
```

#### 1.5 Configure Environment
Create `.env` file in backend directory:
```env
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/travelmemory
PORT=3000
NODE_ENV=production
```

#### 1.6 Start Backend with PM2
```bash
pm2 start index.js --name "travelmemory-backend"
pm2 save
pm2 startup
```

Verify:
```bash
pm2 status
curl -i http://localhost:3000/trip
```

---

### 2. Frontend Setup

#### 2.1 Update API URL
Edit `frontend/src/urls.js`:
```javascript
const url = "https://rushi.online";
export default url;
```

#### 2.2 Build React Application
```bash
cd ../frontend
npm install
npm run build
```

#### 2.3 Deploy Frontend
```bash
# Create web directory
sudo mkdir -p /var/www/travelmemory_frontend

# Copy build files
sudo cp -r build/* /var/www/travelmemory_frontend/

# Set permissions
sudo chown -R www-data:www-data /var/www/travelmemory_frontend
sudo chmod -R 755 /var/www/travelmemory_frontend
```

---

### 3. Nginx Configuration

#### 3.1 Create Nginx Config
Create `/etc/nginx/sites-available/travelmemory`:

```nginx
server {
    listen 80;
    server_name rushi.online www.rushi.online;

    root /var/www/travelmemory_frontend;
    index index.html;

    # Gzip compression
    gzip on;
    gzip_types text/plain text/css text/javascript 
               application/json application/javascript;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;

    # React SPA routing
    location / {
        try_files $uri /index.html;
        expires -1;
        add_header Cache-Control "public, max-age=0";
    }

    # Backend API proxy
    location ^~ /trip {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Health check endpoint
    location /health {
        access_log off;
        return 200 "healthy\n";
        add_header Content-Type text/plain;
    }
}
```

#### 3.2 Enable Nginx Config
```bash
# Test configuration
sudo nginx -t

# Enable site
sudo ln -s /etc/nginx/sites-available/travelmemory /etc/nginx/sites-enabled/

# Remove default site (optional)
sudo rm /etc/nginx/sites-enabled/default

# Restart Nginx
sudo systemctl restart nginx

# Verify status
sudo systemctl status nginx
```

---

### 4. Load Balancer Setup

#### 4.1 Create AMI from EC2 Instance
1. Go to AWS EC2 Console
2. Select your instance
3. **Image and templates** → **Create image**
4. Name: `travelmemory-ami-v1`
5. Create image

#### 4.2 Launch Additional Instances
1. Go to **Instances** → **Launch instances**
2. Select AMI: `travelmemory-ami-v1`
3. Launch 1-2 additional instances

#### 4.3 Create Target Group
1. Go to **EC2** → **Target Groups**
2. Create target group:
   - **Name:** `travelmemory-tg`
   - **Protocol:** HTTP
   - **Port:** 80
   - **Health check path:** `/health`
3. Register all EC2 instances

#### 4.4 Create Application Load Balancer
1. Go to **Load Balancers**
2. Create load balancer:
   - **Name:** `travelmemory-alb`
   - **Scheme:** Internet-facing
   - **Listener:** HTTP:80 → Forward to `travelmemory-tg`

---

### 5. Cloudflare Configuration

#### 5.1 Add Domain to Cloudflare
1. Go to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Add site → Enter `rushi.online`
3. Select **Free plan**

#### 5.2 Update Nameservers
1. At your registrar, change nameservers to:
   ```
   lou.ns.cloudflare.com
   pearl.ns.cloudflare.com
   ```

#### 5.3 Configure DNS Records

| Type | Name | Value | TTL | Proxy |
|------|------|-------|-----|-------|
| CNAME | @ | `travelmemory-alb-xxxxx.region.elb.amazonaws.com` | Auto | Proxied |
| CNAME | www | `travelmemory-alb-xxxxx.region.elb.amazonaws.com` | Auto | Proxied |

#### 5.4 SSL/TLS Settings
1. Go to **SSL/TLS** → **Overview**
2. Set to: **Full (strict)**
3. Enable: **Always Use HTTPS**
4. Minimum TLS Version: **1.2**

---

## 📸 Screenshots

See `docs/screenshots/` folder for deployment screenshots.

---

## 🔒 Security

### Implemented Security Measures:
- ✅ SSL/TLS encryption (Cloudflare Full Strict)
- ✅ Security groups restricting EC2 access
- ✅ MongoDB Atlas IP whitelisting
- ✅ Environment variables for sensitive data
- ✅ Nginx rate limiting (optional)
- ✅ PM2 process isolation
- ✅ Cloudflare DDoS protection

---

## 🐛 Troubleshooting

### Backend not responding:
```bash
pm2 logs travelmemory-backend
pm2 restart travelmemory-backend
```

### Nginx errors:
```bash
sudo nginx -t
sudo systemctl status nginx
sudo tail -f /var/log/nginx/error.log
```

### Load balancer health checks failing:
- Verify security groups allow ALB → EC2 communication
- Check target group health check path: `/trip`
- Ensure backend is running on all instances

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👤 Author

**Rushikesh Argade**  


- 📧 Email: rushikeshargade54@gmail.com
- 💼 LinkedIn: [linkedin.com/in/rushikesh-argade](https://linkedin.com/in/rushikesh-argade)
- 🌐 Portfolio: https://safe-newt-d0e.notion.site/Rushikesh-Argade-2bd128cc292480b1a59cc8f6d0b1318e?source=copy_link
- 📁 GitHub: [@Rushiargade](https://github.com/Rushiargade)

---

**⭐ If this helped you, please give it a star!**
