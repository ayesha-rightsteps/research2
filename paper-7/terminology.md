# Terminology: Resource Allocation and Sharing for UAV-Assisted Integrated TN-NTN with Multi-Connectivity

> Quick reference for all acronyms, models, metrics, and algorithms used in this paper. Terms marked with ⭐ are the five most central concepts.

---

## TN-NTN (Terrestrial Network - Non-Terrestrial Network Integration) ⭐
**What it is:** An integrated architecture that combines conventional ground-based cellular infrastructure (terrestrial network) with airborne and space-based platforms (non-terrestrial network) into a unified communication system.
**In this paper:** The entire system model is built on integrated TN-NTN. UAVs link to both the terrestrial segment (via RBS) and the non-terrestrial segment (via HAP), sitting at the boundary between these two layers and serving as data-relaying intermediaries.
**Easy way to remember:** TN-NTN is the blueprint for future networks that work both on the ground and in the sky simultaneously.

---

## MC (Multi-Connectivity) ⭐
**What it is:** A 3GPP-standardized capability allowing a device to maintain simultaneous active connections to multiple network nodes (e.g., two different base stations), aggregating their capacity and improving reliability through diversity.
**In this paper:** HCU UAVs exploit MC by simultaneously connecting to both an RBS (terrestrial link) and a HAP (non-terrestrial link). The paper demonstrates that MC substantially outperforms single-connectivity (SC) across all evaluated scenarios, and the joint capacity of both links C^h_i = C^h_{i,R} + C^h_{i,H} is the primary performance metric.
**Easy way to remember:** MC is like using two phone carriers at the same time — if one signal drops, the other picks up the slack, and together they deliver more data.

---

## HCU (High-Capacity User) ⭐
**What it is:** In this paper, a UAV that requires high-capacity connections for bulk data transfer (e.g., mission data, sensor payloads). HCUs use multi-connectivity to simultaneously access both UAV-RBS and UAV-HAP links.
**In this paper:** There are I HCUs (set to 20 in simulations). HCUs share their allocated spectrum with LCUs to improve spectral efficiency, which introduces interference. The primary optimization objective is to maximize HCU capacity subject to LCU reliability constraints and power limits.
**Easy way to remember:** HCUs are the "heavy lifters" of the UAV swarm — they move big chunks of data and need solid, high-bandwidth links to do it.

---

## LCU (Low-Capacity User) ⭐
**What it is:** A UAV (or pair of UAVs) that requires reliable local UAV-to-UAV communication for control and coordination purposes (e.g., collision avoidance, trajectory updates) rather than high data rates.
**In this paper:** There are J LCU pairs (set to 20 in simulations). LCUs reuse the spectrum allocated to HCUs, creating interference for both parties. The constraint on LCUs is reliability (outage probability), not capacity: Pr(gamma^l_{j,j} <= gamma^l_0) <= Po.
**Easy way to remember:** LCUs are the "safety talkers" of the swarm — they send short, critical messages that must get through reliably, even if slowly.

---

## HAP (High-Altitude Platform) ⭐
**What it is:** An aircraft, airship, or balloon operating in the stratosphere (typically 17-22 km altitude) that provides wide-area wireless coverage, positioned between terrestrial base stations and satellites in the network hierarchy.
**In this paper:** The HAP is positioned at 17 km altitude directly above the center of the simulation area. HCUs connect to the HAP (UAV-HAP link) as one of their two MC connections. HAP has a noise figure of 3 dB and antenna gain of 8 dBi, and provides the NTN segment of the integrated TN-NTN architecture.
**Easy way to remember:** A HAP is like a very high-flying, slow-moving satellite — it covers a huge area and stays roughly in place, bridging the gap between ground towers and true satellites.

---

