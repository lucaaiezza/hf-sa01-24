
## Reflexion über die Semesterarbeit

### Titel: Entwicklung eines Skripts zur automatisierten Datenbanksicherung

---

### Persönliche Reflexion

**a) Gemachte Aussagen hinterfragen**

Während der Entwicklung des Skripts habe ich festgestellt, dass die Wahl von AWS EC2 als Speicherziel zwar viele Vorteile bietet, jedoch auch mit zusätzlichen Kosten und einer gewissen Komplexität verbunden ist. Hätte ich vielleicht eine lokale Lösung bevorzugen sollen, um Kosten zu sparen und die Implementierung zu vereinfachen? Diese Frage stellte sich mir immer wieder im Verlauf des Projekts.

**b) Lösungen abwägen**

Mehrere Ansätze zur Datensicherung wurden geprüft. Eine lokale Sicherung hätte die Komplexität reduziert, aber die Sicherheit und Verfügbarkeit könnten darunter leiden. Cloud-basierte Lösungen wie AWS bieten hohe Verfügbarkeit und Sicherheit, sind jedoch teurer und komplexer in der Implementierung. Letztendlich entschied ich mich für die AWS-Lösung aufgrund ihrer langfristigen Vorteile in Sachen Skalierbarkeit und Zuverlässigkeit.

### Reflexion der technischen Umsetzung

**c) Eigene Erfahrungen einbringen**

Bei der Implementierung des Skripts konnte ich auf meine bisherigen Erfahrungen mit Bash-Scripting und AWS zurückgreifen. Besonders hilfreich war meine Erfahrung im Umgang mit `mysqldump` und `scp`, die zentrale Komponenten des Projekts darstellten. Der grösste Lernpunkt war die Integration der verschiedenen Tools und die Automatisierung der gesamten Sicherungsprozesse.

**d) Vorbehalte der eigenen Lösung**

Ein wesentlicher Vorbehalt meiner Lösung war die Abhängigkeit von AWS. Sollte der Dienst ausfallen oder die Kosten unerwartet steigen, wäre das Unternehmen erheblich beeinträchtigt. Zudem könnte die Einrichtung und Wartung des Systems für kleinere Unternehmen mit begrenztem technischen Know-how eine Herausforderung darstellen.

**e) Erkenntnisse aus der Bearbeitung**

Während der Projektarbeit wurde mir klar, wie wichtig eine durchdachte Planung und Strukturierung ist. Insbesondere die iterative Testphase zeigte, dass frühzeitige Tests und regelmässige Anpassungen entscheidend sind, um Fehler zu minimieren und die Funktionalität sicherzustellen. Ein weiterer wichtiger Punkt war die Bedeutung von Dokumentation, sowohl für die Nachvollziehbarkeit als auch für die zukünftige Wartung.

### Fazit und mögliche Weiterentwicklung

**f) Strukturierte Reflexion**

**Persönliche Reflexion:**
Das Projekt war eine wertvolle Lernerfahrung. Ich habe nicht nur meine technischen Fähigkeiten verbessert, sondern auch ein tieferes Verständnis für Projektmanagement und die Integration von Cloud-Diensten gewonnen. Es hat mir gezeigt, wie wichtig es ist, sowohl die technischen als auch die wirtschaftlichen Aspekte bei der Lösungsfindung zu berücksichtigen.

**Technische Reflexion:**
Die technische Umsetzung war anspruchsvoll, aber erfolgreich. Die grössten Herausforderungen waren die Konfiguration der EC2-Instanz und die Sicherstellung der sicheren Datenübertragung. Die Wahl der Tools und die Implementierung des Skripts verliefen grösstenteils reibungslos, was ich auf die gründliche Planung und die iterative Testphase zurückführe.

**Fazit:**
Insgesamt bin ich zufrieden mit dem Ergebnis meines Projekts. Das Skript erfüllt die gestellten Anforderungen und bietet eine zuverlässige Lösung zur Datenbanksicherung. Es ist flexibel genug, um in verschiedenen Unternehmensgrössen eingesetzt zu werden und kann bei Bedarf leicht erweitert werden.

**Mögliche Weiterentwicklung:**
Für die Zukunft sehe ich mehrere Möglichkeiten zur Verbesserung:
- **Erweiterte Benachrichtigungssysteme:** Integration von SMS-Benachrichtigungen oder Push-Benachrichtigungen über mobile Apps.
- **Mehrere Cloud-Anbieter:** Entwicklung einer Multi-Cloud-Strategie, um die Abhängigkeit von einem einzelnen Anbieter zu verringern.
- **Automatische Wiederherstellung:** Implementierung von Skripten zur automatischen Wiederherstellung der Datenbanken, um den Prozess der Datenwiederherstellung im Notfall zu beschleunigen.

---

