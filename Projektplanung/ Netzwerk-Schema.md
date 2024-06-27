# Netzwerk-Schema

Dieses Diagramm zeigt die Einrichtung einer lokalen MySQL-VM und das Backup auf einer AWS EC2-Instanz.

## Verfügbarkeitszone und Sicherheitsgruppen
- **Verfügbarkeitszone:** us-east-1c
- **Sicherheitsgruppen:** sg-013fa1d9c4263ff2a (launch-wizard-1)

```mermaid
graph TD
    A[Lokaler Rechner]
    B[MySQL-VM]
    C[AWS EC2-Instanz]
    D[Verfügbarkeitszone<br>us-east-1c]
    E[Sicherheitsgruppe<br>sg-013fa1d9c4263ff2a<br>(launch-wizard-1)]

    A -->|SSH| B
    B -->|SSH| C
    C -.->|Gehört zu| D
    C -.->|Unterliegt| E

    subgraph Lokale Umgebung
        A
        B
    end

    subgraph Cloud Umgebung
        C
        D
        E
    end

    B -->|Backup-Skript| C

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#9f9,stroke:#333,stroke-width:2px
    style D fill:#ffd,stroke:#333,stroke-width:2px,stroke-dasharray: 5,5
    style E fill:#ffd,stroke:#333,stroke-width:2px,stroke-dasharray: 5,5
