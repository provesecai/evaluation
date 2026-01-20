# Scenario 8 (Setting B) — Gateway-adjacent foothold with lateral-movement pressure

**Scenario ID:** `SCN8_SETTING_B_GATEWAY_ADJACENT_LATERAL_MOVEMENT`  
**Setting:** B (aggressive containment / tighter segment pins)  
**Incident type:** `lateral_movement_gateway_adjacent`  
**Time model:** 10-minute buckets, 12-hour horizon (72 buckets)

## What this scenario represents
A compromise begins in the **DMZ** (impacted host `D08`) with **lateral movement attempts through a transit tier** (at-risk `D15`). Because the foothold is adjacent to the gateway/transit area, the defender response prioritises **rapid network segmentation** (disconnecting the Transit and DMZ segments early) plus **identity hardening** (lockdowns + credential resets) and **increased monitoring**.

The RDS (`plannerOutput.json`) encodes these actions as admissible actions with time windows, along with weights/rewards that guide MIROE.

## Missions (missionGoal)
The mission goal requires maintaining availability for the following services:

- **SV_GW (Guarded Gateway Connectivity)**: must remain available until **bucket 18** (≈ 3 hours)
- **SV_C2 (Command and Control)**: must remain available until **bucket 30** (≈ 5 hours)
- **SV_ID (Identity and Authentication)**: must remain available until **bucket 18**
- **SV_MSN_DATA (Mission Data Dissemination)**: must remain available until **bucket 24** (≈ 4 hours)

**Interpretation used by the SRO:** disruptive actions on devices that directly support these required services are delayed until after the service’s min-availability time unless explicitly authorised. Containment via **segment isolation** is allowed when it does not directly take required-service devices offline.

You can see this reflected operationally in the RDS time windows, e.g. `recover_host` on **C2 device `D04`** has **EST = 30**, matching `SV_C2`’s availability guarantee.

## Policies (policyGoal)
Policy focuses on protecting sensitive data types:

- `DT_OPORD` (operations orders)
- `DT_INTEL` (intelligence)
- `DT_KEYS` (cryptographic/identity keys)

**Policy note:** avoid destructive/disruptive recovery on protected-data hosts unless required for containment; prefer segmentation and identity hardening first.

## Contents of this ZIP
- `missionandpolicy.json` — mission + policy goals (kept separate from RDS)
- `plannerOutput.json` — RDS: admissible actions + time windows + weights/rewards
- `devices.json`, `segments.json`, `services.json`, `serviceCriticality.json`, `topology.json` — network model
- `actions.json`, `dependencies.json`, `resources.json`, `initialImpact.json` — action catalogue, constraints, staffing, initial state
- `riskWeights.json`, `weights.json` — objective/risk weight parameters
