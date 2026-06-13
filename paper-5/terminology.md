# Terminology: Digital Twin-Assisted Adaptive Multi-Agent DRL for Intelligent Spectrum and Resource Management in Open-RAN UAV-Enabled 6G Networks

> Quick reference for all technical terms used in this paper. The 5 most central terms are marked ⭐. Terms are grouped by theme.

---

## Core System Architecture

### Open-RAN (Open Radio Access Network) ⭐
**What it is:** A telecommunications standard and architecture that disaggregates the traditional Radio Access Network into open-interface, vendor-neutral components — RUs (Radio Units), DUs (Distributed Units), CUs (Central Units), and RICs (RAN Intelligent Controllers). "Open" means hardware from one vendor and software from another can work together.
**In this paper:** Open-RAN is the overarching network architecture within which the entire system operates. UAVs serve as Aerial Radio Units (ARUs) and fixed towers as Ground Radio Units (GRUs). The DT and Non-RT RIC sit in the O-Cloud for centralized training; the Near-RT RIC operates at the edge for real-time inference. All resource management decisions flow through this hierarchy.
**Easy way to remember:** Think of Open-RAN as converting a proprietary, closed mobile network into an open-source system where smart software (including AI) can plug in and manage any hardware — like Android vs. a proprietary phone OS.

### Digital Twin (DT) ⭐
**What it is:** A continuously synchronized virtual replica of a physical system. The digital twin receives real-time telemetry from sensors in the actual system, updates its model, performs simulations and training, and sends control decisions back to the physical world.
**In this paper:** The DT is hosted in the O-Cloud alongside the Non-RT RIC. It continuously receives state information — UAV positions (Q_u), GRU positions (Q_g), GU positions (Q_n), and channel state (Γ_ru,n) — from the actual network layer. Using this data, it trains MADRL policies centrally, then periodically pushes updated policies to the Near-RT RIC for edge deployment. The DT update equation is D^t_DT = f(D^t_actu).
**Easy way to remember:** A "virtual twin" of the real network living in the cloud — like a flight simulator that is constantly fed real sensor data from a live aircraft so it exactly mirrors what is happening at all times.

### Non-RT RIC (Non-Real-Time RAN Intelligent Controller)
**What it is:** The higher-level AI controller in Open-RAN, operating on timescales greater than one second. Hosted in the O-Cloud. Responsible for long-term optimization, global model training, and policy generation.
**In this paper:** The Non-RT RIC hosts the DT and performs centralized MADRL training. Trained policies π*_u(a_u|s_u) are periodically transmitted to the Near-RT RIC. Policy synchronization happens at slow, periodic timescales (typically every few seconds).
**Easy way to remember:** The head office strategist — works slowly but sees the whole picture and sets the high-level playbook.

### Near-RT RIC (Near-Real-Time RAN Intelligent Controller)
**What it is:** The lower-level AI controller in Open-RAN, operating on timescales from 10 milliseconds to 1 second. Deployed at the network edge. Responsible for executing trained policies in real time and reacting to rapid environment changes.
**In this paper:** The Near-RT RIC receives trained DRL actor policies from the DT (via the Non-RT RIC) and deploys them for real-time inference. Each UAV and GRU uses the latest policy from the Near-RT RIC to make per-slot resource allocation decisions (bandwidth, power, associations) in milliseconds.
**Easy way to remember:** The team captain on the field — executes the coach's playbook (from Non-RT RIC) in real time with fast on-the-fly adjustments.

### O-Cloud (Open Cloud)
**What it is:** The cloud computing infrastructure specified in Open-RAN standards that hosts the Non-RT RIC and other centralized network functions. Provides virtualized compute resources for training, orchestration, and long-horizon optimization.
**In this paper:** The DT and Non-RT RIC run in the O-Cloud. The O-Cloud connects to the Near-RT RIC at the network edge and to the CU/DU for backhaul coordination. The HAP connects the UAVs' aerial backhaul to the O-Cloud.
**Easy way to remember:** The headquarters building that houses the brains of the operation — big, powerful, but not fast enough for split-second decisions.

