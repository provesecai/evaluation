# Scenario 7 (SCN7) — Setting A — Credential Compromise & Identity-Focused Response

## What this scenario represents
Scenario 7 models a defence-style enclave experiencing **credential compromise with suspected privilege escalation**. The attack is represented as an **impacted user endpoint** and an **at-risk identity controller**, consistent with a credential theft / token abuse narrative.

- **Setting:** A (more mission-availability constrained)
- **Incident type:** `credential_compromise_privilege_escalation`
- **Time model:** 10-minute buckets over a 12-hour horizon (**72 buckets**)
- **Network size:** 32 devices across 7 VLAN segments

The response posture prioritises **identity hardening** (lockdowns + credential resets) and **selective containment** of the suspected endpoint, while avoiding disruption to mission-critical services during their protected availability windows.

## Missions (mission goals)
Mission goals are defined in `missionandpolicy.json` and include:

### Required services
- `SV_GW` (Guarded Gateway)
- `SV_C2` (Command and Control)
- `SV_ID` (Identity and Authentication)
- `SV_SITREP` (SITREP Updates)

### Minimum availability windows (T_avail)
Disruptive actions on devices that support required services must not start before:
- `SV_GW`: bucket **24**
- `SV_ID`: bucket **24**
- `SV_SITREP`: bucket **36**
- `SV_C2`: bucket **42**

These are enforced operationally by the SRO through **RDS time windows** (EST/LFT) and admissibility choices, so disruptive recovery actions on mission-critical support sets are delayed unless explicitly authorised.

## Policies (policy goals)
Policies are also defined in `missionandpolicy.json`:

- **Protected data types:** `DT_OPORD`, `DT_INTEL`, `DT_KEYS`
- **Policy intent:** Prefer **non-destructive containment** and **identity hardening first**. Avoid disruptive recovery on protected-data hosts unless required for containment.

## Where to look in the files
- `missionandpolicy.json`: Mission availability guarantees (T_avail), required services, and protected data policy.
- `plannerOutput.json`: The **RDS** (enabled actions, time windows, directives, weights/rewards) compiled from mission/policy reasoning.
- `initialImpact.json`: Initial impacted / at-risk devices (`D14`, `D06`) and health degradation used to represent incident state.
- `segments.json`, `devices.json`, `topology.json`: Network structure and movement pathways (credential paths emphasised: endpoints → identity → mission tiers).

