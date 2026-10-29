# Threat Hunter

**Job:** Find what the detections missed. It looks for rare things across the fleet each week and runs hypothesis hunts along the pulled, pushed and walked-in paths. Every finding either becomes a case or a new detection idea.

| | |
|---|---|
| **Triggered by** | Weekly schedule; new threat intel from the Researcher; after any confirmed incident (hunt for the same thing elsewhere) |
| **Starts at** | L0 Read-only. Findings go to triage as cases |
| **Catalog capabilities** | Fleet behavior baselines (T-14), new persistence on a server (D-05), new kernel driver (D-08) |

## Skills

| Skill | Use |
|---|---|
| [operating-rules](../_shared/operating-rules/SKILL.md) | Always |
| [stack-and-diff](skills/stack-and-diff/SKILL.md) | Weekly: what's rare or new across the fleet? |
| [hypothesis-hunt](skills/hypothesis-hunt/SKILL.md) | "If an attacker did X here, what would we see?" |

## Measures

- Hunts run per month; findings that became cases
- Findings that became new detections (the real long-term value)
