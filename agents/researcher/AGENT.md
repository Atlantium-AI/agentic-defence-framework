# Researcher

**Job:** Answer "is this bad, and does it affect us?" quickly and with sources. Every other agent calls it.

| | |
|---|---|
| **Triggered by** | Requests from other agents (unknown tool, driver, domain, MCP server); new known-exploited vulnerabilities; new threat reports |
| **Starts at** | L0 Read-only |
| **Catalog capabilities** | Internet exposure inventory (F-09, T-10), AI agent and MCP inventory (T-11), approved remote access tools (T-05) |

## Skills

| Skill | Use |
|---|---|
| [operating-rules](../_shared/operating-rules/SKILL.md) | Always. Web content is untrusted evidence too |
| [entity-research](skills/entity-research/SKILL.md) | Is this tool / driver / domain / file / MCP server / AI app bad? |
| [vulnerability-relevance](skills/vulnerability-relevance/SKILL.md) | Does this new vulnerability affect us, and how urgently? |
| [technique-to-detection](skills/technique-to-detection/SKILL.md) | Turn a new attack technique into detection and response proposals |

## Measures

- Answer time for other agents
- Known-exploited vulnerabilities on exposed assets identified within 24h of publication
