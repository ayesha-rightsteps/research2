# Terminology: GAT-A2C for C-V2X Resource Allocation

---

## C-V2X (Cellular Vehicle-to-Everything) ⭐

**Definition:** A wireless communication standard defined by 3GPP that enables vehicles to communicate with other vehicles, infrastructure, networks, and pedestrians using cellular radio technology (LTE and 5G NR).

**Details:** C-V2X operates in licensed and unlicensed spectrum bands (typically around 5.9 GHz, with some deployments at 2 GHz as in this paper). It defines two operational modes: centralized scheduling (Mode 3 / Mode 1), where the base station assigns resources, and distributed sidelink (Mode 4 / Mode 2), where vehicles self-organize resource selection. In this paper, V2V and V2N links **share the same OFDM spectrum via in-band overlay**, meaning they can mutually interfere and require careful resource coordination.

**Why it matters:** C-V2X is the standardized baseline for vehicular communication in 5G-era ITS deployments. Understanding its resource block structure, interference model, and mode distinctions is essential context for the resource allocation problem this paper solves.

---

## V2V (Vehicle-to-Vehicle)

**Definition:** Direct wireless communication between two vehicles, typically without going through infrastructure.

**Details:** V2V communication is used for safety-critical applications such as collision avoidance, cooperative lane changing, and emergency braking warnings. Key requirements are ultra-low latency and high reliability. In this paper, the metric for V2V performance is the **transmission success ratio** (RV2V = correctly received packets / total packets sent), modeled as a sigmoid function of SINR. The paper targets RV2V > 95%.

**In the paper:** V2V links form the nodes of the interference graph. Each vehicle generates V2V links to its nearest neighbors. The goal is to allocate resource blocks and power levels to maximize V2V reliability while co-existing with V2N traffic.

---

## V2N (Vehicle-to-Network)

**Definition:** Wireless communication between a vehicle and a cellular base station (BS), enabling vehicles to access the broader network for non-safety services.

**Details:** V2N supports high-throughput applications such as real-time traffic updates, map downloads, remote diagnostics, and infotainment. Unlike V2V, V2N is latency-tolerant but throughput-sensitive. In this paper, V2N uplink communication shares the same OFDM band with V2V links (in-band overlay), creating interference. The V2N performance metric is **channel capacity** (Mbps), calculated via the Shannon formula using measured SINR.

**In the paper:** The reward function balances V2N rate (lambda * RV2N) and V2V success ratio ((1-lambda) * RV2V). Maximizing V2N rate while maintaining V2V reliability is the joint optimization target. At 100 vehicles, GAT-A2C achieves >20% higher V2N rate than DQN.

---

## Resource Block (RB) ⭐

**Definition:** The basic unit of spectrum allocation in LTE and NR cellular systems. An RB consists of a fixed number of OFDM subcarriers in the frequency domain over a defined time slot in the time domain.

**Details:** In this paper, there are **20 total RBs** in the system at a 2 GHz carrier frequency. Each V2V link agent must select one RB (for transmission) and a power level from {23, 10, 5} dBm. Two links sharing the same RB cause co-channel interference. The goal of resource block allocation is to assign RBs such that highly interfering links receive orthogonal RBs whenever possible.

**Action space:** The actor network selects from m different RB groups and 3 power levels, making the action space size 3m (with m = 20, action space = 60 actions).

---

## GAT (Graph Attention Network) ⭐

**Definition:** A type of graph neural network that uses learnable attention mechanisms to assign different importance weights to different neighbors when aggregating node features.

**Details:** Introduced by Velickovic et al. (2017). For each pair of adjacent nodes (x, y), GAT computes an attention score:
`e(x,y) = LeakyReLU(a^T [W*h_x || W*h_y])`
where W is a shared linear transformation matrix, a is a learnable attention vector, and || denotes concatenation. Scores are normalized via softmax over all neighbors to obtain attention weights alpha(x,y). The updated feature of node x is a weighted sum of its neighbors' transformed features under these weights.

