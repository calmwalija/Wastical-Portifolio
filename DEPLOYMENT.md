# Wastical Portfolio - VPS Deployment Guide

## Overview
This document explains the deployment setup for the Wastical portfolio website on the VPS.

## Project Structure on VPS

### Source Code Location
- **Project Directory**: `/root/wastical-portfolio`
- **Git Repository**: https://github.com/calmwalija/Wastical-Portifolio.git
- **Branch**: main

### Web Server Setup
- **Web Server**: Caddy (running in Docker)
- **Static Files Location**: `/var/www/html/portfolio/`
- **Domain**: wastical.app

## Deployment Process

### Automated Update Script
The portfolio website can be updated using the following process:

1. **SSH into VPS**:
   ```bash
   ssh root@YOUR_VPS_IP
   ```

2. **Navigate to project directory**:
   ```bash
   cd /root/wastical-portfolio
   ```

3. **Fetch and pull latest changes**:
   ```bash
   git fetch origin
   git pull origin main
   ```

4. **Install dependencies** (if needed):
   ```bash
   npm install
   ```

5. **Build the project**:
   ```bash
   npm run build
   ```

6. **Deploy built files**:
   ```bash
   rm -rf /var/www/html/portfolio/*
   cp -r dist/* /var/www/html/portfolio/
   ```

7. **Restart web server**:
   ```bash
   docker restart caddy
   ```

### One-liner Update Command
```bash
ssh root@YOUR_VPS_IP "cd /root/wastical-portfolio && git pull origin main && npm install && npm run build && rm -rf /var/www/html/portfolio/* && cp -r dist/* /var/www/html/portfolio/ && docker restart caddy"
```

## Directory Structure

```
/root/
└── wastical-portfolio/          # Source code repository
    ├── src/                     # Vue.js source files
    ├── dist/                    # Built files (after npm run build)
    ├── package.json
    └── vite.config.js

/var/www/html/
└── portfolio/                   # Deployed static files
```

## SSL Certificates

SSL certificates are automatically managed by Caddy using Let's Encrypt for the wastical.app domain.

## Troubleshooting

### Check Web Server Status
```bash
docker ps | grep caddy
```

### View Web Server Logs
```bash
docker logs caddy
```

### Check Web Server Configuration
```bash
docker exec caddy caddy validate --config /etc/caddy/Caddyfile
```

### Restart Web Server
```bash
docker restart caddy
```

## Security Notes

- Root SSH access enabled
- SSL/TLS encryption enabled via Caddy and Let's Encrypt

---

**Last Updated**: January 9, 2025
**Contact**: For deployment issues, check git repository and Caddy logs first.
