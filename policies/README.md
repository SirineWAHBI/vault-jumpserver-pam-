\# Vault Policies and AppRole



This directory documents the Vault access policies used by the PAM laboratory.



\## Security Principles



\- Apply least-privilege permissions.

\- Restrict access to the required secret paths and operations.

\- Keep Role IDs and Secret IDs out of source code and version control.

\- Avoid embedding administrative tokens in automation scripts.

\- Use short-lived credentials and appropriate token policies where supported.



\## Policy Files



Add only policy files that have been recovered or reconstructed and reviewed.



Any reconstructed policy must be validated in a disposable test environment before use. This documentation does not grant permissions or configure Vault by itself.

