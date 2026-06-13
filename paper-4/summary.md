# Summary: Joint Optimization of Trajectory Control, Resource Allocation, and Task Offloading for Multi-UAV-Assisted IoV

**Authors:** Maoxin Ji, Qiong Wu, Pingyi Fan, Cui Zhang, Nan Cheng, Wen Chen, Khaled B. Letaief | **Year:** 2026 | **Published in:** IEEE (arXiv:2605.04436)

## What is this paper about?
Vehicles in dense cities need to offload compute-heavy tasks (object detection, HD map updates) to edge servers because their own on-board compute is limited. UAVs flying as mobile edge computing nodes can help — but you have three tightly coupled decisions to make: where should each UAV fly (3D trajectory), how should wireless resources be allocated among vehicles, and which tasks should go to which server (offloading ratio)? This paper proposes a hierarchical framework that solves all three jointly, using SOCP for trajectory, a novel DRL+LLM hybrid for resource allocation, and LP for task offloading.

## The Core Idea
Split the problem into a hierarchy. The "top" level uses SOCP (Second-Order Cone Programming) to plan optimal 3D UAV trajectories — including altitude, which is critical for LoS coverage. The "middle" level uses DRL for resource allocation, but adds an LLM (Large Language Model) as a semantic macro-scheduler that monitors failed and surplus tasks and redistributes resources for long-tail fairness. A reward decoupling mechanism prevents the LLM's interventions from corrupting the DRL training signal. The "bottom" level uses LP (Linear Programming) to precisely compute offloading ratios.

## What they did (Method)
Simulated a dense urban environment with 5 UAVs serving vehicles performing task offloading to edge servers. UAVs plan 3D trajectories over 20 time slots. DRL agent allocates communication resources (channel assignment + power) to vehicles. LLM (acting as a reasoning agent) identifies vehicles with failed or surplus tasks and adjusts resource allocation to improve fairness. LP solves for exact task offloading ratios given the current resource allocation. System objective: minimize total delay + energy consumption.

## Key Results
- Proposed method (SOCP trajectory + DRL+LLM resource + LP offloading) achieves best task success rate and lowest delay vs. MADQN, LVM, MADDPG baselines
- Variable-altitude UAV trajectories significantly outperform fixed-altitude — altitude dimension is critical for coverage quality
- LLM improves fairness (more failed tasks rescued) but adds a slight delay increase vs. pure DRL — a Pareto trade-off
- LP-based task offloading outperforms DDPG-based offloading due to precise optimization vs. RL approximation
- Energy consumption: conservative trajectory methods (MADQN, LVM) use less flight energy but have worse coverage

## One-line takeaway
> "Managing UAVs for IoV task offloading requires jointly optimizing trajectory (SOCP), resources (DRL + LLM fairness), and offloading (LP) in a hierarchy — treating them separately misses critical couplings and degrades performance."