## RBS (Radio Base Station)
**What it is:** A conventional ground-based cellular infrastructure node that provides wireless connectivity to devices within its coverage area. Equivalent to a base station, eNodeB, or gNB in different cellular generations.
**In this paper:** One RBS is placed at the center of the 2 km x 2 km simulation area at 20 m height with 8 dBi antenna gain and 5 dB noise figure. HCUs connect to the RBS (UAV-RBS link) as their terrestrial MC connection. The RBS estimates path loss and shadowing for HCU-RBS and LCU-RBS links and collects channel measurements from LCU receivers.
**Easy way to remember:** The RBS is the ground anchor of the system — the conventional cell tower that the UAVs connect to from above.

---

## Spectrum Sharing
**What it is:** A technique where multiple users share the same frequency band, with one user (the primary/incumbent) having priority and others (secondary users) allowed to use the same spectrum provided they meet interference constraints.
**In this paper:** Each HCU is allocated an orthogonal spectrum band; LCUs are allowed to reuse these bands to improve spectral efficiency. The spectrum allocation index mu_{i,j} = 1 if LCU j shares the band of HCU i, otherwise 0. One-to-one pairing is assumed: each HCU's spectrum is shared with at most one LCU, and each LCU accesses at most one HCU's spectrum.
**Easy way to remember:** Spectrum sharing is like subletting a radio frequency — the primary user keeps their slot, but a secondary user can borrow it as long as they don't cause too much interference.

---

## HM (Hungarian Method)
**What it is:** A classical combinatorial algorithm that solves the linear assignment problem — finding the optimal one-to-one matching between two sets (e.g., workers and tasks) to minimize cost or maximize benefit — in polynomial time O(n^3).
**In this paper:** The HM is the core computational tool for both Algorithm 1 and Algorithm 2. After computing the maximum capacity C*_{i,j} for every HCU-LCU pair, the HM finds the optimal spectrum sharing pattern {mu*_{i,j}} that maximizes total (Algo 1) or minimum (Algo 2) HCU capacity. Its polynomial complexity ensures the overall algorithms remain tractable.
**Easy way to remember:** The Hungarian Method is an efficient "matchmaker" algorithm — it finds the best pairing between HCUs and LCUs so the most valuable spectrum-sharing arrangements are made.

---

## Algorithm 1 (Sum HCU Capacity Maximization)
**What it is:** The paper's first proposed algorithm, which maximizes the total sum capacity of all HCU connections (UAV-RBS + UAV-HAP) while guaranteeing reliability for all LCU-to-LCU links and meeting minimum capacity requirements for each HCU.
**In this paper:** Algorithm 1 uses a two-step approach: (1) compute optimal per-pair power allocation P^{h*}_i and P^{l*}_j using the closed-form solution in Eq. (15) and bisection search, (2) apply the HM to the resulting capacity matrix {C*_{i,j}} to find the optimal spectrum sharing pattern. Overall complexity is O(JI log(1/epsilon) + I^3).
**Easy way to remember:** Algorithm 1 is the "maximum total pie" approach — it optimizes the combined throughput across all HCUs, even if some HCUs get a larger slice than others.

---

## Algorithm 2 (Minimum HCU Capacity Maximization / Max-Min Fairness)
**What it is:** The paper's second proposed algorithm, which maximizes the minimum capacity across all HCUs rather than the sum, ensuring that even the worst-served HCU achieves as high a capacity as possible. This is a max-min (Pareto-optimal) fairness objective.
**In this paper:** Algorithm 2 builds on the capacity matrix {C*_{i,j}} from Algorithm 1 and applies a bisection search over the discrete set of candidate minimum capacities, calling the HM at each bisection step to check feasibility. Overall complexity is O(JI log(1/epsilon) + JI log(JI) + I^3 log I). Consistently achieves higher minimum capacity than Algorithm 1.
**Easy way to remember:** Algorithm 2 is the "raise the floor" approach — instead of maximizing the total, it makes sure the worst-off HCU is as good as possible.

---

