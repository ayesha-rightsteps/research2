# Terminology: GNN + DRL for V2X Resource Allocation

> Quick reference for every technical term in this paper.

---

## V2X (Vehicle-to-Everything) ⭐
**What it is:** A category of wireless communication where vehicles exchange data with everything around them — other vehicles, infrastructure, pedestrians, and networks.
**In this paper:** The broader context. The paper focuses on C-V2X (cellular V2X) where 4G/5G networks are used.
**Easy way to remember:** Like a car that can "talk" to everything — V2V = car to car, V2I = car to tower, V2P = car to pedestrian.

---

## C-V2X (Cellular Vehicle-to-Everything) ⭐
**What it is:** The 3GPP-standardized version of V2X that uses cellular (LTE/5G NR) infrastructure and spectrum for vehicle communication.
**In this paper:** The specific standard the paper operates under. Resource blocks and power levels follow C-V2X specs.
**Easy way to remember:** V2X that runs on your phone network's infrastructure.

---

## V2V (Vehicle-to-Vehicle)
**What it is:** Direct communication between two vehicles — safety messages like "I'm braking" or "there's an obstacle ahead."
**In this paper:** V2V links have strict reliability requirements (success rate must be >95%). Their spectrum needs to be protected from V2I interference.
**Easy way to remember:** Car to car — walkie-talkie between cars.

---

## V2I (Vehicle-to-Infrastructure)
**What it is:** Communication between a vehicle and roadside infrastructure (base stations, RSUs).
**In this paper:** V2I links carry high-throughput data (maps, streaming). They share spectrum with V2V and can suffer interference from V2V transmissions.
**Easy way to remember:** Car talking to the road tower — like your phone calling a cell tower.

---

## Resource Block (RB) / Sub-channel
**What it is:** A fixed unit of wireless spectrum in LTE/5G — a specific combination of frequency range and time slot. There are a limited number of RBs available.
**In this paper:** Vehicles must be assigned an RB and a power level for each transmission. Two vehicles using the same RB cause interference.
**Easy way to remember:** Like lanes on a highway — each vehicle should ideally get its own lane; sharing causes accidents.

---

## GNN (Graph Neural Network) ⭐
**What it is:** A class of neural networks designed to work on graph-structured data (nodes + edges). Each node aggregates information from its neighbors to produce a richer feature representation.
**In this paper:** Used to model the interference structure of the V2X network. Vehicles are nodes; potential interference is edges. The GNN gives each vehicle awareness of its neighbors' resource usage.
**Easy way to remember:** A neural network that understands "who is connected to whom" — like social network analysis for vehicles.

---

## GraphSAGE (Graph Sample and Aggregation) ⭐
**What it is:** A specific GNN algorithm that learns how to sample and aggregate neighbor features to produce node embeddings. Inductive — works on new graphs without retraining.
**In this paper:** The specific GNN model used. Chosen because it can handle dynamic graphs (vehicle positions change every time step).
**Easy way to remember:** GraphSAGE "asks" a random sample of neighbors about themselves and combines the answers into one smart summary.

---

## DRL (Deep Reinforcement Learning) ⭐
**What it is:** Combining deep neural networks with reinforcement learning — the agent uses a neural network to represent its policy or value function, allowing it to handle high-dimensional state spaces.
**In this paper:** The decision-making backbone. The DRL agent learns to allocate resources by trial-and-error over many training episodes.
**Easy way to remember:** Teaching a car to drive by letting it crash many times in a simulator until it learns not to.

---

## DDQN (Double Deep Q-Network)
**What it is:** An improved version of DQN that uses two separate networks — one to select actions, one to evaluate them — reducing overestimation of Q-values that causes instability in training.
**In this paper:** The specific RL algorithm paired with GraphSAGE. Outputs discrete actions: which channel and power level to use.
**Easy way to remember:** DQN but double-checks itself before committing to an action.

---

## Q-value
**What it is:** The expected cumulative reward of taking action A in state S and following the optimal policy afterwards. The RL agent tries to learn the Q-value function.
**In this paper:** The agent learns Q(s, a) to decide which resource allocation action is best in each state.
**Easy way to remember:** A "goodness score" for each possible action — the agent picks the action with the highest score.

---

## CSI (Channel State Information)
**What it is:** The current properties of the wireless channel between a transmitter and receiver — path loss, fading, noise. Accurate CSI is needed for optimal resource allocation.
**In this paper:** CSI is used to build the interference graph and inform resource decisions. In high-speed scenarios, CSI becomes noisy and outdated quickly.
**Easy way to remember:** Knowing how clear or noisy the road is before deciding how fast to drive.

---

## SINR (Signal-to-Interference-plus-Noise Ratio)
**What it is:** A measure of communication link quality. High SINR = strong signal, little interference = good communication. Low SINR = link fails.
**In this paper:** V2V success is determined by whether SINR exceeds a threshold. V2I rate depends on SINR as well.
**Easy way to remember:** Like signal bars on your phone — more bars means better connection.

---

## IoV (Internet of Vehicles)
**What it is:** The broader ecosystem of connected vehicles — including V2X communication, cloud connectivity, edge computing, and AI-based driving decisions.
**In this paper:** Mentioned as the application context. V2X resource allocation is a key enabling technology for IoV.
**Easy way to remember:** Vehicles on the internet — smart cars that are always connected.

---

## NP-Hard
**What it is:** A class of computational problems where no known algorithm can solve all instances efficiently (in polynomial time). Resource allocation in wireless networks is typically NP-hard.
**In this paper:** The reason classical optimization can't work in real-time — the search space is too large. Motivates the DRL approach.
**Easy way to remember:** Problems so hard that brute force is the only guaranteed method — but brute force takes forever.
