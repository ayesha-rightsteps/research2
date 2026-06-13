# Research Gaps: ISAC-Enabled UAV Vehicular Network (sensors-25-07295)

## Gaps the Authors Themselves Admit
- Single UAV only — multi-UAV coordination not addressed
- Simplified channel model — Rician fading assumed with fixed parameters; shadowing effects not modeled
- Perfect CSI assumed — channel estimation errors not considered
- Static traffic congestion scenario — dynamic vehicle entry/exit not modeled
- Sensing constraint is per-vehicle minimum MI — more sophisticated sensing objectives (tracking accuracy, resolution) not explored

## Gaps the Authors Did NOT Mention (Your Analysis)
- **BCD-SCA convergence speed not practical:** The iterative BCD-SCA algorithm converges to a local optimum, but the number of iterations required per time slot is never reported against real-time requirements. If each slot is 10ms and BCD-SCA takes 50 iterations of convex optimization, the approach is non-deployable.
- **One GBS only — highly unrealistic:** Real vehicular scenarios have multiple overlapping GBSs. The paper assumes a single GBS + one UAV. With multiple GBSs, inter-cell interference fundamentally changes the problem.
- **ISAC function on UAV adds SWaP constraints:** Size, Weight, and Power — radar hardware on UAVs is heavy. The paper ignores the additional power consumption and weight penalty of ISAC hardware on a UAV, which directly reduces flight endurance.
- **Sensing MI as proxy for actual sensing utility:** Mutual Information is used as the sensing quality metric, but what vehicles actually need is target localization accuracy, velocity estimation, or object detection confidence. MI doesn't directly translate to these application-level metrics.
- **No interference between UAV radar and vehicle communication:** UAV's radar signal could interfere with V2V communication links below. This self-interference in the network (from ISAC) is not modeled.
- **Fixed UAV speed range (6–12 m/s):** Real UAV operational speeds for fixed-wing drones are much higher; for multirotor UAVs serving urban scenarios, battery life at these speeds is never evaluated.
- **No comparison with learning-based methods:** All baselines are optimization-based. DRL-based joint ISAC optimization (emerging literature) is not compared.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing BCD-SCA approaches for ISAC-UAV vehicular networks solve trajectory and resource allocation alternately until convergence, but the iteration count required per time slot exceeds real-time decision budgets in dense scenarios — a single-shot deep learning approximation of the BCD-SCA policy that maintains near-optimal performance with sub-millisecond inference is needed."

> **PS-2:** "Current UAV-ISAC resource allocation uses Mutual Information as a sensing proxy, but vehicles require localization accuracy and object detection confidence — existing MI-constrained optimization does not guarantee application-level sensing utility, and a task-oriented sensing metric for ISAC resource allocation is needed."

> **PS-3:** "Single-UAV ISAC vehicular network models miss the cooperative sensing gains and interference management challenges that arise when multiple ISAC-enabled UAVs share spectrum — a multi-UAV ISAC resource allocation framework with cooperative sensing and inter-UAV interference management is needed."

## Most Promising Gap (Your Pick)
**PS-1 (real-time feasibility of BCD-SCA)** addresses the most fundamental deployment barrier. This paper, like many optimization-based works, proves that BCD-SCA finds good solutions but never asks "fast enough for what?" The 5G/6G V2X timeline requires millisecond decisions. A follow-up that trains a neural network (e.g., DNN or GNN) to mimic BCD-SCA's output in a single forward pass — with provable approximation bounds — would be both practically impactful and theoretically interesting. This "learning to optimize" direction is hot in communications research right now.
