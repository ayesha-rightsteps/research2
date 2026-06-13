# Background: ISAC-Enabled UAV Vehicular Network with BCD-SCA

## Field

This paper addresses **resource allocation and trajectory planning in Integrated Sensing and Communication (ISAC)-enabled UAV-assisted vehicular networks**. The central challenge is that UAVs and ground base stations (GBSs), both equipped with ISAC hardware, must simultaneously serve vehicles with downlink data communication and radar sensing — using the same spectrum — while the UAV navigates toward vehicle-coverage-optimal positions under energy and safety constraints. The paper formulates this as a joint optimization problem and solves it with a classical optimization algorithm (BCD-SCA) rather than machine learning.

---

## Evolution of the Field

### Stage 1 — Separate Radar and Communication Systems
Traditionally, radar sensing and wireless communication occupied entirely separate spectrum bands, hardware platforms, and design paradigms. Radar systems were engineered for detection performance (range, Doppler, angular resolution); communication systems were designed for throughput and reliability. This separation was wasteful in spectrum (ever-growing spectrum scarcity) and hardware (duplication of antennas, RF chains, signal processors).

### Stage 2 — Coexistence and Spectrum Sharing
A transitional phase explored **coexistence** of radar and communication: making each system tolerate the other's emissions through interference mitigation (beamforming, nulling, scheduling). This retained separate hardware but managed mutual interference. Coexistence still does not achieve the efficiency of true integration because each system independently competes for the same spectral resource.

### Stage 3 — ISAC Concept Emerges
The ISAC paradigm — also called dual-function radar-communications (DFRC) — fuses radar and communication into a **single hardware platform and waveform**. The same OFDM signal can carry data to vehicles (communication) and be reflected back to the transmitter for target detection (radar). ISAC achieves:
- **Higher spectrum efficiency:** One signal serves two functions.
- **Reduced hardware cost:** One set of antennas, RF chains, and signal processors.
- **Improved sensing coverage:** Communication infrastructure becomes sensing infrastructure.

ISAC is identified as a key enabling technology for 6G networks. The sensing-communication tradeoff (allocating more subchannels to sensing reduces communication throughput and vice versa) is ISAC's central tension.

### Stage 4 — ISAC in Vehicular Networks (V2X-ISAC)
Applying ISAC to vehicular settings addresses a practical safety need: vehicles often cannot sense beyond their own field of view (behind obstacles, around corners). GBSs equipped with ISAC can provide **beyond-line-of-sight (BLOS) perception** — sensing vehicles at locations invisible to the vehicle's own sensors and sharing that information over the communication link. This enables cooperative perception for safer autonomous driving.

Prior V2X-ISAC work (Zhu et al. 2022; Shi et al. 2021; Yang et al. 2021; Wang et al. 2023) focused on static GBS deployments with fixed coverage. Joint subchannel and power allocation under MI-based sensing constraints was explored but without considering UAV mobility as an optimization variable.

### Stage 5 — UAV-Assisted Vehicular Networks
UAVs entered vehicular networking as **flexible mobile relays and edge nodes**:
- High flexibility and rapid deployment address temporary congestion scenarios.
- Aerial position provides better LOS probability and lower path loss to vehicles compared to ground infrastructure.
- UAVs extend coverage where GBS density is insufficient.

UAV trajectory optimization emerged as a key subproblem: where should the UAV fly to maximize communication throughput? Early UAV trajectory work (Zhang et al. 2020; Rzig et al. 2023; Yan et al. 2024) optimized trajectories for edge computing offloading but did not consider sensing functions.

### Stage 6 — UAV-ISAC: This Paper's Position
This paper introduces **UAV-assisted ISAC** for vehicular networks: UAVs and GBSs both carry ISAC hardware, cooperatively sensing vehicles while communicating with them. This creates a new joint optimization problem:

