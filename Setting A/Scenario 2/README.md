# Scenario 2 (SCN2_SETTING_B_GATEWAY_FOOTHOLD)

## Summary
**Setting B** — gateway-adjacent foothold with lateral-movement pressure. The SRO expresses mission + policy intent in `missionandpolicy.json` and compiles it into the **RDS** (`plannerOutput.json`) via admissibility `E` and action-level time windows `EST/LFT`.

- **Time model:** 10-minute buckets, horizon **72 buckets** (12 hours).
- **Scale:** **32 devices**, **7 VLAN segments**.
- **Initial impact (I0):** gateway foothold on **D07** and suspected credential abuse on **D18**.

## Mission and policy goals (not part of RDS)
From `missionandpolicy.json`:
- **Required services:** `SV_C2`, `SV_GW`
- **Availability guarantees:**
  - `SV_C2` available until **bucket 24** (first 4h)
  - `SV_GW` available until **bucket 18** (first 3h)
- **Protected data types:** `DT_OPORD`, `DT_INTEL`, `DT_KEYS`

## How the mission goal is enforced in the RDS
From `plannerOutput.json`:
- **Immediate containment:** `S1_GW segment_disconnect` is allowed early (`EST=0`) with a directive to finish by bucket 2.
- **Staged reconnection:** `S1_GW segment_reconnect` delayed (`EST=6`) and bounded to complete by bucket 18.
- **Disruptive host actions delayed by availability:** `D07 recover_host` is delayed until **bucket 18** (`EST=18`) to respect `SV_GW`’s availability window.
- **Identity hardening allowed immediately:** `lockdown_identity` and `reset_credentials` on selected devices are permitted with early windows.

## File list
- Organisational/static inputs: `timeBuckets.json`, `services.json`, `serviceCriticality.json`, `segments.json`, `devices.json`, `topology.json`, `riskWeights.json`, `initialImpact.json`, `resources.json`, `actions.json`, `dependencies.json`, `mandatory.json`
- Mission/policy: `missionandpolicy.json`
- RDS: `plannerOutput.json`
