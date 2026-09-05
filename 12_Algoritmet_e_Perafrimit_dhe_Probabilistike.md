# 12 — Algoritmet e Përafrimit (Approximation) dhe Algoritmet Probabilistike

*Kjo është TEMA E RE, e gjetur në materialin shtesë të asistentit (kapitulli 9 i librit McConnell) — NUK ishte e mbuluar në skedarët 01-11 fillestarë. Meqë libri i kursit e ka si kapitull të plotë (dhe lidhet direkt me temën e "optimizimit"/TSP/knapsack/etj. të javës 14 të syllabusit dhe me P/NP-complete të skedarit 09), rekomandohet të mësohet të paktën konceptualisht.*

---

## 1. Algoritmet e Përafrimit Greedy (për probleme NP-complete)

Meqë problemet NP-complete (skedari 09) s'kanë zgjidhje efikase të njohur, në praktikë përdoren **algoritme greedy të përafrimit** — të shpejta, por pa garanci optimaliteti.

**Formula e cilësisë së përafrimit:**
- Për probleme **minimizimi**: `Vlera(Sperafërt) / Vlera(Soptimal)` (sa më afër 1, aq më mirë)
- Për probleme **maksimizimi**: `Vlera(Soptimal) / Vlera(Sperafërt)`

### 1.1 TSP — Përafrim Greedy (Cheapest-Link)
Rregulli: shto degë sipas peshës rritëse, me kusht (1) mos krijo cikël para se të përfshihen të gjitha nyjet, (2) asnjë nyje s'merr më shumë se 2 degë.

**Shembull i punuar**: Për një graf specifik, greedy zgjedh degët në rend dhe jep tur me gjatësi totale **53**, ndërsa turi optimal (i gjetur me kërkim shterrues) ka gjatësi **41**. **Konkluzion: greedy s'është optimal, por është i shpejtë.**

### 1.2 Bin Packing — Algoritmet First-Fit / First-Fit-Decreasing
```
FirstFit(size, N, bin)
  for i = 1 to N do binUsed[i] = 0
  for item = 1 to N do
    binLoc = 1
    while used[binLoc]+size[item] > 1 do binLoc += 1
    bin[item] = binLoc; used[binLoc] += size[item]
```
**Shembull i punuar 1**: objekte (0.5,0.7,0.3,0.9,0.6,0.8,0.1,0.4,0.2,0.5) → First-Fit jep **6 kuti**; optimali është **5 kuti**.

**Shembull i punuar 2 (kujdes — sortimi mund të "dëmtojë")**: objekte (0.2,0.6,0.2,0.5,0.2,0.8,0.3,0.2) → First-Fit i pastër (pa sortim) jep **3 kuti (optimal!)**; First-Fit-**Decreasing** (i sortuar në rend zbritës më parë) jep **4 kuti** — më keq! **Mësim: sortimi paraprak nuk garanton përmirësim gjithmonë.**

**Kufij të njohur**: First-Fit-Decreasing përdor ~50% më shumë kuti se optimali; First-Fit (pa sortim) ~70% më shumë.

