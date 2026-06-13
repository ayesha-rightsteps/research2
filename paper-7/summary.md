# Summary: Resource Allocation and Sharing for UAV-Assisted Integrated TN-NTN with Multi-Connectivity

**Authors:** Abd Ullah Khan, Wali Ullah Khan, Haejoon Jung, Hyundong Shin | **Year:** 2026 (arXiv:2601.15532v2, submitted Jan 2026) | **Published in:** arXiv preprint (cs.NI), targeting IEEE publication

## What is this paper about?
This paper tackles the problem of how to fairly and efficiently share spectrum and power among UAVs (drones) that are simultaneously connected to both ground base stations (RBS) and high-altitude platforms (HAPs) — a setup called integrated TN-NTN (Terrestrial and Non-Terrestrial Networks). The UAVs are heterogeneous: some need high data capacity (HCUs) while others need reliable local drone-to-drone links (LCUs). The authors propose two optimization algorithms that jointly determine which UAV pairs share spectrum and how much power each transmits, accounting for fairness, QoS, and mobility-induced channel variation. Both algorithms are validated through simulation and compared against five baselines.

## The Core Idea
Instead of giving every UAV its own separate spectrum band (wasteful) or letting them share randomly (causes interference), this paper uses the Hungarian Method — a classic combinatorial assignment algorithm — to optimally pair high-capacity UAVs (HCUs) with local UAV-UAV pairs (LCUs) for spectrum reuse, while solving a closed-form power allocation problem for each pair. The key insight is that working with slow-varying large-scale channel statistics (path loss + shadowing) instead of instantaneous CSI makes the problem tractable even under high UAV mobility at 70 km/h.

## What they did (Method)
The authors model a 2 km x 2 km urban scenario with 20 HCUs and 20 LCUs flying at 70 km/h at 0.1 km altitude, one RBS at ground center, and one HAP at 17 km altitude. Each HCU uses multi-connectivity (simultaneous UAV-RBS and UAV-HAP links). The optimization is a two-step process: first, closed-form optimal power allocation is derived for every possible HCU-LCU spectrum-sharing pair using bisection search on a monotone reliability constraint function; second, the Hungarian Method finds the globally optimal one-to-one pairing assignment. Algorithm 1 maximizes sum capacity of HCUs subject to LCU reliability. Algorithm 2 maximizes the minimum (worst-case) HCU capacity for fairness, using a bisection-over-sorted-candidates approach combined again with the Hungarian Method. Simulations sweep J/I ratio, UAV speed, LCU outage probability, and LCU SINR threshold across at least 1000 channel realizations each.

## Key Results
- Algorithm 1 achieves the highest sum HCU capacity across all tested J/I ratios (0.2 to 1.0), substantially outperforming the greedy baseline (Baseline 2) and single-connectivity variants; sum capacity reaches approximately 950 bps/Hz at J/I=0.2 with 22 dBm power.
- Algorithm 2 achieves the highest minimum (fairness) capacity across all J/I values, maintaining approximately 10 bps/Hz minimum at J/I=0.2 versus near-zero for Baseline 2.
- Both proposed algorithms satisfy the LCU SINR reliability constraint of 5 dB at outage probability Po = 10^-3, while Baseline 2 fails to meet this threshold.
- As UAV speed increases from 60 to 80 km/h, sum capacity degrades from approximately 420 to 280 bps/Hz for Algorithm 1 (at 22 dBm), while baselines either start lower or degrade faster.
- Overall computational complexity is polynomial: O(JI log(1/epsilon) + I^3) for Algorithm 1 and O(JI log(1/epsilon) + JI log(JI) + I^3 log I) for Algorithm 2.

## One-line takeaway
> "By pairing UAVs optimally using the Hungarian Method and allocating power based only on large-scale channel statistics, this paper delivers the first framework that jointly achieves high total throughput and fair worst-case rate guarantees for heterogeneous multi-connected UAVs in integrated TN-NTN — something no prior work had done simultaneously."
