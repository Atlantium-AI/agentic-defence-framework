# Security Agents and Skill Packs

Generic agents and skills that work with any EDR, SIEM, IdP and agent platform. Skills use the
[Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) format: a folder with a `SKILL.md`. That's plain
Markdown with a name and description, so it also works as prompt material for other agent platforms.

## The roster, ordered by impact and effort

| # | Agent | What it does | Why it matters | Impact / Effort | Starts at |
|---|---|---|---|---|---|
| 1 | **[Triage](triage/AGENT.md)** | Investigates every alert, classifies how the attacker got in, recommends close/escalate/contain | Every alert gets a full investigation in minutes, which matters most for machine-speed attacks | H / M | Recommend |
| 2 | **[Blind Spot Detector](blind-spot-detector/AGENT.md)** | Finds what you can't see: silent log sources, hosts without EDR, detect-only policies, stale tables, uncovered attack paths | Agentic attackers "blind the guards" first. Mostly reads existing data, so it adds value in week one | H / L | Read-only |
| 3 | **[Alert Tuner](alert-tuner/AGENT.md)** | Cuts noise, drafts narrow exceptions, decides when a rule is ready to switch from recommend to auto | Tier 1 automation is only safe once rules are tuned; this is how rules graduate | H / L | Recommend |
| 4 | **[Data Steward](data-steward/AGENT.md)** | Keeps the reference tables accurate: classifies unknown servers and accounts, finds owners | Automation is only as good as the quarantine list and identity tiers | H / M | Propose |
| 5 | **[Threat Hunter](threat-hunter/AGENT.md)** | Weekly stack-and-diff of the fleet, hypothesis hunts by attack vector | Finds what detections missed; feeds new detections back | M / M | Read-only |
| 6 | **[Researcher](researcher/AGENT.md)** | Answers "is this bad, and does it affect us?" for tools, drivers, domains, MCP servers, CVEs and new techniques | Shared by every other agent; turns threat news into action | M / L | Read-only |

**Next wave** (not built yet):
- **Validator (purple team):** runs safe simulations on a schedule, proves detections fire and actions work, and measures time to contain.
- **Phishing triage:** handles user-reported emails at volume.
- **Reporter:** scorecards and executive summaries.

**80/20 rollout:**

| When | Agents | Why |
|---|---|---|
| Days 1–30 | **Blind Spot Detector** (read-only) + **Triage** (read-only, from day 14) | Blind spot report sets the priorities; triage writes up cases while analysts act |
| Days 31–60 | **Triage** (recommends actions) + **Data Steward** (propose) + **Alert Tuner** (drafts exceptions) | Every alert investigated; quarantine list and identity tiers filled in; noise tuned during recommend-only |
| Days 61–90 | **Alert Tuner** graduation reviews | Graduates the first rules and agent actions to automatic |
| After 90 days | Threat Hunter, Researcher, Validator | The remaining 20%. Until then, triage handles research inline |

## How it fits together

```
 alert ──► Triage ──► recommends actions ──► human approves (later: auto, per action)
             │  uses: Researcher (is this bad?), tables (tiers, exceptions)
             ▼
        false positive? ──► Alert Tuner ──► exception proposal ──► human approves
        unknown host/account? ──► Data Steward ──► table proposal ──► human approves

 weekly ──► Threat Hunter ──► findings ──► Triage / new detection ideas
 daily  ──► Blind Spot Detector ──► gaps ──► owners
```

## Shared skills (every agent loads these)

- [operating-rules](_shared/operating-rules/SKILL.md): evidence standards, treating alert data as untrusted, permission levels, output format
- [containment-decision](_shared/containment-decision/SKILL.md): choosing response actions with tier checks

## Permission levels

Agents earn autonomy **per action type**, not all at once.

| Level | The agent can | Promotion criteria |
|---|---|---|
| **L0 Read** | Query logs and tables, write case notes | Default for every agent |
| **L1 Recommend** | Propose actions and table changes; a human clicks to run them | Default for triage, tuner, steward |
| **L2 Act (reversible, tier A)** | Run reversible actions on tier A hosts/identities (isolate workstation, revoke sessions) | ≥ 95% agreement with analysts over the last 30+ recommendations of that action type, with zero harmful recommendations |
| **L3 Act (broader)** | Run actions on tier B, or disable accounts | Same bar over a longer window, plus sign-off under the containment authority policy |

Tier C assets and identities are **never** acted on by an agent. It pages a human.

## Tools the agents expect

Skills call these generic tools. Each client maps them to its own technology (SIEM queries, EDR API, IdP API, automation platform).

| Tool | Returns |
|---|---|
| `search_logs(query, timeframe)` | Events from the SIEM |
| `get_host(host)` | EDR details: OS, tier tag, logged-on users, sensor health, alerts |
| `get_process_tree(host, process, window)` | Parent/child tree with command lines |
| `get_identity(identity)` | IdP profile, tier, groups, MFA methods, sessions |
| `get_signins(identity, timeframe)` | Sign-ins with IP, ASN, device, app, result |
| `lookup_table(table, key)` | A row from a reference table (asset tiers, exceptions, canary registry, ...) |
| `enrich(indicator)` | Reputation and context for a hash, domain, IP, URL, tool or driver |
| `fleet_search(indicator)` | Every host/identity/mailbox where an indicator appears |
| `run_action(action, target, mode)` | Runs a response action (isolate host, revoke sessions, ...). `mode` = `recommend` or `execute`; the platform enforces the permission level |
| `update_case(case, content)` | Writes findings to the case |
| `page_human(severity, summary)` | Escalates to on-call |
| `propose_change(table, row, reason)` | Submits a table change for human approval |

## Adding a skill to an agent

1. Copy the skill folder into the agent platform's skills directory (for Claude: `.claude/skills/<name>/`).
2. Map the tools above to the client's technology in the client's private repo.
3. Run in L0/L1 for at least two weeks and review the write-ups before raising the permission level.
