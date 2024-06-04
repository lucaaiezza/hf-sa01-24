

1. **Installieren Sie `mailutils`**:
   ```bash
   sudo apt-get update
   sudo apt-get install mailutils
   ```

2. **Konfigurieren Sie `s-nail` oder `mailx` für Hotmail**:
   - Erstellen oder bearbeiten Sie die Konfigurationsdatei `/etc/s-nail.rc`:
     ```bash
     sudo nano /etc/s-nail.rc
     ```

   - Fügen Sie die folgenden Zeilen hinzu, um Hotmail als SMTP-Server zu verwenden:
     ```plaintext
     set smtp=smtp://smtp-mail.outlook.com:587
     set smtp-auth=login
     set smtp-auth-user=your-email@hotmail.com
     set smtp-auth-password=your-email-password
     set ssl-verify=ignore
     set from="your-email@hotmail.com"
     ```

3. **Skript anpassen, um eine E-Mail zu senden**:
   - Fügen Sie in Ihrem Skript `backup.sh` nach dem Backup-Befehl einen Befehl zum Senden einer E-Mail ein:
     ```bash
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

     # Prüfen, ob der Backup-Prozess erfolgreich war
     if [ $? -eq 0 ]; then
         echo "Backup wurde erfolgreich erstellt am $DATE" | mail -s "Backup Erfolg" your-email@hotmail.com
     else
         echo "Backup fehlgeschlagen am $DATE" | mail -s "Backup Fehler" your-email@hotmail.com
     fi
     ```

4. **Testen Sie das Skript manuell**:
   - Führen Sie das Skript manuell aus, um sicherzustellen, dass es funktioniert und die E-Mail-Benachrichtigung gesendet wird:
     ```bash
     /home/ubuntu/backup.sh
     ```

5. **Crontab anpassen, falls nötig**:
   - Wenn alles funktioniert, müssen Sie nichts an der Crontab ändern, da das Skript jetzt die E-Mail-Benachrichtigung enthält. Die bestehende Crontab-Einstellung wird das Skript weiterhin alle 5 Minuten ausführen:
     ```plaintext
     */5 * * * * /home/ubuntu/backup.sh >> /home/ubuntu/backup.log 2>&1
     ```

**Wichtig**: Beim Konfigurieren des E-Mail-Programms mit Ihrem Hotmail-Konto stellen Sie sicher, dass Sie die korrekten SMTP-Einstellungen und Zugangsdaten verwenden. Achten Sie darauf, dass Ihr Hotmail-Konto für den SMTP-Zugriff konfiguriert ist. Bei Bedarf kann es auch erforderlich sein, ein App-Passwort zu erstellen, wenn Ihr Konto die Zwei-Faktor-Authentifizierung aktiviert hat.


# Test 


Um zu überprüfen, ob Ihre E-Mail-Konfiguration funktioniert und ob das Skript erfolgreich eine E-Mail sendet, können Sie das Skript manuell ausführen. Hier ist, wie Sie das machen können:

1. **Öffnen Sie ein Terminal** auf Ihrer Ubuntu-VM.

2. **Wechseln Sie in das Verzeichnis, in dem sich Ihr Skript befindet**, wenn es nicht bereits dort ist. Angenommen, Ihr Skript befindet sich unter `/home/ubuntu`, verwenden Sie den folgenden Befehl:
   ```bash
   cd /home/ubuntu
   ```

3. **Führen Sie das Backup-Skript aus**:
   ```bash
   ./backup.sh
   ```

4. **Überprüfen Sie die Ausgabe**:
   - Stellen Sie sicher, dass das Skript ohne Fehler ausgeführt wird.
   - Überprüfen Sie, ob die Sicherungsdatei erstellt und auf die Backup-VM übertragen wurde.
   - Überprüfen Sie Ihren E-Mail-Posteingang, um sicherzustellen, dass die Test-E-Mail erfolgreich gesendet wurde.

5. **Überprüfen Sie die Logdatei**:
   - Überprüfen Sie die Logdatei `backup.log`, um zu sehen, ob das Skript ohne Fehler ausgeführt wurde und ob die E-Mail-Benachrichtigung gesendet wurde.
   ```bash
   cat /home/ubuntu/backup.log
   ```

Wenn das Skript erfolgreich ausgeführt wird und Sie die Test-E-Mail erhalten, dann funktioniert Ihre E-Mail-Konfiguration und Ihr Skript wie erwartet. Wenn etwas nicht wie erwartet funktioniert, überprüfen Sie die Fehlermeldungen in der Ausgabe und in der Logdatei, um das Problem zu diagnostizieren und zu beheben.