# backup.sh auf der db-vm

```bash 
#!/bin/bash

# Variablen
DB_USER="backupuser"
DB_PASSWORD="password"
DB_NAME="testdb"
BACKUP_DIR="/home/ubuntu/backups"
BACKUP_FILE="${BACKUP_DIR}/backup_$(date +%F_%H-%M-%S).sql.gz"
AWS_BUCKET_NAME="my-backup-bucket"

# Erstelle das Backup-Verzeichnis, falls es nicht existiert
mkdir -p ${BACKUP_DIR}

# Datenbank sichern und komprimieren
mysqldump -u${DB_USER} -p${DB_PASSWORD} ${DB_NAME} | gzip > ${BACKUP_FILE}

# Hochladen zu AWS S3
aws s3 cp ${BACKUP_FILE} s3://${AWS_BUCKET_NAME}/

# Sende eine Benachrichtigung (E-Mail)
echo "Backup completed: ${BACKUP_FILE}" | mail -s "Backup Status" your-email@example.com
```

