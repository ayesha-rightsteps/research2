# Terminology: GNN + DRL for V2X Resource Allocation

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai. Koi short-form/acronym bina poora naam aur matlab bataye nahi chhoda gaya.

---

## V2X (Full form: Vehicle-to-Everything) ⭐
**What it is (from scratch):** Tumhara phone "wireless" hai — matlab ye baaton (data) ko invisible radio waves se bhejta/receive karta hai, bina kisi taar (wire) ke. Ye radio waves hawa mein travel karte hain, jaise FM radio. Ab agar gaadiyon mein bhi aise hi radio-wave transmitters/receivers laga diye jaayein, toh gaadiyaan bhi "wireless baat" kar sakti hain — kisi se bhi: doosri gaadi se, road ke kinare lagi tower se, ya pedestrian (paidal chalne wale) ke phone se. Isi pure concept ko **V2X (Vehicle-to-Everything)** kehte hain — gaadi "everything" (sab kuch) se baat kar sakti hai.
**In this paper:** Ye poori paper isi broad concept ke andar hai — paper specifically **C-V2X** (neeche dekho) pe focus karta hai.
**Real-life analogy:** Jaise tumhare paas ek walkie-talkie ho jisse tum apne dost se, ghar ke intercom se, aur ek bada announcement-tower se — sab se baat kar sako, alag-alag tareeke se.
**Easy way to remember:** "X" = "everything" — gaadi sab se connect ho sakti hai.

---

## C-V2X (Full form: Cellular Vehicle-to-Everything) ⭐
**What it is (from scratch):** Jab tumhara phone internet chalata hai, woh "cellular network" use karta hai — matlab phone companies ke towers (4G/5G) ka network, jo already har jagah laga hua hai. **C-V2X** ka matlab hai: gaadiyon ka wireless communication bhi isi already-existing cellular (4G/5G) infrastructure ke through ho — naya alag system banane ki zaroorat nahi.
**In this paper:** Ye paper isi C-V2X standard ke "rules" follow karta hai — jaise spectrum ke chunks (resource blocks) aur power levels kaise define hote hain, woh sab C-V2X specifications se aata hai.
**Real-life analogy:** Jaise naya app banane ke liye tum apna alag internet network nahi banati — existing WiFi/mobile data use karti ho. C-V2X bhi waise hi existing phone-network infrastructure use karta hai.
**Easy way to remember:** "C" = "Cellular" = tumhare phone ka network — gaadi bhi usi network pe chalti hai.

---

## V2V (Full form: Vehicle-to-Vehicle) ⭐
**What it is (from scratch):** Ye V2X ka ek specific type hai jisme **sirf gaadi-se-gaadi** seedhe baat hoti hai — bina kisi tower ke beech mein aaye. Jaise do walkie-talkies seedhe ek-doosre se baat kar lein.
**In this paper:** V2V ka use **safety messages** ke liye hota hai — jaise "main brake laga raha hoon" ya "aage accident hai." Yeh messages itne important hain ki paper mein requirement hai ki **95% se zyada** successfully deliver hone chahiye — kyunki agar fail ho jaaye toh real accident ho sakta hai.
**Real-life analogy:** Tum apne saath wali gaadi ke driver ko seedha shout karke bata do "bhai brake lagao!" — bina kisi operator/tower ke through gaye.
**Easy way to remember:** V2V = "Vehicle se seedha Vehicle" — dono "V" same level pe hain, koi beech mein nahi.

---

## V2I (Full form: Vehicle-to-Infrastructure) ⭐
**What it is (from scratch):** Ye V2X ka woh type hai jisme gaadi **road ke kinare lagi infrastructure** (jaise ek tower/base station, jisko "roadside unit" ya "base station" kehte hain) se baat karti hai — data bhejne/lene ke liye, jaise maps update, traffic info, ya streaming.
**In this paper:** V2I links **high data rate (throughput)** ke liye important hain — yani "speed" zaroori hai, lekin agar kabhi-kabhi thoda slow ho jaaye toh life-threatening nahi hai (V2V ke comparison mein).
**Real-life analogy:** Jaise tum apne phone se ek WiFi router (jo fix jagah laga hai) se connect karke internet chalati ho — gaadi bhi waise hi roadside tower se "connect" hoti hai data ke liye.
**Easy way to remember:** V2I = "Vehicle se Infrastructure (tower) tak" — ek end fixed hai (tower), ek end move kar rahi hai (gaadi).