## SINR (Signal-to-Interference-plus-Noise Ratio)
**What it is:** A measure of communication quality: the power of the desired signal divided by the combined power of interfering signals and noise. Higher SINR means better link quality and higher achievable data rate.
**In this paper:** Three SINR expressions are defined: gamma^h_{i,R} for HCU i at RBS (Eq. 3), gamma^h_{i,H} for HCU i at HAP (Eq. 5), and gamma^l_{j,j} for LCU j in its local UAV-UAV pair (Eq. 4). The LCU reliability constraint requires gamma^l_{j,j} to exceed gamma^l_0 = 5 dB with outage probability no greater than Po = 10^-3.
**Easy way to remember:** SINR is the "signal clarity score" — a number that tells you how clearly the message arrives above all the noise and interference.

---

## Ergodic Capacity
**What it is:** The time-averaged (statistical expectation) channel capacity over all random realizations of fading, representing the maximum long-term achievable data rate for a communication link with random channel variations.
**In this paper:** HCU capacity is defined as the ergodic capacity: C^h_{i,R} = E[log2(1 + gamma^h_{i,R})] and C^h_{i,H} = E[log2(1 + gamma^h_{i,H})], expressed in bps/Hz. This formulation averages out small-scale fading, requiring only statistical CSI rather than instantaneous CSI, which is the key engineering choice enabling tractable optimization.
**Easy way to remember:** Ergodic capacity is the "average highway speed" of a link — it tells you the long-run throughput even though moment-to-moment conditions fluctuate.

---

## Large-Scale CSI (Channel State Information)
**What it is:** Channel parameters that vary slowly over time and distance — primarily path loss and shadowing — as opposed to small-scale (fast) fading which fluctuates over fractions of a wavelength.
**In this paper:** The paper deliberately bases all resource allocation decisions on large-scale CSI only. This is justified because large-scale parameters remain stable over hundreds of milliseconds (even at 70-80 km/h UAV speeds), making them suitable for centralized optimization without requiring impractically fast feedback. Instantaneous small-scale fading is averaged out in the ergodic capacity calculation.
**Easy way to remember:** Large-scale CSI is the "big picture" channel quality — the average signal strength over a long distance, not the millisecond-by-millisecond fluctuations.

---

## Path Loss
**What it is:** The reduction in signal power as an electromagnetic wave propagates through space, modeled as a function of distance and frequency. In the log-distance model, path loss increases as distance raised to the power of the path loss exponent.
**In this paper:** Path loss is modeled as L_p * D^{-phi}_{x,y} where L_p is a constant factor, D_{x,y} is the distance between nodes x and y, and phi is the path loss exponent. This multiplicative factor appears in all channel power gain expressions (Eq. 1).
**Easy way to remember:** Path loss is signal "fade with distance" — the further apart sender and receiver are, the weaker the signal arrives, following a power-law relationship.

---

## Log-Normal Shadowing
**What it is:** A random variation in received signal power caused by large obstacles (buildings, terrain) blocking or attenuating the signal, modeled as a log-normal random variable (i.e., Gaussian in dB).
**In this paper:** Shadowing gain chi_{x,y} follows a log-normal distribution with standard deviation xi (set to 8 dB for HCU-RBS links and 3 dB for HCU-LCU and HCU-HAP links). It represents the random obstruction effects in the dense urban environment and is included in the large-scale fading component used for resource allocation.
**Easy way to remember:** Log-normal shadowing captures the random "obstacle lottery" — some UAVs happen to fly near buildings that block their signals more than others.

---

## Rayleigh Fading / Small-Scale Fading
**What it is:** Rapid random fluctuations in signal amplitude due to multipath propagation — multiple copies of the signal arriving at the receiver after bouncing off different surfaces, combining constructively or destructively. In Rayleigh fading, the signal envelope follows a Rayleigh distribution, and the power gain follows an exponential distribution.
**In this paper:** Small-scale fading power gain g^(s)_{i,R} and g^(s)_{j,R} are modeled as independent exponential random variables with unit mean (corresponding to Rayleigh fading). Their statistical properties are used in the closed-form capacity derivation (Appendices A-C), but instantaneous values are averaged out in the ergodic capacity expressions.
**Easy way to remember:** Rayleigh fading is the "multipath lottery" — your signal bounces off walls and the copies can either reinforce or cancel each other out, causing rapid signal level changes.

