Scenario 19 (SC19) — Setting B (initially infeasible → repaired)

Defence / military mission enclave with a guarded gateway and identity tier.
- Size: 42 devices, 10 VLAN segments
- Incident type: gateway-adjacent foothold + credential compromise pressure (identity hardening early)
- Setting B: the initial RDS is intentionally conservative (tight pins + many early mandatory identity actions)
  and is infeasible under limited responder capacity. MIROE produces a conflict set and minimal repairs.

Files:
- topology.json, segments.json, services.json, serviceCriticality.json
- missionandpolicy.json (mission + policy goals; NOT part of RDS)
- initialImpact.json (I0 + current device health)
- resources.json, actions.json, dependencies.json
- plannerOutput.json (RDS — infeasible Strict Mode)
- repairDiagnostic.json (MIROE feedback + ranked minimal repairs)
- plannerOutput_repaired.json (RDS after applying rank-1 repair)
