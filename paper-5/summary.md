# Summary: Digital Twin-Assisted Adaptive Multi-Agent DRL for Intelligent Spectrum and Resource Management in Open-RAN UAV-Enabled 6G Networks

**Authors:** Marwan Dhuheir, Thang X. Vu, and Symeon Chatzinotas | **Year:** 2026 | **Published in:** arXiv preprint (arXiv:2606.01324v1), University of Luxembourg

## What is this paper about?
This paper addresses the challenge of managing spectrum and resources in 6G wireless networks where UAVs (drones) act as flying base stations to serve ground users alongside fixed Ground Radio Units (GRUs). The problem is hard because UAV positions, battery levels, user locations, and interference all change dynamically, making classical optimization too slow or rigid. The authors propose a hybrid framework that combines a Digital Twin (a real-time virtual replica of the network hosted in the cloud) with Multi-Agent Deep Reinforcement Learning (MADRL) and Particle Swarm Optimization (PSO) to handle trajectory and resource decisions jointly. Simulations over a 1000m x 1000m area with 100 users show the system outperforms four competing algorithms on data rate, latency, and convergence speed.

## The Core Idea
A Digital Twin continuously mirrors the live Open-RAN network in the cloud, trains centralized RL policies globally (via the Non-RT RIC), and pushes updated models to edge controllers (Near-RT RIC) for millisecond-level real-time execution. UAV path planning uses PSO at the start of each episode, while each UAV/GRU agent runs an enhanced DDPG policy for dynamic spectrum, power, and user-association decisions — the two modules iterate together, combining global coordination with local adaptability.

## What they did (Method)
The system places 3 UAVs and 3 GRUs over a 1000m x 1000m area serving 100 ground users with a non-uniform (power-law) spatial distribution. UAV-to-user links use Rician fading (dominant line-of-sight); GRU-to-user links use Rayleigh fading (rich multipath). The full optimization — maximizing total delivered data subject to energy, latency (≤100 ms), SINR, power (≤35 dBm), bandwidth (≤100 MHz), trajectory, and collision constraints — is a Mixed-Integer Non-Linear Program (MINLP) decomposed into two parts: PSO for UAV trajectory per episode, and adaptive MADRL (twin-critic DDPG with parameter noise and target policy smoothing) for spectrum/power/association per time step. The Digital Twin synchronizes trained models between Non-RT and Near-RT RICs. Benchmarks: MADDPG, MAPPO, MA Actor-Critic, and a Greedy baseline. Implemented in TensorFlow 1.13.1, Python 3.7.16 on an Intel Xeon i7 with 16 GB RAM.

## Key Results
- **Convergence speed:** The proposed framework stabilizes after approximately 200 training episodes, reaching higher average rewards faster than all four benchmarks (MADDPG, MAPPO, MA Actor-Critic, Greedy).
- **Latency:** Converges to approximately 60 ms average end-to-end latency — the lowest and most stable among all compared methods.
- **Data rate scalability:** Achieves consistently higher average data rates across all tested numbers of clusters (1 through 5+), demonstrating that gains hold as network size grows.

## One-line takeaway
> "By pairing a Digital Twin for centralized training with PSO for UAV paths and MADRL for real-time resource decisions, this framework achieves faster convergence, higher throughput, and lower latency than existing multi-agent RL baselines in UAV-assisted 6G Open-RAN networks."