---

## Resource Block (RB) / Sub-channel
**What it is (from scratch):** Radio waves ek bohot bada "spectrum" (range of frequencies) cover karte hain — lekin ek hi waqt mein har frequency par sirf limited devices baat kar sakte hain (jaise FM radio mein 90.5, 98.3, 101.2 alag-alag stations hain, aur tum ek waqt mein sirf ek hi sun sakte ho). Network engineers is bade spectrum ko chhote-chhote "tukdon" mein todte hain — har tukda ek specific frequency-range + time-slot hota hai. Isi chhote tukde ko **Resource Block (RB)** ya **sub-channel** kehte hain. Ek device (gaadi) ko ek time pe ek ya kuch RBs assign hote hain jisi par woh transmit kar sakti hai.
**In this paper:** Har gaadi (har V2V/V2I link) ko har time-step pe ek RB aur ek power level assign karna hai. Limited RBs hain, lekin bahut saari gaadiyaan — isi assignment ko optimize karna hi pura problem hai.
**Real-life analogy:** Ek highway pe limited lanes (RBs) hain, lekin bohot saari gaadiyaan (links) hain jinhe kisi lane mein chalna hai. Agar do gaadiyaan ek hi lane mein side-by-side chalne ki koshish karein, toh takkar (interference) ho jaati hai.
**Easy way to remember:** RB = ek "lane" jo spectrum mein hoti hai, gaadi ko milti hai.

---

## SINR (Full form: Signal-to-Interference-plus-Noise Ratio) ⭐
**What it is (from scratch):** Jab ek gaadi transmit karti hai, uska signal doosre transmitter ke "interference" aur environment ke random "noise" (background disturbance, jaise static/hiss) ke saath mix hokar receiver tak pahunchta hai. **SINR** ek number hai jo compare karta hai: "mera asli signal kitna strong hai" VS "interference + noise kitna strong hai." Agar SINR high hai, matlab signal interference/noise se bohot zyada strong hai — message clearly samajh aayega. Agar SINR low hai, signal interference/noise mein "doob" jaata hai — message kharab ho jaata hai.
**In this paper:** Ek V2V link "successful" tabhi count hota hai jab uska SINR ek certain threshold se upar ho. V2I ka data rate (throughput) bhi SINR par depend karta hai — jitna better SINR, utna zyada data bhej sakte ho.
**Real-life analogy:** Tumhare phone mein "signal bars" hote hain — full bars matlab high SINR (clear call), 1 bar ya "no service" matlab low SINR (call drop ho jaati hai).
**Easy way to remember:** SINR jitna zyada, "voice" jitni clearly sunayi de — kam SINR matlab sab "noise" mein doob gaya.

---

## CSI (Full form: Channel State Information)
**What it is (from scratch):** Wireless signal jab transmitter se receiver tak jaata hai, raaste mein woh buildings, trees, doosri gaadiyon se reflect/weaken hota hai — is "raaste ka current haal" (kitna signal kamzor hua, kitna delay hua, etc.) ko **CSI** kehte hain. Ye basically "abhi is waqt channel kaisa hai" ka real-time report card hai.
**In this paper:** CSI ka use interference-graph banane aur resource decisions lene mein hota hai. Problem ye hai ki gaadiyaan tez chal rahi hain (high speed), toh CSI bahut jaldi purana/outdated ho jaata hai.
**Real-life analogy:** Jaise Google Maps mein "live traffic" — ye batata hai ki abhi road ka haal kaisa hai. Lekin agar tumhara phone 10 second purana data dikha raha ho, toh woh "outdated" traffic info galat decision dila sakta hai.
**Easy way to remember:** CSI = "channel ka abhi ka health report."

---

