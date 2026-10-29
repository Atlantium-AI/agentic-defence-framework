---
name: hypothesis-hunt
description: Run a structured hunt from a hypothesis ("an attacker walked in with a stolen session and added an OAuth app"). Turns it into concrete observables, searches for them, and turns findings into cases or new detections. Use for scheduled hunts, new threat intel, or after an incident.
---

# Hypothesis Hunt

## 1. State the hypothesis

`If <attacker> used <vector> to <goal> here, we would see <observable> in <data source>.`

## 2. Starter hypotheses (rotate monthly)

| Vector | Hypothesis | Observables |
|---|---|---|
| Pulled | A user ran a ClickFix-style paste command | Run dialog or terminal launching powershell/mshta/curl with an encoded or remote command, right after browser activity |
| Pulled | An AI agent was steered by injected content | Agent tool calls that send, share or execute right after reading external content |
| Pushed | An edge device was exploited without an alert | New admin accounts, config changes, or internal connections from the edge device |
| Pushed | A web shell exists on an internet-facing server | Recently written script files in web directories; server processes spawning shells |
| Walked | An attacker holds a stolen session | Same session/token from two ASNs; sign-ins from hosting providers |
| Walked | Persistence without malware | New OAuth consents, MFA methods, service principal secrets, cloud keys, inbox forwarding rules in 30 days |
| Post-access | A machine-speed agent is doing recon | Discovery command bursts; many LDAP queries from a workstation |
| Post-access | Credentials are being collected quietly | Access to credential files, browser stores, decoys; Kerberos RC4 ticket requests |

## 3. Hunt

- [ ] Check the data sources are healthy for the window (else the hunt can't conclude "nothing found").
- [ ] Search; stack the results; investigate the rare ones.
- [ ] Time-box to a few hours.

## 4. Close out

- **Finding** → case to triage
- **Nothing found** → record what was searched and over what window
- **Either way:** could this be a detection? If the observable is high-fidelity, propose it as a new rule with its response action.
