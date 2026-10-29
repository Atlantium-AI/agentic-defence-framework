---
name: entity-research
description: Research an unknown tool, driver, file, domain, IP, browser extension, MCP server or AI app and return a sourced verdict on whether it is malicious, abusable or benign, and how common it is in the fleet. Use when any agent hits something it can't judge.
---

# Entity Research

## 1. Inside first (cheapest, most relevant)

- [ ] `fleet_search`: how many hosts/users, since when, which roles?
- [ ] Prior cases or exceptions mentioning it
- [ ] For tools and agents: on the approved list or in the AI agent inventory?

## 2. Outside

Use `enrich` (sandboxed). Never execute or open the item.

| Entity | Check |
|---|---|
| File / hash | Reputation, signer and certificate validity, first-seen date, known malware family |
| Driver | Known vulnerable/malicious driver lists (e.g. LOLDrivers), signer, revoked certificate |
| Tool | Vendor, purpose, known abuse by threat actors (remote access, tunneling, exfil tools are commonly abused) |
| Domain / IP | Registration age, hosting/ASN type, reputation, lookalike of a brand or the company |
| MCP server / AI app / extension | Publisher identity, source repository, install count, permissions requested, data it can reach, known issues |

## 3. Verdict

```
entity: <item>
verdict: <malicious | abusable (legitimate but often abused) | benign | unknown>
confidence: <high|medium|low>
fleet_prevalence: <n hosts/users, first seen>
reasons: <bullets with sources>
recommendation: <block | approve with conditions | investigate usage | no action>
```

**"Abusable"** is the common answer for remote access and admin tools. The question then is whether *this use* was sanctioned.
