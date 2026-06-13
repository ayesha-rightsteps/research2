# Terminology: MARL Benchmarking for C-V2X Radio Resource Allocation

---

## ⭐ SIG (Stochastic Interference Game)
- **What it is**: A multi-agent game formulation where agents operate over multiple time steps, the channel gains change due to fast fading and vehicle mobility, and the goal is to deliver cooperative awareness messages (CAMs) reliably within each control interval while maximizing sum throughput.
- **In this paper**: The central benchmark task. SIG extends the simpler NFIG by adding time (50 communication steps per episode), fast fading, and diverse vehicular topologies. Variants: SIG SL (single location, for isolating fast fading and action space effects) and SIG ML (multiple locations, for isolating topology generalization).
- **Easy way to remember**: SIG is the realistic game — cars keep moving, channels keep changing, and the policy must handle all of it.

---

## ⭐ Topology Generalization (Robustness and Generalization)
- **What it is**: The ability of a learned policy to perform well across a wide variety of vehicular spatial configurations (topologies), including ones not seen during training. Topology determines path loss values and thus the entire interference structure of the network.
- **In this paper**: Identified as the dominant MARL challenge in C-V2X RRA — more impactful than non-stationarity, coordination, large action space, or partial observability. Performance degrades by 19–42% when moving from single-topology (SIG SL) to multi-topology (SIG ML) training, even for the best algorithms.
- **Easy way to remember**: A policy trained on a quiet suburban street fails on a congested motorway — topology generalization means making a policy that works on both.

---

## ⭐ IPPO (Independent Proximal Policy Optimization)
- **What it is**: A MARL algorithm where each agent independently applies the PPO algorithm (with its clipped surrogate objective for stable policy updates) without any information sharing. An IL, on-policy, actor-critic method.
- **In this paper**: The best-performing algorithm overall — achieves the highest or jointly highest normalized returns across SIG ML and POSIG tasks, and maintains near-optimal performance up to 16 agents. Recommended as the default baseline for C-V2X RRA due to its balance of performance and scalability.
- **Easy way to remember**: PPO for every car, no team meetings — simple, fast, and surprisingly effective.

---

## ⭐ MAPPO (Multi-Agent Proximal Policy Optimization)
- **What it is**: The CTDE extension of IPPO, where a centralized critic has access to the global state during training. Each agent still executes its own policy using local observations at deployment.
- **In this paper**: The actor-critic CTDE algorithm. Performs on par with IPPO in most tasks; provides a consistent advantage over IA2C under partial observability (POSIG). The best-performing algorithm in the hardest NFIG topology (500 mid). Outperforms the best value-based algorithm (QMIX) by 42% in the most complex task.
- **Easy way to remember**: Like IPPO but the cars get a shared map during driver training — at race time, they still drive alone.

---

## ⭐ MARL (Multi-Agent Reinforcement Learning)
- **What it is**: A framework extending RL to environments where multiple agents act simultaneously. Each agent observes (part of) the environment state, takes an action, and receives a reward. Because all agents learn simultaneously, the environment is non-stationary from any single agent's perspective.
- **In this paper**: The overarching framework for all eight algorithms evaluated. MARL is necessary because C-V2X RRA is inherently a multi-vehicle decision problem: every vehicle's channel selection affects every other vehicle's interference level.
- **Easy way to remember**: RL where multiple students are all learning at the same time in the same classroom — each one's behavior changes the environment for the others.

---

