# Netzwerk-Schema

Dieses Diagramm zeigt die Einrichtung einer lokalen MySQL-VM und das Backup auf einer AWS EC2-Instanz.

```mermaid
graph TD
    A[Lokaler Rechner]
    B[MySQL-VM]
    C[AWS EC2-Instanz]

    A -->|SSH| B
    B -->|SSH| C

    subgraph Lokale Umgebung
        A
        B
    end

    subgraph Cloud Umgebung
        C
    end

    B -->|Backup-Skript| C

    style A fill:#f9f,stroke:#333,stroke-width:4px
    style B fill:#bbf,stroke:#333,stroke-width:4px
    style C fill:#9f9,stroke:#333,stroke-width:4px
