---
name: control-coverage
description: Weekly comparison of inventories to find hosts, identities, cloud accounts and AI agents outside the security controls. Covers unmanaged hosts, detect-only policies, risky exclusions, admins without phishing-resistant MFA and unapproved agents. Use on a weekly schedule.
---

# Control Coverage

Compare every inventory against every other one. Anything that exists in one source but is missing a control is a blind spot.

## Hosts

- [ ] In AD, IdP devices, DHCP, cloud inventory or vulnerability scans, but **not in the EDR** = unmanaged host
- [ ] EDR policies in detect-only, or with prevention features off
- [ ] EDR exclusions that are broad (whole folders, wildcards, script interpreters, user-writable paths): list them with their owners
- [ ] Linux servers and edge devices with no EDR and no log forwarding
- [ ] Hosts with no asset tier (falling back to the safe default)

## Identities

- [ ] Admins (privileged identities table) without phishing-resistant MFA
- [ ] Accounts not in HR and not classified as service/agent accounts
- [ ] Service accounts with interactive logon rights or no owner
- [ ] Long-lived cloud access keys older than 90 days

## Cloud and SaaS

- [ ] Cloud accounts/subscriptions not sending control-plane logs
- [ ] Regions or accounts with threat detection turned off

## AI agents

- [ ] Agent processes, MCP configs or AI OAuth apps found in the environment but not in the AI agent inventory
- [ ] Agents that can write or execute but have no owner, no logs in the SIEM, or no kill switch

## Output

A gap list grouped by owner, with counts trending week over week. New gaps first.
