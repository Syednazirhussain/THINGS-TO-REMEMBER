# MongoDB 7.0.x Installation & Replica Set Setup Guide (Ubuntu 24.04)

This document provides step-by-step instructions to install MongoDB 7.0.x and configure a two-node replica set on Ubuntu 24.04. Authentication is not enabled in this guide.

---

## 1. Remove Old MongoDB (if any)
```bash
sudo apt remove -y mongodb*
sudo apt purge -y mongodb*
sudo apt autoremove -y
```

## 2. Add MongoDB Official Repository
```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt update
```

## 3. Install MongoDB 7.0.x
```bash
sudo apt install -y mongodb-org
```
Verify installation:
```bash
mongod --version
```

## 4. Prepare Data Directories
```bash
sudo mkdir -p /var/lib/mongodb/db1
sudo mkdir -p /var/lib/mongodb/db2
sudo mkdir -p /var/log/mongodb
sudo chown -R mongodb:mongodb /var/lib/mongodb
sudo chown -R mongodb:mongodb /var/log/mongodb
```
- `db1` → primary (port 27018)
- `db2` → secondary (port 27019)

## 5. Create MongoDB Config Files

### Primary (`/etc/mongod-rs1-primary.conf`)
```yaml
storage:
  dbPath: /var/lib/mongodb/db1

net:
  port: 27018
  bindIp: 0.0.0.0

replication:
  replSetName: rs1

systemLog:
  destination: file
  path: /var/log/mongodb/mongod-rs1-primary.log
  logAppend: true

processManagement:
  timeZoneInfo: /usr/share/zoneinfo
```

### Secondary (`/etc/mongod-rs1-secondary.conf`)
```yaml
storage:
  dbPath: /var/lib/mongodb/db2

net:
  port: 27019
  bindIp: 0.0.0.0

replication:
  replSetName: rs1

systemLog:
  destination: file
  path: /var/log/mongodb/mongod-rs1-secondary.log
  logAppend: true

processManagement:
  timeZoneInfo: /usr/share/zoneinfo
```

## 6. Create systemd Service Files

### Primary
`/etc/systemd/system/mongod-rs1-primary.service`
```ini
[Unit]
Description=MongoDB ReplicaSet Primary
After=network.target

[Service]
User=mongodb
Group=mongodb
ExecStart=/usr/bin/mongod --config /etc/mongod-rs1-primary.conf
Restart=always
LimitNOFILE=64000

[Install]
WantedBy=multi-user.target
```

### Secondary
`/etc/systemd/system/mongod-rs1-secondary.service`
```ini
[Unit]
Description=MongoDB ReplicaSet Secondary
After=network.target

[Service]
User=mongodb
Group=mongodb
ExecStart=/usr/bin/mongod --config /etc/mongod-rs1-secondary.conf
Restart=always
LimitNOFILE=64000

[Install]
WantedBy=multi-user.target
```

## 7. Start MongoDB Services
```bash
sudo systemctl daemon-reload
sudo systemctl enable mongod-rs1-primary mongod-rs1-secondary
sudo systemctl start mongod-rs1-primary mongod-rs1-secondary
sudo systemctl status mongod-rs1-primary
sudo systemctl status mongod-rs1-secondary
```

## 8. Initialize Replica Set
```bash
mongosh --port 27018
```
```js
rs.initiate({
  _id: "rs1",
  members: [
    { _id: 0, host: "127.0.0.1:27018" },
    { _id: 1, host: "127.0.0.1:27019" }
  ]
})
```
Check status:
```js
rs.status()
```
- 27018 → PRIMARY
- 27019 → SECONDARY

## 9. Firewall / Network Access
```bash
sudo ufw allow 27018
sudo ufw allow 27019
sudo ufw reload
```
Optional: restrict to specific IPs:
```bash
sudo ufw allow from YOUR_IP to any port 27018
sudo ufw allow from YOUR_IP to any port 27019
```

## 10. MongoDB Compass Connection
```text
mongodb://64.23.214.130:27018,64.23.214.130:27019/?replicaSet=rs1
```
- Use `127.0.0.1` if connecting via SSH tunnel

## 11. Optional Next Steps
- Enable authentication (`security.authorization: enabled`) and create users
- Use `keyFile` for replica set internal authentication
- Restrict MongoDB to VPN or trusted IPs for security

## 12. Notes / Best Practices
1. Separate data directories for each replica member
2. Systemd services allow auto-restart and logging
3. Always test configs with:
```bash
mongod --config /etc/mongod-rs1-primary.conf --test
```
4. Do not expose MongoDB without authentication in production
5. Use replica set even for single server if planning to scale in future