---

## Doppler Shift
**What it is:** The change in frequency of a received signal caused by relative motion between the transmitter and receiver. When a UAV moves toward a base station, the received frequency is higher than transmitted; when moving away, it is lower.
**In this paper:** The Doppler shift in the HCU-RBS link is f_{d,iR} = (v/lambda) * cos(theta_{i,R}), where v is UAV speed, lambda is carrier wavelength, and theta_{i,R} is the angle between UAV velocity and the LoS direction. Under the standard assumption of proper Doppler compensation (no inter-carrier interference), the Doppler phase factor does not affect the statistical channel magnitude used for resource allocation.
**Easy way to remember:** Doppler shift is the wireless equivalent of the ambulance siren pitch change — as the UAV flies toward or away from the tower, the signal frequency appears to shift.

---

## Outage Probability (Po)
**What it is:** The probability that the received SINR falls below a minimum required threshold, resulting in a link failure or "outage." It quantifies the reliability of a wireless link.
**In this paper:** Po is the maximum tolerable outage probability for LCU-to-LCU links, set to 10^-3 in simulations. The constraint Pr(gamma^l_{j,j} <= gamma^l_0) <= Po is the reliability requirement for LCUs. This constraint is converted to a closed-form power constraint on HCU transmit power (Eq. 14), enabling analytical optimization.
**Easy way to remember:** Outage probability is the "failure rate" of a link — Po = 10^-3 means the link fails at most once in every 1000 time slots.

---

## Bisection Search
**What it is:** A numerical root-finding method that iteratively halves a search interval by evaluating the function at the midpoint and discarding the half where the solution cannot lie, converging in O(log(1/epsilon)) iterations.
**In this paper:** Bisection is used in two places: (1) to find P^l_{h,m} = f^{-1}(P^h_m) — the maximum LCU power that still allows HCU to transmit at maximum power — exploiting the monotonicity of f(P^l_j); (2) inside Algorithm 2 to search over the discrete set of candidate minimum capacities {C*_{i,j}}. In both cases, the bisection's convergence guarantee is what ensures finite termination of the algorithms.
**Easy way to remember:** Bisection search is like the "hot-cold game" — cut the search range in half each time based on whether you're above or below the target.

---

## Spectrum Allocation Index (mu_{i,j})
**What it is:** A binary indicator variable: mu_{i,j} = 1 means LCU j is sharing the spectrum band originally allocated to HCU i; mu_{i,j} = 0 means no sharing between this pair.
**In this paper:** The set {mu_{i,j}} defines the spectrum sharing pattern, which is the combinatorial variable optimized by the HM. Constraints (12f) and (12g) enforce the one-to-one pairing assumption: each HCU's band can be shared with at most one LCU, and each LCU accesses at most one HCU's band.
**Easy way to remember:** mu_{i,j} is the "permission slip" — a 1 means LCU j has permission to use HCU i's frequency band.

---

## Sum Capacity
**What it is:** The total aggregated data rate across all HCU connections (both UAV-RBS and UAV-HAP links for all I HCUs), measured in bps/Hz. It represents the overall network throughput.
**In this paper:** Maximized by Algorithm 1 (objective function in Eq. 12a). Summed as sum_{i in I} C^h_i where C^h_i = C^h_{i,R} + C^h_{i,H}. In simulations with 20 HCUs and 20 LCUs at J/I = 0.2, Algorithm 1 with MC achieves approximately 950-1000 bps/Hz.
**Easy way to remember:** Sum capacity is the "total bandwidth pie" — it measures how much data all HCUs together can transfer per second per Hz.

---

