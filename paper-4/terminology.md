# Terminology: Joint Optimization of Trajectory Control, Resource Allocation, and Task Offloading for Multi-UAV-Assisted IoV

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai. Koi short-form/acronym bina poora naam aur matlab bataye nahi chhoda gaya.

---

## UAV (Full form: Unmanned Aerial Vehicle) ⭐
**What it is (from scratch):** "Unmanned" matlab "bina insaan ke," "Aerial" matlab "hawa mein/udta hua," aur "Vehicle" matlab "gaadi/machine." Toh **UAV** ek aisi flying machine hai jisme koi pilot baitha nahi hota — ya toh ye remote se control hoti hai, ya khud-ba-khud (autonomously) udती hai. Roz-marra ki zindagi mein hum isko **drone** kehte hain — jaise woh chhote camera-wale drones jo log shaadiyon mein photos lene ke liye use karte hain.
**In this paper:** Yahan UAVs sirf photo lene ke liye nahi hain — har UAV ke saath ek chhota computer (server) bhi attached hai, jo neeche chal rahi gaadiyon ka heavy computational kaam process kar sakta hai. Total **5 UAVs** city ke upar udते hue, gaadiyon ko cover karte hain.
**Real-life analogy:** Socho ek delivery-drone jiske paas, package deliver karne ke alawa, ek chhota laptop bhi strapped hai — jo udते-udते logon ka homework solve karke wapas bhej sakta hai.
**Easy way to remember:** UAV = "bina driver wala flying machine" = drone.

---

## IoV (Full form: Internet of Vehicles) ⭐
**What it is (from scratch):** Pehle tumne "Internet of Things (IoT)" ka naam suna hoga — matlab roz-marra ke devices (lights, fridge, watch) internet se connect ho jaate hain aur smartly kaam karte hain. **IoV** isi idea ko gaadiyon par apply karta hai — har gaadi ek "connected device" ban jaati hai jo internet/cloud se judi hoti hai, doosri gaadiyon se data share karti hai, aur smart driving decisions (jaise self-driving) lene mein madad leti hai.
**In this paper:** IoV poori paper ka "bada context" hai — saari 50 gaadiyaan IoV ka part hain, jo apne sensors/cameras se data collect karke heavy computation tasks generate karti hain (jaise object detection — road par car/insaan/signal pehchanna, ya HD map updates).
**Real-life analogy:** Jaise tumhara smartphone "smart, connected device" hai jo apps/cloud se baat karta hai — IoV mein har gaadi waisa hi ek "smart, connected device" ban jaati hai, bas size badi hai aur road par chalti hai.
**Easy way to remember:** IoV = "gaadiyon ka apna internet" — har gaadi connected aur smart hai.

---

## Task Offloading ⭐
**What it is (from scratch):** Socho tumhare laptop pe ek bahut heavy calculation chal rahi hai — laptop hang ho gaya, slow chal raha hai. Agar tumhare paas option ho ki ye kaam ek powerful desktop computer ko bhej do, woh fast process karke result wapas de de — to tumhara laptop free ho jaayega aur kaam bhi fast hoga. **Task offloading** isi process ka naam hai: apna computational kaam (task) kisi doosri, zyada powerful machine ko "offload" (bhej) dena, aur sirf final result wapas le lena.
**In this paper:** Har gaadi ke paas apna chhota onboard-computer hai, jo heavy tasks (jaise object detection) process karne mein slow hai. Toh gaadi apna task **partially ya fully** kisi UAV ya Base Station ko offload kar sakti hai — yeh decide karna hi "offloading ratio" ka problem hai, jo LP (neeche dekho) se solve hota hai.
**Real-life analogy:** Apna heavy homework ek tutor ko de dena jo tumse fast solve kar sakta hai — tumhe sirf final answer chahiye, kaam karne ka process nahi.
**Easy way to remember:** "Off-load" = apna load (kaam) kisi aur par daal dena.

---

## MEC (Full form: Mobile Edge Computing) ⭐
**What it is (from scratch):** Normally, jab tum koi heavy computation cloud ko bhejti ho, woh data bahut door ek "data center" tak jaata hai (jo kisi doosre shehar/desh mein bhi ho sakta hai), wahan process hota hai, aur wapas aata hai — isme time (latency) lagta hai. **Edge Computing** ka idea hai: computing power ko "edge" (network ke kinare) par rakho — yaani jitna ho sake user ke **paas** — taaki ye lambi journey (delay) bach jaaye. "**Mobile**" Edge Computing matlab ye edge-servers khud bhi move kar sakte hain (jaise yahan, UAVs ke upar laga server udते hue ghoomta hai).
**In this paper:** MEC hi pure system ka foundation hai — UAVs "aerial MEC nodes" (udte hue edge-computers) ki tarah kaam karte hain. Gaadiyaan apna kaam UAV-mounted ya Base-Station-mounted MEC servers ko bhejti hain, taaki unka 1-second ka deadline meet ho sake.
**Real-life analogy:** Apna homework city ke doosre kone mein baithe expert ko mail karne ke bajaye, apne paas khade ek tutor ko de dena — jawaab turant milega.
**Easy way to remember:** MEC = "computing power ko user ke paas le aana," na ki user ko door computing center tak bhejna.

---

