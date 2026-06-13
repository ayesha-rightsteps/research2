# Research Gaps: GNN + DRL for V2X Resource Allocation (2407.06518)

## Gaps the Authors Themselves Admit
- The approach still relies on periodic environment resets during training, which doesn't reflect continuous real-world deployment
- CSI noise is acknowledged as a problem in high-mobility environments, but the model doesn't explicitly model or quantify the degradation caused by imperfect CSI
- The graph construction assumes all V2V/V2I links are known — partial observability in real networks is not fully addressed
- Scalability beyond the tested vehicle density range is not evaluated
- No real-world or hardware-in-the-loop experiments — purely simulation-based

## Gaps the Authors Did NOT Mention (Your Analysis)
- **Static graph topology assumption per time slot:** GraphSAGE is applied to a graph that is reconstructed each time step, but the graph topology change dynamics (how fast links appear/disappear) are not modeled — in real fast-moving traffic, the graph structure changes faster than the inference cycle
- **No heterogeneous link types:** The model treats V2V and V2I as two fixed categories, but real V2X also includes V2P (pedestrian) and V2N (network) with different QoS requirements — this simplification limits generalizability
- **Fixed power levels only:** The paper uses discrete power levels; continuous power control (critical for interference management) is not explored
- **Single-road/static topology:** Experiments use a single simulated environment; intersection scenarios, highway on-ramps, and urban grid layouts with different interference patterns are not tested
- **No latency-aware scheduling:** V2V safety messages have strict latency budgets (e.g., 1ms for some NR-V2X use cases), but latency is not explicitly optimized — only success rate is used as proxy
- **No comparison with MARL approaches:** Only compared against single-agent DQN variants; modern MARL methods (QMIX, MAPPO) would be relevant baselines
- **GraphSAGE is static and not trained end-to-end with DRL:** The GNN is trained separately from the RL agent — joint end-to-end training could improve performance

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing GNN-DRL methods for V2X resource allocation assume fixed discrete power levels and static per-slot graph topology, causing performance degradation in high-mobility intersections where link topology changes faster than inference cycles — a continuous action space with dynamic graph update mechanisms is needed."

> **PS-2:** "Current V2X resource allocation methods do not explicitly enforce per-packet latency deadlines for safety-critical V2V messages, using success rate as a proxy — this fails under bursty traffic patterns where aggregate success rate is high but individual safety messages miss their deadlines."

> **PS-3:** "GNN-based resource allocation policies are trained on single-environment topologies and fail to generalize to unseen road geometries (intersections, merging lanes) without retraining — a topology-agnostic graph representation learning approach is needed."

## Most Promising Gap (Your Pick)
**Latency-deadline-aware resource allocation (PS-2)** is the most promising. The paper uses "V2V success rate" as the target metric, but for autonomous driving use cases, what actually matters is whether each safety packet arrives within its deadline (e.g., 1–10ms). A network can have high average success rate but still fail safety requirements if bursty interference causes tail-latency violations. This gap is practically critical, underaddressed in the literature, and directly extends the current paper's setup by replacing the reward function and adding a deadline-aware scheduler.
