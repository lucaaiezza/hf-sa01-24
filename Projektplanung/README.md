# Projektplan

## 1. Planung und Konzeption (Woche 1)

### Ziel
Grundlegende Planung und Festlegung der Anforderungen.

### Aufgaben
- **Anforderungen festlegen**: Sammeln und Dokumentieren der Anforderungen an die Lösung.
- **Werkzeuge auswählen**: Entscheidung über die zu verwendenden Tools und Technologien (z.B. Python, Bash, mysqldump, gzip, scp/sftp, sendmail/smtplib).

## 2. Implementierung der Lösung (Woche 2-3)

### Ziel
Entwicklung und Testen des automatisierten Backup-Skripts.

### Aufgaben
- **Skripterstellung**: Schreiben eines Skripts zur automatisierten Sicherung der MySQL-Datenbanken.
- **Komprimierung**: Integration von gzip zur Komprimierung der Sicherungen.
- **Speicherung**: Einrichtung der Übertragungsmethode (scp/sftp) zur Sicherung auf einem Cloud-Server.
- **Benachrichtigungen**: Implementierung des E-Mail-Benachrichtigungssystems.
- **Testen**: Durchführung von Tests, um sicherzustellen, dass alle Komponenten korrekt funktionieren.

## 3. Dokumentation und Abschluss (Woche 4)

### Ziel
Erstellung der Dokumentation und Vorbereitung der Präsentation.

### Aufgaben
- **Technische Dokumentation**: Erstellung einer umfassenden Anleitung zur Installation, Konfiguration und Wartung der Lösung.
- **Managergerechte Dokumentation**: Erstellung einer verständlichen Dokumentation für nicht-technische Stakeholder.
- **Finale Tests**: Durchführung abschließender Tests zur Validierung der Lösung.
- **Präsentationsvorbereitung**: Erstellung der Präsentation für die Abschlussvorstellung des Projekts.



[Diagramm anzeigen](./Projket%20Diagramm.jpg)


# Swimlane-Diagramm

## Woche 1
| Rolle           | Aufgaben                                                 |
|-----------------|----------------------------------------------------------|
| **Projektmanager** | Anforderungen festlegen, Werkzeuge auswählen              |
| **Entwickler**      | Auswahl der Werkzeuge unterstützen                        |

## Woche 2-3
| Rolle           | Aufgaben                                                             |
|-----------------|----------------------------------------------------------------------|
| **Projektmanager** | Überwachung der Implementierung                                       |
| **Entwickler**      | Skripterstellung, Komprimierung, Speicherung, Benachrichtigungen        |
| **Tester**         | Testen der Funktionalität (Komprimierung, Speicherung, Benachrichtigungen) |

## Woche 4
| Rolle           | Aufgaben                                                           |
|-----------------|--------------------------------------------------------------------|
| **Projektmanager** | Überwachung der Dokumentation                                       |
| **Entwickler**      | Unterstützung bei der finalen Tests                                |
| **Tester**         | Durchführung abschließender Tests                                   |
| **Dokumentation**  | Erstellung der technischen und managergerechten Dokumentation sowie der Präsentation |

## Übersicht

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#3498db', 'edgeLabelBackground':'#ffffff', 'tertiaryColor': '#f39c12', 'nodeTextColor': '#ffffff'}}}%%
%%{wrapperClasses: 'mermaid-dark'}%%
gantt
    title Projekt-Zeitplan
    dateFormat  YYYY-MM-DD
    section Woche 1
    Anforderungen festlegen, Werkzeuge auswählen  :a1, 2024-06-01, 7d
    Auswahl der Werkzeuge unterstützen            :a2, 2024-06-01, 7d
    section Woche 2-3
    Überwachung der Implementierung               :b1, 2024-06-08, 14d
    Skripterstellung, Komprimierung, Speicherung, Benachrichtigungen :b2, 2024-06-08, 14d
    Testen der Funktionalität                      :b3, 2024-06-08, 14d
    section Woche 4
    Überwachung der Dokumentation                 :c1, 2024-06-22, 7d
    Unterstützung bei der finalen Tests           :c2, 2024-06-22, 7d
    Durchführung abschließender Tests             :c3, 2024-06-22, 7d
    Erstellung der Dokumentation und Präsentation :c4, 2024-06-22, 7d
