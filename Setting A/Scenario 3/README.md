# Scenario 3 (SCN3_SETTING_B_GATEWAY_ADJACENT_LATERAL_MOVEMENT)

**Setting:** B (tighter early resourcing)  
**Incident type:** (ii) gateway-adjacent foothold driving lateral movement risk → rapid segment isolation + staged reconnection  
**Time model:** 10-minute buckets, 72 buckets (12 hours)

## What’s in this package
This folder contains the full Scenario 3 input set, following the same separation used in Scenario 1:

- **missionandpolicy.json** — mission + policy goals only (NOT part of the RDS)
- **plannerOutput.json** — **RDS only**: admissible actions (E), time windows (EST/LFT), and weights/rewards
- Other files are static/organisational inputs (topology, segments, devices, services, actions, dependencies, resources, initial impact, etc.)

## Mission goal (high level)
Required services and minimum availability windows (T_avail) are declared in `missionandpolicy.json`:
- Required services: **SV_C2, SV_GW, SV_RADIO, SV_LOG**
- Minimum-availability-until buckets:
  - SV_GW: 18
  - SV_C2: 30
  - SV_LOG: 24
  - SV_RADIO: 36

**How this shows up in the RDS:** mission availability is *compiled* into `plannerOutput.json` by delaying or forbidding disruptive actions on required-service supporting devices (via admissibility choices and/or EST/LFT bounds). Example in this scenario:
- SV_C2 has `minAvailabilityUntilBucket = 30` → disruptive recovery is delayed: **D02 recover_host EST = 30** in `plannerOutput.json`.

## Network shape
- **Devices:** 40 (`devices.json`)
- **Segments/VLANs:** 11 listed in `segments.json` (S1–S11)
- **Topology:** `topology.json` includes movement difficulty **γ** on edges, emphasising DMZ → gateway/identity/admin paths.

## Response setting (resources)
Setting B is reflected in `resources.json`:
- 1 responder team from bucket 0–12
- 2 responder teams from bucket 12–72

## Files
- missionandpolicy.json
- topology.json
- segments.json
- devices.json
- services.json
- serviceCriticality.json
- timeBuckets.json
- actions.json
- dependencies.json
- resources.json
- initialImpact.json
- riskWeights.json
- weights.json
- mandatory.json (organisational input; NOT part of RDS)
- impactedOnly.json (organisational input; NOT part of RDS)
- plannerOutput.json (RDS)
