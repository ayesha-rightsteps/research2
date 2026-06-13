# Terminology: Resource Allocation and Sharing for UAV-Assisted Integrated TN-NTN with Multi-Connectivity

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai. Koi short-form/acronym bina poora naam aur matlab bataye nahi chhoda gaya.

---

## UAV (Full form: Unmanned Aerial Vehicle)
**What it is (from scratch):** Ek aircraft jisme koi pilot nahi hota — ise ya toh remote se control karte hain, ya woh khud (autonomously, AI/programming se) fly karta hai. Roz-marra ki language mein hum isko "drone" kehte hain.
**In this paper:** Poori paper UAVs (drones) ke around hai — 20 drones HCU role mein hain (bulk data bhejne wale), aur 20 pairs LCU role mein hain (aapas mein safety messages bhejne wale). Sab drones 70 km/h ki speed se, 0.1 km (100 meter) ki height pe fly kar rahe hain.
**Real-life analogy:** Jaise tumne dekha hoga delivery drones jo bina insaan ke khud udte hain aur package drop karte hain — wahi UAV hai.
**Easy way to remember:** UAV = "Unmanned" (bina pilot) + "Aerial" (hawa mein) + "Vehicle" (gaadi) = drone.

---

## TN (Full form: Terrestrial Network)
**What it is (from scratch):** "Terrestrial" ka matlab hota hai "zameen se related." Toh **Terrestrial Network** woh wireless network hai jo zameen par lage hue towers/infrastructure se chalta hai — jaise tumhare aas-paas dikhne wale mobile phone towers, jo cities mein har kuch kilometer pe lagaye gaye hain.
**In this paper:** TN ka represent karne wala element hai **RBS (Radio Base Station)** — woh zameen ka tower jisse HCU drones connect hote hain.
**Real-life analogy:** Jab tum apne phone se call karti ho aur tumhara phone nearest mobile tower se connect ho jaata hai — woh tower TN ka hissa hai.
**Easy way to remember:** TN = "zameen ka network" — jo cheez tum roz dekhti ho (mobile towers), woh TN hai.

---

## NTN (Full form: Non-Terrestrial Network)
**What it is (from scratch):** "Non-Terrestrial" matlab "zameen pe nahi." Toh **Non-Terrestrial Network** woh wireless network hai jiska infrastructure zameen par nahi, balki hawa mein (UAVs), stratosphere mein (HAPs), ya space mein (satellites) hota hai. Ye networks bohot bada area cover kar sakte hain — jaise ek satellite poore desh ko cover kar sakta hai — aur unhi jagahon mein bhi coverage de sakte hain jahan zameen ke towers lagana mushkil/expensive hai (jaise samandar, jungle, ya disaster-hit area).
**In this paper:** NTN ka represent karne wala element hai **HAP (High-Altitude Platform)** — 17 km upar flying platform jisse HCU drones connect hote hain.
**Real-life analogy:** Jab tum kisi remote jungle mein ho jahan mobile tower nahi pahunchta, lekin tumhara satellite phone phir bhi connect ho jaata hai — woh NTN hai.
**Easy way to remember:** NTN = "zameen se upar ka network" — UAV, HAP, satellite — sab "sky-based" connectivity providers.

---

## TN-NTN (Full form: Terrestrial Network – Non-Terrestrial Network Integration) ⭐
**What it is (from scratch):** Jab TN (zameen ke towers) aur NTN (sky-based platforms jaise HAP/satellite) dono ek saath, ek hi unified system ki tarah kaam karte hain — taaki coverage aur capacity dono badhe — usko **TN-NTN integration** kehte hain. Idea ye hai ki dono ke fayde combine ho jaayein: TN fast aur reliable hai jahan tower hai, NTN wahan coverage deta hai jahan TN nahi pahunchta.
**In this paper:** Poori paper ka system model isi TN-NTN integration par based hai — har HCU drone **dono** se ek saath connected hai: zameen ke RBS se (TN segment) aur upar ke HAP se (NTN segment). Yehi setup pure problem ka foundation hai.
**Real-life analogy:** Socho tumhare phone mein dono — mobile tower ka signal bhi hai aur satellite-based emergency-SOS feature bhi hai. Jab dono available hote hain aur system unhe smartly combine karta hai, woh TN-NTN integration jaisa hai.
**Easy way to remember:** TN-NTN = "zameen ka network + aasmaan ka network = ek saath."

---

## RBS (Full form: Radio Base Station)
**What it is (from scratch):** Ye basically ek normal mobile phone tower hai — wireless signals transmit/receive karne wala zameen par laga hua equipment, jo aas-paas ke devices ko network se connect karta hai.
**In this paper:** Ek RBS area ke center mein, 20 meter height par laga hua hai. Har HCU drone is RBS se connect hota hai — ye unka TN (terrestrial) connection hai, jo Multi-Connectivity ka ek hissa hai.
**Real-life analogy:** Wahi tower jiska signal tumhare phone ke top corner mein "4G" ya "5G" icon ke roop mein dikhta hai.
**Easy way to remember:** RBS = "zameen ka tower" — TN ka anchor point.

