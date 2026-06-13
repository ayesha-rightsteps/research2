# Terminology: Dynamic Allocation of C-V2X Communication Resources Based on Graph Attention Network and Deep Reinforcement Learning

> Beginner-friendly reference — har term zero-knowledge se explain kiya gaya hai. Koi short-form/acronym bina poora naam aur matlab bataye nahi chhoda gaya.

---

## C-V2X (Full form: Cellular Vehicle-to-Everything) ⭐
**What it is (from scratch):** Tumhara phone internet chalata hai using "cellular network" — matlab phone companies ke towers (4G/5G), jo already har jagah laga hua hai. **C-V2X** ka matlab hai: gaadiyon ka wireless communication bhi isi already-existing cellular (4G/5G) infrastructure ke through ho, naya alag system banane ki zaroorat nahi. "Everything" ka matlab — gaadi kisi se bhi baat kar sakti hai: doosri gaadi se, tower se, pedestrian se.
**In this paper:** Ye paper C-V2X ke andar V2V aur V2N links ke beech spectrum-sharing problem solve karta hai, 3GPP ke standards follow karte hue.
**Real-life analogy:** Jaise naya app banane ke liye tum apna alag internet network nahi banati — existing mobile-data network use karti ho. C-V2X bhi waise hi existing phone-network infrastructure use karta hai gaadiyon ke liye.
**Easy way to remember:** "C" = "Cellular" = tumhare phone ka network — gaadi bhi usi network pe chalti hai.

---

## V2V (Full form: Vehicle-to-Vehicle) ⭐
**What it is (from scratch):** Ye ek tarah ka wireless communication hai jisme **sirf gaadi-se-gaadi seedhe** baat hoti hai — bina kisi tower ke beech mein aaye. Jaise do walkie-talkies seedhe ek-doosre se baat kar lein.
**In this paper:** V2V links **safety messages** ke liye use hote hain — jaise "main brake laga raha hoon" ya "aage accident hai." Yeh itne important hain ki paper mein requirement hai ki **95% se zyada** successfully deliver hone chahiye (isko "V2V success ratio" kehte hain) — kyunki agar fail ho jaaye toh real accident ho sakta hai. Is paper mein har V2V link interference-graph ka ek **node** banta hai.
**Real-life analogy:** Tum apne saath wali gaadi ke driver ko seedha shout karke bata do "bhai brake lagao!" — bina kisi operator/tower ke through gaye.
**Easy way to remember:** V2V = "Vehicle se seedha Vehicle" — koi beech mein nahi.

---

## V2N (Full form: Vehicle-to-Network) ⭐
**What it is (from scratch):** Ye wireless communication ka woh type hai jisme gaadi **cellular base station** (tower) se baat karti hai — data lene/dene ke liye, jaise maps update, traffic info, ya internet/streaming. Paper-1 mein isko "V2I" (Vehicle-to-Infrastructure) bola gaya tha — ye basically same concept hai, sirf naam thoda different hai (V2N zyada "network/cellular tower se connection" par focus karta hai).
**In this paper:** V2N links **high data rate (throughput)** ke liye important hain — speed zaroori hai (measured in Mbps, megabits per second), lekin agar kabhi-kabhi thoda slow ho jaaye toh life-threatening nahi hai. V2N aur V2V dono ek hi spectrum (20 Resource Blocks) share karte hain "in-band overlay" tareeke se — matlab dono same RBs use kar sakte hain, jisse interference hoti hai.
**Real-life analogy:** Jaise tum apne phone se ek fixed WiFi router se connect karke internet chalati ho — gaadi bhi waise hi roadside tower se "connect" hoti hai data ke liye.
**Easy way to remember:** V2N = "Vehicle se Network (tower) tak" — V2I jaisa hi hai, bas naam alag.

---

