# Research Gaps: MARL Benchmarking for C-V2X (2603.06607)

## Gaps the Authors Themselves Admit
- Benchmark focuses on highway topologies (SUMO-generated) — urban intersection scenarios not included
- Fixed network size (4, 8, 16 agents) — scalability beyond 16 vehicles is not tested
- RRA problem only covers channel and power allocation — joint trajectory optimization is out of scope
- The open-source code is the benchmark, not a state-of-the-art algorithm — no new MARL method is proposed

## Gaps the Authors Did NOT Mention (Your Analysis)
- **Heterogeneous agent types not considered:** All vehicles are assumed to be identical agents with the same capabilities. Real V2X networks include pedestrians, cyclists, RSUs, and infrastructure with very different sensing/compute abilities — a benchmark for heterogeneous agents doesn't exist.
- **Only symmetric interference game formulation:** The SIG framework assumes all agents compete for the same resources. In real V2X, safety-critical V2V messages should preempt infotainment traffic — priority-based resource allocation is not captured by the interference game model.
- **No latency-aware metric:** The normalized return measures aggregate throughput/success rate but doesn't measure tail latency — whether the worst-case V2V message meets its deadline. This is the metric that matters most for safety applications.
- **SUMO highway only — no real traffic data:** The benchmark uses SUMO-simulated vehicle mobility from synthetic highway traces. Real vehicle traces (e.g., from GPS datasets) have very different statistical properties, and it's unknown whether conclusions hold on real data.
- **No transfer learning baseline:** The paper shows that policies don't generalize well across topologies, but doesn't test any transfer learning or domain adaptation approach as a potential solution — leaving the problem open without directions.
- **Energy consumption not modeled:** In edge computing V2X scenarios, the energy cost of inference and communication is not considered — especially relevant for battery-powered vehicles.
- **Reward shaping effects not studied:** The benchmark uses a fixed reward function; reward shaping and its effect on robustness is not explored — different reward structures could completely change the generalization ranking.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing MARL benchmarks for V2X resource allocation use homogeneous vehicle agents on synthetic SUMO highway traces, missing the performance degradation caused by heterogeneous agent types (vehicles, RSUs, pedestrians) and real-world traffic patterns — a heterogeneous-agent V2X benchmark with real GPS traces is needed."

> **PS-2:** "Current MARL policies for V2X fail to generalize across diverse vehicular topologies because they memorize interference patterns rather than learning topology-invariant features — a meta-learning or graph-based representation approach could enable zero-shot transfer to unseen road geometries."

> **PS-3:** "Existing V2X MARL benchmarks optimize for aggregate throughput but do not capture tail-latency violations of safety-critical V2V messages — a latency-deadline-aware benchmark and reward formulation is needed for safety-critical autonomous driving applications."

## Most Promising Gap (Your Pick)
**PS-2 (topology-invariant generalization)** is the most impactful and timely. This paper literally proves that generalization is the #1 bottleneck and shows which algorithms fail — but doesn't solve it. It's an invitation. A follow-up paper that proposes a graph-based or meta-learning approach that can zero-shot transfer to new topologies would directly answer this paper's call, cite it heavily, and have a clear, well-motivated baseline comparison to build from. The benchmark infrastructure is already open-sourced — you don't need to build the evaluation framework from scratch.
