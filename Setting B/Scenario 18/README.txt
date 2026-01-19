Scenario SC18 (Setting B: initially infeasible -> repaired)

Incident type: gateway-adjacent foothold and lateral movement pressure.
Network: 36 devices across 8 VLAN segments (C2, Identity, Mission Data, Logistics, DMZ, Guarded Gateway, SOC, User endpoints).
Mission goal: keep C2 online until bucket 24, gateway connectivity until bucket 12, and identity services until bucket 6.

Why infeasible initially: the first RDS mandates DMZ isolation and several credential resets with very tight completion windows (LFT=4) while only one responder team
is available early. MIROE diagnoses the conflicting RDS items and suggests minimal repairs.

Repair applied: widen LFT for two lower-criticality reset actions (to bucket 10) and slightly widen one more (to bucket 8). Mission availability constraints and
admissible action set are unchanged; the updated RDS becomes feasible.