## Resource Block (RB) ⭐
**What it is (from scratch):** Radio waves ek bohot bada "spectrum" (frequencies ki range) cover karte hain — lekin ek hi waqt mein har frequency par sirf limited devices baat kar sakte hain. Network engineers is bade spectrum ko chhote-chhote "tukdon" mein todte hain — har tukda ek specific frequency-range + time-slot hota hai, jisko **Resource Block (RB)** kehte hain. Ek device (gaadi) ko ek time pe ek RB assign hota hai jisi par woh transmit kar sakti hai.
**In this paper:** Total **20 RBs** hain, 2 GHz carrier frequency par. Har V2V link agent ko ek RB (20 options mein se) aur ek power level (3 options: 23, 10, ya 5 dBm — dBm ek unit hai signal-power measure karne ki) choose karna hota hai — total `20 x 3 = 60` possible combinations, jisko "action space" kehte hain. Do links jo same RB use karte hain, unke beech interference hoti hai.
**Real-life analogy:** Ek highway pe limited lanes (RBs) hain, lekin bohot saari gaadiyaan (links) hain jinhe kisi lane mein chalna hai. Agar do gaadiyaan ek hi lane mein side-by-side chalne ki koshish karein, toh takkar (interference) ho jaati hai.
**Easy way to remember:** RB = ek "lane" jo spectrum mein hoti hai, gaadi ko milti hai.

---

## SINR (Full form: Signal-to-Interference-plus-Noise Ratio) ⭐
**What it is (from scratch):** Jab ek gaadi transmit karti hai, uska signal doosre transmitters ke "interference" aur environment ke random "noise" (background disturbance) ke saath mix hokar receiver tak pahunchta hai. **SINR** ek number hai jo compare karta hai: "mera asli signal kitna strong hai" VS "interference + noise kitna strong hai." High SINR matlab signal clearly samajh aayega; low SINR matlab signal "doob" jaata hai interference/noise mein.
**In this paper:** SINR formula mein har receiving link ke liye dekha jaata hai ki desired signal kitna strong hai, aur uske saath kitna interference aa raha hai (doosre V2V links se, ya base station se). V2V "success" SINR threshold se decide hota hai (sigmoid function ke through), aur V2N ka data rate (Mbps) bhi SINR se Shannon capacity formula ke through nikalta hai. Yeh poora resource-allocation problem basically SINR ko maximize karne ki koshish hai.
**Real-life analogy:** Tumhare phone mein "signal bars" hote hain — full bars matlab high SINR (clear call), 1 bar ya "no service" matlab low SINR (call drop).
**Easy way to remember:** SINR jitna zyada, "voice" jitni clearly sunayi de — kam SINR matlab sab "noise" mein doob gaya.

---

## Interference Graph
**What it is (from scratch):** Ek **graph** mein **nodes** (points) hote hain aur **edges** (lines jo do nodes ko connect karti hain) hote hain. Yahan har V2V communication link ek **node** hai. Do nodes ke beech ek **edge** banti hai agar woh do links ek-doosre ko interfere kar sakte hain — matlab same RB use karne par unke signals mix ho sakte hain.
**In this paper:** Har vehicle apne nearest 20% neighbors ke saath 3 output links banata hai, jisse total nodes lagbhag `3n` ho jaate hain (`n` = vehicles ki ginti). Yeh graph **dynamic** hai — har 0.1 se 1 second mein fresh banta hai (jaise live traffic map), kyunki gaadiyaan move karti hain aur "kaun kis ko interfere kar sakta hai" badalta rehta hai. Har node mein "self-loop" bhi hota hai — matlab node apni khud ki info bhi yaad rakhta hai GAT computation ke dauraan.
**Real-life analogy:** Jaise ek WhatsApp group ka "kaun kis se directly baat kar sakta hai" wala diagram — har baar koi join/leave kare, diagram update hota hai.
**Easy way to remember:** Interference graph = "kaun kis ko disturb kar sakta hai" ka live naqsha.

---

## GNN (Full form: Graph Neural Network)
**What it is (from scratch):** Ek **GNN** ek aisa neural network hai jo specifically graphs (nodes + edges) par kaam karne ke liye design hua hai. Ye har node ko apne connected (neighbor) nodes se "info lene" deta hai — jaise tum apne dost se poochho "tu kya kar raha hai" aur uska jawab apni samajh mein add kar lo. Is process ko repeat karne se har node ko apne aas-paas ka ek "smart summary" mil jaata hai, jisko **embedding** ya **feature vector** kehte hain (basically numbers ka ek chhota set).
**In this paper:** GNN ka specific type jo yahan use hua hai woh **GAT** hai (neeche dekho) — jo plain GNN se ek step aage hai.
**Real-life analogy:** Socho ek WhatsApp group mein har member apne nearby members se "status update" lekar apna decision banata hai — bina group admin se poochne ke.
**Easy way to remember:** GNN = neural network jo "kaun kis se connected hai" (graph) ko samajh kar smart features banata hai.

