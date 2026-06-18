# MongoDB Backup, Transfer & Restore Guide (Replica Set)

This guide explains the full process to:
1. Create MongoDB backup (dump)
2. Compress the backup
3. Transfer it to a remote server
4. Extract the backup
5. Restore the database
6. Verify the restoration

---

## 📌 Environment Details

### Source Server
- User: `nazir`
- Path: `/home/nazir`

### Database Details
- Database Name: `prime-autoz`
- Replica Set: `rs1`
- Ports: `27018`, `27019`

### MongoDB Connection Example
```bash
mongodb://admin:<password>@64.23.214.130:27018,64.23.214.130:27019/?replicaSet=rs1&authSource=admin


🧾 1. Create MongoDB Dump
mongodump \
  --uri="mongodb://127.0.0.1:27018,127.0.0.1:27019/?replicaSet=rs1" \
  --db="prime-autoz" \
  --out="/home/nazir/mongodb-backup"

📦 2. Compress Backup
cd /home/nazir

tar -czvf mongodb-backup.tar.gz mongodb-backup

🚀 3. Transfer Backup to Remote Server
scp /home/nazir/mongodb-backup.tar.gz nazir@64.23.214.130:/home/nazir/
OR (recommended for large files):
rsync -avz --progress /home/nazir/mongodb-backup.tar.gz \
nazir@64.23.214.130:/home/nazir/

📂 4. Extract Backup on Destination Server
ssh nazir@64.23.214.130

cd /home/nazir

tar -xzvf mongodb-backup.tar.gz
This will create:
/home/nazir/mongodb-backup/prime-autoz

♻️ 5. Restore MongoDB Database
mongorestore \
  --uri="mongodb://admin:<password>@64.23.214.130:27018,64.23.214.130:27019/?replicaSet=rs1&authSource=admin" \
  --db="prime-autoz" \
  /home/nazir/mongodb-backup/prime-autoz

mongorestore \
  --uri="mongodb://admin:<password>@64.23.214.130:27018,64.23.214.130:27019/?replicaSet=rs1&authSource=admin" \
  --db="prime-autoz" \
  --drop \
  /home/nazir/mongodb-backup/prime-autoz

✅ 6. Verify Restore
mongosh "mongodb://admin:<password>@64.23.214.130:27018/admin?replicaSet=rs1"