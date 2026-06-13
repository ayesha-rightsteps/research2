# Terminology: ISAC-Enabled UAV Vehicular Network with BCD-SCA

---

## ISAC (Integrated Sensing and Communication) ⭐

**Definition:** A technology that enables a single hardware platform and waveform to simultaneously perform wireless data communication and radar sensing, sharing the same spectrum, antennas, and RF components.

**Details:** In traditional systems, radar and communication are completely separate. ISAC fuses them by transmitting OFDM signals that are simultaneously decoded by receiving vehicles (communication function) and reflected back to the transmitter for target detection (radar function). This eliminates spectrum duplication and hardware redundancy.

**In this paper:** Both UAVs and GBSs are equipped with OFDM-based dual-function transmitters. Each subchannel can be designated for either communication (sccr^{t,c} = 1) or radar sensing (sccr^{t,c} = 0). The radar sensing function processes reflections from vehicle targets to estimate their scattering characteristics, quantified by Mutual Information. ISAC enables vehicles to receive both data service and beyond-line-of-sight (BLOS) perception support from the same infrastructure simultaneously.

**Why it matters for 6G:** ISAC is a key pillar of 6G architecture, enabling communication infrastructure to double as sensing infrastructure for applications including autonomous driving, smart cities, and environmental monitoring.

---

## BCD (Block Coordinate Descent) ⭐

**Definition:** An iterative optimization algorithm that divides the optimization variables into blocks and updates one block at a time while holding the others fixed, cycling through all blocks until convergence.

**Details:** Given a joint optimization problem over variables {x_1, x_2, x_3}, BCD solves:
1. Minimize over x_1 (holding x_2, x_3 fixed)
2. Minimize over x_2 (holding x_1, x_3 fixed)
3. Minimize over x_3 (holding x_1, x_2 fixed)
4. Repeat until convergence

BCD is particularly effective when the full joint problem is intractable (as in MINLP) but each block-level subproblem is easier to solve. Each iteration is guaranteed to produce a solution no worse than the previous, ensuring monotonic non-decreasing objective values and convergence to a stationary point.

**In this paper:** The original MINLP P1 (jointly optimizing vehicle association cb, UAV trajectory CU, and subchannel allocation ca plus function selection sccr) is decomposed into three subproblems:
- P1.1: Vehicle association (solved by interior-point method after linear relaxation)
- P1.2: UAV trajectory planning (solved by SCA + interior-point method)
- P1.3: Subchannel allocation and function selection (solved by SCA + interior-point method)

The algorithm converges within approximately 4 iterations in experiments.

---

## SCA (Successive Convex Approximation) ⭐

**Definition:** An iterative method for solving non-convex optimization problems by locally approximating non-convex functions with convex surrogates (typically via first-order Taylor expansion) at each iteration, then solving the resulting convex problem.

**Details:** At iteration k, a non-convex function f(x) is approximated by its first-order Taylor expansion around the current point x_k:

  f_approx(x; x_k) = f(x_k) + gradient_f(x_k)^T * (x - x_k)

For a convex f, this approximation is a global lower bound. Each SCA step produces a feasible point that improves the original objective. The approximation error is bounded by (L/2) * ||x - x_k||^2 (L = Lipschitz constant of the gradient), which vanishes as x approaches x_k at convergence. SCA converts non-convex problems into a sequence of convex problems solvable by standard tools such as the interior-point method.

**In this paper:**
- In P1.2 (trajectory planning): The non-convex log term in the communication rate (as a function of UAV-vehicle distance squared) is approximated by its Taylor lower bound. The non-convex inter-UAV safety constraint is also linearized via Taylor expansion.
- In P1.3 (subchannel allocation): The non-convex product cat * sccr (both binary variables) is approximated using the identity a*b = ((a+b)^2 - a^2 - b^2)/2 and Taylor expansion on the quadratic terms to obtain a linear constraint.

---

## MINLP (Mixed-Integer Nonlinear Programming) ⭐

**Definition:** An optimization problem class that contains both continuous and discrete (integer) decision variables, with nonlinear objective functions or constraints. MINLP is generally NP-hard; finding a global optimum is intractable in polynomial time for large-scale instances.

**Details:** The intractability arises from the combination of integer combinatorics (exponential search space) and nonlinearity (non-convex constraints, multiplicative coupling between variables). Standard approaches include branch-and-bound, relaxation-and-rounding, and decomposition methods (BCD, Lagrangian relaxation).