---

## HAP (Full form: High-Altitude Platform) ⭐
**What it is (from scratch):** Ye ek bohot bada flying object hai — aircraft, airship, ya balloon jaisa — jo stratosphere mein (zameen se lagbhag 17-22 km upar — yani normal commercial flights se bhi double height) udta hai, aur bohot lambe time tak roughly ek hi jagah "float" karta rehta hai. Apni height ki wajah se ye bohot bada area cover kar sakta hai — ek normal tower se kahin zyada.
**In this paper:** Is paper mein HAP 17 km ki height par, simulation area ke center ke seedha upar position kiya gaya hai. Har HCU drone HAP se bhi connect hota hai — ye unka NTN (non-terrestrial) connection hai, jo Multi-Connectivity ka doosra hissa hai.
**Real-life analogy:** Socho ek bohot bada balloon jo stratosphere mein "park" hua hai aur poore shehar ko WiFi-jaisi connectivity de raha hai — kabhi-kabhi news mein "internet balloons" project suna hoga, wo isi idea pe based hai.
**Easy way to remember:** HAP = "ek bohot upar flying tower" — satellite se neeche, normal tower se upar.

---

## HCU (Full form: High-Capacity User) ⭐
**What it is (from scratch):** Pehle samjho "capacity" ka matlab — capacity ka matlab hai "ek waqt mein kitna data transfer ho sakta hai." Ek **High-Capacity User** woh device/user hai jisko bohot zyada data transfer karna hai — jaise koi bada video file, ya continuous camera-feed, ya bulk sensor-data bhejna ho.
**In this paper:** HCU ek aisa drone hai jo bulk mission-data (jaise sensing data, camera feeds) bhejta hai. Is paper mein 20 HCUs hain. Har HCU **Multi-Connectivity** use karta hai — yani simultaneously RBS (zameen) aur HAP (upar) dono se connected rehta hai, taaki zyada se zyada data transfer kar sake. HCUs apna allocated spectrum LCUs ke saath share karte hain — jo paper ka main optimization-target hai.
**Real-life analogy:** Jaise tumhare ghar mein wo banda jo continuously Netflix 4K stream kar raha ho aur saath mein bade files upload kar raha ho — usko bohot bandwidth chahiye.
**Easy way to remember:** HCU = "High-Capacity" = "bohot data chahiye wala" — ye drones "heavy data senders" hain.

---

## LCU (Full form: Low-Capacity User) ⭐
**What it is (from scratch):** Ek **Low-Capacity User** woh device/user hai jisko bohot kam data bhejna hai (jaise sirf chhote text messages), lekin uska message **reliably** (guaranteed, bina fail hue) pohonchna bohot zaroori hai — kyunki woh message safety se related ho sakta hai.
**In this paper:** LCU is paper mein ek drone-**pair** hai (do drones jo aapas mein local UAV-to-UAV — drone-se-drone — messages bhejte hain), jaise collision-avoidance ya coordination updates. Is paper mein 20 LCU pairs hain. LCUs ka requirement "capacity" nahi balki "reliability" hai — unka SINR ek threshold (5 dB) se upar rehna chahiye, fail hone ka chance (outage probability) 10^-3 (0.1%) se kam rehna chahiye. LCUs apna spectrum HCUs ke saath share kar sakte hain (reuse), jisse spectrum waste nahi hota, lekin interference create hota hai.
**Real-life analogy:** Jaise ghar mein woh banda jo sirf "khana ban gaya, aa jao" jaisa chhota WhatsApp text bhejta hai — usse bohot speed nahi chahiye, lekin uska message pohonchna chahiye, fail nahi hona chahiye.
**Easy way to remember:** LCU = "Low-Capacity" = "kam data, lekin reliable hona chahiye" — ye drones "safety talkers" hain.

---

## MC (Full form: Multi-Connectivity) ⭐
**What it is (from scratch):** Normally ek device ek waqt mein sirf **ek** network/tower se connect hoti hai. **Multi-Connectivity** ka matlab hai — ek device ek **saath, simultaneously, do ya zyada** networks/towers se connected reh sakti hai. Isse do fayde milte hain: (1) agar ek connection slow/weak ho jaaye, doosra backup deta hai, aur (2) dono connections ki capacity mil ke total capacity badh jaati hai.
**In this paper:** Har HCU drone Multi-Connectivity use karta hai — ek saath RBS (zameen) se bhi connected hai aur HAP (upar) se bhi. Uski total capacity dono links ki capacity ka sum hai (RBS-link + HAP-link). Paper mein dikhaya gaya hai ki MC, single-connectivity (SC — sirf ek connection) se hamesha better perform karta hai.
**Real-life analogy:** Jaise tumhara phone kabhi-kabhi ek saath WiFi aur mobile data dono use kar leta hai — agar WiFi slow ho jaaye, mobile data madad karta hai, aur kabhi-kabhi dono mil ke total speed bhi badh jaati hai.
**Easy way to remember:** MC = "ek saath do connections" — "double the lifeline."

