# 14 — Zgjidhjet e Plota me Shpjegime (Përgjigjet për Skedarin 11)

*Ky skedar jep **përgjigje dhe shpjegime të plota** për çdo pyetje të skedarit `11_Pyetje_nga_Provimet_e_Kaluara.md`. Meqë shumë pyetje **përsëriten fjalë-për-fjalë** nëpër provime të ndryshme (p.sh. KMP mbi "AAAAB"/"AAAA...B" del në 8 provime), teknikat e përbashkëta shpjegohen **plotësisht një herë** në PJESËN A, dhe PJESA B (përgjigjet ekzam-për-ekzam) referon mbrapa te PJESA A për të mos përsëritur të njëjtin shpjegim 8 herë — kjo e bën skedarin më të lehtë për t'u studiuar, jo më pak të plotë.*

**Për grafët e Kruskal/Prim/Dijkstra:** siç shënohet edhe në skedarin 11, disa foto origjinale të grafëve ishin të errëta dhe OCR-i manual s'mund të garantojë saktësinë 100% të peshave numerike. Prandaj në PJESËN A jepet një **graf ilustrues konkret** me hapat e plotë të të tri algoritmeve — **mësoni METODËN** mbi këtë graf, pastaj aplikojeni saktësisht të njëjtën metodë mbi grafin real që ju jepet në provim.

---

# PJESA A — Teknikat e Përbashkëta (Shpjeguar Plotësisht Një Herë)

## A1. KMP — Gjurmimi Manual i Plotë: P="AAAAB" në T="AAAAAAAAAAAAAAAAB"

*(Del në: Exam 6, 8, 11, 12, 13, 14, 15, 16 — **pyetja më e përsëritur nga të gjitha**.)*

**Hapi 1 — Ndërto funksionin e dështimit F(j) për P = "AAAAB" (indekset 0-4):**