**In this paper:** The optimization problem P1 is MINLP because:
- Integer variables: Vehicle association cb (binary), subchannel allocation ca (binary), subchannel function selection sccr (binary).
- Continuous variables: UAV trajectory CU (continuous horizontal positions).
- Nonlinear coupling: The communication rate contains terms like cat^{t,c}_v * sccr^{t,c} * log2(1 + h^{t,c}_{u,v} * P_u / N_0), where binary variables multiply a logarithmic function of channel gain that itself depends nonlinearly on UAV position through distance squared.

The BCD-SCA approach converts P1 into tractable subproblems, obtaining an approximate stationary-point solution rather than a global optimum.

---

## Mutual Information (MI) ⭐

**Definition:** An information-theoretic measure of the statistical dependence between two random variables. In ISAC sensing, MI between the target's impulse response (q) and the received reflected signal (z) given the transmitted waveform (s) quantifies how much information about the target is extractable from the radar echo.

**Formula in this paper:**
  MI^{t,c}_{u,v} = (1/2) * B_0 * S * T_s * log2(1 + P^t_u * S * T_s^2 * |Q^c_{u,v}(f)|^2 / sigma^c_{u,rad})

where B_0 is subchannel bandwidth, S is the number of consecutive OFDM symbols per sensing interval, T_s is OFDM symbol duration (including cyclic prefix), P^t_u is UAV transmit power, |Q^c_{u,v}(f)|^2 is the squared Fourier magnitude of the target impulse response at subcarrier frequency f^c, and sigma^c_{u,rad} is radar receiver noise power on subchannel c.

**Why MI is used:** MI is a unified surrogate for classical radar metrics. Under the Gaussian echo model:
- MI is monotonically related to the KL-divergence between target-present and target-absent hypotheses, which directly determines optimal detection probability (Pd) and false-alarm probability (Pfa) under the Neyman-Pearson framework.
- Higher MI implies a tighter Cramer-Rao Bound (lower parameter estimation variance for target range, velocity, and angle).

MI is preferred over Pd/Pfa or CRB in this paper because it is analytically tractable and directly integrable into the convex optimization framework. A minimum MI threshold HMI (200-1000 bits in experiments) is enforced as a sensing performance constraint for each vehicle at each time slot.

**Sensing-communication tradeoff:** Higher MI requires more subchannels allocated for sensing (sccr = 0), directly reducing the subchannels available for communication (sccr = 1) and lowering throughput. This is the core tension modeled throughout the paper.

---

## UAV (Unmanned Aerial Vehicle)

**Definition:** An aircraft that operates without a human pilot onboard, controlled remotely or autonomously.

**In this paper:** Two UAVs (U = 2) are deployed in the simulation. Each UAV is equipped with an OFDM-based dual-function ISAC transmitter and flies at a fixed altitude of 8 m above the highway. UAVs take off from preset starting points, serve vehicle users during T = 30 time slots (tau = 1 s/slot), and must land at preset endpoints. UAV horizontal trajectory is the primary optimization variable in subproblem P1.2. Maximum flight speed is 6-12 m/s in experiments; onboard energy budget is 500 J; energy consumption is 2 J per meter flown.

**Altitude choice (8 m):** Follows common practice for low-altitude UAV sensing/communication studies (5-30 m above roadways). Provides high LOS probability, reduces path loss, gives unobstructed coverage over vehicles (below 4-5 m height), and complies with FAA Part 107 VLOS regulations (below 120 m in controlled areas).

---

## GBS (Ground Base Station)

**Definition:** A fixed wireless communication base station located on the ground, providing coverage to vehicles within its service area.

**In this paper:** One GBS (R = 1) is randomly deployed along the roadside. Like UAVs, the GBS is equipped with an ISAC dual-function transmitter that can simultaneously communicate with and sense vehicles. The GBS position is fixed (not optimized); vehicle association to the GBS is an optimization variable. GBS transmit power is fixed at Pt_r = 3 W. The cooperative sensing between UAVs and GBS provides multi-angle radar coverage of vehicles, improving sensing completeness and enabling beyond-line-of-sight perception.

---

## Subchannel Allocation

**Definition:** The assignment of frequency subchannels (discrete spectrum units) to users for data transmission or sensing.

