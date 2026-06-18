# Ubuntu Deployment Guide

## Node.js (TypeScript API), ReactJS, Next.js with Apache2 and PM2

---

# Table of Contents

1. Server Preparation
2. Install Node.js & PM2
3. Deploy TypeScript Backend API
4. Deploy ReactJS Application
5. Deploy Next.js Application
6. Apache2 Commands
7. PM2 Commands
8. SSL Setup with Certbot
9. Troubleshooting Commands

---

# 1. Server Preparation

Update server packages:

```bash
sudo apt update
sudo apt upgrade -y
```

Install Apache2:

```bash
sudo apt install apache2 -y
```

Enable Apache2:

```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```

Check status:

```bash
sudo systemctl status apache2
```

---

# 2. Install Node.js & PM2

## Install NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash

source ~/.bashrc
```

Verify:

```bash
nvm --version
```

## Install Node.js

```bash
nvm install --lts
nvm use --lts
```

Verify:

```bash
node -v
npm -v
```

---

## Install PM2

```bash
npm install -g pm2
```

Verify:

```bash
pm2 -v
```

---

# 3. Deploy TypeScript Backend API

Example:

```text
Project Path:
/var/www/html/my-api
```

---

## Build Application

```bash
cd /var/www/html/my-api

npm install
npm run build
```

---

## PM2 Ecosystem File

Create:

```bash
nano ecosystem.config.json
```

```json
{
  "apps": [
    {
      "name": "my-api",
      "script": "dist/index.js",
      "cwd": "/var/www/html/my-api",
      "instances": 1,
      "exec_mode": "fork",
      "autorestart": true,
      "watch": false,
      "max_memory_restart": "500M",
      "env": {
        "NODE_ENV": "production",
        "PORT": 5000
      }
    }
  ]
}
```

---

## Start PM2

```bash
pm2 start ecosystem.config.json
pm2 save
pm2 startup
```

---

## Apache Virtual Host

File:

```bash
sudo nano /etc/apache2/sites-available/api.example.com.conf
```

```apache
<VirtualHost *:80>
    ServerName api.example.com

    ProxyPreserveHost On
    ProxyRequests Off

    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/

    ErrorLog ${APACHE_LOG_DIR}/api_error.log
    CustomLog ${APACHE_LOG_DIR}/api_access.log combined
</VirtualHost>
```

Enable:

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers

sudo a2ensite api.example.com.conf

sudo apache2ctl configtest
sudo systemctl reload apache2
```

---

# 4. Deploy ReactJS Application

Example:

```text
Project Path:
/var/www/html/my-react-app
```

---

## Build Application

```bash
cd /var/www/html/my-react-app

npm install
npm run build
```

Output:

```text
build/
```

---

## Apache Virtual Host

```bash
sudo nano /etc/apache2/sites-available/app.example.com.conf
```

```apache
<VirtualHost *:80>
    ServerName app.example.com

    DocumentRoot /var/www/html/my-react-app/build

    <Directory /var/www/html/my-react-app/build>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted

        RewriteEngine On
        RewriteBase /

        RewriteRule ^index\.html$ - [L]

        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteCond %{REQUEST_FILENAME} !-d

        RewriteRule . /index.html [L]
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/react_error.log
    CustomLog ${APACHE_LOG_DIR}/react_access.log combined
</VirtualHost>
```

---

## Enable Required Modules

```bash
sudo a2enmod rewrite
sudo a2enmod headers

sudo systemctl restart apache2
```

---

## Enable Site

```bash
sudo a2ensite app.example.com.conf

sudo apache2ctl configtest

sudo systemctl reload apache2
```

---

## Optional PM2 Static Server

Install:

```bash
npm install -g serve
```

ecosystem.config.json

```json
{
  "apps": [
    {
      "name": "react-app",
      "script": "serve",
      "args": "-s build -l 3000",
      "cwd": "/var/www/html/my-react-app"
    }
  ]
}
```

Start:

```bash
pm2 start ecosystem.config.json
```