1. **UAV trajectory planning** — where should each UAV fly to maximize both communication rate and sensing quality?
2. **Vehicle association** — which UAV or GBS should each vehicle connect to?
3. **Subchannel allocation and function selection** — which subchannels carry data, which carry radar waveforms?

These three sub-problems are tightly coupled (changing UAV position changes channel gains, which affects both communication rates and sensing MI; changing subchannel function affects both throughput and sensing quality). The paper solves them jointly via BCD-SCA.

---

## Key Technologies

### ISAC (Integrated Sensing and Communication)
UAVs and GBSs are equipped with OFDM-based dual-function transmitters. An OFDM signal with S consecutive symbols is transmitted toward vehicle v. Subchannels flagged for **communication** (sccr^{t,c} = 1) contribute to downlink data rate; subchannels flagged for **sensing** (sccr^{t,c} = 0) are reflected by the vehicle back to the transmitter for radar processing.

The sensing performance is quantified via **Mutual Information (MI)** between the target impulse response and the received reflected signal. MI is chosen because it is analytically tractable and serves as a unified surrogate for classical radar metrics (detection probability, false-alarm probability, and Cramer-Rao Bound). Under the Gaussian echo model, MI is monotonically related to the KL-divergence between target-present and target-absent hypotheses.

The sensing-communication tradeoff is governed by subchannel function selection: more communication subchannels increase throughput but reduce sensing MI, and vice versa.

### BCD (Block Coordinate Descent)
The joint optimization problem P1 is a **Mixed-Integer Nonlinear Program (MINLP)** — intractable to solve globally in polynomial time due to the multiplicative coupling between discrete variables (vehicle association, subchannel allocation, subchannel function selection) and the continuous nonlinear UAV trajectory. BCD decompose P1 into three subproblems by **fixing two of the three variable blocks and optimizing the third**:
- Subproblem P1.1: Vehicle association (fix trajectory and subchannel decisions)
- Subproblem P1.2: UAV trajectory planning (fix association and subchannel decisions)
- Subproblem P1.3: Subchannel allocation and function selection (fix trajectory and association)

These subproblems are solved alternately and iteratively. BCD guarantees that the objective function is **monotonically non-decreasing** across iterations (each subproblem solution is no worse than the previous), converging to a stationary point of the original MINLP.

### SCA (Successive Convex Approximation)
Subproblems P1.2 and P1.3 remain non-convex after BCD decomposition. SCA transforms non-convex constraints and objective terms into **locally tight convex approximations** using first-order Taylor expansion at the current iterate. The Taylor lower bound is a global lower bound for the original convex function, ensuring each SCA iteration improves the objective while remaining feasible. SCA-convexified problems are then solved exactly by the **interior-point method**.

SCA convergence is guaranteed under standard conditions: the approximation error is quadratically bounded by ||x - x_k||^2 (Lipschitz constant of gradient), which vanishes near convergence.

### Mutual Information (MI) as Sensing Metric
The MI between target impulse response q and received reflected signal z, given transmitted waveform s, measures how much information the reflected signal carries about the target's scattering properties:
`MI = (1/2) * B_0 * S * T_s * log2(1 + P_t * S * T_s^2 * |Q(f)|^2 / sigma_rad)`
where B_0 is subchannel bandwidth, S is the number of OFDM symbols, T_s is symbol duration, P_t is transmit power, and sigma_rad is radar receiver noise. A minimum MI threshold (HMI, ranging 200–1000 bits in experiments) enforces a sensing quality floor in the optimization constraints.

### UAV Trajectory Optimization
UAV horizontal positions {CU^t_u = (x^t_u, y^t_u)} are optimized over T time slots subject to:
- **Speed constraint:** Position change per slot bounded by (max_speed * slot_duration)^2.
- **Start/end point constraints:** UAVs must begin and end at preset positions.
- **Energy constraint:** Total flight energy (distance × energy_per_meter) bounded by onboard battery.
- **Safety distance constraint:** Inter-UAV distance must exceed d_min = 8 m.

