# Terminology: A Multi-Agent Resource Allocation Method for Unmanned Aerial Vehicle Networks based on Adaptive Spatial Reward Mechanisms and Context-Awareness

> Quick reference for all acronyms, models, metrics, and algorithms used in this paper. Terms marked with ⭐ are the five most central concepts.

---

## UAV (Unmanned Aerial Vehicle)
**What it is:** A remotely piloted or autonomously operated aircraft — colloquially a "drone" — that carries no human pilot on board.
**In this paper:** UAVs act as aerial base stations, flying at a fixed altitude of 100 m over a 500 m-radius area and providing downlink communication to ground users. Each UAV is an independent learning agent.
**Easy way to remember:** Think of each UAV as a flying Wi-Fi hotspot that decides on its own which channel and user to serve.

---

## MARL (Multi-Agent Reinforcement Learning) ⭐
**What it is:** A machine learning paradigm where multiple autonomous agents each learn their own behavioral policy by interacting with a shared environment, receiving individual rewards, and updating their strategies over time.
**In this paper:** MARL is the foundational framework. Each UAV operates as an independent MARL agent, selecting user, subchannel, and power level at each time slot without access to a central controller. The paper builds a MARL-based Actor-Critic structure as the baseline and then extends it with the two novel mechanisms.
**Easy way to remember:** MARL is how you train a team of robots to cooperate without a coach — each robot learns from its own experience while being aware of its teammates.

---

## ASAC (Adaptive Spatial reward and Agent Context-Awareness) ⭐
**What it is:** The paper's proposed algorithm, which extends standard MARL with two components: an adaptive spatial reward mechanism and an agent context-aware communication mechanism.
**In this paper:** ASAC is the main contribution. It achieves 16% higher resource allocation efficiency than the best baseline (MAB) in high-user-count (200-user) networks and converges faster than all compared methods.
**Easy way to remember:** ASAC = standard MARL + "care more about your close neighbors" + "listen to what your neighbors are doing right now."

---

