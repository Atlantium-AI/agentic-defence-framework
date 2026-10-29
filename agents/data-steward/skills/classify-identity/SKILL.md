---
name: classify-identity
description: Work out whether an unclassified account is a human, service account, AI agent identity or break-glass account, propose its containment tier and find an owner. Use for accounts not in the HR feed or not yet in the identity tier table.
---

# Classify an Identity

## 1. Gather

- [ ] `get_identity`: creation date, OU, description, groups, admin roles, MFA methods, password age
- [ ] `get_signins` 30 days: interactive vs. non-interactive, MFA used?, source hosts and IPs, time-of-day pattern
- [ ] In HR? Manager?
- [ ] Hosts it logs on to, and logon types

## 2. Identify the type

| Signals | Type |
|---|---|
| In HR, interactive sign-ins with MFA, working-hours pattern | Human |
| Not in HR, logs on only from servers, fixed schedule, no MFA, old password | Service account |
| OAuth app or workload identity tied to an AI platform, agent, copilot or MCP connector | AI agent |
| Highly privileged, almost never used, excluded from Conditional Access | Break-glass |
| Not in HR, no sign-ins in 90 days | Stale: propose disabling |

## 3. Propose the tier

| Type | Default tier |
|---|---|
| Human, standard | A |
| Human, executive or admin | B |
| AI agent | A for revoking sessions (the agent kill switch); its owner is paged |
| Service account | C until an owner confirms impact |
| Break-glass | C |

## 4. Output

```
identity: <name>
type: <human|service|agent|break-glass|stale>
proposed_tier: <A|B|C>
confidence: <high|medium|low>
evidence: <signals>
likely_owner: <name/team>
```

Submit via `propose_change(identity-tiers, ...)`. Never propose disabling a service account without its owner.
