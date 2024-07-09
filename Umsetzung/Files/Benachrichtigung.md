
## E-Mail-Benachrichtigung für Backup-Skript einrichten: Schritt-für-Schritt-Anleitung


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

## True vs. False und Positive vs. Negative Tests für das E-Mail-Benachrichtigungsskript

Um sicherzustellen, dass Ihr Skript korrekt funktioniert und die entsprechenden E-Mail-Benachrichtigungen sendet, müssen Sie verschiedene Tests durchführen, die sowohl positive als auch negative Fälle abdecken. Hier ist, wie Sie das machen können:

### Testfälle

#### 1. Positive Testfälle (True Positive)

- **Erfolgreiches Backup und E-Mail-Benachrichtigung**
  - Führen Sie das Skript aus und überprüfen Sie, ob die E-Mail "Backup Erfolg" gesendet wird.
  - Überprüfen Sie die Logdatei auf erfolgreiche Backup-Erstellung und E-Mail-Versand.

#### 2. Negative Testfälle (True Negative)

- **Fehlgeschlagenes Backup und E-Mail-Benachrichtigung**
  - Simulieren Sie einen Fehler im Backup-Prozess (z.B. falsche Datenbank-Anmeldeinformationen).
  - Überprüfen Sie, ob die E-Mail "Backup Fehler" gesendet wird.
  - Überprüfen Sie die Logdatei auf Fehlermeldungen und den fehlgeschlagenen Backup-Prozess.

#### 3. Falsche Positive

- **E-Mail-Benachrichtigung wird gesendet, obwohl Backup fehlgeschlagen ist**
  - Ändern Sie das Skript so, dass der Backup-Prozess absichtlich fehlschlägt, aber die Erfolgs-E-Mail gesendet wird.
  - Überprüfen Sie, ob eine falsche Erfolgs-E-Mail gesendet wird, wenn das Backup fehlschlägt.

#### 4. Falsche Negative

- **Backup erfolgreich, aber keine E-Mail-Benachrichtigung**
  - Ändern Sie das Skript so, dass das Backup erfolgreich ist, aber die Erfolgs-E-Mail nicht gesendet wird.
  - Überprüfen Sie, ob keine Erfolgs-E-Mail gesendet wird, obwohl das Backup erfolgreich war.

### Umsetzung der Tests

Hier sind die Schritte, um die oben genannten Testfälle umzusetzen:

#### 1. Erfolgreiches Backup und E-Mail-Benachrichtigung

- Stellen Sie sicher, dass alle Konfigurationsdateien korrekt sind.
- Führen Sie das Skript aus:
  ```bash
  ./backup.sh
  ```
- Überprüfen Sie, ob die E-Mail mit dem Betreff "Backup Erfolg" gesendet wurde.
- Überprüfen Sie die Logdatei:
  ```bash
  cat /home/ubuntu/backup.log
  ```

#### 2. Fehlgeschlagenes Backup und E-Mail-Benachrichtigung

- Ändern Sie die Datenbank-Anmeldeinformationen im Skript auf falsche Werte:
  ```bash
  DB_USER="wronguser"
  DB_PASS="wrongpassword"
  ```
- Führen Sie das Skript aus:
  ```bash
  ./backup.sh
  ```
- Überprüfen Sie, ob die E-Mail mit dem Betreff "Backup Fehler" gesendet wurde.
- Überprüfen Sie die Logdatei:
  ```bash
  cat /home/ubuntu/backup.log
  ```

#### 3. E-Mail-Benachrichtigung wird gesendet, obwohl Backup fehlgeschlagen ist

- Ändern Sie das Skript, um immer die Erfolgs-E-Mail zu senden, auch bei Fehlern:
  ```bash
  if [ $? -eq 0 ]; then
      echo "Backup wurde erfolgreich erstellt am $DATE" | mail -s "Backup Erfolg" your-email@hotmail.com
  else
      echo "Backup wurde erfolgreich erstellt am $DATE" | mail -s "Backup Erfolg" your-email@hotmail.com
  fi
  ```
- Führen Sie das Skript aus, wobei das Backup fehlschlägt (siehe Testfall 2).
- Überprüfen Sie, ob die Erfolgs-E-Mail gesendet wurde, obwohl das Backup fehlschlug.

#### 4. Backup erfolgreich, aber keine E-Mail-Benachrichtigung

- Ändern Sie das Skript, um die E-Mail-Benachrichtigung nicht zu senden, selbst wenn das Backup erfolgreich ist:
  ```bash
  if [ $? -eq 0 ]; then
      echo "Backup wurde erfolgreich erstellt am $DATE" # mail -s "Backup Erfolg" your-email@hotmail.com
  else
      echo "Backup fehlgeschlagen am $DATE" | mail -s "Backup Fehler" your-email@hotmail.com
  fi
  ```
- Führen Sie das Skript aus:
  ```bash
  ./backup.sh
  ```
- Überprüfen Sie, ob keine Erfolgs-E-Mail gesendet wurde, obwohl das Backup erfolgreich war.

### Fazit

Durch das Durchführen dieser Testfälle können Sie sicherstellen, dass Ihr Backup-Skript korrekt funktioniert und die entsprechenden E-Mail-Benachrichtigungen sendet. Dies umfasst das Testen sowohl positiver als auch negativer Szenarien, um mögliche Fehler und unerwartetes Verhalten zu identifizieren und zu beheben.