## BS (Full form: Base Station)
**What it is (from scratch):** Tumhara phone jab call/internet use karta hai, woh ek bade fixed "tower" se connect hota hai jo aas-paas ke sab phones ko signal deta hai — isi tower ko **Base Station** kehte hain. Ye ground par fix hota hai, move nahi karta.
**In this paper:** Is paper ke 300m x 300m wale city-area ke beech mein ek **single Base Station** khada hai. Ye sab gaadiyon ka V2I (gaadi-se-tower) traffic handle karta hai, aur iska compute-power sabse zyada (9 GHz) hai — lekin kyunki sab gaadiyaan isी ek tower ko use kar rahi hain, peak time pe yahan "lambi line" (queuing delay) lag jaati hai.
**Real-life analogy:** Tumhare mohalle ka ek hi mobile-tower jisse sabke phone connect hote hain — sabse powerful hai, lekin agar sab ek saath use karein toh network slow ho jaata hai.
**Easy way to remember:** BS = "fix jagah khada bada tower jisse sab gaadiyaan connect ho sakti hain."

---

## OBU (Full form: On-Board Unit)
**What it is (from scratch):** "On-Board" matlab "gaadi ke andar laga hua." **OBU** matlab gaadi ke andar lage hue computer + communication-radio ka set — yaani gaadi ka apna "built-in laptop + wireless device."
**In this paper:** Har gaadi ka OBU ek fix compute-speed (0.5 GHz) par chalta hai — jo UAV (5 GHz) ya BS (9 GHz) ke compute-speed se kaafi kam hai. Isi wajah se heavy tasks ko offload karna fayda-mand hai — gaadi ka apna OBU slow hai, toh kaam baahar bhejna better hai.
**Real-life analogy:** Gaadi ka OBU ek "entry-level laptop" jaisa hai — chhote kaam kar lega, lekin heavy editing/calculation mein hang ho jaayega.
**Easy way to remember:** OBU = "gaadi ka apna chhota onboard computer."

---

## V2I (Full form: Vehicle-to-Infrastructure)
**What it is (from scratch):** "Vehicle-to-Infrastructure" matlab "gaadi se fixed-infrastructure (jaise tower) tak" wireless communication. Gaadi seedhe ek fix-jagah-khade tower (Base Station) se baat karti hai — data bhejne/lene ke liye.
**In this paper:** Har gaadi har waqt BS se V2I link ke through connect ho sakti hai. Ye link "free-space path loss" (signal hawa mein travel karte hue kamzor padna), "shadow fading," aur "fast Rayleigh fading" (signal strength mein random ups-and-downs, jaise buildings/movement ki wajah se) jaise effects se gujarta hai — jo signal ki quality ko affect karte hain.
**Real-life analogy:** Tumhara phone WiFi-router se connect hota hai — router fix hai, tumhara phone move kar rahaa hai, lekin connection bana rehta hai (signal strength change hoti rehti hai).
**Easy way to remember:** V2I = "gaadi se fix-tower tak" connection.

---

## V2V (Full form: Vehicle-to-Vehicle)
**What it is (from scratch):** "Vehicle-to-Vehicle" matlab do gaadiyon ke beech **seedha** (directly, bina kisi tower ke beech mein aaye) wireless communication — jaise ek gaadi doosri gaadi ko "main brake laga raha hoon" jaisa safety-message bheje.
**In this paper:** V2V ka concept introduction mein IoV ke broader context ke roop mein mention hua hai, lekin is paper ka apna system-model V2V ko directly model nahi karta — ye paper V2I (gaadi-tower) aur V2U (gaadi-drone) links pe focus karta hai.
**Real-life analogy:** Do dost seedhe ek-doosre se whisper kar rahe hain — beech mein koi operator/tower nahi.
**Easy way to remember:** V2V = "Vehicle se seedha Vehicle," koi beech mein nahi.

---

## G2A (Full form: Ground-to-Air)
**What it is (from scratch):** "Ground-to-Air" matlab "zameen (ground device) se hawa mein udते (airborne) device tak" wireless link. Yahan "ground device" matlab gaadi hai, aur "airborne platform" matlab UAV.
**In this paper:** G2A link wo connection hai jisse gaadi neeche se UAV ko signal bhejti hai. Ye link **LoS/NLoS** (neeche dekho) probability model use karke describe kiya gaya hai — jo depend karta hai gaadi aur UAV ke beech ke "elevation angle" (matlab UAV gaadi ke seedha upar hai ya tirछा/door) par. UAV jitna seedha upar hoga, LoS chance utna zyada — lekin agar UAV bahut upar hai, toh distance badhne se signal weak (path loss zyada) ho jaata hai.
**Real-life analogy:** Gaadi apna WiFi-signal seedha upar udते hue drone tak bhej rahi hai.
**Easy way to remember:** G2A = "Ground (gaadi) se Air (drone) tak ka link."

---