## Minimum Capacity (Max-Min Capacity)
**What it is:** The capacity of the worst-served HCU in the network. Maximizing the minimum capacity (max-min fairness) ensures that no HCU is left with a disproportionately poor connection.
**In this paper:** Maximized by Algorithm 2 (objective function in Eq. 19). Algorithm 2 consistently achieves higher minimum capacity than Algorithm 1, which tends to sacrifice weaker HCUs to boost overall sum capacity. At J/I = 0.2, Algorithm 2 achieves minimum capacity around 11-12 bps/Hz, substantially higher than all baselines.
**Easy way to remember:** Minimum capacity is the "weakest link" metric — Algorithm 2 specifically improves the worst-off UAV's connection.

---

## E1(x) (Exponential Integral Function)
**What it is:** A special mathematical function defined as E1(x) = integral from x to infinity of (e^{-t}/t) dt, used in the closed-form expression of ergodic capacity over Rayleigh fading channels with interference.
**In this paper:** Appears in the closed-form HCU capacity expression (Eq. 16): C^h_{i,j} = rho / [(rho - eta) * ln2] * [e^{1/rho} * E1(1/rho) - e^{1/eta} * E1(1/eta)], where rho and eta are SNR-like parameters. This closed form avoids Monte Carlo integration, making the capacity evaluation computationally efficient.
**Easy way to remember:** E1(x) is the "capacity calculator's special function" — it appears whenever you average a log-SINR expression over exponential (Rayleigh) fading in closed form.

---

## QoS (Quality of Service)
**What it is:** A set of minimum performance requirements that a communication link must meet to be considered acceptable for a given application, expressed in terms of data rate, reliability, latency, etc.
**In this paper:** Two differentiated QoS requirements are enforced: HCUs require high capacity (minimum C^h_0 = 0.5 bps/Hz) for major links (UAV-RBS, UAV-HAP), while LCUs require high reliability (outage probability <= Po = 10^-3 at SINR threshold gamma^l_0 = 5 dB) for local UAV-UAV links. This distinction drives the entire optimization problem formulation.
**Easy way to remember:** QoS is the "service level agreement" for each link type — one type needs speed, the other needs reliability.

---

## SC (Single-Connectivity)
**What it is:** The conventional mode where a device maintains only one active connection to the network at a time, as opposed to multi-connectivity.
**In this paper:** SC is presented as a performance baseline alongside the proposed MC algorithms. Figures consistently show that MC (Algorithms 1 and 2) substantially outperforms SC (Algorithms 1 SC and 2 SC) in both sum and minimum capacity, demonstrating the direct benefit of maintaining simultaneous UAV-RBS and UAV-HAP connections.
**Easy way to remember:** SC is "one lane traffic" — you can only go through one road at a time, while MC opens multiple lanes simultaneously.

---

## J/I Ratio
**What it is:** The ratio of the number of LCU pairs (J) to the number of HCUs (I). It reflects the relative density of local UAV-UAV links versus major infrastructure links in the network.
**In this paper:** A key simulation parameter varied from 0.2 to 1.0 in performance evaluation figures. As J/I increases, more LCUs share the same spectrum bands as HCUs, increasing interference and generally degrading HCU capacity. Algorithm 1's sum capacity degrades more gracefully than Algorithm 2 as J/I increases.
**Easy way to remember:** J/I ratio is the "interference loading dial" — turning it up adds more spectrum-sharing LCUs, increasing interference and making resource allocation harder.

---

## UAV-to-UAV Link (UAV-UAV Link / LCU-LCU Link)
**What it is:** A direct wireless communication link between two UAVs, used for local coordination, safety messaging, and relay functions.
**In this paper:** UAV-UAV links are the local (LCU) links. They operate on spectrum shared with HCUs, causing cross-interference. They require high reliability rather than high capacity, and their SINR expression gamma^l_{j,j} (Eq. 4) captures interference from HCUs via the term sum_{i in I} mu_{i,j} P^h_i |h_{i,j}|.
**Easy way to remember:** UAV-UAV links are the "walkie-talkie channels" between drones — short-range, safety-critical communications that must work reliably even with limited bandwidth.