UAV altitude is fixed at 8 m (chosen for LOS probability, sensing resolution, and regulatory compliance with FAA Part 107 VLOS rules). Trajectory optimization is performed on the horizontal plane.

The objective is for UAVs to fly close to vehicle clusters, reducing path loss and increasing both communication rate and sensing MI. After optimization, UAV trajectories curve toward vehicle lanes rather than flying straight from start to end.

---

## Why This Paper Differs from Prior Work

| Aspect | Prior UAV trajectory work | Prior ISAC resource allocation | This paper (TDRA) |
|---|---|---|---|
| ISAC at UAVs | No | Some | Yes (UAVs + GBSs) |
| Cooperative UAV-GBS sensing | No | No | Yes |
| UAV trajectory optimization | Yes | No | Yes |
| Vehicle association | Sometimes | Sometimes | Yes |
| Subchannel function selection (comm vs. sensing) | No | Yes | Yes |
| Sensing constraint (MI-based) | No | Yes | Yes |
| Joint coupling of all above | No | No | Yes — MINLP |
| Solution method | BCD/SCA variations | BCD/SCA variations | BCD-SCA tailored to new coupling structure |

The key novelty is the **simultaneous coupling** of UAV mobility, cooperative sensing by both UAVs and GBSs, and sensing-constrained subchannel function selection — a problem structure not addressed by any single prior work.

---

## Position in the Bigger Picture

### Immediate Problem Context
The scenario targets **temporary traffic congestion** on urban roads (150m x 30m highway section, 3 lanes, 6 vehicles, 2 UAVs, 1 GBS, 30 time slots). This represents a realistic deployment: a major event or accident causes localized congestion; UAVs are rapidly deployed to augment GBS coverage and provide aerial sensing.

### Broader ITS and 6G Context
- **6G networks** designate ISAC as a native capability of base stations and infrastructure nodes. This paper prototypes what ISAC-enabled vehicular infrastructure looks like at a system level.
- **Cooperative perception for autonomous vehicles:** Beyond communication, the sensing function provides BLOS awareness that complements onboard vehicle sensors (LiDAR, cameras), contributing to higher-level autonomous driving safety.
- **Spectrum efficiency under scarcity:** As vehicular communication density grows, reusing spectrum for both data and sensing is essential. The sensing-communication tradeoff is a fundamental design constraint that must be explicitly modeled (as this paper does via MI constraints).
- **UAV regulations and practicality:** The paper explicitly addresses FAA Part 107 (VLOS at 8 m altitude), energy constraints, and multi-UAV collision avoidance — demonstrating awareness of real deployment constraints.

### Comparison With Machine Learning Approaches
Unlike paper-8 (GAT-A2C, a DRL approach), this paper uses **classical convex optimization** (BCD-SCA). This has different tradeoffs:
- **BCD-SCA:** Provides convergence guarantees, interpretable optimization structure, no training data required. Slower per-update (iterative optimization); requires re-solving with each new scenario configuration.
- **DRL approaches:** Fast inference once trained; adapt implicitly to dynamics. Less interpretable; require large training datasets; generalization to new scenarios not guaranteed.

This paper's approach is better suited to moderate-scale scenarios (6 vehicles, 2 UAVs, 1 GBS, 30 time slots) where the MINLP can be solved tractably. Scaling to hundreds of vehicles would require either decomposition or RL-based alternatives.

### Performance Results
- vs. URA (random vehicle association, optimized trajectory + subchannel): +32.42% average achievable rate (vs. UAV speed)
- vs. SAUA (joint association + subchannel, no trajectory optimization): +178.57% average achievable rate (vs. UAV speed) — highlighting that trajectory optimization is the single most impactful component
- vs. SEA (equal subchannel allocation, optimized trajectory + association): +87.01–109.92% across different parameters
- Algorithm converges within ~4 iterations, demonstrating computational efficiency
