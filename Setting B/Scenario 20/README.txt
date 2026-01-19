Scenario 20 (SC20) — Setting B (initially infeasible -> repaired)

Defence / military mission enclave focused on ransomware-like disruption of mission-support services.

Network size: 46 devices, 10 VLAN segments.
Incident type: disruptive malware/ransomware-like impact on mission data and logistics services.

Why initial RDS is infeasible (plannerOutput.json):
- Aggressive requirement to disconnect the Mission-Data segment S3 by bucket 1 (10 minutes) and keep it isolated,
  while also mandating multiple credential resets and host containment in the first 30 minutes.
- Only one responder team is available during buckets 0–5, creating an unschedulable early workload peak.

Repair (plannerOutput_repaired.json):
- Minimal mission-safe edits suggested by MIROE (repairDiagnostic.json):
  (i) widen one non-critical LFT for a credential reset by 1 bucket, and
  (ii) relax a fixed reconnect pin into a window (keep earliest reconnect the same).
- Mission availability guarantees (missionandpolicy.json) remain unchanged.

Files:
- missionandpolicy.json: mission and policy goals (kept separate from RDS).
- plannerOutput.json: initial (infeasible) RDS.
- repairDiagnostic.json: conflict summary + ranked minimal repairs.
- plannerOutput_repaired.json: repaired (feasible) RDS.
- Other JSONs: topology, segments, services, criticality, actions, dependencies, initialImpact, resources.
