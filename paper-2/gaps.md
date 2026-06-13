# Research Gaps: Lyapunov + Diffusion DRL for UAV Vehicular Networks (2507.20524)

## Gaps the Authors Themselves Admit
- CSI feedback delay is modeled simply — a fixed one-slot delay; variable/random delays are not considered
- Single UAV only — no multi-UAV coordination
- UAV follows vehicles along a fixed highway trajectory; 2D flight path is assumed (altitude adjustment only, not full 3D trajectory)
- Interference between multiple vehicles sharing channels is simplified
- No real hardware experiments — purely simulation-based

## Gaps the Authors Did NOT Mention (Your Analysis)
- **Diffusion inference cost at runtime:** The D3PG algorithm requires I denoising steps per time slot. In real-time UAV systems, each time slot may be milliseconds — the O(ILn²) inference cost per slot is never benchmarked against actual latency requirements. A system that's computationally too slow for real-time is not deployable, regardless of theoretical performance.
- **Single highway, single city:** All experiments use one specific highway in Xiamen. Whether the learned policy transfers to different road geometries (urban grids, intersections, rural areas) is never tested — this is a generalization gap.
- **Fixed UAV speed (50 km/h):** UAV speed is fixed to match vehicle speed. Real deployment requires speed adaptation; the effect of speed mismatch (UAV faster/slower than vehicles) is unexplored.
- **No handover mechanism:** When a vehicle moves out of UAV coverage range, there's no handover to ground base station modeled. This is a critical gap for continuous mobility scenarios.
- **Lyapunov queue stability not validated empirically in edge cases:** The Lyapunov virtual queue converges in theory, but under sudden traffic bursts (e.g., vehicles merging from a ramp), the energy queue may spike in ways not captured by the simulation.
- **Stochastic vs. deterministic CSI delay not compared:** The paper only tests one fixed delay value; it doesn't show how performance degrades as delay increases — which is essential for evaluating robustness.
- **Battery recharging / UAV replacement not modeled:** In long-term deployments, the UAV must land and recharge. The interaction between Lyapunov energy management and multi-mission planning is completely ignored.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing diffusion-based RL policies for UAV resource allocation incur O(ILn²) inference cost per time slot, which becomes infeasible in real-time deployments with sub-millisecond decision windows — a computationally-aware diffusion policy with adaptive denoising steps is needed."

> **PS-2:** "Current UAV-assisted vehicular resource allocation policies trained on single-road, single-city datasets fail to generalize to unseen road geometries without retraining, because they overfit to specific interference patterns — a topology-invariant policy representation is needed."

> **PS-3:** "Existing UAV resource management schemes model CSI delay as a fixed single-slot lag, but real feedback delays are variable and correlated with vehicle speed and channel coherence time — an approach that explicitly models delay distribution and adapts decisions accordingly would be more robust."

## Most Promising Gap (Your Pick)
**PS-1 (inference latency)** is the most practically critical and underexplored. The diffusion model is computationally expensive. Existing work (including this paper) evaluates quality of decisions but never measures whether the decision arrives in time. For 5G NR-V2X, scheduling decisions need to happen in microseconds to milliseconds. This is a concrete, measurable, real-world constraint that the paper completely ignores. Solving it (e.g., via early-exit diffusion, distillation, or adaptive denoising steps) would produce a paper with clear practical impact and direct extension from this work.
