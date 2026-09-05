# 01 — Analiza e Algoritmeve: Bazat

*(Java 1-2 e syllabusit: Dizajnimi i algoritmeve, analiza bazë, klasat hyrëse, kompleksiteti)*

---

## 1. Çka është Analiza e Algoritmeve (What is Analysis)

- **Analiza** e një algoritmi jep vlerësim (llogaritje) sa kohë do t'i duhet algoritmit për të zgjidhur problem me N vlera hyrëse (input).
- Qëllimi: **krahasimi i efikasitetit** ndërmjet algoritmeve që zgjidhin **të njëjtin problem** (kurrë nuk krahasohen p.sh. sortim me shumëzim matricash).
- Analiza **nuk** synon të japë kohë të saktë (sekonda/cikle) — kjo do të varej nga kompjuteri, procesori, kompajleri, ngarkesa etj. Prandaj analiza bëhet **e pavarur nga makina** konkrete.
- **Shembull hyrës**: dy algoritme për gjetjen e vlerës më të madhe nga 4 vlera — të dy bëjnë saktësisht 3 krahasime (kompleksitet kohor i njëjtë), por njëri përdor variabël të përkohshme `max` → **hapësirë (space) shtesë**; pra kanë kompleksitet hapësinor të ndryshëm por kohor të njëjtë.
- **Historia**: emri "algoritëm" rrjedh nga matematikani persian **Al-Khwarizmi** (shek. IX).
- **"Algoritëm i shpejtë në makinë të ngadaltë fiton algoritëm të ngadaltë në makinë të shpejtë"** — për n=1,000,000, algoritmi 50n·log₂n bën shumë më pak instruksione se 2n².

### 6 Pyetjet për t'i bërë kur dizajnoni një algoritëm
1. A e kuptoni plotësisht problemin?
2. A ka algoritëm i thjeshtë (edhe nëse jo optimal)?
3. A funksionojnë heuristikat (p.sh. nearest-neighbor për TSP — jep rezultat të mirë por jo domosdo optimal, ka kundërshembuj)?
4. A ka raste të veçanta për t'u kontrolluar?
5. Cili paradigmë dizajnimi përshtatet (divide-and-conquer, greedy, DP, etj.)?
6. A mund të përmirësohet edhe më tej?

### Numërimi i saktë i operacioneve (shembulli i numërimit të karaktereve)
```
for all 256 characters do
    assign zero to the counter
end for
while there are more characters in the file do
    get the next character
    increment the counter for this character by one
end while
```
- Unaza e parë (inicializuese): 257 përcaktime; 256 inkrementime; 257 verifikime kushti.
- Unaza e dytë: N+1 verifikime kushti; N inkrementime.
- **Totali**: Inkrementime = N+256; Përcaktime = 257; Verifikime = N+258.
- **N=500** → 1771 operacione, 770 (43%) lidhur me inicializimin. **N=50,000** → 100,771 operacione, po ai 770 (<1%) — **konkluzion: mos u përqendro te optimizime që s'ndikojnë asimptotikisht kur N rritet**.
- **Vlerësim me lidhje të shkurtër (short-circuit evaluation)**: në `A AND B`, B vlerësohet vetëm nëse A=True; në `A OR B`, B vlerësohet vetëm nëse A=False.

---

## 2. Klasat Hyrëse (Input Classes)

- Hyrja (input) përcakton shtegun e ekzekutimit → analiza duhet të marrë parasysh **të gjitha llojet** e input-it.
- Për N=10 elementë të ndryshëm ka **10! = 3,628,800** renditje — pamundur të shqyrtohen individualisht → **ndahen në klasa** sipas sjelljes së algoritmit.
- Shembulli i max: klasa përcaktohet nga pozita e vlerës më të madhe → **N klasa**.
- Për algoritëm që gjen max **dhe** min: **90 klasa** (10×9).

---

## 3. Çka Numërohet (What to Count)

Dy klasa tipike operacionesh të rëndësishme:
- **Krahasimet (comparisons)**: =, ≠, <, >, ≤, ≥ — kryesore te kërkimi (search) dhe sortimi (sorting).
- **Aritmetike**: **Aditive** (mbledhje, zbritje, inkrement) vs. **Multiplikative** (shumëzim, pjesëtim, modul) — shumëzimet marrin më shumë kohë se mbledhjet, numërohen veç.
- Rast special: shumëzim/pjesëtim me fuqi të 2 → mund të bëhet me **shift** (po aq i shpejtë sa mbledhja) — i rëndësishëm në "përçaj e sundo".

