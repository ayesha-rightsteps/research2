# Research Papers — Comparison Table
**Topic:** AI-Driven Resource Allocation in Next-Generation Wireless Networks (V2X / UAV / 6G)
**Papers Reviewed:** 9 | **Prepared by:** Ayesha

---

## Quick Legend
| Symbol | Meaning |
|---|---|
| ✓ | Addressed in this paper |
| ✗ | Not addressed |
| ◑ | Partially addressed |

---

## Main Comparison Table

| # | Paper (Short Title) | Technique (AI) | Technique (Classical) | UAV? | Multi-Agent? | Trajectory Opt.? | Energy Constraint? | Delayed / Imperfect CSI? | ISAC? | Graph-Based (GNN/GAT)? | Real-World Data? |
|---|---|---|---|---|---|---|---|---|---|---|---|
| P1 | GNN + DDQN for C-V2X | GraphSAGE + DDQN | — | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ (GraphSAGE) | ✗ |
| P2 | Lyapunov + D3PG, UAV Highway | D3PG (Diffusion RL) | Lyapunov Optimization | ✓ | ✗ | ◑ (altitude only) | ✓ | ✓ | ✗ | ✗ | ✓ (Xiamen, SUMO) |
| P3 | MARL Benchmark for C-V2X | MAPPO / IPPO / QMIX (benchmarked) | — | ✗ | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ |
| P4 | Multi-UAV IoV: SOCP + DRL + LLM | DRL + LLM (macro-scheduler) | SOCP (trajectory) + LP (offloading) | ✓ | ✓ (5 UAVs) | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ |
| P5 | Digital Twin + PSO + MADRL, Open-RAN 6G | MADRL (Enhanced DDPG) | PSO (trajectory) | ✓ | ✓ (3 UAVs) | ✓ | ✓ | ◑ (DT sync delay modeled, not robust) | ✗ | ✗ | ✗ |
| P6 | ASAC — Adaptive Spatial Reward, Multi-UAV | ASAC (MA2C + spatial reward) | K-means (init) | ✓ | ✓ (up to 7 UAVs) | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ |
| P7 | TN-NTN Multi-Connectivity, HCU/LCU | — (no AI) | Hungarian Method + Bisection | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ |
| P8 | GAT + A2C for C-V2X | GAT + A2C | — | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ (GAT) | ✗ |
| P9 | ISAC + BCD-SCA, UAV Vehicular | — (no AI) | BCD-SCA | ✓ | ✗ | ✓ | ✗ | ✗ | ✓ | ✗ | ✗ |

---

## Strength & Gap — Per Paper

| # | Core Strength | Main Gap |
|---|---|---|
| P1 | GNN gives each vehicle "global interference awareness" without any central controller — distributed but smart | No UAV, single agent only, discrete power levels, no MARL comparison |
| P2 | Only paper to explicitly handle delayed CSI + battery together; diffusion model handles uncertainty better than deterministic RL | Single UAV only, altitude-only trajectory (no 2D/3D path planning), single highway tested |
| P3 | First systematic MARL benchmark for V2X; proves MAPPO beats all others; open-source framework | No new algorithm proposed, highway topology only, no latency-deadline metric |
| P4 | Hierarchical decomposition: each sub-problem solved by its best tool (SOCP + DRL + LLM + LP) | LLM adds unquantified latency; no UAV collision avoidance; no energy/battery model |
| P5 | Digital Twin enables safe offline training before real deployment; Open-RAN architecture; PSO+MADRL clean decomposition | DT assumes perfect real-time telemetry — fails in disaster/congested scenarios; fixed UAV altitude; tested at tiny scale (3 UAVs only) |
| P6 | Novel spatial reward shaping lets agents care about WHERE they are, not just what they achieve — 16% improvement | UAV trajectories are pre-fixed (biggest contradiction); perfect CSI assumed; static users |
| P7 | Closed-form + Hungarian Method gives guaranteed-optimal one-to-one spectrum sharing; TN-NTN integration is novel | Zero AI/DRL; no trajectory optimization; only one HAP; energy constraint exists in formula but never enforced in simulation |
| P8 | GAT's attention weights prioritize neighbors that matter most — better interference capture than plain GNN | Assumes 100% V2X penetration (real-world is <5%); perfect CSI for attention weights; single urban grid only |
| P9 | First paper to jointly optimize resource allocation + trajectory + ISAC sensing in one framework; 32%+ gain over baselines | Single UAV only; BCD-SCA too slow for real-time deployment; no AI/DRL; perfect CSI assumed |

---

## What's Still Missing Across ALL 9 Papers

| Gap | Papers That Ignore It |
|---|---|
| Multi-UAV + ISAC together | All 9 |
| Delayed/imperfect CSI handled properly | P1, P3, P4, P5 (partial), P6, P7, P8, P9 |
| Joint trajectory + resource allocation + energy | P1, P3, P6, P8 fully; others partially |
| Real-world hardware / real deployment validation | All 9 (simulation only) |
| Low V2X penetration (< 10% connected vehicles) | All 9 assume 100% |
| Latency-deadline-aware scheduling (safety messages) | All 9 |
| Scalability beyond ~10 UAVs tested | P4 (5 UAVs), P5 (3 UAVs), P6 (7 UAVs) — none tested 20+ |

---

## One-Line Takeaway Per Paper (for quick recall)

| # | One Line |
|---|---|
| P1 | "Graph the interference, then let each car's AI use that graph to pick channel + power — no central controller needed." |
| P2 | "Lyapunov handles the battery, diffusion model handles the uncertainty of stale CSI — together they beat all classical RL." |
| P3 | "MAPPO is the best MARL algorithm for V2X — but none of them generalize well to new road topologies." |
| P4 | "For multi-UAV IoV, let SOCP fly the drones, DRL allocate resources, LLM strategize, and LP offload — hierarchy wins." |
| P5 | "Test everything in a digital twin first, then deploy — PSO flies the drones, MADRL handles the rest." |
| P6 | "Reward UAVs not just for what they achieve but for where they are spatially — 16% better than standard MARL." |
| P7 | "Hungarian Method pairs UAV users to spectrum optimally when ground + satellite networks are both available." |
| P8 | "GAT pays more attention to neighbors that interfere more — smarter graph learning than plain GNN for V2X." |
| P9 | "One UAV can sense AND communicate with the same signal (ISAC) — BCD-SCA jointly optimizes both, outperforming separate approaches by 32%+." |

---

*Table prepared from detailed analysis of 9 papers: summary.md, gaps.md, and explanation.md per paper.*
*For full paper-by-paper breakdown, see individual `paper-X/` folders.*