Note: Apache-only deployment is generally preferred for React build files.

---

# 5. Deploy Next.js Application

Example:

```text
Project Path:
/var/www/html/my-next-app
```

---

## Build Application

```bash
cd /var/www/html/my-next-app

npm install
npm run build
```

---

## PM2 Ecosystem File

```json
{
  "apps": [
    {
      "name": "next-app",
      "cwd": "/var/www/html/my-next-app",
      "script": "node_modules/next/dist/bin/next",
      "args": "start -p 7000",
      "instances": 1,
      "exec_mode": "fork",
      "autorestart": true,
      "watch": false,
      "max_memory_restart": "500M",
      "env": {
        "NODE_ENV": "production",
        "PORT": "7000"
      }
    }
  ]
}
```

---

## Start PM2

```bash
pm2 start ecosystem.config.json

pm2 save
pm2 startup
```

---

## Apache Virtual Host

```bash
sudo nano /etc/apache2/sites-available/example.com.conf
```

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com

    ProxyPreserveHost On
    ProxyRequests Off

    ProxyPass / http://127.0.0.1:7000/
    ProxyPassReverse / http://127.0.0.1:7000/

    RequestHeader set X-Forwarded-Proto "http"
    RequestHeader set X-Forwarded-Port "80"

    ErrorLog ${APACHE_LOG_DIR}/next_error.log
    CustomLog ${APACHE_LOG_DIR}/next_access.log combined
</VirtualHost>
```

---

## Enable Modules

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers

sudo systemctl restart apache2
```

---

## Enable Site

```bash
sudo a2ensite example.com.conf

sudo apache2ctl configtest

sudo systemctl reload apache2
```

---

# 6. Apache2 Commands

Enable site:

```bash
sudo a2ensite site.conf
```

Disable site:

```bash
sudo a2dissite site.conf
```

Test configuration:

```bash
sudo apache2ctl configtest
```

Reload:

```bash
sudo systemctl reload apache2
```

Restart:

```bash
sudo systemctl restart apache2
```

Status:

```bash
sudo systemctl status apache2
```

Logs:

```bash
sudo tail -f /var/log/apache2/error.log
```

---

# 7. PM2 Commands

Start:

```bash
pm2 start ecosystem.config.json
```

Restart:

```bash
pm2 restart app-name
```

Reload:

```bash
pm2 reload app-name
```

Stop:

```bash
pm2 stop app-name
```

Delete:

```bash
pm2 delete app-name
```

View logs:

```bash
pm2 logs app-name
```

Status:

```bash
pm2 status
```

Save processes:

```bash
pm2 save
```

Auto-start after reboot:

```bash
pm2 startup
```

---

# 8. SSL Setup with Certbot

Install:

```bash
sudo apt install certbot python3-certbot-apache -y
```

Single domain:

```bash
sudo certbot --apache -d example.com
```

Domain + WWW:

```bash
sudo certbot --apache \
-d example.com \
-d www.example.com
```

Verify:

```bash
sudo certbot certificates
```

Renew test:

```bash
sudo certbot renew --dry-run
```

---

# 9. Troubleshooting Commands

Check listening ports:

```bash
ss -tulpn
```

Check specific port:

```bash
ss -tulpn | grep 5000
```

Kill process on port:

```bash
sudo fuser -k 5000/tcp
```

Check DNS:

```bash
dig example.com +short
```

Check website headers:

```bash
curl -I https://example.com
```

Check PM2 logs:

```bash
pm2 logs
```

Check Apache logs:

```bash
tail -f /var/log/apache2/error.log
```

Check Node version:

```bash
node -v
```

Check npm version:

```bash
npm -v
```

---

# Recommended Production Architecture

## TypeScript Backend

```text
Internet
    ↓
Apache2
    ↓
PM2
    ↓
Node.js API
    ↓
MongoDB
```

## ReactJS

```text
Internet
    ↓
Apache2
    ↓
React Build Files
```

## Next.js

```text
Internet
    ↓
Apache2
    ↓
PM2
    ↓
Next.js Server
```
