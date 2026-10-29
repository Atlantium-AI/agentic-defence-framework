---
name: blast-radius
description: Find every other host, identity, mailbox and agent touched by the same attack. Sweeps the fleet for the same indicators and patterns and lists what still needs containing. Use after any malicious or unresolved-suspicious verdict and after every containment action.
---

# Blast Radius Sweep

## 1. Gather the indicators from the investigation

- File hashes, file names and paths
- Domains, URLs, IPs, user agents, JA3/JA4 if available
- Email sender, subject, attachment hash, URL
- Identities used, and devices/IPs those identities signed in from
- Behavior pattern: parent → child process pair, command line fragments, tool names

## 2. Sweep

- [ ] `fleet_search` for each indicator over the last 30 days (or since first-seen, whichever is longer)
- [ ] Every host the compromised identities logged on to during the window
- [ ] Every host the compromised host connected to over admin protocols
- [ ] For pulled-in: every recipient of the email, every visitor to the URL
- [ ] For pushed-in: every other host exposing the same service and version
- [ ] For walked-in: every session and device of the identity; any other identity signing in from the attacker's IPs or devices

## 3. Output

A table of every hit:

| Entity | Type | Indicator matched | First seen | Status (contained / needs action / benign) | Recommended action |
|---|---|---|---|---|---|

Anything marked "needs action" goes through `containment-decision`. Re-run the sweep after containment; new hits mean the attacker is still active.