## LoS (Full form: Line of Sight) ⭐
**What it is (from scratch):** **LoS** matlab transmitter (signal bhejne wala) aur receiver (signal lene wala) ke beech mein **koi rukawat (obstacle) nahi hai** — ekdum seedha, clear, direct raasta. Jab beech mein koi obstacle (building, tree) na ho, signal bahut strong aur clean reach karta hai.
**In this paper:** UAVs, neeche khadi buildings se upar udते hue, gaadiyon ke saath LoS link banane mein BS (jo ground pe fix hai aur buildings ke beech "phasa" ho sakta hai) se zyada reliable hote hain. LoS hone ki probability ek formula se calculate hoti hai — jo UAV-gaadi ke beech ke angle par depend karti hai. LoS link, NLoS link se 10-20 dB (decibel — signal-strength measure karne ki unit) better hota hai.
**Real-life analogy:** Kisi se room mein seedhe aankhon-se-aankhen milake baat karna (LoS) — bilkul clear — VS deewar ke peeche se chillaakar baat karna (NLoS) — muffled/unclear.
**Easy way to remember:** LoS = "direct, saaf raasta — kuch beech mein nahi aata."

---

## NLoS (Full form: Non-Line of Sight)
**What it is (from scratch):** **NLoS** matlab LoS ka opposite — transmitter aur receiver ke beech mein obstacle(s) hain, toh signal ko unke around "bounce" (reflect) ya "bend" (diffract) karke pahunchna padta hai. Iski wajah se signal weak ho jaata hai — paper mein NLoS ki extra path-loss **20 dB** hai, jabki LoS ki sirf **1 dB**.
**In this paper:** Jab UAV/BS aur gaadi ke beech buildings aa jaate hain, link NLoS ho jaata hai. G2A link ka average path-loss, LoS aur NLoS dono ki probability ke weighted-combination se nikala jaata hai.
**Real-life analogy:** Deewar ke peeche se baat karna — awaaz ko deewar se reflect/bend hokar tum tak pahunchna padta hai, isliye muffled sunayi deta hai.
**Easy way to remember:** NLoS = "raaste mein rukawat hai, signal ko around-the-corner jaana padta hai."

---

## Resource Block (RB)
**What it is (from scratch):** Wireless communication ek "spectrum" (frequencies ki ek bahut badi range) use karta hai — lekin ek waqt mein har frequency par sirf limited devices baat kar sakte hain (jaise FM radio mein har station ka apna alag frequency-number hai, 90.5, 98.3, etc. — tum ek waqt mein sirf ek hi sun sakte ho). Network engineers is bade spectrum ko chhote-chhote "tukdon" mein todte hain — har tukda ek **Resource Block (RB)** kehlata hai, jiska apna fixed bandwidth (yahan 180 kHz) aur time-slot hota hai.
**In this paper:** RBs woh "units" hain jo DRL agent gaadiyon ko allocate karta hai — har gaadi ko ek ya zyada RBs mil sakte hain uske V2I (gaadi-tower) aur/ya V2U (gaadi-drone) links pe. Zyada RBs = zyada data-speed = task fast offload hoga. LLM macro-scheduler bhi RBs ko ek gaadi se hata kar doosri gaadi ko transfer karta hai jab fairness ki zaroorat ho.
**Real-life analogy:** Highway par limited "lanes" — har gaadi ko ek ya zyada lanes mil sakte hain. Jitni zyada lanes, utna fast travel.
**Easy way to remember:** RB = spectrum ka ek chhota "tukda/lane" jo kisi gaadi ko mil sakta hai.

---

## SOCP (Full form: Second-Order Cone Programming) ⭐
**What it is (from scratch):** Pehle "**convex optimization**" samjho (neeche separate entry hai, par yahan short version): ek convex problem ko ek "katore (bowl)" jaisa socho — agar tum ek ball is katore mein chhod do, woh hamesha sabse neeche wale (best/lowest) point pe ruk jaayegi, chahe tum kahin se bhi chhodo. Matlab "ek hi best answer hai, aur reliable methods seedhe wahan tak pahunch sakte hain — bina random try karne ke." **SOCP** ek specific type ka convex optimization hai jisme constraints ek "cone" (jaise ice-cream cone ki shape) jaisi hoti hain — yeh tab use hoti hai jab problem mein "distance" (Euclidean distance, matlab seedhi-line distance) wali constraints ho.
**In this paper:** UAV ki 3D trajectory (position: x, y, aur altitude/height) decide karne wala problem SOCP ki tarah likha gaya hai — kyunki "ye gaadi UAV ki coverage-range ke andar honi chahiye" wali constraint ek distance-based constraint hai, jo naturally cone-shape mein fit hoti hai. Collision-avoidance (drones aapas mein na takraayein) ko ek seedhi-line (tangent-line) constraint se simplify kiya gaya, aur energy-cost ko bhi approximate (quadratic se) kiya gaya — taaki poora problem SOCP-solvable ban jaaye, jo `cvxpy` (Python library) se solve hota hai.
**Real-life analogy:** "Best flight-path dhundo jo in cone-shaped distance-rules ko follow kare" — ek mathematical toolkit jo geometry handle kar sakta hai, lekin itna fast hai ki real-time mein chal sake.
**Easy way to remember:** SOCP = "bowl-shaped (convex) problem, jisme distance/cone-jaisi constraints hain — exact best answer fast milta hai."

---

