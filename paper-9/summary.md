# Summary: Resource Allocation and Trajectory Planning in Integrated Sensing and Communication Enabled UAV-Assisted Vehicular Network

**Authors:** Mingyang Song, Wenyang Zhang, Jingpan Bai | **Year:** 2025 | **Published in:** Sensors (MDPI), Vol. 25, Article 7295

## What is this paper about?
This paper tackles a joint problem in UAV-assisted vehicular networks: how to simultaneously provide good wireless communication AND radar sensing to vehicles, when both functions share the same spectrum (ISAC — Integrated Sensing and Communication). UAVs and Ground Base Stations (GBSs) both have ISAC capability. The challenge is that allocating more spectrum to sensing reduces communication rate, and vice versa. The paper proposes an iterative BCD-SCA algorithm to jointly optimize UAV trajectory, vehicle association, and subchannel allocation while satisfying a minimum radar sensing quality constraint.

## The Core Idea
Use Block Coordinate Descent (BCD): divide the complex joint optimization problem into three simpler subproblems — vehicle association, UAV trajectory planning, and subchannel allocation — and solve them alternately while keeping the others fixed. Each subproblem is non-convex, so Successive Convex Approximation (SCA) is applied to locally approximate it as a convex problem that can be solved efficiently. Iterate until convergence. The key constraint: each vehicle must receive a minimum Mutual Information (MI) from radar sensing even as communication is being optimized.

## What they did (Method)
System: one UAV + one GBS, both with ISAC hardware, serving multiple vehicles in a traffic congestion scenario. Decision variables: UAV trajectory (positions over time), vehicle-to-base-station association, subchannel allocation. Objective: maximize average achievable communication rate subject to minimum radar MI per vehicle. BCD-SCA algorithm solves the three subproblems iteratively. Tested across varying UAV speeds (6–12 m/s), subchannel bandwidths (1–3 MHz), vehicle sensing MI requirements (200–1000 bits), and number of subchannels (10–50).

## Key Results
- Proposed BCD-SCA outperforms URA (Uniform Resource Allocation) by **32.42%** in average achievable rate
- Outperforms SAUA (Static UAV, no trajectory optimization) by **178.57%** — showing trajectory optimization is critical
- Outperforms SEA (Single Entity Allocation, only GBS) by **87.01%** — UAV presence is highly valuable
- Higher UAV flight speed → better performance (more flexibility to find optimal positions)
- Stricter sensing constraints reduce communication rate — fundamental ISAC trade-off confirmed
- Wider subchannel bandwidth → higher rates as expected

## One-line takeaway
> "Jointly optimizing UAV trajectory, vehicle association, and spectrum allocation under ISAC sensing constraints via BCD-SCA delivers up to 178% better communication rates than static UAV deployments — showing that trajectory optimization dominates performance in UAV-ISAC vehicular networks."
