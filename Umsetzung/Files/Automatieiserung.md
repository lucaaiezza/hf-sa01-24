**Um das Skript zu automatisieren, können Sie es in eine Crontab einfügen, damit es zu festgelegten Zeiten automatisch ausgeführt wird. Hier sind die Schritte:**

1. **Skript überprüfen und sicherstellen, dass es ausführbar ist**:
   - Stellen Sie sicher, dass Ihr Skript ausführbar ist:
     ```bash
     chmod +x /path/to/your/script.sh
     ```

2. **Crontab bearbeiten**:
   - Öffnen Sie die Crontab-Datei für den aktuellen Benutzer:
     ```bash
     crontab -e
     ```
   - Fügen Sie eine Zeile hinzu, um das Skript zu den gewünschten Zeiten auszuführen. Zum Beispiel, um das Skript täglich um 2 Uhr morgens auszuführen:
     ```bash
     0 2 * * * /path/to/your/script.sh
     ```

   Hier ist eine kurze Erklärung der Cron-Zeitformatierung:
   - `0` Minute (0–59)
   - `2` Stunde (0–23)
   - `*` Tag des Monats (1–31)
   - `*` Monat (1–12)
   - `*` Wochentag (0–7, wobei 0 und 7 beide Sonntag darstellen)

3. **Crontab-Eintrag Beispiel**:
   - Um das Skript täglich um 2 Uhr morgens auszuführen:
     ```bash
     0 2 * * * /home/ubuntu/dein_skript.sh
     ```

4. **Umgebungsvariablen sicherstellen**:
   - Wenn das Skript Umgebungsvariablen benötigt, stellen Sie sicher, dass sie innerhalb des Skripts oder im Crontab definiert sind.

Hier ein vollständiges Beispiel, wie das aussehen könnte:

```bash
# Öffnen Sie Crontab
crontab -e

# Fügen Sie die folgende Zeile hinzu
0 2 * * * /home/ubuntu/dein_skript.sh
```

**Hinweis:** Achten Sie darauf, dass die Pfade korrekt sind und dass das Skript über die notwendigen Berechtigungen verfügt, um auf die Datenbank zuzugreifen und Dateien zu übertragen.

**Zusätzliche Überlegungen**:
- **Logs und Fehlerprotokolle**: Es kann hilfreich sein, Ausgaben und Fehler des Skripts zu protokollieren, um Probleme zu diagnostizieren. Zum Beispiel:
  ```bash
  0 2 * * * /home/ubuntu/dein_skript.sh > /home/ubuntu/backup.log 2>&1
  ```


1. **Starten Sie Crontab**:
   ```bash
   crontab -e
   ```

2. **Wählen Sie den Editor** (z.B. `1` für `nano`):
   ```bash
   Choose 1-4 [1]: 1
   ```

3. **Fügen Sie die Crontab-Zeile hinzu**:
   ```plaintext
   */5 * * * * /home/ubuntu/backup.sh >> /home/ubuntu/backup.log 2>&1
   ```

4. **Speichern und schliessen**:
   - Bei `nano`: `Ctrl+O` zum Speichern, Enter zum Bestätigen und `Ctrl+X` zum Beenden.

Wenn alles richtig gemacht wurde, wird Ihr Skript `backup.sh` nun alle 5 Minuten ausgeführt und die Ausgabe wird in `backup.log` gespeichert.


# Logs 

Um die Logs anzusehen, können Sie den Inhalt der Logdatei `backup.log` mit verschiedenen Befehlen in der Kommandozeile anzeigen. Hier sind einige nützliche Befehle:

1. **Den gesamten Inhalt der Logdatei anzeigen**:
   ```bash
   cat /home/ubuntu/backup.log
   ```

2. **Den Inhalt der Logdatei seitenweise anzeigen (nützlich bei grossen Dateien)**:
   ```bash
   less /home/ubuntu/backup.log
   ```

   Verwenden Sie die Pfeiltasten, um nach oben und unten zu scrollen, und drücken Sie `q`, um `less` zu beenden.

3. **Die letzten Zeilen der Logdatei anzeigen**:
   ```bash
   tail /home/ubuntu/backup.log
   ```

4. **Die letzten Zeilen der Logdatei fortlaufend anzeigen (nützlich zum Überwachen in Echtzeit)**:
   ```bash
   tail -f /home/ubuntu/backup.log
   ```

   Drücken Sie `Ctrl+C`, um das Monitoring zu beenden.

5. **Die letzten n Zeilen der Logdatei anzeigen**:
   ```bash
   tail -n 100 /home/ubuntu/backup.log
   ```

   Ersetzen Sie `100` durch die gewünschte Anzahl von Zeilen.

Hier ist eine kurze Erklärung, wie Sie diese Befehle verwenden können:

- **`cat`**: Zeigt den gesamten Inhalt der Datei auf einmal an.
- **`less`**: Ermöglicht das seitenweise Durchblättern der Datei.
- **`tail`**: Zeigt die letzten Zeilen der Datei an.
- **`tail -f`**: Zeigt die letzten Zeilen der Datei an und aktualisiert die Anzeige, wenn die Datei wächst.

Wählen Sie den Befehl, der am besten zu Ihren Bedürfnissen passt. Zum Beispiel, wenn Sie nur überprüfen möchten, ob Ihr Skript korrekt ausgeführt wurde und keine Fehler aufgetreten sind, können Sie einfach die letzten Zeilen mit `tail` anzeigen:

```bash
tail /home/ubuntu/backup.log
```

Oder, wenn Sie fortlaufend überwachen möchten, verwenden Sie:

```bash
tail -f /home/ubuntu/backup.log
```

Auf diese Weise können Sie die Logs überprüfen und sicherstellen, dass Ihre Backups ordnungsgemäss durchgeführt werden.