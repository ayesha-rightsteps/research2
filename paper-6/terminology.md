# Terminology: A Multi-Agent Resource Allocation Method for Unmanned Aerial Vehicle Networks based on Adaptive Spatial Reward Mechanisms and Context-Awareness

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai. Koi short-form/acronym bina poora naam aur matlab bataye nahi chhoda gaya.

---

## UAV (Full form: Unmanned Aerial Vehicle) ⭐
**What it is (from scratch):** "Unmanned" matlab "bina insaan ke," "Aerial" matlab "hawai/udने wala," aur "Vehicle" matlab "gaadi/machine." Toh UAV ek aisi flying machine hai jisme koi pilot nahi baitha hota — usse "drone" bhi kehte hain, jo tumne shaadiyon mein photography ke liye udte dekha hoga.
**In this paper:** Yahan UAVs "flying mobile towers" ki tarah kaam karte hain — 100 meter ki height par udd kar neeche zameen par maujood logon (users) ko wireless signal/internet provide karte hain. Har UAV apna decision khud leta hai (kaunsa user serve karna, kaunsa channel, kitni power).
**Real-life analogy:** Socho tumhare ghar ke paas ek mobile tower hai jo signal deta hai — ab usi tower ko pankh lagaa do aur usse aasmaan mein udne do. Wahi UAV hai.
**Easy way to remember:** UAV = "udta hua tower," jisme koi pilot nahi.

---

## MARL (Full form: Multi-Agent Reinforcement Learning) ⭐
**What it is (from scratch):** Pehle "Reinforcement Learning (RL)" samjho — ye ek tareeka hai jisse koi computer-program ("agent") trial-and-error se seekhta hai: agent ek action leta hai, usse ek number ("reward") milta hai jo batata hai action achha tha ya bura, aur time ke saath agent seekh jaata hai ki kaunsi situation mein kaunsa action best hai. Ab "Multi-Agent" ka matlab hai — sirf ek nahi, **kayi agents ek saath** ek hi environment mein seekh rahe hain, har ek apna alag reward paa raha hai, lekin sabka behavior ek-doosre ko affect karta hai.
**In this paper:** Har UAV ek alag MARL agent hai. Koi central computer sabko control nahi karta — har UAV apna user, channel, aur power level khud choose karta hai, apne experience se seekh kar.
**Real-life analogy:** Socho 6 delivery riders ek shehar mein kaam kar rahe hain, har ek apna route khud seekhta hai (kaunsi gali mein traffic kam hai, kaunse area mein zyada orders milte hain) — bina kisi control-room ke unhe order diye.
**Easy way to remember:** MARL = "kayi log, har ek apna trial-and-error karke seekh raha hai, sath rehte hue."

---

## ASAC (Full form: Adaptive Spatial reward-based Actor-Critic) ⭐
**What it is (from scratch):** Ye is paper ka apna naya method hai — naam mein hi 3 cheezein chhupi hain. "Adaptive" matlab "situation ke hisaab se adjust hone wala," "Spatial reward" matlab "location/distance ke hisaab se reward ko adjust karna" (neeche detail mein hai), aur "Actor-Critic" ek RL architecture hai (neeche dekho) jisme ek part decision leta hai aur ek part usse rate karta hai.
**In this paper:** ASAC standard MARL (Actor-Critic) ke upar do naye ideas jodta hai — (1) Adaptive Spatial Reward Mechanism (paas wale agents ka reward zyada count, door walon ka kam) aur (2) Agent Context-Aware Communication (har UAV apna "internal summary" neighbors ke saath share karta hai). Result: dense network (200 users) mein 16% better resource allocation (reward 4.89 vs best baseline ka 4.23).
**Real-life analogy:** Socho ek class mein har student apna homework khud karta hai (Actor-Critic), lekin saath mein woh apne paas baithe classmates se hint leta hai (context-aware communication), aur sirf paas walon ke marks ko apna performance judge karne mein zyada weight deta hai, door baithe students ke nahi (spatial reward).
**Easy way to remember:** ASAC = "standard seekhne ka tareeka + paas walon ki zyada sunو + apna context share karo."

