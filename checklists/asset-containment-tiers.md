# Asset Containment Tiers: the can/cannot auto-quarantine list

**Critical path.** No company has a reliable server inventory, so we build one from the EDR, which is the best inventory we have and the place the tags have to live anyway.

## The three tiers

| Tier | Meaning | Automated response | Typical members |
|---|---|---|---|
| **A: Auto-isolate** | Isolating it hurts one person or nothing critical | Full network isolation in seconds | All workstations and laptops, VDI, non-prod servers, redundant member servers |
| **B: Contain-lite** | Outage hurts, but a short one is survivable | Block lateral + egress, kill process, disable user; page a human with one-click full isolation | Production app servers, file servers, web servers, unknown servers (default) |
| **C: Never auto-isolate** | Isolating it causes a bigger outage than most attacks | Page immediately; contain the **identity** instead of the host; follow runbook M-02 | Domain controllers, hypervisors, IdP/sync servers, PKI, EDR/SIEM/SOAR infrastructure, core network/DNS/DHCP, OT/medical/life-safety, backup servers |

> **The 80/20:** workstations are where pulled-in attacks start, and they can be tier A on **day one** with no inventory work. That covers most hosts at every company. All the inventory work is about servers.

## Checklist

### Day 1: workstations (hours, not weeks)
- [ ] Get sign-off (F-01) that all workstations are tier A
- [ ] List workstation exceptions only: exec assistants on call, kiosks, clinical/shop floor, shared control-room PCs. Tag them B
- [ ] Tag all other workstations A in the EDR (by OS type: workstation vs server)
- [ ] Test isolate + release on one workstation

### Week 1: servers, first pass
- [ ] Export all servers from the EDR (hostname, OS, IP, OU, last user, running services/processes)
- [ ] Default **every server to B**. Unknown servers are never auto-isolated and never ignored
- [ ] Auto-identify tier C by signal, not by asking people:
  - [ ] Domain controllers (DC role / OU)
  - [ ] Hypervisors and virtualization management
  - [ ] IdP connectors and sync servers (AD sync, Okta agents, ADFS)
  - [ ] PKI / certificate authorities
  - [ ] Security tooling (EDR, SIEM, SOAR, vulnerability scanners, PAM)
  - [ ] DNS, DHCP, NAC, core network management
  - [ ] Backup servers and storage management
  - [ ] Anything in OT / medical / building systems segments
- [ ] Auto-identify tier A candidates: hostnames/OUs containing dev, test, qa, stage, lab, sandbox, or older than X days with no logons

### Weeks 2–4: owner confirmation (the only part that needs people)
- [ ] Send each app owner a short list with one question: *"If we see credential theft on this server, can we cut its network in under 60 seconds without asking you?"* Yes → A, No → B, "It'll break the business" → C
- [ ] No answer within 2 weeks = stays B
- [ ] Record tier, owner and reason in the client's `asset-tiers.csv`, and push tags into the EDR

### Ongoing
- [ ] New servers default to B (EDR group/policy default)
- [ ] Quarterly: review tier C list (it should be short, under ~10% of servers), and re-ask owners for B servers
- [ ] Automation reads the EDR tag at action time. A missing tag means B

## Same idea for identities (F-03)

| Tier | Members | Automated response |
|---|---|---|
| A | Standard human users | Revoke sessions, disable, force reset |
| B | Executives, IT admins | Revoke sessions + page; disable needs one click |
| C | Break-glass, service accounts, IdP/sync accounts | Page only. Disabling them can take down production |
