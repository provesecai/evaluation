# Scenario 6 (SCN6) — Setting B — Gateway-adjacent lateral movement

This bundle contains the full JSON set for **Scenario 6**, a defence / mission enclave network where an initial foothold in the **DMZ near the guarded gateway** creates credible pressure for **rapid segment isolation** and **staged reconnection**.

- **Scenario ID:** `SCN6_SETTING_B_GATEWAY_ADJACENT_LATERAL_MOVEMENT`
- **Setting:** **B** (more aggressive containment, tighter early staffing)
- **Incident type:** `lateral_movement_gateway_adjacent`
- **Time model:** 10-minute buckets over a 12-hour horizon (**72 buckets**)
- **Network size:** **48 devices**, **11 VLAN segments**

## What the scenario is about
An attacker foothold on **DMZ web node D20** is assumed (see `initialImpact.json`). The network topology encodes plausible paths from DMZ → gateway → admin jump hosts → identity, and from identity into mission services (C2, SITREP, radio management, mission data). Setting B emphasises **rapid isolation** of key segments (gateway + DMZ) while keeping mission services available.

## Missions (missionGoal)
Mission requirements are defined in **`missionandpolicy.json`** and are *not* stored inside the RDS.

### Required services
`SV_C2`, `SV_GW`, `SV_ID`, `SV_SITREP`, `SV_RADIO_MGMT`

### Minimum availability windows (T_avail)
The mission requires each service to remain available until a minimum bucket time:
- `SV_GW`: available until **bucket 18**
- `SV_ID`: available until **bucket 18**
- `SV_RADIO_MGMT`: available until **bucket 24**
- `SV_SITREP`: available until **bucket 30**
- `SV_C2`: available until **bucket 36**

### Interpretation
Disruptive actions affecting devices that support required services are delayed until after the service’s `minAvailabilityUntilBucket`, unless explicitly authorised.

## Policies (policyGoal)
Policies are defined in **`missionandpolicy.json`**:
- **Protected data types:** `DT_OPORD`, `DT_INTEL`, `DT_KEYS`, `DT_CRYPTO_MAT`
- **Policy intent:** avoid destructive/disruptive actions on protected-data hosts unless required for containment; prefer isolation and identity hardening first.

## How mission/policy appear in the RDS
The RDS is provided in **`plannerOutput.json`** and contains:
- admissible actions (`enabledActions`)
- time windows (`timeWindows`)
- objective shaping weights (`segmentIsolationPenalties`, `deviceUrgency`, `actionRewards`)

Mission/policy goals are compiled into the RDS by:
- **not enabling** (or not scheduling) disruptive actions on required-service devices during their guaranteed availability windows
- using **EST/LFT bounds** and **segment directives** to enforce urgent containment actions (e.g., segment disconnect deadlines) while preserving required services

## File list
- `missionandpolicy.json` — mission + policy goals
- `timeBuckets.json` — bucket definition and horizon
- `segments.json`, `devices.json`, `services.json`, `serviceCriticality.json` — network and service model
- `topology.json` — connectivity / lateral movement difficulty (gamma)
- `actions.json`, `dependencies.json`, `resources.json` — action catalog, precedence, staffing
- `initialImpact.json` — initial compromised / at-risk state
- `riskWeights.json`, `weights.json` — risk and objective weighting
- `plannerOutput.json` — RDS (admissible actions + time windows + weights/rewards)
