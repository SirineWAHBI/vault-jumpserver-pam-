# Results

All results come from the lab described in [Architecture](../architecture/architecture.md). The lab has been dismantled. Screenshots in `docs/screenshots/` provide visual evidence of selected configurations and results.

## 1. Access Control and Audit (JumpServer)

| Feature | Configuration | Observed Result |
|---|---|---|
| Named user | MFA enabled and dedicated authorization rule | Asset visible and reachable from the user's workbench |
| Idle limit | 5 minutes | Session closed automatically after inactivity |
| Login ACL | Reject rule for weekend days | Sign-in refused with an ACL message before the password check |
| Command filter | Reject rules for `rm -rf`, `shutdown`, `reboot`, `mkfs`, and `dd` | `reboot` blocked in real time |
| Audit | Session recording and command log | Sessions replayable; each command listed with its risk level |
| Key-based account | Separate account using an SSH key | Connection works from the CLI and from JumpServer |

## 2. Secrets Management (HashiCorp Vault)

| Item | Result |
|---|---|
| Operating mode | Production mode with file storage and Shamir unsealing (3 of 5 shares) |
| KV v2 | Each rotation adds a version; metadata shows version history |
| SSH certificate authority | A signed certificate is accepted and refused after its validity period expires |
| AppRole | Authentication uses a Role ID and Secret ID; the resulting token is restricted by a policy |
| Negative tests | Unauthorized path access, enabling a secrets engine, and listing the parent path were refused |

## 3. Password Rotation

| Target | Result |
|---|---|
| Linux | Target, Vault, and JumpServer updated in one run |
| Windows (physical host with OpenSSH) | Same workflow validated; the password-change command differs (`Set-LocalUser`) |
| Failure handling | An unreachable target address stopped the run and triggered a Discord alert |

## 4. Anomaly Detection

| Metric | Value |
|---|---:|
| Sessions analyzed | 361 (46 real and 315 simulated) |
| Anomalies flagged | 18 |
| Real sessions flagged | 7 of 46 |
| Injected anomalies detected | 11 of 15 (73.3%) |

## 5. Interpretation

The 73.3% detection rate was measured against anomalies deliberately injected into the simulated dataset. The number of real sessions was limited, and these sessions were generated during laboratory activities. Some flagged sessions therefore reflected late-night testing rather than malicious behavior.

The results demonstrate that the pipeline operates from session extraction through anomaly scoring and alerting. However, they do not establish production-level detection accuracy.

The anomaly detection pipeline processes collected sessions in batches; it is not a real-time detection system.