## A2C / MA2C (Advantage Actor-Critic / Multi-Agent Advantage Actor-Critic)
**What it is:** A reinforcement learning architecture that combines two neural networks per agent: an actor (which selects actions according to a policy) and a critic (which estimates state values to compute an advantage signal for guiding the actor's updates).
**In this paper:** MA2C is the specific architecture implemented inside the ASAC framework. The advantage function is defined as the difference between the Q-function and the value function: A = Q - V. The actor and critic are updated using mini-batch gradient descent with separate learning rates.
**Easy way to remember:** The critic grades the actor's performance; the actor uses that grade to improve its strategy.

---

## Adaptive Spatial Reward Mechanism ⭐
**What it is:** A modification to the standard global reward signal in MARL that applies a geometric discount (kappa^d) to rewards from agents at distance d, so that nearby agents' rewards count more heavily than distant ones.
**In this paper:** The spatial discount factor kappa controls the balance between greedy local control (kappa near 0) and fully cooperative global control (kappa near 1). The aggregated global reward for agent m is summed over all agents within sensing distance D_m, weighted by kappa raised to the distance power.
**Easy way to remember:** Like a sound that fades with distance — a neighbor next door matters more to your reward than someone across the city.

---

## Agent Context-Aware Communication Mechanism ⭐
**What it is:** A mechanism by which each agent encodes its current hidden state and broadcasts it to neighboring agents, which then fuse received messages with their own observations using an LSTM encoder to form a richer input for policy decisions.
**In this paper:** Each agent m collects messages g_{m,t} = h_{i,t-1} (the previous hidden states of neighbors). These are combined with the agent's own encoded observation using feature extraction functions q_o and q_h, and then processed by a 64-neuron LSTM to produce a new hidden state h_{m,t} that drives both the actor and critic.
**Easy way to remember:** Each UAV "whispers" its current state to neighbors before making a decision — like players in a sports team calling out their position.

---

## SINR (Signal-to-Interference-plus-Noise Ratio) ⭐
**What it is:** A measure of communication signal quality: the power of the desired signal divided by the sum of interfering signals and background noise power. Higher SINR means better link quality.
**In this paper:** SINR is the central performance metric. Each UAV m computes its total SINR as the sum over all served users and subchannels. The reward is set to zero if SINR falls below the QoS threshold (gamma-bar = 2.7 dB in simulations), and equals throughput minus power cost otherwise.
**Easy way to remember:** SINR is the "signal clarity score" — a higher score means the message gets through with less interference.

---

## MDP (Markov Decision Process)
**What it is:** A mathematical framework for sequential decision making under uncertainty, defined by states, actions, transition probabilities, and rewards. It is the formal basis for reinforcement learning.
**In this paper:** The joint resource allocation problem (user selection, subchannel choice, power level) for each UAV is modeled as an MDP. The environment is characterized by channel states and user positions; each UAV aims to find the optimal action sequence to maximize long-term discounted reward.
**Easy way to remember:** An MDP is a formal "game rulebook" that tells a learning agent how its actions change the world and what rewards it gets.

---

## LoS (Line-of-Sight)
**What it is:** A propagation condition in which there is an unobstructed direct path between transmitter and receiver, resulting in stronger signals and less multipath fading compared to Non-Line-of-Sight (NLoS).
**In this paper:** UAV-to-ground channels are modeled using two approaches: a deterministic LoS free-space path loss model (channel gain depends on UAV altitude and horizontal distance, with reference gain beta_0 = -60 dB and path loss exponent alpha = 2) and a probabilistic model that weights LoS and NLoS path losses by their occurrence probabilities.
**Easy way to remember:** LoS is a clean direct shot between the UAV and the user — like shining a flashlight with nothing in the way.

---

## NLoS (Non-Line-of-Sight)
**What it is:** A propagation condition in which the direct path between transmitter and receiver is blocked or obstructed, resulting in weaker signals due to diffraction, reflection, and absorption.
**In this paper:** The probabilistic channel model accounts for NLoS with an additional attenuation factor eta_NLoS = 22 dB (compared to eta_LoS = 1.2 dB). The probability of LoS vs. NLoS depends on the elevation angle between the UAV and the ground user, parameterized by environmental constants a = 9.6 and b = 0.15.
**Easy way to remember:** NLoS is when buildings or terrain block the direct path — like trying to talk through a wall.

---

## A2G Channel (Air-to-Ground Channel)
**What it is:** The wireless communication channel between an airborne platform (UAV) and a ground-level device, which has different statistical properties from ground-to-ground channels due to the elevation angle and altitude.
**In this paper:** The A2G channel is central to the system model. It determines channel gain G^k_{m,l}(t) between UAV m and ground user l on subchannel k, which feeds directly into SINR calculations and thus into the reward function.
**Easy way to remember:** The A2G channel is the aerial "pipeline" through which data flows from the flying UAV down to users on the ground.

---

## DQN (Deep Q-Network)
**What it is:** A reinforcement learning algorithm that uses a deep neural network to approximate the Q-value function — the expected long-term reward for taking a given action in a given state.
**In this paper:** Each UAV agent initializes and trains a DQN to learn Q-values for its action space (combinations of user, subchannel, and power level). The Q-value update rule is a standard Bellman equation with learning rate alpha and time discount factor delta = 0.99.
**Easy way to remember:** A DQN is a neural network that acts as the agent's "memory of past rewards" — it learns which actions paid off in which situations.

---

## Epsilon-Greedy Strategy
**What it is:** An exploration-exploitation trade-off strategy in reinforcement learning where the agent chooses the action with the highest Q-value with probability (1 - epsilon) and a random action with probability epsilon.
**In this paper:** The epsilon-greedy strategy is used for action selection during training. Simulations show that epsilon = 0.5 yields optimal performance under both LoS and probabilistic channel models, as extreme values (0 or 1) harm training by either eliminating exploration or making actions completely random.
**Easy way to remember:** Epsilon is the agent's "curiosity dial" — turn it up to explore new options, turn it down to exploit what it already knows works.

---

## LSTM (Long Short-Term Memory)
**What it is:** A type of recurrent neural network cell designed to capture long-range temporal dependencies in sequential data by maintaining a gated memory cell.
**In this paper:** A 64-neuron LSTM encoder f_m processes each agent's concatenated observation (own state + neighbor messages) to produce a hidden state h_{m,t}. This hidden state carries temporal context across time steps and drives both the actor (policy) and critic (value) network outputs.
**Easy way to remember:** The LSTM is the agent's short-term "working memory" — it remembers recent history so current decisions account for what just happened.

---

## EMA Action Smoothing (Exponential Moving Average)
**What it is:** A technique that replaces the raw sampled action with a weighted average of the current action and the previous action, using a smoothing factor rho. This reduces sudden action changes (chattering) and stabilizes training.
**In this paper:** Applied before action execution: a_t = rho * a_{t-1} + (1 - rho) * a_t for t > 1. The smoothing factor rho and window size T_w are tuned as hyperparameters. Excessively large T_w slows responsiveness; too small fails to smooth.
**Easy way to remember:** EMA smoothing is like cruise control — it prevents jerky acceleration by blending the new command with recent history.

---

## Q-Value (Action-Value Function)
**What it is:** In reinforcement learning, Q(s, a) represents the expected total discounted reward an agent will receive if it takes action a in state s and then follows its policy thereafter.
**In this paper:** The Q-value update rule is the core of the MARL learning loop. Extended to the global Q-function Q-bar that incorporates neighborhood states H_m and neighbor policies pi_{N_m}, it captures spatial and social context beyond the agent's own local state.
**Easy way to remember:** Q-value is the agent's "estimated payoff" for each possible choice — the agent always picks the action with the highest Q-value when not exploring.

---

## Subchannel (Sub-channel)
**What it is:** A portion of the total available spectrum bandwidth, created by dividing the total bandwidth W into K orthogonal frequency bands. Each UAV selects at most one subchannel per time slot.
**In this paper:** The total bandwidth W is divided into K subchannels, each with bandwidth W/K = 75 kHz. Different UAVs may occupy the same subchannel, causing inter-UAV interference captured in the SINR formula. Subchannel selection is one of the three components of each agent's action.
**Easy way to remember:** Subchannels are like radio stations — each UAV picks a frequency, but two UAVs on the same frequency interfere with each other.

---

## QoS (Quality of Service) Threshold
**What it is:** A minimum performance requirement that must be satisfied for a communication link to be considered acceptable — in wireless networks, often expressed as a minimum SINR or data rate.
**In this paper:** The QoS constraint requires each UAV's total SINR to meet or exceed gamma-bar (set to 2.7 dB in simulations). If SINR falls below this threshold, the reward for that time slot is set to zero, forcing the agent to learn policies that reliably meet the requirement.
**Easy way to remember:** QoS threshold is the "pass/fail grade" — fall below it and you earn nothing that round.

---

## CSI (Channel State Information)
**What it is:** Knowledge of the current channel conditions between a transmitter and receiver, including path loss, fading, and interference.
**In this paper:** The paper assumes each UAV has CSI for all ground users (i.e., it knows channel gains G^k_{m,l}(t)) but does not have global knowledge of the full network environment, including other UAVs' channel states. This partial-knowledge setting is explicitly stated to match real-world constraints.
**Easy way to remember:** CSI is the "signal quality report card" — each UAV knows how well it can reach its own users, but not everything about its neighbors' situations.

---

## IA2C (Independent Advantage Actor-Critic)
**What it is:** A baseline MARL algorithm where each agent runs its own A2C independently, with no communication or information sharing with other agents.
**In this paper:** IA2C is one of the baseline methods against which ASAC is compared. It represents the simplest possible multi-agent approach and is consistently outperformed by ASAC, particularly in large-scale (high-user-count) scenarios.
**Easy way to remember:** IA2C agents are "lone wolves" — each learns on its own with no awareness of teammates.

---

## MADDPG (Multi-Agent Deep Deterministic Policy Gradient)
**What it is:** A multi-agent extension of the DDPG algorithm where each agent is trained with a centralized critic (that sees all agents' actions and states) but executes with a decentralized actor (using only local observations). Proposed by Lowe et al. (2017).
**In this paper:** MADDPG is one of the three baseline algorithms compared against ASAC. The neighbor action information used in ASAC's critic network is described as "inspired by MADDPG." ASAC outperforms MADDPG by improving both communication efficiency and learning speed through its spatial reward and context-awareness mechanisms.
**Easy way to remember:** MADDPG trains with a "God's-eye view" critic but deploys with a local-only actor — like practicing with full information but competing with only local knowledge.

---

## MAB (Multi-Armed Bandit)
**What it is:** A reinforcement learning approach where an agent repeatedly selects from multiple options ("arms") to maximize cumulative reward, without modeling state transitions. It is stateless compared to full RL.
**In this paper:** MAB is included as one of the baseline MARL methods. It is the best-performing baseline in the paper's experiments (reaching total reward of 4.23 in high-user-count networks), yet ASAC still achieves 4.89, a 16% improvement. The paper notes that ASAC's communication mechanism is more efficient than MAB's.
**Easy way to remember:** MAB is a simple "slot machine player" — it learns which lever to pull but doesn't track how the environment changes over time.

---

## Spatial Discount Factor (kappa)
**What it is:** A parameter (0 <= kappa <= 1) that controls how much weight an agent gives to rewards from neighboring agents at different distances. Rewards from agents at distance d are multiplied by kappa^d.
**In this paper:** Kappa is a key hyperparameter of the adaptive spatial reward mechanism. When kappa approaches 0, each agent acts greedily based on its own local reward; when kappa approaches 1, agents cooperate globally. The mechanism smoothly interpolates between these two extremes.
**Easy way to remember:** Kappa is a "distance filter" — closer neighbors' rewards count more, just like you care more about what's happening on your street than across the city.

---

## Time Discount Factor (delta)
**What it is:** A parameter (0 < delta <= 1) in reinforcement learning that determines how much future rewards are discounted relative to immediate rewards. A value near 1 makes the agent care about long-term outcomes; a value near 0 makes it short-sighted.
**In this paper:** Set to delta = 0.99 in experiments. Used in the long-term reward expression v_m(t) = sum_{tau=0}^{inf} delta^tau * r_m(t+tau) and in all Q-value and value function update equations.
**Easy way to remember:** Delta is the agent's "patience level" — close to 1 means it plans for the future; close to 0 means it only cares about right now.

---

## K-means Initialization
**What it is:** A clustering algorithm that groups data points into K clusters by iteratively assigning each point to its nearest cluster centroid and updating centroids until convergence.
**In this paper:** Used to initialize UAV positions at the start of each training round. UAVs are placed over the cluster centers of ground user locations (computed by K-means), ensuring that the initial UAV deployment is spread across the user distribution rather than randomly placed.
**Easy way to remember:** K-means initialization is like placing food trucks at the centers of busy neighborhoods before the lunch rush starts.

---

## Throughput (Network Utility)
**What it is:** The total amount of data successfully transmitted per unit time across the network, commonly measured in bits per second (bps) or as a log-based capacity expression using Shannon's formula.
**In this paper:** Network throughput for UAV m at time t is W/K * log(1 + gamma_m(t)), derived from the Shannon capacity formula, where W/K is the subchannel bandwidth and gamma_m(t) is the total SINR. The reward function uses throughput minus power cost as the per-step reward signal.
**Easy way to remember:** Throughput is the network's "delivery rate" — how much useful data gets through per second across all UAVs and users.

---

## 3GPP (3rd Generation Partnership Project)
**What it is:** An international standards body that develops technical specifications for mobile telecommunications systems, including 4G LTE and 5G NR.
**In this paper:** Mentioned in the introduction as the standardization framework that UAVs leverage (via 3GPP technology) for rapid deployment to enhance ground network coverage.
**Easy way to remember:** 3GPP writes the rulebook that all cellular equipment must follow — if a UAV uses cellular radio, it plays by 3GPP's rules.
