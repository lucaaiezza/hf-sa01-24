
# Anleitung zur Einrichtung von Multipass und MySQL-Backup

## Schritt 1: Installiere Multipass
Zuerst musst du sicherstellen, dass Multipass auf deinem Laptop installiert ist. Falls nicht, kannst du Multipass von der [offiziellen Webseite](https://multipass.run) herunterladen und installieren.

## Schritt 2: Erstelle zwei VMs
Erstelle zwei VMs, eine für die MySQL-Datenbank und eine für das Backup.

```sh
# Erstelle die erste VM für die MySQL-Datenbank
multipass launch --name mysql-vm --cpus 2 --mem 2G --disk 10G

# Erstelle die zweite VM für die Backups
multipass launch --name backup-vm --cpus 2 --mem 2G --disk 10G
```

## Schritt 3: Installiere MySQL auf der ersten VM
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

Gewähre die notwendigen Berechtigungen für den MySQL-Benutzer, um den Fehler "Access denied; you need (at least one of) the PROCESS privilege(s) for this operation" zu beheben.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Melde dich bei MySQL an
sudo mysql -u root

# Erteile dem Benutzer testuser die erforderlichen Berechtigungen
GRANT PROCESS, RELOAD, LOCK TABLES ON *.* TO 'testuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Schritt 5: Installiere notwendige Tools auf beiden VMs
Installiere `mysqldump`, `gzip` und `scp` auf beiden VMs.

```sh
# Verbinde dich mit beiden VMs und installiere die Tools
for vm in mysql-vm backup-vm; do
  multipass exec $vm -- sudo apt update
  multipass exec $vm -- sudo apt install mysql-client gzip openssh-client -y
done
```

## Schritt 6: Konfiguriere die SSH-Schlüssel für die VMs
Generiere SSH-Schlüssel auf der `mysql-vm` und kopiere den öffentlichen Schlüssel auf die `backup-vm`.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Generiere den SSH-Schlüssel
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_rsa

# Kopiere den öffentlichen Schlüssel auf die backup-vm
multipass copy-files ~/.ssh/id_rsa.pub backup-vm:/home/ubuntu/
```

Erstelle ein SSH-Schlüsselpaar auf der `backup-vm`.

```sh
# Verbinde dich mit der backup-vm
multipass shell backup-vm

# Generiere ein SSH-Schlüsselpaar
ssh-keygen -t rsa -b 2048 -N "" -f ~/.ssh/id_rsa
```

Kopiere den öffentlichen Schlüssel auf die `mysql-vm`.

```sh
# Verbinde dich mit der backup-vm
multipass shell backup-vm

# Kopiere den öffentlichen Schlüssel zur mysql-vm
cat ~/.ssh/id_rsa.pub | ssh ubuntu@mysql-vm 'cat >> ~/.ssh/authorized_keys'
```

Bestätige, dass der Schlüssel auf der `mysql-vm` hinzugefügt wurde.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Überprüfe die authorized_keys Datei
cat ~/.ssh/authorized_keys
```

Teste die SSH-Verbindung von der `mysql-vm` zur `backup-vm`.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Teste die SSH-Verbindung
ssh ubuntu@backup-vm
```

Stelle sicher, dass die Dateiberechtigungen korrekt sind.

```sh
# Auf der mysql-vm
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Auf der backup-vm
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Teste die SSH-Verbindung mit `scp` manuell.

```sh
# Auf der mysql-vm
multipass shell mysql-vm

# Erstelle eine Testdatei
echo "Test file" > testfile.txt

# Versuche die Datei zur backup-vm zu kopieren
scp testfile.txt ubuntu@backup-vm:/home/ubuntu/
```

## Schritt 7: Erstelle das Backup-Skript
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

# Übertragung zur Backup-VM
scp $BACKUP_FILE.gz ubuntu@backup-vm:/home/ubuntu/backups/
EOF

# Mach das Skript ausführbar
chmod +x ~/backup.sh
```

## Schritt 8: Erstelle den Backup-Ordner auf der Backup-VM
Erstelle den Ordner für die Backups auf der `backup-vm`.

```sh
# Verbinde dich mit der backup-vm
multipass shell backup-vm

# Erstelle den Backup-Ordner
mkdir -p /home/ubuntu/backups
```

## Schritt 9: Teste das Backup-Skript
Führe das Backup-Skript auf der `mysql-vm` aus und überprüfe, ob das Backup auf der `backup-vm` angekommen ist.

```sh
# Verbinde dich mit der mysql-vm
multipass shell mysql-vm

# Führe das Backup-Skript aus
./backup.sh

# Überprüfe, ob das Backup auf der backup-vm angekommen ist
multipass shell backup-vm
ls /home/ubuntu/backups