---

## SC (Full form: Single-Connectivity)
**What it is (from scratch):** Ye MC (Multi-Connectivity) ka opposite hai — matlab device sirf **ek** network/tower se connected hoti hai, ek waqt mein.
**In this paper:** SC ko ek comparison/baseline ke roop mein use kiya gaya hai — yani "agar HCU sirf ek hi link (sirf RBS, ya sirf HAP) use kare toh kya hota?" Result ye dikhata hai ki SC ki performance MC se kaafi kam hai — both sum capacity aur minimum capacity mein.
**Real-life analogy:** Jaise tumhare phone mein sirf mobile data hai, WiFi off hai — agar mobile signal weak ho jaaye, koi backup nahi hai.
**Easy way to remember:** SC = "ek hi lane" — MC ka "before" version.

---

## Spectrum / Spectrum Sharing
**What it is (from scratch):** Wireless signals "radio frequencies" ke through travel karte hain — aur ye frequencies ka pura available "range" **spectrum** kehlata hai. Spectrum limited hai — bohot saare devices ke beech baatna padta hai. **Spectrum Sharing** ka matlab hai: ek hi frequency-band ko ek se zyada users use karein — usually ek "primary" user ka woh band hota hai, aur ek "secondary" user usi band ko reuse kar leta hai, jab tak woh primary user ko zyada disturb na kare.
**In this paper:** Har HCU ko apna alag spectrum-band allocate hota hai. Phir ek **LCU** us HCU ke spectrum-band ko **reuse (share)** kar sakta hai — taaki spectrum waste na ho. Lekin isse interference create hoti hai dono taraf — yahi paper ka core trade-off hai jo optimize karna hai. Spectrum-sharing decide karne ke liye ek binary variable use hua hai jisko mu_{i,j} kehte hain — agar mu_{i,j} = 1, matlab LCU j, HCU i ka spectrum share kar raha hai.
**Real-life analogy:** Jaise tum apna WiFi password ek neighbor ko de do — tumhara internet primary hai, neighbor secondary hai. Agar neighbor zyada use kare, tumhara internet slow ho sakta hai — yehi "interference" hai.
**Easy way to remember:** Spectrum sharing = "ek hi frequency-slot, do users — jitna zyada share, utna zyada interference risk."

---

## Hungarian Method (HM) ⭐
**What it is (from scratch):** Ye ek bohot purana (1950s se), classic algorithm hai jo ek specific tarah ke problem ko solve karta hai — **assignment problem**. Assignment problem ka matlab hai: tumhare paas do groups hain (jaise "workers" aur "jobs"), aur har worker ka har job ke saath ek "score" (kitna achha match hai, ya kitna "cost" lagega) hai. Tumhe har worker ko exactly ek job assign karna hai (one-to-one), aisi tarah ki **total score sabse best ho** (ya total cost sabse kam ho) — sirf individually best matches chunne se nahi, balki **overall** best combination dhund ke. Hungarian Method ye guaranteed best answer dhund leta hai, aur ye polynomial time mein chal jaata hai (matlab "sab combinations try karo" jaisa exponentially-slow nahi hai — manageable time mein result mil jaata hai, jiska complexity O(n^3) hai, yani n³ steps lagbhag).
**In this paper:** Yahan "workers" hain HCUs aur "jobs" hain LCUs. Har HCU-LCU pair ka "score" hai — agar yeh dono spectrum share karein, total capacity kitni milegi. Hungarian Method is poori 20x20 matrix (grid) ko dekh kar nikalta hai ki kaunsa HCU kaunse LCU ke saath pair ho — taaki **total capacity** sabse zyada ho (Algorithm 1) ya **minimum capacity** sabse zyada ho (Algorithm 2, bisection ke saath combined). Yehi algorithm dono proposed methods ka "core engine" hai.
**Real-life analogy:** Socho ek class mein 20 students hain aur 20 projects hain. Har student ka har project ke saath alag "fit score" hai (kisi student ko coding project suit karta hai, kisi ko design). Agar tum random assign karo, kuch matches bekar honge. Hungarian Method ek tareeka hai jisse tum **mathematically guaranteed best overall assignment** nikal sakti ho — jisse total class ka "fit score" maximum ho.
**Easy way to remember:** Hungarian Method = "best matchmaker" — do groups ke beech sabse achha overall pairing, guaranteed.

---