**In this paper:** GAT serves as the feature encoding module for the RL state space. Each V2V link is a node; the GAT produces 20-dimensional embeddings capturing interference structure. The node feature vector includes V2N channel gain, V2V channel gain, and total received interference power. The paper uses a 2-layer GAT with 8 attention heads in the first layer.

**GAT vs. GCN vs. GraphSAGE:**
- GCN: Fixed, degree-normalized aggregation weights. No learnable neighbor priority.
- GraphSAGE: Samples neighbors and aggregates uniformly (mean/max). Better for inductive settings but still no attention.
- GAT: Learns which neighbors matter most. Ideal for heterogeneous interference where some links are far more harmful than others.

---

## Attention Mechanism

**Definition:** A neural network component that computes a weighted combination of inputs, where the weights (attention scores) are learned and reflect the relative importance of each input to the current context.

**Details:** In the GAT context, the attention mechanism operates between pairs of nodes. The attention score e(x,y) measures how important neighbor y is to the representation of node x. After softmax normalization, alpha(x,y) forms a probability distribution summing to 1 over all neighbors of x.

**Interpretation in V2V context:** A high attention weight alpha(x,y) means link y is predicted to cause significant interference to link x. The agent, receiving this signal in its state, learns to select resource blocks that avoid conflicting with high-attention neighbors.

---

## Attention Weight (alpha)

**Definition:** The normalized importance score assigned by GAT to a neighbor node's contribution to the current node's feature update.

**Formula:**
`alpha(x,y) = exp(e(x,y)) / sum_{k in N(x)} exp(e(x,k))`
where N(x) is the neighborhood of x.

**Properties:** Attention weights sum to 1 over all neighbors (softmax normalization), preventing any single neighbor from dominating. A node with many low-interference neighbors will have diffuse attention; a node with one dominant interferer will concentrate attention there.

---

## Multi-Head Attention

**Definition:** A technique where multiple independent attention mechanisms (heads) are applied in parallel to the same node pair, and their outputs are concatenated (or averaged) to produce the final node embedding.

**Details:** Each head k uses independent weight matrix W_k and attention vector a_k, producing its own set of attention weights and aggregated features. The K outputs are concatenated:
`z_x = Concat[ReLU(sum_{y in N(x)} alpha_k(x,y) * h_k_y)] for k = 1..K`

**In this paper:** K = 8 heads in the first GAT layer. Ablation experiments show 8 heads is optimal — fewer heads miss complex interference patterns; more heads introduce overfitting and computational overhead without proportional gains.

---

## A2C (Advantage Actor-Critic) ⭐

**Definition:** A policy-gradient reinforcement learning algorithm that combines a policy network (Actor) and a value network (Critic), using the advantage function to reduce variance in gradient estimates.

**Details:**
- **Actor:** Parameterized policy pi(a|s; theta_pi) — outputs a probability distribution over actions for a given state. Updated by maximizing J(theta_pi) = E[log pi(a|s) * A(s,a)] plus an entropy regularization term beta*H(pi) that discourages premature convergence.
- **Critic:** Parameterized Q-value estimator Q(s,a; theta_Q). Updated by minimizing the TD (temporal difference) error: (r + gamma * Q_target(s', argmax_a pi(a|s')) - Q(s,a))^2.
- **Advantage function:** A(s,a) = Q(s,a) - V(s), where V(s) = sum_a pi(a|s) * Q(s,a) is the expected state value. A(s,a) > 0 means the action outperforms average; A(s,a) < 0 means it is below average. This decomposition reduces variance compared to using raw Q-values.

**Comparison with DDQN:** DDQN is value-based (learns Q directly, picks argmax). A2C learns a stochastic policy, which handles exploration more naturally and avoids overestimation bias. A2C also benefits from the Critic providing baseline subtraction (via advantage), making training more stable.

**In this paper:** State input dimension = 102 (82 base features + 20 GAT embedding). Actor outputs distribution over 60 actions (20 RBs x 3 power levels). Critic estimates state-action value. Both networks have 3 hidden layers (500, 250, 120 neurons). Soft target network updates every C steps using Polyak averaging (tau = 0.01).

