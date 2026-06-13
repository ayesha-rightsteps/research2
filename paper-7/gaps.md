# Research Gaps: Resource Allocation and Sharing for UAV-Assisted Integrated TN-NTN with Multi-Connectivity

## Gaps the Authors Themselves Admit
- The algorithms exploit only slow-varying large-scale channel parameters (path loss + shadowing), which limits the ability to adapt to instantaneous small-scale fading and thus leaves potential short-term gains unexploited.
- Spectrum sharing is restricted to a strict one-to-one HCU-LCU pairing. When the number of LCUs exceeds the number of HCUs (J > I), spectrum resources are under-utilized because unpaired LCUs get no spectrum.
- Independent power allocation across UAV-RBS, UAV-HAP, and UAV-UAV links is not considered — all three link types are not jointly power-optimized together.
- Simplified straight-line UAV trajectory is assumed; real mission trajectories (curved, waypoint-based, reactive to obstacle or weather) are not modeled.
- MC is only considered with respect to one RBS and one HAP; multi-RBS and multi-satellite MC scenarios are not addressed.
- Future work is flagged for: more complex interference scenarios, many-to-one spectrum sharing, and extending to delay-Doppler domain modeling (OTFS).

## Gaps the Authors Did NOT Mention (Your Analysis)

- **Single HAP assumption is unrealistic at scale.** The entire system has exactly one HAP at 17 km and one RBS. In a real urban 6G deployment, there would be multiple RBSs with overlapping coverage and potentially multiple HAPs or LEO satellites. The interference model and Hungarian Method pairing both collapse when there are more infrastructure nodes — the current one-to-one pairing logic breaks entirely.

- **No trajectory optimization — mobility is treated as a noise source only.** UAV trajectories are hardcoded as straight lines with speed swept as a parameter. The paper does not co-optimize trajectory and resource allocation. In reality, choosing where a UAV flies affects its channel gain to both RBS and HAP simultaneously — this is a major missed joint optimization opportunity that papers like [23] and [25] show matters significantly.

- **Statistical CSI assumption breaks down for slow UAVs or hovering scenarios.** The entire mathematical framework relies on Rayleigh fast-fading being averaged out over ergodic time-slots. But UAVs hovering or moving very slowly (< 5 km/h) for surveillance or delivery tasks exhibit quasi-static channels where instantaneous CSI is feasible — the paper's statistical-only approach is then sub-optimal or even incorrect.

- **Simulation scale is tiny and not validated against real measurements.** The experiment uses only 20 HCUs and 20 LCUs in a 2 km x 2 km square. The paper claims polynomial scalability but never actually tests with 100+ UAVs. Real 6G urban deployments are expected to have hundreds to thousands of UAVs. The O(I^3) Hungarian Method term becomes a bottleneck at that scale (e.g., 500 UAVs → 1.25 x 10^8 operations per assignment round).

- **No energy/battery constraint is actually enforced.** Constraint (12h) introduces a battery energy limit E_x^max, but the simulation section never presents results showing how battery depletion affects performance, nor does any algorithm explicitly adapt power allocation as battery drains over time. The energy constraint appears in the formulation but is analytically ignored.

- **Missing baseline: joint trajectory + resource allocation.** All baselines either use fixed trajectories or are purely spectrum/power methods. A key missing comparison is against DRL-based joint trajectory and resource allocation (e.g., similar to [34] in the paper's own references), which would challenge whether the proposed closed-form approach actually beats learned joint policies.

- **No heterogeneous QoS in the fairness algorithm.** Algorithm 2 maximizes the minimum capacity identically for all HCUs, but in practice different HCU types (e.g., medical drone vs. cargo drone) may have different minimum rate requirements. Weighted fairness (weighted max-min) is never explored.

- **Channel model ignores air-to-air blockage and 3D geometry.** The paper uses a log-distance path-loss model with log-normal shadowing and Rayleigh fading — a ground-to-ground model adapted for UAVs. Real UAV-to-UAV and UAV-to-HAP links at altitude have strong LoS components better modeled by Rician fading, and the 3D geometry (elevation angle, rotor downwash scattering) is not captured.

- **No uplink/downlink distinction.** The paper models all UAV links as uplink (UAV transmits, RBS/HAP receives). Downlink from RBS to UAV (e.g., control commands) is not modeled, which matters for full-duplex or FDD system design.

- **Security threats are not considered.** In dense urban UAV swarms, eavesdropping on UAV-RBS links or spoofing LCU-LCU safety-critical messages is a real threat. The paper does not consider physical-layer security, even though safety-critical LCU links are explicitly described as vulnerable.

## Potential Problem Statements This Gap Suggests

> **PS-1:** "Existing UAV resource allocation frameworks for integrated TN-NTN assume one-to-one spectrum sharing between HCUs and LCUs, which causes spectral under-utilization when J > I; a many-to-one dynamic spectrum sharing framework that allows multiple LCUs to reuse a single HCU's band under controlled aggregate interference constraints is needed but does not exist."

> **PS-2:** "Current resource allocation algorithms for multi-connected UAVs in TN-NTN treat UAV trajectories as fixed inputs, ignoring that changing the flight path changes channel gains to both RBS and HAP simultaneously; existing methods therefore cannot reach the true joint optimum of trajectory, spectrum, and power, especially under time-varying QoS demands."

> **PS-3:** "Statistical CSI-based resource allocation for UAV-TN-NTN networks fails when UAVs hover or move slowly (< 10 km/h), because the ergodic averaging assumption breaks down and quasi-static Rician channels emerge; no existing framework detects this regime and switches to instantaneous-CSI-based allocation accordingly."

> **PS-4:** "The Hungarian Method used in UAV spectrum-sharing assignment has O(I^3) complexity, which becomes computationally infeasible for dense swarms of 500+ UAVs in 6G urban scenarios; existing frameworks have no scalable approximate assignment mechanism that preserves near-optimal pairing quality under real-time latency constraints."

## Most Promising Gap (Your Pick)

**PS-2 (Joint trajectory and resource allocation) is the most researchable gap.**

Here is why: The paper's own simulation shows that as UAV speed increases, capacity degrades monotonically — but the paper treats speed as an uncontrollable external variable. In reality, a UAV can choose its speed and direction. The connection between trajectory and channel gain to both RBS and HAP is deterministic given a map (urban propagation models exist). The gap is precisely bounded: the paper provides a closed-form capacity expression (Eq. 16) and a two-step algorithm structure that could be extended to incorporate waypoint updates as a third optimization variable. There is a clear existing literature thread to compare against (references [22], [23], [25] in this paper), a well-defined metric (sum vs. min capacity under trajectory constraints), and the problem is not already solved — none of the 12 MC papers in Table I jointly optimize trajectory with spectrum sharing and power under heterogeneous QoS. This makes it simultaneously novel, technically tractable, and practically motivated.
