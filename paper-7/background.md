# Background: Resource Allocation and Sharing for UAV-Assisted Integrated TN-NTN with Multi-Connectivity

## What is this paper about? (so you know what to prep for)
This paper is about deciding which UAVs should share radio spectrum with which other UAVs, and how much transmit power each should use, in a network where UAVs are simultaneously connected to a ground base station AND a high-altitude platform. It's an optimization paper — math-heavy, with two proposed algorithms compared against baselines.

## What You Need to Know Before You Start

### TN-NTN (Terrestrial Network - Non-Terrestrial Network Integration)
This is the umbrella architecture: "TN" = your normal ground cell towers, "NTN" = anything airborne or space-based (UAVs, HAPs, satellites). "Integrated TN-NTN" just means both layers work together as one network, with UAVs sitting at the boundary, talking to both. The paper assumes you already know this is a real 6G research direction being standardized — it won't pause to motivate why TN and NTN are being merged.

### HAP (High-Altitude Platform)
Think of it as a stratospheric "almost-satellite" — an aircraft or balloon parked at ~17 km altitude that covers a huge area like a satellite but with much lower latency. The paper treats "HAP" as a known node type from page 1 onward; it won't re-explain what distinguishes a HAP from a satellite or a drone.

### Multi-Connectivity (MC) — the 3GPP concept
MC means a single device keeps two (or more) simultaneous active connections to different network nodes at once — like your phone staying connected to two carriers and combining their bandwidth. This is a standardized 3GPP feature, not something this paper invents. The paper just applies it: each "HCU" UAV maintains a live UAV-RBS link AND a live UAV-HAP link at the same time, and its total capacity is the sum of both. If you don't already know MC is a real, standardized capability (as opposed to a hypothetical), the system model will feel under-explained.

### HCU and LCU — the two UAV "roles" (confirm this before reading!)
- **HCU = High-Capacity User.** A UAV that needs to move large volumes of data (e.g., mission/sensor payloads) and uses multi-connectivity (RBS + HAP simultaneously) to do it.
- **LCU = Low-Capacity User.** A UAV (or a UAV-to-UAV pair) that mainly needs *reliable* short local links for coordination/safety (collision avoidance, trajectory updates) — not high data rate.

The paper sets up a world where there are I HCUs and J LCU-pairs, and the central question is: should an LCU pair be allowed to reuse (share) the spectrum band that's been given to an HCU? Sharing boosts spectral efficiency but creates interference for both sides. Everything in the paper revolves around this HCU-LCU pairing/sharing decision plus power levels — so get comfortable with "HCU = wants throughput, LCU = wants reliability" before page 1.

### Spectrum Sharing (and the binary indicator mu_{i,j})
This is the general idea of letting two users occupy the same frequency band at the same time, as long as the resulting interference stays within acceptable limits — like subletting a frequency slot. The paper formalizes this with a binary variable mu_{i,j}: 1 if LCU-pair j is sharing HCU i's band, 0 if not. It also assumes strict one-to-one pairing (each HCU shares with at most one LCU pair, and vice versa) — know this constraint going in, because it's central to why the Hungarian Method gets used at all.

### Hungarian Method (HM) / Assignment Problem
This is a classic, decades-old algorithm from combinatorial optimization that solves the "assignment problem": given a matrix of costs/benefits for every possible pairing between two groups (e.g., workers and jobs, or here, HCUs and LCU-pairs), it finds the overall-best one-to-one matching in polynomial time (O(n^3)). The paper assumes you know HM is a standard, well-understood tool — it won't re-derive it. What it WILL do is build a "capacity matrix" (one value per possible HCU-LCU pairing) and then feed that matrix straight into HM. If "assignment problem" or "Hungarian Method" is a new phrase to you, that's the one concept worth a quick refresher before diving in.

### Large-scale vs. small-scale channel fading (and "statistical CSI")
Wireless channels vary at two speeds: large-scale changes (path loss from distance, and shadowing from buildings) evolve slowly over many meters; small-scale fading (Rayleigh multipath fading) fluctuates extremely fast, almost instantaneously. "Statistical CSI" (Channel State Information) means making decisions based on the slow-changing, averaged statistics rather than chasing every instantaneous fluctuation. The paper's entire tractability argument rests on this distinction — it assumes you already know why averaging out fast fading (via "ergodic capacity") makes an optimization problem solvable in real time, even when UAVs move at 70-80 km/h.

### Resource allocation / power allocation basics
General familiarity with the idea of "jointly deciding transmit power levels and frequency assignments to maximize some network-wide objective (throughput, fairness, reliability) subject to constraints" will help — this is the broad category the paper's optimization problem (Eq. 12) belongs to. SINR (signal-to-interference-plus-noise ratio), outage probability, and QoS (minimum rate / reliability requirements) are all used as if you already know what they mean.

## Prior Methods/Papers This One Compares Against or Builds On
The paper compares its two algorithms (Algorithm 1: sum-capacity maximization; Algorithm 2: max-min fairness) against five baselines, including a **greedy spectrum-sharing baseline** (pairs UAVs greedily rather than optimally) and **single-connectivity (SC) variants** of its own algorithms (where HCUs use only one link — RBS or HAP — instead of both, to isolate the benefit of MC itself). It also positions itself against three broad prior research threads it says were previously studied in isolation: (1) cognitive-radio-based interference management for UAVs (which ignored HAP links), (2) multi-agent deep reinforcement learning approaches to dynamic channel/power allocation (which ignored UAV heterogeneity), and (3) prior multi-connectivity-for-UAV studies (which ignored heterogeneity, differentiated QoS, and joint spectrum+power optimization together). You don't need to have read these — just know they exist as the "everyone solved one piece, nobody solved all of it together" backdrop the paper uses to justify itself (see its Table I comparison).

## The Setup You'll See on Page 1
Picture a 2 km x 2 km urban area. At the center, on the ground, sits one RBS (a regular cell tower). Directly above the whole area, at 17 km altitude, hovers one HAP. Flying around at low altitude (0.1 km) and fairly fast (70 km/h) are 20 "HCU" UAVs and 20 "LCU" UAV-pairs. Each HCU UAV is simultaneously linked to both the RBS (ground) and the HAP (sky) — that's its multi-connectivity setup — and is hungry for throughput. Each LCU pair just needs its own short UAV-to-UAV link to stay reliable for safety/coordination. The question the paper answers: which LCU pairs, if any, should be allowed to reuse (share) which HCU's frequency band, and at what power levels, so that HCUs get as much capacity as possible (or as fair a capacity as possible) while LCUs stay reliable enough?

## Ready-to-Read Check
- [ ] I understand what TN-NTN integration means and where UAVs/HAPs/RBS sit in that picture
- [ ] I know HCU = high-capacity multi-connected UAV, LCU = low-capacity but high-reliability UAV pair, and that the paper is about whether/how they share spectrum
- [ ] I know the Hungarian Method solves "best one-to-one matching given a cost/benefit matrix" — and that this paper builds a capacity matrix specifically to feed it
- [ ] I understand why "large-scale / statistical CSI" (averaging out fast fading) is the trick that keeps this optimization tractable despite fast-moving UAVs
- [ ] I know why the core problem is hard: letting LCUs share spectrum with HCUs boosts efficiency but creates mutual interference, and you need to jointly pick *both* the pairing pattern *and* the power levels to balance HCU throughput against LCU reliability

(If something's unchecked, skim `terminology.md` for that term first — then come back.)
