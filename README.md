# Agentic Defence Framework

**Goal:** detect, contain, and investigate agentic (AI-driven) attacks at machine speed, at every company we work with.

Agentic attackers go from access to impact in minutes (fastest observed eCrime breakout: 27 seconds). A human in the loop for *every* containment decision is too slow. The answer is a small set of **pre-approved, reversible, automated actions** triggered by **deterministic, high-fidelity detections**, plus an AI triage agent for everything else.

## How it's organized

| Path | What it is |
|---|---|
| [reports/](reports/machine-speed-containment-90-day-plan.pdf) | Executive report: the 30/60/90 plan for leadership ([PDF](reports/machine-speed-containment-90-day-plan.pdf), [HTML](reports/machine-speed-containment-90-day-plan.html)) |
| [PLAN.md](PLAN.md) | The 80/20 rollout plan and the **"basics done" bar** for every company |
| [catalog/capabilities.yaml](catalog/capabilities.yaml) | **Source of truth.** Every capability, scored by impact and effort |
| [checklists/](checklists/) | Generated from the catalog: build `checklists/` with `python tools/build.py` |
| [catalog/stack-coverage.yaml](catalog/stack-coverage.yaml) | If a company runs X, the logs, detections and actions it needs. Drives the blind spot skill |
| [checklists/stack-coverage.md](checklists/stack-coverage.md) | Generated: stack coverage as a readable checklist |
| [catalog/data-tables.yaml](catalog/data-tables.yaml) | The reference data tables automation reads, and how each is maintained |
| [checklists/data-tables.md](checklists/data-tables.md) | Generated: tables by priority, with sources and who maintains what |
| [checklists/asset-containment-tiers.md](checklists/asset-containment-tiers.md) | Critical path: building the can/cannot auto-quarantine list |
| [agents/](agents/README.md) | Six security agents (triage, blind spot detector, alert tuner, data steward, threat hunter, researcher) with generic skill packs, permission levels and the tools they expect |
| [client-template/](client-template/) | Copy into each client's **private** repo/folder. No client data lives here |
| [tools/build.py](tools/build.py) | Regenerates checklists from the catalog |

## Mental models

**How they get in (initial access):**

| Vector | What it is | Where it shows up first |
|---|---|---|
| **Pulled in** | A user or AI agent brings it in: phishing, download, ClickFix, prompt-injected agent | Workstations. Parent is Outlook, a browser, or an agent |
| **Pushed in** | Attacker exploits an internet-facing service: VPN, web app, edge device | Servers and edge devices. Server process spawns a shell |
| **Walked in** | Valid access: stolen creds, AitM session theft, compromised vendor, insider | Identity provider. Valid login from somewhere odd, trusted tool misbehaving |

**What they do once in:** *get the keys → blind the guards → move → cash out.*
**How we respond:** *cut off whatever they're using.* If it's a host, isolate the host. If it's an identity, kill the identity's sessions. If it's a key, revoke the key.

## Response tiers

| Tier | Detection quality | Response |
|---|---|---|
| **Tier 0** | n/a | Foundations: authority, asset tiers, tested actions, telemetry |
| **Tier 1** | Deterministic, near-zero false positives | **Automated** containment within pre-approved guardrails, seconds |
| **Tier 2** | Good signal, needs context | **Triage agent** investigates and recommends. Graduates to acting on its own per action type once its track record is good |

## Using it at a client

1. Copy `client-template/` into the client's private workspace.
2. Run the blind spot skill ([stack-intake](agents/blind-spot-detector/skills/stack-intake/SKILL.md), or `/blind-spot-check` in Claude Code). It fills in `stack.yaml` and writes `blind-spots.md`.
3. Work [PLAN.md](PLAN.md) Phase 0 → Phase 1, tracking status in the client's `scorecard.yaml`.
4. Technology-specific queries/playbooks get filled in per client and, once reused at two or more clients, promoted back here.

## Editing the catalog

```bash
python tools/build.py
```

Requires `pyyaml` (`pip install pyyaml`).