---

## Actor-Critic (A2C / MA2C — Full form: Advantage Actor-Critic / Multi-Agent Advantage Actor-Critic) ⭐
**What it is (from scratch):** RL mein agent ko 2 kaam karne hote hain — (1) decide karna ki kya action lena hai, aur (2) ye andaza lagaana ki "ye action kitna achha tha/hoga." **Actor-Critic** architecture mein in dono kaamon ke liye 2 alag "parts" (neural networks) hote hain: **Actor** action choose karta hai (ye "kya karu" decide karta hai), aur **Critic** us action ko rate karta hai (ye batata hai "ye decision kitna achha tha, isse future mein kitna reward milega"). Critic ki feedback se Actor apna decision-making improve karta hai.
**In this paper:** Jab is Actor-Critic setup ko **multiple agents** (multiple UAVs) ke saath use karte hain, toh usse **MA2C (Multi-Agent Actor-Critic)** kehte hain — yahi is paper ka base/foundation framework hai, jiske upar ASAC ke do naye innovations add kiye gaye hain.
**Real-life analogy:** Ek student (Actor) practice questions solve karta hai, aur ek teacher (Critic) uske answers check karke batata hai "ye approach kitni sahi thi" — student us feedback se apna next attempt better banata hai.
**Easy way to remember:** "Actor karta hai, Critic batata hai kaisa kiya" — dono milkar agent ko better banate hain.

---

## Adaptive Spatial Reward Mechanism ⭐
**What it is (from scratch):** Normal RL mein agent ka reward sirf uske apne action ke result se aata hai. Lekin jab multiple agents ek saath kaam kar rahe hon (MARL), toh sawal aata hai — "mujhe sirf apna reward dekhna chahiye ya doosron ka bhi?" Agar sirf apna dekhun, toh main "selfish" ban jaata hoon aur overall system suboptimal ho sakta hai. Agar **sabka** reward equally dekhun, toh itni saari information aa jaati hai ki "mere action ka asli effect kya tha" pata karna mushkil ho jaata hai (training confuse ho jaati hai). Is paper ka idea: **beech ka raasta nikalo** — paas wale agents ke rewards ko zyada importance do, door walon ko kam — distance ke hisaab se ek "discount" (kam karna) lagao.
**In this paper:** Har UAV apna training-reward calculate karte waqt apne paas wale UAVs ke rewards ko bhi include karta hai, lekin un rewards ko **spatial discount factor (kappa)** se multiply karke kam kar deta hai — jitna door agent, utna kam uska reward count hota hai.
**Real-life analogy:** Socho tum Andheri mein delivery karti ho. Bandra wale rider (paas wala) ne kaisa kiya, uska tumhare route-planning par thoda effect padna chahiye — agar usne overlap kiya, tumhe pata chalna chahiye. Lekin Pune wale rider (bahut door) ka performance tumhare faisle par seedha asar nahi daalna chahiye.
**Easy way to remember:** "Jitna paas, utna zyada matter karta hai" — jaise awaaz jitni paas se aaye, utni loud sunayi deti hai, door se aaye toh halki.

---

## Spatial Discount Factor — kappa ⭐
**What it is (from scratch):** Ye ek number hai (0 aur 1 ke beech) jo control karta hai ki "doosre agents ka reward, mere reward mein kitna count ho — distance ke hisaab se." Formula simple hai: kisi agent ka reward, distance "d" door hai toh usse **kappa ki power d** (kappa^d) se multiply karke kam kar dete hain. Agar kappa = 0 ke paas hai, matlab "main sirf apna reward dekhunga" (selfish/greedy). Agar kappa = 1 ke paas hai, matlab "main poori duniya ka reward equally count karunga" (fully cooperative). Beech ki value ek balance deti hai.
**In this paper:** Kappa is paper ka **sabse bada naya idea (core novelty)** hai. Ye control karta hai ki UAV apne paas-paas ke neighbors ke rewards ko training mein kitna weight de.
**Real-life analogy:** Socho tumhare ghar mein gaana baj raha hai — jo room tumhare paas hai, uska gaana tumhe loud sunayi dega; jo room door hai, uska gaana halka/almost-na-sunayi-dene-wala hoga. Kappa "volume kam karne ka dial" hai jo distance ke saath automatically ghoomta hai.
**Important distinction:** Ye **temporal discount (delta)** se bilkul ALAG hai (neeche dekho) — kappa "SPACE/distance" ke hisaab se kam karta hai, delta "TIME" ke hisaab se kam karta hai. Dono alag-alag cheezon ko control karte hain, aur paper mein dono saath use hote hain.
**Easy way to remember:** Kappa = "DOOR HONE PAR kam importance" (spatial/location-based discount).

