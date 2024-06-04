

Um den öffentlichen Schlüssel aus einer PEM-Datei (`keygen01.pem`) zu extrahieren, können Sie den Befehl `ssh-keygen` verwenden. Hier ist die genaue Vorgehensweise:

1. **Navigieren Sie zum Verzeichnis mit Ihrer PEM-Datei:**

   ```sh
   cd /path/to/your/pem/file
   ```

2. **Extrahieren Sie den öffentlichen Schlüssel:**

   ```sh
   ssh-keygen -y -f keygen01.pem > keygen01.pub
   ```

   - `-y` steht für "read private key file and print public key".
   - `-f` gibt die Datei an, die gelesen werden soll.
   - `> keygen01.pub` leitet die Ausgabe (den öffentlichen Schlüssel) in eine Datei um.

3. **Überprüfen Sie den extrahierten öffentlichen Schlüssel:**

   ```sh
   cat keygen01.pub
   ```

   Dies zeigt den Inhalt der `keygen01.pub`-Datei an, in der sich der öffentliche Schlüssel befindet.

