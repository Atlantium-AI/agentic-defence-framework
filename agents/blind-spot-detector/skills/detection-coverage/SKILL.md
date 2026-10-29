---
name: detection-coverage
description: Map what is actually detected and contained against the pulled, pushed and walked-in attack paths and the capability catalog. Finds attack paths with no working detection, rules that never fire, and detections with no response action. Use weekly or monthly, and after onboarding a new client.
---

# Detection Coverage

## 1. Build the current picture

For each capability in the client scorecard marked done or in-progress:
- [ ] Does the rule exist and is it enabled?
- [ ] When did it last fire (real or test)? A rule that has **never** fired is unproven.
- [ ] Does it have a working response action, and was the action last tested within 90 days?
- [ ] Do its data sources pass `telemetry-health` right now?

## 2. Walk the attack paths

For each vector, walk the stages and ask "would we see it, and would we stop it?"

| Stage | Pulled in | Pushed in | Walked in |
|---|---|---|---|
| Entry | Office/browser/agent spawns a payload | Server spawns a shell; edge device change | Risky sign-in, new MFA method, MFA burst |
| Keys | Credential theft, decoy read | same | Token replay, new OAuth grant |
| Guards | EDR tamper, log cleared | same | IdP/cloud logging changed |
| Move | Recon burst, remote access tool, admin protocols | same | Identity fan-out, new privilege |
| Cash out | Exfil tool, ransomware precursors | same | Mass download, external sharing |

Mark each cell: **detect + auto-contain**, **detect + recommend**, **detect only**, or **blind**.

## 3. Output

- The matrix above, filled in for this client
- Blind cells first, then detect-only cells on the Entry and Keys rows (earliest chance to stop an attack)
- For each gap, the catalog capability that closes it and its impact/effort
- Rules that haven't fired in 90 days: recommend a safe simulation to prove them
