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

    classDef info fill:#fff,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5;
    
    D[Verfügbarkeitszone: us-east-1c]
    E[Sicherheitsgruppen: sg-013fa1d9c4263ff2a (launch-wizard-1)]
    
    D -.-> C
    E -.-> C