## SINR (Full form: Signal-to-Interference-plus-Noise Ratio)
**What it is (from scratch):** Jab koi device wireless signal bhejti hai, woh signal receiver tak pohonchte-pohonchte do tarah ki "gadbad" ke saath mix ho jaata hai: **interference** (doosre transmitters ka signal jo galti se mix ho gaya) aur **noise** (random background disturbance, jaise static/hiss). **SINR** ek number hai jo compare karta hai: "mera asli signal kitna strong hai" VERSUS "interference + noise kitna strong hai." Jitna high SINR, utna clearly signal samajh aata hai.
**In this paper:** Teen SINR expressions define ki gayi hain — HCU ka SINR jab woh RBS ko signal bhejta hai, HCU ka SINR jab woh HAP ko signal bhejta hai, aur LCU ka SINR uski apni local pair ke saath. LCU ke liye ek hard requirement hai — uska SINR 5 dB se upar rehna chahiye, kam se kam 99.9% time (matlab fail hone ka chance — outage probability — 0.1% se kam ho).
**Real-life analogy:** Tumhare phone ke "signal bars" — full bars matlab high SINR (clear call), 1 bar ya "no service" matlab low SINR (call drop ho jaati hai ya awaaz kati-kati aati hai).
**Easy way to remember:** SINR = "signal ki clarity score" — jitna high, utna clear; jitna low, utna "doob gaya noise mein."

---

## Path Loss
**What it is (from scratch):** Jab koi wireless signal transmitter se nikalta hai aur hawa mein travel karta hai, woh distance ke saath weak hota jaata hai — exactly jaise tum jitna door khade ho kisi se baat karo, awaaz utni hi halki sunayi degi. **Path Loss** isi "distance ki wajah se signal kamzor hone" ko mathematically describe karta hai.
**In this paper:** Path loss ko ek formula se model kiya gaya hai jisme distance ka ek "power" (exponent) hota hai — yani distance double hone par signal sirf double kamzor nahi hota, balki usse zyada (jaise distance² ya distance^3 ke hisaab se). Ye formula har link (HCU-RBS, HCU-HAP, LCU-LCU) ke channel-strength calculation mein use hota hai.
**Real-life analogy:** Tum apne dost se 1 meter door khade ho toh awaaz clear sunayi degi; 100 meter door khade ho toh chillana padega. Distance badhne se signal "fade" hota hai.
**Easy way to remember:** Path loss = "distance ki wajah se signal weak hona" — door, woh kamzor.

---

## Shadowing (Log-Normal Shadowing)
**What it is (from scratch):** Sirf distance hi nahi, **obstacles** (buildings, trees, badi structures) bhi signal ko weak/block kar dete hain. Ye effect random hota hai — kabhi koi drone kisi building ke peeche se guzarta hai (signal bohot weak ho jaata hai), kabhi clear raaste se guzarta hai (signal strong rehta hai). Isi random, obstacle-ki-wajah-se hone wale signal-variation ko **shadowing** kehte hain.
**In this paper:** Shadowing ko ek random value se model kiya gaya hai (log-normal distribution — yani agar usko dB mein measure karo toh woh normal bell-curve jaisa distribute hota hai). Different links ke liye different shadowing strength use hui hai — jaise HCU-RBS link ke liye zyada (8 dB), aur HCU-LCU/HCU-HAP links ke liye kam (3 dB) — kyunki upar ki taraf obstacles kam hote hain.
**Real-life analogy:** Socho tum apne phone pe call kar rahi ho aur ek bus ke peeche chali jaati ho — signal thodi der ke liye weak ho jaata hai, phir bus hatne par wapas normal. Ye "random obstacle lottery" hi shadowing hai.
**Easy way to remember:** Shadowing = "raste mein aane wali random rukawaten" — building ke "shadow" mein signal kamzor.

---

## Rayleigh Fading / Small-Scale Fading
**What it is (from scratch):** Jab ek wireless signal transmitter se nikalta hai, woh ek seedhe raaste se nahi jaata — woh buildings, zameen, doosri cheezon se "bounce" (reflect) hota hai, aur uski multiple "copies" alag-alag raaston se receiver tak pohonchti hain. Ye copies aapas mein mil ke kabhi ek-doosre ko **strengthen** karti hain (signal strong ho jaata hai) aur kabhi **cancel** karti hain (signal weak ho jaata hai) — aur ye sab bohot tezi se, millisecond-by-millisecond hota rehta hai. Isi rapid, random ups-and-downs ko **small-scale fading** ya specifically **Rayleigh fading** kehte hain (jab koi single "direct" strong path na ho, sab paths reflected hon).
**In this paper:** Ye sabse "fast-changing" component hai — aur authors ne isko mathematically **average out** kar diya (ergodic capacity ke through), taaki sirf slow-changing components (path loss + shadowing) ke base par decisions le sakein. Closed-form capacity formula (E1(x) wala) derive karne mein iski statistical properties use hui hain.
**Real-life analogy:** Socho tum kisi tunnel mein gaadi chala rahi ho aur radio sun rahi ho — signal beech-beech mein "crackle" karta hai, kabhi clear kabhi static — ye multiple reflected signals ke aapas mein mix hone ka effect hai.
**Easy way to remember:** Rayleigh fading = "multipath ka lottery effect" — bahut tezi se signal strong-weak hota rehta hai, isiliye isko "average" karke ignore kiya gaya.