---

## Advantage Function

**Definition:** In RL, the advantage A(s,a) = Q(s,a) - V(s) measures how much better (or worse) a specific action is compared to the expected value of all actions in state s under the current policy.

**Role in A2C:** The Actor gradient update is proportional to A(s,a). Actions with positive advantage get their probability increased; actions with negative advantage get decreased. This is more stable than using raw Q-values because V(s) serves as a baseline, reducing variance.

**Practical computation:** In this paper, A(s,a) = Q(s,a; theta_Q) - sum_a pi(a|s)*Q(s,a; theta_Q), where the Critic provides both Q estimates.

---

## Actor-Critic

**Definition:** A class of RL algorithms that maintains both a policy function (Actor) and a value function (Critic) simultaneously. The Critic evaluates actions taken by the Actor; the Actor updates its policy using the Critic's evaluation.

**Relationship to other RL families:**
- Pure policy gradient (REINFORCE): Uses Monte Carlo returns — high variance.
- Value-based (DQN, DDQN): No explicit policy, uses argmax over Q-values — limited to discrete actions.
- Actor-Critic: Hybrid. The Critic's value estimate replaces noisy Monte Carlo returns, reducing variance while maintaining policy-gradient flexibility.

---

## GCN (Graph Convolutional Network)

**Definition:** A GNN architecture that generalizes convolution operations to graph-structured data. Each node updates its representation by aggregating features from its neighbors using fixed, normalization-based weights.

**Key formula:** h_v^(l+1) = sigma(sum_{u in N(v) union v} (1/sqrt(d_v * d_u)) * W * h_u^(l))
where d_v is the degree of node v and W is a shared weight matrix.

**Limitation for vehicular networks:** GCN weights are determined by graph structure (node degrees), not by learned relevance. All neighbors are treated with equal structural importance, making it unable to prioritize high-interference links.

---

## GraphSAGE (Graph Sample and Aggregate)

**Definition:** A GNN method that generates node embeddings by sampling a fixed number of neighbors and aggregating their features using learnable aggregation functions (mean, max-pooling, or LSTM).

**Key idea:** Inductive learning — can generalize to unseen nodes without retraining. Aggregation is uniform (mean) or fixed (max).

**Limitation for vehicular networks:** While inductive and scalable, GraphSAGE treats all sampled neighbors equally or by simple fixed operations. It cannot assign higher attention to more harmful interferers, making it less effective than GAT for interference-aware resource allocation.

**Relevance:** The primary GNN baseline in this paper's comparison set (Ji et al. 2025, GNN-DDQN) uses GraphSAGE. GAT-A2C consistently outperforms it, especially at 80–100 vehicles.

---

## Interference Graph

**Definition:** A graph representation where nodes correspond to communication links (not individual vehicles) and edges indicate potential interference between pairs of links.

**Details in this paper:** Each node N(i,j) represents the unidirectional V2V link from vehicle i to vehicle j. An edge E(x,y) is created between nodes x and y if they may share resource blocks and node x's transmission could degrade reception at node y's receiver. To keep node count tractable, each vehicle creates only 3 output links (to the nearest 20% of vehicles), giving approximately 3n total nodes for n vehicles.

**Dynamic updates:** The adjacency matrix A is an |LV2V| x |LV2V| binary matrix rebuilt at each time step (every 0.1–1 s) based on current positions and channel states. Self-loops (A(x,x) = 1) preserve each node's own features during GAT computation.

---

## Manhattan Grid

**Definition:** A simulation road topology that mimics a rectangular city block layout, with roads forming a regular grid (like Manhattan, New York City), including multiple lanes and intersections.

**In this paper:** The simulation scenario is an intersection within a Manhattan grid following 3GPP TR 36.885 parameters. The road has a (2+2)x4 lane configuration. Vehicles are distributed according to a Poisson distribution with speeds between 36–54 km/h (Gaussian). A base station is located at the center (intersection). The scenario tests vehicle densities from 20 to 100 vehicles, covering sparse to dense urban traffic.

---

## 3GPP TR 36.885