**Details:** The available spectrum is divided into C orthogonal subchannels, each with bandwidth B_0. Each subchannel can be assigned to only one vehicle at a time (exclusive allocation: sum_v cat^{t,c}_v = 1 for each subchannel c at each time slot t). The allocation decision ca = {cat^{t,c}_v} is binary. C ranges from 30 to 60 in experiments; B_0 = 1-3 MHz.

**Optimization role:** Subchannel allocation determines which vehicles receive spectrum resources. Together with subchannel function selection, it controls whether those resources serve communication (increasing data rate) or sensing (increasing radar MI). As the number of subchannels increases, the proposed algorithm achieves a 19.75% improvement over URA, demonstrating the value of intelligent allocation.

---

## Vehicle Association

**Definition:** The assignment of each vehicle to a specific edge node (UAV or GBS) that will serve it for communication and sensing in a given time slot.

**Details:** Each vehicle must associate with exactly one UAV or one GBS per time slot (constraint: sum_r cb^t_{r,v} + sum_u cb^t_{u,v} = 1 for all vehicles v). The association decision cb = {cb^t_{v,r}, cb^t_{v,u}} is binary.

**Optimization role:** Vehicle association determines the channel gain experienced, because path loss depends on distance to the associated node. Associating a vehicle with the nearest UAV or GBS generally improves both communication rate and sensing MI. Subproblem P1.1 is solved by linear relaxation, logarithmic barrier method, exterior penalty function, Newton's method, and random rounding.

---

## Trajectory Planning (UAV)

**Definition:** The optimization of a UAV's flight path over time to achieve mission objectives — here: maximize average achievable communication rate under energy, safety, and sensing constraints.

**Details:** The UAV trajectory is a sequence of 2D horizontal positions {CU^t_u = (x^t_u, y^t_u)} for t = 1, ..., T, subject to: speed constraint (step distance bounded by max_speed * tau), start/end point constraint, energy consumption constraint (total flight energy below 500 J), and inter-UAV safety distance constraint (minimum 8 m between UAVs). Altitude is fixed at 8 m; optimization is on the horizontal plane.

**Key result from experiments:** After trajectory optimization, UAVs fly closer to vehicle lanes rather than along predetermined straight-line routes. This reduces UAV-to-vehicle distance, lowering path loss and increasing both communication rate and sensing MI. TDRA outperforms SAUA (no trajectory optimization) by 178.57% — the largest gain over any baseline — showing trajectory optimization is the most critical component of the joint framework.

---

## Radar Sensing Constraint

**Definition:** A constraint in the optimization problem that enforces a minimum level of radar sensing quality, expressed as a Mutual Information threshold, for each vehicle at each time slot.

**In this paper:**
  TMI^t_v >= HMI, for all vehicles v in VE and time slots t in TI

where TMI^t_v is the total MI obtained for sensing vehicle v at time t (summed over all sensing-designated subchannels from both the associated UAV and the GBS sensing vehicle v), and HMI is the minimum MI threshold (200-1000 bits in experiments).

**Effect on optimization:** Higher HMI forces more subchannels to be designated for sensing (sccr = 0), reducing the communication bandwidth available and lowering average achievable rate. As HMI increases from 200 to 1000 bits, average achievable rate decreases monotonically (Figure 8) — the quantitative expression of the sensing-communication tradeoff.

---

## Rician Channel

**Definition:** A statistical channel model capturing the combination of a dominant LOS component and multiple weaker scattered NLoS components, with the amplitude following a Rice distribution parameterized by the K-factor.

**Details:** As K approaches 0, Rician becomes Rayleigh fading (pure scattering); as K approaches infinity, it becomes pure LOS. Rician channels are typical for low-to-medium altitude UAV-to-ground links due to high LOS probability.

**Relevance to this paper:** The paper adopts a simplified free-space path loss model (LoS-dominant for the low-altitude simulation scenario), but acknowledges incorporating Rician fading, shadowing, altitude-dependent fading, and Doppler variation as directions for future work.

---

## LoS (Line-of-Sight)

**Definition:** A propagation path between transmitter and receiver where there is a direct, unobstructed signal path.

**In this paper:** At 8 m UAV altitude with vehicles below 4-5 m, LoS is the dominant propagation mode. Channel gain model: h^{t,c}_{u,v} = h^c_{u,v} / (d^t_{u,v})^2, reflecting free-space (LoS) path loss. LoS paths improve both communication rate (higher received signal power) and radar sensing MI (stronger echo return). UAV trajectory optimization effectively maximizes LoS conditions by minimizing UAV-vehicle distance.

---

## NLoS (Non-Line-of-Sight)