## Second-Order Cone (SOC)
**What it is (from scratch):** Ek "cone" socho — jaise ek ice-cream cone, ya ek funnel. Mathematically, agar tum 3D space mein ek point se "ek certain distance ke andar" wale saare points draw karo, tumhe ek sphere (gola) milega. Ab agar tum aise spheres ko ek line ke saath stack karte jao (chhote se bade), tumhe ek cone-shape milती hai. **Second-Order Cone (SOC)** isi shape ko mathematically describe karne ka tareeka hai.
**In this paper:** Sabse important constraint — "gaadi ka UAV se horizontal-distance, UAV ki altitude-dependent coverage-radius ke andar hona chahiye" — ko ek SOC-constraint ke roop mein likha gaya hai (after "linear relaxation," matlab ek complex on/off-jaisi condition ko simple continuous-number mein convert karna).
**Real-life analogy:** Socho tum apne ghar se "kitni door tak delivery ho sakti hai" ka circle banati ho — agar height (3D) ke saath ye circles stack karo, cone-shape milta hai. Yahi shape SOC-constraints ka base hai.
**Easy way to remember:** SOC = "distance-based constraints ki cone-shape."

---

## LP (Full form: Linear Programming) ⭐
**What it is (from scratch):** **Linear Programming** ek classical (bahut purana aur well-understood) optimization technique hai, jisme tumhara goal (objective — jaise "cost minimize karo") aur saari conditions/limits (constraints — jaise "total resource ki ek limit hai") sab **linear** (matlab "seedhi-line jaisi" relationships — agar input double karo, output bhi double ho jaata hai, koi curve/complexity nahi) hote hain. School mein "kitne kilo aam aur kitne kilo santre kharido taaki paisa bachey lekin total weight bhi match ho" wala sawal — wahi LP hai. Aise problems ko computer **exactly** (best possible answer, koi guess-work nahi) aur fast solve kar sakta hai.
**In this paper:** Ek baar UAV-trajectories aur resource-allocations fix ho jaaye, toh bachta hai sirf yeh decide karna: har gaadi apna task kitna % locally (apne OBU pe), kitna % UAV ko, aur kitna % BS ko bheje — taaki total delay minimum ho. Estimated queuing-delays ko substitute karke, ye problem ek "linear min-max" problem ban jaata hai, jo LP se solve hota hai (Python ki `scipy.linprog` library use hui).
**Real-life analogy:** "Kitne aam aur kitne santre khareedun" wala school-level sawal — bas yahan "aam-santre" ki jagah "computation-task ke fractions" hain.
**Easy way to remember:** LP = "seedhi-line relationships wala problem, exact best answer fast milta hai."

---

## Convex Optimization
**What it is (from scratch):** Ek "convex" function/shape ek "bowl/katora" jaisi hoti hai — uska sirf **ek** sabse neecha (lowest) point hota hai, aur woh point hi "global best" (overall sabse achha) answer hota hai. Iska matlab: agar tum kahin se bhi "neeche jaane" wale direction mein chalna shuru karo, tum hamesha usi ek best point par pahunchogi — koi "fake lowest point" (local minimum) tumhe bhatkayega nahi.
**In this paper:** UAV trajectory ka original problem **convex nahi** tha (matlab usme multiple "fake lowest points" ho sakte the, jo confusion create karte). Authors ne ise convex banaya — energy-terms ko "Taylor expansion" (ek complex curve ko seedhi-line se approximate karna) se simplify kiya, aur collision-avoidance ke "non-convex" (irregular-shape) regions ko tangent-line (sidha-line) constraints se replace kiya. Isse poora problem ek convex (SOCP) problem ban gaya, jo reliably solve ho sakta hai.
**Real-life analogy:** Ek "bowl-shaped" valley mein ball chhodo — woh hamesha sabse neeche point pe aake rukegi, chahe tum use kahin se bhi chhodo. (Agar valley "irregular/wavy" hoti, ball kisi bhi chhote gaddhe mein phas sakti thi — wahi non-convex ka problem hai.)
**Easy way to remember:** Convex = "ek hi best answer, reliably milta hai."

---

## Alternating Optimization
**What it is (from scratch):** Jab ek problem mein bahut saare "variables" (decisions) hote hain jo aapas mein juḏe (coupled) hote hain — sabko ek saath solve karna mushkil ho jaata hai. **Alternating Optimization** ka idea hai: ek-ek variable (ya group) ko "fix" kar do, baaki ko optimize karo — phir doosra group fix karo, pehle ko optimize karo — isi tarah baar-baar (iteratively) karte raho, jab tak sab "settle" (converge) na ho jaaye. Isko **Block Coordinate Descent (BCD)** bhi kehte hain.
**In this paper:** Authors do levels par alternating optimization use karte hain. Outer-level pe: UAV trajectories VS resource/offloading decisions alternate hote hain. Inner-level pe (har time-slot ke andar): resource-scheduling (DRL+LLM) aur task-offloading (LP) ek "DRL → LP → LLM → LP" loop mein alternate hote hain.
**Real-life analogy:** Camera ka tripod adjust karna — height aur angle dono ek saath perfect set nahi ho paate, toh tum baar-baar dono ko thoda-thoda adjust karte ho jab tak sahi position na mil jaaye.
**Easy way to remember:** "Ek fix karo, doosra optimize karo — baari-baari se, jab tak settle na ho."

---

