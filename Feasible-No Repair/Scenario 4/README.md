# Scenario 4 — Setting A (Ransomware-like Mission-Support Recovery)

**Scenario ID:** `SCN4_SETTING_A_RANSOMWARE_MISSION_SUPPORT_RECOVERY`  
**Setting:** A (more permissive staffing/authorisation; 2 responder teams throughout)  
**Incident type:** Disruptive malware / ransomware-like degradation of mission-support services  
**Scale:** 28 devices across 6 VLAN segments  
**Time model:** 10-minute buckets, 72 buckets total (12 hours)

## Scenario narrative
A ransomware-like event begins in the **User/Endpoint tier** and causes rapid service degradation in **Logistics** and **Mission Data** systems (e.g., encrypted fileshares and disrupted logistics applications). The response prioritises **containment + eradication early**, then **staged recovery** of critical mission-support services.

## Missions (missionandpolicy.json)
The mission requires sustained availability of these services until specified buckets:
- **SV_GW (Gateway Connectivity):** available until bucket **18**
- **SV_C2 (Command & Control):** available until bucket **36**
- **SV_LOG (Logistics Planning):** available until bucket **24**
- **SV_MSN_DATA (Mission Data Dissemination):** available until bucket **30**

**Support sets (devices that underpin required services):**
- SV_GW → `D07`
- SV_C2 → `D01`, `D02`
- SV_LOG → `D15`, `D16`, `D17`
- SV_MSN_DATA → `D12`, `D13`, `D14`

**Operational interpretation:** disruptive actions on devices supporting a required service should not begin before that service’s `minAvailabilityUntilBucket`, unless explicitly authorised.

## Policies (missionandpolicy.json)
- **Protected data types:** `DT_OPORD`, `DT_INTEL`, `DT_KEYS`
- **Policy intent:** avoid destructive/disruptive actions on protected-data hosts unless required for containment.

## How mission/policy are reflected in the RDS (plannerOutput.json)
The RDS contains only:
- **Enabled (admissible) actions**,
- **Action time windows (EST/LFT)**, and
- **Weights/rewards** (urgency, isolation penalties, action rewards).

Mission availability guarantees are compiled into the RDS as **delayed EST/LFT** for disruptive recovery actions on affected mission-support services. Examples:
- `SV_LOG` availability until bucket **24** → `D15 recover_host` has `EST = 24`.
- `SV_MSN_DATA` availability until bucket **30** → `D13 recover_host` and `D14 recover_host` have `EST = 30`.

Early actions focus on:
- Endpoint containment/eradication (`D05`),
- Logistics containment/eradication (`D15`),
- Mission file share containment/eradication (`D13`),
- Identity hardening (`lockdown_identity` on `D03`, `D04`),
- Optional user-segment isolation with staged reconnection (`S6_USER`).

## Files included
- `missionandpolicy.json` — mission + policy goals
- `plannerOutput.json` — RDS (enabled actions + windows + weights/rewards)
- `timeBuckets.json`, `segments.json`, `devices.json`, `services.json`, `serviceCriticality.json`
- `topology.json`, `actions.json`, `dependencies.json`, `resources.json`, `initialImpact.json`, `riskWeights.json`, `weights.json`
