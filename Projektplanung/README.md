# Scrum-Projektplanung für die Automatisierte Datenbank-Backup-Lösung mit AWS

## Rollen

- **ICH(LucaAiezza):** übernehme die Rolle des Product Owners, Scrum Masters und des Entwicklungsteams.

## Artefakte

- **Product Backlog:** Liste aller Aufgaben und Anforderungen, die im Projekt erledigt werden müssen.
- **Sprint Backlog:** Auswahl von Aufgaben aus dem Product Backlog, die im aktuellen Sprint bearbeitet werden.
- **Inkrement:** Das am Ende jedes Sprints fertige Produkt, das einen wertvollen Fortschritt darstellt.

## Zeremonien

- **Sprint Planning:** Planen des kommenden Sprints und Auswahl der Aufgaben aus dem Product Backlog.
- **Daily Stand-up:** Tägliches kurzes Meeting (Selbstreflexion), um den Fortschritt zu überprüfen und Hindernisse zu identifizieren.
- **Sprint Review:** Präsentation der Arbeitsergebnisse am Ende des Sprints und Überprüfung, ob die Ziele erreicht wurden.
- **Sprint Retrospective:** Reflexion über den letzten Sprint und Identifikation von Verbesserungsmöglichkeiten.

## Sprint-Zyklus

Das Projekt wird in mehreren zweiwöchigen Sprints durchgeführt.

## Planung der Sprints

### Sprint 1: Einrichtung der Infrastruktur (2 Wochen)

**Ziele:**

- Erstellen der VMs mit Multipass.
- Installation und Konfiguration von MySQL auf der `db-vm`.
- Einrichtung des AWS S3-Buckets.

**Aufgaben:**

- Multipass-VMs erstellen.
- MySQL-Installation und Konfiguration.
- AWS S3-Bucket erstellen.

### Sprint 2: Entwicklung des Backup-Skripts (2 Wochen)

**Ziele:**

- Entwicklung des Bash-Skripts zum Sichern der Datenbank.
- Testen und Verifizieren des Skripts.

**Aufgaben:**

- Backup-Skript entwickeln.
- Skript testen und Fehler beheben.

### Sprint 3: Automatisierung und Wartung (2 Wochen)

**Ziele:**

- Automatisierung des Backup-Skripts mittels Cron-Jobs.
- Implementierung von Benachrichtigungen bei erfolgreichem und fehlgeschlagenem Backup.

**Aufgaben:**

- Cron-Job einrichten.
- E-Mail-Benachrichtigungen implementieren und testen.

### Sprint 4: Dokumentation und Abschluss (2 Wochen)

**Ziele:**

- Erstellung der Projektdokumentation.
- Review und Anpassung der Lösung basierend auf Feedback.

**Aufgaben:**

- Dokumentation erstellen.
- Review und Anpassungen vornehmen.

## Scrum Meetings

1. **Sprint Planning:**
   - Definition der Sprint-Ziele.
   - Auswahl der User Stories und Aufgaben aus dem Product Backlog.
2. **Daily Stand-ups:**
   - Selbstreflexion: Was wurde seit dem letzten Stand-up erreicht?
   - Was wird bis zum nächsten Stand-up erledigt?
   - Gibt es irgendwelche Hindernisse?
3. **Sprint Review:**
   - Präsentation der Arbeitsergebnisse.
   - Überprüfung, ob die Ziele erreicht wurden.
4. **Sprint Retrospective:**
   - Was lief gut?
   - Was könnte verbessert werden?
   - Aktionspunkte für den nächsten Sprint.

## Zusammenfassung

Durch die Anwendung von Scrum wird das Projekt in klar definierte Sprints unterteilt, was eine kontinuierliche Lieferung von Fortschritten ermöglicht und Flexibilität bei der Anpassung an neue Anforderungen bietet. Die regelmäßigen Meetings und Reviews stellen sicher, dass du auf dem richtigen Weg bleibst und potenzielle Probleme frühzeitig erkannt und behoben werden.
