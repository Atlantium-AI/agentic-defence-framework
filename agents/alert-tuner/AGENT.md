# Alert Tuner

**Job:** Make detections quiet enough to automate. It cuts noise with the narrowest possible exceptions, and decides when a rule has earned the move from recommend-only to automatic.

| | |
|---|---|
| **Triggered by** | Every benign verdict from triage; weekly review of the noisiest rules; end of each rule's recommend-only period |
| **Starts at** | L1 Recommend. Exceptions go through `propose_change`; a human approves every one |
| **Catalog capabilities** | Containment exceptions and allowlists table (T-03); the recommend-only period of every Tier 1 rule |

## Skills

| Skill | Use |
|---|---|
| [operating-rules](../_shared/operating-rules/SKILL.md) | Always |
| [noise-analysis](skills/noise-analysis/SKILL.md) | Find which rules and entities produce the false positives |
| [exception-drafting](skills/exception-drafting/SKILL.md) | Write the narrowest exception that removes a false positive |
| [graduation-review](skills/graduation-review/SKILL.md) | Decide if a rule or an agent action type is ready for automation |

## Measures

- False positives per rule per week
- % of exceptions that are narrow (hash/host/account-scoped, not wildcards)
- Rules graduated to automatic