### Rastet për shqyrtim
- **Rasti më i mirë (Best Case)**: hyrja që kërkon më pak punë — rrallë analizohet (zakonisht trivial).
- **Rasti më i keq (Worst Case)**: jep **kufi të epërm (upper bound)** të performancës — më i analizuari.
- **Rasti mesatar (Average Case)**: hapat: (1) përcakto grupet e hyrjes, (2) përcakto gjasën e secilit grup, (3) përcakto kohën për secilin grup.
  - **A(n) = Σ (i=1..m) pᵢ · tᵢ**  (formula e përgjithshme)
  - Nëse gjasat janë të barabarta (pᵢ=1/m): **A(n) = (1/m) · Σ tᵢ**

---

## 4. Dokumentimi Matematikor (Mathematical Background)

- **Dysheme (Floor) ⌊X⌋**: numri i plotë më i madh ≤ X. `⌊2.5⌋=2, ⌊-7.3⌋=-8`.
- **Tavan (Ceiling) ⌈X⌉**: numri i plotë më i vogël ≥ X. `⌈2.5⌉=3, ⌈-7.3⌉=-7`.
- **Faktorieli**: N! = N·(N-1)·...·1. `10!=3,628,800`.
- **Logaritmet**: log_B X — rritje strikte (X>Y ⇒ log_B X > log_B Y); funksion 1-për-1.
- **Pemët binare**: N nyje e paketuar ngushtë → të paktën **⌊lg N⌋+1** nivele. Niveli K ka **2^(K-1)** nyje. Pemë komplete me J nivele → **2^J - 1** nyje.
- **Shumat e njohura**: Σ(i=1..N) i = **N(N+1)/2**; Σ(i=0..N) 2^i = **2^(N+1) - 1**.

---

## 5. Big-O, Big-Theta, Big-Omega — Definicionet Formale

*(Nga ligjërata "Performansa e Algoritmit" — përdor këto definicione formale për provim, jo vetëm intuitën)*

### Θ-Notation (Rendi i njëjtë / Tight bound)
Themi se **f(n) = Θ(g(n))** nëse ekzistojnë konstantet pozitive **n₀, c₁, c₂**, të tilla që për të gjitha n ≥ n₀:
$$c_1 g(n) \le f(n) \le c_2 g(n)$$
Ky notacion e **kufizon funksionin nga të dyja anët** (brenda faktorit konstant).

### O-Notation (Kufiri i epërm)
Themi se **f(n) = O(g(n))** nëse ekzistojnë konstantet pozitive **n₀, c**, të tilla që për n ≥ n₀:
$$f(n) \le c \cdot g(n)$$
Jep **kufirin e lartë** — nëse algoritmi A ∈ O(g) i algoritmit B, atëherë B nuk performon më mirë se A. **Kjo është klasa më e përdorur/interesante praktikisht.**

### Ω-Notation (Kufiri i poshtëm)
Themi se **f(n) = Ω(g(n))** nëse ekzistojnë konstantet pozitive **n₀, c**, të tilla që për n ≥ n₀:
$$f(n) \ge c \cdot g(n)$$
Jep **kufirin e poshtëm**.

**Marrëdhënia:** Θ(f) = Ω(f) ∩ O(f) — një funksion është Θ(g) vetëm nëse është njëkohësisht edhe O(g) edhe Ω(g).

**Mënyra praktike e gjetjes së Big-O:** g ∈ O(f) nëse `lim(n→∞) g(n)/f(n)` është numër real i fundëm.

### Kompleksiteti — Worst-case vs Average-case
- **Kompleksiteti rasti-më-i-keq**: koha e ekzekutimit për hyrje të çfarëdo madhësie do të jetë ≤ kufirin e epërm, përveç në disa raste ku arrihet maksimumi.
- **Kompleksiteti i rastit-mesatar**: mesatarja e operacioneve mbi të gjitha instancat e problemit të një madhësie të dhënë.
- Meqë vlerësimi i sjelljes statistikore të hyrjes është i vështirë, **shpesh kënaqemi me rastin më të keq**.
- Familjet kryesore të kompleksitetit: **n** (linear), **log n** (logaritmik), **nᵃ, a≥2** (polinomial), **aⁿ** (eksponencial).

