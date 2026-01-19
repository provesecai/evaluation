# Scenario 9 (SCN9) — Setting A — Ransomware-like Service Degradation

This package contains **Scenario 9** in the same split you requested:

- `missionandpolicy.json` contains **mission and policy goals**.
- `plannerOutput.json` is the **RDS** (admissible actions + time windows + objective weights/rewards), with mission/policy constraints already compiled into EST/LFT bounds.

## What the scenario represents

- **Setting:** A (balanced response; fewer hard pins)
- **Incident type:** disruptive malware / **ransomware-like service degradation**
- **Time model:** 10-minute buckets over a 12-hour horizon (**72 buckets**)
- **Network:** 32 devices across 7 VLAN segments

The attack is modelled as rapid spread from **user endpoints → mission-support apps → mission data / identity**, with a focus on **prioritised recovery** while respecting mission availability windows.

## Missions (from `missionandpolicy.json`)

### Required services
The mission requires maintaining availability of:
- `SV_GW` (Gateway Connectivity)
- `SV_C2` (Command and Control)
- `SV_ID` (Identity and Authentication)
- `SV_MSN_DATA` (Mission Data Dissemination)

### Minimum-availability windows
For each required service, disruptive recovery actions on its supporting devices must not start before:
- `SV_GW`: **bucket 12** (≈ 120 minutes)
- `SV_ID`: **bucket 18** (≈ 180 minutes)
- `SV_MSN_DATA`: **bucket 18** (≈ 180 minutes)
- `SV_C2`: **bucket 24** (≈ 240 minutes)

These windows are enforced operationally in the RDS via EST/LFT constraints. Example:
- `recover_host` on **D03** (C2-supporting) has **EST = 24** in `plannerOutput.json`.
- `restore_from_backup` on **D08** (mission-data supporting) has **EST = 18** in `plannerOutput.json`.

## Policies (from `missionandpolicy.json`)

- Protected data types: `DT_OPORD`, `DT_INTEL`, `DT_KEYS`.
- Policy intent: **avoid destructive recovery** on protected-data hosts unless required to stop spread; prefer **staged containment**, **identity hardening**, and **restoration from trusted backups**.

## Where to start

- `initialImpact.json` shows initially impacted devices (endpoint + portal + logistics + mission share) and their degraded health.
- `plannerOutput.json` shows the admissible response plan space:
  - flexible containment on the user segment (`S6_USERS`)
  - prioritised recovery/restore actions for degraded services
  - identity hardening (`lockdown_identity`) to reduce reinfection and privilege abuse