**Definition:** A propagation condition where the direct path between transmitter and receiver is blocked by obstacles, requiring signals to travel via reflections, diffraction, or scattering.

**In vehicular networks:** Buildings, trees, and other vehicles create NLoS conditions that add attenuation, delay spread, and multipath fading beyond free-space path loss. The current paper model assumes LoS-dominant conditions. The paper explicitly acknowledges that incorporating NLoS channel models (including urban blockage effects) is left for future work.

---

## Convex Optimization

**Definition:** A subclass of mathematical optimization where both the objective function and the feasible set are convex. Any local minimum is also a global minimum, enabling efficient polynomial-time algorithms.

**Key tools used in this paper:**
- Interior-point method: Solves convex LP and convex QP/SOCP problems by searching through the feasible region interior using barrier functions and Newton's method.
- Logarithmic barrier method: Converts inequality constraints into log-penalty terms in the objective, enabling unconstrained optimization via Newton's method.
- Exterior penalty function: Converts equality constraints into quadratic penalty terms.

After BCD decomposition and SCA convexification, each subproblem (P1.1.2, P1.2.2, P1.3.2) is a convex problem solvable by the interior-point method.

---

## SCA Approximation

**Definition:** The specific technique within SCA — replacing a non-convex function with its first-order Taylor expansion (a linear, hence convex, lower bound) at the current iterate.

**Mathematical property:** For a convex and continuously differentiable function f(x), the first-order Taylor approximation at x_k gives: f_approx(x; x_k) = f(x_k) + gradient_f(x_k)^T * (x - x_k) <= f(x). This is a global lower bound. The approximation error satisfies: 0 <= f(x) - f_approx(x; x_k) <= (L/2) * ||x - x_k||^2, where L is the Lipschitz constant of the gradient. Near convergence (where ||x - x_k||^2 becomes small), the error vanishes.

**In P1.2 (trajectory):** The log rate term delta^{t,c}_{v,u} = log2(1 + h * P / (||CU - CO||^2 + H^2)) is concave in the distance squared. Its Taylor expansion around the current iterate provides a convex lower-bound constraint.

**In P1.3 (subchannel):** The product cat * sccr is approximated using the quadratic identity and Taylor expansion of the squared sum and individual squares, yielding a linear constraint.

---

## ITS (Intelligent Transportation Systems)

**Definition:** An integrated application of advanced sensing, communication, computing, and control technologies to transportation infrastructure and vehicles to improve safety, efficiency, and sustainability.

**Scope:** ITS encompasses traffic signal control, V2X communication, autonomous driving, incident detection and management, and cooperative adaptive cruise control (CACC).

**In this paper:** Temporary traffic congestion is the primary use case — a sudden event (accident, roadwork, peak hour) causes a localized surge in communication and sensing demand that overwhelms fixed GBS capacity. UAV-ISAC provides rapid, flexible, supplementary coverage that alleviates both the communication burden and the sensing blind spots, directly supporting ITS safety applications through BLOS perception.

---

## Sensing-Communication Tradeoff

**Definition:** The fundamental tension in ISAC systems between dedicating spectrum resources to communication versus sensing. Improving one function typically degrades the other.

**Mechanism in this paper:** Each subchannel can serve communication (sccr = 1: contributes to vehicle data rate) or sensing (sccr = 0: contributes to radar MI). The optimization problem P1 balances this by maximizing average achievable communication rate (objective) while satisfying minimum sensing MI constraints (TMI^t_v >= HMI). As HMI increases, more subchannels must be allocated to sensing, leaving fewer for communication.

**Quantitative evidence:** Figure 8 shows average achievable rate decreasing monotonically as HMI increases from 200 to 1000 bits across all compared algorithms. The proposed TDRA algorithm consistently outperforms all baselines even under tight sensing constraints.

---

## URA (User association Random Assignment)

**Definition:** A benchmark algorithm that optimizes UAV trajectory and subchannel allocation but assigns vehicle-to-edge-node associations randomly, without optimization.

**Full name from paper:** "Algorithm with User association Random Assignment" — the UAV maximizes downlink rate by optimizing trajectory and subchannel allocation, but vehicle association is random.

**Performance gap vs. TDRA:** TDRA outperforms URA by an average of 32.42% in average achievable rate (varying UAV speed). This gap isolates the contribution of vehicle association optimization to the joint optimization framework.

---

## SAUA (Subchannel allocation and User Association)

