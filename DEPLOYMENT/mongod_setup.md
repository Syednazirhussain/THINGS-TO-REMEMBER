### MongoDB INSTALLATION
-   sudo apt update
-   sudo apt install -y gnupg curl
-   curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc \
    | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
-   echo "deb [ arch=amd64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" \
    | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
-    sudo apt update
-    sudo apt install -y mongodb-org=7.0.14 \
        mongodb-org-server=7.0.14 \
        mongodb-org-shell=7.0.14 \
        mongodb-org-mongos=7.0.14 \
        mongodb-org-tools=7.0.14
-    sudo systemctl enable --now mongod
-    sudo systemctl status mongod

### SETUP MONGODB ON SERVER

-   SERVER ROOT /mongod-replication.json
{
  "apps": [
    {
      "name": "db1",
      "script": "mongod",
      "args": "--dbpath=/mongodb/db1/data --replSet=rs1 --port=27018 --bind_ip=0.0.0.0"
    },
    {
      "name": "db2",
      "script": "mongod",
      "args": "--dbpath=/mongodb/db2/data --replSet=rs1 --port=27019 --bind_ip=0.0.0.0"
    }
  ]
}