---

## GAT (Full form: Graph Attention Network) ⭐
**What it is (from scratch):** Pehle GNN samjho (upar dekho) — har node apne neighbors se info collect karta hai. Plain GNN (jaise GraphSAGE, jo paper-1 mein tha) sab neighbors ko **roughly equal weight** deta hai — sabki baat barabar sunta hai. **GAT** isse aage jaata hai: ye har neighbor ko ek **attention score** (seekha hua "ye neighbor mere liye kitna important hai" wala number) deta hai, aur jis neighbor ka score zyada hai, uski info ko zyada weight deta hai apna naya feature banate waqt.
**In this paper:** Har V2V link node apne interfering neighbors ko dekhta hai, aur GAT decide karta hai "kaunsa neighbor mujhe sabse zyada interference dega — usi par zyada dhyan do." Ye attention-weighted features (20-dimensional embedding) RL agent ke state mein jaate hain. Paper ne 2-layer GAT use kiya hai, jisme first layer mein **8 attention heads** hain (neeche "Multi-Head Attention" dekho).
**Real-life analogy:** Group project mein, jab final decision lena ho, tum apne saare teammates ki baat ek jaisi weight nahi deti — jo teammate is specific topic mein expert hai, uski baat zyada sunti ho. GAT bhi yehi karta hai — kuch neighbors ki baat zyada "sunta" hai.
**Easy way to remember:** GAT = GNN + "kaun zyada important hai, ye bhi seekho" (attention).

---

## Attention Mechanism / Attention Score ⭐
**What it is (from scratch):** **Attention mechanism** ek tarika hai jisse neural network seekh sakta hai "is decision ke liye, kaunsi input sabse zyada relevant/important hai." Ye ek number deta hai — **attention score** — har input (yahan: har neighbor node) ke liye, jo batata hai "is neighbor ko kitna 'dhyan' dena chahiye." Score jitna high, utna zyada important woh neighbor hai current decision ke liye.
**In this paper:** GAT har node-pair (x aur y) ke beech ek attention score `e(x,y)` calculate karta hai — ye dono nodes ke features ko combine karke ek formula se nikalta hai (formula mein **LeakyReLU**, neeche dekho, use hota hai). Phir is score ko **softmax** (neeche dekho) se normalize karke final **attention weight** (alpha) milta hai, jo batata hai "node y, node x ke decision mein kitna percent contribute karta hai." High alpha = "node y mujhe (node x ko) zyada interference de sakta hai — is RB se door raho."
**Real-life analogy:** Class mein teacher ek sawal poochta hai, aur tum apne 5 dosto se opinion lete ho — lekin jiska answer pichhli baar sabse accurate tha, uski baat is baar bhi zyada dhyan se sunte ho. Wo "zyada dhyan" hi attention score hai.
**Easy way to remember:** Attention score = "is neighbor ko kitna sunna chahiye" — ek number jo network khud seekhta hai.

---

## Multi-Head Attention ⭐
**What it is (from scratch):** Ek "attention head" matlab ek complete attention-calculation process (upar wala). **Multi-head attention** ka matlab hai — ye process **ek se zyada baar parallel mein** chalaya jaata hai, har baar alag-alag "perspective" (alag weight-sets) ke saath. Phir sab heads ke results ko jodke (concatenate karke) ek final, richer feature banaya jaata hai.
**In this paper:** First GAT layer mein **8 attention heads** use hue hain — matlab interference pattern ko 8 alag "angles" se dekha jaata hai, jaise 8 experts ek hi situation ko apne-apne specialization se analyze kar rahe hon, aur phir sabki report combine ho jaati hai. Authors ne experiment karke dekha ki 8 heads optimal hai — kam heads (jaise 2-4) complex interference patterns miss kar dete hain, zyada heads (jaise 16) overfitting (training data pe achha lekin naye data pe kharab perform karna) aur extra computation laate hain bina extra fayde ke.
**Real-life analogy:** Ek crime scene ko investigate karne ke liye 8 alag detectives bhejna — har ek apne tareeke se clues dekhta hai, phir sab apni findings share karke ek final report banate hain. Single detective se zyada complete picture milti hai.
**Easy way to remember:** Multi-head = "ek hi kaam, multiple parallel 'experts' se karwao, phir combine karo."

