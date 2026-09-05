# 08 — Algoritmet e Grafeve (Graph Algorithms)

*(Java 10-12 e syllabusit — Prim/Kruskal dalin praktikisht në ÇDO provim, zakonisht me graf konkret të dhënë për të gjurmuar hapat.)*

---

## 1. Terminologjia

- **Graf G=(V,E)**: V=kulmet (vertices), E=degët (edges).
- **I padrejtuar** vs **i drejtuar (digraf)**: dega si bashkësi {vi,vj} vs çift i renditur.
- **Lak (loop)**: degë te vetja. **Degë e shumëfishtë**: >1 degë mes të njëjtit çift.
- **Graf komplet Kₙ**: degë mes çdo çifti → **(N²−N)/2** degë (padrejtuar); N²−N (digraf).
- **Shteg (path)**: sekuencë degësh me nyje unike. **Cikël/qark (cycle)**: shteg që kthehet te fillimi.
- **Graf i peshuar**: çdo degë ka kosto; shtegu më i shkurtër = kosto minimale (jo domosdo më pak degë).
- **I lidhur (connected)**: shteg mes çdo çifti nyjesh. **Urë (bridge)**: degë që, e hequr, e ndan grafin.
- **Shkalla e nyjes deg(V)**: numri i degëve që takohen (laku numërohet 2 herë).
- **Pikë artikulimi**: nyje që, e larguar, e bën grafin jo të lidhur.

## 2. Strukturat e të Dhënave

- **Matrica e fqinjësisë**: N×N, O(1) qasje, O(N²) hapësirë — mirë për grafe të dendura/të vogla.
- **Lista e fqinjësisë**: N listë të lidhura — mirë për grafe të rralla me shumë nyje.

## 3. DFS dhe BFS

```
DepthFirstTraversal(G, v)
  Visit(v); Mark(v)
  for every edge vw in G do
    if w not marked then DepthFirstTraversal(G, w)
```
```
BreadthFirstTraversal(G, v)
  Visit(v); Mark(v); Enqueue(v)
  while queue not empty do
    Dequeue(x)
    for every edge xw in G do
      if w not marked then Visit(w); Mark(w); Enqueue(w)
```
- DFS = **stek** (rekursion); BFS = **radhë (queue)**.
- Të dyja: **O(N)** — çdo nyje vizitohet 1 herë.
- **Aplikim**: nëse DFS/BFS nuk arrin të gjitha nyjet, grafi s'është i lidhur.
- **Komponentet bikonektuara**: bazuar në DFS + "lowlink" indeksi → O(N), më efikas se kontrollimi forcë-brutale O(N²).

## 4. Pema Minimale e Përhapjes (MST)

### Algoritmi Prim (Dijkstra-Prim)
Rritje nga një nyje: mban listë "periferie (fringe)", zgjedh gjithmonë degën më të lirë drejt periferisë.
```
select starting node; build fringe from its neighbors
while nodes left do
  choose smallest-weight edge to fringe; add node to tree
  update fringe (shto fqinjë të rinj, azhuro peshat minimale)
```

### Algoritmi Kruskal
Përqendrohet te degët: sorto të gjitha sipas peshës, shto në rend rritës nëse s'krijon cikël (union-find).
```
sort edges by weight ascending
for each edge (u,v) in order do
  if FindRoot(u) ≠ FindRoot(v) then
    add edge to MST; Union(u,v)
```
- **Kompleksiteti: O(E log E)** (i përcaktuar nga sortimi i degëve).
- Struktura **Union-Find (Partition)**: `Parent[]`, `FindRoot()` (ngjitet te rrënja), `Union()` (bashkon të voglin te i madhi). **Pseudokodi i plotë (me trukun e vlerave negative për madhësinë e komponentit) është në skedarin 13, §3** — kjo strukturë qëndron pas çdo implementimi real të Kruskal-it dhe mund të pyetet veç e veç.

⚠️ **MST NUK jep domosdo shtegun më të shkurtër mes 2 nyjesh** — mund të "flijojë" një degë të shtrenjtë duke krijuar shteg më të gjatë mes disa çifteve specifike.

**Dallimi Prim vs Kruskal**: Prim rritet **nga një nyje** (gjithmonë nënpemë e lidhur), i mirë për grafe të dendura; Kruskal shqyrton **degët globalisht** (mund të ketë komponentë të shkëputur gjatë procesit deri në fund), kërkon union-find, i mirë për grafe të rralla.

## 5. Shtegu më i Shkurtër — Dijkstra

Ndryshe nga Prim (pesha e 1 dege), Dijkstra konsideron **peshën totale të shtegut** nga nyja fillestare.
```
select starting node; build fringe
while jo te destinacioni do
  zgjidh nyjen në fringe me shtegun më të shkurtër nga fillimi
  shto në tree
  azhuro fringe (shto fqinjë, rillogarit shtigjet më të shkurtra përmes nyjes së re)
```
**Shembull**: A→G me disa rrugë — MST i të njëjtit graf mund të japë A→G=11, ndërsa Dijkstra jep shtegun real më të shkurtër A→G=10.

## 6. Shtegu/Qarku i Euler-it

Historia: **7 Urat e Königsberg-ut** (Euler, 1735).

- **Shteg i Euler-it**: kalon çdo degë saktësisht 1 herë (i hapur).
- **Qark i Euler-it**: si më sipër, por i mbyllur (kthehet te fillimi).
- Grafi i lidhur s'mund të ketë të dyja njëkohësisht.

**3 Teoremat e Euler-it:**
1. **0 nyje teke** + i lidhur → ka **Qark** të Euler-it.
2. **Saktësisht 2 nyje teke** + i lidhur → ka **Shteg** të Euler-it (fillon/mbaron te nyjet teke).
3. Shuma e shkallëve = 2×(numri i degëve) → **numri i nyjeve teke është gjithmonë çift**.

| Nr. nyjesh teke | Ekziston |
|---|---|
| 0 | Qark i Euler-it |
| 2 | Shteg i Euler-it |
| >2 | Asnjëri |

**Algoritmi i Fleury-t**: "mos i digjni urat" — shmang kalimin nëpër urë (bridge) përveç kur s'ka alternativë tjetër.

**Euler-izimi**: shtim i degëve DUPLIKATE (jo të reja) për të kthyer nyjet teke në çifte, minimizuar rikalimet.

## 7. Shtojcë: Hamilton dhe TSP

- **Shteg/Qark i Hamilton-it**: kalon çdo **nyje** 1 herë (ndryshe nga Euler = degë).
- **TSP**: qarku optimal i Hamilton-it në graf komplet të peshuar. Brute-force: (N-1)! qarqe — joefikas. Heuristikat: fqinji më i afërt (nearest-neighbor), linku më i lirë (cheapest-link) — efikase por jo optimale.
- *(TSP si problem NP-complete trajtohet plotësisht në skedarin 09.)*
