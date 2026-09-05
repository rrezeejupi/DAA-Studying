# 13 — Ushtrime Shtesë dhe Banka e Problemeve (nga materiali i asistentit)

*Ky skedar konsolidon problemet nga dokumenti "Përmbledhje detyrash nga DAA" i asistentit, "Pseudo Code Practice Problems", dhe ushtrime shtesë të gjetura nëpër tekstin e plotë të kursit. Shumë prej këtyre PËRSËRITEN në provimet e listuara në skedarin 11 — ku kjo ndodh, është shënuar.*

---

## 1. Probleme "LeetCode-style" (kodim) — Banka e Plotë

### 1.1 Maximum Value & Verifikimi i Korrektësisë
Për sekuencë numrash, dizajno dhe analizo (kohë) algoritmin që gjen vlerën maksimale; trego hapat për verifikimin e korrektësisë. *(Shfaqet si Detyra 1 në GATI TË GJITHA provimet e listuara në skedarin 11 nga ky grup — shih "template-i standard i provimit" më poshtë.)*

### 1.2 Rendit Sipas Frekuencës së Karaktereve (Sort Characters By Frequency) — E RE, del shpesh
Duke pasur parasysh një varg **Vargu**, renditni atë në rend **zbritës** bazuar në frekuencën e karaktereve. Ktheni vargun e renditur (nëse ka më shumë se një përgjigje, ktheni një prej tyre).
- Shembulli 1: Hyrja: `"tree"` → Dalja: `"eert"` ('e' 2 herë, 'r' dhe 't' nga 1 herë)
- Shembulli 2: Hyrja: `"cccaaa"` → Dalja: `"aaaccc"` (të dyja të vlefshme, 'c' dhe 'a' 3 herë secili)
- **KUJDES**: `"cacaca"` është përgjigje **e pasaktë** — karakteret e njëjta duhet të shfaqen bashkë (të grupuara), jo të alternuara!
- **Zgjidhje sugjeruar**: numëro frekuencën (hash map), sorto çelësat sipas frekuencës zbritëse, ndërto string duke përsëritur çdo karakter aq herë sa frekuenca e tij. **Kompleksiteti: O(n + k log k)** ku k = numri i karaktereve distinkte (dominuar nga sortimi i frekuencave).
- *(Shfaqet si Detyra 5/4 në Kolokviumi 1-2023 dhe Provimi 05.01.2024 — 9-15 pikë, pra pikë e konsiderueshme!)*

### 1.3 Elementi i Shumicës (Majority Element) — E RE
Duke pasur parasysh varg numrash me madhësi n, ktheni elementin që shfaqet **më shumë se ⌊n/2⌋** herë. Supozoni se ekziston gjithmonë.
- **Zgjidhje optimale — Algoritmi i Votimit Boyer-Moore**: mban një `candidate` dhe një `count`; nëse count=0, cakto elementin aktual si candidate; nëse elementi aktual = candidate, count++, përndryshe count--. **Kompleksiteti: O(n) kohë, O(1) hapësirë.**
- *(Shfaqet si Detyra 2 në Provimi 05.01.2024, 7 pikë.)*

### 1.4 Numri Minimal i Sallave të Konferencës (Meeting Rooms II) — E RE
Duke pasur parasysh varg intervalesh takimesh `[[s1,e1],[s2,e2],...]` (si<ei), gjeni numrin minimal të sallave të nevojshme.
- **Zgjidhje standarde**: sorto orët e fillimit dhe të mbarimit veç e veç; përdor two-pointer ose min-heap mbi orët e mbarimit — numëro takime të mbivendosura njëkohësisht. **Kompleksiteti: O(n log n)** (dominuar nga sortimi).