---

## Propulsion Power
**What it is:** The mechanical power consumed by a UAV's motors and rotors to sustain flight at a given speed, as distinct from the electrical power used for communication electronics.
**In this paper:** Denoted P^prop_x(v_x) for UAV x flying at speed v_x. Total UAV power is P^tot_x(t) = P^prop_x(v_x(t)) + P^comm_x(t), and accumulated energy over T slots is E^tot_i = tau * sum_{t=1}^{T} P^tot_i(t). The battery energy constraint E^tot_x <= E^max_x (Eq. 12h) is included as a feasibility constraint.
**Easy way to remember:** Propulsion power is the "gas tank drain" — it's the energy cost of just staying in the air, separate from the cost of transmitting data.

---

## Spatial Poisson Process
**What it is:** A stochastic model for the random placement of points (devices, UAVs) in a spatial area, where the number of points in any region follows a Poisson distribution and placements in non-overlapping regions are independent.
**In this paper:** UAV positions are modeled as following a spatial Poisson process in simulations, with non-uniform density that varies with UAV speed (denser UAVs when slow, sparser when fast, with average inter-UAV distance equal to twice the absolute speed in m/s).
**Easy way to remember:** A spatial Poisson process is like randomly scattering seeds on a field — no pattern, just random independent placement across the area.

---

## Interference Channel
**What it is:** A wireless channel through which an unintended signal (interference) propagates from a transmitter to a receiver that is not its intended destination, degrading the SINR at that receiver.
**In this paper:** Four interference channels are defined: h_{j,R} (LCU-to-RBS interference), h_{i,j} (HCU-to-LCU interference), h_{j,H} (LCU-to-HAP interference), and implicitly the reverse cross-interference terms. These channels create coupled SINR expressions for both HCUs and LCUs when spectrum sharing is active.
**Easy way to remember:** An interference channel is the "cross-talk path" — it carries someone else's signal into your receiver, making it harder to hear your intended sender.

---

## Pareto Optimality
**What it is:** A resource allocation outcome is Pareto optimal if no individual user's performance can be improved without worsening at least one other user's performance. It represents the efficient frontier of achievable trade-offs.
**In this paper:** The max-min optimization solved by Algorithm 2 is noted to guarantee Pareto optimality: maximizing the minimum HCU capacity ensures that no HCU can be further improved without reducing another HCU's capacity. This property is cited from prior multi-objective signal processing literature.
**Easy way to remember:** Pareto optimality is the "no free lunch" frontier — you've squeezed out all the easy gains, and now improving one user genuinely costs another.

---

## 3GPP (3rd Generation Partnership Project)
**What it is:** The international standardization body that produces technical specifications for mobile telecommunications systems, including 5G NR and the 6G roadmap. It has been actively standardizing NTN features and multi-connectivity.
**In this paper:** Cited as the organization that standardized multi-connectivity as a key 6G technology. The paper notes that 3GPP standardized Release 15 features for enhanced LTE support for connected UAVs, addressing interference issues from aerial vehicles to neighboring cells.
**Easy way to remember:** 3GPP is the "rules committee" for global mobile networks — if a technology works in 5G or 6G, 3GPP wrote the specification for it.

---

## NTN (Non-Terrestrial Network)
**What it is:** The segment of a communication network that uses airborne or space-based platforms — UAVs, HAPs, satellites — rather than fixed ground infrastructure to provide wireless connectivity.
**In this paper:** The NTN segment consists of HAPs and UAVs. The UAV-HAP link (h_{i,H}) is the NTN-to-NTN segment of the multi-connectivity architecture, while the UAV-RBS link is the TN-to-NTN segment. Resource allocation must account for both simultaneously.
**Easy way to remember:** NTN is everything above the ground in the network — the flying and orbiting nodes that extend coverage beyond what towers can reach.