---

## A2C (Full form: Advantage Actor-Critic) ⭐
**What it is (from scratch):** A2C ek **Reinforcement Learning (RL)** technique hai. RL ka basic idea (paper-1 se yaad karo): ek agent apne environment ko dekhta hai (**state**), ek **action** leta hai, aur **reward** milta hai — achha action toh positive reward, bura action toh kam/negative. A2C mein ye agent **do hisson** mein bata hua hai: **Actor** (jo action choose karta hai) aur **Critic** (jo grade deta hai ki action kaisa tha). "Advantage" batata hai — "ye action average se kitna better ya worse tha."
**In this paper:** A2C ka state input 102 numbers ka hota hai (82 local features + 20-dimensional GAT embedding). Actor in 102 numbers ko dekh kar 60 possible actions (20 RBs x 3 power levels) mein se ek choose karta hai. Har V2V link apna alag A2C agent chalata hai — lekin training ke time global info share hoti hai (CTDE — neeche dekho).
**Real-life analogy:** Ek student (Actor) exam mein answer likhta hai, aur ek teacher (Critic) batata hai "ye answer class-average se kitna better ya worse tha." Student is feedback se seekhta hai aage kya likhna better hai.
**Easy way to remember:** A2C = "Actor jo karta hai" + "Critic jo grade deta hai" + "Advantage jo batata hai kitna better/worse."

---

## Actor ⭐
**What it is (from scratch):** **Actor** RL ke andar woh hissa hai jo actual **action** choose karta hai — matlab "is state mein, main kya karu?" ka jawab deta hai. Ye ek neural network hota hai jo state ko input leta hai aur output mein har possible action ke liye ek probability deta hai (kaunsa action kitna "pasand" hai abhi).
**In this paper:** Actor, state (102 numbers — jisme GAT ka attention-weighted summary bhi shamil hai) leke decide karta hai — kaunsa RB (20 mein se) aur kaunsi power level (3 mein se: 23, 10, ya 5 dBm) is V2V link ko use karna chahiye. Actor network mein 3 hidden layers hain (500, 250, 120 neurons).
**Real-life analogy:** Restaurant mein order lene wala waiter — menu (state) dekh ke "aaj ye dish suggest karta hoon" (action) bolta hai.
**Easy way to remember:** Actor = "action lene wala."

---

## Critic ⭐
**What it is (from scratch):** **Critic** RL ke andar woh hissa hai jo grade deta hai — "is state mein average action se kitna reward milne ki expectation hai?" Ye number "**value**" kehlata hai. Critic khud koi action nahi leta — ye sirf judge karta hai ki Actor ka decision kaisa tha.
**In this paper:** Critic Actor ke saath training ke dauraan kaam karta hai — har action ke baad, Critic batata hai "ye action expected-se-better tha ya worse" (Advantage), aur Actor is feedback se seekhta hai apni future choices improve karne ke liye. Critic network bhi 3 hidden layers (500, 250, 120 neurons) ka hai, aur **soft target network updates** (Polyak averaging, tau = 0.01) use karta hai — matlab Critic ka "target" version slowly-slowly update hota hai, jisse training stable rehti hai.
**Real-life analogy:** Ek coach jo match ke baad player ko bolता hai "tumne jo shot khela, woh average player ke shot se better tha" — coach khud shot nahi khelta, sirf evaluate karta hai.
**Easy way to remember:** Critic = "grade dene wala, action nahi leta."

---