---

## Time Discount Factor — delta
**What it is (from scratch):** RL mein agent sirf "abhi" ka reward nahi dekhta, "future" mein milne wale rewards ko bhi consider karta hai. Lekin future ka reward "abhi" ke reward jitna valuable nahi maana jaata — jaise "aaj 100 rupaye milna" "1 saal baad 100 rupaye milne" se zyada valuable lagta hai. **Time Discount Factor (delta)** ek number hai (0 se 1 ke beech) jo decide karta hai ki future ka reward kitna "kam karke" dekha jaaye. Delta jitna 1 ke paas, agent utna "patient" (long-term-soch wala); delta jitna 0 ke paas, agent utna "short-sighted" (sirf abhi ka sochta hai).
**In this paper:** Delta = 0.99 set kiya gaya — matlab agent bahut "patient" hai, future rewards ko almost poora weight deta hai. Yeh formula mein use hota hai: total future reward = sum of (delta ki power "kitne steps future mein") x (us step ka reward).
**Real-life analogy:** Agar tum aaj exam ke liye padhai karo (abhi thoda mushkil/boring) taaki 6 mahine baad achhe marks aayein — tumne "future reward" ko high value diya, matlab delta high. Agar tum sirf "aaj party karne" ko prefer karo, future ko ignore karke — delta low.
**Important distinction:** Delta **TIME** ke hisaab se discount karta hai (kab milega reward), jabki kappa (upar) **SPACE/distance** ke hisaab se discount karta hai (kahan se aa raha hai reward). Ye dono is paper mein saath-saath use hote hain lekin do bilkul alag chhezon ko control karte hain — isse confuse na karo.
**Easy way to remember:** Delta = "LATER milne par kam importance" (time-based discount) — kappa se mat mix karo.

---

## Agent Context-Aware Communication Mechanism ⭐
**What it is (from scratch):** "Context-aware" matlab "apne aas-paas ki situation ko samajh kar usi ke hisaab se apna behavior badalna" — fixed/hardcoded rule follow karne ke jagah. Is mechanism mein har agent apni "current understanding ka summary" (jisko hidden state kehte hain — neeche LSTM mein detail hai) apne paas wale neighbors ko bhej deta hai. Jab koi agent decision lene wala hota hai, woh apni khud ki info + neighbors se aaye summaries — dono ko combine karke ek "richer" (zyada complete) tasveer banata hai, aur usi ke hisaab se decision leta hai.
**In this paper:** Har UAV apna LSTM-encoded hidden state apne neighboring UAVs ke saath share karta hai. Neighbor ye message apne observation ke saath combine karke apna naya hidden state banata hai, jo uski Actor (decision lene wali) aur Critic (rate karne wali) — dono networks ko feed hota hai.
**Real-life analogy:** Sports team mein players khelte waqt chillake apni position batate hain ("main left side cover kar raha hoon!") — taaki teammates apna positioning usi ke hisaab se adjust kar saken, bina kisi coach se poochhe.
**Easy way to remember:** "Apna context bata do neighbors ko, taaki sabki decisions better-coordinated ho."

---

