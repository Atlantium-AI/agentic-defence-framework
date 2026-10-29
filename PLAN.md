# The Plan: 30 / 60 / 90 Days

**Objective:** at every company, reach **"basics done"** in 90 days: the 34 capabilities that give 80% of the protection against machine-speed attacks. Almost all are low effort; the full list is in [checklists/baseline.md](checklists/baseline.md).

## The "basics done" bar

A company is basics done when it can answer **yes** to all nine:

1. **Authority.** Are automated containment actions pre-approved in writing?
2. **Know what to protect.** Is every host, identity and cloud principal tagged with what automation may do to it, and do we know what's internet-facing and which AI agents can act?
3. **See.** Is EDR on > 95% of hosts in prevent mode with anti-tamper, and are the "Must have" log sources flowing?
4. **Act safely.** Can automation isolate a host, kill a process, network block, revoke sessions, disable an account, reset credentials, revoke cloud credentials and restore cloud logging, each with undo, bulk undo, rate limits and a log that answers "did security do this to me?"
5. **Tripwires.** Do decoys, canary credentials and canary accounts contain whoever touches them, without being turned against us?
6. **Automatic containment.** Are credential theft, MFA fatigue, machine-speed recon, ransomware precursors, cloud logging tampering and stolen cloud credentials contained in seconds on tier A?
7. **Edge.** Are edge management interfaces off the internet, VPN behind SSO + MFA, and known-exploited bugs on exposed devices patched within 72 hours and checked for compromise?
8. **Triage.** Does everything else (other EDR alerts, risky sign-ins, new MFA methods, server shells, new privileges, edge changes, uncorroborated EDR tampering, prompt injection) get an agent investigation in under 5 minutes?
9. **Identity.** Do all admins use phishing-resistant MFA?

## How the timing was set (80/20)

Effort points per phase (low = 1, medium = 3), counted where the build work happens:

| Phase | Items | Effort points | Why this load |
|---|---|---|---|
| Days 1–30 | 9 | 15 | Lighter on build work, because signatures, owners and inventories take calendar time |
| Days 31–60 | 16 | 22 | The heavy build: actions and most detections, all in recommend-only mode |
| Days 61–90 | 9 | 21 | Cloud, edge and the rules that depend on others; graduation to automatic |

Changes from the first draft: EDR prevent-mode enforcement moved to days 31–60. The AI agent inventory and phishing-resistant MFA completion moved to days 61–90. The AI kill switch moved past day 90. EDR tampering moved to days 61–90, because it needs the other detections running first to corroborate it.

## Days 1–30: See and authorize

**Goal:** permission to act, a record of every action, and an honest picture of what we can see.

- [ ] Containment authority policy drafted in week 1, signed by day 30 (F-01)
- [ ] Workstations tagged auto-isolate on day 1; servers default to contain-lite ([checklist](checklists/asset-containment-tiers.md)) (F-02)
- [ ] Identity tiers, including cloud principals (F-03)
- [ ] Containment action log: the help desk can answer "did security do this to me?" (F-12)
- [ ] "Must have" log sources flowing with lag measured; "Collect now" by day 60 ([checklist](checklists/critical-log-sources.md)) (F-06)
- [ ] Internet exposure inventory + 72-hour patch SLA for known-exploited bugs (F-09)
- [ ] Edge lockdown: management interfaces off the internet, VPN through SSO + MFA ([edge plan](checklists/edge-devices.md)) (H-08)
- [ ] Decoy files, canary credentials and canary AD accounts deployed in recommend-only mode (A-01, A-02)
- [ ] Phishing-resistant MFA rollout started for admins (H-01)
- [ ] **Blind Spot Detector** agent running read-only ([agents](agents/README.md))
- [ ] Data tables started: asset tiers, identity tiers, exceptions, decoy registry, exposure inventory, cloud accounts and regions ([data tables](checklists/data-tables.md))

**Exit:** policy signed · workstations tagged · action log live · first blind spot report delivered.

## Days 31–60: Build the actions, learn the noise

**Goal:** every action works and can be undone; most detections run in recommend-only mode.

- [ ] Action safety: tier checks, undo and bulk undo, rate limits per rule and action type that page P1 when hit (F-04)
- [ ] Actions: isolate host via EDR, kill process, network block, revoke sessions, disable account, reset password and MFA (ACT-01, ACT-10, ACT-07, ACT-03, ACT-04, ACT-05)
- [ ] EDR prevent mode and anti-tamper enforced, coverage gaps closed (F-05)
- [ ] Decoys and canary accounts switch to **automatic** after 2 clean weeks (A-01, A-02), with the mass-hit breaker in place
- [ ] Recommend-only: credential theft (A-03), MFA fatigue (A-06), machine-speed recon on Windows and Linux (A-13), ransomware precursors (A-15)
- [ ] **Triage** agent in recommend mode: EDR high/critical alerts, risky sign-ins, new MFA method or help-desk reset, server spawning a shell (D-01, D-02, D-03, D-04)
- [ ] **Alert Tuner** starts drafting exceptions from day 31
- [ ] **Data Steward** proposes server tiers; app owners answer one yes/no question per server