## GNN (Full form: Graph Neural Network) ⭐
**What it is (from scratch):** Pehle samjho "graph" kya hai (normal graph/chart nahi — yeh ek alag concept hai): ek **graph** mein **nodes** (points/circles, jaise log) hote hain aur **edges** (lines jo do nodes ko connect karti hain, jaise dosti/connection) hote hain. Jaise Facebook ka "friend network" — har insaan ek node hai, har friendship ek edge hai. Ab **GNN** ek aisa neural network hai jo specifically aise graphs par kaam karne ke liye design hua hai. Ye har node ko apne connected (neighbor) nodes se "info lene" deta hai — jaise tum apne dost se poochho "tu kya kar raha hai" aur uska jawab apni samajh mein add kar lo. Is process ko repeat karne se har node ko apne aas-paas ka ek "smart summary" mil jaata hai.
**In this paper:** Yahan, har communication link (V2V ya V2I) ek **node** hai, aur do links jo ek-doosre ko interfere kar sakte hain unke beech ek **edge** hai. GNN har link ko batata hai: "tere aas-paas wale links kya kar rahe hain, kahan interference ho sakta hai" — bina ek central computer ko poora network dekhne ki zaroorat ke.
**Real-life analogy:** Socho ek WhatsApp group mein har member apne nearby members se "status update" lekar apna decision banata hai — bina group admin se poochne ke. GNN bhi har node ko "apne paas walon ka summary" deta hai.
**Easy way to remember:** GNN = neural network jo "kaun kis se connected hai" (graph) ko samajh kar smart features banata hai.

---

## GraphSAGE (Full form: Graph SAmple and aggreGatE) ⭐
**What it is (from scratch):** GraphSAGE ek specific tarah ka **GNN** (upar dekho) hai. Normal GNN ko kaam karne ke liye kabhi-kabhi poora graph ek saath dekhna padta hai — jo slow ho sakta hai agar graph bada ho ya har second badal raha ho. GraphSAGE iska solution deta hai: har node apne **sab** neighbors se nahi, balki **kuch random-sample liye gaye** neighbors se info leta hai, aur unko "aggregate" (mix/combine) karke apna summary banata hai. Isse ye fast rehta hai aur naye/badalte graphs par bhi kaam kar sakta hai (jisko "inductive" kehte hain — naye graph par bhi bina dobara-training kaam karna).
**In this paper:** Har time step pe gaadiyon ka graph badal jaata hai (kyunki gaadiyaan move kar rahi hain) — GraphSAGE isi changing graph ke saath fast kaam kar sakta hai, jo ye paper ke real-time requirement ke liye perfect hai.
**Real-life analogy:** Agar tumhe apne poore mohalle ka mood janna ho, tum **har ghar** mein nahi jaa sakti — tum kuch (random sample) ghar jaake poochti ho aur unke jawab se ek overall andaza laga leti ho. GraphSAGE bhi yahi karta hai — sab neighbors ke jagah, ek sample.
**Easy way to remember:** "SAmple and aggreGatE" — naam mein hi process likha hai: kuch sample lo, mix karo.

---

## DRL (Full form: Deep Reinforcement Learning) ⭐
**What it is (from scratch):** Pehle **Reinforcement Learning (RL)** samjho: ye ek tarah ka learning hai jisme ek "agent" (decision-maker) apne environment ko dekhta hai (**state**), ek **action** leta hai, aur uske result mein usko ek **reward** (number — achha kaam kiya toh positive, bura kiya toh kam/negative) milta hai. Agent dheere-dheere seekhta hai ki kis state mein kaunsa action best reward deta hai — exactly jaise tum kisi pet ko treat dekar train karti ho: achha kaam → treat, bura kaam → kuch nahi. **Deep** RL matlab ye "seekhne wala dimaag" ek **neural network** hai — jo bahut complex/large state-spaces ko handle kar sakta hai (jab states bahut zyada/complicated hote hain, simple lookup-table tareeka kaam nahi karta, neural network chahiye).
**In this paper:** DRL hi decision-making ka "core engine" hai — har gaadi (agent) seekhti hai ki kis situation mein kaunsa channel + power best hai, trial-and-error (training) ke through.
**Real-life analogy:** Ek bachhe ko cycle chalana sikhana — woh girta hai (negative reward/bad outcome), balance banata hai (positive reward), aur baar-baar try karke seekh jaata hai — bina kisi ne use exact "rules" likh kar diye ho.
**Easy way to remember:** RL = trial-and-error se seekhna; "Deep" = isme neural network ka "dimaag" use hota hai.

---

