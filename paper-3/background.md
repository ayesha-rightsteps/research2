# Background: MARL Benchmarking for C-V2X Radio Resource Allocation

## Field

This paper belongs to the field of multi-agent reinforcement learning (MARL) applied to wireless communications — specifically to radio resource allocation (RRA) in cellular vehicle-to-everything (C-V2X) networks. Its primary contribution is methodological rather than algorithmic: it provides a systematic benchmarking framework that isolates and measures the individual impact of each known MARL challenge on C-V2X performance, and it identifies the dominant challenge that the field has largely underestimated.

## Evolution of MARL in V2X: A Narrative of the Field

**The RRA problem in C-V2X.** In a C-V2X network, vehicles need to share a limited pool of uplink subchannels. V2I links (vehicle-to-base-station) are pre-assigned dedicated subchannels. V2V links (vehicle-to-vehicle, used for cooperative awareness messages) must dynamically select which subchannels to reuse and at what power level, balancing their own throughput against the interference they cause to V2I links and other V2V links. This joint subchannel-and-power allocation problem, repeated every millisecond across many simultaneous vehicles, is intractable with classical optimization methods.

**Single-agent DRL as the first wave.** Around 2018, researchers began applying deep Q-networks (DQN) to C-V2X RRA, treating each vehicle as an independent learner. These early studies showed clear performance gains over heuristic baselines, but they treated other vehicles as part of an unchanging "environment" — ignoring that all agents are learning simultaneously. This approach, called Independent Learning (IL), is conceptually simple and scales easily, but it sidesteps the fundamental multi-agent nature of the problem.

**MARL as the next step.** As the field matured, researchers introduced proper MARL frameworks. Two paradigms emerged: Independent Learning (IL), where each agent trains with its own local data as if alone, and Centralized Training with Decentralized Execution (CTDE), where agents share global information during training but act locally during deployment. CTDE methods such as QMIX (value decomposition) and MAPPO (centralized critic) were proposed to handle multi-agent coordination, non-stationarity (other agents' policies change during training, making the environment non-stationary from any single agent's perspective), partial observability (vehicles cannot see every other vehicle's channel state), and large joint action spaces (the space of all possible joint subchannel-power combinations grows exponentially with the number of agents).

**The benchmark gap.** Despite substantial algorithmic progress, the field lacked a principled way to compare methods. Different papers used different scenarios (urban vs. highway), different numbers of vehicles, different payload sizes, and different baselines. Findings from one paper could not be reliably compared to findings from another. More critically, papers would propose solutions to specific challenges (non-stationarity, partial observability, coordination) without establishing which challenge actually mattered most in a realistic vehicular setting. The academic community had no agreed-upon benchmark — equivalent to the role that Atari games or MuJoCo physics tasks played in advancing single-agent DRL.

**This paper's contribution.** The authors build that missing benchmark. They formulate C-V2X RRA as a hierarchy of multi-agent interference games (NFIG → SIG → POSIG) that progressively add realism and thus complexity. Each game is designed to isolate one or more MARL challenges. They generate large-scale, diverse vehicle position datasets using SUMO under standardized 3GPP/ETSI highway scenarios, and they benchmark eight classical MARL algorithms (IDQN, Hys-IDQN, VDN, QMIX, IA2C, MAA2C, IPPO, MAPPO) across all games. The result is a reproducible, open-source foundation — code, datasets, and the interference-game suite are all released publicly.

## Key Technologies

- **MARL (Multi-Agent Reinforcement Learning)**: extends RL to environments where multiple agents act simultaneously, each affecting the others' rewards.
- **CTDE (Centralized Training with Decentralized Execution)**: agents share global information during offline training but use only local observations when deployed.
- **Independent Learning (IL)**: each agent trains independently using single-agent RL, treating other agents as part of the environment.
- **Value decomposition (VDN, QMIX)**: CTDE approach where a joint Q-function is factored into per-agent utility functions, enabling coordinated action selection at execution time.
- **Actor-critic methods (A2C, PPO)**: policy gradient RL algorithms that pair a policy network (actor) with a value estimator (critic); on-policy updates naturally track the current joint policy, which is an advantage in multi-agent settings.
- **SUMO**: open-source traffic simulator used to generate realistic, diverse vehicle positional datasets for training and testing.

## Why This Paper's Approach is Different

Most C-V2X MARL papers are algorithm papers: they propose a new method and show it beats one or two baselines on one scenario. This paper makes no algorithmic contribution — its contribution is the benchmark itself. By systematically controlling which challenges are present in each experimental condition, the authors can ask and answer a question no prior work addressed cleanly: among non-stationarity, coordination difficulty, large action space, partial observability, and topology generalization, which one actually limits performance in realistic C-V2X settings? The surprising answer — topology generalization dominates — is only visible when the experimental design is constructed to isolate it. Furthermore, the open-source release ensures that future algorithm proposals can be evaluated on a common, reproducible platform.

## Where It Sits in the Bigger Picture

This paper occupies the "infrastructure" layer of the research ecosystem: it provides the controlled experiments and common ground that future algorithm work can build on. The finding that topology generalization (learning policies that work across diverse vehicular interference patterns, including ones not seen during training) is the dominant challenge redirects attention toward meta-learning, domain randomization, and graph-based state representations as promising directions. The paper also establishes a practical recommendation: IPPO (Independent PPO) should be the default baseline for C-V2X RRA due to its superior combination of performance, scalability, and implementation simplicity relative to more complex CTDE methods.