**Variante të tjera** (të kërkuara si ushtrime): **Best-Fit** (vendos ku mbetet hapësira më e vogël), **Next-Fit** (kurrë s'rikthehet te kuti të mëparshme), **Worst-Fit** (vendos ku mbetet hapësira më e madhe).

### 1.3 Knapsack — Përafrim Greedy sipas Raportit Vlerë/Madhësi
Rregulli: sorto objektet sipas raportit vlerë/madhësi (zbritës), shto derisa të mbushet kapaciteti.

**Shembull i punuar**: objekte (madhësi,vlerë): (25,50),(20,80),(20,50),(15,45),(30,105),(35,35),(20,10),(10,45). Kapaciteti=80. Greedy (sipas raportit) merr 4 objektet e para → madhësi totale 75, vlerë **275**. **POR zgjidhja optimale** (objektet 1,2,3,5) jep madhësi 80, vlerë **280** — greedy s'është optimal këtu.

### 1.4 Subset-Sum — Algoritmi Shumë-Kalimësh (Multi-Pass)
Gjeneron gradualisht kombinime më të mëdha (0 elemente, 1 element, 2 elemente, ...) dhe kontrollon cilat i afrohen kufirit pa e kaluar. **Shembull**: madhësi {27,22,14,11,7,1}, kufi 55 → zgjidhja optimale **55** gjendet që në kalimin e 3-të.

### 1.5 Graph Coloring — Ngjyrosje Sekuenciale Greedy
```
ColorGraph(G)
  for i = 1 to N do
    c = 1
    while ka nyje fqinje të nyjes i të ngjyrosur me c do c += 1
    ngjyros nyjen i me c
```
Përdor **C = (shkalla maksimale e grafit) + 1** ngjyra. **Rezultat teorik i njohur**: asnjë algoritëm polinomial i njohur nuk garanton ≤2× numrin optimal të ngjyrave — nëse do të ekzistonte, kjo do të implikonte **P=NP**. (Përjashtim: grafet planare kanë algoritme polinomiale ngjyrosjeje eficiente.)

---

## 2. Algoritmet Probabilistike (4 kategori)

### 2.1 Algoritme Numerike Probabilistike
**Gjilpëra e Buffon-it**: vlerësim i π duke hedhur shkopinj mbi dysheme me spica paralele — gjasa e prekjes së një spice është 1/π.

**Vlerësimi i π me "shenjestër" (dartboard)**: rrethi i brendashkruar në katror, hedh pika rastësore → π ≈ 4·(pika brenda rrethit)/(totali i pikave).

**Integrimi Monte Carlo**:
```
Integrate(f, dartCount)
  hits = 0
  for i=1 to dartCount do
    x=uniform(0,1); y=uniform(0,1)
    if y <= f(x) then hits += 1
  return hits/dartCount
```

**Paradoksi i Ditëlindjes**: në dhomë me 25 njerëz, P(≥2 kanë të njëjtën ditëlindje) > 56%.

### 2.2 Algoritme Monte Carlo (mund të japin përgjigje të gabuar, por gjithmonë të shpejtë)

**`Monte3(x)`**: thirr algoritmin Monte Carlo 3 herë, kthe shumicën — përmirëson saktësinë (p.sh. nga 80% në ~90%).

**Elementi i Shumicës (Majority Element) — algoritëm probabilistik "true-biased":**
```
Majority(list, N)
  choice = uniform(1,N); count=0
  for i=1 to N do if list[i]=list[choice] then count+=1
  return (count > N/2)
```
Thirrur 5 herë → saktësi ~97%, kompleksiteti O(N) (5N).
*(Shënim: ky ndryshon nga zgjidhja deterministike O(n) e "Majority Element" — algoritmi i Boyer-Moore Voting, i cili jep gjithmonë përgjigjen e saktë nëse elementi shumicë ekziston me siguri — shih skedarin 13 për versionin deterministik që del në provime.)*

**Testi Monte Carlo i Primitetit**: për N=60,329=23×43×61, testimi i pjesëtueshmërisë me numër të rastit nga 2..√N ka vetëm ~1.2% gjasë të zbulojë se N është i përbërë në 1 provë — prandaj përsëritet shumë herë.

### 2.3 Algoritme Las Vegas (gjithmonë japin përgjigje korrekte, por koha e ekzekutimit është e ndryshueshme)

**Formula e kohës së pritur:**
$$time(x) = success(x) + \frac{1-p(x)}{p(x)} \cdot failure(x)$$

**Shembull: 8-Mbretëreshat (8-Queens) me Las Vegas**: vendos mbretëreshat rastësisht rresht-për-rresht mes pozitave jo të sulmuara, rifillon nga zero nëse ngec. Probabiliteti i suksesit ≈ 0.1293, numri i pritur i tentativave të dështuara ≈ 6.971 → gjithsej ~55 tentativa totale të pritura.

### 2.4 Algoritme Sherwood (përmirësojnë rastin mesatar, pa ndryshuar korrektësinë)

- **Quicksort me pivot të rastësishëm**: zgjedh pivotin rastësisht (jo gjithmonë të parin) — shmang rastin më të keq O(n²) mbi hyrje të renditura paraprakisht (shih skedarin 05, §7 — kjo është zgjidhja praktike për "dobësinë" e Quicksort naiv).
- **Kërkimi Binar Sherwood**: në vend të gjithmonë kontrollit të elementit të mesit, kontrollo element të zgjedhur rastësisht (jo domosdo mesin) — hedh poshtë 75% ose 25% të elementeve të mbetura (në vend të gjithmonë 50%/50%) — kompensohet statistikisht mbi shumë kërkime.

---

## 3. Pse kjo Temë Lidhet me Skedarët e Tjerë

- Lidhet me **skedarin 09** (P/NP/NP-complete) — përafrimi është "zgjidhja praktike" kur problemi është NP-complete.
- Lidhet me **skedarin 05** (Quicksort) — pivoti i rastësishëm (Sherwood) është përgjigja standarde ndaj pyetjes "si e shmangni rastin më të keq O(n²) të Quicksort?"
- Lidhet me **skedarin 08** (TSP, te Euler/Hamilton) — përafrimi greedy i TSP është vazhdimësi direkte e diskutimit teorik të TSP.

**Vlerësim provimi**: kjo temë s'u shfaq në asnjë nga 15 provimet e kaluara të shqyrtuara deri tani, kështu që prioriteti është **i ulët-mesëm** — mjafton të kuptoni konceptualisht "çka është një algoritëm përafrimi/greedy" dhe "çka janë algoritmet Monte Carlo vs Las Vegas" (këto koncepte të përgjithshme mund të pyeten teorikisht), pa qenë nevoja të mësoni përmendësh çdo shembull numerik.
