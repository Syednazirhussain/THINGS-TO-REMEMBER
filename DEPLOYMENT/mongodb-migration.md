##  Clean any previous dump folder
    rm -rf /home/nazir/archetype_stagging_dump
    mkdir -p /home/nazir/archetype_stagging_dump

## Mongodump without Credentials
    mongodump \
    --host="64.23.214.130:27018,64.23.214.130:27019" \
    --db=archetype_stagging \
    --out=/home/nazir/archetype_stagging_dump \
    --verbose

## Mongorestore without Credentials
    mongorestore \
    --host="64.23.214.130:27018,64.23.214.130:27019" \
    --db=archetype_stagging \
    --drop \
    /home/nazir/archetype_stagging_dump/archetype_stagging

## Mongorestore with Credentials
    mongorestore \
    --host="rs1/64.23.214.130:27018,64.23.214.130:27019" \
    --username="admin" \
    --password="thermodynamic94X" \
    --authenticationDatabase="admin" \
    --db="archetype_stagging" \
    --drop \
    /home/nazir/archetype_stagging_dump/archetype_stagging

## Mongodump with Credentials
    mongodump \
    --uri="mongodb://admin:thermodynamic94X@64.23.214.130:27018,64.23.214.130:27019/?replicaSet=rs1&authSource=admin" \
    --db="archetype_stagging" \
    --out="/home/nazir/backups/archetype_stagging_$(date +%Y-%m-%d)"

##  Verify dump
    ls -l /home/nazir/archetype_stagging_dump
    ls -l /home/nazir/archetype_stagging_dump/archetype_stagging