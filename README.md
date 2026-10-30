\# PAM Solution with JumpServer and HashiCorp Vault



\## Overview



This project documents a Privileged Access Management (PAM) solution developed during a cybersecurity engineering internship in 2026.



It combines centralized privileged access management, secrets management, password rotation automation, SSH certificate workflows, and behavioral anomaly detection.



The anomaly detection evaluation reported in the project documentation includes simulated anomalous sessions. The reported detection rate must be interpreted within that test dataset.



\## Objectives



\- Centralize privileged access and session auditing.

\- Protect secrets with HashiCorp Vault.

\- Automate password rotation and credential synchronization.

\- Apply least-privilege access through Vault AppRole.

\- Explore SSH certificate-based authentication.

\- Analyze collected sessions with Isolation Forest.

\- Present detection results through a dashboard and notifications.

\- Validate the password rotation workflow against a Windows target.



\## Architecture



The documented solution uses JumpServer, HashiCorp Vault, Python automation, an anomaly detection workflow, and a Windows target environment.



See \[Architecture](docs/architecture/architecture.md) for the component overview and workflow.



\## Main Features



\- Privileged access management with JumpServer.

\- Versioned secret storage with Vault KV v2.

\- Restricted access using Vault AppRole.

\- Password rotation automation.

\- SSH certificate workflow documentation.

\- Session analysis using Isolation Forest.

\- Discord notifications, where configured.

\- Streamlit monitoring dashboard.

\- Windows password rotation validation.



\## Technologies



JumpServer, HashiCorp Vault, Python, Isolation Forest, Streamlit, REST APIs, Linux, Windows.



\## Results



The project documentation covers the following outcomes:



Configuration of privileged access management and access controls.



Integration of Vault for secret management and restricted access.



Password rotation workflow validation against a Windows target.



Analysis of collected session data using Isolation Forest.



Presentation of anomaly detection results through a dashboard and notifications.



The anomaly detection results are specific to the evaluated dataset, which includes simulated sessions. They should not be interpreted as a guarantee of detection performance in production.



See Detailed Results for more information.



\## Screenshots



\### JumpServer — Access Control

!\[Access control](docs/screenshots/01-jumpserver-access-control.png)



\### Vault — Secret Management

!\[Vault](docs/screenshots/02-vault-secret-management.png)



\### Anomaly Detection

!\[Anomaly detection](docs/screenshots/03-anomaly-detection.png)



\### Discord Alerts

!\[Discord alert](docs/screenshots/04-discord-alert.png)



\## Context



Cybersecurity engineering internship project, 2026.