## LSTM (Full form: Long Short-Term Memory) ⭐
**What it is (from scratch):** Ye ek special type ka neural network hai jo "sequence" — yaani time ke saath ek-ke-baad-ek aane wali information — ko process karne mein achha hai, kyunki ye "purani information ko yaad rakh sakta hai" jab nayi information process kar raha ho. Normal neural network har input ko "fresh/isolated" treat karta hai (jaise koi cheez bina context ke dekhna); LSTM "memory" rakhta hai — jaise tumhara dimaag yaad rakhta hai 5 minute pehle kya hua tha, jab tum abhi koi decision le rahi ho.
**In this paper:** Ek **64-neuron LSTM** har UAV ke "observation + neighbors se aaye messages" ko process karta hai aur ek "hidden state" (current situation ka compressed summary) banata hai. Ye hidden state UAV ki Actor (decision) aur Critic (rating) — dono ko diya jaata hai, aur agle time-step ke liye bhi carry hota hai (taaki history yaad rahe).
**Real-life analogy:** Jab tum kisi conversation mein ho, tum sirf "abhi jo bola gaya" nahi sun rahi — tumhe "pichle 5 minute mein kya discuss hua" bhi yaad hai, aur usi context mein tum apna reply deti ho. LSTM bhi waise hi "conversation ka context" yaad rakhta hai.
**Easy way to remember:** LSTM = "neural network jiski short-term memory hoti hai" — purani info ko bhula nahi deta, usse current decision mein use karta hai.

---

## SINR (Full form: Signal-to-Interference-plus-Noise Ratio) ⭐
**What it is (from scratch):** Jab koi UAV signal bhejta hai, woh signal "doosri transmissions ke interference" (jaise doosre UAVs ka signal jo same channel use kar rahe hain) aur "background noise" (random disturbance, jaise radio static) ke saath mix ho kar receiver tak pahunchta hai. **SINR** ek number hai jo compare karta hai: "mera asli signal kitna strong hai" VS "interference + noise kitna strong hai." High SINR = signal clearly samajh aata hai; low SINR = signal "doob" jaata hai interference/noise mein.
**In this paper:** Har UAV apna total SINR calculate karta hai (apne saare served users aur sub-channels par sum karke). Agar ye SINR ek minimum threshold (QoS threshold, gamma-bar = 2.7 dB) se kam ho jaaye, toh reward **zero** ho jaata hai — matlab "is round mein kuch nahi mila."
**Real-life analogy:** Tumhare phone mein "signal bars" — full bars = high SINR (call clear), 1 bar/no-service = low SINR (call drop).
**Easy way to remember:** SINR jitna zyada, signal ki "voice" utni clear — kam SINR matlab "noise mein doob gaya."

---

## MDP (Full form: Markov Decision Process)
**What it is (from scratch):** Ye ek "mathematical rulebook" hai jo RL problems ko formally describe karne ke liye use hota hai. Ye 4 cheezein define karta hai: **State** (agent ki current situation kya hai), **Action** (agent kya kar sakta hai), **Reward** (action lene ke baad kya milta hai), aur **Transition** (action lene ke baad next state kya banega). "Markov" naam isliye hai kyunki ek property assume hoti hai — "next state sirf current state aur action par depend karta hai, purani history par nahi" (matlab "abhi" hi sab kuch decide karta hai aage kya hoga).
**In this paper:** Har UAV ka resource-allocation problem (kaunsa user, kaunsa sub-channel, kitni power) ek MDP ke through formally describe kiya gaya hai. Environment ki state mein channel conditions aur user positions shamil hain.
**Real-life analogy:** Ek board-game ka rulebook — "tum yahan ho (state), tum ye moves kar sakte ho (actions), har move ka ye result hoga (reward), aur agla position ye banega (transition)."
**Easy way to remember:** MDP = "RL ka rulebook" — state, action, reward, transition.

---

## Reward
**What it is (from scratch):** Reward ek number hai jo agent ko har action lene ke baad milta hai — ye batata hai "ye action kitna achha/bura tha." High/positive reward = "achha kiya," low/negative ya zero reward = "kuch faayda nahi hua ya bura hua." Agent ka goal hai apna total reward (especially future rewards, time-discount ke saath) maximize karna.
**In this paper:** Reward = "throughput minus power cost" — matlab "kitna useful data successfully transfer hua" minus "kitni battery/power use hui." Agar SINR threshold se neeche gaya, reward = 0.
**Real-life analogy:** School mein assignment submit karne par marks milna — achha kaam = zyada marks, kuch nahi kiya ya galat kiya = kam/zero marks.
**Easy way to remember:** Reward = "performance ka score, jisse agent seekhta hai."

