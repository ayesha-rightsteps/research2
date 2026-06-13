# Summary: A Multi-Agent Resource Allocation Method for Unmanned Aerial Vehicle Networks based on Adaptive Spatial Reward Mechanisms and Context-Awareness

**Authors:** Mingsheng Cao, Jun Wang, Yuxuan Ji, Peng Xu, Hongyan Wang, Yuhua Yao, Ji Geng, Pengfei Liu | **Year:** 2026 | **Published in:** BDICN 2026 (5th International Conference on Big Data, Information and Computer Network), Kuala Lumpur, Malaysia

## What is this paper about?
This paper tackles the problem of dynamic resource allocation in multi-UAV communication networks, where each UAV must decide which user to serve, which sub-channel to use, and at what power level — all without a central controller. The authors propose a Multi-Agent Reinforcement Learning method called ASAC (Adaptive Space Reward Mechanism and Agent Contextual Awareness) that lets UAVs cooperate intelligently by sharing state information with neighbors and adjusting how much they weight rewards from nearby vs. distant agents. The result is a system that converges faster and allocates communication resources more efficiently, especially when the network is dense with many users.

## The Core Idea
Instead of treating every other UAV's reward equally (which causes instability in large networks), ASAC introduces a spatial discount factor that automatically reduces the influence of distant agents on a UAV's learning — nearby agents matter more. On top of this, each UAV shares its encoded hidden state with its neighbors through an LSTM-based communication mechanism, so agents can make smarter decisions using richer local context.

## What they did (Method)
The authors modeled a multi-UAV downlink network (M UAVs, L ground users, K sub-channels) as a Markov Decision Process and built a Multi-Agent Actor-Critic (MA2C) framework on top of it. They added two innovations: (1) an Adaptive Spatial Reward Mechanism that weights neighbor rewards by a spatial discount factor kappa based on inter-agent distance, and (2) an Agent Context-Aware Communication Mechanism where each UAV encodes its local state via a 64-neuron LSTM and exchanges hidden states with neighbors, combined with Exponential Moving Average action smoothing to reduce training instability. Experiments were run in a simulated 500m-radius disk area with 6 UAVs at 100m altitude, 200 ground users, and compared against IA2C, MAB, and MADDPG baselines under both LoS and probabilistic channel models.

## Key Results
- In high-user-count networks (M=7, K=5, L=200), ASAC achieves a total reward of 4.89 vs. the best baseline MAB at 4.23 — a 16% improvement in resource allocation efficiency.
- ASAC outperforms all baselines (IA2C, MAB, MADDPG) in both convergence speed and average reward across all tested network sizes (small: M=4, K=3, L=50; large: M=7, K=5, L=200).
- The optimal exploration factor is epsilon = 0.5; both lower and higher values degrade performance, and this holds consistently across both LoS and probabilistic channel models.
- ASAC consistently achieves higher network utility than all baselines across all tested user counts from 50 to 200 users (M=4, K=3 setting).

## One-line takeaway
> "By making UAVs share encoded state with neighbors and discount faraway agents' rewards spatially, ASAC achieves 16% better resource allocation efficiency in dense UAV networks compared to state-of-the-art MARL baselines."
