---
name: classify-asset
description: Work out what an unclassified server or host does, propose its containment tier (A auto-isolate, B contain-lite, C never isolate), find a likely owner and draft the owner question. Use for hosts the deterministic tiering script couldn't classify.
---

# Classify an Asset

## 1. Gather

- [ ] `get_host`: OS, OU, name, IP/segment, uptime, last logged-on users
- [ ] Listening ports and running services/processes
- [ ] Who logs on to it, and how (interactive admins vs. service accounts)
- [ ] Who connects to it: many clients (shared service) or few?
- [ ] Is it internet-facing (exposure inventory)? Production or non-production signals (name, OU, segment)?

## 2. Identify the role

| Signals | Likely role | Proposed tier |
|---|---|---|
| Domain services, Kerberos/LDAP listeners | Domain controller | C |
| Hypervisor or virtualization management | Virtualization | C |
| IdP connector, sync, federation | Identity infrastructure | C |
| Backup agent server, storage management | Backup | C |
| Security tools (EDR, SIEM, scanner, PAM) | Security infrastructure | C |
| OT/medical/building segment | Operational technology | C |
| Database engine, many app-server clients | Production database | B |
| Web/app server, internet-facing | Production app | B |
| dev/test/qa/stage/lab in name, OU or segment | Non-production | A |
| No logons or connections in 30+ days | Possibly abandoned | A, and ask if it can be retired |

## 3. Find the owner

Most frequent interactive admin, group owning the OU, app name in the hostname, ticket history.

## 4. Propose

```
host: <name>
proposed_tier: <A|B|C>
role: <role>
confidence: <high|medium|low>
evidence: <the signals above>
likely_owner: <name/team>
owner_question: "If we see credential theft on <host> (<role>), can we cut its network in under 60 seconds without asking you?"
```

Submit via `propose_change(asset-tiers, ...)`. Proposals for tier C need human approval; low confidence stays B.
