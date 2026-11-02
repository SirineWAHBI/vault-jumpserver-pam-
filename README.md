Privileged Access Management with JumpServer and HashiCorp Vault

A laboratory implementation of a Privileged Access Management (PAM) solution, built during a cybersecurity engineering internship in 2026. It centralizes and audits privileged access, keeps secrets in HashiCorp Vault, rotates passwords automatically, and flags unusual sessions with a machine-learning model.

Problem

Privileged accounts are high-value targets. Static passwords, shared administrator accounts and missing session traces make misuse hard to prevent and hard to investigate. The project addresses one question:



How can privileged access be controlled and secured, secrets protected, sessions traced, and unusual user behavior detected automatically?

Highlights

Area

What was built

Access control

Named users with MFA, authorization rules, login ACLs, idle and maximum session time

Command filtering

Real-time blocking of risky commands (reboot, shutdown, rm -rf, mkfs, dd)

Audit

Recorded and replayable sessions, with a per-command history

Secrets

Vault KV v2 with a new version for every rotation

Least privilege

Automation authenticates through Vault AppRole with a narrow policy

Rotation

Target host, then Vault, then JumpServer; failures raise a Discord alert

SSH certificates

Short-lived certificates signed by a Vault CA

Anomaly detection

Isolation Forest on session features, with dashboard and alerts

Multi-OS

The rotation workflow validated on a Linux and a Windows target



Architecture

flowchart LR

&#x20;   U\[Administrator] -->|web or SSH| JS\[JumpServer<br/>access, sessions, audit]

&#x20;   JS -->|managed sessions| T\[Targets<br/>Linux and Windows]

&#x20;   V\[HashiCorp Vault<br/>KV v2, SSH CA, AppRole] -->|rotated secrets| T

&#x20;   A\[Python automation] -->|read and write secrets| V

&#x20;   A -->|synchronize credentials| JS

&#x20;   JS -->|session data via API| ML\[Isolation Forest]

&#x20;   ML --> D\[Streamlit dashboard]

&#x20;   ML --> N\[Discord alerts]

&#x20;   A --> N



Full description: docs/architecture/architecture.md.

Results at a glance

Metric

Value

Sessions analyzed

361 (46 real, 315 simulated)

Anomalies flagged

18

Flagged among real sessions

7 of 46

Injected anomalies detected

11 of 15 (73 %)





The 73 % figure is measured on anomalies injected into simulated data, and the real sessions come from the lab itself. It shows that the pipeline works end to end; it does not predict production performance. Details: docs/results.md.

Repository contents

.

├── docs/

│   ├── architecture/architecture.md

│   ├── results.md

│   └── screenshots/

├── policies/           

├── .env.example

├── .gitignore

└── README.md

Screenshots

JumpServer access control

Vault secret management

!\[Access control](docs/screenshots/01-jumpserver-access-control.png) 

!\[Vault](docs/screenshots/02-vault-secret-management.png) 





Anomaly dashboard

Discord alert

!\[Anomaly detection](docs/screenshots/03-anomaly-detection.png) 

!\[Discord alert](docs/screenshots/04-discord-alert.png) 



Context

Engineering internship, Société Tunisienne de l'Électricité et du Gaz (STEG), ESPRIT, academic year 2025–2026.