---

## Sub-channel
**What it is (from scratch):** Wireless communication "spectrum" (radio frequencies ka range) use karta hai data bhejne ke liye. Ye spectrum limited hota hai, isliye usse chhote-chhote "tukdon" mein todte hain — har tukda ek **sub-channel** hai. Jaise ek building mein limited lifts hain — har lift ek "sub-channel" jaisi hai, aur sabko unhi mein adjust hona hai.
**In this paper:** Total bandwidth "W" ko K sub-channels mein todte hain (har ek ki width = W/K = 75 kHz). Har UAV har time-step par ek sub-channel choose karta hai. Agar 2 UAVs same sub-channel use karein, unke signals interfere karte hain.
**Real-life analogy:** Radio stations — 91.1 FM, 93.5 FM, etc. Har station apni alag frequency par hai; agar do stations same frequency use karein, dono ka audio mix ho jaata hai.
**Easy way to remember:** Sub-channel = "frequency ka chhota tukda jo ek UAV ko milta hai."

---

## Epsilon-Greedy Strategy
**What it is (from scratch):** RL mein agent ko 2 cheezon ke beech balance banana padta hai — "exploitation" (jo action ab tak best lagta hai, wahi lo — safe choice) aur "exploration" (kuch naya/random try karo — shayad usse bhi achha kuch mil jaaye jo abhi tak pata nahi). **Epsilon** ek number hai (0 se 1) jo decide karta hai ki kitne % chance agent random/naya try karega (exploration), aur baaki % chance apna best-known action lega (exploitation).
**In this paper:** Epsilon-greedy action-selection ke liye use hota hai. Experiments mein paaya gaya ki **epsilon = 0.5** sabse best performance deta hai — bahut kam ya bahut zyada dono se performance kharab hota hai, aur ye result LoS aur probabilistic — dono channel models mein consistent tha.
**Real-life analogy:** Naye sheher mein khaane ke liye — kabhi apna favourite (already pata) restaurant try karo (exploitation), kabhi bilkul naya restaurant try karo (exploration) — taaki naye achhe options bhi mil sakein. Epsilon = "kitni baar naya try karoge" ka dial.
**Easy way to remember:** Epsilon = "curiosity dial" — high epsilon = zyada naya try karna, low epsilon = jo pata hai wahi karna.

---

## EMA Action Smoothing (Full form: Exponential Moving Average)
**What it is (from scratch):** Agar agent ka action har step par bahut achanak/zyada badal jaaye (jaise "abhi full power, agle second zero power"), toh ye training ko unstable bana deta hai — system "jhatkon" mein chalega. **EMA** ek simple trick hai — naya action lene ke jagah, "naye action aur purane action ka weighted mix" lo. Ek "smoothing factor" (rho) decide karta hai kitna weight purane action ko milta hai aur kitna naye ko.
**In this paper:** Formula: naya-action = rho × purana-action + (1-rho) × abhi-wala-action (jab t>1 ho). Isse UAV ke decisions (jaise power level) smoothly change hote hain, sudden jumps nahi aate, jisse training stable rehti hai.
**Real-life analogy:** Car mein cruise control — speed achanak se 40 se 100 nahi ho jaati, gradually badhti hai. EMA bhi agent ke actions ko "gradually" badalta hai.
**Easy way to remember:** EMA smoothing = "achanak change mat karo, purane aur naye ko mix karo" — jaise cruise control.

---