## Advantage Function
**What it is (from scratch):** **Advantage** ek number hai jo batata hai: "jo action maine liya, woh average action se kitna better ya worse tha?" Formula simple hai: `Advantage = (is action ka estimated reward) - (average/expected reward is state mein)`. Agar Advantage positive hai, action average se better tha. Agar negative hai, average se worse tha.
**In this paper:** Advantage Actor ko bataata hai kaunsi actions ki probability badhani hai (positive advantage wali) aur kaunsi ki kam karni hai (negative advantage wali). Ye seedha Q-value (paper-1 ka "goodness score") use karne se zyada stable hai, kyunki "average se compare karna" noise kam kar deta hai.
**Real-life analogy:** Tumne test mein 85/100 liye. Class average 70 hai. Tumhara "advantage" +15 hai — matlab tumne average se better kiya, toh tum jo strategy use ki, usi ko aage continue karogi.
**Easy way to remember:** Advantage = "average se kitna upar/neeche."

---

## Softmax Normalization
**What it is (from scratch):** **Softmax** ek mathematical function hai jo kisi bhi list of numbers ko "probabilities" mein convert kar deta hai — matlab sab numbers 0 aur 1 ke beech ho jaate hain, aur sab milke total 1 (ya 100%) ban jaate hain. Bade numbers ko bada percentage milta hai, chhote numbers ko chhota.
**In this paper:** GAT ke attention scores (`e(x,y)`) ko softmax se guzar ke final attention weights (alpha) milte hain — taaki sab neighbors ka total "attention" milke 100% ho jaaye, aur koi ek neighbor sab attention "khud le" na le.
**Real-life analogy:** Tumhare paas 5 dost hain aur tumhe apna pura din (100%) unke saath baatne mein divide karna hai. Jis dost se zyada zaroori baat karni hai, usko zyada % time milega — lekin total milake 100% hi rahega.
**Easy way to remember:** Softmax = "raw numbers ko fair percentages mein badal do, jo total 100% banaye."

---

## LeakyReLU
**What it is (from scratch):** Ye ek "activation function" hai — neural networks mein har layer ke baad ek chhota sa math-step hota hai jo decide karta hai signal aage jaaye ya nahi, aur kitna. **ReLU** (ek common activation) negative numbers ko seedha 0 bana deta hai. **LeakyReLU** thoda alag hai — negative numbers ko bilkul 0 nahi karta, balki bahut chhota kar deta hai (jaise 0.01x) — taaki signal poori tarah "mar" na jaaye.
**In this paper:** GAT ke attention score formula (`e(x,y) = LeakyReLU(...)`) mein LeakyReLU use hota hai — isse training ke dauraan gradients (learning signals) flow karte rehte hain, even jab pre-attention scores negative hote hain, jo training ko stable banata hai.
**Real-life analogy:** Ek volume knob jo negative input par bilkul mute nahi karta, balki bahut low volume par rakhta hai — taaki tum bilkul "kuch sunai nahi de raha" wali situation mein na phaso.
**Easy way to remember:** LeakyReLU = "negative ko zero nahi, bahut chhota kar do."

---

## MDP (Full form: Markov Decision Process)
**What it is (from scratch):** **MDP** ek framework hai jo RL problems ko define karne ke liye use hota hai — isme 4 cheezein hoti hain: **States** (agent kya dekh sakta hai), **Actions** (agent kya kar sakta hai), **Transitions** (action lene ke baad state kaise badalta hai), aur **Rewards** (action lene ka result, number ke roop mein).
**In this paper:** State = local features (channel gain, interference, kitna data bacha hai, etc.) + GAT ka 20-dimensional embedding = total 102 numbers. Action = RB + power level choice (60 options). Reward = `lambda * V2N rate + (1-lambda) * V2V success - time penalty` — matlab teen cheezon ka mix: V2N speed, V2V reliability, aur transmission deadline ka pressure.
**Real-life analogy:** Ek video game ka rule-book — "tum kya dekh sakte ho (state), kya kar sakte ho (action), aur har move ka reward kya hai" — sab MDP define karta hai.
**Easy way to remember:** MDP = RL problem ka "rule-book": state, action, reward, sab define karta hai.

---