## DDQN (Full form: Double Deep Q-Network)
**What it is (from scratch):** Pehle **DQN (Deep Q-Network)** samjho: ye ek DRL technique hai jisme neural network har possible action ke liye ek **Q-value** seekhta hai (Q-value ka matlab neeche separate term mein hai — basically ek "goodness score"). Agent woh action choose karta hai jiska Q-value sabse high ho. Problem ye hai ki plain DQN apne Q-values ko kabhi-kabhi **zyada estimate (overestimate)** kar deta hai — matlab woh kisi action ko "bahut achha" samajh leta hai jab woh actually itna achha nahi hota, jisse training unstable ho jaati hai. **DDQN** isko fix karta hai do separate networks use karke: ek network "action choose" karta hai, aur ek doosra network us action ki "quality check" karta hai — taaki overconfidence kam ho.
**In this paper:** DDQN hi woh final "decision-maker" hai jo GNN se mile features lekar batata hai — "is gaadi ko kaunsa channel aur kaunsi power use karni chahiye."
**Real-life analogy:** Jaise koi bada decision lete waqt — ek dost suggestion deta hai ("ye karo"), aur dusra dost double-check karta hai ("sach mein? sahi hai kya?") — taaki galat overconfidence na ho.
**Easy way to remember:** "Double" = do networks, ek dusre ko check karte hain.

---

## Q-value
**What it is (from scratch):** **Q-value** ek number hai jo har "state + action" combination ke liye hota hai, aur ye batata hai: "agar main is state mein hote hue ye action lun, toh future mein mujhe total kitna reward milne ki expectation hai." Jitna high Q-value, utna "achha" maana jaata hai woh action. RL agent basically seekhta hai ki har situation mein kis action ka Q-value sabse high hai, aur wahi action leta hai.
**In this paper:** Agent Q(state, action) seekhta hai taaki har situation mein best resource-allocation action (channel+power) choose kar sake.
**Real-life analogy:** Restaurant menu mein har dish ka tumhara apna "mental rating" (1-10) hota hai based on past experience — jab order karna ho, tum highest-rated dish choose karti ho. Q-value bhi waisi hi ek "rating" hai, jo agent experience se seekhta hai.
**Easy way to remember:** Q-value = "is action ka rating, future reward ke hisaab se."

---

## IoV (Full form: Internet of Vehicles)
**What it is (from scratch):** Tumne "Internet of Things (IoT)" suna hoga — matlab roz-marra ke devices (lights, fridges, watches) internet se connected hote hain aur smart decisions le sakte hain. **IoV** isi idea ko gaadiyon par apply karta hai — gaadiyaan internet/cloud se connected hain, ek-doosre se data share karti hain, aur AI ke through smart driving decisions (jaise autonomous driving) lene mein madad milti hai.
**In this paper:** IoV sirf "bigger picture" / application context hai — V2X resource allocation IoV ko possible banane wali ek foundational technology hai.
**Real-life analogy:** Jaise tumhara smartphone "internet of things" ka hissa hai (apps, cloud, notifications) — IoV mein gaadi bhi waisी hi "internet-connected smart device" ban jaati hai.
**Easy way to remember:** IoV = "smart, connected gaadiyon ki duniya."

---

## NP-Hard
**What it is (from scratch):** Kuch problems mein, jaise-jaise input ka size badhta hai (yahan: jaise-jaise gaadiyon ki ginti badhti hai), **possible solutions ki ginti exponentially (bahut tezi se) badh jaati hai** — itni tezi se ki ek powerful computer bhi "sab options check karke best dhundo" wala kaam reasonable time mein nahi kar sakta. Aise problems ko **NP-hard** kehte hain. Example: agar 3 gaadiyaan aur 3 channels hain, options manageable hain. Lekin 50 gaadiyaan aur kayi channels + power-levels ho, toh combinations itni ho jaati hain ki "sab try karo" approach practically impossible ho jaata hai.
**In this paper:** Yahi reason hai ki classical "exact" optimization (sab combinations check karna) real-time vehicular network mein kaam nahi karta — isi liye DRL (learning-based) approach use kiya gaya, jo ek baar train hone ke baad **instantly** (bina sab combinations check kiye) ek "achhi" decision de sakta hai.
**Real-life analogy:** Socho tumhe 50 dosto ke beech seating arrangement decide karna hai taaki sab khush rahein — agar tum HAR POSSIBLE arrangement try karke best dhundogi, toh saalon lag jaayenge. Isiliye log "smart shortcuts" (jaise yahan DRL) use karte hain.
**Easy way to remember:** NP-Hard = "itne options hain ki sab check karna practically impossible hai."

---

(5 most important terms marked with ⭐: V2X, C-V2X, V2V, V2I, SINR, GNN, GraphSAGE, DRL — in saare concepts bina samjhe paper ka koi bhi page samajhna mushkil hoga, isliye inhe pehle pakka karo.)