**Definition:** A 3GPP technical report titled "Study on LTE-based V2X Services" that defines channel models, deployment scenarios, and simulation parameters for vehicular communication research.

**Relevance:** This paper's simulation is based on the channel model and scenario settings defined in TR 36.885, including the Manhattan grid intersection layout, path loss models for LOS and NLOS conditions, antenna parameters (vehicle: 3 dBi at 1.5 m height; BS: 8 dBi at 25 m height), and noise figures. Using this standard ensures comparability with other vehicular communication research.

**Note:** The paper references TR 36.885 but cites the related TR 36.814 in its reference list — both are part of the 3GPP Release 9 vehicular channel modeling family.

---

## SINR (Signal-to-Interference-plus-Noise Ratio)

**Definition:** The ratio of the desired received signal power to the sum of all interfering signal powers plus background noise power.

**Formula in this paper:**
`SINR(V_i) = P(V_i) / (sum_{j != i} delta(j,i)*I(V_j) + delta(c,i)*I_c + N_0)`
where P(V_i) is the received signal power from the tagged link, I(V_j) is interference from vehicle j, I_c is interference from the base station, delta indicators equal 1 only when resource blocks overlap, and N_0 is noise power.

**Role:** SINR is the fundamental metric linking physical layer parameters (power, channel gain, resource allocation) to communication performance (V2V success ratio, V2N throughput via Shannon capacity). The resource allocation problem is fundamentally about maximizing SINR for desired links while minimizing SINR degradation caused by interference.

---

## Path Loss

**Definition:** The reduction in signal power density as a radio wave propagates through space, primarily due to geometric spreading and absorption.

**In this paper:** Path loss (PL) is one of three components of channel gain: GCh = 10^(-(PL + Ls + Lf)/10), where Ls is shadowing (slow fading due to obstacles) and Lf is fast fading (rapid fluctuations due to multipath). The 3GPP TR 36.885 model includes separate LOS and NLOS path loss formulas for the Manhattan grid scenario.

---

## Doppler Effect

**Definition:** The shift in observed frequency (and thus phase) of a radio signal due to relative motion between the transmitter and receiver.

**In vehicular networks:** Vehicle speeds of 36–54 km/h at 2 GHz produce Doppler shifts on the order of hundreds of Hz. This contributes to **fast fading** (the Lf term in channel gain) and can cause inter-carrier interference (ICI) in OFDM systems if the Doppler spread exceeds the subcarrier spacing. In this paper, the Doppler effect contributes to the time-varying nature of the channel that motivates dynamic adjacency matrix reconstruction and real-time state updates.

---

## Spectrum Sharing

**Definition:** The practice of multiple communication services or links using the same frequency band simultaneously, managed by coordination mechanisms to limit mutual interference.

**In this paper:** V2V and V2N communications share the same 20 RBs via **in-band overlay** (also called spectrum reuse). The BS uses some RBs for V2N uplink; V2V links can reuse these same RBs. The resulting co-channel interference between V2V and V2N links is the central problem the GAT-A2C framework addresses. Effective spectrum sharing requires choosing RBs and power levels to maximize SINR for each link while limiting interference to others.

---

## O(n³H) Complexity

**Definition:** The overall computational complexity of the GAT-A2C model per training iteration, where n is the number of vehicles and H is the number of attention heads.

**Derivation:**
- Each vehicle has 3 V2V output links → total nodes = 3n.
- GAT attention score computation requires O(n²H) operations (for the n x n adjacency interactions, with H heads).
- For all 3n nodes: O(3n * n²H) = O(n³H).
- The A2C network inference is O(3n * B) where B is batch size, which is subdominant for large n.

**Implication:** The cubic scaling in n means the model's computational cost grows rapidly with vehicle density. The paper identifies this as a key limitation: the model is best suited for moderate-density scenarios. A pretrain-and-deploy strategy (training at low density, deploying at higher density) is proposed as a practical mitigation.

---

## MDP (Markov Decision Process)

**Definition:** A mathematical framework for sequential decision-making. An MDP is defined by states S, actions A, transition probabilities P(s'|s,a), and reward function R(s,a,s').

