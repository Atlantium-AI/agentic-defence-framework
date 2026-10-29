---
name: initial-access-classification
description: Classify how an attacker got in as pulled in (user or AI agent brought it in), pushed in (exploited internet-facing service) or walked in (valid access). Decides where to look next and what to sweep. Use on every incident that isn't closed as benign.
---

# Initial Access Classification

Trace the activity back to its earliest event, then classify.

| Vector | Clues | Most likely on |
|---|---|---|
| **Pulled in** | Parent chain leads to Outlook, Office, a browser, an archive/ISO opener, or an AI agent/coding assistant process. File has Mark-of-the-Web or came from an email attachment cache. User was interactive | Workstations |
| **Pushed in** | Parent chain leads to a server process (web server, app server, database, file transfer, VPN/edge device). Web shell files, service crash before the activity, exploit signatures in WAF/IDS logs | Servers, edge devices |
| **Walked in** | No malware at the start: a valid login from a new device/ISP/location, a new MFA method, a help desk reset, vendor/RMM access, a trusted tool doing something new | Identity provider, VPN, SaaS, cloud |

## Steps

1. [ ] Walk the process tree up to the root (`get_process_tree`). Note the earliest parent you can see.
2. [ ] For a file: find its origin (download URL, email attachment, written by which process).
3. [ ] For the identity: check sign-ins in the 24h before the first malicious event (`get_signins`). New device, IP or MFA method?
4. [ ] For a server: is it in the internet exposure inventory? Any known-exploited vulnerability on it?
5. [ ] Classify, with confidence. If evidence points at more than one, say so (e.g. walked in via phished creds = pulled → walked).

## What it drives

| Vector | Blast radius sweep focus | Also check |
|---|---|---|
| Pulled | Everyone who received the same email, visited the same URL or ran the same file | Purge email, block domain; was an AI agent the one that pulled it? |
| Pushed | Other hosts exposing the same service/version; other web shells | Patch status, other exposed assets with the same vulnerability |
| Walked | Every session, device and token of the identity; what it accessed after login | MFA changes, OAuth consents, inbox rules, help desk tickets |