### 1.5 Zgjedhja e Dhomës më të Përshtatshme (Closest Room Query) — E RE, komplekse
Hotel me n dhoma `rooms[i]=[roomId_i, size_i]` (id unike). k kërkesa `queries[j]=[preferred_j, minSize_j]`. Për çdo kërkesë, gjej `id` të dhomës me `size ≥ minSize`, që minimizon `abs(id - preferred)`; në rast barazimi, zgjidh id-në më të vogël. Nëse s'ka dhomë të tillë, përgjigja -1.
- **Zgjidhje eficiente**: sorto dhomat sipas madhësisë (zbritëse); sorto kërkesat sipas minSize (zbritëse); përdor një **sorted-set/BST mbi id-të** dhe shto dhoma në të ndërsa përpunon kërkesat me minSize gjithnjë e më të vogël (offline processing) — kërko fqinjët më të afërt me binary search në BST. **Kompleksiteti: O((n+k) log n).**
- *(Problem me vështirësi "hard" — pak gjasa të kërkohet implementim i plotë në provim, por mund të pyetet konceptualisht ose si pjesë e problemeve shtesë.)*

### 1.6 Vlerësimi i Polinomit — Zbatim Konkret (Horner)
Vlerëso `2x³ - 6x² + 2x - 1` për `x=3`, duke shkruar kodin dhe duke analizuar kompleksitetin kohor.
- *(Shfaqet si Detyra 5 në Kolokviumi 2-2023 dhe si Detyra 7 në disa provime "Shkurt 2024" — implementoni Metodën e Horner-it, skedari 06 §1, dhe llogaritni: 2(27)-6(9)+2(3)-1 = 54-54+6-1 = 5.)*

### 1.7 Diferenca Maksimale mes Elementeve Fqinje pas Sortimit (Radix Sort) — konfirmuar shumë e zakonshme
Ktheni diferencën maksimale midis dy elementeve të njëpasnjëshme (fqinjë) në formën e sortuar të vargut. Nëse grupi ka <2 elemente, ktheni 0.
- Shembull: `[9,18,27,3]` → i sortuar `[3,9,18,27]` → dallimet 6,9,9 → **Dalja: 9**.
- *(E njëjta pyetje shfaqet fjalë-për-fjalë në: Kolokviumi i Parë (pa datë), Provimi 05.01.2024, DHE tek Detyra 1 e "Përmbledhjes" — kërkohet ZBATIM me Radix Sort specifikisht, jo çfarëdo sortim, pasi ky është shembull klasik "Maximum Gap" problem me kërkesë kohore lineare O(n).)*

### 1.8 132 Pattern (konfirmuar shumë e zakonshme)
Kontrollo nëse ekziston modeli `132`: indekse i<j<k të tilla që `vargu[i] < vargu[k] < vargu[j]`.
- Shembull: `[3,1,4,2]` → `true` (i=1,j=2,k=3: vargu[1]=1 < vargu[3]=2 < vargu[2]=4). `[1,2,3,4]` → `false`.
- **Zgjidhje eficiente: O(n) me stack monoton** (ruaj kandidatët për "2" në stack, ndiq maksimumin e mundshëm për "3").
- *(Shënim: një version i dokumentit ("Përmbledhje", Detyra 16) e shkruan gabimisht kushtin si `vargu[i]<vargu[k]>vargu[j]` — ky duket typo/gabim transkriptimi, pasi definicioni korrekt i "132 pattern" (siç del edhe në skedarin 11, Exam 5, dhe në shumë burime LeetCode) është `vargu[i] < vargu[k] < vargu[j]`. Përdorni definicionin e saktë: **i<j<k dhe vargu[i]<vargu[k]<vargu[j]**.)*

### 1.9 Rrënja Katrore pa Built-in (konfirmuar shumë e zakonshme)
Kthe ⌊√x⌋ pa përdorur asnjë funksion/operator built-in. *(Skedari 11 — zgjidhet me kërkim binar mbi hapësirën e përgjigjeve [0,x].)*

### 1.10 Perimetri më i Madh i Poligonit (konfirmuar shumë e zakonshme)
*(Shih skedarin 11 për problemin e plotë dhe shembujt.)*

