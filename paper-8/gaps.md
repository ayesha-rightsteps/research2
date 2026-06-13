# Research Gaps: GAT-A2C for C-V2X Resource Allocation (sensors-25-05209)

## Gaps the Authors Themselves Admit
- O(n³H) complexity — not scalable to very high vehicle counts in real time; latency increases with n
- Assumes 100% connected vehicle penetration rate — in practice, most vehicles are non-connected
- No explicit future work section; limitations mentioned informally
- Simulation only on Manhattan grid — one specific urban topology

## Gaps the Authors Did NOT Mention (Your Analysis)
- **Perfect CSI assumption:** GAT uses precise channel state information for attention weights. Real V2X channels have estimation errors, especially at high speeds — the model's sensitivity to CSI error is never tested.
- **Static attention = static graph snapshot:** GAT computes attention weights based on a snapshot of the interference graph. In 100 km/h traffic, this graph changes faster than the inference cycle — the model doesn't account for graph dynamics between decisions.
- **No non-connected vehicles (the 100% penetration problem is huge):** The paper itself admits 100% penetration assumption — but current real-world C-V2X penetration is <5%. A network with 95% non-connected vehicles behaves completely differently, making the interference graph sparse and partially observable.
- **A2C vs. other actor-critic methods not ablated:** Why A2C specifically? PPO and SAC are typically more stable and sample-efficient. No ablation of the RL algorithm choice is provided.
- **No comparison with MARL approaches:** Each vehicle runs a local A2C agent, but there's no centralized coordination. MARL methods (QMIX, MAPPO) that coordinate decisions globally could outperform independent A2C agents.
- **Manhattan grid only:** Manhattan grid has a very regular interference pattern. Real urban environments (irregular block sizes, one-way streets, highways merging with city roads) have different topologies that could break the learned attention patterns.
- **No V2P (vehicle-to-pedestrian) links:** Modern C-V2X networks include pedestrian safety as a priority use case — this is entirely absent from the model.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing GAT-based resource allocation for C-V2X assumes full vehicle connectivity, but real-world penetration rates below 10% create a sparse, partially observable interference graph — current attention mechanisms fail because most nodes have no V2X signal to attend to; a robust sparse-graph attention approach is needed."

> **PS-2:** "Graph Attention Networks for V2X resource allocation are computed on static per-slot graph snapshots, but at vehicle speeds of 100 km/h the interference topology changes significantly within a single slot, causing attention weight staleness — a temporal graph attention network that models graph dynamics is needed."

> **PS-3:** "Current V2X resource allocation methods based on GAT assume perfect CSI for attention computation, but CSI estimation errors at high vehicular speeds degrade attention weights and cause resource collision — a CSI-uncertainty-aware graph attention mechanism with robust attention under noisy edge weights is needed."

## Most Promising Gap (Your Pick)
**PS-1 (low penetration rate / sparse graph)** is the most practically relevant gap. The paper explicitly admits 100% penetration assumption — which is completely unrealistic. In 2025–2030, C-V2X penetration will be in single digits. A network where only 1 in 10 vehicles is V2X-capable has a fundamentally different interference structure: sparse graphs, dominant non-V2X interference, mixed decisions for connected vs. non-connected. This is a gap nobody in the current literature seriously addresses, it's technically challenging, and it's the gap between lab results and real deployment.
