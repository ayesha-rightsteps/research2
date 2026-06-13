# Research Gaps: Multi-UAV Joint Optimization for IoV (2605.04436)

## Gaps the Authors Themselves Admit
- SOCP trajectory optimization is computationally expensive — not real-time feasible for large networks
- LLM inference adds latency — the interaction cost between DRL and LLM at each decision step is not quantified
- Simulated urban environment only — no real-world or hardware validation
- Fixed number of UAVs (5) — dynamic UAV joining/leaving not considered
- Task deadlines are assumed fixed — variable deadline scenarios not studied

## Gaps the Authors Did NOT Mention (Your Analysis)
- **LLM is a black box in a safety-critical system:** The paper uses an LLM as a macro-scheduler, but LLMs can hallucinate, are non-deterministic, and lack formal safety guarantees. In autonomous driving scenarios, this is a serious reliability concern that is never discussed.
- **SOCP assumes perfect vehicle location knowledge:** The trajectory optimization requires knowing where all vehicles are. In real deployments, vehicle GPS has ~1-5m error and reporting delay — the effect of location uncertainty on SOCP trajectory quality is ignored.
- **No UAV collision avoidance:** 5 UAVs are optimizing trajectories independently — the paper doesn't address potential flight path collisions between UAVs, which is mandatory in real airspace.
- **Reward decoupling mechanism not ablated:** The paper introduces "reward decoupling" to isolate DRL from LLM interventions — but never ablates whether this mechanism actually helps. What happens without it? Is the LLM help worth the decoupling overhead?
- **LLM is not specified:** Which LLM is used? What prompt engineering? What happens if the LLM misreads the task state? These reproducibility gaps make the LLM component unverifiable.
- **No inter-UAV communication:** UAVs optimize trajectories sequentially (by index order), not cooperatively. Cooperative trajectory planning with inter-UAV communication could dramatically improve coverage.
- **IoT task types not varied:** All offloaded tasks have similar characteristics. Real IoV workloads mix small latency-sensitive tasks (safety alerts) and large throughput-sensitive tasks (HD maps) — the scheduler doesn't differentiate.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing DRL+LLM hybrid resource allocation systems for UAV-IoV do not quantify the inference latency cost of LLM invocation per decision step — and this latency may exceed the slot duration in real-time V2X scheduling, rendering the approach non-deployable; a lightweight LLM distillation or asynchronous invocation strategy is needed."

> **PS-2:** "Current multi-UAV trajectory optimization for IoV assumes perfect vehicle location information and solves UAV paths sequentially by index, missing cooperative coverage gains and causing performance degradation under GPS positioning errors — a cooperative, uncertainty-aware multi-UAV trajectory co-optimization is needed."

> **PS-3:** "Existing UAV-IoV task offloading systems treat all vehicle tasks with equal priority, failing to differentiate safety-critical V2V messages (1ms deadline) from infotainment tasks (100ms deadline) — a deadline-aware, priority-based offloading scheduler that co-optimizes UAV resources is needed."

## Most Promising Gap (Your Pick)
**PS-3 (priority-aware offloading)** is the most impactful and novel. The entire motivation of IoV is safety — autonomous driving decisions depend on real-time safety messages. Yet this paper (and most like it) treats all tasks uniformly. A scheduler that explicitly handles mixed-criticality workloads (safety-critical + infotainment) within the same UAV-IoV framework would be a genuinely new contribution. It extends directly from this paper's hierarchical framework (LP offloading step can be extended to priority-aware), cites the growing 6G and NR-V2X literature on QoS differentiation, and has clear real-world relevance.
