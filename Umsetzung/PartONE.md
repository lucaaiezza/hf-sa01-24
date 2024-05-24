# Setup und Durchführung mit Bash-Skripten

1. Setup der Multipass-VMs
Um mit der Einrichtung zu beginnen, erstellen wir zwei virtuelle Maschinen (VMs) mit Multipass. Eine davon wird für die Datenbank und die andere für die Backups verwendet.

```bash
# Erstellen der Datenbank-VM
multipass launch -n db-vm --cpus 2 --mem 2G --disk 5G
```

```bash
# Erstellen der Backup-VM
multipass launch -n backup-vm --cpus 2 --mem 2G --disk 10G
```

Mit diesen Befehlen erstellen wir zwei VMs: ```db-vm``` für die Datenbank und ```backup-vm``` für die Backups. Wir geben ihnen jeweils Ressourcen wie CPUs, Arbeitsspeicher und Festplattenspeicher.

2. Installation und Konfiguration der MySQL-Datenbank
Nachdem die VMs erstellt wurden, verbinden wir uns mit der ```db-vm``` und installieren MySQL.

```bash
multipass exec db-vm -- sudo apt update
multipass exec db-vm -- sudo apt install mysql-server -y
```

Nach der Installation erstellen wir eine Testdatenbank und einen Benutzer mit entsprechenden Berechtigungen.


```bash
multipass exec db-vm -- sudo mysql -e "CREATE DATABASE testdb;"
multipass exec db-vm -- sudo mysql -e "CREATE USER 'backupuser'@'%' IDENTIFIED BY 'password';"
multipass exec db-vm -- sudo mysql -e "GRANT ALL PRIVILEGES ON testdb.* TO 'backupuser'@'%';"
multipass exec db-vm -- sudo mysql -e "FLUSH PRIVILEGES;"
```
Diese Befehle erstellen eine Testdatenbank namens ```testdb``` und einen Benutzer namens ```backupuser``` mit dem Passwort ```password```. Der Benutzer erhält alle Berechtigungen für die Testdatenbank.

3. Installation der AWS CLI
Als nächstes installieren wir die AWS Command Line Interface (CLI) auf der ```db-vm```, um eine Verbindung zu Amazon Web Services (AWS) herzustellen.

```bash
multipass exec db-vm -- sudo apt install awscli -y
```

Nach der Installation konfigurieren wir die AWS CLI mit den Zugangsdaten, damit sie auf AWS-Dienste zugreifen kann.


```bash
multipass exec db-vm -- aws configure   
```

Mit diesem Befehl führen wir die Konfiguration aus, die uns auffordert, Zugangsdaten wie den Zugriffsschlüssel und den geheimen Zugriffsschlüssel einzugeben, um auf AWS zuzugreifen.

Diese Schritte ermöglichen es uns, eine sichere und effiziente Umgebung für die Datenbank und Backups einzurichten und AWS für die Speicherung von Backups zu nutzen.


4. Entwicklung des Backup-Skripts

Erstelle ein Bash-Skript, das auf der db-vm läuft und die Datenbank sichert. Das Skript komprimiert die Sicherung und lädt sie auf S3 hoch.