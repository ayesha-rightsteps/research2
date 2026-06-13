# Background: GAT-A2C for C-V2X Resource Allocation

## Field

This paper sits at the intersection of **cellular vehicle-to-everything (C-V2X) communication** and **deep reinforcement learning (DRL) combined with graph neural networks (GNNs)**. The core problem is dynamic radio resource management (RRM) in vehicular networks: allocating spectrum resource blocks (RBs) and controlling transmission power across V2V and V2N links simultaneously, in real time, as vehicles move and interference patterns shift.

---

## Evolution of the Field

### Stage 1 — Early Vehicular Communication Standards (DSRC)
Dedicated Short-Range Communications (DSRC), based on IEEE 802.11p, was the first standardized approach to vehicle-to-vehicle and vehicle-to-infrastructure communication. It uses a 5.9 GHz band with contention-based channel access. While low-latency, it lacks centralized coordination and struggles in dense deployments due to hidden-node problems and limited QoS control.

### Stage 2 — Cellular V2X (C-V2X)
3GPP introduced C-V2X as a cellular alternative starting with LTE-V2X (Release 14) and later NR-V2X (Release 16). C-V2X operates in two modes:
- **Mode 3 / Mode 1 (centralized):** The network (base station) schedules resource blocks for vehicles. Provides good coordination but requires heavy signaling overhead and accurate global channel state information (CSI), creating scalability issues.
- **Mode 4 / Mode 2 (distributed/sidelink):** Vehicles select resources semi-autonomously using sensing-based schemes. Lower signaling cost but subject to frequent collisions and unreliable links in dense scenarios.

C-V2X allows V2V and V2N links to share the same OFDM spectrum via **in-band overlay**, which introduces inter-link interference that must be managed.

### Stage 3 — Traditional Optimization Approaches
Classical RRM methods (convex optimization, game theory, fractional programming) offer provably optimal or near-optimal solutions under stationary channel models but break down in highly mobile vehicular environments due to:
- High mobility causing rapidly changing channel conditions.
- Non-stationarity making closed-form solutions outdated before they can be acted upon.
- Scalability limits as vehicle density increases.

### Stage 4 — Deep Reinforcement Learning (DRL)
DRL-based approaches (DQN, DDPG, Double DQN/DDQN, Actor-Critic) introduced agents that learn resource allocation policies through environment interaction, removing the need for explicit channel models. Key works (Ye et al. 2019; Miao et al. 2022) demonstrated DRL could outperform heuristic baselines for V2V resource selection.

**Limitations of standalone DRL:**
- Agents treat states as flat feature vectors, with no explicit representation of interference topology.
- Each agent operates on local observations alone, making it blind to which neighbors are likely to cause the most harmful interference.
- Fixed-size state-action spaces struggle with varying vehicle densities.

### Stage 5 — GNN + DRL Hybrid Approaches
Researchers recognized that vehicular networks have inherent **graph structure**: V2V links are nodes, and interference relationships form edges. Graph Neural Networks (GNNs) — including Graph Convolutional Networks (GCNs) and GraphSAGE — were integrated with DRL to produce richer, topology-aware state representations.

A key prior work (Ji et al. 2025) combines GraphSAGE with Double DQN (GNN-DDQN). GraphSAGE aggregates neighbor features by sampling and averaging, treating all neighbors with equal or predefined weights.

**Remaining limitations of GNN-DRL:**
- GCN and GraphSAGE do not differentiate the relative importance of neighbors — all contributing links are weighted equally (or by fixed structural rules).
- Most methods use static or infrequently updated graph structures.
- DDQN is a value-based method requiring a discrete action space, with limited policy expressiveness compared to policy-gradient approaches.

### Stage 6 — GAT + A2C (This Paper)
This paper proposes **GAT-A2C**, advancing the state of the art in two dimensions:

