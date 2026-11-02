\# Vault Policies and AppRole



This directory documents the access model used by the PAM laboratory. The files in this directory are documentation and do not configure Vault automatically.



\## 1. Files



| File | Purpose |

|---|---|

| `ssh-rotation-policy.hcl` | Policy attached to the AppRole used by the password-rotation script. Reconstructed from the project documentation; the original file was not recovered. |



\## 2. Security Principles



\- Grant only the paths and operations required by the script.

\- Avoid broad wildcards, delete permissions, and administrative paths.

\- Keep Role IDs and Secret IDs out of source code and version control.

\- Never embed administrative tokens in automation scripts.

\- Use short-lived credentials and restrictive policies.



\## 3. AppRole Settings Used in the Lab



| Setting | Value |

|---|---|

| Role name | `ssh-rotation-script` |

| Attached policy | `ssh-rotation-policy` |

| Token TTL | 1 hour |

| Token maximum TTL | 4 hours |

| Secret ID TTL | 1 hour 30 minutes |

| Secret ID uses | 10 |



\## 4. Vault Configuration Commands



Run these commands only in a test environment where you have permission to configure Vault.



```bash

vault policy write ssh-rotation-policy policies/ssh-rotation-policy.hcl



vault auth enable approle



vault write auth/approle/role/ssh-rotation-script \\

&#x20; token\_policies="ssh-rotation-policy" \\

&#x20; token\_ttl=1h \\

&#x20; token\_max\_ttl=4h \\

&#x20; secret\_id\_ttl=90m \\

&#x20; secret\_id\_num\_uses=10

```



\*\*Important:\*\* These commands create or configure Vault resources. They do not reconstruct the missing policy file automatically. The policy must exist at the specified path before the first command is run.



\## 5. Validation Checklist



\- \[ ] Authentication with the Role ID and Secret ID returns a token.

\- \[ ] Reading and updating the two authorized secret paths succeeds.

\- \[ ] Reading unauthorized paths is denied.

\- \[ ] Enabling a secrets engine is denied to the restricted role.

\- \[ ] Listing unauthorized parent paths is denied.

\- \[ ] An expired or exhausted Secret ID is rejected.





