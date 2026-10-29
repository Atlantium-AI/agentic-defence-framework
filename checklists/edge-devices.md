# Edge Devices: 80/20 Action Plan

VPNs, firewalls, file transfer appliances and remote access gateways are the top exploitation target, and they can't run EDR. You can't detect your way out of this, so the 80/20 is: **shrink the attack surface, patch fast, assume compromise when a known-exploited bug lands, and watch the few logs that matter.**

## 1. Know them (days 1–30, part of the internet exposure inventory)
- [ ] List every internet-facing edge device: vendor, model, firmware version, owner, what it authenticates against (AD/LDAP, SSO)
- [ ] Subscribe the owner to the vendor's security advisories and the CISA known-exploited vulnerability (KEV) feed

## 2. Shrink the attack surface (days 1–30, H-08, low effort)
- [ ] **Management interfaces not reachable from the internet.** This single change removes a large share of edge exploitation.
- [ ] **VPN sign-in through SSO with MFA**, phishing-resistant for admins. Revoking a user's SSO sessions then also ends their VPN access.
- [ ] Named edge admin accounts (no shared "admin"), as few as possible
- [ ] Unused features off (web portals, legacy protocols, clientless VPN)

## 3. Patch fast (ongoing)
- [ ] Known-exploited vulnerability on an exposed edge device: patch or mitigate within **48–72 hours**
- [ ] Can't patch in time? Apply the vendor mitigation, restrict access to known IPs, or take it offline. Leadership signs the exception.

## 4. Assume compromise when a known-exploited bug lands (manual runbook)
Patching closes the door but doesn't remove someone already inside. When a known-exploited vulnerability affects an exposed device:
- [ ] Run the vendor's integrity checker, if one exists
- [ ] Look for changes since the disclosure date: new admin accounts, config changes, unknown files, firmware changes
- [ ] Review VPN sessions in that window from new devices or unusual locations
- [ ] Reset credentials that pass through the device: VPN users who signed in during the exposure window, and the LDAP/AD service account the device uses
- [ ] If compromise is confirmed: fail over or rebuild from known-good firmware, block the attacker's IPs, and run the triage agent's blast radius sweep on every identity that authenticated through it

## 5. Watch the logs that matter (days 31–60, D-12)
Forward only these from each edge device to the SIEM:
- [ ] Admin sign-ins (success and failure)
- [ ] Config and firmware changes
- [ ] Admin account creation and changes

Rules, sent to the triage agent:
- [ ] New edge admin account
- [ ] Admin sign-in from outside the management network
- [ ] Config or firmware change with no matching change ticket

## 6. Respond

| Situation | Action | Automation |
|---|---|---|
| Suspicious edge admin activity | Disable the edge admin account, network block the source IP | Recommend (triage agent) |
| Credentials possibly harvested through the device | Revoke sessions, reset password and MFA for affected users | Recommend |
| Confirmed device compromise | Fail over or take offline, rebuild | Human only |

## Deferred (the remaining 20%)
Network flow anomaly detection from edge devices, automated config drift monitoring, and full packet capture.