**Definition:** A benchmark algorithm that jointly optimizes vehicle association and subchannel allocation but does not optimize the UAV trajectory — UAV flies along a predetermined, unoptimized route.

**Full name from paper:** "Joint optimization algorithm with Subchannel allocation and User Association" — demonstrates performance without trajectory optimization.

**Performance gap vs. TDRA:** TDRA outperforms SAUA by an average of 178.57% in average achievable rate (varying UAV speed) — the largest gap against any baseline. SAUA's achievable rate does not change with UAV maximum flight speed (because trajectory is not optimized), confirming that trajectory optimization is the single most impactful component of TDRA.

---

## SEA (Sub-channel Equal Allocation)

**Definition:** A benchmark algorithm that jointly optimizes UAV trajectory and vehicle association but allocates subchannels equally among vehicles, without optimizing subchannel function selection.

**Full name from paper:** "Algorithm with Sub-channel Equal Allocation" — demonstrates performance without subchannel allocation optimization.

**Performance gap vs. TDRA:** TDRA outperforms SEA by 87.01-109.92% depending on the parameter varied. This demonstrates that intelligent subchannel allocation and function selection (balancing communication vs. sensing subchannels per vehicle per time slot) is a critical optimization lever.

---

## Average Achievable Rate

**Definition:** The primary performance metric of this paper — the time-averaged total downlink communication rate across all vehicles:
  f(s) = (1/T) * sum_{t=1}^{T} TR^t
where TR^t = sum_v TR^t_v is the total communication rate for all vehicles at time slot t.

**Details:** TR^t_v is the achievable rate for vehicle v at time t, drawn from its associated edge node over subchannels designated for communication:
  R^{t,c}_{u,v} = cat^{t,c}_v * sccr^{t,c} * B_0 * log2(1 + h^{t,c}_{u,v} * P^t_u / N_0)

This metric captures sustained system throughput across the entire optimization horizon. It is maximized subject to sensing performance constraints (MI >= HMI), energy constraints, safety distance constraints, and minimum per-vehicle data rate constraints (TR^t_v >= TR^req_v).

---

## OFDM (Orthogonal Frequency-Division Multiplexing)

**Definition:** A multi-carrier modulation scheme that divides available bandwidth into multiple orthogonal subcarriers, enabling simultaneous transmission on all subcarriers with inter-symbol interference protection via a cyclic prefix.

**In this paper:** UAVs and GBSs use OFDM-based dual-function transmitters. The OFDM structure enables ISAC because different subcarriers can independently serve communication or sensing, and the Fourier transform of the target impulse response at each subcarrier frequency |Q(f^c)|^2 directly enters the MI formula, linking OFDM waveform design to radar sensing performance.

**Doppler compatibility:** At vehicle speeds (2 m/s) and UAV speeds (up to 12 m/s) in this simulation, Doppler spread remains within NR-V2X subcarrier spacing tolerances, so OFDM does not suffer severe inter-carrier interference. OTFS (Orthogonal Time Frequency Space modulation), which offers improved Doppler robustness, is identified as a future extension.

---

## Doppler Effect

**Definition:** The shift in observed signal frequency due to relative motion between transmitter and receiver.

**In this paper:** Both UAV mobility and vehicle mobility produce Doppler shifts. For the radar sensing function, vehicle velocity creates a frequency shift in the reflected OFDM signal, which is captured implicitly through the target impulse response q in the MI formula. The paper explicitly justifies OFDM use by noting that the Doppler spread in the considered scenario falls within the tolerance range of standardized OFDM-based NR-V2X and LTE-V2X systems.

---

## Interior-Point Method

**Definition:** A class of algorithms for solving convex optimization problems by following a path through the feasible region's interior using barrier functions and Newton's method.

**Properties:** Polynomial-time convergence; handles large-scale LP and convex QP/SOCP; robust to degenerate or unbounded cases; faster than simplex for large problems.

**In this paper:** After relaxation and SCA convexification, each subproblem is solved by the interior-point method. For vehicle association (P1.1): the logarithmic barrier method transforms inequality constraints into penalty terms, and the exterior penalty function handles the single-association equality constraint. For trajectory (P1.2) and subchannel allocation (P1.3): the SCA-convexified problems are directly solved by the interior-point method.

---

## Random Rounding

**Definition:** A technique for converting a relaxed continuous solution of a binary (0-1) integer program into a binary feasible solution by treating each relaxed value as the probability of that variable being 1.

