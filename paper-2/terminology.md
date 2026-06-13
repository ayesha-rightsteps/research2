# Terminology: Lyapunov-Guided Diffusion-Based DRL for UAV-Assisted Vehicular Networks

---

## ⭐ D3PG (Diffusion-Based Deep Deterministic Policy Gradient)
- **What it is**: The novel algorithm proposed in this paper. It replaces the standard MLP actor network in DDPG with a diffusion model-based denoiser that generates actions through an iterative denoising process.
- **In this paper**: D3PG jointly optimizes channel allocation, power control, and UAV flight altitude adjustment in each time slot. It is trained via standard actor-critic updates (TD error for the critic, policy gradient for the actor) but uses I denoising steps to produce each action vector.
- **Easy way to remember**: Think of it as DDPG but instead of the actor drawing the action in one shot, it sculpts the action gradually out of noise — like a painter who starts with a blank canvas and refines it stroke by stroke.

---

## ⭐ Lyapunov Optimization
- **What it is**: A mathematical technique from control theory that converts a long-term stochastic constrained optimization problem into a sequence of simpler, per-slot deterministic problems. It does this by introducing a virtual queue and a drift-plus-penalty function.
- **In this paper**: Used to handle the UAV's long-term energy constraint (C8 in P1). The original multi-stage mixed-integer nonlinear program (MINLP) is transformed into per-slot problem P2, which the DRL agent can solve one time slot at a time without knowing future channel states or vehicle positions.
- **Easy way to remember**: Lyapunov acts like a financial budget tracker — instead of worrying about your annual spending all at once, you get a daily penalty if you overspend today, which keeps the year-end total in check.

---

## ⭐ Virtual Queue (Q(t))
- **What it is**: An artificial queue variable introduced by Lyapunov optimization to track the accumulated amount by which a long-term constraint has been violated or is at risk of being violated.
- **In this paper**: Tracks how much the UAV's cumulative flight energy consumption has exceeded the allowed threshold EU_th. Updated each slot as Q(t+1) = max{Q(t) + P(t)Δ − EU_th, 0}. The DRL agent observes Q(t) as part of its state and is penalized when the queue grows.
- **Easy way to remember**: It is like a running overdraft counter. Each time slot you overspend energy, the counter goes up; the agent is trained to keep the counter from growing too large.

---

## ⭐ Diffusion Model (DDPM — Denoising Diffusion Probabilistic Model)
- **What it is**: A generative model that learns to produce structured outputs by reversing a noise-adding process. In the forward process, Gaussian noise is progressively added to a clean sample over many steps until it looks like pure noise. In the reverse process, a neural network (the denoiser) learns to remove noise step by step, reconstructing the original sample.
- **In this paper**: Adapted as the actor network in D3PG. The "image" being generated is the action vector (channel allocation, power levels, altitude adjustment). Starting from Gaussian noise, I denoising steps produce the final action. The denoiser ηθ is parameterized by an MLP that takes the current noisy action, the step index, and the system state as inputs.
- **Easy way to remember**: Think of a sculptor revealing a statue from a block of marble — each chisel stroke (denoising step) removes a bit of "noise" until the final shape (the optimal action) emerges.

---

## ⭐ CSI Feedback Delay (Tdelay)
- **What it is**: The time lag between when a channel changes in the physical world and when the receiver receives an updated channel state information (CSI) report. In vehicular networks, this lag causes the base station to act on outdated channel data.
- **In this paper**: V2V channel gains (hV_k(t) and hV_{m,k}(t)) are periodically reported to the UAV, introducing a delay of Tdelay milliseconds. The mismatch between the estimated and actual channel is modeled using a first-order Gauss-Markov process involving the zero-order Bessel function J0. D3PG explicitly incorporates this delay into its state and decision-making; D3PG-WCSI (the ablation baseline) does not.
- **Easy way to remember**: Like getting last week's weather forecast to plan today's clothes — the information is stale, and ignoring how stale it is leads to bad decisions.

---

## DDPG (Deep Deterministic Policy Gradient)
- **What it is**: An actor-critic DRL algorithm for continuous action spaces. The actor is a deterministic policy network (no random sampling of actions); the critic estimates the Q-value. Uses a replay buffer and target networks for stable training.
- **In this paper**: Serves as a benchmark to show what happens when the diffusion model actor is replaced by a standard MLP actor. D3PG consistently outperforms DDPG because the MLP actor often converges to local optima.
- **Easy way to remember**: DDPG picks actions like a GPS that computes one fixed route; D3PG is like exploring multiple routes before committing.