1. **Graph Attention Network (GAT) instead of GCN/GraphSAGE:** GAT computes learnable attention weights for each neighbor, allowing the model to assign higher weight to links that pose genuine interference threats. This attention mechanism is trained end-to-end and adapts to real-time channel conditions, producing interference-aware node embeddings.

2. **Advantage Actor-Critic (A2C) instead of DDQN:** A2C is a policy-gradient method that maintains both a policy network (Actor) and a value network (Critic). It produces a probability distribution over actions and uses the advantage function to reduce variance in gradient updates, leading to more stable and flexible policy learning than DDQN.

Additionally, the graph's adjacency matrix is **dynamically reconstructed** at each time step based on current vehicle positions and channel states, ensuring the interference model stays current.

---

## Key Technologies

### C-V2X Standard
Defined under 3GPP specifications (LTE-V2X, NR-V2X). This paper follows the channel model of 3GPP TR 36.885 (a precursor technical report for vehicular channel modeling), using a Manhattan grid intersection scenario at 2 GHz, 20 total resource blocks, and channel components including path loss, shadowing, and fast fading. V2V and V2N share spectrum through in-band overlay.

### Graph Attention Network (GAT)
GAT (Velickovic et al., 2017) extends GCNs by introducing a learnable attention mechanism. For each pair of adjacent nodes (x, y), GAT computes an attention score using a shared learnable attention vector applied to the concatenated feature representations of both nodes. These scores are normalized via softmax to form a probability distribution over neighbors. The final node embedding is a weighted sum of neighbor features under these attention weights. Multi-head attention runs K independent attention heads in parallel and concatenates (or averages) their outputs, improving representational diversity and stability.

**Comparison with related GNN variants:**
- **GCN:** Aggregates neighbor features using fixed, normalized structural weights (degree-based). No learnable attention.
- **GraphSAGE:** Samples a fixed number of neighbors and aggregates using mean, LSTM, or max-pooling — uniform treatment with no attention-based differentiation.
- **GAT:** Learns which neighbors matter most via trainable attention, making it best suited for heterogeneous interference patterns where some neighbors are much more harmful than others.

### Attention Mechanism
In this paper's context, the attention weight alpha(x,y) for a neighbor y of node x reflects the **estimated interference importance** of link y to link x. Higher alpha means link y is more likely to cause resource conflicts with link x. This guides the RL agent to proactively avoid high-interference neighbors when selecting resource blocks.

The attention score is computed as:
`e(x,y) = LeakyReLU(a^T [h_x || h_y])`
then normalized via softmax over all neighbors of x.

### Multi-Head Attention
K attention heads independently compute attention coefficients and aggregated outputs; their outputs are concatenated. The paper uses K=8 heads in the first GAT layer, with the output embedding dimension set to 20. Multi-head attention improves the expressiveness and stability of feature aggregation.

### Advantage Actor-Critic (A2C)
A2C belongs to the policy-gradient family of RL algorithms. It maintains:
- **Actor network:** Outputs a probability distribution over actions given the current state. Updated by maximizing expected advantage, with an entropy regularization term to prevent premature convergence.
- **Critic network:** Estimates the Q-value (state-action value). Updated by minimizing the temporal difference (TD) error.
- **Advantage function:** A(s,a) = Q(s,a) - V(s). Measures how much better a chosen action is compared to the average action under the current policy. Positive advantage increases action probability; negative advantage decreases it.

A2C uses soft target network updates (Polyak averaging) to stabilize training.

