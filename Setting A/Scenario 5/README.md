# Scenario 5 (SCN5) — Setting B: Credential compromise + privilege escalation

This bundle contains the complete JSON inputs for **Scenario 5**, structured so that:
- **Mission + policy goals** live in `missionandpolicy.json`.
- The **RDS** (admissible actions + time windows + weights/rewards only) lives in `plannerOutput.json`.

## Scenario summary
- **Setting:** B (tighter staffing/authorisation constraints; 1 responder team early, 2 later).
- **Incident type:** credential compromise with risk of privilege escalation.
- **Network size:** 40 devices across **9 VLAN segments**.
- **Core response idea:** perform **rapid identity hardening** (identity lockdown + targeted credential resets) and **selective containment** of the suspected compromised endpoint, while preserving mission-critical services.

## Missions (missionGoal)
Mission availability guarantees are expressed as *service-level minimum availability windows* and supporting device sets.
The required services are:
- `SV_C2` (Command and Control)
- `SV_GW` (Guarded Gateway Connectivity)
- `SV_SITREP` (Operations / SITREP Updates)
- `SV_ID` (Identity and Authentication)

Service availability guarantees (`T_avail`, in 10-minute buckets):
- `SV_ID` available until **bucket 18**
- `SV_GW` available until **bucket 24**
- `SV_SITREP` available until **bucket 30**
- `SV_C2` available until **bucket 36**

Interpretation (from `missionandpolicy.json`):
> Disruptive actions on devices supporting required services are delayed until after that service's min-availability time unless explicitly authorised.

## Policies (policyGoal)
Protected data types:
- `DT_OPORD`, `DT_INTEL`, `DT_KEYS`

Policy notes:
- Avoid destructive/disruptive actions on devices hosting protected data unless required for containment.

## How mission/policy are compiled into the RDS
The RDS (`plannerOutput.json`) reflects mission/policy **without embedding missionGoal/policyGoal directly** by:
- **Admissibility pruning:** no disruptive actions are enabled for mission-critical gateway/C2/identity servers during their guaranteed windows.
- **Time-window shaping (EST/LFT):** identity actions are allowed early (modeled as non-destructive containment), while segment isolation and monitoring actions are used to reduce spread without violating service availability.

## Files
- `missionandpolicy.json`: mission + policy goals (authoritative).
- `plannerOutput.json`: RDS (enabled actions, time windows, penalties, urgency, rewards).
- Supporting scenario structure: `devices.json`, `segments.json`, `services.json`, `serviceCriticality.json`, `topology.json`, `actions.json`, `dependencies.json`, `resources.json`, `initialImpact.json`, `riskWeights.json`, `weights.json`, `timeBuckets.json`.