**In this paper:** The resource allocation problem is modeled as an MDP where:
- **State** S_t: concatenation of local environment features [channel capacity, interference power, channel gain, neighbor resource occupancy, remaining transmission time, remaining data] plus the 20-dimensional GAT embedding.
- **Action** a_t: joint selection of resource block (from 20 RBs) and power level (from {23, 10, 5} dBm) → 60 possible actions.
- **Reward** r_t = lambda * RV2N + (1-lambda) * RV2V - (T_limit - T_left)/T_limit, balancing V2N rate, V2V success, and transmission urgency.

---

## Softmax Normalization

**Definition:** A function that converts a vector of raw scores (logits) into a probability distribution, where each element is in (0,1) and all elements sum to 1.

**Formula:** softmax(e_i) = exp(e_i) / sum_j exp(e_j)

**In GAT:** Used to normalize attention scores e(x,y) into attention weights alpha(x,y) over all neighbors of node x. Ensures the contributions of all neighbors sum to 1, preventing dominant neighbors from overwhelming the aggregated representation.

---

## LeakyReLU

**Definition:** An activation function that is linear for positive inputs (f(x) = x) and has a small negative slope for negative inputs (f(x) = alpha*x, typically alpha = 0.01), preventing the "dying ReLU" problem.

**In GAT:** Used in the attention score computation before softmax: e(x,y) = LeakyReLU(a^T [W*h_x || W*h_y]). The small negative slope allows gradients to flow even for negative attention pre-scores, stabilizing training.

---

## Temporal Difference (TD) Error

**Definition:** In RL, the TD error is the difference between the estimated value of the current state-action pair and the bootstrapped target value (immediate reward plus discounted estimate of next state value).

**Formula (in this paper):** TD error = y_Q - Q(s,a; theta_Q), where y_Q = r_t + gamma * Q_target(s', argmax_a pi_target(a|s')).

**Role:** The Critic network is trained to minimize the squared TD error, improving its ability to accurately estimate the value of state-action pairs. This in turn provides more accurate advantage estimates to the Actor.

---

## Replay Memory

**Definition:** A data structure (buffer) that stores past experience tuples (s, a, r, s') for later reuse in training, enabling decorrelated mini-batch sampling.

**In this paper:** Each V2V link agent stores transition tuples in a replay buffer of capacity 1 million. During training, mini-batches of size B = 2000 are sampled to update the Actor, Critic, and GAT networks. Replay memory breaks the temporal correlation between consecutive training samples, improving training stability.

---

## Centralized Training / Distributed Execution (CTDE)

**Definition:** A training paradigm where agents share global information during training (to learn better policies and value functions) but execute decisions independently using only local information at deployment time.

**In this paper:** During training, both the GAT and RL components use global network information (all vehicle positions, channel states, interference graph). During inference/deployment, each V2V link agent uses only its local state and the GAT embedding computed from its local subgraph. This makes the system scalable for real-world deployment where real-time global coordination is impractical.

---

## Entropy Regularization

**Definition:** A term added to the Actor's objective function that rewards maintaining high policy entropy (i.e., not being too deterministic), encouraging exploration.

**Formula:** H(pi(·|s; theta)) = -sum_a pi(a|s; theta) * log pi(a|s; theta)

**Role in A2C:** The Actor maximizes J'(theta) = J(theta) + beta * H(pi), where beta > 0 is the entropy coefficient. Entropy regularization prevents the policy from collapsing to a deterministic (and potentially suboptimal) action too quickly, helping avoid local optima in the non-stationary vehicular environment.

---

## Discount Factor (gamma)

**Definition:** A parameter in RL (0 < gamma <= 1) that determines how much future rewards are weighted relative to immediate rewards.

**In this paper:** gamma = 0.5 (labeled as lambda in the reward formula, distinct from the V2N/V2V balance coefficient also called lambda). A discount factor of 0.5 means the agent prioritizes near-term rewards, appropriate for a fast-changing vehicular environment where distant future states are uncertain.
