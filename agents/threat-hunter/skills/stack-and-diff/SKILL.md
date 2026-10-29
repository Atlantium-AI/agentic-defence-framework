---
name: stack-and-diff
description: Weekly fleet-wide frequency analysis. Counts persistence entries, drivers, services, remote access tools, outbound destinations and AI agent configs, and surfaces what is rare or new since last week. Use on a weekly schedule.
---

# Stack and Diff

Attackers are rare; the business is common. Count everything, look at the bottom of the list.

## What to stack

| Category | Key | Rare means |
|---|---|---|
| Persistence (services, tasks, run keys, WMI, cron, systemd, authorized_keys) | name + path + signer | On < 3 hosts, or new this week |
| Kernel drivers | hash + signer + name | Not in the fleet driver baseline |
| Remote access / tunneling tools | product | Not on the approved list |
| Server outbound destinations | domain/ASN per server role | New for that role |
| Processes running from user-writable paths | path pattern + signer | Unsigned or rare signer |
| AI agent configs / MCP servers | server name + source | Not in the AI agent inventory |
| Local admin group members | account per host | Account on a few hosts only |

## Steps

1. [ ] Collect this week's counts and diff against last week's baseline.
2. [ ] Drop anything covered by a valid exception.
3. [ ] For each rare or new item: who/what created it, when, signer, and how many hosts. Ask the Researcher about anything unknown.
4. [ ] Triage each: **benign** (add to the baseline), **suspicious** (open a case for triage), **malicious** (open a P1 case).
5. [ ] Update the baseline.

## Output

Top 20 rarest items with a verdict each, plus any item that should become a permanent detection.
