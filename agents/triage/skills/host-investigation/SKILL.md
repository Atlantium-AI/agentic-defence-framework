---
name: host-investigation
description: Investigate activity on a single endpoint or server from EDR and log telemetry. Builds the timeline 30 minutes either side of the alert, covering process tree, network, logons, files, persistence and credential access. Use when an alert involves a host.
---

# Host Investigation

## Collect (in parallel)

- [ ] `get_host`: OS, role, asset tier, sensor health, recent alerts, logged-on users
- [ ] `get_process_tree` for the alerting process, ±30 min
- [ ] Network connections from the process tree: destinations, ports, bytes, first-seen?
- [ ] Logons to the host ±24h: who, logon type (interactive, network, RDP), from where
- [ ] Files written by the process tree; download origin (Mark-of-the-Web)
- [ ] New persistence ±24h: services, scheduled tasks, run keys, WMI subscriptions, cron/systemd, authorized_keys
- [ ] Credential access: LSASS handles, hive saves, browser credential stores, decoy file reads

## Read the timeline

Look for the attacker sequence: **get the keys → blind the guards → move → cash out.**

| Stage | Signs |
|---|---|
| Recon | Bursts of discovery commands (whoami, net, nltest, systeminfo, AD queries). A burst inside 60 seconds suggests an automated agent or script |
| Keys | Credential access, decoy reads, Kerberos ticket requests |
| Guards | Security tools stopped, logs cleared, drivers loaded, exclusions added |
| Move | Outbound admin protocols (SMB, WinRM, RDP, PsExec, WMI) to other hosts, new remote access tools |
| Cash out | Archive creation, exfil tools, large uploads, shadow copy deletion |

## Questions to answer

1. What is the earliest malicious event, and what started it?
2. Which identities were used on or from this host during the window? (They're all potentially compromised.)
3. Did anything leave this host for another host? (List them; they're next.)
4. Is the sensor healthy for the whole window? If not, record the gap.