## DRL (Full form: Deep Reinforcement Learning) ⭐
**What it is (from scratch):** Pehle **Reinforcement Learning (RL)** samjho: ek "agent" (decision-maker) apne environment ko dekhta hai (**state**), ek **action** leta hai, aur uska result milta hai ek **reward** (number) ke roop mein — achha kaam = positive reward, bura kaam = kam/negative reward. Agent dheere-dheere trial-and-error se seekhta hai ki kis state mein kaunsa action best hota hai. **"Deep"** RL matlab ye "seekhne wala dimaag" ek **neural network** (bahut saare interconnected "calculation-units" ka network, jo complex patterns seekh sakte hain) hai — jo bahut complex, large state-spaces (jaha "situations" bahut zyada/complicated hote hain) ko handle kar sakta hai.
**In this paper:** DRL hi resource-scheduling (channel + power allocation) ka "core engine" hai — agent vehicle ke task-loads, channel-quality, aur UAV-connection info dekhkar har gaadi ke liye power-level aur priority decide karta hai. Yeh offline train hota hai, aur fir live-deployment mein har time-slot pe initial allocation generate karta hai.
**Real-life analogy:** Ek bachhe ko cycle chalana sikhana — woh girta hai (negative outcome), balance banata hai (positive outcome), aur baar-baar try karke seekh jaata hai — bina kisi ne use "exact rules" likh kar diye ho.
**Easy way to remember:** RL = trial-and-error se seekhna; "Deep" = isme neural network ka "dimaag" use hota hai.

---

## DDPG (Full form: Deep Deterministic Policy Gradient)
**What it is (from scratch):** DDPG ek **DRL algorithm** hai, jo specifically un situations ke liye banaya gaya hai jaha action **continuous** ho — matlab "left/right/up/down" jaise discrete (limited) choices nahi, balki "0 se 1 ke beech koi bhi number" (jaise "power level = 0.73") jaisi smooth choices. DDPG do networks use karta hai: ek **Actor** (jo action decide karta hai) aur ek **Critic** (jo us action ki "quality" judge karta hai) — plus "experience replay" (purane experiences yaad rakhna aur unse seekhna) aur "target networks" (training ko stable rakhne ke liye ek "slow-updating copy").
**In this paper:** DDPG hi specific DRL algorithm hai jo resource-scheduling mein use hua hai — Actor har gaadi ke liye normalized transmit-power aur priority output karta hai, aur Critic combined LP-cost ko "reward signal" ke roop mein evaluate karta hai.
**Real-life analogy:** Ek "dial" jise tum 0 se 1 ke beech kahin bhi set kar sakti ho (continuous) — na ki sirf "ON/OFF" (discrete) switch.
**Easy way to remember:** DDPG = "continuous-dial-jaisi actions ke liye DRL, jisme ek Actor decide karta hai aur Critic check karta hai."

---

## LLM (Full form: Large Language Model) ⭐
**What it is (from scratch):** Tumne ChatGPT use kiya hai — **LLM** wahi tarah ka AI model hai. Ye ek neural network hai jo bahut saare text — kitabein, websites, conversations, code — padh-padhke train hota hai, taaki "agle word/token ka prediction" kar sake. Jab ye model bahut bada (billions of "parameters" — yaani internal calculation-settings) ho jaata hai, tab usme **emergent reasoning** (matlab khud-ba-khud, planned-nahi, lekin "soch-samajh kar steps mein decision lene" jaisi ability) dikhayi deti hai — woh analogy bana sakta hai, plan kar sakta hai, aur natural-language jaisi logic apply kar sakta hai.
**In this paper:** LLM (specifically **Qwen3-235B-A22B** naam ka model) ek **macro-scheduler** (neeche dekho) ki tarah kaam karta hai — usse failed-tasks aur surplus-resources ki structured JSON-summary di jaati hai, aur woh reason karke decide karta hai ki kaunse Resource Blocks transfer karne chahiye aur power-levels kaise adjust karni chahiye. Ye **few-shot prompting** (pehle se diye gaye solved-examples se seekhna, neeche dekho) aur **Chain-of-Thought reasoning** (apni "soch" steps mein dikhana, neeche dekho) use karta hai.
**Real-life analogy:** Ek bahut well-read consultant jisne tumhari specific company pe kabhi kaam nahi kiya, lekin apni general knowledge se intelligently reason kar sakta hai aur useful suggestions de sakta hai.
**Easy way to remember:** LLM = "text se trained AI jo reasoning/decisions natural-language jaisi steps mein kar sakta hai" — jaise ChatGPT.

---

## Macro-Scheduler ⭐
**What it is (from scratch):** "Macro" matlab "bada-picture/overall," aur "Scheduler" matlab "kaam baatne wala system." Ek **Macro-Scheduler** ek high-level controller hai jo chhote-chhote (per-task) decisions nahi karta — balki overall system ko dekhta hai aur sirf **bade imbalances/exceptions** ko fix karta hai.
**In this paper:** LLM yahi macro-scheduler ka role play karta hai. Yeh DRL agent ko replace nahi karta — balki DRL ke results ko "overview" karta hai, aur sirf extreme cases mein intervene karta hai: jab kisi gaadi ka task **fail** ho gaya (deadline miss), ya kisi gaadi ke paas **surplus** (zyada/bekaar) resources ho gaye. LLM tab in dono ke beech resources transfer karta hai.
**Real-life analogy:** Ek factory floor ka shift-supervisor jo poori assembly-line dekhta hai (jaha workers/DRL apna kaam kar rahe hain), aur sirf tab intervene karta hai jab koi ek station overload ho jaaye — har chhota kaam khud nahi karta, sirf exceptions handle karta hai.
**Easy way to remember:** Macro-Scheduler = "overall dekhne wala, sirf bade imbalance fix karne wala."