### Interference Graph
The vehicular network is modeled as a directed graph G = (N, E) where:
- Each **node** N(i,j) represents a unidirectional V2V communication link from transmitter i to receiver j.
- Each **edge** E(x,y) indicates that node x can cause interference to node y (i.e., they may share the same resource block and the transmission of x degrades reception at y's receiver).

To maintain tractability, each vehicle selects three output links (to the nearest 20% of vehicles), keeping the total node count linear in vehicle count (~3n nodes). The adjacency matrix is binary (1 if interference exists, else 0), with self-loops (diagonal = 1). It is rebuilt every T seconds (typically 0.1–1 s) or upon significant position changes.

### Dynamic Adjacency Matrix
At each time step, the adjacency matrix A (size 3k x 3k, where k is the number of V2V pairs) is reconstructed based on current vehicle positions, channel states, and resource usage. This ensures GAT aggregates up-to-date interference information rather than stale topology data.

### Manhattan Grid Scenario
Simulation follows 3GPP TR 36.885 settings: an intersection with roads in a grid pattern, vehicles distributed according to a Poisson process, speeds between 36–54 km/h assigned via Gaussian distribution, 2 GHz carrier frequency, 20 resource blocks, (2+2)x4 lane configuration, 150 m neighbor distance threshold. Both LOS and NLOS channel conditions are modeled.

### SINR and Channel Model
SINR for any receiver V_i:
`SINR(V_i) = P(V_i) / (sum of interfering powers + noise)`
Channel gain includes path loss (PL), shadowing (Ls), and fast fading (Lf). The channel capacity follows Shannon's formula C = B * log2(1 + SINR). V2V transmission success ratio is modeled with a sigmoid function of SINR relative to a minimum decoding threshold.

---

## How This Paper Differs from Paper-1 (GraphSAGE + DDQN)

| Dimension | Paper-1 (GNN-DDQN / GraphSAGE) | Paper-8 (GAT-A2C) |
|---|---|---|
| GNN type | GraphSAGE — uniform neighbor sampling and aggregation | GAT — learnable attention weights per neighbor pair |
| Neighbor differentiation | Equal or sampled treatment; no priority for high-interference links | Attention scores explicitly quantify interference importance |
| RL algorithm | Double DQN (value-based, discrete, fixed action-value table) | A2C (policy-gradient, probability distribution over actions, advantage-based updates) |
| Policy gradient | No | Yes — entropy regularization prevents local optima |
| Graph dynamics | Typically static or slowly updated | Dynamically rebuilt at each timestep based on real-time positions and channel states |
| Training paradigm | Centralized training / distributed execution | Centralized training / distributed execution (explicit) |

The key insight is that not all interfering neighbors are equally harmful. GAT's attention mechanism allocates computational weight proportionally to interference severity, producing embeddings that guide the agent toward more targeted resource avoidance. A2C's policy-gradient approach handles the exploration-exploitation tradeoff more gracefully than value-based DDQN in continuous-density environments.

---

## Position in the Bigger Picture

This paper addresses a specific but critical sub-problem within intelligent transportation systems (ITS): how to maintain reliable V2V safety communication (>95% success ratio) while simultaneously maximizing V2N throughput, under the spectrum constraints of C-V2X in-band overlay, across varying vehicle densities (20–100 vehicles).

The broader research landscape this work connects to:
- **6G vehicular networks:** Future networks will need distributed, adaptive resource management at far greater vehicle densities. GAT-A2C's scalability (demonstrated across 20–100 vehicles) and its O(n³H) complexity (with n = vehicle count, H = attention heads) sets a baseline for understanding where GNN-DRL methods hit computational bottlenecks.
- **Multi-agent RL (MARL):** Each V2V link acts as an independent agent in a centralized-training/distributed-execution paradigm. The paper acknowledges that future integration with MARL (mean-field MARL, MADDPG variants) could further improve coordination.
- **Graph-based resource management:** By demonstrating that attention weights naturally capture interference priority, this work motivates further use of GAT in other wireless resource problems (e.g., power control in heterogeneous networks, beam management in mmWave systems).
- **Comparison point for the research series:** Within a series studying GNN+DRL approaches to V2X resource management, this paper establishes the GAT+A2C combination as superior to GCN/GraphSAGE+DDQN, especially in high-density (80–100 vehicle) scenarios where interference topology is complex and rapidly changing.