**Exit:** all actions tested end to end · decoys automatic · every high alert gets an agent write-up.

## Days 61–90: Cloud, edge, and go automatic

**Goal:** machine-speed containment on tier A across endpoint, identity and cloud, measured.

- [ ] Cloud actions: revoke cloud credentials (tier-aware), restore cloud logging (ACT-06, ACT-12)
- [ ] Cloud detections: logging or security tool disabled (A-08), cloud credentials used outside the provider or in unused regions (A-12), new privileges, keys, root or break-glass use (D-06)
- [ ] Edge device admin and config changes to the triage agent (D-12)
- [ ] EDR tampering **corroborated by a second signal**, with the fleet circuit breaker (A-04)
- [ ] AI agent inventory: agents that can write or execute (F-10)
- [ ] Phishing-resistant MFA complete for all admins (H-01)
- [ ] **Alert Tuner** graduation reviews: rules that pass go **automatic for tier A**
- [ ] Safe attack simulation: decoy read, credential theft, recon burst, MFA burst, cloud trail disable. Measure detect-to-contain; target < 5 minutes
- [ ] Executive scorecard

**Exit:** 9 detections containing automatically · time to contain measured · scorecard delivered.

## Automation at day 90

| Detection | Response | Day 90 |
|---|---|---|
| Decoy, canary credential or canary account touched | Isolate host, revoke sessions | **Auto** |
| Credential theft | Isolate host, revoke sessions, reset credentials, network block | **Auto** |
| MFA fatigue then approval | Revoke sessions, reset password and MFA | **Auto** |
| Machine-speed recon (Windows, Linux) | Isolate host, revoke session | **Auto** on workstations |
| Ransomware precursors | Isolate host, disable user, network block | **Auto** |
| Cloud logging or security tool disabled | Restore it, revoke the actor's cloud credentials (tier-aware) | **Auto** |
| Cloud credentials used outside the provider / unused region | Revoke active sessions on the role, page | **Auto** |
| EDR tampering + a second signal | Revoke sessions, attempt isolation, page | **Auto** if it passes graduation, otherwise recommend |
| EDR tampering or silent sensor alone | Triage agent decides | Recommend |
| Prompt injection found in evidence | Triage + Researcher determine; never contained on injection alone | Recommend |
| Other EDR alerts, risky sign-ins, new MFA methods, server shells, new privileges, edge changes | Triage agent | Recommend |

"Auto" means tier A only: workstations, standard users, human cloud roles. Workload roles only get session revocation. Tier C assets and identities always go to a human.

## Cloud automation, safely

| Cloud principal | Auto | Recommend | Never auto |
|---|---|---|---|
| Human users and roles (tier A) | Revoke sessions, deactivate keys, deny-all | — | — |
| Workload, CI/CD, service roles (tier B) | Revoke active sessions (the workload gets fresh credentials; the attacker's copy dies) | Deactivate keys, deny-all | — |
| Root, break-glass, org admin, security tooling, automation platform roles (tier C) | — | — | Everything: page only |
| Any actor that disabled logging | Restore the logging | — | — |

## What we need from leadership

| Ask | When |
|---|---|
| Sign the containment authority policy | Day 30 |
| Mandate phishing-resistant MFA for admins | Day 1 |
| Network team: take edge management interfaces off the internet | Days 1–30 |
| App owners: 15 minutes each to answer "can we isolate this server?" | Days 31–60 |
| Read-only API access for the agents; automation platform access for actions | Day 1 |

## How we keep it safe

- **Recommend first.** Every rule runs recommend-only for 2 weeks before acting.
- **Tier A only at first.** Critical servers, accounts and cloud principals always go to a human.
- **Undo, bulk undo, rate limits that page P1, and an action log** on every action.
- **No outage from our own tools.** EDR tampering needs a second signal, and a fleet circuit breaker stops automation during EDR outages.
- **Decoys can't be weaponized.** Only real content reads count, and mass hits become one incident instead of mass isolation.
- **Prompt injection never triggers containment** on its own.
- **Agents earn autonomy per action type** (≥ 95% agreement with analysts, zero harmful calls, passes an injection test set).

## Metrics

| Metric | Day 30 | Day 90 target |
|---|---|---|
| EDR coverage in prevent mode | measure | > 95% |
| "Must have" log sources flowing | measure | 100% |
| Hosts and identities with a containment tier | workstations | 100% (rest on safe defaults) |
| Detections containing automatically | 0 | 9 |
| Time from alert to triage write-up | measure | < 5 min |
| Detect-to-contain in simulation | — | < 5 min |
| Admins on phishing-resistant MFA | measure | 100% |
| Edge management interfaces exposed to the internet | measure | 0 |

## Not in the first 90 days

Deliberately deferred: AI agent kill switch and AI agent detections, phishing response automation (adjacent; most companies already run a phishing program), network-layer quarantine, cloud instance isolation, SaaS audit-log detections, workstation-to-workstation blocking, driver baselines, threat hunting and research agents, purple-team validation, exfiltration analytics, hypervisor (ESXi) logging. All are in [checklists/by-priority.md](checklists/by-priority.md), ready for the next quarter.