### HAP (High-Altitude Platform)
**What it is:** An aircraft or balloon operating at altitude that provides connectivity over wide areas and serves as an aerial backhaul aggregation point.
**In this paper:** UAVs maintain aerial backhaul by connecting upward to the HAP. The HAP then connects to the O-Cloud. This creates the backhaul path: GU → UAV → HAP → O-Cloud → Near-RT RIC decisions → back to UAV. The HAP thus plays a relay role in the control plane.
**Easy way to remember:** An aerial relay tower that bridges the drone swarm to the internet backbone.

### CU/DU (Central Unit / Distributed Unit)
**What it is:** In Open-RAN, the CU handles higher-layer radio protocols and coordinates multiple DUs. The DU handles real-time physical-layer processing. Together they implement the base station function in a disaggregated way.
**In this paper:** GRUs connect their backhaul to the Open-RAN CU/DU via terrestrial fiber or microwave links. The CU/DU coordinates with both the Near-RT RIC and the O-Cloud.
**Easy way to remember:** The brains and muscles of a base station, now split into two boxes — CU thinks, DU acts.

---

## Network Nodes and Roles

### UAV (Unmanned Aerial Vehicle)
**What it is:** An autonomous or remotely piloted aircraft. In this paper, rotary-wing UAVs (drones) operate at fixed altitude within defined clusters, following closed-loop trajectories starting and ending at the cluster center.
**In this paper:** Each UAV functions as an Aerial Radio Unit (ARU), extending coverage to GUs in its cluster. The system has U UAVs organized in 3 clusters, each paired with a GRU. UAV positions Q_u = (x_u, y_u, H_u) are optimized by PSO. UAVs have limited battery capacity modeled using propulsion power (induced, blade, parasite components) and must maintain minimum energy reserves to return to their docking station.
**Easy way to remember:** A flying Wi-Fi access point and base station combined, with a limited battery that forces it to be energy-aware.

### ARU (Aerial Radio Unit)
**What it is:** A UAV functioning as a radio unit within the Open-RAN architecture — providing wireless access to GUs from the air, just as a ground-mounted RU does from a tower.
**In this paper:** UAVs are consistently referred to as ARUs to emphasize their role within the Open-RAN framework, placing them formally within the standardized component taxonomy of Open-RAN.
**Easy way to remember:** A drone that has been "promoted" to a full member of the network infrastructure team.

### GRU (Ground Radio Unit)
**What it is:** A fixed, terrestrial radio unit — a conventional base station or antenna tower — that provides wireless access to GUs in its geographic cluster.
**In this paper:** The system has G GRUs, each forming a cluster paired with a UAV. GRUs can become overloaded or fail (e.g., during post-disaster scenarios or peak events), motivating UAV deployment. GRU-GU terrestrial links follow Rayleigh fading. GRUs connect backhaul to CU/DU via fiber or microwave.
**Easy way to remember:** The traditional cell tower that the drone is sent to help when it gets overwhelmed.

### GU (Ground User)
**What it is:** An end-user device on the ground — a smartphone, sensor, vehicle, or any device that needs wireless connectivity. GUs are the ultimate beneficiaries of the system.
**In this paper:** N = 100 GUs distributed across the 1000m × 1000m coverage area following a power-law spatial distribution to simulate realistic non-uniform density. Each GU is served by exactly one RU (either a UAV or a GRU) at any time slot. The objective is to maximize total data delivered to all GUs.
**Easy way to remember:** The customers — every decision in the system is ultimately about serving them better.

### RU Cluster
**What it is:** A geographic division of the service area, each centered on a GRU and supported by a paired UAV. All GUs within a cluster are nominally served by that cluster's GRU and UAV.
**In this paper:** The 1000m × 1000m area is divided into 3 clusters, each containing one GRU and one UAV. Each UAV operates within its cluster and follows a closed-loop path (constraint C6: Q0_u = QL_u, start equals end position).
**Easy way to remember:** A neighborhood service zone — one fixed station plus one mobile helper covers all residents in that zone.

