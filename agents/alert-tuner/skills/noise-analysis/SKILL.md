---
name: noise-analysis
description: Find which detection rules generate false positives and why. Groups benign alerts by rule and by the entity, process or pattern behind them, and recommends tune, exception, fix or retire. Use weekly, or when a rule exceeds its false positive budget.
---

# Noise Analysis

## 1. Pull the data (last 30 days)

- [ ] Alerts per rule, with verdicts (benign / suspicious / malicious)
- [ ] For benign ones: the stated reason from the triage write-up

## 2. Group the benign alerts

For each noisy rule, group by: host, account, parent process, signer, command line pattern, destination.
Usually 1–3 groups make up 80% of the noise.

## 3. Decide per group

| Pattern | Recommendation |
|---|---|
| One sanctioned tool, account or host doing it repeatedly | **Exception** → `exception-drafting` |
| Rule logic too broad (matches things it was never meant to) | **Fix the rule.** Describe the logic change |
| Benign because data was missing (e.g. no change window recorded) | **Fix the data.** Route to the Data Steward |
| Rule has never caught anything real and is mostly noise | **Retire or downgrade** to a hunting query |
| Noise is real risky behavior the business accepts | **Document the risk** and keep alerting at lower priority |

## Never

- Suppress a Tier 1 high-fidelity rule (decoys, canaries, credential theft) wholesale. Only narrow, owned, expiring exceptions.
- Recommend an exception for something that also appeared in a malicious or suspicious case.

## Output

Per rule: noise volume, the top groups, recommendation, and expected false positive reduction.