---

## Large-Scale CSI (Full form: Large-Scale Channel State Information)
**What it is (from scratch):** **CSI (Channel State Information)** ka matlab hai "channel ka current haal" — signal kitna weak/strong hai abhi. Iske do parts hote hain: **large-scale** (jo slowly badalta hai — path loss aur shadowing, jo seconds ya zyada time mein change hote hain) aur **small-scale** (jo bohot tezi se badalta hai — fading, milliseconds mein change). **Large-scale CSI** sirf woh "slowly-changing, big-picture" part hai.
**In this paper:** Authors ne deliberately apna pura resource-allocation decision **sirf large-scale CSI** par based kiya — small-scale (fast fading) ko average kar diya. Ye choice isliye ki gayi kyunki large-scale parameters hundreds of milliseconds tak stable rehte hain — chahe drone 70-80 km/h pe bhi fly kar raha ho — jisse calculations practical aur fast ho jaate hain, bina super-fast feedback ki zaroorat ke.
**Real-life analogy:** Google Maps ka "average traffic on this route" (jo dheere badalta hai) vs "is exact second is signal pe traffic" (jo har second badalta hai). Authors ne sirf "average traffic" wala data use kiya — kyunki woh kaafi der tak reliable rehta hai.
**Easy way to remember:** Large-scale CSI = "channel ki big-picture health" — slow-changing, isliye practical to use.

---

## Ergodic Capacity
**What it is (from scratch):** Pehle "capacity" samjho — capacity ka matlab hai "ek link per second, per Hz frequency mein kitna data bhej sakta hai" (unit: bps/Hz). Ab wireless channel random hai — kabhi strong, kabhi weak (fading ki wajah se). **Ergodic Capacity** ka matlab hai: in sab random ups-and-downs ka **long-term average** — yani "agar bohot lambi time tak observe karo, average capacity kya hogi."
**In this paper:** Har HCU ki capacity (RBS-link aur HAP-link dono ke liye) ko ergodic capacity ke roop mein define kiya gaya — yani fast-changing fading ko "expectation" (average) le ke nikala gaya. Ye formulation hi enable karta hai ki sirf large-scale CSI use ho, instantaneous CSI ki zaroorat na pade — yehi paper ki sabse important engineering choice hai.
**Real-life analogy:** Ek highway ka "average speed" — kabhi traffic hota hai (slow), kabhi clear road (fast), lekin overall average ek number deta hai jo long-term planning ke liye useful hai.
**Easy way to remember:** Ergodic capacity = "long-run average data rate" — moment-to-moment fluctuations ignore, overall average matters.

---

## Outage Probability (Po)
**What it is (from scratch):** **Outage** ka matlab hai "link fail ho gaya" — yani SINR ek minimum required level se neeche chala gaya, aur communication kaam nahi kar payi. **Outage Probability** batata hai "kitne percent time mein ye fail hone ka chance hai."
**In this paper:** LCU links ke liye outage probability ka maximum allowed value Po = 10^-3 (0.001, yani 0.1%) set kiya gaya hai — matlab LCU ka link 1000 mein se sirf 1 baar (ya kam) fail ho sakta hai, baaki 999 baar uska SINR threshold (5 dB) se upar rehna chahiye. Ye constraint ek formula (Eq. 14) ke through HCU ki transmit power par limit laga deta hai.
**Real-life analogy:** Jaise koi bridge "99.9% safe" certified ho — matlab 1000 mein se sirf 1 baar kuch galat ho sakta hai. LCU links ke liye bhi waisi hi high-reliability guarantee chahiye.
**Easy way to remember:** Outage probability = "fail hone ka chance" — Po = 0.1% matlab bohot rarely fail hoga.

---

## Bisection Search
**What it is (from scratch):** Ye ek simple numerical method hai jisse tum kisi equation ka answer dhund sakte ho bina seedha solve kiye — bas guess-aur-narrow-down karte ho. Tum ek "range" lete ho jisme answer ho sakta hai, beech wala point check karte ho, aur dekhte ho answer "upar wale half" mein hai ya "neeche wale half" mein — phir us half ko hi range bana lete ho, aur repeat karte ho. Har step mein range aadha ho jaata hai, isliye bohot jaldi (logarithmic steps mein) answer mil jaata hai.
**In this paper:** Bisection search do jagah use hua hai. Pehla: optimal power values nikalne ke liye (jab kisi function ka "monotone" — yani ek hi direction mein badhne/ghatne wala — behavior pata ho). Doosra: Algorithm 2 mein, candidate "minimum capacity" values ke beech best value dhundne ke liye, har step pe Hungarian Method se feasibility check karte hue.
**Real-life analogy:** "Hot-cold" guessing game — koi number 1-100 ke beech socha gaya hai, tum guess karti ho "50?", jawab milta hai "kam hai," toh tum 1-49 mein guess karti ho, aur isi tarah range halve hota jaata hai jab tak number mil jaaye.
**Easy way to remember:** Bisection search = "range ko baar-baar aadha karo jab tak answer mil jaaye."

