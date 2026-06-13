# Summary: A Lyapunov-Guided Diffusion-Based Reinforcement Learning Approach for UAV-Assisted Vehicular Networks with Delayed CSI Feedback

**Authors:** Zhang Liu, Lianfen Huang, Zhibin Gao, Xianbin Wang, Dusit Niyato, Xuemin (Sherman) Shen | **Year:** 2025 | **Published in:** IEEE (arXiv:2507.20524)

## What is this paper about?
UAVs acting as aerial base stations can help vehicles in areas where ground infrastructure is limited or overloaded. But managing UAV resources is hard: you need to allocate channels to vehicles, control transmission power, and decide the UAV's altitude — all while dealing with delayed channel state information (CSI) feedback and the UAV's limited battery. This paper proposes a two-part solution: Lyapunov optimization to handle the long-term energy constraint, and a diffusion-model-based RL algorithm (D3PG) to handle the real-time resource allocation decisions under delayed CSI.

## The Core Idea
Two clever ideas working together. First: Lyapunov optimization mathematically converts a long-term "don't run out of battery" constraint into a per-slot penalty term — so instead of worrying about the whole future, the agent just pays an extra cost each time slot when it uses too much energy. Second: instead of a standard RL policy (which outputs a single action), use a diffusion model as the policy — it generates a distribution of possible actions through a denoising process, which handles delayed/noisy CSI much better than deterministic policies.

## What they did (Method)
The system: one UAV serving multiple vehicles on a real 2km highway in Xiamen, China (map from OpenStreetMap, vehicle mobility from SUMO simulator). The problem: jointly optimize channel allocation, transmit power, and UAV altitude each time slot to maximize vehicle-to-UAV sum rate, subject to long-term energy constraint. Lyapunov converts the energy constraint. Then D3PG (Diffusion-based Deep Deterministic Policy Gradient) runs: a denoiser neural network iteratively refines a noisy action into the final decision through I denoising steps per time slot. Compared against DDPG, TD3, and SAC benchmarks.

## Key Results
- D3PG outperforms DDPG, TD3, and SAC on vehicle-to-UAV sum rate across all tested scenarios
- Handles delayed CSI feedback better than deterministic policy baselines — diffusion model's stochastic exploration helps where exact CSI is unavailable
- UAV energy constraint is satisfied while maximizing communication rate
- Uses real-world Xiamen highway with SUMO-generated vehicle mobility — not synthetic data
- Complexity: O(STILn²) — linear in episodes, time slots, denoising steps; quadratic in network width

## One-line takeaway
> "When a UAV doesn't know the exact channel state (because feedback is delayed), using a diffusion model as the RL policy — guided by Lyapunov energy constraints — gives better resource allocation than classical deterministic RL approaches."