## A2G Channel (Full form: Air-to-Ground Channel)
**What it is (from scratch):** Ye us wireless "raaste" ko describe karta hai jisse signal **UAV (aasmaan mein)** se **ground user (zameen par)** tak travel karta hai. Ye raasta normal ground-to-ground (jaise mobile-tower-se-phone) raaste se alag hota hai, kyunki UAV ki height/angle alag-alag effects laata hai.
**In this paper:** A2G channel decide karta hai ki UAV se user tak signal kitna strong pahunchega (channel gain) — jo seedha SINR aur isliye reward calculation mein use hota hai.
**Real-life analogy:** Ek flashlight ko upar se neeche point karna vs ek flashlight ko seedha (horizontal) point karna — height/angle ki wajah se light kaise pahunchti hai, ye alag hota hai. A2G channel "upar se neeche" wala scenario describe karta hai.
**Easy way to remember:** A2G = "Air-to-Ground" — UAV (air mein) se user (ground par) tak ka signal-raasta.

---

## LoS (Full form: Line-of-Sight)
**What it is (from scratch):** "Line of sight" matlab "seedhi nazar ki line" — agar UAV aur user ke beech koi obstacle (building, tree, etc.) nahi hai, aur signal seedha-seedha travel kar sakta hai, toh ye **LoS** condition hai. Isme signal strong rehta hai.
**In this paper:** Ek channel model jisme UAV-to-user channel gain seedha UAV ki height aur horizontal distance par depend karta hai (formula mein reference gain beta_0 = -60 dB aur path-loss exponent alpha = 2 use hota hai).
**Real-life analogy:** Flashlight ko kisi cheez ke saamne point karna jisme beech mein kuch na ho — light seedhi pahunchti hai, bright dikhti hai.
**Easy way to remember:** LoS = "saaf/seedha raasta, kuch beech mein nahi."

---

## NLoS (Full form: Non-Line-of-Sight)
**What it is (from scratch):** Iska opposite — agar UAV aur user ke beech kuch obstacle hai (building, terrain), toh signal seedha nahi pahunch sakta, usse ghoom kar (reflect/diffract ho kar) jaana padta hai, jisse woh weak ho jaata hai. Isi ko **NLoS** kehte hain.
**In this paper:** Probabilistic channel model mein NLoS ke liye extra "attenuation" (signal kamzori) factor hai — eta_NLoS = 22 dB, jabki LoS ke liye eta_LoS sirf 1.2 dB hai. Kaunsi condition (LoS ya NLoS) hogi, ye UAV-user ke beech ke "elevation angle" (kitna upar-neeche ka angle hai) par depend karta hai, jo constants a=9.6 aur b=0.15 se calculate hota hai.
**Real-life analogy:** Deewar ke peeche se baat karna — awaaz pahunchti hai, lekin muffled/weak ho jaati hai, deewar ke saamne khade hone ke comparison mein.
**Easy way to remember:** NLoS = "raaste mein rukawat hai, signal kamzor ho jaata hai."

---

## K-means
**What it is (from scratch):** Ye ek **clustering algorithm** hai — matlab ek tareeka jisse bahut saare data-points (jaise locations) ko "groups" (clusters) mein automatically baant diya jaata hai, aur har group ka "center" (centroid) nikal diya jaata hai. "K" matlab "kitne groups banane hain" — ye number pehle se decide karna padta hai.
**In this paper:** Training round shuru hone se pehle, ground users ki random locations par K-means chalaya jaata hai, aur UAVs ko unhi cluster-centers par initially place kiya jaata hai — taaki UAVs shuru se hi users ke "busy areas" ke paas hon, random scattered jagah nahi.
**Real-life analogy:** Ek city mein food-trucks ko randomly kahin bhi khada karne ke jagah, pehle ye dekho ki "log kahan-kahan zyada ikattha hain" (clusters), aur trucks ko unhi spots ke center mein khada karo.
**Easy way to remember:** K-means = "data-points ko groups mein baanto, har group ka center nikalo."

---