---

## Machine Learning Methods

### MADRL (Multi-Agent Deep Reinforcement Learning) ⭐
**What it is:** A reinforcement learning framework where multiple agents simultaneously learn policies in a shared environment. Each agent observes its local state, takes actions, and receives rewards. "Deep" indicates neural networks represent the policies.
**In this paper:** Each RU agent (UAV or GRU) learns to jointly optimize its RU-GU association, bandwidth allocation, and transmit power. The DT provides the centralized critic (global information for training) while each agent's actor executes locally. This is the CTDE paradigm. The algorithm is an enhanced DDPG with twin critics, target policy smoothing, and parameter noise exploration.
**Easy way to remember:** A soccer team where each player learns their own role through practice, but during training they share a coach who watches everything — then during the game each player acts on their own.

### CTDE (Centralized Training, Decentralized Execution)
**What it is:** An architecture for multi-agent learning where a centralized module with access to global state is used during training, but during deployment each agent acts based only on its own local observations.
**In this paper:** The DT in the Non-RT RIC performs centralized training — the critic network aggregates global state-action pairs from all agents. The Near-RT RIC then deploys decentralized actor policies — each RU agent makes real-time decisions from its local state only. This matches the Open-RAN hierarchy naturally.
**Easy way to remember:** The team trains together with full information sharing, but executes independently during the game.

### PSO (Particle Swarm Optimization) ⭐
**What it is:** A population-based metaheuristic optimization algorithm inspired by the collective movement of bird flocks. A swarm of "particles" (candidate solutions) moves through the search space, each updating its velocity based on its own best-found position and the swarm's global best.
**In this paper:** PSO optimizes UAV positions (x_u, y_u) at the beginning of each episode by maximizing data rates subject to UAV mobility and coverage constraints. PSO runs for 10 episodes to find good UAV trajectories, then MADRL takes over for resource allocation. PSO is chosen because the UAV positioning problem is non-convex and the low-complexity metaheuristic handles it efficiently without gradient computation.
**Easy way to remember:** A flock of birds searching for the best perch — each bird remembers where it found the best spot, and they share that information, collectively converging on the best location.

### DDPG (Deep Deterministic Policy Gradient)
**What it is:** An off-policy DRL algorithm for continuous action spaces. Uses an Actor network (policy, outputs deterministic actions) and a Critic network (value function, evaluates actions). Employs experience replay and target networks for stable training.
**In this paper:** The MADRL framework is built on an enhanced DDPG architecture. Enhancements include: (1) twin critics Qθ1 and Qθ2 using the minimum prediction to reduce overestimation bias; (2) target policy smoothing — adding clipped Gaussian noise ϵ ~ N(0, σ²) to target actor outputs; (3) parameter noise adaptation — perturbing actor network weights using KL-divergence for better state-dependent exploration.
**Easy way to remember:** A sophisticated trial-and-error learner for decisions that are not just "left or right" but "dial between 0 and 1" — the enhancements make it more cautious (twin critics) and more curious (noise for exploration).

### Twin Critics
**What it is:** An architectural technique in DRL where two independent Q-networks (critics) are trained simultaneously. The minimum of their predictions is used as the learning target, preventing overestimation of Q-values that plagues single-critic algorithms.
**In this paper:** The two critics are Qθ1(s, a) and Qθ2(s, a). The target value is y = r + γ min(Q_θ̄1, Q_θ̄2). This stabilizes training in the complex multi-agent Open-RAN environment.
**Easy way to remember:** Getting a second opinion before acting on advice — if two independent evaluators disagree, take the more conservative (lower) estimate to avoid overconfidence.

