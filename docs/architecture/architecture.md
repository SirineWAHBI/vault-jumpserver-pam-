\# Architecture



\## Overview



The lab consists of three virtual machines (JumpServer, Vault, and a Linux target) and one physical Windows target. JumpServer is the single entry point for administrators. Vault manages secrets, while Python automation coordinates password rotation and credential synchronization and supports the anomaly-detection workflow.



```mermaid

flowchart LR

&#x20;   U\[Administrator] -->|Web or SSH| JS\[JumpServer]

&#x20;   JS -->|Managed sessions| LT\[Linux target]

&#x20;   JS -->|Managed sessions| WT\[Physical Windows target]

&#x20;   V\[HashiCorp Vault] -->|Rotated secrets| LT

&#x20;   V -->|Rotated secrets| WT

&#x20;   A\[Python automation] --> V

&#x20;   A --> JS

&#x20;   JS -->|Session API| ML\[Isolation Forest]

&#x20;   ML --> D\[Streamlit dashboard]

&#x20;   ML --> N\[Discord notifications]

```



\## Components



| Component | Role | Notes |

|---|---|---|

| JumpServer Community Edition | Bastion for users, assets, authorization rules, session recording, and command filtering | Selected after comparing Teleport, Guacamole with Keycloak, and Boundary |

| HashiCorp Vault | KV v2 secrets, SSH certificate authority, and AppRole authentication | File storage and Shamir unseal configuration (3 of 5) |

| Python automation | Password rotation, synchronization with JumpServer, and alerting | Uses `hvac`, `requests`, `paramiko`, and HTTP-signature authentication |

| Anomaly detection | Scores sessions using Isolation Forest | Batch pipeline operating on collected sessions |

| Dashboard and alerts | Streamlit dashboard and Discord notifications | Alerts identify sessions flagged as unusual |

| Targets | Ubuntu Linux and a physical Windows host with OpenSSH | The Windows host is used to validate multi-OS portability |



\## Workflows



\### 1. Privileged Session



```mermaid

sequenceDiagram

&#x20;   participant U as Administrator

&#x20;   participant J as JumpServer

&#x20;   participant T as Target

&#x20;   U->>J: Sign in

&#x20;   J->>J: Check login ACL, MFA where configured, authorization, and time limits

&#x20;   U->>J: Open a session on an asset

&#x20;   J->>T: Connect using the stored account secret

&#x20;   J-->>U: Provide terminal session

&#x20;   J->>J: Record session and apply configured command filtering

```



\### 2. Password Rotation



```mermaid

sequenceDiagram

&#x20;   participant S as Automation script

&#x20;   participant V as Vault

&#x20;   participant T as Target

&#x20;   participant J as JumpServer

&#x20;   S->>V: Authenticate using AppRole

&#x20;   S->>T: Change password

&#x20;   T-->>S: Return operation result

&#x20;   S->>V: Store new version in KV v2 after success

&#x20;   S->>J: Update account secret using signed API request

&#x20;   Note over S: On failure, stop the run and send a Discord alert

```



The intended order is \*\*target first, then Vault, then JumpServer\*\*. This avoids updating Vault before the target password has been changed successfully. JumpServer is updated last to keep its stored account credential synchronized.



\### 3. Anomaly Detection



1\. Extract collected sessions through the JumpServer API.

2\. Convert durations and timestamps into numeric values.

3\. Build features such as hour, night, weekend, duration, and user frequency.

4\. Score sessions using an Isolation Forest model configured with 200 trees.

5\. Send alerts for real sessions flagged as unusual and update the dashboard.



This pipeline runs after sessions have been collected. It is not continuous, real-time detection.



\## Design Decisions



| Decision | Reason |

|---|---|

| Separate Vault and JumpServer | Provides specialized secret management and separates access management from secret storage |

| AppRole instead of a root token | Limits automation to the permissions granted by its policy |

| KV v2 | Maintains version history for secrets updated during password rotation |

| Isolation Forest | Supports anomaly detection without requiring labeled training data |

| Target, then Vault, then JumpServer | Updates the target first and synchronizes the stored credentials afterward |





