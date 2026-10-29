# The Plan

**Objective:** at every company, reach the **"basics done" bar**: the 20% of effort that gets 80% of the protection against machine-speed attacks. Then chase the rest.

## The "basics done" bar

A company is **basics done** when it can answer *yes* to all of these:

1. **Authority.** Are automated containment actions pre-approved in writing? (F-01)
2. **Asset tiers and data tables.** Is every host tagged A/B/C, with workstations auto-isolatable, and are the tables automation reads kept fresh by scripts with safe defaults? (F-02, F-11)
3. **Identity tiers.** Do we know which accounts automation must never disable? (F-03)
4. **Actions.** Can automation isolate a host (via EDR *and* at the network layer), revoke sessions, disable an account, reset credentials, revoke cloud credentials, block an IOC, purge an email, collect evidence and kill-switch an AI agent, each with one-click undo? (ACT-01 to ACT-11, F-04)
5. **Visibility.** Is EDR on > 95% of hosts in prevent mode, and are the critical log sources flowing? (F-05, F-06)
6. **Tripwires.** Are decoy files and canary accounts deployed and wired to auto-contain? (A-01, A-02)
7. **Tier 1 automation.** Are credential theft, EDR tampering, Office→script, logging tampering, machine-speed recon, unapproved remote access tools, ransomware precursors and exfil tools auto-contained? (A-03, A-04, A-05, A-08, A-13 to A-16)
8. **Identity.** Are MFA fatigue auto-contained and new MFA methods triaged, with admins on phishing-resistant MFA? (A-06, D-03, H-01)
9. **Exposure.** Do we know what's internet-facing, patch actively exploited vulnerabilities on it within days, and watch edge device admin changes? (F-09, D-12)
10. **Our own AI.** Do we know every AI agent and MCP server, what it can do, and can we kill-switch it without touching its users? (F-10, ACT-11, D-13, D-14, H-07)
11. **Triage.** Does every high/critical alert get host + identity evidence packs and a recommendation within 5 minutes? (D-01, I-01, I-02)
12. **Proof.** Have we measured detect-to-contain under 5 minutes in a simulation this quarter? (F-08)

The full list of baseline items, sorted by impact and effort, is in [checklists/baseline.md](checklists/baseline.md).

## Phases

### Phase 0: Foundations (weeks 1–3)
Nothing automated is safe without these. Do them in parallel.

- [ ] F-01 Containment authority signed
- [ ] F-02 Asset tiers, starting with workstations = A on day 1 ([checklist](checklists/asset-containment-tiers.md))
- [ ] F-03 Identity tiers / exclusion group
- [ ] Response actions built and tested with undo, rate limits and audit (ACT-01 to ACT-11, F-04)
- [ ] F-05 EDR coverage gaps and detect-only policies
- [ ] F-06 Critical log sources ([checklist](checklists/critical-log-sources.md))
- [ ] F-07 Out-of-band comms + break-glass
- [ ] F-09 Internet exposure inventory and known-exploited patch SLA
- [ ] F-10 AI agent / MCP server inventory
- [ ] F-11 Reference data tables: the Tier 1 dependency tables first ([checklist](checklists/data-tables.md))

### Phase 1: Tier 1 automation (weeks 2–6)
Each rule runs in **recommend-only mode for ~2 weeks**, then switches to auto once false positives are understood.

- [ ] A-01 / A-02 Decoys and canary accounts (fastest, highest-fidelity win: deploy first)
- [ ] A-03 Credential theft → isolate + revoke
- [ ] A-05 Office → script host → isolate
- [ ] A-08 Logging tampering → page / deny-all
- [ ] A-04 EDR tamper / silent sensor → network-layer quarantine
- [ ] A-06 MFA fatigue → revoke + reset
- [ ] A-13 Machine-speed recon burst → contain
- [ ] A-14 Unapproved remote access / tunneling tool → kill + block
- [ ] A-15 Ransomware precursors → isolate + disable user
- [ ] A-16 Exfiltration tool on a server → kill + block + contain

### Phase 2: Tier 2 triage, recommend mode (weeks 4–10)
Agents and skill packs are in [agents/](agents/README.md). Start the **Blind Spot Detector** (read-only) in Phase 0. It needs no approvals and finds the gaps. Then start the **Triage** agent in recommend mode here.

- [ ] I-01 / I-02 Host and identity evidence packs on every case
- [ ] D-01 Agent write-up on every high/critical EDR alert
- [ ] D-02 / D-03 / D-06 Identity detections into the agent
- [ ] D-04 Server spawning shell into the agent
- [ ] D-12 Edge device admin/config changes into the agent
- [ ] D-13 / D-14 AI agent misbehavior and new MCP servers/connectors into the agent
- [ ] H-07 Least privilege for AI agents
- [ ] I-03 Blast radius sweep, I-05 initial access classification
- [ ] M-01 / M-02 / M-03 Manual runbooks written

### Phase 3: Graduation and the remaining 20%
- [ ] Graduate Tier 2 actions to auto one at a time. Criterion: the agent's recommendation matched the analyst's decision on ≥ 95% of the last N cases (e.g. 30) for that action type
- [ ] Non-baseline capabilities, sorted by impact/effort ([checklists/by-priority.md](checklists/by-priority.md))
- [ ] F-08 quarterly simulation becomes routine

## Design rules (apply everywhere)

- **Cut off whatever they're using.** Host → isolate. Identity → kill sessions. Key → revoke.
- **Every automated action is reversible in one click** and logged.
- **Tier checks at action time.** Missing tag = the safer tier (B).
- **Rate limits.** Above N auto-isolations per hour, stop and page. A runaway rule must never cause the outage itself.
- **Recommend first, then automate.** Every rule starts in recommend-only mode.
- **Technology-agnostic here; specifics in the client repo.** Promote an implementation back to this repo once it's used at two or more clients.
