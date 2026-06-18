# Archetype CI/CD & SSH Setup Guide

This document explains **step-by-step** how to set up:
- Passwordless SSH access from local machine to server
- GitHub SSH authentication on server
- GitHub Actions CI/CD deployment for a Node.js project using PM2

This guide is reusable for future projects.

---

## 1. Server & Project Basics

- **Server Provider:** DigitalOcean
- **OS:** Ubuntu
- **Server IP:** `64.23.214.130`
- **Server User:** `nazir`
- **Node Version:** `22.x` (via NVM)
- **Process Manager:** PM2
- **Web Server:** Apache2
- **Repository:** GitHub (SSH-based)
- **Branch:** `master`

---

## 2. Generate SSH Key on Local Machine

On your **local machine**, generate an SSH key (if not already created):

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

This creates:
- Private key: `~/.ssh/id_ed25519`
- Public key: `~/.ssh/id_ed25519.pub`

---

## 3. Add SSH Public Key to Server

### 3.1 Copy public key (LOCAL)

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the full output.

---

### 3.2 Login to server using password (ONE TIME)

```bash
ssh nazir@64.23.214.130
```

---

### 3.3 Add key to authorized_keys (SERVER)

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste the public key and save.

---

### 3.4 Fix SSH permissions (VERY IMPORTANT)

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chown -R nazir:nazir ~/.ssh
```

---

### 3.5 Verify passwordless login (LOCAL)

```bash
ssh -i ~/.ssh/id_ed25519 nazir@64.23.214.130
```

✅ You should log in **without password**.

---

## 4. Setup GitHub SSH Access on Server

### 4.1 Generate SSH key on server (if needed)

```bash
ssh-keygen -t ed25519 -C "github-server"
```

### 4.2 Add server public key to GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

GitHub → **Settings → SSH and GPG keys → New SSH key**

### 4.3 Test GitHub SSH connection

```bash
ssh -T git@github.com
```

Expected output:
```
Hi <username>! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 5. Ensure Git Repo Uses SSH (NOT HTTPS)

```bash
git remote -v
```

Correct format:
```
origin git@github.com:username/repo.git (fetch)
origin git@github.com:username/repo.git (push)
```

If not:
```bash
git remote set-url origin git@github.com:username/repo.git
```

---

## 6. Install Node.js via NVM (SERVER)

```bash
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
nvm alias default 22
```

---

## 7. Install PM2 (SERVER)

```bash
npm install -g pm2
pm2 startup
```

Run the command PM2 outputs (with sudo) and then:
```bash
pm2 save
```

---

## 8. PM2 Ecosystem Config Example

```js
module.exports = {
  apps: [
    {
      name: "archetype-api",
      script: "index.js",
      cwd: "/var/www/html/arhetype-backend-api",
      instances: 1,
      exec_mode: "fork",
      env: {
        NODE_ENV: "production"
      }
    }
  ]
};
```

Start app:
```bash
pm2 start ecosystem.config.js
pm2 save
```

---

## 9. GitHub Actions – CI/CD Setup

### 9.1 Add SSH private key to GitHub Secrets

Local machine:
```bash
cat ~/.ssh/id_ed25519
```

GitHub Repo → **Settings → Secrets → Actions**

Add secret:
```
Name: SSH_PRIVATE_KEY
Value: <private key content>
```

### 9.2 GitHub Actions Workflow Example (Exact Used)

```yaml
name: Deploy Archetype Backend API

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup SSH agent
        uses: webfactory/ssh-agent@v0.9.0
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Deploy to server
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: 64.23.214.130
          username: nazir
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            # Load NVM for non-interactive shell
            export NVM_DIR="$HOME/.nvm"
            [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"

            # Navigate to project
            cd /var/www/html/arhetype-backend-api

            # Pull latest code
            git pull origin master

            # Use correct Node version
            nvm use 22

            # Install production dependencies
            npm install --production

            # Restart or start PM2 process
            pm2 restart ecosystem.config.js || pm2 start ecosystem.config.js
```

✅ Notes:
- Uses **SSH agent** with GitHub secret key for both `git pull` and `ssh-action`
- Loads **NVM manually** for non-interactive shell
- Ensures PM2 starts process if not already running

---

## 10. Common Errors & Fixes

### ❌ Exit code 255
- SSH key missing or incorrect
- Wrong permissions
- Password-based SSH

### ❌ Git asking for username/password
- Repo using HTTPS instead of SSH

### ❌ PM2 not found
- Installed under different user
- NVM not sourced

---

## 11. Final Best Practices

- ✅ Use **non-root user** (`nazir`)
- ✅ Use **SSH keys only**
- ✅ Use **PM2 reload** for zero downtime
- ✅ Use **NVM** instead of system Node
- ✅ Keep CI/CD non-interactive
- ✅ Use `pm2 restart || pm2 start` in workflow
- ✅ Ensure **Secrets** are set in GitHub Actions for SSH_PRIVATE_KEY

---

## ✅ Setup Complete

Your deployment pipeline is now:

**Git Push → GitHub Actions → Server Pull → PM2 Reload**

🚀 Production-ready & repeatable

