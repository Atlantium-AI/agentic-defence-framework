---
name: telemetry-health
description: Daily check that every critical log source and reference data table is flowing, fresh and complete. Flags silent sources, ingest lag, sudden volume drops, unhealthy EDR sensors and stale tables. Use on a daily schedule or when an investigation hits missing data.
---

# Telemetry Health

## Log sources

For each source in the client's critical log source list:
- [ ] **Silent:** no events in the last N minutes, where N depends on the source's normal volume
- [ ] **Lagging:** ingest time minus event time > 15 minutes
- [ ] **Dropping:** today's volume < 50% of the same weekday's average (partial outage, filter change, broken forwarder)
- [ ] **Partial:** a subset went quiet (one DC, one region, one cloud account, one firewall)

## EDR sensors

- [ ] Hosts whose sensor hasn't checked in for > 1h but that are still seen in DHCP, DNS, IdP or firewall logs. **Highest priority: this is the EDR tampering signature.**
- [ ] Sensors in degraded, reduced-functionality or detect-only mode
- [ ] Sensor versions more than N releases behind

## Reference data tables

For each table (asset tiers, identity tiers, exceptions, canary registry, ...):
- [ ] Last refresh within its refresh window
- [ ] Row count change > 20% from last run (broken source or real change: say which)
- [ ] % of hosts/identities with no row (falling back to the safe default)
- [ ] Decoy health: decoys still present, canary accounts still disabled, auditing still on

## Output

Open one ticket per owner, worst first. A silent sensor on a host still on the network is paged to on-call, not ticketed.