---

## E1(x) (Full form: Exponential Integral Function, "E-one of x")
**What it is (from scratch):** Ye ek "special" mathematical function hai — matlab ek ready-made formula jo mathematicians ne already define kar rakha hai, jaise sin(x) ya log(x). Iska use hota hai jab tum kisi expression ko "average" karna chaho jisme log aur exponential dono involved hon, aur underlying randomness Rayleigh-fading-type ho.
**In this paper:** Ye function HCU ki closed-form (seedhe formula se nikalne wali) capacity expression mein appear hota hai (Equation 16). Iski wajah se authors ko har baar simulation/Monte-Carlo (random samples lekar average nikalna, jo slow hota hai) chalane ki zaroorat nahi padi — seedha is function ko evaluate karke fast answer mil jaata hai.
**Real-life analogy:** Jaise tumhare calculator mein ek "sin" button hota hai aur tumhe sin(30°) manually derive nahi karna padta — bas button dabao, answer mil jaata hai. E1(x) bhi waisा hi ek "ready-made tool" hai capacity calculate karne ke liye.
**Easy way to remember:** E1(x) = "capacity-formula ka special calculator button" — Rayleigh-fading capacity ko fast, closed-form mein nikalne ke liye.

---

## QoS (Full form: Quality of Service)
**What it is (from scratch):** **QoS** ka matlab hai — minimum performance standards jo ek connection ko meet karna chahiye taaki use "acceptable" maana jaaye. Ye standards data-rate, reliability, ya delay ke roop mein ho sakte hain — alag-alag applications ke alag-alag requirements hote hain.
**In this paper:** Do alag QoS requirements define ki gayi hain — HCUs ke liye minimum capacity requirement (kam se kam 0.5 bps/Hz unke major links pe), aur LCUs ke liye reliability requirement (5 dB SINR, 0.1% se kam outage probability). Ye dono requirements hi poori optimization problem ki "rules" hain.
**Real-life analogy:** Jaise ek video-call app ko minimum internet speed chahiye taaki video na ruke (speed-based QoS), lekin ek emergency-alert app ko sirf yeh chahiye ki message kabhi miss na ho (reliability-based QoS) — chahe woh chhota hi ho.
**Easy way to remember:** QoS = "service ka minimum acceptable standard" — har application ka alag standard hota hai.

---

## J/I Ratio
**What it is (from scratch):** Ye ek simple ratio hai — **I** represents HCUs ki total ginti, aur **J** represents LCU-pairs ki total ginti. **J/I Ratio** batata hai "har HCU ke against, kitne LCU-pairs hain" — yani spectrum-sharing ka "load level."
**In this paper:** Simulations mein J/I ko 0.2 se 1.0 tak vary kiya gaya. Jab J/I badhta hai, matlab zyada LCUs spectrum share karne ki koshish kar rahe hain — jisse interference badhta hai aur HCU capacity generally kam ho jaati hai. Algorithm 1 ka sum capacity, Algorithm 2 ke comparison mein, J/I badhne par "gracefully" (kam tezi se) degrade hota hai.
**Real-life analogy:** Socho ek office mein I = number of meeting rooms, aur J = number of teams jo unhe book karna chahti hain. J/I high hone ka matlab — har room ke liye zyada competition, jisse scheduling-conflicts (interference) badhte hain.
**Easy way to remember:** J/I ratio = "LCUs vs HCUs ka load-ratio" — jitna high, utna zyada spectrum-sharing competition.

---

## Sum Capacity
**What it is (from scratch):** "Sum" ka matlab hai "total/jod." **Sum Capacity** matlab — sab HCUs ki individual capacities ko add karke jo total number milta hai, woh poore network ka "overall throughput" represent karta hai.
**In this paper:** Algorithm 1 ka primary goal hai sum capacity ko maximize karna — har HCU ki (RBS-link + HAP-link) capacity ko jod kar, phir sab HCUs ka total. Simulation mein, 20 HCUs aur 20 LCUs ke saath J/I = 0.2 par, Algorithm 1 (with MC) approximately 950-1000 bps/Hz tak pohonchta hai.
**Real-life analogy:** Jaise ek company ki "total revenue" — har employee kitna kama raha hai uska sum, regardless ki kisi ek employee ki performance kaisi hai.
**Easy way to remember:** Sum capacity = "total network throughput" — sab HCUs ka combined data-rate.

---

