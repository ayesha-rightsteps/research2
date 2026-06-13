# Terminology: ISAC-Enabled UAV Vehicular Network with BCD-SCA

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai, plus full math detail for depth.

---

## ISAC (Full form: Integrated Sensing and Communication) ⭐

**What it is (from scratch):** Tumhara phone ek "communication" device hai — yeh data (calls, messages, internet) bhejta/receive karta hai radio waves se. Ab socho ek **bat (chamgadar)** ko — woh apni awaaz nikalti hai, aur jab woh awaaz kisi cheez se "takra kar wapas aati hai" (echo), bat ko pata chal jaata hai ki uske aage kya hai, kitni door hai — isi ko **radar/sensing** kehte hain. Ab socho agar ek hi device ek hi signal se **dono kaam kar sake** — data bhi bhejo, aur usi signal ki "echo" se yeh bhi pata karo ki aas-paas kya hai — to woh ek signal "double duty" kar raha hai. Isi cheez ko **ISAC (Integrated Sensing and Communication)** kehte hain — "Integrated" matlab "jode hue", yaani sensing aur communication ek hi system mein mile hue hain.

**What it is (Definition):** A technology that enables a single hardware platform and waveform to simultaneously perform wireless data communication and radar sensing, sharing the same spectrum, antennas, and RF components.

**Details:** Traditional systems mein radar aur communication poori tarah alag hote hain — alag hardware, alag antenna, alag spectrum. ISAC unhe fuse kar deta hai by transmitting OFDM signals (OFDM ka matlab neeche dekho) jo ek saath gaadiyon dwara decode bhi ho jaate hain (communication function), aur transmitter ki taraf reflect/wapas bhi aa jaate hain target detection ke liye (radar function). Isse spectrum duplication aur hardware redundancy dono khatam ho jaate hain.

**In this paper:** UAVs aur GBSs dono OFDM-based dual-function transmitters se equipped hain. Har subchannel ko communication (sccr^{t,c} = 1) ya radar sensing (sccr^{t,c} = 0) ke liye designate kiya ja sakta hai. Radar sensing function gaadi-targets se reflections process karta hai unki scattering characteristics estimate karne ke liye, jise Mutual Information se quantify kiya jaata hai. ISAC se gaadiyon ko ek hi infrastructure se data service aur beyond-line-of-sight (BLOS) perception support ek saath milta hai.

**Real-life analogy:** Bat ki echolocation socho — woh ek hi "chirp" (awaaz) se apne saathiyon ko message bhi de sakti hai (communication) aur uski echo se yeh bhi jaan sakti hai ki aage deewar hai ya khula raasta (sensing). Ek hi signal, do kaam — yehi ISAC hai.

**Why it matters for 6G:** ISAC 6G architecture ka ek key pillar hai, jisse communication infrastructure khud sensing infrastructure bhi ban jaata hai — autonomous driving, smart cities, environmental monitoring jaisi applications ke liye.

**Easy way to remember:** ISAC = "ek signal, do jobs — baat bhi karo, dekho bhi."

---

## BCD (Full form: Block Coordinate Descent) ⭐

**What it is (from scratch):** Socho tumhe apna kamra clean karna hai — kapde fold karna, table saaf karna, aur floor jhaadu lagana — teen kaam ek saath. Agar tum teeno ko ek hi waqt mein, mix karke karne ki koshish karo, toh confusion ho jaayega. Isके bajaye, tum ek-ek kaam karti ho: pehle kapde fold karo (table aur floor ko temporarily "as-is" chhod do), phir table saaf karo (kapdon ko touch nahi karna — woh "fixed/frozen" hain), phir floor jhaadu lagao. Iske baad zaroori ho toh wapas pehle kaam par jao aur dobara thoda adjust karo. **BCD** isi idea ko ek math problem par apply karta hai — jab ek bade problem mein bahut saari "decisions" (variables) ek saath optimize karni ho, toh unhe groups ("blocks") mein baant do, aur ek time pe sirf ek block ko optimize karo, baaki sab blocks ko temporarily "fix"/freeze rakho. Phir next block pe jao. Yeh cycle repeat hota hai jab tak sab "settle" (converge) na ho jaaye.

**What it is (Definition):** An iterative optimization algorithm that divides the optimization variables into blocks and updates one block at a time while holding the others fixed, cycling through all blocks until convergence.