**Formula:** P[psi = 1] = psi_tilde (relaxed value from continuous solution)

**In this paper:** After solving the linear relaxations of P1.1 and P1.3, the optimal relaxed solutions (cb* and ca*, sccr*) are converted to binary using random rounding. This introduces a small suboptimality gap relative to the continuous relaxation but maintains feasibility with respect to the binary constraints.

---

## Quasi-Static Channel Model

**Definition:** A modeling assumption that channel conditions (vehicle positions, path loss) remain approximately constant within a time slot but change between time slots.

**In this paper:** Each time slot has duration tau = 1 second. Within each slot, all nodes are modeled as stationary. Across slots, vehicle positions update at constant velocity (2 m/s). This is standard in UAV-V2X optimization literature and is justified because movement within 1 second is small relative to the communication and sensing timescales being optimized.

---

## Subchannel Function Selection

**Definition:** The binary decision (per subchannel, per time slot) of whether a subchannel is used for downlink data communication or for radar sensing.

**Variable:** sccr = {sccr^{t,c}} where sccr^{t,c} = 1 means subchannel c serves communication at time t (contributing to vehicle data rate), and sccr^{t,c} = 0 means it serves radar sensing (contributing to vehicle sensing MI).

**Significance:** Subchannel function selection is how the sensing-communication tradeoff is operationalized. It enables fine-grained control over spectrum partitioning between the two ISAC functions on a per-subchannel, per-time-slot basis. This variable is jointly optimized with subchannel allocation ca in subproblem P1.3.

---

## Free-Space Path Loss

**Definition:** The attenuation of signal power in a free-space propagation environment, proportional to distance squared.

**Formula (UAV-vehicle link):**
  h^{t,c}_{u,v} = h^c_{u,v} / (d^t_{u,v})^2

where d^t_{u,v} = sqrt(||CU^t_u - CO^t_v||^2 + H_u^2) is the 3D Euclidean distance (incorporating fixed altitude H_u = 8 m), and h^c_{u,v} is the reference channel gain at 1 m on subchannel c (set to -60 dB in simulations). For GBS-vehicle links, a general path loss exponent a >= 0 is used: h^{t,c}_{r,v} = h^c_{r,v} / (d^t_{r,v})^a.

**Justification:** Free-space path loss is appropriate for LoS-dominant low-altitude UAV-to-ground links in open-road scenarios. The paper notes that more complex models (Rician, shadowing, altitude-dependent effects) can be incorporated in future extensions.

---

## Cramer-Rao Bound (CRB)

**Definition:** A theoretical lower bound on the variance of any unbiased estimator of a deterministic parameter, quantifying the best achievable radar estimation accuracy.

**Connection to MI:** Higher MI corresponds to a tighter CRB (lower estimation variance for target range, velocity, and angle of arrival). The paper cites this to justify MI as the sensing performance metric: maximizing MI effectively minimizes fundamental estimation uncertainty. The connection is established through the Gaussian echo model and the monotonic relationship between MI and KL-divergence between target-present and target-absent hypotheses.

---

## Lagrangian / Barrier Function

**Definition:** Mathematical tools that incorporate constraints into the objective function through penalty or barrier terms, converting constrained optimization into unconstrained optimization.

**Logarithmic barrier function (used in P1.1):**
  F1C(cb) = -sum log(TR^t_v - TR^req_v) - sum log(TMI^t_v - HMI^t_v) - sum log(1 - cb^t_{v,r}) - sum log(cb^t_{v,r}) - ...

The log barrier penalizes solutions approaching constraint boundaries (preventing infeasibility). The exterior penalty function (quadratic penalty on equality constraint violations) handles the single-association constraint (sum = 1). Together they enable Newton's method to solve the resulting unconstrained problem. The penalty parameter mu is decreased across outer iterations (multiplied by a decrement coefficient lambda in (0,1)) to tighten the barrier approximation.

---

## Time-Slot Framework

**Definition:** The discretization of the optimization horizon into T equal-length periods (slots), within each of which resource allocation decisions are made and held constant.

**In this paper:** T = 30 time slots, tau = 1 s/slot (total optimization horizon = 30 s). Within each slot t, vehicle association cb^t, subchannel allocation ca^t, and subchannel function selection sccr^t are optimized. UAV trajectory positions CU^t are also updated per slot (step size bounded by max_speed * tau). The total achievable rate is averaged over all T slots to compute the objective f(s) = (1/T) * sum_t TR^t.
