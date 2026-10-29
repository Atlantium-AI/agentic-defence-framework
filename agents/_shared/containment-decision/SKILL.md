---
name: containment-decision
description: Choose response actions for a suspected compromise. Maps what the attacker is using (host, identity, cloud key, email, AI agent) to actions, checks asset and identity tiers and exceptions, and decides recommend vs execute. Use whenever an investigation reaches "contain?".
---

# Containment Decision

**Rule: cut off whatever they're using.**

## 1. What is the attacker using?

| They're using | Primary action | Add if |
|---|---|---|
| A host (malware, hands-on-keyboard, recon) | Isolate host via EDR | EDR is tampered or silent → isolate at the network layer |
| An identity (stolen password, session, token) | Revoke sessions and tokens | Credential known stolen → reset password and MFA methods; still active → disable account |
| A cloud key or principal | Revoke cloud credentials | — |
| An email (phish delivered to many) | Purge email from all mailboxes | Payload or domain known → network block (domain, IP, URL) |
| An AI agent (injected or misbehaving) | Kill switch for the AI agent | Source content known → block or remove it |
| A tool (remote access, tunneling, exfil) | Kill process and quarantine file | Network block on its domains |
| A remote server (C2, payload host, exfil destination) | Network block (domain, IP, URL) | — |

Usually more than one applies. Credential theft on a host = isolate the host **and** revoke sessions for every user logged on to it.

## 2. Check before acting

For each target:
1. `lookup_table(asset-tiers | identity-tiers, target)`. A missing row means use the safe default (tier B).
2. `lookup_table(exceptions, rule + target)`: is this a sanctioned activity with a valid, unexpired entry?
3. `lookup_table(change-windows, target)`: planned work right now?
4. Collect evidence (live triage package) **before** isolating if the action allows it; isolation keeps the EDR channel open, so collection can also follow immediately.

## 3. Tier rules

| Tier | Host | Identity |
|---|---|---|
| A | Full isolation | Revoke, disable, reset |
| B | Kill process, network block, page human with one-click isolation | Revoke sessions + page; disable needs a human |
| C | **Never act.** Page immediately, recommend the manual runbook, contain the identity instead | **Never act.** Page immediately |

## 4. Recommend or execute?

Execute only if **all** are true:
- You hold L2+ for this action type and this tier.
- The evidence meets a Tier 1 bar (deterministic, high-fidelity signal, or multiple independent strong signals).
- The action is reversible and you can state the undo.
- The automation rate limit hasn't been hit.

Otherwise recommend, and say exactly what evidence would justify executing.

## 5. Record

For every action: target, tier, action, mode (recommend/executed), the evidence that justified it, and the undo step.
