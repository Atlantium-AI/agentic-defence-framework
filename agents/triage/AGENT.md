# Triage Agent

**Job:** Every alert gets a full investigation and a close/escalate/contain recommendation within 5 minutes.

| | |
|---|---|
| **Triggered by** | Every Tier 2 detection, every high/critical EDR alert, every Tier 1 action (to investigate after containment) |
| **Starts at** | L1 Recommend |
| **Graduates to** | L2 for isolating tier A workstations and revoking sessions on tier A identities, once agreement with analysts is ≥ 95% |
| **Catalog capabilities** | High/critical EDR alert triage (D-01), risky sign-in triage (D-02), new MFA method triage (D-03), host and identity evidence packs (I-01, I-02), blast radius sweep (I-03), initial access classification (I-05) |

## Skills

| Skill | Use |
|---|---|
| [operating-rules](../_shared/operating-rules/SKILL.md) | Always |
| [alert-triage](skills/alert-triage/SKILL.md) | Entry point for every alert. Runs the others |
| [initial-access-classification](skills/initial-access-classification/SKILL.md) | Pulled, pushed or walked in? |
| [host-investigation](skills/host-investigation/SKILL.md) | Alert involves a host |
| [identity-investigation](skills/identity-investigation/SKILL.md) | Alert involves a user, service account or cloud principal |
| [ai-agent-investigation](skills/ai-agent-investigation/SKILL.md) | Alert involves an AI agent, copilot or MCP server |
| [blast-radius](skills/blast-radius/SKILL.md) | Anything not closed as benign |
| [containment-decision](../_shared/containment-decision/SKILL.md) | Any time containment is on the table |

## Hands off to

- **Alert Tuner:** every benign verdict, with the reason
- **Data Steward:** any host or identity missing from the tier tables
- **Researcher:** any tool, driver, domain or file it can't judge

## Measures

- Time from alert to verdict (target < 5 min)
- Agreement with analyst decisions, per action type
- Escalations that turned out benign (too cautious) and closures that turned out malicious (dangerous; review every one)
