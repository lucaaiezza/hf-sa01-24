
## Schritt 1: Installiere und konfiguriere Multipass auf deinem lokalen Rechner
Stelle sicher, dass Multipass auf deinem Laptop installiert ist. Falls nicht, kannst du Multipass von der [offiziellen Webseite](https://multipass.run) herunterladen und installieren.

## Schritt 2: Erstelle die MySQL-VM lokal
Erstelle die MySQL-VM, die lokal auf deinem Rechner laufen wird.

```sh
# Erstelle die VM für die MySQL-Datenbank
multipass launch --name mysql-vm --cpus 2 --mem 2G --disk 10G
```

## Schritt 3: Installiere MySQL auf der MySQL-VM
Verbinde dich mit der `mysql-vm` und installiere MySQL.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Aktualisiere die Paketliste und installiere MySQL
sudo apt update
sudo apt install mysql-server -y

# Starte den MySQL-Dienst
sudo systemctl start mysql

# Sichere die MySQL-Installation (Optional)
sudo mysql_secure_installation
```

## Schritt 4: Erstelle eine Test-Datenbank
Erstelle eine Test-Datenbank und füge einige Daten hinzu.

```sh
# Melde dich bei MySQL an
sudo mysql -u root

# Erstelle eine Test-Datenbank
CREATE DATABASE testdb;

# Erstelle einen Benutzer für die Datenbank
CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'password';

# Gewähre dem Benutzer alle Rechte auf die Test-Datenbank
GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'@'localhost';

# Wähle die Test-Datenbank aus
USE testdb;

# Erstelle eine Test-Tabelle und füge einige Daten hinzu
CREATE TABLE testtable (id INT PRIMARY KEY, data VARCHAR(100));
INSERT INTO testtable (id, data) VALUES (1, 'Hello, World!'), (2, 'Backup Test');
EXIT;
```

Gewähre die notwendigen Berechtigungen für den MySQL-Benutzer.

```sh
# Melde dich bei MySQL an
sudo mysql -u root

# Erteile dem Benutzer testuser die erforderlichen Berechtigungen
GRANT PROCESS, RELOAD, LOCK TABLES ON *.* TO 'testuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Schritt 5: Installiere notwendige Tools auf der MySQL-VM
Installiere `mysqldump`, `gzip` und `scp` auf der MySQL-VM.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Installiere die Tools
sudo apt update
sudo apt install mysql-client gzip openssh-client -y
```

## Schritt 6: Erstelle eine EC2-Instanz auf AWS
Erstelle eine EC2-Instanz auf AWS, die als Backup-VM dienen wird. Folge der AWS-Dokumentation zur Einrichtung einer EC2-Instanz. Stelle sicher, dass du einen SSH-Schlüssel (PEM-Datei) zur Authentifizierung generierst und herunterlädst.

## Schritt 7: Konfiguriere die EC2-Instanz (Backup-VM)
Verbinde dich mit der EC2-Instanz und installiere die notwendigen Tools.

```sh
# Verbinde dich mit der EC2-Instanz
ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>

# Installiere die Tools
sudo apt update
sudo apt install mysql-client gzip -y

# Erstelle den Backup-Ordner
mkdir -p /home/ubuntu/backups
```

## Schritt 8: Konfiguriere die SSH-Schlüssel für die VMs
Generiere SSH-Schlüssel auf der `mysql-vm` und kopiere den öffentlichen Schlüssel auf die EC2-Instanz.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Generiere den SSH-Schlüssel
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_rsa

# Kopiere den öffentlichen Schlüssel zur EC2-Instanz
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@<your-ec2-public-ip>
```

Stelle sicher, dass die Dateiberechtigungen korrekt sind.

```sh
# Auf der mysql-vm
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Auf der EC2-Instanz
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Teste die SSH-Verbindung von der `mysql-vm` zur EC2-Instanz.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Teste die SSH-Verbindung
ssh ubuntu@<your-ec2-public-ip>
```

## Schritt 9: Erstelle das Backup-Skript auf der MySQL-VM
Erstelle ein Backup-Skript auf der `mysql-vm`.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Erstelle ein Backup-Skript
cat << 'EOF' > ~/backup.sh
#!/bin/bash

# Konfigurationsvariablen
DB_NAME="testdb"
DB_USER="testuser"
DB_PASS="password"
BACKUP_PATH="/home/ubuntu/backups"
DATE=$(date +%F)
BACKUP_FILE="$BACKUP_PATH/$DB_NAME-$DATE.sql"

# Sicherstellen, dass das Backup-Verzeichnis existiert
mkdir -p $BACKUP_PATH

# Sicherung der Datenbank
mysqldump -u $DB_USER -p$DB_PASS $DB_NAME > $BACKUP_FILE

# Komprimierung der Sicherungsdatei (überschreiben)
gzip -f $BACKUP_FILE

# Übertragung zur Backup-VM (EC2-Instanz)
scp $BACKUP_FILE.gz ubuntu@<your-ec2-public-ip>:/home/ubuntu/backups/
EOF

# Mach das Skript ausführbar
chmod +x ~/backup.sh
```

## Schritt 10: Teste das Backup-Skript
Führe das Backup-Skript auf der `mysql-vm` aus und überprüfe, ob das Backup auf der EC2-Instanz angekommen ist.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Führe das Backup-Skript aus
./backup.sh

# Überprüfe, ob das Backup auf der EC2-Instanz angekommen ist
ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
ls /home/ubuntu/backups
```

Diese Schritte sollten sicherstellen, dass die MySQL-Datenbank lokal auf einer Multipass-VM läuft und die Backups in einer AWS EC2-Instanz gespeichert werden. Achte darauf, die entsprechenden IP-Adressen, Benutzernamen und Pfade anzupassen.