## Minimum Capacity (Max-Min Capacity)
**What it is (from scratch):** **Minimum Capacity** ka matlab hai — poore network mein jo HCU sabse kam capacity pa raha hai, uski capacity. Isko maximize karne ka concept (**max-min**) ye hai: "sabse worst-off user ko bhi jitna acha bana sako, banao" — taaki koi bhi ek user bohot zyada peeche na reh jaaye.
**In this paper:** Algorithm 2 ka goal hai isi minimum capacity ko maximize karna. Result mein, Algorithm 2 consistently Algorithm 1 se higher minimum capacity deta hai — J/I = 0.2 par lagbhag 10-12 bps/Hz, jabki Algorithm 1 mein kuch HCUs bohot kam capacity pa sakte hain (kyunki Algorithm 1 sirf "total" ki chinta karta hai).
**Real-life analogy:** Class mein agar teacher "average marks badhao" ka goal rakhe (sum approach) vs "sabse weak student ke marks badhao" ka goal rakhe (max-min approach) — Algorithm 2 doosre type ka hai.
**Easy way to remember:** Minimum capacity = "sabse weak link ki capacity" — Algorithm 2 specifically isi ko behtar banata hai.

---

## Pareto Optimality
**What it is (from scratch):** Ek solution ko **Pareto Optimal** kaha jaata hai jab — koi bhi ek user ki performance ko improve karna ho, toh kisi **doosre** user ki performance ko worse karna hi padega. Matlab "easy/free improvements" sab khatam ho gaye hain — jo bhi gain hoga, woh kisi aur ke cost pe hoga.
**In this paper:** Algorithm 2 ka max-min solution Pareto optimal hone ka claim kiya gaya hai — kyunki minimum HCU capacity maximize karne ke baad, kisi bhi HCU ko aur better banane ke liye kisi doosre HCU ki capacity kam karni padegi.
**Real-life analogy:** Socho ek pizza already perfectly bant gaya hai sab logon mein based on unki needs — agar ab kisi ko aur slice dena ho, kisi doosre se slice lena hi padega. Koi "extra" slice bacha nahi hai.
**Easy way to remember:** Pareto optimal = "no free lunch" — improvement sirf trade-off se possible hai.

---

## Algorithm 1 (Sum HCU Capacity Maximization)
**What it is (from scratch):** Ye paper ka pehla proposed method hai — iska poora goal hai "total capacity ko maximum karna," subject to (yani is condition ke saath) LCU reliability aur HCU minimum-capacity requirements satisfy hote rehna chahiye.
**In this paper:** Do steps mein chalta hai — (1) har HCU-LCU pair ke liye optimal power closed-form formula (Equation 15) aur bisection search se nikalna, (2) resulting capacity-matrix par Hungarian Method chalakar best pairing pattern nikalna jo sum capacity maximize kare. Overall complexity O(JI log(1/epsilon) + I^3) hai — polynomial, manageable.
**Real-life analogy:** "Maximum total pie" approach — total throughput badhao, chahe kisi ko bada slice mile aur kisi ko chhota.
**Easy way to remember:** Algorithm 1 = "total ko maximize karo" version.

---

## Algorithm 2 (Minimum HCU Capacity Maximization / Max-Min Fairness)
**What it is (from scratch):** Ye paper ka doosra proposed method hai — iska goal hai "sabse weak HCU ki capacity ko bhi jitna ho sake utna acha banao" (max-min fairness), instead of total ko maximize karna.
**In this paper:** Algorithm 1 wali capacity-matrix par based hote hue, ek bisection search candidate minimum-capacity-values ke beech chalti hai, aur har step pe Hungarian Method se check hota hai "kya ye minimum sabhi HCUs ke liye achievable hai?" Complexity O(JI log(1/epsilon) + JI log(JI) + I^3 log I) hai. Yeh consistently Algorithm 1 se higher minimum capacity deta hai.
**Real-life analogy:** "Raise the floor" approach — sabse neeche wale ko upar laao, total chahe kam ho jaaye.
**Easy way to remember:** Algorithm 2 = "fairness ko maximize karo" version.

---

## Propulsion Power
**What it is (from scratch):** "Propulsion" ka matlab hai "aage badhne/move karne ki shakti." **Propulsion Power** woh energy hai jo ek UAV apne motors/rotors chalane mein use karta hai sirf hawa mein udte rehne aur move karne ke liye — ye communication (data bhejne) mein use hone wali energy se alag hai.
**In this paper:** Total UAV power do parts mein baata gaya hai — propulsion power (flying ke liye) + communication power (data bhejne ke liye). Inka total, ek time-period mein, UAV ki battery-energy consumption deta hai, jisko ek maximum limit (E^max) ke neeche rehna chahiye (Equation 12h).
**Real-life analogy:** Tumhare phone ki battery — kuch battery sirf screen-on rehne mein consume hoti hai (jaise "flying"), aur kuch data download/upload (jaise "communication") mein. Dono mil ke total drain karte hain.
**Easy way to remember:** Propulsion power = "udte rehne ka energy-cost" — communication power se alag.

---

