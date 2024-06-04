
# Projekt Umsetzung

## Herangehensweise

In diesem Projekt wurde die gesamte Aufgabe in verschiedene Teile unterteilt, um das Verständnis zu erleichtern und die Schwierigkeit schrittweise zu steigern.

Zunächst wurde darauf geachtet, dass alle Tests auf einer "lokalen VM" durchgeführt werden können. 

### Lokale Tests

Dazu wurde Multipass verwendet. Als erstes wurden zwei VMs erstellt: eine für die Datenbank und eine für den Cloud-Backup-Server. Diese wurden ebenfalls auf Multipass erstellt, um die technische Umsetzung zu verstehen, bevor alles in die Cloud migriert wurde. 

Somit wurde das gesamte Projekt lokal getestet.

[Hier ist ein Link zur gesamten lokalen Umsetzung](./PartTOW.md)

## Umsetzung in die Cloud

Nachdem alle lokalen Tests erfolgreich abgeschlossen waren, wurde die Backup-VM in AWS erstellt, um das Datenbank-Backup dort zu speichern, anstatt lokal.

[Hier ist ein Link zur vollständigen Dokumentation der Cloud-Umsetzung](./Multipass-AWS.md)

---