**The Math (if you're curious):** Given a joint optimization problem over variables {x_1, x_2, x_3}, BCD solves:
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

**Real-life analogy:** Ek saath teen kaam karne ke bajaye, ek-ek karke karna — kapde fold karo (baaki freeze), table saaf karo (baaki freeze), floor jhaadu lagao (baaki freeze), repeat karo jab tak kamra "settled" na lage.

**Easy way to remember:** BCD = "ek baari mein ek hi tukda solve karo, baaki sab ko abhi ke liye 'pause' kar do."

---

## SCA (Full form: Successive Convex Approximation) ⭐

**What it is (from scratch):** Pehle samjho **convex** kya hai — ek convex problem ka "shape" ek katore (bowl) jaisa hota hai. Agar tum bowl ke kisi bhi point se shuru karo aur "neeche ki taraf" chalna shuru karo, tum hamesha sabse neeche (best/lowest point) tak pahunch jaaogi, kyunki sirf ek hi sabse neecha point hota hai — koi confusion nahi. Lekin **non-convex** problem ka shape bahut saari pahadiyon-aur-valleys (ups-and-downs) jaisa hai — agar tum "neeche jaate raho" wala simple tareeka use karo, tum kisi chhoti valley mein phans sakti ho jo asal mein sabse neechi jagah nahi hai (sirf "local" neechi jagah hai, "global" sabse neechi nahi). **SCA** ek tareeka hai non-convex (pahadi-jaisi) problems ko solve karne ka: tum jahan abhi khadi ho (current point), us point ke bilkul aas-paas wale chhote se area ko "seedha/flat, bowl-jaisa" maan lo (yeh approximation hai — exact nahi, par paas-paas mein sahi hai). Phir us simple "flat/bowl" version ko aasaani se solve karo — tumhe ek naya, behtar point milega. Ab wahan se phir ek nayi "flat" approximation banao, phir solve karo — aise repeat karte raho. Dheere-dheere tum sahi (ya bahut paas wale) answer tak pahunch jaaogi.

**What it is (Definition):** An iterative method for solving non-convex optimization problems by locally approximating non-convex functions with convex surrogates (typically via first-order Taylor expansion) at each iteration, then solving the resulting convex problem.

**The Math (if you're curious):** At iteration k, a non-convex function f(x) is approximated by its first-order Taylor expansion around the current point x_k:

  f_approx(x; x_k) = f(x_k) + gradient_f(x_k)^T * (x - x_k)

For a convex f, this approximation is a global lower bound. Each SCA step produces a feasible point that improves the original objective. The approximation error is bounded by (L/2) * ||x - x_k||^2 (L = Lipschitz constant of the gradient), which vanishes as x approaches x_k at convergence. SCA converts non-convex problems into a sequence of convex problems solvable by standard tools such as the interior-point method.

**In this paper:**
- In P1.2 (trajectory planning): The non-convex log term in the communication rate (as a function of UAV-vehicle distance squared) is approximated by its Taylor lower bound. The non-convex inter-UAV safety constraint is also linearized via Taylor expansion.
- In P1.3 (subchannel allocation): The non-convex product cat * sccr (both binary variables) is approximated using the identity a*b = ((a+b)^2 - a^2 - b^2)/2 and Taylor expansion on the quadratic terms to obtain a linear constraint.

**Real-life analogy:** Andhere mein ek tedhe-medhe pahadi raaste par chalna — tum poora raasta nahi dekh sakti, lekin apne paas ka chhota area "seedha/flat" maan kar ek safe step le sakti ho. Step lene ke baad, naye point se phir "flat" maan kar agla step — aise-aise sahi manzil ki taraf badhti jaati ho.

**Easy way to remember:** SCA = "tedhi cheez ko abhi ke liye seedha maan lo, solve karo, phir update karo — repeat."

---

## MINLP (Full form: Mixed-Integer Nonlinear Programming) ⭐

**What it is (from scratch):** Optimization problems mein hum "best decision" dhundte hain. Kabhi-kabhi decisions sirf **"haan ya na"** (0 ya 1) type ki hoti hain — jaise "kya yeh subchannel sensing ke liye use ho ya nahi" — inhe **integer/binary variables** kehte hain. Aur kabhi decisions **continuous numbers** hoti hain — jaise "drone ki exact position (x, y coordinates)" — yeh koi bhi decimal value le sakti hai. Jab ek hi problem mein **dono types** (binary + continuous) decisions mile hue ho, AUR formulas mein "log", "square", ya do variables ka multiplication jaisi **nonlinear** (non-seedhi-line) cheezein bhi ho — toh us problem ko **MINLP** kehte hain. Yeh problems generally bahut hard hote hain — jaise-jaise size badhta hai, possible combinations explode ho jaate hain, aur "best" answer dhundna practically impossible ho jaata hai (isi ko "NP-hard" kehte hain).

**What it is (Definition):** An optimization problem class that contains both continuous and discrete (integer) decision variables, with nonlinear objective functions or constraints. MINLP is generally NP-hard; finding a global optimum is intractable in polynomial time for large-scale instances.

**The Math (if you're curious):** The intractability arises from the combination of integer combinatorics (exponential search space) and nonlinearity (non-convex constraints, multiplicative coupling between variables). Standard approaches include branch-and-bound, relaxation-and-rounding, and decomposition methods (BCD, Lagrangian relaxation).

**In this paper:** The optimization problem P1 is MINLP because:
- Integer variables: Vehicle association cb (binary), subchannel allocation ca (binary), subchannel function selection sccr (binary).
- Continuous variables: UAV trajectory CU (continuous horizontal positions).
- Nonlinear coupling: The communication rate contains terms like cat^{t,c}_v * sccr^{t,c} * log2(1 + h^{t,c}_{u,v} * P_u / N_0), where binary variables multiply a logarithmic function of channel gain that itself depends nonlinearly on UAV position through distance squared.

The BCD-SCA approach converts P1 into tractable subproblems, obtaining an approximate stationary-point solution rather than a global optimum.

**Real-life analogy:** Socho ek event plan kar rahi ho jisme kuch decisions "haan/na" hain (kaunse 5 dost aaoge — invite ya nahi) aur kuch decisions numbers hain (budget kitna rakhna hai, exact). Aur dono mil kar ek complicated formula bana rahe hain jisme "log" jaisi tedhi calculations hain. Itne saare combinations ki "sab try karke best dhundo" approach yahan bhi practically impossible hai.

**Easy way to remember:** MINLP = "haan/na decisions + exact-number decisions + tedhi (nonlinear) formula — sab mixed, isiliye hard."

---

## Mutual Information (MI) ⭐ (Full form: Mutual Information)

**What it is (from scratch):** Socho tum ek dost ko ek "signal" bhejti ho aur woh signal kisi cheez se reflect hokar wapas tumhare paas aata hai (jaise echo). Ab is "echo" mein kuch **information** hoti hai us cheez ke baare mein — uska size, distance, shape, etc. **Mutual Information (MI)** ek number hai jo batata hai: "is echo se mujhe us cheez ke baare mein kitni 'naya/useful' information mil rahi hai?" Jitna zyada MI, utna zyada confidently tum keh sakti ho "haan, woh cheez wahan hai, aur uske baare mein mujhe itna pata hai." Kam MI matlab echo bahut weak/confusing hai, aur tumhe us cheez ke baare mein zyada pata nahi chal raha.

**What it is (Definition):** An information-theoretic measure of the statistical dependence between two random variables. In ISAC sensing, MI between the target's impulse response (q) and the received reflected signal (z) given the transmitted waveform (s) quantifies how much information about the target is extractable from the radar echo.

**The Math (if you're curious):**
  MI^{t,c}_{u,v} = (1/2) * B_0 * S * T_s * log2(1 + P^t_u * S * T_s^2 * |Q^c_{u,v}(f)|^2 / sigma^c_{u,rad})

where B_0 is subchannel bandwidth, S is the number of consecutive OFDM symbols per sensing interval, T_s is OFDM symbol duration (including cyclic prefix), P^t_u is UAV transmit power, |Q^c_{u,v}(f)|^2 is the squared Fourier magnitude of the target impulse response at subcarrier frequency f^c, and sigma^c_{u,rad} is radar receiver noise power on subchannel c.

**Why MI is used:** MI is a unified surrogate for classical radar metrics. Under the Gaussian echo model:
- MI is monotonically related to the KL-divergence between target-present and target-absent hypotheses, which directly determines optimal detection probability (Pd) and false-alarm probability (Pfa) under the Neyman-Pearson framework.
- Higher MI implies a tighter Cramer-Rao Bound (lower parameter estimation variance for target range, velocity, and angle).

MI is preferred over Pd/Pfa or CRB in this paper because it is analytically tractable and directly integrable into the convex optimization framework. A minimum MI threshold HMI (200-1000 bits in experiments) is enforced as a sensing performance constraint for each vehicle at each time slot.

**Sensing-communication tradeoff:** Higher MI requires more subchannels allocated for sensing (sccr = 0), directly reducing the subchannels available for communication (sccr = 1) and lowering throughput. This is the core tension modeled throughout the paper.

**Real-life analogy:** Andhere kamre mein ek torch jala kar deewar par chamkaayi — agar reflection clear hai (bright spot dikh raha hai), tumhe pata chal jaata hai "wahan deewar hai, itni door hai" — yeh high MI hai. Agar reflection bahut faint/blurry hai, tumhe confidently kuch nahi pata chalta — yeh low MI hai. MI basically "echo se mili clarity" ka measure hai.

**Easy way to remember:** MI = "echo se mili 'kitna pata chala' ki score" — jitna zyada MI, utna behtar radar/sensing.

---

## UAV (Full form: Unmanned Aerial Vehicle) ⭐

**What it is (from scratch):** **UAV** ek aircraft hai jisme koi pilot andar baith ke nahi udata — yeh remotely (door se) ya khud-ba-khud (autonomously) udta hai. Roz-marra ki language mein, isi ko "drone" kehte hain. Yeh chhota bhi ho sakta hai (jaise camera-drone) aur bada bhi.

**What it is (Definition):** An aircraft that operates without a human pilot onboard, controlled remotely or autonomously.

**In this paper:** Two UAVs (U = 2) are deployed in the simulation. Each UAV is equipped with an OFDM-based dual-function ISAC transmitter and flies at a fixed altitude of 8 m above the highway. UAVs take off from preset starting points, serve vehicle users during T = 30 time slots (tau = 1 s/slot), and must land at preset endpoints. UAV horizontal trajectory is the primary optimization variable in subproblem P1.2. Maximum flight speed is 6-12 m/s in experiments; onboard energy budget is 500 J; energy consumption is 2 J per meter flown.

**Altitude choice (8 m):** Follows common practice for low-altitude UAV sensing/communication studies (5-30 m above roadways). Provides high LOS probability, reduces path loss, gives unobstructed coverage over vehicles (below 4-5 m height), and complies with FAA Part 107 VLOS regulations (below 120 m in controlled areas).

**Real-life analogy:** Jaise koi delivery-drone jo bina pilot ke, apne aap ek raaste par udta hai aur cheezein deliver karta hai — yahan UAV "deliver" karta hai signal/coverage, gaadiyon tak.

**Easy way to remember:** UAV = "drone" — udne wala device jisme koi insaan andar nahi baitha.

---

## GBS (Full form: Ground Base Station) ⭐

**What it is (from scratch):** Tumne mobile phone ke towers dekhe honge road ke kinare ya buildings par lage hue — yeh towers tumhare phone ko signal dete hain. Isi tower ko technical language mein **Base Station** kehte hain. **GBS (Ground Base Station)** matlab woh base station jo zameen par fix hai (UAV ki tarah udta nahi) — yeh ek jagah par permanently khada rehta hai aur apne aas-paas ki gaadiyon/devices ko coverage deta hai.

**What it is (Definition):** A fixed wireless communication base station located on the ground, providing coverage to vehicles within its service area.

**In this paper:** One GBS (R = 1) is randomly deployed along the roadside. Like UAVs, the GBS is equipped with an ISAC dual-function transmitter that can simultaneously communicate with and sense vehicles. The GBS position is fixed (not optimized); vehicle association to the GBS is an optimization variable. GBS transmit power is fixed at Pt_r = 3 W. The cooperative sensing between UAVs and GBS provides multi-angle radar coverage of vehicles, improving sensing completeness and enabling beyond-line-of-sight perception.

**Real-life analogy:** Jaise tumhare ghar ka WiFi router — woh ek hi jagah par fix hai, aur jo bhi uske range mein aaye usko signal deta hai. GBS bhi waisa hi hai, sirf bahut bada scale par aur gaadiyon ke liye.

**Easy way to remember:** GBS = "zameen par khada hua fix tower" (UAV ke ulta — woh udta hai, GBS nahi).

---

## Subchannel Allocation ⭐

**What it is (from scratch):** Radio signals ek bohot bada "frequency range" (spectrum) cover karte hain — jaise FM radio mein 88 MHz se 108 MHz tak alag-alag stations hain. Network engineers is bade spectrum ko chhote-chhote, equal-size "tukdon" mein todte hain — har tukde ko **subchannel** kehte hain. Ek bade cake ko equal slices mein katne jaisa socho — har slice ek subchannel hai. Ab **Subchannel Allocation** ka matlab hai: decide karna ki har slice (subchannel) kis gaadi ko diya jaaye — taaki sab gaadiyon ko apna data bhejne/lene ke liye spectrum mil sake, bina ek-doosre se "takraye" (interfere kiye).

**What it is (Definition):** The assignment of frequency subchannels (discrete spectrum units) to users for data transmission or sensing.

**The Math (if you're curious):** The available spectrum is divided into C orthogonal subchannels, each with bandwidth B_0. Each subchannel can be assigned to only one vehicle at a time (exclusive allocation: sum_v cat^{t,c}_v = 1 for each subchannel c at each time slot t). The allocation decision ca = {cat^{t,c}_v} is binary. C ranges from 30 to 60 in experiments; B_0 = 1-3 MHz.

**Optimization role:** Subchannel allocation determines which vehicles receive spectrum resources. Together with subchannel function selection, it controls whether those resources serve communication (increasing data rate) or sensing (increasing radar MI). As the number of subchannels increases, the proposed algorithm achieves a 19.75% improvement over URA, demonstrating the value of intelligent allocation.

**Real-life analogy:** Ek pizza ko slices mein katna, aur decide karna kaun-kaunsa slice kisko milega — kisi ko ek slice mil sakta hai, kisi ko zyada — depend karta hai unki "need" par.

**Easy way to remember:** Subchannel = "spectrum ka ek slice"; Allocation = "kaunsa slice kisko mile" decide karna.

---

## Vehicle Association ⭐

**What it is (from scratch):** Jab tumhara phone WiFi ya mobile-data use karta hai, woh kisi ek specific tower/router se "connect" hota hai (jismein se signal lega). **Vehicle Association** ka matlab hai — har gaadi ke liye decide karna ki woh kis "edge node" (yaani GBS ya kis UAV) se connect hogi, apna data/sensing service lene ke liye. Yeh decision important hai kyunki gaadi jis node se jitni paas hai, uska signal usually utna strong hoga.

**What it is (Definition):** The assignment of each vehicle to a specific edge node (UAV or GBS) that will serve it for communication and sensing in a given time slot.

**The Math (if you're curious):** Each vehicle must associate with exactly one UAV or one GBS per time slot (constraint: sum_r cb^t_{r,v} + sum_u cb^t_{u,v} = 1 for all vehicles v). The association decision cb = {cb^t_{v,r}, cb^t_{v,u}} is binary.

**Optimization role:** Vehicle association determines the channel gain experienced, because path loss depends on distance to the associated node. Associating a vehicle with the nearest UAV or GBS generally improves both communication rate and sensing MI. Subproblem P1.1 is solved by linear relaxation, logarithmic barrier method, exterior penalty function, Newton's method, and random rounding.

**Real-life analogy:** Jaise tum decide karti ho ki tumhara phone ghar ke WiFi se connect ho ya mobile data (tower) se — depend karta hai kaunsa zyada strong/useful hai abhi. Vehicle association bhi har gaadi ke liye yehi decision hai, GBS vs UAV(s) ke beech.

**Easy way to remember:** Association = "kaun kisse connect hoga" — gaadi ka "partner" (GBS ya UAV) decide karna.

---

## Trajectory Planning (UAV) ⭐

**What it is (from scratch):** **Trajectory** ka matlab hai "raasta" ya "path" — jaise tum ghar se college jaane ka raasta plan karti ho. **UAV Trajectory Planning** ka matlab hai — drone apni flight ke har time-slot mein kahan-kahan (kis position par) hoga, yeh decide karna. Yeh sirf "udo aur ghoomo" nahi hai — drone ka exact raasta carefully choose karna padta hai taaki woh gaadiyon ke jitna possible "paas" rahe (jisse signal strong rahega, jaise tum apne dost se door se chillane ke bajaye paas jaa kar baat karo toh awaaz clear sunayi degi), lekin energy bhi limited hai aur safety rules bhi follow karne hain (jaise do drones ek-doosre se bahut paas na udein).

**What it is (Definition):** The optimization of a UAV's flight path over time to achieve mission objectives — here: maximize average achievable communication rate under energy, safety, and sensing constraints.

**The Math (if you're curious):** The UAV trajectory is a sequence of 2D horizontal positions {CU^t_u = (x^t_u, y^t_u)} for t = 1, ..., T, subject to: speed constraint (step distance bounded by max_speed * tau), start/end point constraint, energy consumption constraint (total flight energy below 500 J), and inter-UAV safety distance constraint (minimum 8 m between UAVs). Altitude is fixed at 8 m; optimization is on the horizontal plane.

**Key result from experiments:** After trajectory optimization, UAVs fly closer to vehicle lanes rather than along predetermined straight-line routes. This reduces UAV-to-vehicle distance, lowering path loss and increasing both communication rate and sensing MI. TDRA outperforms SAUA (no trajectory optimization) by 178.57% — the largest gain over any baseline — showing trajectory optimization is the most critical component of the joint framework.

**Real-life analogy:** Ek delivery-person jo fix route follow karne ke bajaye, real-time mein dekh kar decide kare ki "abhi sabse zyada orders kahan hain, wahan jaata hoon" — uska delivery time bahut better hoga fix-route walay se. Drone ka smart trajectory bhi waise hi "jahan zyada zaroorat hai, wahan jaana" hai.

**Easy way to remember:** Trajectory = "drone ka GPS-route, jo waqt ke saath khud hi best banaya gaya hai" (fix raaste ki jagah).

---

## Radar Sensing Constraint ⭐

**What it is (from scratch):** **Constraint** ka matlab hai "rule/limit" jo tum cross nahi kar sakti. Yahan **Radar Sensing Constraint** ka matlab hai: chahe optimization kuch bhi decide kare (drone kahan jaaye, kaunsi gaadi kis se connect ho), ek "minimum rule" hamesha follow hona chahiye — har gaadi ko har waqt kam se kam itni "sensing quality" (MI) milni chahiye. Yeh ek "safety floor" jaisa hai — communication ko jitna bhi maximize karo, sensing ko itna kam nahi hone dena ki gaadi "blind" ho jaaye.

**What it is (Definition):** A constraint in the optimization problem that enforces a minimum level of radar sensing quality, expressed as a Mutual Information threshold, for each vehicle at each time slot.

**The Math (if you're curious):**
  TMI^t_v >= HMI, for all vehicles v in VE and time slots t in TI

where TMI^t_v is the total MI obtained for sensing vehicle v at time t (summed over all sensing-designated subchannels from both the associated UAV and the GBS sensing vehicle v), and HMI is the minimum MI threshold (200-1000 bits in experiments).

**Effect on optimization:** Higher HMI forces more subchannels to be designated for sensing (sccr = 0), reducing the communication bandwidth available and lowering average achievable rate. As HMI increases from 200 to 1000 bits, average achievable rate decreases monotonically (Figure 8) — the quantitative expression of the sensing-communication tradeoff.

**Real-life analogy:** Jaise ek car mein "minimum fuel reserve" rule — chahe tum kahin bhi jao, tank itna empty nahi hone dena ki car beech raaste mein ruk jaaye. Yahan, MI ka "tank" har gaadi ke liye ek minimum level se neeche nahi jaana chahiye.

**Easy way to remember:** Radar Sensing Constraint = "har gaadi ke liye minimum 'radar-vision' guarantee — isse kam nahi chalega."

---

## Rician Channel

**What it is (from scratch):** Jab koi signal transmitter se receiver tak jaata hai, woh kabhi seedha-seedha (direct path se) pahunchta hai, aur kabhi raaste mein cheezon se reflect/bounce hokar bhi pahunchta hai (alag-alag "copies" ban jaati hain, jo thodi der se pahunchti hain). **Rician Channel** ek mathematical model hai jo dono types ko mix karta hai — ek "strong direct path" (jaisे tum kisi se seedha aankh-milaa kar baat karo) plus kuch "weaker bounced paths" (jaise unki awaaz deewar se bhi thodi reflect ho kar tum tak aaye). Yeh model batata hai ki overall signal kaisa "behave" karega jab dono mix ho.

**What it is (Definition):** A statistical channel model capturing the combination of a dominant LOS component and multiple weaker scattered NLoS components, with the amplitude following a Rice distribution parameterized by the K-factor.

**Details:** As K approaches 0, Rician becomes Rayleigh fading (pure scattering); as K approaches infinity, it becomes pure LOS. Rician channels are typical for low-to-medium altitude UAV-to-ground links due to high LOS probability.

**Relevance to this paper:** The paper adopts a simplified free-space path loss model (LoS-dominant for the low-altitude simulation scenario), but acknowledges incorporating Rician fading, shadowing, altitude-dependent fading, and Doppler variation as directions for future work.

**Real-life analogy:** Ek bade khaali hall mein kisi se baat karna — tumhe unki awaaz seedhi bhi sunayi degi (direct/LOS), aur deewaron se reflect hokar "echo" jaisi bhi thodi sunayi degi (scattered/NLoS) — Rician model dono ko ek saath account karta hai.

**Easy way to remember:** Rician = "direct signal + halka-sa echo, dono mile hue."

---

## LoS (Full form: Line-of-Sight)

**What it is (from scratch):** **Line-of-Sight (LoS)** ka matlab hai — transmitter aur receiver ke beech ek seedha, bina-rukaawat (obstruction-free) raasta hai. Jaise tum apne dost ko kholay maidan mein dekh sakti ho, beech mein koi deewar/building nahi hai. Wireless signals ke liye, LoS hone se signal strong aur clear rehta hai.

**What it is (Definition):** A propagation path between transmitter and receiver where there is a direct, unobstructed signal path.

**The Math (if you're curious):** At 8 m UAV altitude with vehicles below 4-5 m, LoS is the dominant propagation mode. Channel gain model: h^{t,c}_{u,v} = h^c_{u,v} / (d^t_{u,v})^2, reflecting free-space (LoS) path loss. LoS paths improve both communication rate (higher received signal power) and radar sensing MI (stronger echo return). UAV trajectory optimization effectively maximizes LoS conditions by minimizing UAV-vehicle distance.

**Real-life analogy:** Jaise tum apne dost se ek khuli maidan mein seedha baat kar rahi ho — kuch bhi beech mein nahi aata, awaaz clear pahunchti hai. Drone agar gaadiyon ke "seedhe upar" ya bahut paas hai, toh signal ke beech mein kuch nahi aata — yeh LoS hai.

**Easy way to remember:** LoS = "seedha rasta, kuch beech mein nahi" — signal ke liye best condition.

---

## NLoS (Full form: Non-Line-of-Sight)

**What it is (from scratch):** **NLoS** LoS ka opposite hai — transmitter aur receiver ke beech ka seedha raasta kisi cheez (building, tree, doosri gaadi) se block ho gaya hai. Toh signal ko "ghoom kar" (reflect, bend, ya scatter ho kar) pahunchna padta hai, jisse signal weak aur delayed ho jaata hai.

**What it is (Definition):** A propagation condition where the direct path between transmitter and receiver is blocked by obstacles, requiring signals to travel via reflections, diffraction, or scattering.

**In vehicular networks:** Buildings, trees, and other vehicles create NLoS conditions that add attenuation, delay spread, and multipath fading beyond free-space path loss. The current paper model assumes LoS-dominant conditions. The paper explicitly acknowledges that incorporating NLoS channel models (including urban blockage effects) is left for future work.

**Real-life analogy:** Jaise tum apne dost se baat kar rahi ho lekin beech mein ek bada truck khada hai — tumhari awaaz unhe seedhi nahi, balki truck ke around se "ghoom kar" (zyada faint/echo ke saath) pahunchegi.

**Easy way to remember:** NLoS = "rasta block hai, signal ko ghoom kar jaana padta hai" — weaker signal.

---

## Convex Optimization ⭐

**What it is (from scratch):** Ek **convex** problem ka shape ek katore (bowl) jaisa hota hai — agar tum kisi bhi point se "neeche ki taraf" chalna shuru karo, tum hamesha sabse neeche (best point, jise "minimum" kehte hain) tak hi pahunchogi, kyunki bowl mein sirf ek hi sabse neecha point hai. Yeh property bahut powerful hai kyunki isse computer ko "best answer" dhundne ke liye sirf "neeche jaate raho" jaisa simple, fast tareeka chal jaata hai — bina poora bowl explore kiye. **Convex Optimization** in problems ko solve karne ki techniques ka group hai.

**What it is (Definition):** A subclass of mathematical optimization where both the objective function and the feasible set are convex. Any local minimum is also a global minimum, enabling efficient polynomial-time algorithms.

**The Math (if you're curious) — Key tools used in this paper:**
- Interior-point method: Solves convex LP and convex QP/SOCP problems by searching through the feasible region interior using barrier functions and Newton's method.
- Logarithmic barrier method: Converts inequality constraints into log-penalty terms in the objective, enabling unconstrained optimization via Newton's method.
- Exterior penalty function: Converts equality constraints into quadratic penalty terms.

After BCD decomposition and SCA convexification, each subproblem (P1.1.2, P1.2.2, P1.3.2) is a convex problem solvable by the interior-point method.

**Real-life analogy:** Ek bowl mein marble (kanchaa) daalo — chahe kahan se bhi daalo, woh hamesha bowl ke sabse neeche, bilkul center mein aakar ruk jaayega. Yeh "guaranteed best answer milna" hi convex optimization ki taqat hai.

**Easy way to remember:** Convex = "bowl-shape, ek hi best point, dhundna easy."

---

## SCA Approximation (Full form: Successive Convex Approximation — Taylor-based step)

**What it is (from scratch):** Yeh **SCA** (upar dekho) ka ek specific "tool" hai. Jab koi formula tedha/curvy (non-convex) ho, toh us formula ko current point ke "bahut paas" ek **seedhi line** se approximate kar diya jaata hai — isi seedhi-line-approximation ko **Taylor expansion** kehte hain. Socho tum ek curve ko zoom-in karke dekho — agar tum bahut zoom-in karo, woh curve bhi local area mein almost "seedhi line" jaisi lagne lagti hai. SCA Approximation isi "zoomed-in seedhi line" ko use karta hai taaki tedha formula temporarily "seedha/convex" ban jaaye aur solve kiya ja sake.

**What it is (Definition):** The specific technique within SCA — replacing a non-convex function with its first-order Taylor expansion (a linear, hence convex, lower bound) at the current iterate.

**The Math (if you're curious):** For a convex and continuously differentiable function f(x), the first-order Taylor approximation at x_k gives: f_approx(x; x_k) = f(x_k) + gradient_f(x_k)^T * (x - x_k) <= f(x). This is a global lower bound. The approximation error satisfies: 0 <= f(x) - f_approx(x; x_k) <= (L/2) * ||x - x_k||^2, where L is the Lipschitz constant of the gradient. Near convergence (where ||x - x_k||^2 becomes small), the error vanishes.

**In P1.2 (trajectory):** The log rate term delta^{t,c}_{v,u} = log2(1 + h * P / (||CU - CO||^2 + H^2)) is concave in the distance squared. Its Taylor expansion around the current iterate provides a convex lower-bound constraint.

**In P1.3 (subchannel):** The product cat * sccr is approximated using the quadratic identity and Taylor expansion of the squared sum and individual squares, yielding a linear constraint.

**Real-life analogy:** Ek gol ghoomti hui sadak ko zoom-in karke dekho — chhote se hisse mein woh seedhi lagti hai. Driving karte waqt tum thoda-thoda "seedha maan kar" steer karte ho, phir update karte ho — yehi Taylor-based approximation hai.

**Easy way to remember:** SCA Approximation = "curve ko zoom-in karke seedha (Taylor line) maan lena, step-by-step."

---

## ITS (Full form: Intelligent Transportation Systems)

**What it is (from scratch):** **ITS** ek bada umbrella-term hai jo sab "smart" transportation technologies ko cover karta hai — traffic signals jo khud adjust hote hain, gaadiyan jo ek-doosre se baat karti hain (V2X), self-driving cars, accident-detection systems, etc. Basically, "transportation ko technology se safe aur efficient banane ka pura ecosystem."

**What it is (Definition):** An integrated application of advanced sensing, communication, computing, and control technologies to transportation infrastructure and vehicles to improve safety, efficiency, and sustainability.

**Scope:** ITS encompasses traffic signal control, V2X communication, autonomous driving, incident detection and management, and cooperative adaptive cruise control (CACC).

**In this paper:** Temporary traffic congestion is the primary use case — a sudden event (accident, roadwork, peak hour) causes a localized surge in communication and sensing demand that overwhelms fixed GBS capacity. UAV-ISAC provides rapid, flexible, supplementary coverage that alleviates both the communication burden and the sensing blind spots, directly supporting ITS safety applications through BLOS perception.

**Real-life analogy:** Jaise ek city ka "smart traffic control room" jo cameras, sensors, aur signals sab mil kar manage karta hai taaki traffic smooth chale aur accidents kam ho — ITS isi "smart system" ka naam hai.

**Easy way to remember:** ITS = "transportation ka smart/tech-enabled ecosystem" — isi ke andar yeh paper ka UAV-ISAC use-case aata hai.

---

## Sensing-Communication Tradeoff ⭐

**What it is (from scratch):** **Tradeoff** ka matlab hai — "ek cheez zyada chahiye toh dusri cheez kam karni padegi." Yahan, ISAC system ke paas limited spectrum (resource) hai jo sensing aur communication dono mein baatna padta hai. Agar zyada spectrum sensing ko do (taaki radar accha kaam kare), toh communication ke liye kam bachega (data slow ho jaayega). Agar zyada spectrum communication ko do (fast data), toh sensing weak ho jaayega (gaadi "blind" reh sakti hai). Yeh ek fundamental "tension" hai jo poore paper mein har jagah dikhta hai.

**What it is (Definition):** The fundamental tension in ISAC systems between dedicating spectrum resources to communication versus sensing. Improving one function typically degrades the other.

**The Math (if you're curious) — Mechanism in this paper:** Each subchannel can serve communication (sccr = 1: contributes to vehicle data rate) or sensing (sccr = 0: contributes to radar MI). The optimization problem P1 balances this by maximizing average achievable communication rate (objective) while satisfying minimum sensing MI constraints (TMI^t_v >= HMI). As HMI increases, more subchannels must be allocated to sensing, leaving fewer for communication.

**Quantitative evidence:** Figure 8 shows average achievable rate decreasing monotonically as HMI increases from 200 to 1000 bits across all compared algorithms. The proposed TDRA algorithm consistently outperforms all baselines even under tight sensing constraints.

**Real-life analogy:** Ek limited budget mein, agar tum zyada paisa "savings" mein daalo, toh "spending" ke liye kam bachega — aur vice versa. Yahan "budget" = spectrum, "savings" = sensing, "spending" = communication.

**Easy way to remember:** Sensing-Communication Tradeoff = "ek cheez ko zyada do, dusri ko kam milega — dono ko balance karna hai."

---

## URA (Full form: User association Random Assignment)

**What it is (from scratch):** Yeh paper ka ek **baseline** (comparison ke liye banaya gaya simpler version) hai — matlab "agar hum smart vehicle-association use na karein, sirf random assign karein, toh kya hota hai?" yeh check karne ke liye. Trajectory aur subchannel allocation toh optimize hote hain, par "kaun kis se connect hoga" — yeh random decide hota hai.

**What it is (Definition):** A benchmark algorithm that optimizes UAV trajectory and subchannel allocation but assigns vehicle-to-edge-node associations randomly, without optimization.

**Full name from paper:** "Algorithm with User association Random Assignment" — the UAV maximizes downlink rate by optimizing trajectory and subchannel allocation, but vehicle association is random.

**Performance gap vs. TDRA:** TDRA outperforms URA by an average of 32.42% in average achievable rate (varying UAV speed). This gap isolates the contribution of vehicle association optimization to the joint optimization framework.

**Real-life analogy:** Jaise ek restaurant mein, agar tables customers ko **random** assign kiye jaayein (bina dekhe kaun-si table kis customer ke liye sabse paas/best hai) — overall experience thoda kam ho jaayega, chahe khaana (trajectory) aur menu (subchannel) achha ho.

**Easy way to remember:** URA = "association random hai, baaki sab optimize hai" — yeh batata hai association optimize karne se kitna fayda hota hai.

---

## SAUA (Full form: joint optimization algorithm with Subchannel allocation and User Association)

**What it is (from scratch):** Yeh ek aur **baseline** hai — isme vehicle association aur subchannel allocation toh smartly optimize hote hain, lekin **drone ka trajectory optimize NAHI hota** — drone ek pehle-se-decided (fix), seedhe raaste par udta hai, jaise woh "auto-pilot pe fix route" pe hai. Yeh check karne ke liye banaya gaya hai ki "agar drone smart trajectory na chune, toh kitna fark padta hai?"

**What it is (Definition):** A benchmark algorithm that jointly optimizes vehicle association and subchannel allocation but does not optimize the UAV trajectory — UAV flies along a predetermined, unoptimized route.

**Full name from paper:** "Joint optimization algorithm with Subchannel allocation and User Association" — demonstrates performance without trajectory optimization.

**Performance gap vs. TDRA:** TDRA outperforms SAUA by an average of 178.57% in average achievable rate (varying UAV speed) — the largest gap against any baseline. SAUA's achievable rate does not change with UAV maximum flight speed (because trajectory is not optimized), confirming that trajectory optimization is the single most impactful component of TDRA.

**Real-life analogy:** Ek delivery-rider jo apna route kabhi nahi badalta, chahe orders kahin se bhi aa rahe hon — woh hamesha same fix route follow karta hai. Iske against, ek smart rider jo orders ke hisaab se route adjust karta hai — performance mein bahut bada fark aata hai (yahan 178%!).

**Easy way to remember:** SAUA = "Static UAV (fix route), baaki sab optimize" — yeh batata hai trajectory optimization sabse zyada important kyun hai.

---

## SEA (Full form: Algorithm with Sub-channel Equal Allocation)

**What it is (from scratch):** Yeh teesra **baseline** hai — isme drone ka trajectory aur vehicle association toh smartly optimize hote hain, lekin **subchannels sabko equally (barabar-barabar) baat diye jaate hain**, bina yeh smartly decide kiye ki kaun-sa subchannel sensing ke liye jaaye aur kaunsa communication ke liye. Jaise ek pizza ke slices sab guests ko exactly equal-equal de diye jaayein, bina yeh dekhe ki kisko zyada bhookh hai.

**What it is (Definition):** A benchmark algorithm that jointly optimizes UAV trajectory and vehicle association but allocates subchannels equally among vehicles, without optimizing subchannel function selection.

**Full name from paper:** "Algorithm with Sub-channel Equal Allocation" — demonstrates performance without subchannel allocation optimization.

**Performance gap vs. TDRA:** TDRA outperforms SEA by 87.01-109.92% depending on the parameter varied. This demonstrates that intelligent subchannel allocation and function selection (balancing communication vs. sensing subchannels per vehicle per time slot) is a critical optimization lever.

**Real-life analogy:** Ek class mein agar teacher sabko exactly equal time de (chahe kisi student ko zyada help chahiye ho ya kam) — overall result utna achha nahi hoga jitna "jisko zyada zaroorat hai usko zyada time dena" se hoga. SEA "equal-baatwara" hai, TDRA "need-based smart baatwara" hai.

**Easy way to remember:** SEA = "Subchannels Equally bate hain, baaki sab optimize" — equal-split kabhi best nahi hota.

---

## Average Achievable Rate ⭐

**What it is (from scratch):** Yeh is poore paper ka **main scorecard** hai — "result kaisa hai" judge karne ka primary tareeka. **Rate** ka matlab hai "data kitni speed se transfer ho raha hai" (jaise Mbps mein internet speed). **Average Achievable Rate** matlab — pure time (saare 30 time-slots) mein, saari gaadiyon ko milne wali total data-speed ka **average**. Jitna zyada yeh number, utna behtar — matlab gaadiyon ko zyada/fast data mil raha hai.

**What it is (Definition):** The primary performance metric of this paper — the time-averaged total downlink communication rate across all vehicles:
  f(s) = (1/T) * sum_{t=1}^{T} TR^t
where TR^t = sum_v TR^t_v is the total communication rate for all vehicles at time slot t.

**The Math (if you're curious):** TR^t_v is the achievable rate for vehicle v at time t, drawn from its associated edge node over subchannels designated for communication:
  R^{t,c}_{u,v} = cat^{t,c}_v * sccr^{t,c} * B_0 * log2(1 + h^{t,c}_{u,v} * P^t_u / N_0)

This metric captures sustained system throughput across the entire optimization horizon. It is maximized subject to sensing performance constraints (MI >= HMI), energy constraints, safety distance constraints, and minimum per-vehicle data rate constraints (TR^t_v >= TR^req_v).

**Real-life analogy:** Jaise tumhare ghar ke WiFi ki speed test — agar tum din mein 30 baar speed-test karo aur sab ka average nikalo, woh "average achievable rate" jaisa hai. Yeh ek hi number, poore din ka overall performance batata hai.

**Easy way to remember:** Average Achievable Rate = "overall, gaadiyon ko kitni data-speed mil rahi hai (average)" — yeh number jitna zyada, system utna achha.

---

## OFDM (Full form: Orthogonal Frequency-Division Multiplexing) ⭐

**What it is (from scratch):** Socho tumhe ek hi waqt mein 5 dosto ko alag-alag messages bhejna hai. Agar sab messages ek hi "channel" se ek saath bheje jaayein, woh aapas mein mix ho jaayenge (interference). **OFDM** ek smart tareeka hai — yeh available spectrum ko bahut saare chhote-chhote "subcarriers" (mini-channels) mein todta hai, aur har subcarrier par alag-alag data bheja ja sakta hai bina ek-doosre ko disturb kiye (ye "orthogonal" hone ka fayda hai — matlab woh ek-doosre se "independent/non-overlapping" hain). Plus, har message ke beech ek chhota "gap" (cyclic prefix) bhi rakha jaata hai taaki agar signal thoda late/delayed pahunche bhi, tab bhi messages aapas mein mix na hon.

**What it is (Definition):** A multi-carrier modulation scheme that divides available bandwidth into multiple orthogonal subcarriers, enabling simultaneous transmission on all subcarriers with inter-symbol interference protection via a cyclic prefix.

**In this paper:** UAVs and GBSs use OFDM-based dual-function transmitters. The OFDM structure enables ISAC because different subcarriers can independently serve communication or sensing, and the Fourier transform of the target impulse response at each subcarrier frequency |Q(f^c)|^2 directly enters the MI formula, linking OFDM waveform design to radar sensing performance.

**Doppler compatibility:** At vehicle speeds (2 m/s) and UAV speeds (up to 12 m/s) in this simulation, Doppler spread remains within NR-V2X subcarrier spacing tolerances, so OFDM does not suffer severe inter-carrier interference. OTFS (Orthogonal Time Frequency Space modulation), which offers improved Doppler robustness, is identified as a future extension.

**Real-life analogy:** Ek bade orchestra mein har instrument apna alag "frequency/note" bajata hai — sab ek saath baj rahe hain, par kisi ek instrument ki awaaz dusre ko "dhak" nahi rahi, kyunki sabki frequencies alag-alag (non-overlapping) hain. OFDM bhi data ko aise "alag-alag non-overlapping frequencies" (subcarriers) mein bhejta hai.

**Easy way to remember:** OFDM = "spectrum ko bahut saare chhote, non-overlapping 'lanes' mein todna" — taaki bahut sa data ek saath, bina mix hue, bheja ja sake.

---

## Doppler Effect

**What it is (from scratch):** Tumne notice kiya hoga — jab ek ambulance siren bajaate hue tumhare paas se guzarti hai, uski awaaz pehle "high-pitch" lagti hai (jab woh tumhari taraf aa rahi hoti hai) aur fir "low-pitch" ho jaati hai (jab woh door ja rahi hoti hai) — chahe ambulance ka actual siren-sound constant hi ho. Yeh **Doppler Effect** hai — jab transmitter aur receiver ke beech relative motion (movement) ho, toh signal ki "frequency" thodi shift (badal) jaati hai.

**What it is (Definition):** The shift in observed signal frequency due to relative motion between transmitter and receiver.

**In this paper:** Both UAV mobility and vehicle mobility produce Doppler shifts. For the radar sensing function, vehicle velocity creates a frequency shift in the reflected OFDM signal, which is captured implicitly through the target impulse response q in the MI formula. The paper explicitly justifies OFDM use by noting that the Doppler spread in the considered scenario falls within the tolerance range of standardized OFDM-based NR-V2X and LTE-V2X systems.

**Real-life analogy:** Ambulance siren ka pitch-change — paas aate waqt high, door jaate waqt low. Wireless signals mein bhi, jab UAV ya gaadi move karti hai, signal ki frequency thodi "shift" hoti hai — isi ko account karna padta hai.

**Easy way to remember:** Doppler Effect = "movement ki wajah se frequency mein shift" — jaise ambulance siren ka pitch badalna.

---

## Interior-Point Method ⭐

**What it is (from scratch):** Yeh ek **algorithm** (step-by-step computer procedure) hai jo convex (bowl-shaped) optimization problems ko solve karta hai. Socho tumhe ek bowl ke sabse neeche point tak pahunchna hai, lekin bowl ke "boundary" (kinaron) ko touch nahi karna (kyunki kinare "rules/constraints" represent karte hain jo cross nahi karne). **Interior-Point Method** bowl ke "andar-andar" (interior mein) rehte hue, dheere-dheere sabse neeche point ki taraf badhta hai — kinaron ko avoid karte hue. Yeh bahut efficient hai aur bade problems ke liye bhi fast kaam karta hai.

**What it is (Definition):** A class of algorithms for solving convex optimization problems by following a path through the feasible region's interior using barrier functions and Newton's method.

**Properties:** Polynomial-time convergence; handles large-scale LP and convex QP/SOCP; robust to degenerate or unbounded cases; faster than simplex for large problems.

**In this paper:** After relaxation and SCA convexification, each subproblem is solved by the interior-point method. For vehicle association (P1.1): the logarithmic barrier method transforms inequality constraints into penalty terms, and the exterior penalty function handles the single-association equality constraint. For trajectory (P1.2) and subchannel allocation (P1.3): the SCA-convexified problems are directly solved by the interior-point method.

**Real-life analogy:** Ek bowl ke "andar-andar," kinaron se thoda door rehte hue, sabse neeche tak chalna — jaise tum ek katore mein marble ko dheere-dheere center tak roll karo, edges se takraye bina.

**Easy way to remember:** Interior-Point Method = "bowl ke andar-andar chal kar, kinaron ko avoid karte hue, sabse neeche tak pahunchna."

---

## Random Rounding

**What it is (from scratch):** Optimization mein kabhi-kabhi answer "haan/na" (0 ya 1, binary) hona chahiye — jaise "yeh subchannel sensing ke liye hai (1) ya nahi (0)." Lekin solve karne ke aasaani ke liye, hum pehle in variables ko "kuch bhi 0 se 1 ke beech" (jaise 0.7) hone deते hain (isko "relaxation" kehte hain) — solve karna easy ho jaata hai. Par final answer toh strictly 0 ya 1 hi hona chahiye. **Random Rounding** ek tareeka hai — agar relaxed value 0.7 hai, toh us variable ko **70% chance se 1, aur 30% chance se 0** bana do (jaise ek biased-coin toss). Isse final answer binary ban jaata hai, aur "0.7" wali information bhi probability ke through use ho jaati hai.

**What it is (Definition):** A technique for converting a relaxed continuous solution of a binary (0-1) integer program into a binary feasible solution by treating each relaxed value as the probability of that variable being 1.

**The Math (if you're curious):**
  P[psi = 1] = psi_tilde (relaxed value from continuous solution)

**In this paper:** After solving the linear relaxations of P1.1 and P1.3, the optimal relaxed solutions (cb* and ca*, sccr*) are converted to binary using random rounding. This introduces a small suboptimality gap relative to the continuous relaxation but maintains feasibility with respect to the binary constraints.

**Real-life analogy:** Socho tumhe "haan/na" decide karna hai par tumhare paas sirf "70% confidence" hai ki "haan" sahi hai. Toh tum ek dice/coin use karti ho jo 70% chance se "haan" bole — final decision toh "haan/na" hi aata hai, par 70% wala "feel" bhi reflect ho jaata hai.

**Easy way to remember:** Random Rounding = "0.7 wale answer ko 70% chance se 1, 30% chance se 0 — coin-flip se final binary decision."

---

## Quasi-Static Channel Model

**What it is (from scratch):** **"Static"** ka matlab "fix/na-badalne wala" hota hai. **Quasi-Static** ka matlab hai "thodi der ke liye static maan lo, chahe asal mein woh thoda-thoda badal raha ho." Yahan, har "time slot" (jaise 1-second ka chhota interval) ke andar, hum maan lete hain ki gaadiyon ki positions/channel-conditions "fix" hain (na badal rahe). Lekin ek time-slot se doosre time-slot mein, positions/conditions update ho jaati hain. Yeh simplification calculations ko bahut aasaan banata hai.

**What it is (Definition):** A modeling assumption that channel conditions (vehicle positions, path loss) remain approximately constant within a time slot but change between time slots.

**In this paper:** Each time slot has duration tau = 1 second. Within each slot, all nodes are modeled as stationary. Across slots, vehicle positions update at constant velocity (2 m/s). This is standard in UAV-V2X optimization literature and is justified because movement within 1 second is small relative to the communication and sensing timescales being optimized.

**Real-life analogy:** Jaise ek photo-slideshow — har photo (1-second snapshot) ke andar sab "freeze/static" hai, par next photo mein thoda movement dikh jaata hai. Quasi-static model bhi "snapshot-by-snapshot" sochta hai, continuous video ki tarah nahi.

**Easy way to remember:** Quasi-Static = "har chhote interval ke andar 'freeze-frame' maan lo, par frames ke beech movement allowed hai."

---

## Subchannel Function Selection ⭐

**What it is (from scratch):** Humne dekha ki spectrum ko subchannels (chhote tukdon) mein baata jaata hai, aur har subchannel kisi gaadi ko diya jaata hai (Subchannel Allocation). Ab ek aur decision hai — har subchannel, jise diya gaya hai, woh **kis "job" ke liye use hoga** — communication (data bhejne ke liye) ya sensing (radar/echo ke liye)? Isi "job-assignment" ko **Subchannel Function Selection** kehte hain. Yeh bilkul wahi decision hai jo "Sensing-Communication Tradeoff" ko practically implement karta hai.

**What it is (Definition):** The binary decision (per subchannel, per time slot) of whether a subchannel is used for downlink data communication or for radar sensing.

**The Math (if you're curious):** Variable: sccr = {sccr^{t,c}} where sccr^{t,c} = 1 means subchannel c serves communication at time t (contributing to vehicle data rate), and sccr^{t,c} = 0 means it serves radar sensing (contributing to vehicle sensing MI).

**Significance:** Subchannel function selection is how the sensing-communication tradeoff is operationalized. It enables fine-grained control over spectrum partitioning between the two ISAC functions on a per-subchannel, per-time-slot basis. This variable is jointly optimized with subchannel allocation ca in subproblem P1.3.

**Real-life analogy:** Socho ek slice pizza tumhe mil gaya (allocation ho gaya) — ab decide karo woh slice "abhi khaane" ke liye hai ya "kal ke liye bacha ke rakhna" hai. Subchannel mil jaane ke baad, uska "use-case" (communication ya sensing) decide karna hi function selection hai.

**Easy way to remember:** Function Selection = "subchannel mil gaya, par usse communication ke liye use karein ya sensing ke liye?"

---

## Free-Space Path Loss ⭐

**What it is (from scratch):** Jab koi signal transmitter se nikal kar khule space mein travel karta hai (bina kisi obstruction ke), woh distance ke saath "weak" hota jaata hai — jitni zyada distance, signal utna kamzor. Isi "weakening with distance" ko **Path Loss** kehte hain, aur **Free-Space** ka matlab hai "khula, bina obstruction ka environment." Important baat: yeh weakening linearly nahi hota — distance double hone par signal sirf aadha kamzor nahi hota, balki **distance ke square ke hisaab se** kamzor hota hai (matlab distance double = signal 4x kamzor).

**What it is (Definition):** The attenuation of signal power in a free-space propagation environment, proportional to distance squared.

**The Math (if you're curious) — Formula (UAV-vehicle link):**
  h^{t,c}_{u,v} = h^c_{u,v} / (d^t_{u,v})^2

where d^t_{u,v} = sqrt(||CU^t_u - CO^t_v||^2 + H_u^2) is the 3D Euclidean distance (incorporating fixed altitude H_u = 8 m), and h^c_{u,v} is the reference channel gain at 1 m on subchannel c (set to -60 dB in simulations). For GBS-vehicle links, a general path loss exponent a >= 0 is used: h^{t,c}_{r,v} = h^c_{r,v} / (d^t_{r,v})^a.

**Justification:** Free-space path loss is appropriate for LoS-dominant low-altitude UAV-to-ground links in open-road scenarios. The paper notes that more complex models (Rician, shadowing, altitude-dependent effects) can be incorporated in future extensions.

**Real-life analogy:** Jaise tum kisi se door se baat karo — agar woh 2 meter door hai, awaaz clear sunayi degi; agar 8 meter door (4x distance), awaaz sirf "thodi kam" nahi, balki bahut zyada kam (16x kamzor, square-wise) sunayi degi. Distance ka effect "square" mein hota hai, simple proportion mein nahi.

**Easy way to remember:** Free-Space Path Loss = "signal, distance ke SQUARE ke hisaab se kamzor hota hai" — double distance = 4x weak.

---

## Cramer-Rao Bound (CRB) (Full form: Cramer-Rao Bound)

**What it is (from scratch):** Jab tum radar se kisi cheez ki position/speed "estimate" (guess/measure) karti ho, woh estimate kabhi 100% perfect nahi hoti — usme thoda "error/uncertainty" hota hai. **Cramer-Rao Bound (CRB)** ek mathematical limit hai jo batata hai: "best-possible measurement technique bhi, is se kam error nahi de sakti" — yeh ek "theoretical best-case" hai. Jitna kam CRB, utna accurate measurement possible hai (kam error).

**What it is (Definition):** A theoretical lower bound on the variance of any unbiased estimator of a deterministic parameter, quantifying the best achievable radar estimation accuracy.

**Connection to MI:** Higher MI corresponds to a tighter CRB (lower estimation variance for target range, velocity, and angle of arrival). The paper cites this to justify MI as the sensing performance metric: maximizing MI effectively minimizes fundamental estimation uncertainty. The connection is established through the Gaussian echo model and the monotonic relationship between MI and KL-divergence between target-present and target-absent hypotheses.

**Real-life analogy:** Jaise ek measuring-tape ki "minimum possible error" — chahe kitna bhi careful measure karo, ek certain limit se kam error nahi aa sakta us tape se. CRB radar-measurement ke liye waisi hi "best-case error limit" hai. Higher MI = tighter (better) CRB = kam minimum-error.

**Easy way to remember:** CRB = "measurement ka best-case error-limit" — MI zyada toh CRB better (tighter).

---

## Lagrangian / Barrier Function ⭐

**What it is (from scratch):** Optimization problems mein "constraints" (rules/limits) hote hain — jaise "yeh value 5 se kam honi chahiye." Inko directly handle karna mushkil hota hai. **Barrier Function** ek trick hai — constraint ko ek "penalty" mein convert kar dete hain jo objective (goal) mein add ho jaati hai: agar tum constraint ke "boundary" (limit) ke bahut paas jaane lagti ho, penalty bahut bada ho jaata hai (jaise "alarm bajne lagta hai"), jisse optimization automatically boundary se door rehta hai. Isse "constraint ke saath solve karo" wala problem, "bina-constraint ke solve karo (penalty ke saath)" mein convert ho jaata hai — jo aasaan hai.

**What it is (Definition):** Mathematical tools that incorporate constraints into the objective function through penalty or barrier terms, converting constrained optimization into unconstrained optimization.

**The Math (if you're curious) — Logarithmic barrier function (used in P1.1):**
  F1C(cb) = -sum log(TR^t_v - TR^req_v) - sum log(TMI^t_v - HMI^t_v) - sum log(1 - cb^t_{v,r}) - sum log(cb^t_{v,r}) - ...

The log barrier penalizes solutions approaching constraint boundaries (preventing infeasibility). The exterior penalty function (quadratic penalty on equality constraint violations) handles the single-association constraint (sum = 1). Together they enable Newton's method to solve the resulting unconstrained problem. The penalty parameter mu is decreased across outer iterations (multiplied by a decrement coefficient lambda in (0,1)) to tighten the barrier approximation.

**Real-life analogy:** Jaise ek invisible electric-fence — jab tum boundary ke paas jaati ho, halka current lagta hai (penalty), jisse tum automatically boundary se door rehti ho, bina kisi "hard wall" ke. Barrier function bhi optimization ko "automatically" constraints ke andar rakhta hai.

**Easy way to remember:** Barrier Function = "constraint ko ek 'invisible fence/penalty' mein convert karna" — boundary ke paas jaate hi cost badh jaata hai.

---

## Time-Slot Framework ⭐

**What it is (from scratch):** Continuous, real-time decisions lena (har micro-second pe) bahut complex hota hai. Toh time ko chhote-chhote, equal **"slots"** (jaise har 1-second ka ek slot) mein baant diya jaata hai. Har slot ke andar, ek decision liya jaata hai aur woh slot khatam hone tak "fix" rehta hai. Phir naya slot shuru hota hai, naya decision. Isi structure ko **Time-Slot Framework** kehte hain — jaise ek movie ko "frames" mein dekha jaata hai, ek-ek frame ek waqt.

**What it is (Definition):** The discretization of the optimization horizon into T equal-length periods (slots), within each of which resource allocation decisions are made and held constant.

**In this paper:** T = 30 time slots, tau = 1 s/slot (total optimization horizon = 30 s). Within each slot t, vehicle association cb^t, subchannel allocation ca^t, and subchannel function selection sccr^t are optimized. UAV trajectory positions CU^t are also updated per slot (step size bounded by max_speed * tau). The total achievable rate is averaged over all T slots to compute the objective f(s) = (1/T) * sum_t TR^t.

**Real-life analogy:** Jaise ek class-timetable — har "period" (slot) mein ek subject fix hota hai, aur period khatam hone par naya subject shuru hota hai. Poore din (T periods) ka overall "performance" sab periods ke average se nikalta hai.

**Easy way to remember:** Time-Slot Framework = "time ko chhote, fix-decision wale 'periods' mein todna" — har period mein decisions fix, periods ke beech update.

---

(5 most important terms marked with ⭐: ISAC, BCD, SCA, MINLP, Mutual Information, UAV, GBS, Subchannel Allocation, Vehicle Association, Trajectory Planning, Radar Sensing Constraint, Convex Optimization, Sensing-Communication Tradeoff, Average Achievable Rate, OFDM, Interior-Point Method, Subchannel Function Selection, Free-Space Path Loss, Lagrangian/Barrier Function, Time-Slot Framework — yeh saare concepts paper ke core mechanism ko samajhne ke liye zaroori hain. Agar koi term abhi bhi confusing lage, `explanation.md` mein conceptual/intuitive version dekho.)