## Spatial Poisson Process
**What it is (from scratch):** Ye ek mathematical/statistical tareeka hai random points ko ek area mein "place" karne ka — jisme har point ki position completely random aur independent hoti hai (kisi pattern ke bina), lekin overall "density" (kitne points per area) control ki ja sakti hai.
**In this paper:** UAVs ki starting positions ko simulation mein spatial Poisson process ke through random place kiya gaya — density UAV speed ke hisaab se vary hoti hai (jab UAVs slow hote hain, zyada dense; jab fast, kam dense — average inter-UAV distance, speed ka double rakha gaya).
**Real-life analogy:** Socho tum random seeds ek khet mein bina kisi pattern ke phenk rahi ho — kuch jagah pe close-close gir jaayenge, kuch jagah door-door, lekin overall density tum control kar sakti ho.
**Easy way to remember:** Spatial Poisson process = "random scattering with controlled density."

---

## Doppler Shift
**What it is (from scratch):** Jab transmitter aur receiver ek-doosre ke relative move kar rahe hote hain (ek paas aa raha ho ya door ja raha ho), toh signal ki frequency thodi badal jaati hai jo actually transmit hui thi — agar paas aa rahi ho toh frequency thodi badh jaati hai, door jaa rahi ho toh thodi kam ho jaati hai. Isi effect ko **Doppler Shift** kehte hain.
**In this paper:** Doppler shift ko formula se describe kiya gaya hai — jo UAV ki speed aur uski direction (RBS ke relative angle) par depend karta hai. Paper assume karta hai ki "Doppler compensation" already ho jaata hai (system isko handle kar leta hai), isliye iska effect resource-allocation decisions par directly nahi padta.
**Real-life analogy:** Ambulance ki siren — jab ambulance tumhari taraf aa rahi hoti hai, siren ki pitch high lagti hai; jab door ja rahi hoti hai, pitch low lagti hai. Wireless signals mein bhi same effect hota hai, frequency ke saath.
**Easy way to remember:** Doppler shift = "ambulance-siren effect" — relative motion se frequency change.

---

## Interference Channel
**What it is (from scratch):** Ek **interference channel** woh "raasta" hai jisse ek device ka signal, kisi **doosre** (unintended) device ke receiver tak pohonch jaata hai — aur waha jaake uska "apna" signal samajhna mushkil bana deta hai.
**In this paper:** Spectrum-sharing ki wajah se chaar interference-channels exist karte hain — LCU-se-RBS, HCU-se-LCU, LCU-se-HAP, aur unke reverse paths. Yehi channels HCU aur LCU ke SINR expressions ko "coupled" (aapas mein jude hue) banate hain — ek ka signal doosre ke liye "noise" ban jaata hai.
**Real-life analogy:** Socho do log alag-alag phone calls kar rahe hain ek hi room mein, aur ek ki awaaz dusre ke phone ke microphone mein bhi chali jaati hai — ye "cross-talk" hi interference channel hai.
**Easy way to remember:** Interference channel = "cross-talk path" — kisi aur ka signal tumhare receiver mein leak ho jaana.

---

## UAV-to-UAV Link (LCU-LCU Link)
**What it is (from scratch):** Ye ek direct wireless connection hai do drones ke beech — koi tower/HAP beech mein nahi aata, dono drones seedhe ek-doosre se baat karte hain.
**In this paper:** Yeh LCU links hain — local coordination aur safety messaging ke liye use hote hain (jaise collision avoidance). Ye HCUs ke saath shared spectrum pe operate karte hain, jisse cross-interference hoti hai. Inka requirement high-reliability hai, high-capacity nahi.
**Real-life analogy:** Walkie-talkie se seedha doosre walkie-talkie se baat karna — bina kisi central operator ke through gaye.
**Easy way to remember:** UAV-to-UAV link = "drone-se-drone seedha walkie-talkie" — yehi LCU links hain.

---

## 3GPP (Full form: 3rd Generation Partnership Project)
**What it is (from scratch):** Ye ek international organization hai jo mobile networks (3G, 4G, 5G, aur ab 6G) ke "rules"/technical-specifications likhti hai — taaki duniya bhar ke phones/towers/networks ek-doosre ke saath compatible rahein.
**In this paper:** 3GPP ko reference kiya gaya hai us organization ke roop mein jisne Multi-Connectivity ko ek key 6G feature ke roop mein standardize kiya, aur connected-UAVs ke liye specific interference-related rules (Release 15) bhi define kiye.
**Real-life analogy:** Jaise ek "rules committee" jo decide karti hai ki USB-C port universal hoga — taaki har company ke devices compatible rahein. 3GPP wahi role mobile networks ke liye karta hai.
**Easy way to remember:** 3GPP = "global mobile-network rules-committee."

---

(5 most important terms marked with ⭐: TN-NTN, HCU, LCU, MC, Hungarian Method — in paanch concepts ke bina paper ka page-1 bhi samajhna mushkil hoga, isliye inhe pehle pakka karo. Baaki terms — especially SINR aur Spectrum Sharing — bhi bohot zaroori hain, but ye 5 sabse "core" hain.)