---

## Reward Decoupling ⭐
**What it is (from scratch):** "De-coupling" matlab "alag kar dena/judi hui cheezon ko separate karna." Ye ek mechanism hai jo DRL agent ke "seekhne ka signal" (reward) ko, ek **bahar ke corrector** (yahan LLM) ke changes se **alag** rakhta hai. Agar decoupling na ho, toh LLM ke changes seedhe DRL ke training-signal mein mix ho jaayenge — jisse DRL "confuse" ho jaayega ki "achha result mere decision se aaya ya LLM ke correction se?" — aur uska seekhna kharab ho jaayega.
**In this paper:** Pehle DRL apna original allocation deta hai, aur ek **pehli LP** is allocation par solve hoti hai — uska result DRL ke training-reward ke roop mein record ho jaata hai, **LLM ke koi changes karne se pehle**. Phir LLM apne changes karta hai, aur ek **dusri LP** final result ke liye solve hoti hai. Lekin training-memory mein sirf pehli LP ka reward jaata hai — LLM-improved result nahi. Isse DRL sirf apne khud ke decisions se seekhta hai.
**Real-life analogy:** Ek student ke test ko uske **original** answers ke base par grade karna, na ki tutor ke correction ke baad — taaki student apni khud ki galtiyon se seekhe, tutor ke correction se confuse na ho.
**Easy way to remember:** "Decoupling" = DRL ki training-grade aur LLM ka final-correction — dono ko alag-alag rakhna.

---

## In-Context Learning (ICL)
**What it is (from scratch):** Normally AI models ko "training" karna padta hai — bahut saara data dekha kar unke internal-settings (weights) update karne padte hain. **In-Context Learning** ka matlab hai: LLM ko **prompt ke andar hi** kuch solved-examples dikha do (bina uske weights update kiye), aur woh **inference-time** (jab woh real answer de raha ho) par hi un examples se pattern pakad kar naye case mein apply kar leta hai.
**In this paper:** LLM ko "few-shot ICL" (matlab thode-se examples) diye jaate hain — ek bade, offline-trained LLM ne pehle se high-quality scheduling-examples generate kiye hain, jo system-prompt mein embed (shamil) kar diye jaate hain. Deployed (real-time) LLM in examples se guide hokar naye cases handle karta hai — bina kisi fine-tuning ke.
**Real-life analogy:** Kisi ko 3 solved math-problems dikha kar, fir ek naya problem solve karne ko kehna — koi formal training-session nahi, sirf "sheet pe diye examples se seekho."
**Easy way to remember:** ICL = "prompt ke andar diye gaye examples se hi turant seekh lena, weights change kiye bina."

---

## Chain-of-Thought (CoT) Prompting
**What it is (from scratch):** **Chain-of-Thought** ek prompting-technique hai jisme LLM ko sirf "final answer" nahi, balki **beech ke reasoning-steps** bhi likhne ke liye kaha jaata hai — jaise school mein "apna rough-work/working dikhao" wala instruction. Isse LLM ki accuracy improve hoti hai, aur humein ye bhi pata chalta hai ki LLM ne "kyun" ye decision liya.
**In this paper:** LLM apna output sirf actions (RB-transfers, power-adjustments) ke roop mein nahi deta — balki ek "Analysis" field bhi deta hai jisme natural-language mein likha hota hai ki "ye action kyun choose kiya gaya." Ye CoT hai — reasoning ka trail bhi output ka part hai.
**Real-life analogy:** Math ke sawal mein "apna working dikhao" — beech ke steps dikhane se answer behtar bhi hota hai, aur tum samajh bhi paate ho ki kaise socha gaya.
**Easy way to remember:** CoT = "answer ke saath, reasoning ke steps bhi dikhana."

---

## KV Caching (Full form: Key-Value Caching)
**What it is (from scratch):** LLM (jo "transformer" architecture pe based hote hain) jab kisi prompt ko process karte hain, har word/token ke liye kuch internal "Key" aur "Value" matrices (number-grids) calculate karte hain — ye step LLM ke "attention mechanism" ka hissa hai. Agar prompt ka ek hissa **hamesha same** rehta hai (static), toh uske Key-Value matrices ko **ek baar** calculate karke "cache" (store) kar lo — taaki har baar dobara calculate na karna pade.
**In this paper:** System-prompt (LLM ko diye gaye instructions) aur few-shot examples — yeh static hote hain aur prompt ka bada hissa banate hain. Inka KV-cache ek baar pre-compute ho jaata hai. Sirf chhota, dynamic data-portion (current failed/surplus tasks ki JSON) hi naya calculate hota hai har baar — isse LLM ka response-time (inference latency) bahut kam ho jaata hai.
**Real-life analogy:** Multiplication-table ko ek baar yaad kar lena, taaki har baar 7x8 ko dobara-derive na karna pade — sirf naya part calculate karo.
**Easy way to remember:** KV Caching = "fix/static part ko ek baar yaad rakho, sirf naya part dobara calculate karo."

---

