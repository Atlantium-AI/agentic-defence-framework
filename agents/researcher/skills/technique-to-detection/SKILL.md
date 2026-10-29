---
name: technique-to-detection
description: Turn a new attack technique or threat report into concrete proposals covering which observables to detect, in which data source, how high-fidelity it is, and which response action it should trigger. Use when a new technique, tool or campaign is reported, or after an incident reveals a missed technique.
---

# Technique to Detection

## 1. Break the technique down

- [ ] Which vector (pulled / pushed / walked) and stage (keys / guards / move / cash out)?
- [ ] What does it look like in telemetry: process, command line, file, registry, network, identity event, cloud API call?
- [ ] What's the attacker's **unavoidable** step? Detect that rather than the tool, because tools change.

## 2. Check what we already have

- [ ] Does an existing capability in the catalog already cover it? (Often yes: credential theft, recon burst, remote access tools)
- [ ] Do we collect the data source? If not, flag it to the Blind Spot Detector.

## 3. Propose

```
technique: <name, reference>
vector/stage: <...>
observable: <what to match, in plain language>
data_source: <which log source>
fidelity: <deterministic (Tier 1 candidate) | needs context (Tier 2)>
expected_noise: <what legitimate activity looks similar>
response: <response action(s), tier-aware>
test: <how to safely simulate it>
```

Deterministic + low noise → propose as a Tier 1 candidate (starts recommend-only).
Needs context → route to the triage agent as a Tier 2 detection.
