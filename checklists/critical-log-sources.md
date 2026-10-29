# Critical Log Sources

80/20 rule: a log is **critical** only if a detection or response in the 90-day plan can't work without it. Only collect sources the company actually has.

| Tier | Meaning | When |
|---|---|---|
| **Must have** | A day-90 detection or response depends on it | Days 1–30 |
| **Collect now** | No day-90 detection, but investigations need it | Days 1–60 |
| **Later** | Powers next-quarter detections | After day 90 |

Rules for every Must have and Collect now source:
- [ ] Near real-time into the SIEM (target < 5 min lag), with lag on a dashboard
- [ ] Alert when the source goes silent (silence is a "blind the guards" signal)
- [ ] ≥ 90 days searchable retention

## Must have

| Source | Examples | Powers |
|---|---|---|
| **EDR alerts and telemetry** (process, file, network) | Any EDR | Credential theft, machine-speed recon, ransomware precursors, server spawning a shell, EDR alert triage |
| **EDR sensor health / heartbeat** | EDR sensor status | EDR tampering / silent sensor |
| **A second "host is alive" source** | DHCP leases, internal DNS queries, or IdP device sign-ins | Tells a silent-but-online host (tampering) from a switched-off laptop |
| **EDR console audit** | Policy, exclusion, uninstall-token changes | Catches an attacker blinding the EDR from the console |
| **Decoy file read auditing** | EDR file-read telemetry, or Windows object-access auditing (SACL) on decoys | Decoy tripwires. File reads aren't logged by default |
| **Identity provider sign-ins, including MFA events** | Entra sign-in logs (interactive + non-interactive), Okta System Log | MFA fatigue, risky sign-in triage, token replay |
| **Identity provider audit** | MFA method changes, role changes, app consents | MFA fatigue response, identity investigations |
| **Identity provider risk detections** | Entra ID Protection, Okta ThreatInsight / Identity Threat Protection | Risky sign-in triage |
| **Domain controller security logs** | Logons, Kerberos ticket requests, account/group changes, log cleared | Canary account hits, credential and lateral movement context |
| **Edge device admin and config logs** | Admin sign-ins, config/firmware changes, admin account changes on VPNs and firewalls | Edge device admin or config change ([edge plan](edge-devices.md)) |
| **Cloud control plane** (if there are cloud workloads) | AWS CloudTrail (org trail, all regions), Azure Activity log | Cloud logging tampering, new cloud admin/keys, root and break-glass use |
| **Cloud threat detection findings** (if there are cloud workloads) | GuardDuty, Defender for Cloud, on in all accounts and regions | Cloud credentials used outside the provider, unused-region activity |

## Collect now

| Source | Examples | Why |
|---|---|---|
| **DNS query logs** | Internal resolvers or protective DNS | Proves network blocks work; blast radius ("who else talked to this domain?") |
| **Email gateway logs** | Delivery, clicks, attachments | "How did it get here, and who else got it?" for pulled-in attacks |
| **VPN / remote access sign-ins** | VPN, ZTNA, VDI gateways | Walked-in investigations; edge devices are a top exploitation target |
| **AI agent platform logs** (agents that can write or execute) | Prompts, tool calls, actions taken, connector/MCP changes | The only record of what a misbehaving agent did |
| **Automation platform audit** | Every automated action, who/what triggered it, undo | Containment must be auditable and reversible |

## Later (next quarter)

| Source | Powers |
|---|---|
| Microsoft 365 unified audit log / SaaS audit logs | Inbox rules, mass download, external sharing after account takeover |
| Cloud identity sign-ins (e.g. AWS IAM Identity Center) | Human access into cloud consoles |
| Firewall / proxy / web gateway | Server egress, downloads, exfiltration volume |
| WAF / load balancer logs | Probing and exploitation of public apps |
| Windows Security logs on member servers, Linux auditd | Persistence detections where EDR is weak |
| AD-to-cloud sync / connector logs | Abuse of identity sync servers |
| Privileged access vault, RMM tools, help desk tickets | Credential theft at scale, vendor access, reset correlation |
| Code platform and CI/CD audit | Secrets and pipeline abuse |
| Key SaaS (CRM, HR, file sharing) | Data theft after identity takeover |