---

## TD3 (Twin Delayed DDPG)
- **What it is**: An extension of DDPG that addresses overestimation bias in the critic by using two critic networks and delayed policy updates.
- **In this paper**: Mentioned in related work as an existing DRL approach for continuous control; not used directly as a baseline.
- **Easy way to remember**: DDPG with a double-check system for the critic's estimates.

---

## SAC (Soft Actor-Critic)
- **What it is**: An off-policy actor-critic DRL algorithm that maximizes both cumulative reward and entropy, encouraging exploration naturally.
- **In this paper**: Referenced in related literature; not one of the evaluated baselines.
- **Easy way to remember**: SAC adds a "curiosity bonus" to the reward, preventing the agent from becoming too single-minded.

---

## H-DDQN (Hungarian Algorithm + Double Deep Q-Network)
- **What it is**: A hybrid algorithm where channel allocation is solved with the Hungarian matching algorithm (a classical combinatorial optimizer) and power/altitude decisions are made by a Double DQN (value-based DRL with discrete action levels).
- **In this paper**: One of the three benchmark baselines. Its weakness is that DDQN forces power and altitude into discrete levels, reducing control precision.
- **Easy way to remember**: H-DDQN is like choosing from a fixed menu; D3PG can order anything off the full kitchen.

---

## UAV (Unmanned Aerial Vehicle)
- **What it is**: A remotely piloted or autonomous aircraft. In communications research, UAVs are deployed as aerial base stations or relay nodes.
- **In this paper**: A single UAV travels at 50 km/h along a Xiamen highway, acting as an aerial base station for V2U links while V2V links share its spectrum. Its flight altitude is continuously adjusted as one of the optimization variables.
- **Easy way to remember**: A flying cell tower on a budget — powerful but with a limited battery life.

---

## V2X (Vehicle-to-Everything)
- **What it is**: An umbrella term for communication between a vehicle and any entity: infrastructure (V2I), other vehicles (V2V), network (V2N), or UAVs (V2U).
- **In this paper**: The overarching communication paradigm. The system includes both V2U (vehicle uploads sensing data to the UAV) and V2V (vehicles exchange safety messages directly) links sharing the same spectrum.
- **Easy way to remember**: V2X = "vehicle talks to anything."

---

## V2U (Vehicle-to-UAV)
- **What it is**: Uplink communication from a ground vehicle to a UAV acting as an aerial base station.
- **In this paper**: The primary communication link whose sum rate D3PG is trying to maximize. M = 10 V2U links are pre-allocated to orthogonal channels.
- **Easy way to remember**: A car sending its sensor data up to a flying tower.

---

## V2V (Vehicle-to-Vehicle)
- **What it is**: Direct communication between two vehicles using device-to-device (D2D) technology, used for safety-critical messages like collision warnings.
- **In this paper**: K V2V links reuse the spectrum of V2U channels (spectrum sharing), creating co-channel interference. The reliability of V2V links is protected by a SINR constraint C7.
- **Easy way to remember**: Cars talking directly to each other without going through any base station.

---

## V2I (Vehicle-to-Infrastructure)
- **What it is**: Communication from a vehicle to a fixed ground infrastructure element (base station, roadside unit).
- **In this paper**: Mentioned as context for why UAVs are needed (V2I infrastructure is inadequate in rural/highway areas). Not the focus of the system model.
- **Easy way to remember**: A car checking in with a fixed roadside tower.

---

## LoS (Line-of-Sight)
- **What it is**: A propagation condition where the transmitter and receiver have an unobstructed direct path, resulting in much lower path loss than non-line-of-sight (NLoS).
- **In this paper**: The probability of a LoS link between the UAV and a ground vehicle is modeled as a function of the elevation angle. One motivation for adjusting UAV altitude is to increase the LoS probability, which directly improves channel quality.
- **Easy way to remember**: Clear sky between the car and the drone versus blocked by buildings — LoS is the clear path.

---

