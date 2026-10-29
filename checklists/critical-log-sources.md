# Critical Log Sources

The minimum telemetry needed to detect pulled / pushed / walked-in attacks and feed the Tier 1 and Tier 2 detections. Only collect sources that exist at the client.

**Critical** = required for the baseline. Most Tier 1 automation depends on it.
**Important** = collect next. Most Tier 2 investigations need it.

Rules for every source:
- [ ] Near real-time into the SIEM (target < 5 min lag), with lag on a dashboard
- [ ] Alert when a source goes silent (silence is a "blind the guards" signal)
- [ ] Retention ≥ 90 days hot, ≥ 1 year somewhere

## Endpoints

| Source | Tier | Why |
|---|---|---|
| EDR alerts + telemetry (process, network, file, driver) | Critical | Primary view of pulled-in and post-access activity |
| EDR sensor health / heartbeat | Critical | Detects tampering and silent sensors (A-04) |
| EDR console audit (policy, exclusion, uninstall-token changes) | Critical | Attacker with console access can blind everything |
| Windows Security log on servers (logons, account/group changes, service/task creation, log cleared) | Important | Context where EDR is weak; required on servers without EDR |
| Linux auth + auditd (sudo, SSH, cron, systemd, authorized_keys) | Important | Linux servers often have weaker EDR coverage |

## Active Directory

| Source | Tier | Why |
|---|---|---|
| Domain controller Security logs: logons, Kerberos TGT/TGS requests, account and group changes, log cleared | Critical | Canary accounts (A-02), fan-out (A-09), Kerberoasting, privilege grants |
| Directory changes (GPO, ACL, AdminSDHolder, trusts) | Important | Persistence and privilege escalation |
| AD-to-cloud sync / connector logs (Entra Connect, Okta AD agent) | Important | Sync servers are a bridge from on-prem to cloud identity |

## Entra ID / Azure

| Source | Tier | Why |
|---|---|---|
| Sign-in logs (interactive, non-interactive, service principal, managed identity) | Critical | Walked-in detection; token replay shows in non-interactive |
| Audit logs (MFA method changes, app consents, role assignments, SP secrets, federation) | Critical | Identity persistence (D-03, D-06) |
| Identity Protection risk detections | Critical | Risky sign-in triage (D-02) |
| Azure Activity log (subscription control plane) | Critical (if Azure workloads) | Logging tampering, role grants, resource abuse |
| Key Vault, storage and NSG flow logs | Important | Secret access and data exfiltration |

## Microsoft 365

| Source | Tier | Why |
|---|---|---|
| Unified audit log (Exchange, SharePoint, OneDrive, Teams) | Critical | Inbox rules, mass download, internal phishing after takeover |
| Email gateway / Defender for Office (delivery, clicks, attachments) | Critical | How pulled-in attacks arrived, and who else received them |
| Mailbox audit (MailItemsAccessed) | Important | Scoping what an attacker read |

## Okta

| Source | Tier | Why |
|---|---|---|
| System Log (all events: authentication, MFA push/verify, factor enroll/reset, admin actions, session, policy changes) | Critical | Walked-in detection, MFA fatigue (A-06), new factors (D-03) |
| ThreatInsight / Identity Threat Protection events | Critical | Risk signals and universal logout events |
| Admin console and API token activity | Critical | IdP admin takeover = keys to everything |

## AWS

| Source | Tier | Why |
|---|---|---|
| CloudTrail management events, org trail, all regions | Critical | Logging tampering, IAM changes, unused-region activity |
| GuardDuty findings, all accounts and regions | Critical | Instance creds used outside AWS, credential exfiltration (A-12) |
| IAM Identity Center / SSO sign-ins | Critical | Human access into AWS |
| CloudTrail data events for sensitive S3 buckets | Important | Cash-out detection |
| VPC flow logs, Route 53 resolver logs | Important | Egress and C2 from workloads |

## Network and remote access

| Source | Tier | Why |
|---|---|---|
| DNS query logs (internal resolvers or protective DNS) | Critical | C2, AitM phishing domains, blast-radius sweeps (I-03) |
| VPN / ZTNA / remote access authentication | Critical | Walked-in via stolen creds; edge devices are a top pushed-in target |
| Firewall / proxy / secure web gateway (allow + deny) | Important | Egress, downloads, new outbound from servers (D-10) |
| Edge device admin and config logs (VPN appliances, firewalls) | Important | Pushed-in on devices that can't run EDR |
| WAF / load balancer logs for internet-facing apps | Important | Probing and exploitation of public apps |

## Other enterprise tools

| Source | Tier | Why |
|---|---|---|
| Automation platform (SOAR) audit | Critical | Containment actions must be auditable, and the platform is itself a target |
| Privileged access / password vault (check-outs, policy changes) | Important | Credential theft at scale |
| RMM / remote support tools (sessions, new devices) | Important | Common walked-in path via compromised vendor |
| Help desk tickets (password and MFA resets) | Important | Correlate resets with suspicious sign-ins (D-03) |
| Code platform and CI/CD (GitHub/GitLab audit, secret scanning) | Important | Secrets, pipeline abuse, OIDC trust abuse |
| AI agent platforms: prompts, tool calls, actions taken, MCP/connector changes (Copilot, low-code agents, coding assistants, custom agents) | Critical (for agents that can write or execute) | Prompt-injected agents are a pulled-in path; without these logs you can't tell what the agent did or why |
| Key SaaS (Salesforce, Workday, Google Workspace, etc.) | Important | Cash-out and data theft after identity takeover |