### MDP (Markov Decision Process)
**What it is:** A mathematical framework for sequential decision-making: at each time step, an agent observes a state, takes an action, receives a reward, and transitions to a new state. The Markov property means the future depends only on the current state, not the history.
**In this paper:** The resource management problem is modeled as an MDP. State s_ru = {Q_ru, Q_n, Ω_u, Γ_n,ru, τ_tot,n} includes positions, energy, SINR, and latency. Actions a_ru = {a_n,ru, B_n,ru, P_n,ru} cover association, bandwidth, and power. Reward r_ru = Σ R_n,ru is the total achieved data rate.
**Easy way to remember:** A formalized "given where I am and what I know, what should I do next to maximize long-term payoff?" decision framework.

---

## Communications and Signal Processing

### OFDM (Orthogonal Frequency Division Multiplexing)
**What it is:** A modulation technique that divides the available bandwidth into multiple narrow, orthogonal subcarriers (subchannels). Each subchannel carries a portion of the data, and orthogonality prevents interference between subchannels.
**In this paper:** RUs communicate with GUs over M OFDM subchannels. All RUs (both UAVs and GRUs) share the same set of subchannels M = {1, 2, ..., M}, creating a shared-spectrum environment requiring coordinated interference management.
**Easy way to remember:** Broadcasting on many narrow radio lanes simultaneously — each lane is separate from the others, but together they deliver a wide highway of data.

### SINR (Signal-to-Interference-plus-Noise Ratio) ⭐
**What it is:** A metric for wireless link quality. It is the ratio of the desired signal power to the sum of interference from other transmitters and background noise. Higher SINR means better link quality and higher achievable data rate.
**In this paper:** Γ_n,ru = P_n,ru * a_n,ru * g_n,ru / (Σ interference + N_0). The system must maintain SINR above a minimum threshold Γ_min for each link (constraint C3). PSO optimizes UAV positions partly to improve per-GU SINR by reducing interference.
**Easy way to remember:** How well you can hear someone (signal) over background noise plus other conversations (interference) — the louder your voice relative to the din, the better.

### Rician Fading
**What it is:** A statistical model for wireless channel variation that includes both a dominant direct (LoS) component and scattered multipath components. The K-factor describes the ratio of LoS power to scattered power.
**In this paper:** UAV-GU air-to-ground channels follow Rician fading (h_n,ru = √(K/(K+1)) × ψ_LoS + √(1/(K+1)) × ψ_NLoS), reflecting the strong LoS component typical of aerial links, contrasting with GRU-GU Rayleigh fading.
**Easy way to remember:** A radio link where you can see the person you are talking to (LoS component dominates) but there are still some echoes bouncing off walls.

### Rayleigh Fading
**What it is:** A statistical model for wireless channels dominated entirely by multipath scattering, with no significant direct path. Amplitude follows a Rayleigh distribution.
**In this paper:** GRU-GU terrestrial links follow Rayleigh fading (h_n,g ~ CN(0,1)), appropriate for urban environments where buildings obstruct direct paths between ground towers and users.
**Easy way to remember:** Talking to someone in a room full of mirrors — the signal reaches you from many reflected paths but not directly, making the channel unpredictable.

### xURLLC (Extended Ultra-Reliable Low Latency Communication)
**What it is:** A 6G service class requiring sub-millisecond latency, extremely high reliability, and support for massive simultaneous connections — beyond the URLLC requirements of 5G.
**In this paper:** xURLLC is cited in the introduction as a defining requirement of 6G. The end-to-end latency constraint (τ_tot,n ≤ τ_max = 100 ms, constraint C2) operationalizes this requirement in the optimization formulation.
**Easy way to remember:** The "sports car" service level for 6G — instant response, never drops, handles huge crowds.

### Spectrum Sharing
**What it is:** Multiple users or systems using the same frequency bands simultaneously, with interference management to ensure acceptable service quality for all.
**In this paper:** UAVs and GRUs share the available spectrum of GRUs to enhance spectral efficiency and overall throughput. Interference coordination between aerial and terrestrial links is a central challenge, addressed through joint bandwidth and power allocation by the MADRL agents.
**Easy way to remember:** Carpooling on a highway — same road (spectrum), multiple users, needs careful coordination to avoid crashes (interference).

