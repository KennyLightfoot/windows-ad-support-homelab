# Troubleshooting Scenarios

For each scenario:

1. Restore or clone the clean snapshot.
2. Open a ticket using the reported symptom only.
3. If another person is available, have them apply the fault and keep the cause hidden.
4. Diagnose with the troubleshooting method.
5. Record commands, tests, results, root cause, repair, and verification.
6. Restore the intended baseline afterward.

| ID | User report | Skill area | Fault options for facilitator |
|---|---|---|---|
| 01 | “The internet is down on my computer.” | Scope, IP, gateway | Disabled adapter, wrong gateway, lost DHCP lease |
| 02 | “Websites work by address, but names do not.” | DNS | Wrong client DNS, stopped resolver, blocked DNS |
| 03 | “I can sign in, but the shared folder says access denied.” | Identity/permissions | Wrong group, stale token, share/NTFS mismatch |
| 04 | “My password suddenly stopped working.” | Account support | Locked account, expired password, wrong domain context |
| 05 | “The internal application will not open.” | Service/network | Stopped service, blocked port, full disk |
| 06 | “Only the printer is offline.” | Device/network | Wrong address, queue stopped, unreachable simulated device |
| 07 | “No one can sign in to the domain.” | Critical service | DNS/DC service issue, time problem, unreachable DC |
| 08 | “The computer became slow after an update.” | Endpoint/logs | Resource pressure, pending restart, failed service |
| 09 | “A former employee may still have access.” | Offboarding/security | Active account, missed group, unrevoked simulated access |
| 10 | “Monitoring shows the server is unavailable.” | Alert validation | Host down, service down, monitoring path failure |

Do not intentionally introduce malware, expose services publicly, or run destructive commands. A scenario should always have a known rollback method.