## Replay Memory
**What it is (from scratch):** RL training mein agent ka experience hota hai: "state dekha, action liya, reward mila, naya state aaya" — isko ek "tuple" kehte hain. **Replay memory** ek bada buffer (storage) hai jisme ye tuples store hote hain, aur training ke time random tuples uthaye jaate hain training ke liye — turant-wale experience pe seedha train karne ke jagah.
**In this paper:** Replay memory ki capacity **1 million tuples** hai. Training ke har step mein, **2000 samples (batch size)** randomly uthaye jaate hain Actor, Critic, aur GAT networks ko update karne ke liye. Isse consecutive (ek-ke-baad-ek) experiences ka asar kam ho jaata hai — jo training ko zyada stable banata hai.
**Real-life analogy:** Ek diary jisme tum apne din ke har decision aur uska result likhti ho. Mahine ke end mein, tum random pages padhke seekhti ho — sirf aaj ka din nahi, balki sab kuch mix karke pattern dhundti ho.
**Easy way to remember:** Replay memory = "purane experiences ka random-mix se seekhna."

---

## CTDE (Full form: Centralized Training, Distributed Execution)
**What it is (from scratch):** Ye ek training strategy hai jisme **training ke time** agents ko global/full information dikhayi jaati hai (jaise sabka data ek saath), taaki woh behtar seekh sakein. Lekin **actual use (deployment) ke time**, har agent sirf apni **local** info se kaam karta hai — bina kisi central server se poocha.
**In this paper:** Training mein GAT aur RL networks global network info (sab vehicles ki positions, channel states, poora interference graph) dekhte hain. Lekin deployment mein, har V2V link agent sirf apna local state aur apne local subgraph se GAT embedding use karta hai — koi central coordination real-time mein nahi chahiye, jisse system real-world mein scalable banta hai.
**Real-life analogy:** Training camp mein players ko full game-footage aur stats dikhaye jaate hain (centralized). Lekin actual match mein, har player apne hi field-of-view se decisions leta hai (distributed) — koi unhe live mein poori game ka data nahi de raha hota.
**Easy way to remember:** CTDE = "seekho sab dekh kar, kaam karo apne hisaab se."

---

## Manhattan Grid
**What it is (from scratch):** Ye ek simulation ka road-layout hai jo New York City ke "Manhattan" jaise rectangular blocks/grid pattern wale roads ko represent karta hai — straight roads jo right-angles par cross karti hain, multiple lanes, aur intersections.
**In this paper:** Simulation ek Manhattan-grid intersection mein hota hai, jisme roads (2+2)x4 lanes ki hain, aur base station (tower) intersection ke center mein hai. Gaadiyaan Poisson distribution (ek random-but-realistic spread) se road par hain, speed 36-54 km/h ke beech. Density 20 se 100 vehicles tak test ki gayi.
**Real-life analogy:** Socho ek city-block jaisa road-map jisme har road seedhi hai aur perfect right-angles par doosri road se milti hai — jaise chess board ki grid lines.
**Easy way to remember:** Manhattan grid = "perfect rectangular-block road layout, jaise NYC ki streets."

---

## 3GPP TR 36.885
**What it is (from scratch):** **3GPP** ek international organization hai jo mobile/cellular technology (4G, 5G) ke "rules/standards" banati hai — taaki saari companies/researchers same tareeke se kaam karein aur sab compatible rahe. **TR 36.885** ek specific document/report hai jisme V2X simulations ke liye exact settings di gayi hain — jaise signal kaise weaken hota hai distance ke saath (channel models), antenna heights, vehicle speeds, etc.
**In this paper:** Saari simulation settings (Manhattan grid, channel models, antenna parameters: vehicle 3 dBi at 1.5m height, base station 8 dBi at 25m height) TR 36.885 se aati hain — taaki results doosre vehicular-network papers se fairly compare ho sakein.
**Real-life analogy:** Jaise ek standard exam-paper format jo sab schools follow karte hain — taaki har school ke results comparable hon.
**Easy way to remember:** 3GPP TR 36.885 = "vehicular network simulation ka official rule-book."

---