## MDP (Full form: Markov Decision Process)
**What it is (from scratch):** **MDP** ek mathematical "framework" (structure) hai jisse hum sequential decision-making problems describe karte hain — isme hote hain: **states** (abhi world kaisa dikh raha hai), **actions** (kya choices hain), **transitions** (ek action lene se world kaise badalega), aur **rewards** (har action ka result kya milta hai). "**Markov**" property ka matlab hai: "agla state sirf **current** state aur action par depend karta hai — pichhle poore history par nahi."
**In this paper:** DRL-based resource-scheduling problem ko MDP ke roop mein likha gaya hai — state = har gaadi ka task-load aur channel-quality; action = har gaadi ko power aur priority; reward = LP-solution se nikla negative total-cost.
**Real-life analogy:** "Abhi mai jahan hoon, usi ke basis pe, sabse zyada points kamane ke liye agla move kya hona chahiye?" — ek formal version of decision-making.
**Easy way to remember:** MDP = "state + action + reward ka formal structure, jisme sirf 'abhi' matter karta hai, poori history nahi."

---

## MADQN (Full form: Multi-Agent Deep Q-Network)
**What it is (from scratch):** Pehle **DQN (Deep Q-Network)** samjho: ye ek DRL algorithm hai jo har possible action ke liye ek "**Q-value**" (matlab "is action ka goodness-score, future reward ke hisaab se") seekhta hai, aur sabse high Q-value wala action choose karta hai. DQN sirf **discrete** (limited, fixed-choices) actions handle kar sakta hai. "**Multi-Agent**" matlab ek se zyada agents (yahan, ek se zyada drones) hain, jin mein har ek ka apna alag Q-network hai, aur sab ek shared environment mein apni-apni policy seekhte hain (decentralized).
**In this paper:** MADQN paper ke **proposed method ka part nahi hai** — ye ek **baseline comparison** hai, sirf UAV-trajectory planning ke liye. Paper dikhata hai ki SOCP-based trajectory-approach MADQN se behtar perform karta hai.
**Real-life analogy:** Multiple players, har ek apna alag "brain" rakhte hue, sab same video-game khel rahe hain aur apne-apne score se seekh rahe hain.
**Easy way to remember:** MADQN = "kayi independent DQN-agents, ek shared environment mein."

---

## MADDPG (Full form: Multi-Agent Deep Deterministic Policy Gradient)
**What it is (from scratch):** Ye **DDPG** (upar dekho) ka multi-agent version hai. Isme "**Centralized Training, Decentralized Execution (CTDE)**" approach use hota hai — matlab har agent ka apna **Actor** (jo independently action leta hai) hota hai, lekin training ke waqt sab agents ek **shared/centralized Critic** (jo poora global-state dekh sakta hai) se guidance lete hain.
**In this paper:** MADDPG bhi ek **baseline comparison** hai — task-offloading subproblem ke liye. Paper argue karta hai ki LP-based offloading, MADDPG se behtar hai, kyunki LP fixed-resources ke given, offloading-ratios ka **globally-optimal (exact best)** solution guarantee karta hai, jabki MADDPG sirf approximate karta hai.
**Real-life analogy:** DDPG jaisa hi, lekin ek poori team khel rahi hai — training ke waqt ek coach sabko dekh raha hai, lekin match mein har player apne-aap decide karta hai.
**Easy way to remember:** MADDPG = "team-version of DDPG, training mein shared-coach (Critic), execution mein independent players (Actors)."

---

## LVM (Full form: Large Vision Model)
**What it is (from scratch):** Jaise LLM text padh-padhke train hota hai, **LVM** waisa hi neural network hai jo **images/visual-data + text** ke bade datasets se train hota hai — taaki ye visual information samajh sake aur usi ke base par decisions le sake.
**In this paper:** LVM bhi sirf ek **baseline comparison** hai — UAV trajectory-planning ke liye test kiya gaya, ki kya ek vision-based AI, SOCP ko replace kar sakta hai. Result: SOCP-approach LVM se behtar nikla UAV trajectory-control mein.
**Real-life analogy:** Ek computer-vision-AI ko geometric flight-path-planning karne do — impressive generalist hai, lekin purpose-built math-tools (SOCP) se peeche reh jaata hai.
**Easy way to remember:** LVM = "image-trained AI," jo yahan trajectory-planning mein baseline ki tarah use hua.

---

## FIFO (Full form: First In First Out)
**What it is (from scratch):** **FIFO** ek "queue" (line) ka rule hai — jo task **pehle aaya**, woh **pehle process** hoga. Bilkul jaise kisi shop ki line — jo pehle line mein lagega, uska number pehle aayega.
**In this paper:** UAVs aur BS, dono apne computation-queues ko FIFO se process karte hain. Iska matlab: agar koi gaadi ka data UAV ke paas pahuncha jab UAV pehle se kisi doosri gaadi ka task process kar raha hai, toh naye task ko **wait** karna padega — yahi "queuing delay" ka source hai.
**Real-life analogy:** Coffee-shop ki line — jo pehle aaya, uska order pehle banega.
**Easy way to remember:** FIFO = "pehle aao, pehle paao."

---

