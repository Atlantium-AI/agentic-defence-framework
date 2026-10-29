---
name: graduation-review
description: Decide whether a detection rule is ready to move from recommend-only to automatic containment, or whether an agent has earned a higher permission level for an action type. Uses false positive history and agent-analyst agreement. Use at the end of a rule's recommend-only period or monthly for agent permissions.
---

# Graduation Review

## Rule: recommend-only → automatic

All must be true:
- [ ] Ran in recommend-only for at least 2 weeks with real traffic
- [ ] False positives over that period ≤ 1 per month projected, or each one is now covered by a narrow exception
- [ ] The response action is tested, reversible and tier-aware
- [ ] Rate limit configured
- [ ] Rule fired at least once (real or simulation) with the expected action recommended
- [ ] Listed in the signed containment authority policy

If any fail: say which, and what would fix it.

## Agent: permission level for an action type

For one action type (e.g. "isolate tier A workstation"):
- [ ] At least 30 recommendations of that action type reviewed by an analyst
- [ ] ≥ 95% agreement (agent recommended it and the analyst ran it, or both declined)
- [ ] **Zero** harmful recommendations (would have isolated a critical system, or disabled the wrong person)
- [ ] Every disagreement reviewed, and the cause fixed (skill change, data fix, exception)

| Result | Recommendation |
|---|---|
| All pass | Promote one level for that action type and tier only |
| Agreement 85–95% | Stay; list the disagreement patterns to fix |
| Any harmful recommendation | Stay or demote; root-cause before the next review |

## Output

A one-page recommendation for the containment approver: what to promote, the evidence, and the rollback plan.