## Path Loss
**What it is (from scratch):** Jab radio signal transmitter se receiver tak travel karta hai, raaste mein iski power kam hoti jaati hai — distance badhne se, aur raaste mein buildings/obstacles ki wajah se. Is power-loss ko **path loss** kehte hain.
**In this paper:** Channel gain (signal kitna strong pahuchega) ka calculation path loss, shadowing (slow signal-drop due to obstacles), aur fast fading (rapid signal fluctuations) — teeno ko combine karke hota hai. TR 36.885 LOS (Line-of-Sight, matlab direct raasta) aur NLOS (Non-Line-of-Sight, matlab beech mein obstruction hai) ke alag-alag formulas deta hai Manhattan grid ke liye.
**Real-life analogy:** Tumhari awaaz jitni door jaati hai, utni "kamzor" sunayi deti hai — aur agar beech mein deewar ho, toh aur bhi kamzor.
**Easy way to remember:** Path loss = "signal distance ke saath kamzor hota jaata hai."

---

## Doppler Effect
**What it is (from scratch):** Jab transmitter aur receiver ek-doosre ke relative move kar rahe hote hain (jaise ek moving car aur fixed tower), toh signal ki frequency thodi shift ho jaati hai — isi shift ko **Doppler effect** kehte hain. (Tumne ye real life mein suna hoga — ambulance ka siren paas aate waqt high-pitch aur door jaate waqt low-pitch sunayi deta hai, wo bhi Doppler effect hai.)
**In this paper:** Vehicles ki speed (36-54 km/h) par 2 GHz frequency ke saath, Doppler shift "hundreds of Hz" range mein hota hai. Ye **fast fading** (signal ki rapid fluctuation) mein contribute karta hai, aur OFDM systems mein "inter-carrier interference" bhi laa sakta hai. Yehi reason hai ki interference graph ko har 0.1-1 second mein fresh banaya jaata hai — channel itni jaldi badalta hai.
**Real-life analogy:** Ambulance ka siren — paas aate waqt awaaz "high" lagti hai, door jaate waqt "low" — same effect radio signals mein bhi hota hai moving vehicles ke saath.
**Easy way to remember:** Doppler effect = "movement ki wajah se signal/frequency mein shift."

---

## Spectrum Sharing
**What it is (from scratch):** Jab ek se zyada communication "services" (yahan: V2V aur V2N) ek hi spectrum (radio-frequency range) use karte hain, isko **spectrum sharing** kehte hain. Ye karna padta hai kyunki spectrum limited hai aur naya banaya nahi ja sakta.
**In this paper:** V2V aur V2N **same 20 RBs** share karte hain "in-band overlay" tareeke se — matlab dono same RBs use kar sakte hain. Isi sharing ki wajah se interference hoti hai, aur isi interference ko minimize karna GAT-A2C ka core goal hai.
**Real-life analogy:** Ek hi WiFi router ko ghar ke saare devices (phone, laptop, TV) share karte hain — agar sab ek saath heavy use karein, sabki speed slow ho jaati hai. Spectrum sharing bhi waisi hi situation hai radio-world mein.
**Easy way to remember:** Spectrum sharing = "ek hi resource, multiple users — coordination zaroori hai."

---

## O(n³H) Complexity
**What it is (from scratch):** "Complexity" batata hai ki kisi algorithm ko chalane mein "kaam" (computation) kitna badhta hai jab input-size (yahan: vehicles ki ginti `n`) badhta hai. `O(n³H)` ka matlab hai — agar `n` (vehicles) 2x ho jaaye, toh computation `2³ = 8x` ho jaata hai (cube relationship), aur `H` (attention heads ki ginti) bhi linearly affect karta hai.
**In this paper:** Ye GAT-A2C model ki overall complexity hai — har vehicle 3 V2V links banata hai (total nodes `3n`), aur GAT attention computation `O(n²H)` hai har node ke liye, jisse total `O(n³H)` banta hai. Authors khud admit karte hain ki ye cubic scaling high-density scenarios mein bottleneck ban sakta hai — isliye unhone "pretrain at low density, deploy at higher density" wala practical solution suggest kiya.
**Real-life analogy:** Socho ek group-chat mein har naya member aane se, sab existing members ko unse "interact" karna padta hai — chat ka size badhne se total interactions bahut tezi se (linearly se zyada) badh jaate hain.
**Easy way to remember:** O(n³H) = "vehicles double honge toh kaam 8x ho jaayega — bahut fast badhta hai."

---