### Optimaliteti
- Algoritmi është **optimal** nëse kompleksiteti i tij arrin kufirin e poshtëm përgjatë të gjitha algoritmeve që zgjidhin atë problem.
- **Shembull**: çdo algoritëm që zgjidh "pikëprerjen e n segmenteve" ekzekuton së paku n² operacione në rastin më të keq → problemi ka kompleksitet **Ω(n²)**. Nëse gjendet algoritëm O(n²), ai është optimal dhe i kompleksitetit **Θ(n²)**.

### Reduktimi (Reduction)
- Teknikë për vlerësimin e kompleksitetit të problemit përmes **transformimit** të problemit (reduktim).
- Nëse dihet kufiri i poshtëm i problemit A, dhe A mund të transformohet në problemin B me hapa më të lirë se zgjidhja e A, atëherë **B ka të njëjtin kufi të poshtëm si A**.
- *(Kjo teknikë përdoret më vonë për të vërtetuar që probleme janë NP-complete — shih skedarin 09.)*

---

## 6. Kompleksiteti Hapësinor (Space Complexity)

- Historikisht kritik kur memoria ishte e kufizuar; sot më pak i rëndësishëm, por ende i analizuar.
- **Shembull**: numër real me 1 shifër precizioni [-10,+10] si `real` → 4-8 bajt; i ruajtur si `integer` shumëzuar me 10 → vetëm 1 bajt (kursim 3-7 bajt/vlerë).
- Nëse hapësira shtesë s'varet nga N → **O(1)** (konstante). Nëse është proporcionale me N → **O(n)**.
- *(Analiza e detajuar iterative/rekursive e kompleksitetit hapësinor gjendet në skedarin 02, §4.)*

---

## 7. Algoritmet "Përçaj dhe Sundo" (Divide and Conquer)

### Skema e Përgjithshme
```
DivideAndConquer( data, N, solution )
  if (N ≤ SizeLimit) then
      DirectSolution( data, N, solution )
  else
      DivideInput( data, N, smallerSets, smallerSizes, numberSmaller )
      for i = 1 to numberSmaller do
          DivideAndConquer( smallerSets[i], smallerSizes[i], smallSolution[i] )
      end for
      CombineSolutions( smallSolution, numberSmaller, solution )
  end if
```
**4 komponentë për analizë**: (1) zgjidhja direkte (base case), (2) ndarja e hyrjes, (3) numri/thirrjet rekursive, (4) kombinimi i zgjidhjeve.

### Shembull: Faktorieli Rekursiv
```
Factorial( N )
  if (N = 1) then return 1
  else
      smaller = N - 1
      answer = Factorial( smaller )
      return (N * answer)
  end if
```
- Zgjidhje direkte: 0 operacione. Ndarja: 1 zbritje. 1 thirrje rekursive (N-1). Kombinimi: 1 shumëzim.
- Relacioni i rekurrencës: **T(n) = T(n-1) + 2**.

**Kjo është paradigma që del shpesh në provim** për probleme si "katrori magjik" (magic square) — shiko skedarin 11 për shembullin konkret të kërkuar në provim.

---

## 8. Kufinjtë e Poshtëm (Lower Bounds) — Shembulli i Sortimit

- **Pema e vendosjes (Decision Tree)**: nyjet = krahasime; gjethet = renditje finale; shtegu më i gjatë = rasti më i keq.
- **Derivimi i kufirit të poshtëm për sortim me krahasime:**
  1. Çdo permutacion i mundshëm duhet përfaqësuar nga ≥1 gjethe → duhen **N! gjethe**.
  2. Pema binare me L nivele ka 2^(L-1) nyje → **N! ≤ 2^(L-1)**.
  3. ⇒ **L = O(N log N)**.
- **Përfundim**: çdo algoritëm sortimi bazuar në krahasime ka kufi të poshtëm **O(N log N)** — asnjë s'mund të jetë më i shpejtë. Sortet O(N log N) (Merge, Heap) janë **optimale**.
- **Përjashtim**: **Radix Sort** punon në kohë lineare sepse **nuk krahason** vlerat çift-për-çift.

---

## 9. Analizimi i Programeve (Profiling)

- Teknikë: **numëratorë globalë**, një për secilin nënprogram, që inkrementohen kur thirret ai nënprogram → identifikon cilat nënprograme thirren më shpesh (ato duhen optimizuar së pari, jo domosdo ato "komplekse" por rrallë të thirrura).
- Baza e mjeteve moderne të **profiling/instrumentation**.

---

*(Vazhdon në skedarin `02_Kompleksiteti_Kohor_dhe_Hapesinor.md` për analizën e detajuar të Big-O për algoritme iterative/rekursive dhe Master Theorem-in e plotë.)*
