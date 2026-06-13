# Background: A Multi-Agent Resource Allocation Method for Unmanned Aerial Vehicle Networks based on Adaptive Spatial Reward Mechanisms and Context-Awareness

## What is this paper about? (so you know what to prep for)
This paper is about teaching a fleet of UAVs (acting as flying base stations) to decide — on their own, with no central controller — which ground user to serve, which sub-channel to use, and how much power to transmit at, using a multi-agent reinforcement learning method called ASAC. The two big ideas are "care more about your nearby neighbors than distant ones" (spatial reward) and "share your current state with neighbors before deciding" (context-awareness).

## What You Need to Know Before You Start

### Reinforcement Learning Basics (states, actions, rewards, Q-values)
At its core, RL is: an agent looks at its current "state," picks an "action," gets a "reward," and over time learns which actions in which states lead to the best long-term reward. The "Q-value" Q(s,a) is the agent's running estimate of how good it is to take action a in state s. The paper assumes you already know this loop — it jumps straight into UAV-specific states and actions without re-explaining the RL framework itself.

### Markov Decision Process (MDP)
The paper formalizes the resource allocation problem as an MDP — a rulebook of states, actions, transition dynamics, and rewards. If you've seen an MDP defined before (tuple of S, A, P, R, discount factor), you're set. The paper writes this down quickly in Section 2/3 and moves on; it won't teach you what an MDP is from scratch.

### Multi-Agent Reinforcement Learning (MARL)
This is the step up from single-agent RL: instead of one agent, you have multiple agents (here, multiple UAVs) learning simultaneously in a shared environment, each getting its own reward but affecting each other through interference and shared resources. Key tension to keep in mind: should an agent be "independent" (ignore others) or "cooperative" (consider others)? ASAC sits in between. The paper assumes you already know why naive independent learning (each agent ignoring everyone else) becomes unstable as the number of agents grows.

### Actor-Critic Methods (A2C / Advantage Actor-Critic)
An Actor-Critic agent has two neural networks: the "actor" picks actions (the policy), and the "critic" estimates how good a state/action is (the value function), and feeds that signal back to improve the actor. The "advantage" A = Q - V tells the actor how much better an action was than average. The paper builds its entire method (MA2C — Multi-Agent A2C) on top of this, and assumes you know what actor and critic networks are and how they're trained together — it does not re-derive Actor-Critic from scratch.

### Deep Q-Networks (DQN) and Epsilon-Greedy Exploration
DQN is just a neural network used to approximate Q-values when the state/action space is too large for a lookup table. Epsilon-greedy is the standard trick for exploration: with probability epsilon, pick a random action (explore); otherwise pick the best-known action (exploit). The paper tunes epsilon (finding 0.5 optimal) and assumes you already know what "exploration vs. exploitation" means and why both extremes (epsilon=0 or epsilon=1) are bad.

### Reward Shaping / Discounting (spatial and temporal)
Two different "discounting" ideas show up and it's easy to confuse them:
- **Temporal discounting (delta)** — the classic RL idea that rewards further in the future matter less (delta=0.99 here, meaning the agent is patient).
- **Spatial discounting (kappa)** — this paper's own twist, where rewards from OTHER AGENTS matter less the farther away (in physical distance) those agents are. This is new vocabulary the paper introduces, but the underlying intuition (discount factor as "weight that shrinks with distance/time") is something you should already be comfortable with before reading.

### UAV Networks as Aerial Base Stations
The basic setup: UAVs fly at a fixed altitude and act like mobile cell towers, providing wireless downlink coverage to ground users. The paper assumes you know that UAVs are attractive for this because they're quick to deploy (disaster relief, temporary events, military) and tend to have Line-of-Sight (LoS) — i.e., a relatively clean, unobstructed signal path — to ground users compared to terrestrial base stations.

### Wireless Communication Fundamentals (SINR, sub-channels, throughput, QoS)
You'll see these on page 1 without introduction:
- **Sub-channels**: the total spectrum is sliced into K frequency bands; each UAV picks one per time slot, and two UAVs on the same sub-channel interfere with each other.
- **SINR (Signal-to-Interference-plus-Noise Ratio)**: a single number capturing how "clean" a link is — higher is better.
- **Throughput** via Shannon's formula: throughput = bandwidth * log(1 + SINR). The paper uses this directly to compute rewards.
- **QoS threshold**: a minimum SINR the system must hit; fall below it and the reward for that step becomes zero.

### LSTM (Long Short-Term Memory networks)
A type of recurrent neural network that maintains a "memory" across time steps, useful for sequences. The paper uses a small (64-neuron) LSTM as the core of its "context-aware communication mechanism" — each UAV encodes its state into this LSTM and shares the resulting hidden state with neighbors. If you've never seen an LSTM before, at minimum know that it takes a sequence of inputs and produces a "hidden state" that summarizes relevant history — the paper won't explain LSTM mechanics itself.

### K-means Clustering (brief)
Used only for initialization — UAVs are placed at the cluster centers of where ground users are located at the start of each training round. You just need to know K-means groups points into K clusters around centroids; the paper doesn't elaborate further.

## Prior Methods/Papers This One Compares Against or Builds On
ASAC is benchmarked against three baseline MARL algorithms — knowing roughly what each one is will help a lot when the results tables come up:
- **IA2C (Independent A2C)**: each UAV runs its own A2C completely independently, with zero communication between agents — the "lone wolf" baseline.
- **MADDPG (Multi-Agent Deep Deterministic Policy Gradient, Lowe et al. 2017)**: agents train with a "centralized critic" that sees everyone's state/actions, but act using only local info at deployment. ASAC's idea of letting the critic see neighbor actions is explicitly inspired by MADDPG.
- **MAB (Multi-Armed Bandit)**: a simpler, stateless approach — picks actions to maximize reward without modeling how the state evolves over time. This is actually the strongest baseline in the paper's results.

Conceptually, ASAC also builds on the channel modeling conventions from earlier UAV communication work (probabilistic LoS/NLoS path-loss models and deterministic LoS free-space path loss models) — these are treated as "off-the-shelf" physics and not derived from scratch.

## The Setup You'll See on Page 1
Picture a 500-meter-radius circular area on the ground, with M UAVs hovering at a fixed altitude of 100m above it, each acting as its own little base station. Scattered below are L ground users (up to 200 in the larger experiments) who need wireless connectivity. The total available spectrum is split into K sub-channels. At every time step, each UAV independently decides three things: (1) which ground user to serve, (2) which sub-channel to transmit on, and (3) what power level to use. UAVs don't talk to a central controller — instead, nearby UAVs exchange small bits of encoded state information with each other (the "context-aware communication" piece) and each UAV's learning signal is shaped by how close other UAVs are (the "spatial reward" piece). The goal: maximize total network throughput while meeting a minimum QoS (SINR) requirement, learned entirely through MARL.

## Ready-to-Read Check
- [ ] I understand the basic RL loop (state, action, reward, Q-value) and what an MDP is
- [ ] I understand Actor-Critic architecture (actor = policy, critic = value estimator, advantage = Q - V)
- [ ] I understand the difference between temporal discounting (delta, for future rewards) and what "discounting by distance" might mean for multiple agents
- [ ] I know what SINR, sub-channels, and throughput mean in a wireless network
- [ ] I have a rough sense of what an LSTM does (takes a sequence, produces a memory/hidden state)
- [ ] I know why independent learning (each UAV ignoring its neighbors) becomes unstable as the network gets denser — this is the core problem ASAC is trying to solve
(If something's unchecked, skim `terminology.md` for that term first — then come back.)