---

## Energy and Physical Models

### Propulsion Power Model
**What it is:** A physics-based model for the electrical power consumed by a rotary-wing UAV's motors. Includes induced power (lift generation), blade profile power (air resistance on blades), and parasite power (body drag at speed).
**In this paper:** ε_prop,u = induced power (η_i) + blade power (η_b) + parasite power (½ f_0 φ r D_a v³). Total energy per slot is ε_u,tot = ε_prop,u × t (moving) or ε_hov,u × t (hovering). This model drives constraint C1 (battery ≥ Ω_min) and the energy component of the reward function.
**Easy way to remember:** Like calculating a car's fuel consumption based on how fast it is going and how heavy it is — flying fast costs more, hovering in place has a steady base cost.

### Battery Constraint (C1)
**What it is:** The physical limit on how long a UAV can operate before running out of power. Battery state decreases over time as the UAV flies and transmits.
**In this paper:** Battery state updates as Ω_u^t = Ω_u^(t-1) - ε_u,tot^t. The minimum energy Ω_min is reserved for the UAV to safely return to its docking station. Constraint C1 enforces Ω_u^t ≥ Ω_min at all times.
**Easy way to remember:** The drone's fuel gauge — always keep enough in the tank to get home.

### End-to-End Latency
**What it is:** The total delay for data traveling from a GU through the entire network. In this paper it spans three hops: access, backhaul, and cloud coordination.
**In this paper:** τ_tot,n = τ_ru-n (RU to GU access) + τ_ru-h,m (RU to HAP/MBS backhaul) + τ_h,m-oc (HAP/MBS to O-Cloud, including DT synchronization delay τ_DT^dev). Constraint C2 requires τ_tot,n ≤ τ_max = 100 ms for all GUs at all times.
**Easy way to remember:** The total delivery time from ordering to receiving — the drone leg, the highway leg, and the warehouse processing time all add up.

### DT Synchronization Delay (τ_DT^dev)
**What it is:** The latency introduced by the need to synchronize the digital twin model with the current state of the physical network before a DT-informed decision can be applied.
**In this paper:** τ_DT^dev is explicitly included as a component of the cloud coordination latency τ_h,m-oc. The paper notes that predictive synchronization and proactive updates compensate for this delay, ensuring low-latency service delivery despite the DT being a finite-precision model.
**Easy way to remember:** The lag between the real world and its digital copy — like the delay between a live sports game and the TV broadcast.

---

## Optimization Formulation

### MINLP (Mixed-Integer Nonlinear Programming)
**What it is:** An optimization problem class that includes both continuous variables (like power levels or positions) and discrete/binary variables (like association indicators), with nonlinear objective or constraints. MINLP is in general NP-hard.
**In this paper:** The joint optimization problem (4) is classified as MINLP because it includes binary association variables a_n,ru ∈ {0,1} alongside continuous UAV positions, bandwidth, and power, with nonlinear channel dynamics. This intractability motivates decomposing the problem into PSO (trajectory) and MADRL (resources).
**Easy way to remember:** An optimization problem that asks you to simultaneously choose whole-number counts (yes/no associations) and real-valued amounts (how much power), with complex relationships between them — the hardest class of optimization.

### Association Variable (a_n,ru)
**What it is:** A binary decision variable indicating whether GU n is served by RU ru at time slot t. a_n,ru = 1 means the GU is assigned to this RU; a_n,ru = 0 means it is not.
**In this paper:** Association is one of the three action dimensions for each MADRL agent (alongside bandwidth and power). Constraint C10 enforces binary association; constraint C11 ensures each GU is served by at most one RU per time slot.
**Easy way to remember:** The rooming assignment in a hotel — each guest (GU) is assigned to exactly one room (RU), and getting the assignment right affects everyone's comfort.