## Entropy Regularization
**What it is (from scratch):** "Entropy" yahan ek measure hai ki Actor ka decision kitna "confident/fixed" hai vs "open to exploration." High entropy matlab Actor abhi bhi multiple actions try karne ko ready hai (exploration); low entropy matlab Actor ek hi action par "set" ho gaya hai (exploitation). **Entropy regularization** Actor ke training-goal mein ek extra term add karta hai jo high entropy ko reward deta hai — taaki Actor jaldi se ek hi action par "lock" na ho jaaye.
**In this paper:** Actor ke objective mein entropy term add hota hai (coefficient beta ke saath) — isse training ke early stages mein Actor different RB/power combinations try karta rehta hai, aur "local optimum" (ek thoda-achha-but-not-best solution par stuck ho jaana) mein phasne se bachta hai — jo zaroori hai kyunki vehicular environment continuously badal raha hai.
**Real-life analogy:** Naya restaurant try karna chhod kar hamesha same dish order karna — short-term mein safe lagta hai, lekin tumhe better options miss ho sakte hain. Entropy regularization Actor ko "kabhi-kabhi naya try karo" wala nudge deta hai.
**Easy way to remember:** Entropy regularization = "jaldi se ek hi choice par fix mat ho, explore karte raho."

---

## Discount Factor (gamma)
**What it is (from scratch):** RL mein agent ko sirf abhi ka reward nahi, future ke rewards bhi matter karte hain — lekin future thoda "uncertain" hota hai. **Discount factor (gamma)**, jo 0 aur 1 ke beech hota hai, decide karta hai future rewards ko "kitna kam" weight diya jaaye compared to immediate reward. Gamma = 1 matlab future bhi present jitna important; gamma chhota (jaise 0.5) matlab "abhi ka reward zyada matter karta hai, future thoda kam."
**In this paper:** Gamma = **0.5** — matlab agent near-term rewards ko zyada priority deta hai. Ye sense banata hai kyunki vehicular environment bahut fast-changing hai — distant future states ka prediction unreliable hota hai, toh "abhi ka best decision lo" zyada practical hai.
**Real-life analogy:** Agar tumhe pata hai kal weather kaisa hoga (predictable), toh tum kal ke plans ko aaj jaisa hi weight degi. Lekin agar agle hafte ka weather bahut unpredictable hai, toh tum usko kam weight degi apne aaj ke decisions mein. Gamma = "future ko kitna trust karo."
**Easy way to remember:** Gamma chhota = "abhi socho, future thoda kam matter karta hai (kyunki uncertain hai)."

---

## GraphSAGE (Full form: Graph SAmple and aggreGatE) — quick recap
**What it is (from scratch):** GraphSAGE bhi ek GNN type hai (paper-1 mein detail se cover hua) — har node apne **sab nahi, balki random-sample kiye gaye kuch neighbors** se info leta hai, aur unko "aggregate" (mix/combine, jaise average) karke apna summary banata hai. Sab neighbors ko roughly **equal weight** milta hai.
**In this paper:** GraphSAGE GAT ka "purana version" ya baseline-comparison ke roop mein zikar hota hai — GAT-A2C ka comparison plain DQN-based (GraphSAGE-style, no attention) approach se kiya gaya hai, aur GAT-A2C consistently better raha, especially 80-100 vehicles wale dense scenarios mein.
**Real-life analogy:** Agar tumhe apne poore mohalle ka mood janna ho, tum kuch (random sample) ghar jaake poochti ho, aur sabke jawab ko "barabar weight" dekar overall andaza lagati ho — koi ek ghar ki baat zyada important nahi maanti.
**Easy way to remember:** GraphSAGE = "sample lo, sabko equal weight dekar mix karo" — GAT iske aage jaata hai "kuch ko zyada weight do" ke saath.

---

(5 most important terms marked with ⭐: C-V2X, V2V, V2N, Resource Block, SINR, GAT, Attention Mechanism/Score, Multi-Head Attention, A2C, Actor, Critic — in saare concepts bina samjhe paper ka koi bhi page samajhna mushkil hoga, isliye inhe pehle pakka karo. Agar paper-1 padh chuki ho, toh C-V2X/V2V/V2N/RB/SINR tumhe already familiar lagenge — naya focus GAT aur A2C par karo.)
