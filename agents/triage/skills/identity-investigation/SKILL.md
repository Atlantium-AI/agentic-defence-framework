---
name: identity-investigation
description: Investigate a user, service account or cloud principal for account takeover or misuse. Covers sign-ins, MFA changes, sessions, privilege changes, OAuth consents and post-login actions. Use when an alert involves an identity, or a host investigation finds identities used on a compromised host.
---

# Identity Investigation

## Collect

- [ ] `get_identity`: tier, type (human/service/agent), groups, admin roles, MFA methods (with registration dates), active sessions
- [ ] `get_signins` 7 days: IP, ASN, country, device, app, result, MFA method used
- [ ] Help desk tickets for this identity in the last 7 days (password or MFA reset)
- [ ] Changes made **to** the identity: MFA methods added, group/role changes, password resets
- [ ] Changes made **by** the identity: inbox rules, OAuth consents, mail forwarding, new app registrations or secrets, cloud keys, permission grants, file sharing

## Account takeover signs (strongest first)

1. New MFA method registered from a never-seen device or IP
2. MFA burst (many denied pushes) followed by an approval
3. Help desk reset followed by a sign-in from a new device within 24h
4. Session token used from a different IP/ASN than where it was issued (token replay)
5. Sign-in from new device + new ISP together (location alone is weak because attackers use residential proxies)
6. Post-login: inbox rules hiding mail, mass file downloads, internal phishing sent, OAuth consent to an unknown app

## Service accounts and cloud principals

- Compare against the service account baseline: is this a new source host, destination or logon type?
- Cloud principals: activity from outside the cloud provider, new regions, bursts of List/Describe calls, new keys or policy attachments.

## Questions to answer

1. When did the attacker first use this identity, and from where?
2. Which sessions and tokens are attacker-controlled right now? (These need revoking.)
3. What did they change that would survive a password reset (MFA methods, OAuth grants, keys, rules)? Each one needs reverting.
4. What did they access or send?