## Throughput (Network Utility)
**What it is (from scratch):** **Throughput** ka matlab hai "kitna data successfully bheja gaya, per unit time" — jaise tumhare internet ki "speed" (Mbps). Ye ek standard formula (Shannon's capacity formula) se calculate hota hai, jisme SINR aur bandwidth (channel ki "width") use hote hain — basically "channel jitna wide aur signal jitna clear (high SINR), utna zyada data ja sakta hai."
**In this paper:** Throughput = (sub-channel ki bandwidth) × log(1 + SINR). Ye reward formula ka pehla hissa hai (reward = throughput minus power cost).
**Real-life analogy:** Ek pipe se pani ka flow — pipe jitni wide (bandwidth) aur pressure jitna achha (SINR), utna zyada pani (data) per second ja sakta hai.
**Easy way to remember:** Throughput = "delivery speed" — kitna data per second successfully gaya.

---

## QoS Threshold (Full form: Quality of Service Threshold)
**What it is (from scratch):** "Quality of Service" matlab "service ki minimum acceptable quality." **QoS Threshold** ek minimum requirement hai — agar connection ki quality (yahan SINR) is se kam ho jaaye, toh us connection ko "acceptable nahi" maana jaata hai.
**In this paper:** QoS threshold = gamma-bar = 2.7 dB. Agar UAV ka total SINR is se kam hai, reward = 0 ho jaata hai (chahe throughput kitna bhi ho) — agent ko "majboor" kiya jaata hai ki woh hamesha minimum quality maintain kare.
**Real-life analogy:** Exam mein "pass marks" — agar tumhare marks pass-threshold se kam hain, chahe tumne kuch likha ho, "fail" hi count hoga.
**Easy way to remember:** QoS threshold = "pass/fail line" — neeche gaye toh reward zero.

---

## CSI (Full form: Channel State Information)
**What it is (from scratch):** Wireless signal jab transmitter (UAV) se receiver (user) tak jaata hai, raaste mein woh weak/distorted ho sakta hai (distance, obstacles, etc. ki wajah se). **CSI** basically "abhi is waqt channel ka kya haal hai" ka real-time report hai — kitna signal kamzor hoga, kitna gain milega, etc.
**In this paper:** Har UAV ko apne saare ground users ke saath CSI pata hai (matlab channel gain G — kitna signal pahunchega — pata hai), lekin UAV ko **doosre UAVs ki CSI ya unka full state pata nahi** — ye "partial knowledge" setting real-world jaisi hai.
**Real-life analogy:** Tumhe pata hai tumhare apne WiFi ka signal kaisa hai apne ghar mein (apni CSI), lekin tumhe nahi pata ki paas wale ghar ka WiFi signal kaisa hai unke ghar mein (doosron ki CSI).
**Easy way to remember:** CSI = "mera apna signal kitna achha hai, iska real-time report" — sirf apna pata hai, doosron ka nahi.

---

## IA2C (Full form: Independent Advantage Actor-Critic)
**What it is (from scratch):** Ye ek **baseline** (comparison ke liye purana/simple method) hai. "Independent" matlab "har agent bilkul akela, kisi se communication nahi." Toh IA2C matlab — har agent apna Actor-Critic (upar dekho) chalata hai, bilkul apne-aap, koi info doosre agents ke saath share nahi karta.
**In this paper:** IA2C ek baseline hai jiske saath ASAC compare hota hai. Ye sabse "simple" multi-agent approach hai, aur ASAC ne ise consistently outperform kiya, especially large/dense networks (200 users) mein.
**Real-life analogy:** 6 students jo ek hi exam ki taiyari kar rahe hain, lekin koi ek-doosre se baat nahi karta, notes share nahi karta — har ek bilkul akela padh raha hai.
**Easy way to remember:** IA2C = "lone wolf" agents — Actor-Critic hai, lekin koi communication nahi.

---

## MADDPG (Full form: Multi-Agent Deep Deterministic Policy Gradient)
**What it is (from scratch):** Ye ek aur **baseline** MARL method hai. Iska idea hai — training ke time, har agent ka "Critic" (rating dene wala part) **sabka** state/action dekh sakta hai (jaise "God's-eye view"), lekin actual decision lete waqt (deployment), agent sirf apni local info use karta hai.
**In this paper:** MADDPG teesra baseline hai jis se ASAC compare hota hai. ASAC ke critic-design mein "neighbor action info" use karne ka idea MADDPG se inspired hai, lekin ASAC apne spatial-reward aur context-aware mechanisms se isse better perform karta hai.
**Real-life analogy:** Exam ki practice karte waqt tumhe sab dosto ke answers dekhne diye jaate hain (training mein full info), lekin actual exam mein sirf apna paper dekh sakti ho (deployment mein local info only).
**Easy way to remember:** MADDPG = "training mein sabka pata, real-time mein sirf apna pata."

---

## MAB (Full form: Multi-Armed Bandit)
**What it is (from scratch):** Ye naam ek casino ke "slot machine" (jisko "one-armed bandit" kehte hain — kyunki ye paise "loot" leta hai!) se aata hai. Socho kayi slot machines (multiple "arms") hain, aur tumhe baar-baar koi ek choose karna hai — kaunsi machine zyada deti hai, ye seekhna hai. **MAB** approach mein agent "state" ko track nahi karta (matlab "abhi situation kya hai" wala concept nahi hai) — sirf "kaunsa option average mein best raha hai" yaad rakhta hai.
**In this paper:** MAB teen baseline methods mein sabse achha perform karta hai — dense network mein reward = 4.23 (sabse close ASAC ke 4.89 ke). Phir bhi ASAC ne ise 16% se beat kiya, kyunki ASAC ka communication mechanism zyada efficient hai.
**Real-life analogy:** Ek bachha jo 5 ice-cream stalls try karta hai, aur "average mein kaunsa sabse tasty tha" yaad rakhkar future mein wahi choose karta hai — bina ye soche ki "aaj garmi hai ya mausam alag hai" (state ko ignore karta hai).
**Easy way to remember:** MAB = "best-average-option yaad rakhne wala," state/situation ko consider nahi karta.

---

## Q-value (Action-Value)
**What it is (from scratch):** **Q-value** ek number hai jo har "main is situation mein hote hue, ye action lun toh" combination ke liye hota hai — ye batata hai "agar main ye action lun, toh future mein mujhe total kitna reward milne ki ummeed hai." Jitna high Q-value, utna "achha" maana jaata hai woh action.
**In this paper:** Q-value update karne ke liye standard Bellman equation (RL ka core formula) use hota hai, jisme learning-rate aur time-discount-factor (delta=0.99) shamil hain. Extended version "Q-bar" mein neighborhood states aur neighbor policies bhi shamil ki jaati hain — taaki spatial/social context bhi reflect ho.
**Real-life analogy:** Restaurant menu mein har dish ko apna "mental rating" (1-10) dena based on past experience — order karte waqt highest-rated dish choose karna.
**Easy way to remember:** Q-value = "is action ka rating, future reward ke hisaab se."

---

## 3GPP (Full form: 3rd Generation Partnership Project)
**What it is (from scratch):** Ye ek international group hai jo mobile/cellular networks (4G, 5G, etc.) ke "rules" (technical standards) banata hai — jaise ek rulebook jisse follow karke saari companies ke devices/towers aapas mein compatible rehte hain.
**In this paper:** Introduction mein mention hota hai ki UAVs 3GPP-defined cellular technology ka use karke jaldi deploy ho sakte hain aur ground network coverage ko enhance karte hain — ye sirf "bigger picture/context" hai, paper ka core method isse directly nahi juda.
**Real-life analogy:** Jaise BIS (Bureau of Indian Standards) decide karta hai ki electrical plugs/sockets kaise design hone chahiye taaki sab compatible rahe — 3GPP cellular technology ke liye waisa hi karta hai.
**Easy way to remember:** 3GPP = "mobile network ka rulebook" jisse sab devices follow karte hain.

---

(5 most important terms marked with ⭐: UAV, MARL, ASAC, Actor-Critic, Adaptive Spatial Reward Mechanism, Spatial Discount Factor (kappa), Agent Context-Aware Communication Mechanism, LSTM, SINR — in saare concepts ko samjhe bina paper ka core idea samajhna mushkil hoga, isliye inhe pehle pakka karo. Especially **kappa (spatial discount) vs delta (temporal discount)** ka difference clearly samajhna — ye dono is paper mein saath chalte hain lekin bilkul alag cheezon ko control karte hain.)