### 1.11 Fshirja në Kohë O(1) (E RE — pyetje konceptuale mbi strukturat e të dhënave)
Përshkruaj si mund të implementohet, ashtu që operacionet vijuese në vargun e dhënë të kenë kohë ekzekutimi **të pavarur nga madhësia e vargut n**:
- (i) Fshirja e elementit të i-të në një varg (pa kërkesë renditjeje)
- (ii) Fshirja e elementit të i-të në një varg **të sortuar**, ku vargu i mbetur duhet të mbetet i sortuar

**Diskutim/përgjigje e sugjeruar:**
- (i) Në varg **të pasortuar**: zëvendëso elementin e i-të me elementin e **fundit** dhe shkurto gjatësinë e vargut me 1 — kjo bëhet në **O(1)** sepse renditja e elementeve s'ka rëndësi.
- (ii) Në varg **të sortuar**: fshirja e elementit të i-të duke ruajtur sortimin dhe duke arritur **vërtetë O(1)** NUK është e mundur me një varg klasik (array) — çdo fshirje kërkon zhvendosjen e elementeve pasues, pra O(n) rasti më i keq. **Zgjidhja konceptuale**: përdor **strukturë tjetër të dhënash** (p.sh. **listë e lidhur** (linked list) — fshirja bëhet në O(1) nëse tashmë ke pointer-in te nyja, por kërkimi për ta gjetur nyjen mbetet O(n); ose një **skip list**/**AVL tree me lazy deletion** — fshirja logjike (marking) në O(log n) mesatarisht, jo O(1) i vërtetë). **Konkluzion i rëndësishëm për provim**: kjo pyetje synon të testojë nëse e kuptoni **trade-off-in themelor** — vargu i sortuar ofron kërkim të shpejtë O(log n) por fshirje/insertim të ngadalshëm O(n); nuk ekziston strukturë "falas" (pa trade-off) që të japë të dyja veçoritë njëkohësisht në O(1)/O(log n) me vargje klasike.

---

## 2. Ushtrime "Radhitja e Funksioneve sipas Rritjes" — praktikë shtesë

**Ushtrimi klasik** (nga teksti i plotë): Radhitni funksionet nga rendi më i lartë te më i ulëti (rrethoni ata të rendit të njëjtë):
```
2ⁿ, lg lg n, n³+lg n, lg n, n−n²+5n³, n−1, n², n³, n lg n, (lg n)², √n, 6, n!, n, (3/2)ⁿ
```
**Qasja**: (1) identifiko familjen e çdo funksioni (konstante, log log n, log n, √n, n, n log n, n², n³, nᵏ, aⁿ, n!); (2) rendit familjet sipas rregullit të njohur: **konstante < log log n < log n < √n < n < n log n < n² < n³ < 2ⁿ < 3ⁿ < ... < n!**; (3) brenda familjes polinomiale (p.sh. n−n²+5n³), mbaj vetëm termin dominant (këtu: 5n³, pra n³).

**Ushtrim shtesë**: për çdo çift f(n), g(n) të dhëna, përcaktoni nëse f=O(g) ose g=O(f):
- f(n)=(n²−n)/2 vs g(n)=6n → **g=O(f)** (f është rendi n²)
- f(n)=n+2√n vs g(n)=n² → **f=O(g)**
- f(n)=n+n log n vs g(n)=n√n → **f=O(g)** (n log n < n√n aq kohë sa log n < √n)
- f(n)=n²+3n+4 vs g(n)=n³ → **f=O(g)**

*(Kjo është praktikë e drejtpërdrejtë e skedarit 02, §4 — ushtrojeni me këto shembuj konkretë nëse pyetja e provimit del në formatin "krahaso f dhe g" në vend të "rendit katër funksione".)*

---

## 3. Union-Find (Partition) — Struktura e të Dhënave pas Kruskal

*(Kjo strukturë NUK ishte e detajuar në skedarin 08 origjinal — shtohet këtu si plotësim i rëndësishëm, pasi qëndron pas çdo implementimi të Kruskal-it.)*

```
InitializePartition(N)
  for i = 1 to N do Parent[i] = -1   // -1 do të thotë: rrënjë, madhësia e komponentit = 1

FindRoot(x)
  while Parent[x] > 0 do x = Parent[x]
  return x

Union(root1, root2)
  // bashko komponentin më të vogël në atë më të madh (union by size)
  if Parent[root1] < Parent[root2] then      // root1 ka komponent më të madh (numra negativë më të mëdhenj në vlerë absolute)
    Parent[root1] = Parent[root1] + Parent[root2]
    Parent[root2] = root1
  else
    Parent[root2] = Parent[root2] + Parent[root1]
    Parent[root1] = root2
```
**Truku**: vlerat negative në `Parent[]` kodojnë **madhësinë** e komponentit (kur nyja është rrënjë); vlera pozitive tregon indeksin e prindit.

**Përdorimi te Kruskal**: para se të shtohet një degë (u,v), thirr `FindRoot(u)` dhe `FindRoot(v)` — nëse janë të ndryshme, shto degën dhe bëj `Union`; përndryshe (të njëjtin root) do të krijonte cikël, hidhet poshtë.

**Kompleksiteti**: me path-compression + union-by-size, pothuajse O(1) amortizuar (O(α(n)), funksioni invers i Ackermann-it — praktikisht konstante). Pa optimizime, O(log n) rasti më i keq për `FindRoot`.

---

## 4. Përputhja e Përafërt e Stringjeve (Edit Distance / Approximate Matching) — plotësim i skedarit 07

*(E gjetur në tekstin e plotë — ky ËSHTË shembull i programimit dinamik, i lidhur me DP-në e mbuluar në skedarin 06 §5, por i aplikuar te stringjet.)*

**Problemi**: gjej numrin minimal të ndryshimeve (zëvendësim, fshirje, shtim karakteri) për të përshtatur substring S me tekstin T (aka **Edit Distance / Levenshtein Distance**).

**Rekurrenca DP:**
```
diffs[i,j] = min(
  diffs[i-1,j-1] + (0 nëse S[i]=T[j], përndryshe 1),   // zëvendësim/përputhje
  diffs[i-1,j] + 1,                                      // fshirje nga S
  diffs[i,j-1] + 1                                       // shtim në S
)
```
**Kompleksiteti: O(S·T)** — tabelë DP me S+1 rreshta, T+1 kolona. Hapësira mund të reduktohet në O(S) (2 kolona njëkohësisht).

**Shembull konceptual**: gjetja e "trim" brenda "try the trumpet" — më i afërti përputhje me vetëm 1 dallim gjendet te "trumpet" (te shkronja 'm', me 'u' në vend të 'i').

---

## 5. Pseudokod — Bazat (nëse ju duhet rifreskim i shpejtë sintakse)

Nga "Pseudo Code Practice Problems": pseudokodi ka 5 komponentë — **Variablat**, **Përcaktimi** (`Set x to 5`), **Input/Output** (`Read`/`Write`), **Selektimi** (`if/else`), **Përsëritja** (`while`). Shembuj bazë praktike (shkrimi i numrave çift, gjetja e min/max nga 5 numra, kontrolli rasti-me-rasti i intervaleve) — të dobishme vetëm si rifreskim nëse keni humbur kontaktin me pseudokodin bazë; jo material provimi më vete.

---

## 6. Shembuj Shtesë të Kompleksitetit (kod praktik, nga materiali i asistentit)

**isPrime(n) — kontrollo kompleksitetin:**
```javascript
function isPrime(n) {
  for (let i = 2; i <= Math.sqrt(n); ++i) {
    if (n % i === 0) return false;
  }
  return true;
}
```
**Kompleksiteti: O(√n)** — unaza kalon nga 2 deri √n.

**Dy unaza të pavarura (jo të ndërthurura!):**
```javascript
let a = 0, b = 0, n;
let m = n * 10;
for (let i = 0; i < n; ++i) { a = a + i; }
for (let j = 0; j < m; ++j) { b = b + j; }
```
**Kompleksiteti kohor: O(n)** (loop i parë O(n), loop i dytë O(m)=O(10n)=O(n) — jo të ndërthurura, pra MBLIDHEN jo shumëzohen: O(n)+O(n)=O(n)). **Hapësinor: O(1)** (vetëm variabla skalare a, b, m, i, j).
*(Kujdes: shumë studentë gabimisht i shumëzojnë këto sepse duken "dy loops" — por s'janë të ndërthurura (nested), janë sekuenciale (njëra pas tjetrës), prandaj kompleksitetet MBLIDHEN, jo shumëzohen!)*

---

## 7. Radix Sort — Ushtrim i Plotë i Punuar (nga teksti, praktikë shtesë)

Për listën `[1113, 2231, 3232, 1211, 3133, 2123, 2321, 1312, 3223, 2332, 1121, 3312]` (numra 4-shifrorë, 3 vlera të mundshme për shifër: 1,2,3):
- **Kalimi 1** (shifra e njësheve): ndaj në kova sipas shifrës së fundit.
- **Kalimi 2** (shifra e dhjetëshëve): rikombino, ndaj sipas shifrës së dytë nga fundi.
- **Kalimi 3** (shifra e qindësheve): rikombino, ndaj sipas shifrës së tretë.
- Rezultati final është lista plotësisht e sortuar.

*(Praktikoni këtë manualisht mbi listën e dhënë — është saktësisht formati i pyetjes "Detyra 1" të Kolokviumit të Parë dhe Provimit 05.01.2024 në skedarin 11, të cilat kërkojnë Radix Sort të zbatuar për "diferencën maksimale".)*

---

## 8. "Template-i Standard" i Provimeve (konfirmuar nga 8+ provime shtesë)

Nga analiza e krejt materialit (skedari 11 + provimet e reja), struktura e provimeve DAA ndjek pothuajse gjithmonë këtë skemë me 4-5 pjesë:

| Pjesa | Tema (variacione të mundshme) |
|---|---|
| **Detyra 1** | Gjetja e vlerës maksimale + analizë kompleksiteti + verifikim korrektësie; PLUS definimi i **Big-O** (ose ndonjëherë **Big-Omega**!) me graf ilustrues |
| **Detyra 2** | Kërkimi sekuencial (pseudokod) + kërkimi binar (hapat + parakushti + kompleksiteti) mbi një varg konkret të vogël (p.sh. [2,3,7,10,90]) — NDONJËHERË zëvendësohet me pyetje rekurrence (p.sh. zgjidh an=-an-1+4an-2+4an-3) |
| **Detyra 3** | Avantazhet e sorteve (Insertion/Quick/Heap/Merge/Bubble/Radix/Shell) — best/worst case; OSE skenar aplikimi (libri i vendit të gabuar; varg me k-distancë) |
| **Detyra 4** | Grafet: Kruskal + (Prim OSE Dijkstra) mbi TË NJËJTIN graf; NDONJËHERË + KMP mbi të njëjtën pyetje |
| **Detyra 5+** | E ndryshueshme: P/NP/NP-complete/Backtracking; OSE analizë kompleksiteti kodesh specifike; OSE bad-character/good-suffix rule + KMP; OSE Horner's method; OSE pyetje O(1) mbi struktura të dhënash |

**Rrjedhimi praktik**: nëse keni kohë të kufizuar, sigurohuni që të zotëroni PLOTËSISHT këto 5 "sllote" të provimit — ato përbëjnë >90% të pikëve në çdo provim të shqyrtuar deri tani (23 provime/kolokviume gjithsej të analizuara).
