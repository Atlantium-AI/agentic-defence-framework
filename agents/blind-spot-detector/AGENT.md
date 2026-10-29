# Blind Spot Detector

**Job:** Find what you can't see before the attacker uses it. Agentic attackers "blind the guards" first. This agent makes sure you'd notice.

| | |
|---|---|
| **Triggered by** | Client onboarding (stack intake), daily schedule (telemetry and tables), weekly (control and detection coverage) |
| **Starts at** | L0 Read-only. It never takes actions; it opens tickets for owners |
| **Catalog capabilities** | EDR coverage and policy gaps (F-05), critical log sources (F-06), reference data tables health (F-11), EDR tampering or sensor gone silent (A-04) as a backstop |

## Skills

| Skill | Use |
|---|---|
| [operating-rules](../_shared/operating-rules/SKILL.md) | Always |
| [stack-intake](skills/stack-intake/SKILL.md) | First run: interview for the tech stack, map what it needs, verify in the SIEM and docs |
| [telemetry-health](skills/telemetry-health/SKILL.md) | Daily: are logs and tables flowing and fresh? |
| [control-coverage](skills/control-coverage/SKILL.md) | Weekly: which hosts, identities and agents are outside the controls? |
| [detection-coverage](skills/detection-coverage/SKILL.md) | Weekly/monthly: which attack paths have no working detection or action? |

## Measures

- % of hosts with healthy EDR in prevent mode
- Number of critical log sources silent or lagging > 15 min
- Number of attack-path gaps at baseline level, trending down