## CTDE (Centralized Training with Decentralized Execution)
- **What it is**: A MARL learning paradigm. During training, agents have access to global information (all agents' states, observations, or actions). During deployment, each agent acts using only its local observation.
- **In this paper**: One of the two main learning frameworks evaluated (the other is IL). CTDE algorithms include VDN, QMIX (value-based) and MAA2C, MAPPO (actor-critic). Findings: CTDE offers modest benefits over IL for actor-critic methods, but the gap is small in fully observable settings (SIG SL) and actually reverses for value-based methods at large scale in POSIG.
- **Easy way to remember**: Study together, test alone — CTDE uses teamwork to build better individual agents.

---

## IL (Independent Learning)
- **What it is**: A MARL learning paradigm where each agent trains using single-agent RL, treating other agents as part of the stationary environment. No explicit coordination mechanism is used.
- **In this paper**: The other main framework. IL algorithms include IDQN, Hys-IDQN (value-based) and IA2C, IPPO (actor-critic). Despite its simplicity, IPPO (IL) matches or outperforms CTDE methods on most tasks, suggesting that the benefits of centralized training are limited in this setting.
- **Easy way to remember**: Every student studies alone — no group work, but some students (IPPO) do just as well.

---

## IDQN (Independent Deep Q-Network)
- **What it is**: Each agent uses the standard DQN algorithm independently. Each maintains its own Q-network and target network, trained off-policy from its own replay buffer. IL, value-based, off-policy.
- **In this paper**: The most common approach in prior C-V2X literature and the simplest baseline here. Performs well in single-topology tasks (NFIG, SIG SL) but degrades severely in multi-topology tasks (SIG ML), reaching near-zero or negative returns at 16 agents.
- **Easy way to remember**: The classic approach — give every car its own Q-table and hope for the best.

---

## Hys-IDQN (Hysteretic IDQN)
- **What it is**: A variant of IDQN that uses two different learning rates: a smaller rate for negative TD errors (bad outcomes) and a larger rate for positive TD errors (good outcomes). Designed to reduce the impact of other agents' suboptimal exploratory actions on a given agent's learning.
- **In this paper**: Shows modest improvement over IDQN in some SIG SL settings, but the gains are small and do not carry over to multi-topology tasks.
- **Easy way to remember**: An IDQN that learns more from successes than failures — glass-half-full learning.

---

## VDN (Value Decomposition Networks)
- **What it is**: A CTDE, value-based MARL algorithm where the joint Q-function is the sum of individual per-agent utility functions. This additivity ensures the IGM (Individual-Global-Max) condition is satisfied: maximizing each agent's individual utility also maximizes the joint Q-value.
- **In this paper**: Outperforms IDQN in multi-step and multi-topology tasks (SIG SL, SIG ML 4-agent). At 16 agents in SIG ML, VDN collapses to near-zero returns. In POSIG, its advantage over IL methods disappears at larger scales because the IGM assumption is hard to satisfy with local observations.
- **Easy way to remember**: VDN says "the team score is just the sum of everyone's individual scores" — simple, clean, but not always true.

---

## QMIX
- **What it is**: A CTDE, value-based MARL algorithm where the joint Q-function is a monotonic (non-linear) function of individual utility functions, implemented via a mixing network. More expressive than VDN's additive constraint but still satisfies the IGM principle under monotonicity.
- **In this paper**: Performs similarly to VDN in most tasks. At 16 agents in SIG ML, QMIX scores 0.00 (random-policy level). In POSIG 4-agent tasks, QMIX (0.77) underperforms VDN (0.78). The mixing network's benefits do not consistently materialize in this setting.
- **Easy way to remember**: VDN with a smarter team scorecard — still based on individual contributions but allows for non-linear synergies.

---

## IA2C (Independent Advantage Actor-Critic)
- **What it is**: Each agent independently applies the synchronous A2C algorithm. The actor network outputs a policy (action probabilities); the critic estimates the value function. On-policy updates from collected rollouts.
- **In this paper**: IL, on-policy, actor-critic. Performs comparably to IA2C's CTDE counterpart (MAA2C) in fully observable tasks. Outperforms value-based IL at larger agent counts due to on-policy nature.
- **Easy way to remember**: Each car has its own driver training school, using the advantage function as a progress report.

---

## MAA2C (Multi-Agent Advantage Actor-Critic)
- **What it is**: The CTDE version of IA2C, where the critic has access to the global state (all agents' observations and actions) during training. Actors still use only local observations at execution.
- **In this paper**: Consistently outperforms IA2C under partial observability (POSIG). In fully observable settings, offers little advantage because global state is already available to all agents.
- **Easy way to remember**: A2C but the driving instructor can see the entire road network during training, not just one student's view.

---

## NFIG (Normal-Form Interference Game)
- **What it is**: The simplest game in the benchmark hierarchy. A single time-step game (no sequential decision-making) with a fixed vehicular topology and no fast fading. Reduces C-V2X RRA to a matrix game where agents must coordinate on a joint action.
- **In this paper**: Designed to isolate coordination difficulty and non-stationarity. Results show that nearly all algorithms achieve near-optimal performance (0.97–1.00 normalized return), revealing that these challenges are not problematic in simplified settings.
- **Easy way to remember**: A one-shot game — cars must coordinate their channel choices in a single moment, like a traffic light intersection with no memory.

---

## POSIG (Partial Observable Stochastic Interference Game)
- **What it is**: The most complex game in the hierarchy. Extends SIG by restricting each agent to a local observation: its own V2V channel gains, the interference power it received in the previous interval, and its own queue length. Global channel information is not available.
- **In this paper**: Surprisingly, POSIG outperforms SIG ML across most algorithms and all scales. The authors attribute this to the fact that local observations are lower-dimensional and contain less irrelevant information than the high-dimensional global state used in SIG ML. Partial observability itself is not the bottleneck.
- **Easy way to remember**: Each car only sees out its own windows — but it turns out the smaller, focused view is often enough.

---

## Non-Stationarity
- **What it is**: A fundamental challenge in MARL: because all agents learn simultaneously, the environment transitions each agent experiences change over time as other agents' policies evolve. This violates the stationarity assumption required for convergence of single-agent RL algorithms.
- **In this paper**: One of the five MARL challenges explicitly isolated. Results show that non-stationarity is not a dominant challenge in any of the interference games studied — algorithms perform well even without specific mechanisms to address it.
- **Easy way to remember**: Playing chess against an opponent who keeps getting better while you are also learning — the rules are fixed but the difficulty keeps changing.

---

## Partial Observability
- **What it is**: Each agent only receives information about its own local environment, not the full global state. This is realistic because vehicles cannot directly observe other vehicles' channel states or queue lengths.
- **In this paper**: Isolated by comparing SIG ML (global state) vs. POSIG (local observations). Partial observability is found to be not a critical limitation; the reduction in state dimensionality in POSIG actually helps algorithms generalize better to diverse topologies.
- **Easy way to remember**: You can only see your own dashboard, not everyone else's — but your dashboard actually tells you enough.

---

## Coordination Difficulty
- **What it is**: The challenge that arises when an agent's optimal action depends on the actions of other agents at the same time step, and multiple Nash equilibria exist — some optimal, some suboptimal. Agents may converge to a suboptimal equilibrium.
- **In this paper**: Isolated in the NFIG task. The paper introduces a Coordination Difficulty Score (CDS) computed from the statistics of Nash equilibria for each vehicular topology. High-CDS topologies (e.g., 500 mid with CDS=0.368) are harder to learn, but even here most algorithms approach the optimum.
- **Easy way to remember**: Choosing which side of the road to drive on — if everyone picks the same side, it works; if they pick different sides, it fails. The game structure determines how hard it is to coordinate.

---

## Relative Overgeneralization
- **What it is**: A coordination pathology where an agent learns to prefer a "safe" but suboptimal action that performs reasonably regardless of what other agents do, rather than committing to an optimal action that only works if others cooperate.
- **In this paper**: One of two coordination pathologies identified in the NFIG setting. It leads the learned joint policy to converge to a suboptimal Nash equilibrium.
- **Easy way to remember**: Playing it safe in a team sport — a striker always passes instead of shooting because shots only go in when teammates set up the right play.

---

## Miscoordination
- **What it is**: A coordination failure where multiple optimal Nash equilibria exist and different agents select actions corresponding to different equilibria, resulting in poor joint performance.
- **In this paper**: The second coordination pathology in NFIG. Topologies with multiple heterogeneous Nash equilibria (high CDS) produce more miscoordination and lower average performance.
- **Easy way to remember**: Two people both trying to step aside to let each other through a doorway — each individually rational, but both blocking each other.

---

## Value Decomposition
- **What it is**: A CTDE technique for value-based MARL that factorizes the joint Q-function into per-agent utility functions. The IGM (Individual-Global-Max) condition ensures that greedy local action selection is globally optimal.
- **In this paper**: Used by VDN (additive factorization) and QMIX (monotonic factorization). Benefits over IL are clearer in fully observable multi-step settings; benefits diminish at large scale with partial observability because the IGM condition is harder to satisfy.
- **Easy way to remember**: The team Q-function is assembled from individual report cards — you can pick the best team strategy by having each player maximize their own score.

---

## IGM (Individual-Global-Max) Condition
- **What it is**: A theoretical condition in value decomposition MARL stating that the joint action maximizing the global Q-function must equal the set of individual actions maximizing each agent's individual utility function.
- **In this paper**: The theoretical justification for VDN and QMIX. The paper notes that both additivity (VDN) and monotonicity (QMIX) are sufficient but not necessary for IGM, and that when local observations are used in POSIG, satisfying IGM becomes harder because individual utility functions are conditioned on partial state.
- **Easy way to remember**: The team wins when every player's personal best strategy also happens to be the best team strategy — achieving this alignment is the hard part.

---

## Actor-Critic Method
- **What it is**: A class of RL algorithms that maintains two networks: an actor (the policy, mapping states/observations to actions) and a critic (the value function, estimating how good a state or state-action pair is). The critic's value estimates guide the actor's policy updates.
- **In this paper**: Includes IA2C, MAA2C, IPPO, MAPPO. Actor-critic methods are on-policy, meaning they update from data collected under the current policy, which naturally adapts to the evolving joint policy in multi-agent settings. This is why they outperform value-based methods in complex, multi-topology tasks.
- **Easy way to remember**: The actor performs on stage; the critic in the audience gives real-time feedback on how to improve. Together, they produce a better performance than either alone.

---

## On-Policy vs. Off-Policy
- **What it is**: In on-policy RL, the agent learns from data generated by the current policy. In off-policy RL, the agent can learn from data generated by any policy, stored in a replay buffer. On-policy is more sample-inefficient but avoids stale data; off-policy is more sample-efficient but can diverge on stale samples.
- **In this paper**: Off-policy: IDQN, Hys-IDQN, VDN, QMIX. On-policy: IA2C, MAA2C, IPPO, MAPPO. As agent count increases, off-policy methods suffer more because their replay buffers contain experience from earlier, different joint policies, while on-policy methods always train on current behavior.
- **Easy way to remember**: On-policy: only learn from what you just did. Off-policy: learn from anything in your diary, including old entries.

---

## C-V2X (Cellular Vehicle-to-Everything)
- **What it is**: A 3GPP-standardized wireless technology that uses cellular infrastructure (LTE/5G) to enable vehicle communications. V2I links connect vehicles to base stations; V2V links connect vehicles directly.
- **In this paper**: The target application domain. C-V2X RRA is the problem of assigning subchannels and power levels to V2V links that reuse the uplink subchannels of V2I links.
- **Easy way to remember**: Cell phone technology adapted for cars — the car's radio talks to the cell tower and other cars using the same cellular spectrum standards.

---

## RRA (Radio Resource Allocation)
- **What it is**: The assignment of communication resources — subchannels (frequency) and transmit power — to users or links in a wireless network to maximize performance (throughput, reliability) while managing interference.
- **In this paper**: The core optimization problem. Each V2V link agent must decide which subchannel to reuse (from V2I subchannels) and at what power level, every 1 ms.
- **Easy way to remember**: Dividing lanes of a highway among cars — give too many to one group and others get stuck; give too few and lanes are wasted.

---

## Normalized Return
- **What it is**: A standardized performance metric defined as (achieved return − random policy return) / (approximate optimal return − random policy return). A value of 1.0 means optimal performance; 0.0 means no better than random.
- **In this paper**: The primary evaluation metric, enabling fair comparison across tasks with different absolute reward scales. Gmin is the return of a uniform random policy; Gmax is obtained by exhaustive search (4-agent) or greedy algorithms (larger settings).
- **Easy way to remember**: A score from 0 to 1 — 0 means you played randomly, 1 means you played perfectly.

---

## Zero-Shot Transfer
- **What it is**: The ability of a policy trained on one set of conditions to perform well on entirely new conditions (unseen during training) without any additional training or fine-tuning.
- **In this paper**: A key desideratum identified in the conclusion. Ideally, a C-V2X policy trained on some vehicular topologies should automatically handle new topologies encountered at deployment time. Results show current algorithms fail at this: performance on unseen topologies is similar to (IPPO) or significantly worse than (IDQN) performance on seen topologies.
- **Easy way to remember**: A doctor who learned from patients in one city should still be able to treat patients in another city without going back to medical school.

---

## CAM (Cooperative Awareness Message)
- **What it is**: A standardized V2V safety message defined by ETSI that vehicles broadcast periodically (every 100 ms) to inform nearby vehicles of their position, speed, and heading. Successful delivery is safety-critical.
- **In this paper**: The payload of V2V communications. Each control interval generates one CAM of size 6 × 1,060 bytes per V2V link. The task objective is to deliver this CAM within the 100 ms control interval.
- **Easy way to remember**: The "I'm here, watch out for me" message that cars send to each other 10 times per second.

---

## SUMO (Simulation of Urban MObility)
- **What it is**: An open-source microscopic traffic simulation platform that models individual vehicle movements on real or synthetic road networks, supporting realistic speed, density, and lane-change behaviors.
- **In this paper**: Used to generate diverse vehicle position datasets for training and testing. Vehicle positions are sampled every 100 ms on a simulated 2 km six-lane highway under three ETSI-specified speed/density scenarios (35 veh/km at 250 km/h, 123 veh/km at 70 km/h, 500 veh/km at 50 km/h).
- **Easy way to remember**: A video game traffic simulator that produces realistic car movement data for wireless network researchers.

---

## PPO (Proximal Policy Optimization)
- **What it is**: An on-policy policy gradient RL algorithm that constrains how much the policy can change in a single update step using a clipped surrogate objective. This prevents large, destabilizing policy updates.
- **In this paper**: The single-agent base algorithm for both IPPO and MAPPO. PPO consistently outperforms A2C (its less constrained counterpart) across tasks, especially at 16 agents, due to its clipped objective's stabilizing effect.
- **Easy way to remember**: Policy gradient with a speed limiter — you can still improve, but not so fast that you veer off the road.

---

## A2C (Advantage Actor-Critic)
- **What it is**: An on-policy actor-critic algorithm that uses the advantage function (Q-value minus state value) to reduce variance in policy gradient estimates. The synchronous version (A2C) updates all agents together after collecting a fixed batch of experience.
- **In this paper**: The single-agent base algorithm for IA2C and MAA2C. Performs well in simplified tasks but is generally outperformed by PPO-based methods in complex multi-topology tasks because its updates are less stable.
- **Easy way to remember**: Actor-critic with a "how much better than average?" signal guiding the actor's learning.

---

## CDS (Coordination Difficulty Score)
- **What it is**: A metric introduced in this paper to quantify how hard it is for agents to coordinate on the optimal Nash equilibrium in a given NFIG topology. Computed from the spread and number of Nash equilibria: CDS combines the gap between the best and worst equilibria with the gap between the best and average equilibria.
- **In this paper**: Correlates negatively with normalized return (Pearson r = −0.696, p = 0.037) across NFIG topologies — high-CDS topologies like 500 mid are harder to learn. Demonstrates that the topology-induced game structure, not the learning algorithm, primarily determines coordination difficulty.
- **Easy way to remember**: A difficulty rating for each road layout — some crossings are easy to coordinate, others are chaos.

---

## Large Action Space
- **What it is**: As the number of V2V agents grows, the joint action space grows exponentially (each agent has a subchannel × power combination). This makes exploration and credit assignment harder.
- **In this paper**: Isolated by comparing 4-, 8-, and 16-agent SIG SL FF settings. Value-based IL methods (IDQN) degrade significantly at 16 agents (0.84); PPO-based methods maintain near-optimal performance (IPPO/MAPPO: 1.00). Found to be a secondary challenge compared to topology generalization.
- **Easy way to remember**: Imagine having to pick one combination from 1,000 options instead of 10 — the number of wrong choices grows faster than the agent can learn.
