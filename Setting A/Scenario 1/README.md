# Scenario 1 (SCN1_SETTING_A_CRED_THEFT_IDENTITY_TIER)

This bundle contains the complete JSON inputs for **Scenario 1**.

## Scenario summary
- **Setting:** A
- **Incident type:** credential compromise + privilege escalation (identity-tier focused)
- **Network size:** 32 devices, 7 VLAN segments
- **RDS design rule:** `plannerOutput.json` contains **only** (i) admissible actions + time windows, and (ii) weights/rewards.

## Mission & policy goals (missionandpolicy.json)
**Mission-required services:** `SV_C2`, `SV_GW`, `SV_ID`

**Service availability guarantees (T_avail):**
- `SV_ID` available until **bucket 12**
- `SV_GW` available until **bucket 18**
- `SV_C2` available until **bucket 24**

**Policy protections:** protect data types `DT_OPORD`, `DT_INTEL`, `DT_KEYS` and avoid disruptive/destructive actions on protected-data hosts unless required for containment.

## How mission constraints appear in the RDS
Mission availability guarantees are declared in `missionandpolicy.json` and compiled into `plannerOutput.json` primarily via **EST/LFT** constraints and admissibility selection.

Example: `SV_C2` is guaranteed until bucket **24**, and the RDS reflects this by delaying disruptive recovery on a C2 device:
- `D02 recover_host` has `EST = 24`.

## Files included
- `missionandpolicy.json`
- `topology.json`
- `segments.json`
- `devices.json`
- `services.json`
- `serviceCriticality.json`
- `timeBuckets.json`
- `actions.json`
- `dependencies.json`
- `resources.json`
- `initialImpact.json`
- `riskWeights.json`
- `weights.json`
- `mandatory.json`
- `impactedOnly.json`
- `plannerOutput.json`