## NLoS (Non-Line-of-Sight)
- **What it is**: A propagation condition where the direct path between transmitter and receiver is obstructed. Path loss is significantly higher than LoS.
- **In this paper**: Accounted for in the path loss model via additional attenuation αNLoS = 20 dB (versus αLoS = 1 dB). The channel gain is a weighted sum of LoS and NLoS components.
- **Easy way to remember**: The signal has to go around or through obstacles, losing strength.

---

## Sum Rate
- **What it is**: The total data rate achieved by summing the individual throughputs of all communication links in the network, measured in bits per second (Mbps).
- **In this paper**: The objective function: maximize (1/TM) Σ_t Σ_m R^U_m(t), the time- and link-averaged V2U uplink sum rate. Results in Fig. 8 show D3PG achieving up to 30.67% higher sum rate than H-DDQN.
- **Easy way to remember**: The total throughput of all cars uploading data to the UAV — a bigger number means more data gets through.

---

## SINR (Signal-to-Interference-plus-Noise Ratio)
- **What it is**: A measure of link quality: the ratio of the desired signal power to the sum of interference power and noise power. Higher SINR means a cleaner signal and a higher achievable data rate.
- **In this paper**: Central to both the V2U rate formula (Shannon capacity: R = B·log2(1+γ)) and the V2V reliability constraint C7 (γV_k(t) must exceed γV_th = 10 dB with outage probability ≤ 1%).
- **Easy way to remember**: Like the signal bars on a phone — but accounting for all the noise from neighboring transmissions.

---

## Rician Channel
- **What it is**: A wireless channel model where the signal has both a strong LoS component and scattered multipath components. Named after Stephen Rice.
- **In this paper**: The V2U channel model implicitly combines LoS and NLoS components using a probabilistic weighting. The V2V channel uses Rayleigh small-scale fading (CN(0,1)) — a special case of Rician where the LoS component is absent.
- **Easy way to remember**: Rician = mostly one strong path plus some scattered echoes; Rayleigh = only scattered echoes, no dominant path.

---

## Gauss-Markov Process (for CSI delay modeling)
- **What it is**: A stochastic process where the current value depends on the previous value plus Gaussian noise, capturing temporal correlation in time-varying channels.
- **In this paper**: Used to model how the small-scale fading coefficient evolves over a feedback delay Tdelay. The correlation coefficient is J0(2π·fc·srel/c·Tdelay), the zero-order Bessel function, which decreases as speed or delay increases.
- **Easy way to remember**: Yesterday's channel + a bit of randomness = today's channel estimate.

---

## Bessel Function J0
- **What it is**: A mathematical function that appears naturally in models of waves and oscillations. J0(x) starts at 1 for x=0 and decreases toward zero as x grows.
- **In this paper**: J0(2π·fc·srel·Tdelay/c) measures the correlation between the reported CSI and the true current channel. As vehicle speed (srel) or delay (Tdelay) increases, J0 decreases, meaning the CSI becomes less reliable.
- **Easy way to remember**: J0 is a fidelity score for the CSI report — the closer to 1, the more trustworthy.

---

## OFDM (Orthogonal Frequency Division Multiplexing)
- **What it is**: A multi-carrier modulation scheme that divides the spectrum into multiple orthogonal subchannels, each carrying a portion of the data.
- **In this paper**: The spectrum is divided into M = 10 orthogonal channels. Each V2U link occupies one channel; V2V links reuse these channels (spectrum sharing).
- **Easy way to remember**: OFDM is like a highway with multiple lanes — each car (V2U link) gets its own lane, but V2V cars can share lanes if traffic permits.

---

## MINLP (Mixed-Integer Nonlinear Programming)
- **What it is**: An optimization problem class where some variables are integers (discrete) and others are continuous, with nonlinear constraints or objectives. MINLP is generally NP-hard.
- **In this paper**: The original joint channel allocation, power control, and UAV altitude adjustment problem P1 is an MINLP: channel allocation is binary (integer), while power and altitude are continuous. The nonlinearity comes from SINR expressions.
- **Easy way to remember**: A puzzle where some pieces snap into fixed positions (discrete) and others slide freely (continuous) — solving it optimally is very expensive.

---

## MDP (Markov Decision Process)
- **What it is**: A mathematical framework for sequential decision-making under uncertainty, consisting of states, actions, transition probabilities, and rewards.
- **In this paper**: The per-slot problem P2 (after Lyapunov transformation) is cast as an MDP. The state includes channel gains and virtual queue length; actions are channel allocation, power, and altitude; the reward is the negative of the P2 objective.
- **Easy way to remember**: The formal language that DRL speaks — states, choices, consequences, repeat.

