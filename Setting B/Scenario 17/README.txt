Scenario 17 (SC17) — Setting B (infeasible-then-repaired)

Defence enclave with 42 devices and 9 VLAN segments (10-minute buckets, 12-hour horizon). Incident is ransomware-like disruption affecting a mission data repository (D11) plus a compromised jump host (D20) and one user endpoint.

Mission & policy are in missionandpolicy.json. The SRO emits an RDS (plannerOutput.json) containing admissible actions, time windows, and optimisation weights.

Why the initial RDS is infeasible:
- The segment directive for S2_DATA sets maxIsolationBuckets=8, but also sets earliest reconnect at bucket 10.
- The segment cannot remain isolated long enough to reach its own reconnection window.

MIROE diagnostic & repair:
- repairDiagnostic.json reports the conflicting RDS items and recommends the minimal mission-safe edit: increase maxIsolationBuckets to 12.
- plannerOutput_repaired.json applies that single change; the repaired RDS becomes schedulable while preserving action admissibility and mission availability windows.