## Hierarchical Framework ⭐
**What it is (from scratch):** "Hierarchical" matlab "layers/levels mein organized" — jaise ek company mein different levels hote hain (CEO, manager, employee), aur har level apne scope ke decisions leta hai, jabki upar wale levels bigger-picture set karte hain.
**In this paper:** Pura algorithm 4 "layers" mein organized hai — SOCP slow-timescale par UAV-trajectory (geometry) decide karta hai; DRL medium-timescale par resource-scheduling karta hai; LLM exception-cases mein macro-level adjustments karta hai; aur LP har time-slot mein exact offloading-ratios calculate karta hai. Har layer ka apna tool aur apna "timescale" (kitni baar update hota hai) hai.
**Real-life analogy:** Company-hierarchy jaisa — CEO bigger strategy set karta hai (LLM), managers team-resources allocate karte hain (DRL), accountants budget-optimize karte hain (LP), aur engineers physical-kaam execute karte hain (SOCP trajectory).
**Easy way to remember:** Hierarchical = "alag-alag level pe alag decisions, sab milkar overall goal achieve karte hain."

---

## Trajectory Planning ⭐
**What it is (from scratch):** **Trajectory** matlab "ek moving object ka raasta/path" — kahan se kahan tak, kis sequence mein positions cover kiye. **Trajectory Planning** matlab ye decide karna ki ek moving agent (yahan UAV) ko kis sequence mein positions occupy karni chahiye, taaki koi objective (jaise "coverage maximize karo," "energy minimize karo") achieve ho, while kuch limits (jaise speed-limit, dusre objects se na takraana) follow kiye jaayein.
**In this paper:** UAV trajectory-planning, har UAV ki har time-slot par 3D position (x, y, altitude) decide karta hai. SOCP-based "sequential distributed algorithm" (matlab har UAV apni trajectory ek tarteeb mein, ek-ek karke, plan karta hai) gaadiyon ki coverage ko maximize karta hai aur altitude ko minimize karta hai (taaki path-loss aur energy kam ho), jabki kinematic (movement-related) aur collision-constraints follow karta hai.
**Real-life analogy:** Apne delivery-drone ka route plan karna — maximum customers cover ho, minimum fuel use ho.
**Easy way to remember:** Trajectory Planning = "kahan-kab jaana hai, ye decide karna — sirf 'kahan hoon' nahi, 'kaise pahuncha' bhi."

---

## Latency / Delay
**What it is (from scratch):** **Delay/Latency** matlab "kitna time lagta hai" — yahan, ek task generate hone se lekar uska result poori tarah ready hone tak ka **total time**. Isme shamil hai: data bhejne ka time (transmission), line mein wait karne ka time (queuing), aur actual calculation ka time (computation).
**In this paper:** System ka objective hai delay aur energy dono ko minimize karna. Har gaadi ke task ki **deadline 1 second** hai — agar isse zyada time lage, task "**failed**" count hota hai. "Normalized delay" (actual delay ÷ deadline) ka use hota hai taaki different deadlines wale tasks ko fairly compare kiya ja sake.
**Real-life analogy:** "Submit" button dabane se result dikhne tak ka total wait-time — upload + queue + processing, sab milkar.
**Easy way to remember:** Delay = "shuru se end tak ka total time."

---

## Energy Consumption
**What it is (from scratch):** **Energy Consumption** matlab pure system ne kitni electrical-energy/power use ki — yahan UAV ko udaane ki energy, gaadi ke signal-transmission ki energy, aur UAV/BS ke computation ki energy — sab milakar.
**In this paper:** Delay aur energy ko jointly minimize kiya jaata hai, do weights (ω1 = delay ke liye, ω2 = energy ke liye) ke saath. UAV ki propulsion (udaane ki) energy ek "rotary-wing aerodynamic model" follow karti hai — jisme ek "U-shaped" curve hoti hai (matlab UAV moderate-speed pe sabse efficient hota hai, bahut slow ya bahut fast/hover karne se zyada energy lagti hai).
**Real-life analogy:** Pure system ka "bijli ka bill" — drones udna, signal bhejna, compute karna — sab energy consume karte hain.
**Easy way to remember:** Energy Consumption = "total bijli-kharcha, har component ka mil ke."

---

## Task Success Rate ⭐
**What it is (from scratch):** **Task Success Rate** ek percentage hai — jitne tasks system ko diye gaye, unme se kitne percent tasks apni **deadline ke andar** poori tarah complete hue.
**In this paper:** Ye paper ka **headline/main metric** hai. Proposed method (SOCP + DRL+LLM + LP) sab baselines (MADQN, LVM, MADDPG) se behtar task-success-rate deta hai. LLM macro-scheduler specifically isi metric ko improve karta hai — kyunki woh "failed-hone-wale" tasks ko bachata hai jinhe DRL akela chhod deta.
**Real-life analogy:** "Kitne percent orders time pe deliver hue" — customer-satisfaction ka main number.
**Easy way to remember:** Task Success Rate = "deadline ke andar pure hue tasks ka %."

---

(5 most important terms marked with ⭐: UAV, IoV, Task Offloading, MEC, SOCP, LP, DRL, LLM, Macro-Scheduler, Reward Decoupling, Hierarchical Framework, Trajectory Planning, Task Success Rate — in saare concepts ke bina is paper ka core idea samajhna mushkil hoga, isliye inhe pehle pakka karo. Agar koi term abhi bhi confusing lage, `explanation.md` mein step-by-step context ke saath dobara samjhaya gaya hai.)