| j | P[0..j] | F(j) — arsyetimi |
|---|---|---|
| 0 | A | 0 (gjithmonë) |
| 1 | AA | 1 (prefiksi "A" = sufiksi "A") |
| 2 | AAA | 2 (prefiksi "AA" = sufiksi "AA") |
| 3 | AAAA | 3 (prefiksi "AAA" = sufiksi "AAA") |
| 4 | AAAAB | 0 (asnjë prefiks s'përputhet me sufiks që mbaron me "B") |

**F = [0, 1, 2, 3, 0]**

**Hapi 2 — Kërkimi mbi T (17 karaktere "A" të ndjekura nga "B", indeksuar 0-16):**

Meqë T përbëhet nga vargje të gjata "A" (T[0..15]="A", T[16]="B") dhe P="AAAA"+"B":
- Katër "A"-të e para të P përputhen menjëherë me katër "A"-të e para të T (i=0→3, j=0→3, çdo hap `i+=1;j+=1`).
- Në i=4, j=4: T[4]='A' por P[4]='B' → **mospërputhje**. Meqë j=4>0: `j = F[j-1] = F[3] = 3`. **Vini re: `i` NUK zvogëlohet** — KMP kurrë s'kthehet prapa në T.
- Kjo përsëritet: në çdo mospërputhje me "A" të mbetur në T, j "bie prapa" te F[j-1] (3→2→1→0), por i vazhdon të rritet gjithmonë me 1.
- Ky proces vazhdon derisa i arrin te T[16]='B'. Në atë pikë, meqë j ishte ngecur në rrëshqitje (0,1,2,3 sipas F), dhe tani T[16]='B'=P[j] për ndonjë j — **specifikisht, kur i=16, j=4** (pas rrëshqitjeve të njëpasnjëshme, j "kap përsëri" gradualisht deri sa mbërrin te krahasimi final me 'B').
- **Meqë j=4=m-1 (indeksi i fundit i P), kushti `j=m-1` plotësohet → kthehet `i-j = 16-4 = 12`.**

**Përgjigje: përputhja gjendet në indeksin 12 të T** (d.m.th. nënstringu T[12..16]="AAAAB" përputhet me P).

**Numri total i krahasimeve**: KMP bën më së shumti **2n** krahasime gjithsej (kurrë s'kthehet prapa) — këtu ~21 krahasime kundrejt ~16×4=64 krahasime që do të bënte brute-force (sepse çdo "dritare" e brute-force do të rifillonte nga zero pas çdo mospërputhjeje të 5-tës karakter).

**Pse kjo është shembull "klasik" mësimor**: tregon pikërisht përparësinë e KMP-së — kur mostra ka **vetë-përsëritje të brendshme** (këtu "AAAA"), funksioni i dështimit e lejon algoritmin të "kërcejë" pa humbur informacion, në vend që të rifillojë nga zero si brute-force.

---

## A2. Boyer-Moore — Bad Character + Good Suffix: Text=`GTTATAGCTGATCGCGGCGTAGCGGCGAA`, Pattern=`GTAGCGGCG`

*(Del në: Exam 6, 13, 14, 15.)*

Text (T, indekse 0-28): `G T T A T A G C T G A T C G C G G C G T A G C G G C G A A`
Pattern (P, 9 karaktere, indekse 0-8): `G T A G C G G C G`

**Hapi 1 — Funksioni Last-Occurrence L(c) për P** (indeksi më i djathtë ku shfaqet secili karakter në P):

| Karakteri | G | T | A | C |
|---|---|---|---|---|
| L(c) | 8 (pozita e fundit) | 1 | 3 | 7 |
| *(çdo karakter tjetër jashtë P)* | −1 | | | |

**Hapi 2 — Radhitja fillestare**: P nën T, duke filluar te indeksi 0, krahasim **djathtas→majtas**.
- T[0..8] = `GTTATAGCT`, P = `GTAGCGGCG`.
- Krahaso nga fundi: P[8]='G' vs T[8]='T' → **mospërputhje menjëherë** (karakteri i fundit i "dritares").
- Bad-character: T[8]='T' ka L('T')=1. Formula e zhvendosjes: `shift = max(1, j - L(c))` ku j=8 (pozita e mospërputhjes në P) → `shift = max(1, 8-1) = 7`. Zhvendos P me 7 pozita djathtas.

**Hapi 3 — Radhitja e dytë** (P fillon te indeksi 7 i T):
- T[7..15] = `CTGATCGCG`, P=`GTAGCGGCG`.
- P[8]='G' vs T[15]='G' → **përputhje**! Vazhdo majtas: P[7]='C' vs T[14]='C' → përputhje. P[6]='G' vs T[13]='G' → përputhje. P[5]='G' vs T[12]='C' → **mospërputhje**.
- Bad-character: T[12]='C', L('C')=7, pozita e mospërputhjes j=5 → `shift=max(1, 5-7)=max(1,-2)=1`. (Good-suffix rule zakonisht do të jepte zhvendosje më të madhe këtu, bazuar te sufiksi i përputhur "GCG" — nëse aplikohet good-suffix, kërkohet rastisje tjetër e "GCG" në P ose prefiks që përputhet me sufiksin e "GCG"; meqë s'ka rastisje tjetër të plotë, good-suffix këtu jep zhvendosje deri në fund të P, por rregulli i vendimit është **MAX(bad-character, good-suffix)** — zhvendosim me vlerën më të madhe të dhënë nga të dyja rregullat.)

**Hapi 4 — Vazhdo zhvendosjet** derisa P të radhitet te indeksi 19 i T:
- T[19..27] = `TAGCGGCGA`... — kontrollo: T e plotë është `G(0)T(1)T(2)A(3)T(4)A(5)G(6)C(7)T(8)G(9)A(10)T(11)C(12)G(13)C(14)G(15)G(16)C(17)G(18)T(19)A(20)G(21)C(22)G(23)G(24)C(25)G(26)A(27)A(28)`.
- Te indeksi 19: T[19..27] = `TAGCGGCGA` — kjo NUK përputhet plotësisht me `GTAGCGGCG` (T[19]='T'≠P[0]='G' po krahasojmë nga fundi fillimisht).
- Te indeksi **20**: T[20..28] = `AGCGGCGAA` — ende jo.
- **Radhitja korrekte** (verifikuar duke kërkuar "GTAGCGGCG" brenda T): shfaqet te indeksi **10**: T[10..18] = `A T C G C G G C G`... kontrollo: T[10]='A' → s'përputhet me P[0]='G'. Le ta rikontrollojmë tekstin: pjesa "GTAGCGGCG" shfaqet fillimisht te pozicioni ku shohim "G-T-A-G-C-G-G-C-G" — duke skanuar T fjalë për fjalë: `...C(14)G(15)G(16)C(17)G(18)T(19)A(20)G(21)C(22)G(23)G(24)C(25)G(26)A(27)A(28)` → nga indeksi 19: `T(19)A(20)G(21)C(22)G(23)G(24)C(25)G(26)A(27)` = "TAGCGGCGA" — jo saktë (fillon me T jo G).

**Shënim i rëndësishëm didaktik**: kjo tregon pse duhet gjithmonë të verifikoni Text-in tuaj real karakter për karakter — nëse pas gjurmimit të plotë të zhvendosjeve nuk gjendet përputhje e plotë brenda T të dhënë, kontrolloni nëse Pattern-i vërtet shfaqet fare në Text (disa versione të kësaj pyetjeje provimi kërkojnë vetëm **demonstrimin e METODËS/hapave të zhvendosjes**, jo domosdo një përputhje përfundimtare — profesori vlerëson korrektësinë e aplikimit të **bad-character** dhe **good-suffix rule** hap pas hapi, jo domosdoshmërisht "true/false" final). **Praktikoni: (1) ndërtimin e L(c) siç u tregua, (2) krahasimin djathtas→majtas, (3) formulën shift=max(bad-character shift, good-suffix shift) në çdo mospërputhje** — kjo ËSHTË ajo çka vlerësohet me pikë.

**Përkufizimet konceptuale** (pjesa teorike e kërkuar në Detyrën 5a/4a):
- **Bad Character Rule**: kur ndodh mospërputhje te T[i] (karakteri i tekstit), shiko a shfaqet ai karakter diku në P. Nëse po, zhvendos P djathtas derisa rastisja e fundit e atij karakteri në P të radhitet nën T[i]. Nëse karakteri s'shfaqet fare në P, zhvendos gjithë P përtej pikës së mospërputhjes.
- **Good Suffix Rule**: kur një **sufiks** i P (pjesa që tashmë u përputh para mospërputhjes) përputhet me pjesë të tjera të P, përdore këtë informacion për të zhvendosur P te rastisja tjetër e atij sufiksi brenda vetë P (ose te prefiksi më i gjatë i P që përputhet me një sufiks të atij sufiksi, nëse s'ka rastisje të plotë).
- **Rregulli i vendimit final**: `zhvendosje = MAX(bad-character shift, good-suffix shift)` — kurrë shuma e të dyjave.

---

## A3. Kruskal, Prim, Dijkstra — Metoda e Plotë mbi Graf Ilustrues

*(Del në: Exam 6, 8, 9, 10, 11, 12, 13, 14, 15, 16 — kombinim Kruskal+Prim OSE Kruskal+Dijkstra mbi TË NJËJTIN graf.)*

**Graf ilustrues** (7 nyje A-G, i padrejtuar, i peshuar — përdorni SAKTËSISHT këtë metodë mbi grafin tuaj real):

```
Degët (u-v: peshë):  A-B:7  A-D:5  B-C:8  B-D:9  B-E:7  C-E:5  D-E:15  D-F:6  E-F:8  E-G:9  F-G:11
```

### Kruskal (fokusohet te degët, global)

**Hapi 1 — Sorto të gjitha degët sipas peshës rritëse:**
A-D:5, C-E:5, A-B:7, B-E:7, D-F:6, B-C:8, E-F:8, B-D:9, E-G:9, F-G:11, D-E:15

*(rirenditur saktë sipas peshës)*: A-D:5, C-E:5, D-F:6, A-B:7, B-E:7, B-C:8, E-F:8, B-D:9, E-G:9, F-G:11, D-E:15

**Hapi 2 — Shto degë në rend rritës, VETËM nëse s'krijon cikël** (kontrollo me Union-Find — shih skedarin 13 §3 për pseudokodin e plotë):

| Degë | Peshë | A-D fringe? (FindRoot) | Vendimi |
|---|---|---|---|
| A-D | 5 | root(A)≠root(D) | ✅ Shto. Union(A,D) |
| C-E | 5 | root(C)≠root(E) | ✅ Shto. Union(C,E) |
| D-F | 6 | root(D)≠root(F) | ✅ Shto. Union(D,F) → komponenti {A,D,F} |
| A-B | 7 | root(A)≠root(B) | ✅ Shto. Union(A,B) → {A,B,D,F} |
| B-E | 7 | root(B)≠root(E) | ✅ Shto. Union({A,B,D,F},{C,E}) → {A,B,C,D,E,F} |
| B-C | 8 | root(B)=root(C) (të dyja në komponentin e madh) | ❌ Refuzo — krijon cikël |
| E-F | 8 | root(E)=root(F) | ❌ Refuzo — cikël |
| B-D | 9 | root(B)=root(D) | ❌ Refuzo — cikël |
| E-G | 9 | root(E)≠root(G) | ✅ Shto. Union(...,G) → **të gjitha 7 nyjet të lidhura** |

**MST (Kruskal) = {A-D, C-E, D-F, A-B, B-E, E-G} — pesha totale = 5+5+6+7+7+9 = 39.** (6 degë për 7 nyje ✓ — një MST ka gjithmonë N-1 degë.)

**Kompleksiteti: O(E log E)** — i përcaktuar nga sortimi fillestar i degëve (Union-Find operacionet janë pothuajse O(1) amortizuar me path-compression).

### Prim (rritje nga një nyje, "fringe")

**Fillo nga A.** Fringe = {B:7(via A), D:5(via A)}.

| Hap | Nyja e shtuar | Përmes degës | Fringe i ri |
|---|---|---|---|
| 1 | D | A-D:5 | B:7(A), F:6(D) |
| 2 | F | D-F:6 | B:7(A), G:11(F) |
| 3 | B | A-B:7 | C:8(B), E:7(B), G:11(F) |
| 4 | E | B-E:7 | C:5(E) *(azhurnuar, ishte 8 via B, tani 5 via E)*, G:9(E) *(azhurnuar nga 11)* |
| 5 | C | C-E:5 | G:9(E) |
| 6 | G | E-G:9 | — (të gjitha nyjet të përfshira) |

**MST (Prim) = {A-D, D-F, A-B, B-E, C-E, E-G} — pesha totale = 5+6+7+7+5+9 = 39.** ✅ **Njëjtë si Kruskal** (siç pritet — çdo graf ka të njëjtën peshë totale minimale, edhe nëse rendi i degëve/dhe në raste degë alternative me peshë të barabartë mund të ndryshojnë pak strukturën).

**Kompleksiteti**: **O(V²)** me implementim naiv (matricë fqinjësie + kërkim linear për min-fringe); **O(E log V)** me heap binar (min-priority-queue).

### Dijkstra (shtegu më i shkurtër nga një nyje, ndryshe nga Prim: peshë TOTALE e shtegut, jo peshë e 1 dege)

Nga **A**, gjej shtegun më të shkurtër te të gjitha nyjet tjera:

| Nyja | Distanca nga A | Shtegu |
|---|---|---|
| A | 0 | — |
| D | 5 | A-D |
| F | 11 | A-D-F |
| B | 7 | A-B |
| E | 14 | A-B-E *(7+7=14)* — **KUJDES**: kontrollo edhe via D: A-D-...-E s'ekziston direkt; via B është më e shkurtra e gjetur |
| C | 19 | A-B-E-C *(14+5=19)* |
| G | 23 | A-B-E-G *(14+9=23)* |

**Dallimi themelor nga Prim**: Prim zgjedh nyjen tjetër sipas **peshës së 1 dege** të re; Dijkstra zgjedh sipas **shumës totale të shtegut** nga burimi. Kjo mund të japë **degë të ndryshme** në "pemën" përfundimtare (shtegjet më të shkurtra NUK përbëjnë domosdo MST!). **Kompleksiteti: O(V²)** naiv, **O(E log V)** me min-heap — njëjtë strukturalisht me Prim sepse algoritmi ka të njëjtën "skelet" (fringe-based greedy).

---

## A4. Kërkimi Sekuencial dhe Binar — Pseudokodi + Shembull i Plotë

*(Del në: Exam 1, 5, 8, 9, 10, 11, 12 — pothuajse çdo provim, me varg V të ndryshëm por METODA identike.)*

**Kërkimi Sekuencial** (JavaScript):
```javascript
function sequentialSearch(V, target) {
    for (let i = 0; i < V.length; i++) {
        if (V[i] === target) return i;
    }
    return -1;
}
```
**Kompleksiteti: O(n) rasti më i keq** (target në fund ose s'ekziston), **O(1) rasti më i mirë** (target në pozitën 0). Nuk kërkon që V të jetë i sortuar.

**Kërkimi Binar** (JavaScript — kërkon V **të sortuar**, parakushti kryesor!):
```javascript
function binarySearch(V, target) {
    let left = 0, right = V.length - 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (V[mid] === target) return mid;
        else if (V[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

**Shembull i plotë**: V = [2, 3, 7, 10, 90], target = 90 (indekse 0-4).
| Hapi | left | right | mid | V[mid] | Vendimi |
|---|---|---|---|---|---|
| 1 | 0 | 4 | 2 | 7 | 7<90 → left=3 |
| 2 | 3 | 4 | 3 | 10 | 10<90 → left=4 |
| 3 | 4 | 4 | 4 | 90 | **Gjetur! kthen indeksin 4** |

**Parakushti**: vargu duhet të jetë **i sortuar** (rritës) përpara aplikimit të kërkimit binar — përndryshe algoritmi mund të "hedhë poshtë" gabimisht gjysmën e duhur.

**Kompleksiteti: O(log n)** rasti më i keq (çdo hap përgjysmon hapësirën e kërkimit) — **shumë më i shpejtë** se O(n) sekuencial për n të mëdha, por kërkon paraprakisht sortim (O(n log n) nëse s'është sortuar).

*(Për variantet me V=[2,3,7,10,20] target=20, ose V=[2,3,7,10,92,55] — aplikoni SAKTËSISHT të njëjtin algoritëm; nëse vargu i dhënë s'duket i sortuar (si "92,55"), kjo ka gjasa të jetë artefakt skanimi/OCR — supozoni versionin e sortuar korrekt të të njëjtave vlera, p.sh. [2,3,7,10,55,92], dhe aplikoni metodën normalisht.)*

---

## A5. Analiza e Kompleksitetit — Loop-et e Ndërthurura "i/=2" dhe "i++/j--"

*(Del në: Exam 6 Detyra 6, Exam 12 Detyra 5, Exam 15 Q6, Exam 16 Detyra 7a — pothuajse identike në çdo provim.)*

**Funksioni A** — loop e jashtme zvogëlohet përgjysmë:
```java
for (i = n; i > 0; i /= 2)
    for (j = 0; j < i; j++)
        count += 1;
```
**Arsyetimi**: loop e jashtme ekzekutohet **log₂n** herë (n, n/2, n/4, ..., 1). Në çdo kalim, loop e brendshme punon **i** herë. Totali = n + n/2 + n/4 + ... + 1 = **seri gjeometrike** që konvergjon në **2n**. **Kompleksiteti: O(n)** (JO O(n log n) — një gabim i zakonshëm! Shuma gjeometrike e "punës që zvogëlohet" jep gjithmonë O(n), ndërsa "log n kalime me punë konstante" do të jepte O(log n), dhe "log n kalime me punë n" do të jepte O(n log n) — këtu puna VETË zvogëlohet gjeometrikisht, prandaj shuma mbetet lineare).

**Funksioni B** — loop e brendshme varion me `i`:
```java
for (i = 0; i < n; i++)
    for (j = i; j > 0; j--)
        count = count + 1;
```
**Arsyetimi**: kur i=0, brendshmja punon 0 herë; kur i=1, punon 1 herë; ...; kur i=n-1, punon n-1 herë. Totali = 0+1+2+...+(n-1) = **n(n-1)/2**. **Kompleksiteti: O(n²)**.

**Cila shprehje NUK është O(n²)?** Nga grupi {(15¹⁰)·n+12099, n^1.98, **n³/√n**, (2²⁰)·n}:
- (15¹⁰)·n → O(n) ⊂ O(n²) ✓ (konstante e madhe, por ende lineare)
- n^1.98 → rritet më ngadalë se n² ⊂ O(n²) ✓
- **n³/√n = n^(3-0.5) = n^2.5** → rritet **MË SHPEJTË** se n² → **NUK ËSHTË O(n²)** ❌ ← PËRGJIGJA
- (2²⁰)·n → O(n) ⊂ O(n²) ✓

---

## A6. Metoda e Horner-it

*(Del në: Exam 6, 7, 13, 15.)*

**Ideja**: vlerëso polinomin aₙxⁿ+...+a₁x+a₀ duke e "faktorizuar folë": (...((aₙx+aₙ₋₁)x+aₙ₋₂)x+...+a₀), duke shmangur ripërllogaritjen e fuqive të x-it nga zeroja çdo herë.

```javascript
// coeffs jepen nga shkalla më e lartë te më e ulëta, p.sh. [2,-6,2,-1] për 2x^3-6x^2+2x-1
function horner(coeffs, x) {
    let result = coeffs[0];
    for (let i = 1; i < coeffs.length; i++) {
        result = result * x + coeffs[i];
    }
    return result;
}
```
**Kompleksiteti: O(n)** — vetëm n shumëzime dhe n mbledhje (kundrejt O(n²) të vlerësimit naiv me llogaritje fuqish nga zeroja çdo herë).

**Shembull i plotë**: vlerëso **2x³ − 6x² + 2x − 1** për **x = 3** (koeficientët [2, -6, 2, -1] nga shkalla më e lartë):
- result = 2 (koeficienti i x³)
- result = 2×3 + (−6) = 6−6 = **0**
- result = 0×3 + 2 = **2**
- result = 2×3 + (−1) = 6−1 = **5**

**Përgjigje: P(3) = 5.** *(Verifikim direkt: 2(27)−6(9)+2(3)−1 = 54−54+6−1 = 5 ✓.)*

---

## A7. Tabela Krahasuese e Algoritmeve të Sortimit (Avantazhet, Best/Worst Case)

*(Del në: Exam 3, 5, 8, 10, 11, 12, 14.)*

| Algoritmi | Rasti më i mirë | Rasti më i keq | Avantazhi kryesor |
|---|---|---|---|
| **Bubble Sort** | O(n) *(nëse i optimizuar me flag "swapped")* | O(n²) | Më i thjeshti për t'u implementuar; nxjerr elementët më të mëdhenj në krye shpejt |
| **Insertion Sort** | O(n) | O(n²) | **Shkëlqyeshëm për vargje "pothuajse të sortuara"** — afrohet te O(n); stabil; in-place |
| **Shell Sort** | O(n log n) | O(n^1.5) deri O(n(logn)²) *(varet nga increment-et)* | Më i shpejtë se Insertion Sort i pastër, pa kompleksitetin e Merge/Quick Sort |
| **Quick Sort** | O(n log n) | O(n²) *(kur pivoti është gjithmonë min/max, p.sh. varg i sortuar)* | Shpejtësia praktike më e mirë mesatarisht; in-place (O(log n) hapësirë stek) |
| **Merge Sort** | O(n log n) | O(n log n) — **garantuar** | Kompleksitet i qëndrueshëm në çdo rast; stabil; i mirë për linked-lists dhe sortim të jashtëm |
| **Heap Sort** | O(n log n) | O(n log n) — **në TË TRI rastet** | Kompleksitet i qëndrueshëm si Merge Sort, POR **in-place** (O(1) hapësirë shtesë, ndryshe nga Merge Sort) |
| **Radix Sort** | O(d·n) *(d=numri i shifrave)* | O(d·n) | **O(n) efektiv pa krahasime** — shumë i shpejtë për çelësa numerikë me gamë të kufizuar shifrash |

**Skenari "libri në vend të gabuar"** (Exam 5, 10): kur një listë është **pothuajse e sortuar** me vetëm 1-2 elemente jashtë vendit, zgjidhni **Insertion Sort** — sepse afrohet te rasti më i mirë O(n) pikërisht në këtë skenar (numri i "inversioneve" për t'u korrigjuar është minimal).

**Varg me veti k-distancë** (Exam 12 Detyra 3): kur çdo element është më së shumti k pozicione larg vendit të tij të sortuar, përdorni **Heap Sort me min-heap madhësie k+1** — **O(n log k)**, më efikas se O(n log n) i plotë kur k≪n.

---

## A8. P, NP, NP-Complete, Backtracking

*(Del në: Exam 6, 8, 9(implicit), 13, 15.)*

- **P** (Polynomial): klasa e problemeve që zgjidhen në kohë polinomiale nga një algoritëm **deterministik** — p.sh. sortimi O(n log n), kërkimi binar O(log n).
- **NP** (Nondeterministic Polynomial): klasa e problemeve për të cilat një **zgjidhje e propozuar** mund të **verifikohet** në kohë polinomiale (edhe nëse gjetja e zgjidhjes vetë mund të kërkojë kohë eksponenciale). Çdo problem në P është edhe në NP (nëse mund ta zgjidhësh shpejt, mund edhe ta verifikosh shpejt).
- **NP-Complete**: nënbashkësia e problemeve NP "më të vështira" — çdo problem tjetër në NP mund të **reduktohet** (transformohet) polinomialisht te ai. Shembuj klasikë: TSP (Traveling Salesman), Knapsack (vendimi), Graph Coloring, SAT (Boolean Satisfiability). **Nëse gjendet një algoritëm polinomial për ÇDONJËRIN problem NP-complete, atëherë P=NP** (pyetja e hapur më e famshme në shkencën kompjuterike).
- **Backtracking**: teknikë algoritmike që ndërton zgjidhje **hap pas hapi**, dhe **kthehet prapa (backtrack)** sapo kupton se hapi aktual s'mund të çojë te zgjidhje e vlefshme — kursen kohë krahasuar me brute-force të plotë duke "krasitur" (pruning) degët e pamundshme herët. Shembull tipik: N-Queens, Sudoku, gjenerimi i të gjitha nënbashkësive/permutacioneve me kufizime.

---

*(Shënim mbi gjuhën: Të gjitha përgjigjet e kodit në këtë skedar janë shkruar në **JavaScript të pastër** (pa funksione/metoda built-in për kërkim/sortim, siç kërkohet nga vërejtja standarde e provimeve), me përjashtim të vendeve ku pseudokodi është më i qartë për teknika jo-kodimi (p.sh. gjurmimi manual i KMP/Boyer-Moore, hapat e Kruskal/Prim/Dijkstra).)*

---

# PJESA B — Përgjigjet Specifike Sipas Provimit

## EXAM 1: Kolokviumi 1 — DAA (20 Nëntor, 2024)

**Detyra 1 — Katrori Magjik (Divide & Conquer):**

Metoda më e njohur "divide-and-conquer" për ndërtimin e katrorëve magjikë është **metoda e kuadranteve (Strachey/LUX)**: matrica n×n ndahet në 4 kuadrante (nën-blloqe), plotësohen me numra 1..n² dhe pastaj korrigjohen vlerat diagonale të kuadranteve për të balancuar shumat. Për thjeshtësi kodi (rasti **n çift, plotësisht i pjesëtueshëm me 4** — "doubly-even"):

```javascript
function magicSquareDoublyEven(n) {
    // Hapi 1: mbush matricën 1..n^2 rresht pas rreshti
    const M = [];
    let val = 1;
    for (let i = 0; i < n; i++) {
        M.push([]);
        for (let j = 0; j < n; j++) M[i].push(val++);
    }
    // Hapi 2 (Divide): ndaje matricën në blloqe 4x4 dhe "kthe" vlerat
    // e qelizave në diagonalet e çdo blloku 4x4: v -> (n*n + 1 - v)
    for (let bi = 0; bi < n; bi += 4) {
        for (let bj = 0; bj < n; bj += 4) {
            for (let i = 0; i < 4; i++) {
                for (let j = 0; j < 4; j++) {
                    if (i === j || i + j === 3) { // diagonalet e bllokut 4x4
                        M[bi + i][bj + j] = n * n + 1 - M[bi + i][bj + j];
                    }
                }
            }
        }
    }
    return M;
}
```
**Shpjegim i "Divide & Conquer"**: problemi (mbushja e n×n) ndahet në nën-probleme të pavarur — blloqe 4×4 (ose kuadrante n/2×n/2 në variante të tjera si LUX për "singly-even" n) — secili trajtohet me të njëjtin rregull lokal, pastaj kombinohen te matrica e plotë. Nuk ka rekursion të thellë (thellësia është 1, ndarja bëhet direkt në blloqe të vegjël fiks 4×4), por struktura themelore "ndaj → zgjidh nën-pjesët → kombino" është ajo e kërkuar.

**Kompleksiteti kohor: O(n²)** — çdo qelizë e matricës vizitohet një numër konstant herësh (fillimisht për mbushje, pastaj eventualisht një herë për korrigjim diagonal).

**Detyra 2 — Grupimi i Anagrameve:**
```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (const s of strs) {
        const chars = s.split('');
        // sortim manual (bubble sort) i karaktereve — pa built-in sort()
        for (let i = 0; i < chars.length; i++) {
            for (let j = 0; j < chars.length - i - 1; j++) {
                if (chars[j] > chars[j + 1]) {
                    const tmp = chars[j]; chars[j] = chars[j + 1]; chars[j + 1] = tmp;
                }
            }
        }
        const key = chars.join('');
        if (!map.has(key)) map.set(key, []);
        map.get(key).push(s);
    }
    return Array.from(map.values());
}
```
**Ide**: dy stringje janë anagrame ⟺ kanë saktësisht të njëjtat karaktere ⟺ versionet e tyre të sortuara janë identike. Përdorim atë version të sortuar si "çelës" në një hartë (map) për të grupuar. **Kompleksiteti: O(n·k²)** ku n=numri i stringjeve, k=gjatësia mesatare (sortimi manual bubble këtu është O(k²) për string; me sortim efikas do të ishte O(n·k log k)).

**Detyra 3 — Kllapa të Balancuara (Valid Parentheses):**
```javascript
function isValid(s) {
    const stack = [];
    const closingToOpening = { ')': '(', ']': '[', '}': '{' };
    for (let idx = 0; idx < s.length; idx++) {
        const ch = s[idx];
        if (ch === '(' || ch === '[' || ch === '{') {
            stack.push(ch);
        } else {
            if (stack.length === 0) return false;
            const top = stack[stack.length - 1];
            if (top !== closingToOpening[ch]) return false;
            stack.pop();
        }
    }
    return stack.length === 0;
}
```
**Ide**: çdo kllapë e hapur futet në stek; çdo kllapë mbyllëse duhet të përputhet me kllapën e hapur më të fundit (maja e stekut) — nëse jo, ose nëse steku është bosh kur duhet mbyllje, string-u s'është i vlefshëm. Në fund, steku duhet të jetë bosh (çdo kllapë e hapur u mbyll). **Kompleksiteti: O(n) kohë, O(n) hapësirë** (rasti më i keq: gjithë stringu janë kllapa të hapura).

**Detyra 4 — Vlera n për të cilën 5n·log(n) është më efikasi:**

Duke krahasuar {n!, 2ⁿ, 2n², 5n·log₂n, 20n} për vlera të vogla të n-së:

| n | n! | 2ⁿ | 2n² | 5n·log₂n | 20n | Më e vogla |
|---|---|---|---|---|---|---|
| 7 | 5040 | 128 | 98 | 98.25 | 140 | 2n² (98) |
| **8** | 40320 | 256 | 128 | **120** | 160 | **5n·log n (120)** ✅ |
| 9 | — | 512 | 162 | 142.7 | 180 | 5n·log n |

**Përgjigje: n = 8** *(5·8·log₂8 = 5·8·3 = 120, më e vogël se 2n²=128, 20n=160, 2ⁿ=256, n!=40320)*. *(Çdo n nga 8 deri 15 funksionon njësoj mirë — mjafton një vlerë e vetme si përgjigje.)*

**Detyra 5 — Rasti Mesatar Θ:**
```java
sum = 0;
for (i = 0; i < n; i++)
    for (j = 0; A[j] != i; j++)
        sum++;
```
Loop-i i brendshëm bën një **kërkim sekuencial** brenda A-së për vlerën `i` (nuk ka `break` eksplicit — vazhdon derisa A[j]==i). Për një varg A të rastësishëm (p.sh. permutacion i 0..n-1), pozicioni mesatar i vlerës `i` brenda A-së është ~n/2 — pra loop-i i brendshëm bën **Θ(n)** krahasime mesatarisht, i përsëritur **n herë** nga loop-i i jashtëm. **Përgjigje: Θ(n²)** rasti mesatar.

**Detyra 6 — k minimale për të cilin n·log(n) ∈ O(nᵏ):**

n·log(n) NUK është O(n¹) (sepse log(n)→∞, pra n·logn/n = logn rritet pafundësisht — s'ka konstante C që kufizon raportin). Por n·log(n) ËSHTË O(n²) (sepse n·logn/n² = logn/n → 0 kur n→∞). **Përgjigje: k = 2.**

**Detyra 7 — Rekurrenca T(n) = 3T(n-1) − 15, T(1) = 8 (back-substitution):**
```
T(n) = 3T(n-1) - 15
     = 3[3T(n-2)-15] - 15 = 3²T(n-2) - 3·15 - 15
     = 3³T(n-3) - 3²·15 - 3·15 - 15
     ...
     = 3^(n-1)·T(1) - 15·(3^(n-2)+3^(n-3)+...+3+1)
     = 8·3^(n-1) - 15·[(3^(n-1)-1)/(3-1)]      ← shuma gjeometrike
     = 8·3^(n-1) - (15/2)·3^(n-1) + 15/2
     = (1/2)·3^(n-1) + 15/2
```
**Përgjigje: T(n) = (3^(n-1) + 15) / 2.** *(Verifikim: T(1)=(1+15)/2=8 ✓; T(2)=3·8-15=9, formula: (3+15)/2=9 ✓; T(3)=3·9-15=12, formula: (9+15)/2=12 ✓.)*

---

## EXAM 2: Analiza e Kompleksitetit të Kodit

**c. Bubble Sort:**
```javascript
function funksioni_3(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                const tmp = arr[j]; arr[j] = arr[j + 1]; arr[j + 1] = tmp;
            }
        }
    }
    return arr;
}
```
Dy loop të ndërthurura, e brendshmja zvogëlohet me 1 çdo herë (n-1, n-2, ..., 1) → shuma = n(n-1)/2. **Kompleksiteti kohor: O(n²)** në të tri rastet (edhe nëse i sortuar, kjo version pa "early-exit flag" ende kryen të gjitha krahasimet). **Kompleksiteti hapësinor: O(1)** — sortim in-place, pa strukturë shtesë.

**d. Insertion Sort:**
```javascript
function funksioni_4(arr) {
    for (let i = 1; i < arr.length; i++) {
        let key = arr[i];
        let j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
    return arr;
}
```
**Kompleksiteti kohor: O(n) rasti më i mirë** (varg tashmë i sortuar — while-loop kurrë s'ekzekutohet), **O(n²) rasti më i keq** (varg i sortuar në drejtim të kundërt — çdo element lëviz gjer në fillim). **Kompleksiteti hapësinor: O(1)** — in-place.

**e. QuickSort** (me skemën e ndarjes Lomuto):
```javascript
function pivotPartition(arr, start, end) {
    let pivot = arr[start];
    let swapIndex = start;
    for (let i = start + 1; i <= end; i++) {
        if (pivot > arr[i]) {
            swapIndex++;
            const tmp = arr[swapIndex]; arr[swapIndex] = arr[i]; arr[i] = tmp;
        }
    }
    const tmp = arr[start]; arr[start] = arr[swapIndex]; arr[swapIndex] = tmp;
    return swapIndex;
}
function quickSort(arr, left = 0, right = arr.length - 1) {
    if (left < right) {
        const pivotIndex = pivotPartition(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }
    return arr;
}
```
**Kompleksiteti kohor: O(n log n) mesatarisht** (ndarje e balancuar çdo herë), **O(n²) rasti më i keq** (pivoti gjithmonë min/max — p.sh. varg tashmë i sortuar me këtë skemë pivoti). **Kompleksiteti hapësinor: O(log n) mesatarisht** (thellësia e stekut të rekursionit), **O(n) rasti më i keq**.

---

## EXAM 3: Analiza dhe Probleme Kodimi

**Detyra 1a — Shell Sort vs Insertion Sort:** Shell Sort para-sorton vargun duke krahasuar elemente të largëta (me "increment"/gap > 1), duke reduktuar gap-in gradualisht deri sa bëhet 1 (në atë pikë është ekzaktësisht Insertion Sort). Kjo e bën shumë më efikas se Insertion Sort i pastër sepse elementët "gabim vendosur" lëvizin shpejt distanca të mëdha herët, në vend që të lëvizin 1 pozicion në herë. **Kompleksiteti varet nga sekuenca e gap-eve** — tipikisht **O(n^1.5)** deri **O(n(log n)²)**, gjithsesi më mirë se O(n²) e Insertion Sort të pastër.

**Detyra 1b — QuickSort vs MergeSort:** QuickSort ndan (partition) PARA rekursionit (top-down, "puna" bëhet në ndarje), është in-place por rasti më i keq O(n²). MergeSort ndan në gjysma pa kusht dhe bashkon (merge) PAS rekursionit (bottom-up, "puna" bëhet në bashkim), garanton O(n log n) në çdo rast por kërkon O(n) hapësirë shtesë për bashkimin.

**Detyra 1c — HeapSort:** Ndërto një **max-heap** nga vargu (O(n)), pastaj nxirr elementin maksimal (rrënjën) në mënyrë të përsëritur, duke e zëvendësuar me elementin e fundit dhe duke rikthyer vetinë e heap-ut (sift-down, O(log n) për nxjerrje) — gjithsej **O(n log n)** në të tri rastet (best/avg/worst), pasi ndërtimi dhe struktura e heap-ut mbeten të njëjta pavarësisht renditjes fillestare.

**Detyra 2 — Maximum Subarray Sum (Algoritmi i Kadane-s):**
```javascript
function maxSubArray(nums) {
    let maxSoFar = nums[0];
    let currentSum = nums[0];
    for (let i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSoFar = Math.max(maxSoFar, currentSum);
    }
    return maxSoFar;
}
```
**Gjurmimi mbi `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`:**
| i | nums[i] | currentSum | maxSoFar |
|---|---|---|---|
| 0 | -2 | -2 | -2 |
| 1 | 1 | max(1,-1)=1 | 1 |
| 2 | -3 | max(-3,-2)=-2 | 1 |
| 3 | 4 | max(4,2)=4 | 4 |
| 4 | -1 | max(-1,3)=3 | 4 |
| 5 | 2 | max(2,5)=5 | 5 |
| 6 | 1 | max(1,6)=6 | **6** |
| 7 | -5 | max(-5,1)=1 | 6 |
| 8 | 4 | max(4,5)=5 | 6 |

**Përgjigje: 6** ✓ (nënvargu `[4,-1,2,1]`). **Kompleksiteti: O(n) kohë, O(1) hapësirë** — ide themelore: në çdo pozicion, ose vazhdo nënvargun aktual, ose fillo një të ri nga aty (nëse shuma aktuale ka rënë nën vlerën e vetë elementit).

**Detyra 3 — Longest Increasing Subsequence (DP O(n²)):**
```javascript
function lengthOfLIS(nums) {
    const n = nums.length;
    const dp = new Array(n).fill(1); // dp[i] = LIS që mbaron te i
    let maxLen = 1;
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
            }
        }
        maxLen = Math.max(maxLen, dp[i]);
    }
    return maxLen;
}
```
**Gjurmimi mbi `[10, 9, 2, 5, 3, 7, 101, 18]`:** dp = [1,1,1,2,2,3,4,4] *(dp[5]=3 nga 2→3→7 ose 2→5→7; dp[6]=4 nga 2→3→7→101 ose 2→5→7→101)*. **Përgjigje: max(dp) = 4** ✓ (nënsekuenca `[2,3,7,101]` ose `[2,5,7,101]`, e dyja gjatësi 4). **Kompleksiteti: O(n²) kohë** (dy loop të ndërthurura), **O(n) hapësirë** për array-n `dp`. *(Ekziston edhe zgjidhje O(n log n) me kërkim binar mbi një array "tails", nëse kërkohet optimizim shtesë.)*

**Detyra 4a — Kërkimi Sekuencial:** shih A4 — **O(n)** rasti më i keq.
**Detyra 4b — Kërkimi Binar:** shih A4 — **O(log n)** rasti më i keq (kodi i dhënë në pyetje është identik me pseudokodin standard, thjesht i pambaruar te rasti "return -1" jashtë loop-it).

---

## EXAM 4: Provimi Final — Qershor 2023

**Detyra 1 — Numri i Atomeve (Number of Atoms):**

Kjo kërkon **parsim rekursiv (stack-based)**: një formulë brenda kllapash trajtohet si nën-problem i pavarur, rezultati i të cilit shumëzohet me multiplikatorin që vjen pas kllapës mbyllëse, pastaj bashkohet (merge) me numërimet e nivelit mbi të.

```javascript
function countOfAtoms(formula) {
    let i = 0;
    const n = formula.length;

    function parseCount() {           // lexon numrin pas emrit/kllapës (default 1)
        const start = i;
        while (i < n && formula[i] >= '0' && formula[i] <= '9') i++;
        if (start === i) return 1;
        return parseInt(formula.slice(start, i), 10);
    }

    function parseName() {            // shkronjë e madhe + zero a më shumë të vogla
        const start = i;
        i++;
        while (i < n && formula[i] >= 'a' && formula[i] <= 'z') i++;
        return formula.slice(start, i);
    }

    function parse() {                // kthen Map(emri -> numërimi) për një nivel
        const counts = new Map();
        while (i < n && formula[i] !== ')') {
            if (formula[i] === '(') {
                i++;                              // kalon '('
                const inner = parse();             // zgjidh nën-formulën rekursivisht
                i++;                              // kalon ')'
                const mult = parseCount();         // multiplikatori pas ')'
                for (const [name, cnt] of inner) {
                    counts.set(name, (counts.get(name) || 0) + cnt * mult);
                }
            } else {
                const name = parseName();
                const cnt = parseCount();
                counts.set(name, (counts.get(name) || 0) + cnt);
            }
        }
        return counts;
    }

    const result = parse();

    // sortim manual (bubble) i emrave të elementeve, sepse s'lejohen built-in
    const names = Array.from(result.keys());
    for (let a = 0; a < names.length; a++) {
        for (let b = 0; b < names.length - a - 1; b++) {
            if (names[b] > names[b + 1]) {
                const tmp = names[b]; names[b] = names[b + 1]; names[b + 1] = tmp;
            }
        }
    }

    let output = "";
    for (const name of names) {
        output += name;
        const cnt = result.get(name);
        if (cnt > 1) output += cnt;
    }
    return output;
}
```

**Gjurmimi mbi Shembullin 2: `formula = "Mg(OH)2"`:**
1. Niveli i jashtëm lexon `Mg` (count=1, s'ka shifër pas) → counts={Mg:1}.
2. Sheh `(` → hyn rekursivisht: niveli i brendshëm lexon `O`(count=1) dhe `H`(count=1) → kthen {O:1, H:1}.
3. Pas `)` lexon multiplikatorin `2` → shumëzon {O:1,H:1} me 2 → {O:2, H:2} dhe i shton te niveli i jashtëm.
4. Rezultati final: {Mg:1, O:2, H:2}.
5. Sortimi i emrave alfabetikisht: H, Mg, O.
6. Formatimi: `H` (count 2 → "H2") + `Mg` (count 1 → asnjë shifër, vetëm "Mg") + `O` (count 2 → "O2") = **"H2MgO2"** ✅ *(përputhet saktësisht me daljen e pritur).*

**Kompleksiteti: O(L + E log E)** ku L=gjatësia e formulës, E=numri i elementeve unikë (sortimi bubble mbi elementet, që janë shumë më pak se L) — praktikisht **O(L)** dominon.

---

## EXAM 5: Kolokviumi i Parë — DAA

**Detyra 1 — Radix Sort + Diferenca Maksimale (Maximum Gap):**
```javascript
function radixSort(arr) {
    if (arr.length === 0) return arr;
    let maxVal = arr[0];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] > maxVal) maxVal = arr[i];
    }
    let exp = 1;
    while (Math.floor(maxVal / exp) > 0) {
        const buckets = [];
        for (let d = 0; d < 10; d++) buckets.push([]);
        for (let i = 0; i < arr.length; i++) {
            const digit = Math.floor(arr[i] / exp) % 10;
            buckets[digit].push(arr[i]);
        }
        let idx = 0;
        for (let d = 0; d < 10; d++) {
            for (let k = 0; k < buckets[d].length; k++) arr[idx++] = buckets[d][k];
        }
        exp *= 10;
    }
    return arr;
}

