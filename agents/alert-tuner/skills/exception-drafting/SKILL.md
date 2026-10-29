---
name: exception-drafting
description: Draft the narrowest possible exception that removes a specific false positive without blinding the rule. Produces a proposal with match criteria, owner, reason and expiry for human approval. Use when noise analysis or triage identifies a sanctioned activity.
---

# Exception Drafting

## Narrowest first

Use the most specific match that covers the benign activity. Prefer, in order:

1. **Hash + host + account**
2. **Signer + path + account** (survives updates)
3. **Signer + path** (all hosts)
4. **Account only** or **host only**: needs a strong reason
5. **Wildcards, script interpreters (powershell, cmd, python), user-writable paths:** **never**

An attacker can reuse anything you exclude. Ask: "If an attacker ran this exact thing, would the exception hide it?" If yes, narrow it further.

## Required fields

```
rule:      <rule the exception applies to>
match:     <exact criteria>
owner:     <person who vouches for the activity>
reason:    <what the activity is and why it's legitimate>
evidence:  <cases/alerts showing it's benign>
expires:   <date, max 12 months; shorter for broad matches>
```

## Before proposing

- [ ] Search 90 days: has this exact match ever appeared in a malicious or suspicious case? If so, stop.
- [ ] Count how many alerts it would have suppressed. Way more than the false positives you're targeting means it's too broad.
- [ ] Submit via `propose_change(exceptions, row, reason)`. Never apply it directly.