### Data Rate Maximization
**What it is:** The optimization objective of maximizing the total volume of data successfully delivered to all GUs across all time slots.
**In this paper:** The objective function (4) is max Σ_t Σ_ru Σ_n S_n,ru^t, where S_n,ru^t = δ_t × R_n,ru^t is the service amount (data volume) delivered to GU n by RU ru during time slot t. This is maximized subject to energy (C1), latency (C2), SINR (C3), bandwidth (C4), power (C5), trajectory (C6-C9), and association (C10-C11) constraints.
**Easy way to remember:** Maximizing the total packets delivered to all customers — not just one fast link but the aggregate across the whole network.

---

## Benchmark Comparisons

### MADDPG (Multi-Agent Deep Deterministic Policy Gradient)
**What it is:** The multi-agent extension of DDPG using centralized training with decentralized execution. A standard baseline algorithm for cooperative MARL in wireless networks.
**In this paper:** One of four baseline algorithms. It is used without DT enhancement or the twin-critic and noise-adaptation improvements. The proposed method converges faster and achieves higher reward and lower latency than MADDPG.
**Easy way to remember:** The previous state-of-the-art that this paper improves upon.

### MAPPO (Multi-Agent Proximal Policy Optimization)
**What it is:** A multi-agent variant of PPO, an on-policy DRL algorithm that constrains policy updates to prevent destabilizing large changes.
**In this paper:** A second baseline algorithm. On-policy updates without experience replay can be sample-inefficient in complex environments. The proposed method outperforms MAPPO in convergence speed, data rate, and latency.
**Easy way to remember:** A more cautious learner — takes smaller steps to avoid falling, but gets to the destination slower.

### MA Actor-Critic
**What it is:** A basic multi-agent Actor-Critic algorithm without the architectural enhancements of twin critics, target policy smoothing, or parameter noise.
**In this paper:** The third baseline, representing a simpler CTDE approach. It shows the weakest performance among all algorithms, illustrating the value of the proposed architectural enhancements.
**Easy way to remember:** The plain version of the multi-agent team — good enough to work, but not optimized.

### Greedy Solution
**What it is:** A heuristic approach that makes the locally best decision at each step without considering future consequences. Fast to compute but typically suboptimal.
**In this paper:** The fourth baseline — assigns each GU to the nearest RU and allocates maximum power and bandwidth without intelligent coordination. Represents the naive non-learning approach. The proposed framework substantially outperforms it.
**Easy way to remember:** The driver who always takes the immediately shortest road without thinking about traffic ahead — fast to decide, but usually not the best overall route.

---

## Performance Metrics

### Spectral Efficiency
**What it is:** A measure of how much data (bits per second) can be transmitted per unit of bandwidth (Hz). Higher spectral efficiency means the system extracts more useful data from the available radio spectrum.
**In this paper:** One of the three headline performance metrics (alongside data rate and energy utilization) on which the framework is evaluated. The shared-spectrum approach between UAVs and GRUs is designed to improve spectral efficiency through coordinated interference management.
**Easy way to remember:** Miles-per-gallon for spectrum — how much data you squeeze out of each slice of radio frequency.

### Energy Utilization
**What it is:** A metric capturing how efficiently the system uses energy — high data rates achieved with low energy expenditure indicate better energy utilization.
**In this paper:** Energy utilization is both a reward component in the MADRL formulation (the reward penalizes high energy consumption) and a headline evaluation metric. The UAV battery constraint (C1) makes energy a hard physical limit.
**Easy way to remember:** How much you get done per battery charge — a system that delivers twice the data for the same energy is twice as energy-efficient.

### 6G / Beyond-5G (B5G)
**What it is:** The next generation of wireless networks (standardization target: 2030+), requiring ultra-high data rates (up to 1 Tbps), sub-millisecond latency, AI-native intelligence at all layers, and seamless integration of terrestrial and non-terrestrial networks.
**In this paper:** The target deployment context and design motivation. The paper proposes architecture components (DT + Open-RAN + UAV + MADRL) that directly address 6G requirements for self-evolving, autonomous, intelligent network management.
**Easy way to remember:** 5G was fast; 6G is fast AND smart — the network thinks and adapts for itself.