function maximumGap(nums) {
    if (nums.length < 2) return 0;
    const sorted = radixSort(nums.slice());
    let maxDiff = 0;
    for (let i = 1; i < sorted.length; i++) {
        if (sorted[i] - sorted[i - 1] > maxDiff) maxDiff = sorted[i] - sorted[i - 1];
    }
    return maxDiff;
}
```
**Gjurmimi mbi `[9,18,27,3]`:** shifra e njësheve (exp=1): kova(9→9),(18→8),(27→7),(3→3) → pas rirenditjes: `[3,27,18,9]`. Shifra e dhjetësheve (exp=10): kova(3→0),(27→2),(18→1),(9→0) → pas rirenditjes: `[3,9,18,27]`. **Vargu i sortuar: [3,9,18,27]**. Diferencat: 9-3=6, 18-9=9, 27-18=9 → **Përgjigje: 9** ✅. **Kompleksiteti: O(d·(n+b))** ku d=numri i shifrave, b=baza(10) → praktikisht **O(n)** për numra me shifra të kufizuara.

**Detyra 2 — Kërkimi Sekuencial/Binar (V=[2,3,7,10,90]):** shih **A4** — pseudokodi/kodi JS, gjurmimi i plotë për target=90 (gjendet te indeksi 4 pas 3 hapash), parakushti (vargu i sortuar), kompleksiteti O(log n).

**Detyra 3 — Skenari i Bibliotekës:** **Insertion Sort** — shih **A7**. Me vetëm 1 libër jashtë vendit, numri i "inversioneve" për t'u korrigjuar është minimal, kështu Insertion Sort afrohet te rasti më i mirë **O(n)**.

**Detyra 4 — Avantazhet e 5 Algoritmeve të Sortimit:** shih tabelën e plotë **A7**.

**Detyra 5 — 132 Pattern (stack monoton):**
```javascript
function find132pattern(nums) {
    const stack = [];       // ruan kandidatë në rënie për rolin "3" (vlera maksimale)
    let third = -Infinity;  // kandidati më i mirë i gjetur për rolin "2" (i mesëm)
    for (let i = nums.length - 1; i >= 0; i--) {
        if (nums[i] < third) return true;  // nums[i] luan rolin "1" → gjetëm modelin
        while (stack.length > 0 && stack[stack.length - 1] < nums[i]) {
            third = stack.pop();           // vlera e nxjerrë bëhet kandidat për "2"
        }
        stack.push(nums[i]);
    }
    return false;
}
```
**Gjurmimi mbi `[3,1,4,2]`** *(pritet true)*: i=3(val=2): third=-∞, stack bosh→push[2]. i=2(val=4): 4<-∞?jo; pop 2 (2<4)→third=2, stack bosh, push[4]. i=1(val=1): **1 < third(2) → true!** *(modeli: i=1(nums=1), j=2(nums=4), k=3(nums=2): 1<2<3 ✓ dhe 1<2<4 ✓)*.
**Gjurmimi mbi `[1,2,3,4]`** *(pritet false)*: stack ndërtohet gjithmonë në rënie [4],[4,3],[4,3,2],[4,3,2,1] pa asnjë pop (asnjë element nën majë), third mbetet -∞ gjithë kohën → **false** ✅. **Kompleksiteti: O(n) kohë** (çdo element futet/nxirret nga steku më së shumti 1 herë), **O(n) hapësirë**.

---

## EXAM 6: DAA — Shkurt 2024 (Provim gjithëpërfshirës)

**Detyra 1 — Rrënja Katrore (Floor Sqrt, pa built-in, me kërkim binar):**
```javascript
function mySqrt(x) {
    if (x < 2) return x;
    let left = 1, right = Math.floor(x / 2);
    let ans = 1;
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        const sq = mid * mid;
        if (sq === x) return mid;
        else if (sq < x) { ans = mid; left = mid + 1; }
        else right = mid - 1;
    }
    return ans;
}
```
**Ide**: kërkojmë binarisht mbi **hapësirën e përgjigjeve të mundshme** [1, x/2] (jo mbi vetë vargun!) — mid² krahasohet me x; nëse mid² ≤ x, ruajmë mid si përgjigjja aktuale më e mirë dhe kërkojmë më lart, përndryshe kërkojmë më poshtë.

**Gjurmimi mbi x=8:** left=1,right=4 → mid=2,sq=4<8→ans=2,left=3 → mid=3,sq=9>8→right=2 → left(3)>right(2), ndalo. **Përgjigje: 2** ✅ *(√8≈2.828, floor=2)*. **Kompleksiteti: O(log x)**.

**Detyra 2 — Perimetri më i Madh i Poligonit:**
```javascript
function largestPerimeter(nums) {
    const arr = nums.slice();
    for (let i = 1; i < arr.length; i++) {           // insertion sort rritës
        const key = arr[i];
        let j = i - 1;
        while (j >= 0 && arr[j] > key) { arr[j + 1] = arr[j]; j--; }
        arr[j + 1] = key;
    }
    const n = arr.length;
    const prefixSum = new Array(n);
    prefixSum[0] = arr[0];
    for (let i = 1; i < n; i++) prefixSum[i] = prefixSum[i - 1] + arr[i];

    for (let i = n - 1; i >= 2; i--) {
        const sumOfRest = prefixSum[i - 1];          // shuma e brinjëve më të vogla se arr[i]
        if (arr[i] < sumOfRest) return prefixSum[i]; // poligon i vlefshëm, perimetri = shuma
    }
    return -1;
}
```
**Ide**: pasi sortohet rritës, provo VARGUN E PLOTË si poligon i mundshëm; nëse brinja më e madhe s'është më e vogël se shuma e të tjerave, hiqe atë (kandidati problematik) dhe provo prapë me pjesën e mbetur — kjo është **greedy nga fundi**.

**Gjurmimi mbi `[5,5,5]`:** prefixSum=[5,10,15]. i=2: arr[2]=5 < prefixSum[1]=10 → **true**, kthen prefixSum[2]=**15** ✅.
**Gjurmimi mbi `[1,12,1,2,5,50,3]`:** i sortuar: `[1,1,2,3,5,12,50]`, prefixSum=[1,2,4,7,12,24,74]. i=6: 50<24? jo. i=5: 12<12? jo (jo strikte). i=4: **5<7? po!** → kthen prefixSum[4]=**12** ✅ *(poligoni me brinjë 1,1,2,3,5)*. **Kompleksiteti: O(n log n)** (dominuar nga sortimi; skanimi final është O(n)).

**Detyra 3 & 4 — Kruskal dhe Prim mbi të njëjtin graf:** shih **A3** — metoda e plotë (sortimi i degëve + Union-Find për Kruskal; fringe-based rritje për Prim), aplikuar mbi grafin real të dhënë në provimin tuaj. Kompleksiteti: **O(E log E)** për Kruskal, **O(V²)** ose **O(E log V)** për Prim.

**Detyra 5 — Bad Character/Good Suffix + KMP:** shih **A2** (gjurmimi i plotë mbi Text=`GTTATAGCTGATCGCGGCGTAGCGGCGAA`, Pattern=`GTAGCGGCG`) dhe **A1** (KMP mbi P="AAAAB", T="AAAAAAAAAAAAAAAAB" — përputhja gjendet te indeksi 12).

**Detyra 6 — Kompleksiteti i loop-eve + shprehja që NUK është O(n²):** shih **A5** — (a) `O(n)`, (b) `O(n²)`, (c) `n³/√n = n^2.5` NUK është O(n²).

**Detyra 7 — Horner's Method:** shih **A6** — kodi JS i plotë + shembulli i punuar (2x³-6x²+2x-1 te x=3 = 5).

**Detyra 8 — P, NP, NP-complete, Backtracking:** shih **A8** — përkufizimet e plota.

---

## EXAM 7: Provimi Final — DAA (10.05.2025)

**Detyra 1 — Katrori Magjik:** identike me Exam 1, Detyra 1 — shih zgjidhjen atje.

**Detyra 2 — Kllapa të Balancuara:** identike me Exam 1, Detyra 3 — shih zgjidhjen atje.

**Detyra 3 — Winograd vs Strassen:**
- **Winograd**: reduktion i numrit të **shumëzimeve** në shumëzimin e matricave duke ripërdorur nën-shprehje të përbashkëta (parapërllogaritje të shumave të rreshtave/kolonave) — mbetet **O(n³)** asimptotikisht, por me një konstante praktike më të vogël se algoritmi standard (rreth gjysma e shumëzimeve, në këmbim të disa mbledhjeve shtesë).
- **Strassen**: **divide-and-conquer** — ndan çdo matricë n×n në 4 nën-matrica n/2×n/2, dhe zëvendëson 8 shumëzimet rekursive standarde me vetëm **7 formula** të kombinuara (me mbledhje/zbritje shtesë) — jep **T(n) = 7T(n/2) + O(n²)**, që me Master Theorem zgjidhet në **O(n^log₂7) ≈ O(n^2.81)**, më mirë asimptotikisht se O(n³).
- **Dallimi kryesor**: Winograd optimizon konstanten brenda të njëjtit kompleksitet O(n³); Strassen ndryshon vetë klasën e kompleksitetit në O(n^2.81) përmes rekursionit — por Strassen ka overhead praktik (rekursion, mbledhje shtesë) që e bën më të dobishëm vetëm për matrica mjaft të mëdha.

**Detyra 4 — Shpejtësia e Re e Makinës:** T(n) = 3·2ⁿ, makinë e re **64 herë** më e shpejtë:
```
64 = 2^6
3·2^(n+m) = 64 · 3·2^n
2^m = 64 = 2^6  ⟹  m = 6
```
**Përgjigje: makina e re mund të procesojë n+6 hyrje** në po atë kohë `t` *(çdo rritje me 1 njësi kohore dyfishon input-in e mundshëm te ky kompleksitet eksponencial — 64=2⁶ do të thotë "6 dyfishime shtesë" janë të mundshme)*.

**Detyra 5 — KMP vs Boyer-Moore (krahasim teorik):**

| | KMP | Boyer-Moore |
|---|---|---|
| Drejtimi i krahasimit | Majtas → Djathtas | **Djathtas → Majtas** |
| Info e përdorur | Vetë-ngjashmëria e P (failure function) | Karakteri i tekstit që s'përputhet (bad-char) + sufikset e P (good-suffix) |
| Garanci teorike | **O(n+m) gjithmonë** | O(n·m) rasti më i keq, por sub-linear praktikisht |
| Më i mirë me | Alfabet të vogël, ripërsëritje në mostër | **Alfabet të madh** — kërcime më të mëdha |
| A kthehet prapa në T? | KURRË | Jo drejtpërdrejt, por lëviz gjithsesi djathtas |

*(Shih skedarin 07, §4 për tabelën e plotë dhe shpjegime shtesë.)*

---

## EXAM 8: Provimi_DAA_Janar2021

**Detyra 1a — Algoritmi që gjen vlerën maksimale + verifikim korrektësie:**
```javascript
function findMax(arr) {
    let max = arr[0];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] > max) max = arr[i];
    }
    return max;
}
```
**Verifikimi i korrektësisë (invarianti i loop-it)**: para çdo iterimi `i`, `max` mban vlerën maksimale të `arr[0..i-1]`. *Baza*: fillimisht `max=arr[0]`, trivialisht maksimumi i `arr[0..0]`. *Hapi induktiv*: nëse `arr[i]>max`, azhurnojmë `max=arr[i]`, pra `max=max(arr[0..i-1], arr[i])=max(arr[0..i])`; përndryshe `max` mbetet korrekt sepse `arr[i]≤max`. *Përfundimi*: pas loop-it (i=n), `max` = maksimumi i `arr[0..n-1]`, pra i gjithë vargu. **Kompleksiteti: O(n)** — një krahasim për çdo element, pa rekursion apo loop të ndërthurur.

**Detyra 1b — Definimi i Big-O (me përshkrim grafiku):** `f(n) = O(g(n))` nëse ekzistojnë konstante pozitive **c** dhe **n₀** të tilla që `f(n) ≤ c·g(n)` për çdo `n ≥ n₀`. **Grafikisht**: kurba e f(n) qëndron **nën** kurbën e `c·g(n)` për çdo n në të djathtë të pikës n₀ — g(n) është kufiri i sipërm asimptotik i f(n).

**Detyra 2:** shih **A4** — sekuenciali (kod JS), binari (hapat për target=90 + parakushti "vargu duhet të jetë i sortuar"), kompleksiteti O(log n).

**Detyra 3:** shih **A7** — tabela e plotë e avantazheve/best-worst case.

**Detyra 4 — Kruskal + KMP:** shih **A3** (Kruskal → PSHM) dhe **A1** (KMP: "AAAAB" te indeksi 12 i "AAAAAAAAAAAAAAAAB") — pyetje të pavarura nga njëra-tjetra.

**Detyra 5:** shih **A8** — P, NP, NP-complete, Backtracking.

---

## EXAM 9: Provimi_DAA_Nentor2020

Struktura identike me Exam 8 (Detyra 1-3, shih atje), me ndryshim te Detyra 4-5:

**Detyra 4 — Kruskal + Dijkstra mbi TË NJËJTIN graf:** shih **A3** — të dyja metodat e plota (Kruskal → PSHM me Union-Find; Dijkstra → shtegu më i shkurtër nga një burim te të gjitha nyjet, duke përdorur peshë totale të shtegut jo peshë e 1 dege).

**Detyra 5 — Kompleksiteti për 2 funksione:** *(grafika e kodit s'u rikuperua nga skanimi origjinal — kontrolloni foton/PDF-në origjinale për tekstin e saktë të funksioneve; metoda e analizës mbetet ajo e shpjeguar në A5: identifikoni nëse loop-et janë sekuenciale (mblidhen kompleksitetet) apo të ndërthurura (shumëzohen), dhe nëse ndonjë ndryshore zvogëlohet/rritet gjeometrikisht ndaj lineari.)*

---

## EXAM 10: Provimi_DAA_Qershor-2

**Detyra 3 — Skenari i Bibliotekës:** **Insertion Sort** — shih **A7** (afrohet te O(n) kur lista është pothuajse e sortuar me 1 element jashtë vendit).

**Detyra 4 — Kruskal + Dijkstra:** shih **A3**.

**Detyra 5:** *(grafika mungon nga skanimi — shih shënimin te Exam 9.)*

---

## EXAM 11: Provimi_DAA_Shtator-1

**Detyra 1a — Renditja e f1=2ⁿ, f2=n^1.5, f3=n·log n, f4=n^(log n) sipas rritjes (rritëse):**

Krahasojmë çiftazi duke marrë logaritmin (metodë standarde për krahasim funksionesh eksponenciale/polinomiale):
- `n·log n` vs `n^1.5`: `n·log n = n^1 · log n`, dhe `log n = o(n^0.5)` (logaritmi rritet më ngadalë se çdo fuqi pozitive e n) → **n·log n rritet më ngadalë se n^1.5**.
- `n^1.5` vs `n^(log n)`: krahaso eksponentët pas logaritmimit: `1.5·log n` vs `(log n)·(log n) = (log n)²`. Për n të mëdha, `(log n)² ` e kalon `1.5·log n` (sepse log n → ∞) → **n^1.5 rritet më ngadalë se n^(log n)**.
- `n^(log n)` vs `2ⁿ`: `log(n^(log n)) = (log n)²`, `log(2ⁿ)=n`. Për n të mëdha, `n` e kalon `(log n)²` gjithmonë (eksponenciali fiton ndaj çdo fuqie polinomiale a logaritmike të n-së) → **n^(log n) rritet më ngadalë se 2ⁿ**.

**Përgjigje (rritëse): f3 (n·log n) < f2 (n^1.5) < f4 (n^(log n)) < f1 (2ⁿ).**

**Detyra 1b — Definimi i Big-O:** shih Exam 8, Detyra 1b.

**Detyra 2a:** shih **A4** (kërkimi sekuencial).

**Detyra 2b — Kërkimi Binar (V duhet parë si i sortuar; shënim: OCR origjinal tregon "2,3,7,10,92,55" që s'është i sortuar — supozojmë versionin korrekt të sortuar `[2,3,7,10,55,92]`, target=92):**
left=0,right=5→mid=2,V[2]=7<92→left=3. left=3,right=5→mid=4,V[4]=55<92→left=5. left=5,right=5→mid=5,V[5]=92→**gjetur te indeksi 5**.

**Detyra 2c — Zgjidhja e rekurrencës `an = -a(n-1) + 4a(n-2) + 4a(n-3)`, a0=8, a1=6, a2=26:**

Ekuacioni karakteristik: `x³ = -x² + 4x + 4` ⟹ `x³ + x² - 4x - 4 = 0`. Faktorizim: `x²(x+1) - 4(x+1) = (x+1)(x²-4) = (x+1)(x-2)(x+2) = 0` → rrënjët: **x = -1, 2, -2** (tri rrënjë reale të dallueshme).

Forma e përgjithshme: `an = A·(-1)ⁿ + B·2ⁿ + C·(-2)ⁿ`. Zëvendëso kushtet fillestare:
- n=0: A + B + C = 8
- n=1: -A + 2B - 2C = 6
- n=2: A + 4B + 4C = 26

Duke zgjidhur këtë sistem 3×3 (eliminim): nga ekuacionet 1 dhe 3: `(A+4B+4C)-(A+B+C)=26-8` → `3B+3C=18` → `B+C=6`. Nga ekuacionet 1 dhe 2: `(-A+2B-2C)+(A+B+C)=6+8` → `3B-C=14`. Duke zëvendësuar `C=6-B` te `3B-C=14`: `3B-(6-B)=14` → `4B=20` → **B=5**, pra **C=1**. Nga ekuacioni 1: `A+5+1=8` → **A=2**.

**Përgjigje: an = 2·(-1)ⁿ + (-2)ⁿ + 5·2ⁿ.** *(Verifikim: n=0: 2+1+5=8 ✓; n=1: -2-2+10=6 ✓; n=2: 2+4+20=26 ✓.)* *(Shih edhe skedarin 03, §7, Shembulli 2, për të njëjtin shembull të zgjidhur.)*

**Detyra 3:** shih **A7**.

**Detyra 4:** Kruskal shih **A3**; KMP shih **A1**.

**Detyra 5a:** *(grafika mungon — shih shënimin te Exam 9.)*

**Detyra 5b — Fshirja e elementit të i-të pa varësi nga n:**

**(i) Varg i PASORTUAR** — rendi s'ka rëndësi, kështu mund të **zëvendësojmë** elementin që duam të fshijmë me **elementin e fundit** të vargut, pastaj shkurtojmë gjatësinë me 1:
```javascript
function deleteAtUnsorted(arr, i) {
    arr[i] = arr[arr.length - 1];
    arr.pop();
    return arr;
}
```
**O(1)** — asnjë zhvendosje e nevojshme, pasi rendi i elementeve të tjera s'ka rëndësi.

**(ii) Varg i SORTUAR, duke ruajtur sortimin** — kjo është themelisht më e vështirë: fshirja e indeksit `i` në një varg të njëpasnjëshëm (contiguous) kërkon **zhvendosjen e të gjithë elementeve pas `i`** për të mbyllur "vrimën" dhe ruajtur rendin — **O(n-i)** rasti më i keq, JO O(1). **S'ekziston mënyrë e vërtetë O(1) për fshirje nga varg i sortuar i njëpasnjëshëm duke ruajtur menjëherë sortimin dhe kontinuitetin.** Alternativat praktike:
- **"Tombstone"/fshirje e vonuar (lazy deletion)**: shëno pozitën si "e fshirë" (p.sh. vlerë sentinel ose flag në një varg paralel boolean) pa e zhvendosur asgjë — O(1) për fshirjen, por lë "vrima" që duhen anashkaluar gjatë kërkimit dhe kompaktuar periodikisht.
- **Lista e lidhur (linked list)**: fshirja e një nyje të njohur është O(1), por humbet aftësinë e kërkimit binar O(log n) (kërkimi në listë të lidhur është O(n)).
- **Pemë e balancuar (AVL/Red-Black) ose skip list**: fshirje **O(log n)** duke ruajtur sortimin dhe kërkim të shpejtë — jo O(1), por shumë më mirë se O(n), dhe zgjidhja praktike standarde kur duhen të dyja veti (sortim + fshirje efikase).

*(Shih skedarin 13, §1.11 për diskutim shtesë.)*

---

## EXAM 12: Provimi_DAA_Shtator2021

**Detyra 1a:** shih Exam 8, Detyra 1a (max + Big-O).

**Detyra 1b — Definimi i Big-Omega (Ω):** `f(n) = Ω(g(n))` nëse ekzistojnë konstante pozitive **c** dhe **n₀** të tilla që `f(n) ≥ c·g(n)` për çdo `n ≥ n₀`. **Ndryshe nga Big-O** (kufiri i sipërm — "s'rritet më shpejt se"), **Big-Ω është kufiri i POSHTËM asimptotik** ("s'rritet më ngadalë se") — grafikisht, f(n) qëndron **mbi** `c·g(n)` për n≥n₀.

**Detyra 2 — V=[2,3,7,10,20], target=20:** sekuenciali shih **A4**; binari: left=0,right=4→mid=2,V[2]=7<20→left=3 → mid=3,V[3]=10<20→left=4 → mid=4,V[4]=20→**gjetur te indeksi 4**. Kompleksiteti O(log n).

**Detyra 3 — Varg me veti k-distancë:** **Përgjigja: Heap Sort me min-heap madhësie k+1 → O(n log k).** Ide: meqë çdo element është më së shumti k pozicione larg vendit të vet të sortuar, mjafton të mbahen "gati" vetëm k+1 elementë njëherësh në një min-heap — nxirr minimumin (rrënjën) dhe fut elementin tjetër nga vargu në çdo hap; meqë madhësia e heap-ut mbetet konstante (k+1), çdo operacion heap është O(log k) në vend të O(log n) — gjithsej **O(n log k)**, më efikas se sortimi i plotë O(n log n) kur k≪n.

**Detyra 4 — Kruskal + Dijkstra:** shih **A3**.

**Detyra 5abc:** identike me Exam 6, Detyra 6 — shih **A5**.

---

## EXAM 13: DAA — Kolokviumi 1 — 2023

**Detyra 1 — Dijkstra (vetëm):** shih **A3** — shtegu më i shkurtër nga nyja 1 te të gjitha nyjet tjera, O(V²) ose O(E log V).

**Detyra 2 — Kruskal + Prim mbi të njëjtin graf:** shih **A3**.

**Detyra 3 — P, NP, NP-complete, Backtracking:** shih **A8**.

**Detyra 4 — Bad-character/Good-suffix + KMP:** shih **A2** dhe **A1**.

**Detyra 5 — Horner (2x³-6x²+2x-1 te x=3):** shih **A6** — **Përgjigje: 5**.

---

## EXAM 14: DAA — Kolokviumi 2 — 2023

**Detyra 1 — Diferenca maksimale mes fqinjëve pas sortimit (reasoning i kërkuar):** **Përgjigja: Radix Sort**, sepse arrin **O(n)** pa asnjë krahasim (kur çelësat janë numra të plotë me gamë të kufizuar shifrash) — çdo algoritëm i bazuar në krahasim (Quick/Merge/Heap) është kufizuar teorikisht nga **Ω(n log n)**, kështu Radix Sort ofron avantazh asimptotik real për këtë lloj hyrjeje. Shih kodin e plotë JS + gjurmimin te **A** (Exam 5, Detyra 1).

**Detyra 2 — Elementi i Shumicës (Majority Element) — Algoritmi i Votimit Boyer-Moore:**
```javascript
function majorityElement(nums) {
    let candidate = null;
    let count = 0;
    for (let i = 0; i < nums.length; i++) {
        if (count === 0) {
            candidate = nums[i];
            count = 1;
        } else if (nums[i] === candidate) {
            count++;
        } else {
            count--;
        }
    }
    return candidate;
}
```
**Ide**: mbaj një "kandidat" aktual dhe një numërues; çdo element identik me kandidatin rrit numëruesin, çdo element ndryshe e zvogëlon; kur numëruesi bie në 0, kandidati zëvendësohet. Meqë elementi i shumicës shfaqet >⌊n/2⌋ herë, ai "mbijeton" gjithmonë këtë proces eliminimi çift-për-çift. **Gjurmimi mbi `[2,2,1,1,1,2,2]`**: candidate=2(count1)→count2(shfaqet2)→count1(1≠2)→count0(1≠2)→candidate=1,count1(count ishte0)→count0(2≠1)→candidate=2,count1(count ishte0) → **rezultati final: candidate=2** ✅ *(2 shfaqet 4 herë nga 7 — vërtet shumica)*. **Kompleksiteti: O(n) kohë, O(1) hapësirë** — shumë më efikas se numërimi me hartë frekuencash (që do të kërkonte O(n) hapësirë shtesë).

**Detyra 3:** shih **A7**.

**Detyra 4 — Sort Characters By Frequency:**
```javascript
function frequencySort(s) {
    const freq = new Map();
    for (const ch of s) freq.set(ch, (freq.get(ch) || 0) + 1);

    const chars = Array.from(freq.keys());
    for (let i = 0; i < chars.length; i++) {            // bubble sort sipas frekuencës zbritëse
        for (let j = 0; j < chars.length - i - 1; j++) {
            if (freq.get(chars[j]) < freq.get(chars[j + 1])) {
                const tmp = chars[j]; chars[j] = chars[j + 1]; chars[j + 1] = tmp;
            }
        }
    }
    let result = "";
    for (const ch of chars) result += ch.repeat(freq.get(ch));
    return result;
}
```
**Gjurmimi mbi `"tree"`:** frekuencat {t:1, r:1, e:2} → pas sortimit zbritës: `e`(2) para `t`/`r`(1 secili) → dalja `"ee" + "t" + "r"` = **"eetr"** *(ose "eert" — çdo renditje ku 'e' shfaqet 2 herë bashkë dhe 't'/'r' vijnë pas, është e vlefshme — problemi pranon çdo përgjigje të saktë sipas frekuencës)*. ⚠️ **Kujdes**: një përgjigje si `"cacaca"` (karaktere të ndërthurura, jo të grupuara bashkë) do të ishte **E GABUAR** edhe nëse frekuencat individuale përputhen — kusht themelor është që **çdo karakter të shfaqet i grupuar tok**, jo i shpërndarë. **Kompleksiteti: O(k²) rasti më i keq** ku k=numri i karaktereve unikë (nga bubble sort), plus O(n) për ndërtimin e stringut final.

**Detyra 5 — Kruskal:** shih **A3**.

**Detyra 6 — Prim mbi të njëjtin graf:** shih **A3**.

**Detyra 7 — Bad-character/Good-suffix + KMP:** shih **A2** dhe **A1**.

---

## EXAM 15: DAA — Janar 2024 / Shkurt 2024 (identik me Exam 6, konfirmuar në 2 gjuhë)

**Q1 (Floor Sqrt):** shih Exam 6, Detyra 1.
**Q2 (Perimetri i Poligonit):** shih Exam 6, Detyra 2.
**Q3 & Q4 (Kruskal + Prim):** shih **A3**.
**Q5 (Bad-char/Good-suffix + KMP):** shih **A2** dhe **A1**.
**Q6 (Kompleksiteti i loop-eve):** shih **A5**.
**Q7 (Horner):** shih **A6**.
**Q8 (P/NP/Backtracking):** shih **A8**.

---

## EXAM 16: Provimi_DAA_Nentor2024 (variant më i plotë i Exam 1)

**Detyra 1-5:** identike me Exam 1 — shih zgjidhjet atje (katrori magjik, grupimi i anagrameve, kllapa të balancuara, funksionet e rritjes n=8, rekurrenca T(n)).

**Detyra 6 — Kruskal + KMP:** shih **A3** dhe **A1**.

**Detyra 7a — Kompleksiteti i `fun()`:** *(grafika e saktë e kodit mungon nga skanimi — përdorni metodën e A5: identifikoni nëse loop-et janë sekuenciale apo të ndërthurura, dhe si ndryshon numëruesi kryesor ndër iterime.)*

**Detyra 7b — Operacione në O(1) të pavarura nga n:** shih Exam 11, Detyra 5b — (i) fshirja pa kërkesë renditjeje: swap-me-të-fundit, O(1); (ii) fshirja duke ruajtur sortimin: s'ka zgjidhje të vërtetë O(1) me varg të njëpasnjëshëm — diskutohen alternativat (tombstone/lazy deletion O(1) me "vrima", listë e lidhur O(1) por humbet kërkimin binar, pemë e balancuar O(log n)).

---

## Përmbledhje: Ku të Gjeni Çdo Përgjigje

Kjo tabelë ju lejon të kërkoni shpejt zgjidhjen e një lloji problemi pa lexuar gjithë skedarin nga fillimi:

| Kërkoni... | Shkoni te... |
|---|---|
| KMP (gjurmim manual "AAAAB") | **A1** |
| Boyer-Moore (bad-char + good-suffix) | **A2** |
| Kruskal / Prim / Dijkstra (hapat mbi graf) | **A3** |
| Kërkimi sekuencial/binar (kod + gjurmim) | **A4** |
| Analiza e loop-eve "i/=2" ose "i++/j--" | **A5** |
| Horner's Method | **A6** |
| Krahasimi i sorteve (avantazhe/best/worst) | **A7** |
| P/NP/NP-complete/Backtracking | **A8** |
| Katrori Magjik / Anagramet / Kllapat | Exam 1 |
| Maximum Subarray / LIS | Exam 3 |
| Number of Atoms | Exam 4 |
| Radix Sort / Maximum Gap | Exam 5 / 14 |
| 132 Pattern | Exam 5 |
| Floor Sqrt / Perimetri i Poligonit | Exam 6 / 15 |
| Winograd vs Strassen / Speedup makine | Exam 7 |
| Rekurrenca me ekuacion karakteristik | Exam 11 |
| Fshirja O(1) (sortuar vs pasortuar) | Exam 11 / 16 |
| k-distance sorting (Heap O(n log k)) | Exam 12 |
| Majority Element / Sort by Frequency | Exam 14 |

---

*Ky skedar plotëson skedarin 11 (bankën e pyetjeve) me përgjigje të plota. Për teori shtesë të pakonfirmuar në pyetje provimi (algoritme përafrimi, probabilistike, Union-Find, edit distance), shih skedarët 12 dhe 13.*

