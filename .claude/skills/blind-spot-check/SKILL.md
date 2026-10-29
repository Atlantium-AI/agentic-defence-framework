---
name: blind-spot-check
description: Run the Blind Spot Detector's stack intake for a client from this repo. Interviews the user about their tech stack, maps it to required logs, detections and actions, verifies what it can through connected SIEM, wiki or CMDB tools, and writes stack.yaml and blind-spots.md to the client's private folder. Use when the user wants to onboard a client, list their tech stack, or find blind spots.
---

# Blind Spot Check

Follow [agents/blind-spot-detector/skills/stack-intake/SKILL.md](../../../agents/blind-spot-detector/skills/stack-intake/SKILL.md) with these specifics for this repo.

## Setup

1. Ask for the client folder name (short, lowercase). Client data lives in `clients/<name>/`, which git ignores. Never write client details anywhere else in this repo.
2. If the folder doesn't exist, copy `client-template/` into it.
3. Run `python tools/build.py --client clients/<name>` to sync `stack.yaml` and `scorecard.yaml` with the catalog.

## Interview

- Use AskUserQuestion, one component per question. Offer the component's `examples` from `catalog/stack-coverage.yaml` as options. Free-text "Other" covers the rest.
- After each answer, update `clients/<name>/stack.yaml` straight away so nothing is lost if the session stops.
- Status values: `in_use` is `unknown | not-used | partial | in-use`; each log is `unknown | missing | partial | flowing`.

## Verify

- Check which tools this session has: SIEM queries, Confluence / SharePoint / Notion search, a CMDB connector. Use only what's connected and read-only.
- If nothing is connected, say which connectors would help (SIEM, wiki, CMDB) and finish with Passes 1–2.

## Finish

1. Re-run `python tools/build.py --client clients/<name>` to check that `stack.yaml` is still valid.
2. Write `clients/<name>/blind-spots.md` in the format the stack-intake skill sets out.
3. Tell the user the top 3 gaps in chat, each with its readable name. Codes go in brackets.
