\# Architecture Overview



\## Components



\### JumpServer

Centralizes privileged access and provides session information for auditing.



\### HashiCorp Vault

Stores versioned secrets, applies access policies, and supports the documented SSH certificate workflow.



\### Python Automation

Coordinates the documented password rotation workflow and processes collected session information.



\### Anomaly Detection

Uses an Isolation Forest model to identify potentially anomalous behavior in collected session data.



\### Notifications and Dashboard

The workflow can generate Discord notifications and display analysis results through Streamlit.



\### Windows Target

A Windows machine was used to validate the password rotation workflow.



\## Workflow



1\. A privileged connection is managed through JumpServer.

2\. Relevant secrets are stored and managed through Vault.

3\. The rotation workflow updates credentials and synchronizes the relevant components.

4\. Collected session data is prepared for analysis.

5\. Isolation Forest generates anomaly predictions.

6\. Results are displayed in the dashboard and notifications are generated according to the implemented workflow.



\## Security Notes



\- Do not expose internal IP addresses, credentials, tokens, or private keys.

\- Use least-privilege policies and secure token handling.

\- Treat lab settings as test configurations, not production recommendations.

\- The anomaly detection workflow must not be described as continuous real-time detection unless that behavior has been implemented and verified.

