# The Plan: 30 / 60 / 90 Days

**Objective:** at every company, reach **"basics done"** in 90 days: the 23 capabilities that give 80% of the protection against machine-speed attacks. Everything else waits.

## The "basics done" bar

A company is basics done when it can answer **yes** to all eight:

1. **Authority.** Are automated containment actions pre-approved in writing?
2. **Know what to protect.** Is every host and identity tagged with what automation may do to it, and do we know what's internet-facing and which AI agents we run?
3. **See.** Is EDR on > 95% of hosts in prevent mode with anti-tamper, and are the critical log sources flowing?
4. **Act.** Can automation isolate a host, block a domain/IP network-wide, revoke sessions, disable an account and kill-switch an AI agent, each with one-click undo?
5. **Tripwires.** Do decoy files and canary credentials auto-contain whoever touches them?
6. **Automatic containment.** Are credential theft, EDR tampering, MFA fatigue, machine-speed recon and ransomware precursors contained in seconds on tier A?
7. **Triage.** Does every high/critical alert, risky sign-in and server-spawned shell get an agent investigation and recommendation in under 5 minutes?
8. **Identity.** Do all admins use phishing-resistant MFA?

The 23 capabilities behind these are in [checklists/baseline.md](checklists/baseline.md).

## Days 1–30: See and authorize

**Goal:** permission to act, and an honest picture of what we can and can't see.

- [ ] Containment authority policy drafted in week 1, signed by day 30 (F-01)
- [ ] All workstations tagged auto-isolatable; servers default to contain-lite (F-02, [checklist](checklists/asset-containment-tiers.md))
- [ ] Identity tiers: humans vs. service accounts, admins and break-glass (F-03)
- [ ] EDR coverage gaps closed, prevent mode, anti-tamper on (F-05)
- [ ] Critical log sources flowing with lag measured (F-06, [checklist](checklists/critical-log-sources.md))
- [ ] Internet exposure inventory + patch SLA for known-exploited vulnerabilities (F-09)
- [ ] AI agent and MCP server inventory (F-10)
- [ ] Decoy files and canary credentials deployed in recommend-only mode (A-01)
- [ ] **Blind Spot Detector** agent running read-only ([agents](agents/README.md))
- [ ] Phishing-resistant MFA rollout started for admins (H-01)

**Exit:** policy signed · workstations tagged · first blind spot report delivered.

## Days 31–60: Build the actions, switch on the tripwires

**Goal:** every containment action works and can be undone; detections run in recommend-only mode so we learn their noise.

- [ ] Response actions built and tested with undo, rate limits and audit: isolate host via EDR, network block (domain/IP/URL), revoke sessions, disable account, AI agent kill switch (F-04, ACT-01, ACT-07, ACT-03, ACT-04, ACT-11)
- [ ] Decoys switch to **automatic** (after 2 clean weeks)
- [ ] Recommend-only: credential theft (A-03), EDR tampering / silent sensor (A-04), MFA fatigue (A-06), machine-speed recon (A-13), ransomware precursors (A-15)
- [ ] **Triage** agent in recommend mode on EDR high/critical alerts, risky sign-ins and server-spawned shells (D-01, D-02, D-04)
- [ ] **Data Steward** agent proposes server tiers; app owners answer one yes/no question per server

**Exit:** all five actions tested end to end · decoys automatic · every high alert gets an agent write-up.

## Days 61–90: Go automatic and prove it

**Goal:** machine-speed containment on tier A, measured.

- [ ] **Alert Tuner** runs graduation reviews; rules that pass go **automatic for tier A** (workstations, standard users)
- [ ] Phishing-resistant MFA complete for all admins (H-01)
- [ ] Safe attack simulation: decoy read, credential theft, recon burst, MFA burst. Measure detect-to-contain; target < 5 minutes
- [ ] Executive scorecard: % basics done per attack vector, and what's next

**Exit:** 6 detections containing automatically · time to contain measured · scorecard delivered.

## What we need from leadership

| Ask | When |
|---|---|
| Sign the containment authority policy | Day 30 |
| Mandate phishing-resistant MFA for admins | Day 1 |
| App owners: 15 minutes each to answer "can we isolate this server?" | Days 31–60 |
| Read-only API access for the agents; automation platform access for actions | Day 1 |

## How we keep it safe

- **Recommend first.** Every rule runs recommend-only for 2 weeks before acting.
- **Tier A only at first.** Workstations and standard users. Never-isolate servers and critical accounts always go to a human.
- **One-click undo** on every action, **rate limits** so a bad rule can't cause an outage, and a full audit log.
- **Agents earn autonomy per action type** (≥ 95% agreement with analysts, zero harmful calls).

## Metrics

| Metric | Day 30 | Day 90 target |
|---|---|---|
| EDR coverage in prevent mode | measure | > 95% |
| Critical log sources flowing | measure | 100% |
| Hosts with a containment tier | workstations | 100% (servers default to contain-lite) |
| Detections containing automatically | 0 | 6 |
| Time from alert to triage write-up | measure | < 5 min |
| Detect-to-contain in simulation | — | < 5 min |
| Admins on phishing-resistant MFA | measure | 100% |

## Not in the first 90 days

Deliberately deferred: network-layer quarantine, cloud-specific automation, workstation-to-workstation blocking, driver baselines, threat hunting and research agents, purple-team validation, SaaS exfil analytics. All are in [checklists/by-priority.md](checklists/by-priority.md), ready for the next quarter.