---

## Replay Buffer
- **What it is**: A memory bank in off-policy DRL that stores past experience tuples (state, action, reward, next state). Random mini-batches are sampled from it to train the neural networks, breaking temporal correlations.
- **In this paper**: Used in D3PG (inherited from DDPG) to store transition tuples [s(t), a(t), r(t), s(t+1)] and provide decorrelated training samples.
- **Easy way to remember**: A recycling bin for past experiences — the agent learns from its history, not just what happened a moment ago.

---

## Target Network
- **What it is**: A copy of the actor or critic network whose parameters are updated slowly (soft updates) rather than after every gradient step. Stabilizes training by providing consistent TD targets.
- **In this paper**: D3PG uses both a target actor (ηˆθ) and a target critic (Qˆϕ), updated with rate τ = 0.005 per step.
- **Easy way to remember**: Like having a "reference version" of the policy that updates slowly so training doesn't chase a moving target.

---

## SUMO (Simulation of Urban MObility)
- **What it is**: An open-source, microscopic traffic simulation platform that generates realistic vehicle trajectories on real or synthetic road networks.
- **In this paper**: Used to simulate vehicle mobility on a real Xiamen, China highway network imported from OpenStreetMap. Vehicle positions at each time slot are recorded and fed into the wireless channel simulator.
- **Easy way to remember**: The virtual city where the paper's cars drive — it makes the simulation believable because the roads and traffic patterns are real.

---

## OpenStreetMap
- **What it is**: A free, collaborative geographic database providing detailed road network data worldwide.
- **In this paper**: The Xiamen highway road network is extracted from OpenStreetMap and imported into SUMO to define the simulation geometry.
- **Easy way to remember**: Wikipedia for maps — anyone can contribute, and researchers use it to ground simulations in real geography.

---

## Flight Altitude Adjustment (ΔH(t))
- **What it is**: One of the three optimization variables. The UAV can increase or decrease its altitude by up to ΔH_max = 5 m per time slot, within a range of [50, 200] m.
- **In this paper**: Adjusting altitude changes the 3D distance to vehicles, altering path loss and LoS probability. Higher altitude generally improves LoS but increases distance; lower altitude does the opposite.
- **Easy way to remember**: The UAV is like an elevator — go up for a better view (LoS), go down to get closer to specific vehicles.

---

## Lyapunov Drift-Plus-Penalty Function (D(Q(t)))
- **What it is**: The combined objective function in Lyapunov optimization that trades off constraint satisfaction (via the drift term ΔL) against the original objective (via the penalty term −V·reward).
- **In this paper**: D(Q(t)) = ΔL(Q(t)) − V·E{sum rate | Q(t)}. By minimizing an upper bound on D(Q(t)), the long-term problem P1 is transformed into the per-slot problem P2. The parameter V balances how aggressively the agent pursues throughput vs. energy conservation.
- **Easy way to remember**: A teeter-totter — V controls how much weight is on the "maximize throughput" side vs. the "don't waste energy" side.

---

## Denoiser (ηθ)
- **What it is**: The neural network at the heart of the diffusion model that, at each reverse step i, takes a noisy action vector πi(t), the step index i, and the system state s(t) as inputs, and predicts the noise component to be removed.
- **In this paper**: Implemented as a three-layer fully connected MLP with ReLU activations and a tanh output layer. It serves as the actor network in D3PG.
- **Easy way to remember**: The denoiser is the sculptor's chisel — applied repeatedly, it removes noise until the clean action (the statue) is revealed.

---

## UAV Propulsion Energy Model
- **What it is**: A physics-based model of how much electrical power a rotary-wing UAV consumes for flight. It includes blade profile power (maintaining rotor spin), induced power (generating lift), parasite power (aerodynamic drag), and vertical flight power.
- **In this paper**: The energy model (equation 13) depends on the UAV's horizontal and vertical velocity components. This energy must be managed within the long-term constraint C8 (average power ≤ EU_th = 120 J per slot).
- **Easy way to remember**: Flying is expensive — hovering, moving sideways, and climbing each burn a different amount of battery.
