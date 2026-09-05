# 11 — Pyetje nga Provimet e Kaluara (Bankë Pyetjesh, 2023-2025)

*Ky skedar përmban pyetje **verbatim** nga **15 sesione provimesh/kolokviumesh** të ndryshme (2020-2025) — 6 nga fotot origjinale në folderin "Afate", plus 9 të tjera nga materiali shtesë i asistentit (folderi "drive-download..."). Ky ËSHTË dokumenti më i vlefshëm për përgatitje — praktikoni çdo problem më poshtë pa parë zgjidhjen fillimisht, pastaj kontrolloni kundrejt skedarëve 01-13.*

**Shënim:** disa foto ishin të errëta/të fokusuara dobët (veçanërisht grafët e dorëshkruar) — kontrolloni fotot origjinale në folderin "Afate" për detaje të sakta numerike të grafëve, sepse OCR-i manual mund të mos jetë 100% i saktë për diagramet.

---

## EXAM 1: Kolokviumi 1 — DAA (20 Nëntor, 2024) — 90 minuta

> **Vërejtje e përgjithshme (shfaqet në shumë provime):** *"Të mos përdoret asnjë funksion/metodë e gatshme (built-in) që ndihmon në procesin e kërkimit/sortimit."*

**Detyra 1. [8 pikë]** Një katror magjik është vargu n×n ku çdo numër nga 1 deri n² paraqitet vetëm një herë në varg. Shuma e numrave në çdo rresht, kolonë dhe diagonale të vargut është e njëjtë.
```
Shembull (Katrori i rendit 3x3):
8 1 6
3 5 7
4 9 2
```
Të zgjidhet problemi duke përdorur ndonjërin nga algoritmet e familjes **divide-and-conquer** dhe të analizohet kompleksiteti kohor i zgjidhjes së ofruar.

**Detyra 2. [10 pikë]** Duke pasur parasysh një varg stringjesh `strs`, gruponi anagramet së bashku. Ju mund ta ktheni përgjigjen në çdo mënyrë.
- Shembull 1: Hyrja: `strs = ["eat","tea","tan","ate","nat","bat"]` → Dalja: `[["bat"],["nat","tan"],["ate","eat","tea"]]`
- Shembull 2: Hyrja: `strs=[""]` → Dalja: `[[""]]`
- Shembull 3: Hyrja: `strs=["a"]` → Dalja: `[["a"]]`

**Detyra 3. [7 pikë]** Jepet një string `s` që përmban vetëm karakteret `'(', ')', '{', '}', '[' dhe ']'`, përcaktoni nëse stringu i hyrjes është i vlefshëm. Një string hyrës është i vlefshëm nëse: (a) Kllapat e hapura duhet të mbyllen me të njëjtin lloj kllapash. (b) Kllapat e hapura duhet të mbyllen në rendin e duhur. Çdo kllapë mbyllëse ka një kllapë të hapur përkatëse të të njëjtit lloj.
- Shembull 1: `s="()"` → `true`. Shembull 2: `s="()[]{}"` → `true`. Shembull 3: `s="(]"` → `false`. Shembull 4: `s="([])"` → `true`.

**Detyra 4. [2 pikë]** Ju ipet ky grup funksionesh të rritjes: **n!, 2ⁿ, 2n², 5nlogn, 20n**. Për funksionin e rritjes 5nlogn, shkruani një vlerë `n` (numër i plotë pozitiv) për të cilin ky funksion është më efikasi nga të gjithë. Nëse nuk ka vlerë të plotë për të cilën është më efikase, shkruani "asnjë".

**Detyra 5. [1 pikë]** Gjeni rastin mesatar Θ (Big Theta) për kodin në vijim:
```java
sum = 0;
for (i = 0; i < n; i++) {
    for (j = 0; A[j] != i; j++)
        sum++;
}
```

**Detyra 6. [1 pikë]** Cili është numri i plotë më i vogël **k** për të cilin **nlogn** është në rangun **O(nᵏ)**?

**Detyra 7. [1 pikë]** Vendosni (shndërroni) relacionin vijues të rekurrencës në formë të mbyllur:
```
T(n) = 3T(n-1) - 15
T(1) = 8
```
*(Zgjidhet me back-substitution — shih skedarin 03, §5 për metodën e plotë të zgjidhjes.)*

---

## EXAM 2: (foto e errët, ndoshta vazhdim i një kolokviumi tjetër) — Analiza e Kompleksitetit të Kodit

*(Duket vazhdim/faqe e dytë e një provimi tjetër me pyetje analize kompleksiteti — jepet edhe një fragment binary search: `... left=mid+1; else { right=mid-1; } return -1;`)*

**c. [3 pikë]** Të tregohet kompleksiteti kohor dhe hapësinor për funksionin në vijim:
```javascript
function funksioni_3(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    return arr;
}
```
*(Ky është Bubble Sort — shih skedarin 05, §2)*

**d. [3 pikë]** Të tregohet kompleksiteti kohor dhe hapësinor për funksionin në vijim:
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
*(Ky është Insertion Sort — shih skedarin 05, §1)*

**e. [3 pikë]** Të tregohet kompleksiteti kohor dhe hapësinor për funksionin në vijim:
```javascript
function quickSort(arr, left = 0, right = arr.length - 1) {
    if (left < right) {
        let pivotIndex = pivot(arr, left, right);
        quickSort(arr, left, pivotIndex - 1);
        quickSort(arr, pivotIndex + 1, right);
    }
    return arr;
}
function pivot(arr, start = 0, end = arr.length - 1) {
    let pivot = arr[start];
    let swapIndex = start;
    for (let i = start + 1; i <= end; i++) {
        if (pivot > arr[i]) {
            swapIndex++;
            [arr[swapIndex], arr[i]] = [arr[i], arr[swapIndex]];
        }
    }
    [arr[start], arr[swapIndex]] = [arr[swapIndex], arr[start]];
    return swapIndex;
}
```
*(Ky është QuickSort — shih skedarin 05, §7. Rasti më i keq O(n²), mesatar O(n log n), hapësira O(log n) mesatarisht.)*

---

## EXAM 3: (foto tjetër, i njëjti stil "funksioni_1/funksioni_2")

**Detyra 1. [12 pikë]**
- a. **[4 pikë]** Çka dallon Shell Sort nga Insertion Sort, dhe cili është kompleksiteti kohor i tij? *(Shih skedarin 05, §3 — përgjigje: Shell Sort para-sorton me increment>1 përpara kalimit final me increment=1 (Insertion Sort i plotë), duke reduktuar lëvizjet krahasuar me Insertion Sort të pastër — kompleksiteti varet nga sekuenca e increment-eve, tipikisht O(N^1.5) deri O(N(logN)²), më mirë se O(N²) i Insertion Sort.)*
- b. **[4 pikë]** Çka dallon Quick Sort nga Merge Sort? *(Shih skedarin 05, §7 — puna bëhet para rekursionit te Quick Sort (top-down) vs pas rekursionit te Merge Sort (bottom-up); Quick Sort in-place por O(n²) worst-case, Merge Sort O(n log n) garantuar por O(n) hapësirë shtesë.)*
- c. **[4 pikë]** Si funksionon HeapSort algoritmi dhe çfarë kompleksiteti kohor ka? *(Shih skedarin 05, §5 — ndërtimi i heap-it O(N), nxjerrja O(N log N), gjithsej O(N log N) në të gjitha rastet.)*

**Detyra 2. [15 pikë]** Duke pasur parasysh një varg numrash të plotë, shkruani një funksion që gjen nënvargun e ngjitur me shumën më të madhe. Nënvargu duhet të jetë i ngjitur, që do të thotë se elementët duhet të jenë në një sekuencë dhe nuk mund të ndahen. Funksioni duhet të kthejë shumën. Për shembull, për vargun `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`, shuma maksimale e nënvargut është **6** (nënvargu është `[4, -1, 2, 1]`).
*(Ky është problemi klasik "Maximum Subarray Sum" — zgjidhet optimalisht me algoritmin e Kadane-s, O(n) kohë, O(1) hapësirë — nuk u trajtua drejtpërdrejtë në ligjërata, por është aplikim i drejtpërdrejtë i të menduarit "iterativ me gjendje" (running sum, rivendos në 0 nëse bëhet negativ).)*

**Detyra 3. [18 pikë]** Duke pasur parasysh një varg numrash të plotë, shkruani një funksion që gjen gjatësinë e nënrenditjes më të gjatë në rritje. Një nënsekuencë është një sekuencë elementësh që shfaqen në të njëjtin rend në vargun origjinal, por jo domosdoshmërisht në mënyrë njëpasnjëshme. Funksioni duhet të kthejë gjatësinë e nënsekuencës më të gjatë në rritje. Për shembull, për vargun `[10, 9, 2, 5, 3, 7, 101, 18]`, nënsekuenca më e gjatë në rritje është `[2, 3, 7, 101]` dhe gjatësia e saj është **4**.
*(Ky është problemi klasik "Longest Increasing Subsequence" — zgjidhje standarde DP O(n²), ose O(n log n) me kërkim binar.)*

**Detyra 4. [15 pikë]**
- a. **[3 pikë]**
```javascript
function funksioni_1(arr, x) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === x) { return i; }
    } return -1;
}
```
*(Kërkim sekuencial — O(n) worst-case)*
- b. **[3 pikë]**
```javascript
function funksioni_2(arr, x) {
    let left = 0;
    let right = arr.length - 1;
    while (left <= right) {
        let mid = Math.floor((left + right) / 2);
        if (arr[mid] === x) { return mid; }
        else if (arr[mid] < x) { left = mid + 1; }
        else { right = mid - 1; }
    }
    ...
}
```
*(Kërkim binar — O(log n) worst-case)*

---

## EXAM 4: Provimi Final — Qershor 2023

**Detyra 1. [18 pikë]** Duke pasur parasysh variablën `formula` — e tipit string, që përfaqëson një formulë kimike, ktheni numrin e secilit atom. Elementi atomik gjithmonë fillon me një karakter të madh, pastaj zero ose më shumë shkronja të vogla, që përfaqësojnë emrin. Një ose më shumë shifra që përfaqësojnë numërimin e atij elementi mund të pasojnë nëse numërimi është më i madh se 1. Nëse numërimi është 1, nuk do të ketë asnjë shifër. Për shembull, "H2O" dhe "H2O2" janë të mundshme, ndërsa "H1O2" jo. Dy formula janë të lidhura së bashku për të prodhuar një formulë tjetër. Për shembull, "H2O2He3Mg4" është gjithashtu formulë. Një formulë e vendosur në kllapa dhe një numërim (i shtuar opsionalisht) është gjithashtu formulë. Për shembull, "(H2O2)" dhe "(H2O2)3" janë formula.

Ktheni numrin e të gjithë elementëve si string në formën e mëposhtme: emrin e parë (i sortuar), i ndjekur nga numri i tij (nëse ai numër është më i madh se 1), i ndjekur nga emri i dytë (i sortuar), i ndjekur nga numërimi (count) i tij (nëse ai numër është më i madh se 1), e kështu me radhë.

- Shembulli 1: Hyrja: `formula = "H2O"` → Dalja: `"H2O"` — Numri i elementeve: {'H':2, 'O':1}
- Shembulli 2: Hyrja: `formula = "Mg(OH)2"` → Dalja: `"H2MgO2"` — Numri i elementeve: {'H':2,'Mg':1,'O':2}
- Shembulli 3: Hyrja: `formula = "K4(ON(SO3)2)2"` → Dalja: `"K4N2O14S4"` — Numri i elementeve: {'K':4,'N':2,'O':14,'S':4}

*(Ky është problemi klasik "Number of Atoms" — kërkon parsim rekursiv/stack-based i stringut me kllapa, numërim, dhe sortim final i emrave të elementeve. Del si problemi kryesor kodimi në KËTË provim final, me 18 nga pikët totale — praktikojeni patjetër!)*

---

## EXAM 5: Kolokviumi i Parë — DAA (pa datë të dukshme, ndoshta 2020/2021 ose variant tjetër)

**1. [9 pikë]** Të shkruhet kodi në gjuhë programuese që e zgjidh problemin në vijim duke përdorur **radix sort** algoritmin: Duke pasur parasysh një varg numrash të plotë, ktheni diferencën maksimale midis dy numrave të njëpasnjëshëm në formën e tyre të sortuar. Nëse grupi përmban më pak se dy elemente, ktheni 0.
```
Shembull: Hyrja: vargu = [9,18,27,3]
Dalja: 9
Sqarim: Vargu i sortuar është [3,9,18,27], dhe çiftet (9,18) dhe (18,27) kanë diferencën më të madhe 9.
```

**2. [6 pikë]** Është dhënë vargu `V = [2, 3, 7, 10, 90]`
- a. Të shkruhet pseudokodi për kërkimin sekuencial.
- b. Të tregohet hapat se si do të punoj algoritmi për kërkimin binar nëse vlera që kërkohet është 90 në vargun e dhënë dhe tregoni cili është parakushti që duhet të plotësohet për aplikimin e kërkimit binar.
- c. Të tregohet kompleksiteti kohor për kërkimin binar.

**3. [2 pikë]** Supozoni qe keni një koleksion librash. Ju e dini që librat në koleksionin tuaj janë pothuajse të renditur sipas titullit, me përjashtim të një libri i cili është në vendin e gabuar. Ju dëshironi që katalogu të jetë renditur plotësisht, cilin nga algoritmet e mëposhtme do të aplikonit:
a. Insertion Sort  b. Merge Sort  c. Shell Sort  d. Heap Sort
*(Përgjigja e sugjeruar: **Insertion Sort** — sepse për listë "pothuajse të sortuar" me vetëm 1 element jashtë vendit, Insertion Sort afrohet te rasti më i mirë O(n), duke qenë shumë efikas pikërisht në këtë skenar — shih skedarin 05, §1.)*

**4. [4 pikë]** Cilat janë avantazhet e Bubble Sort, QuickSort, HeapSort, Radix Sort dhe MergeSort? Tregoni rastin më të keq dhe më të mirë të kompleksitetit kohor.

**5. [9 pikë]** Të shkruhet kodi i cili përmes inputit e krijon vargun **vargu** me n elemente numra të plotë. Të kontrollohet nëse ekziston modeli (ang. Pattern) **132** i tre numrave `vargu[i], vargu[j], vargu[k]`, ashtu që të vlej kushti: **i<j<k** dhe **vargu[i]<vargu[k]<vargu[j]**. Shfaq **true** nëse ekziston 132 modeli në atë varg, përndryshe shfaq **false**.
- Shembulli a) Hyrja: `vargu=[3,1,4,2]` → Dalja: `true`
- Shembulli b) Hyrja: `vargu=[1,2,3,4]` → Dalja: `false`
*(Ky është problemi klasik "132 Pattern" — zgjidhje efikase O(n) me stack monoton.)*

---

## EXAM 6: DAA — Shkurt 2024 (Provim gjithëpërfshirës, disa faqe)

**Detyra 1. [10 pikë]** Duke pasur parasysh një numër të plotë jo negativ x, ktheni rrënjën katrore të x të rrumbullaksuar poshtë në numrin e plotë më të afërt. Numri i plotë i kthyer duhet të jetë gjithashtu jo negativ. **Nuk duhet të përdorni asnjë built-in funksion/metodë ose operator.**
- Shembulli 1: Hyrja: `x=4` → Dalja: `2`
- Shembulli 2: Hyrja: `x=8` → Dalja: `2` (rrënja katrore e 8 është 2.82842..., rrumbullakohet poshtë)
*(Zgjidhet me kërkim binar mbi hapësirën e përgjigjeve të mundshme [0,x] — praktika direkte e skedarit 04, §3.)*

**Detyra 2. [15 pikë]** Ipet një varg me numra të plotë pozitiv `nums`, me gjatësi n. Një poligon është një figurë e rrafshët e mbyllur që ka të paktën 3 anë. Brinja më e gjatë e një poligoni është më e vogël se shuma e brinjëve të tjera të tij. Anasjelltas, nëse keni k (k≥3) numra realë pozitivë a1≤a2≤a3≤...≤ak dhe a1+a2+a3+...+ak-1 > ak, atëherë ekziston gjithmonë një poligon me k brinjë. Perimetri i një poligoni është shuma e gjatësive të brinjëve të tij. Ktheni perimetrin më të madh të mundshëm të një poligoni, anët e të cilit mund të formohen nga `nums`, ose **-1** nëse nuk është e mundur të krijohet një poligon.
- Shembulli 1: Hyrja: `nums=[5,5,5]` → Dalja: `15`
- Shembulli 2: Hyrja: `nums=[1,12,1,2,5,50,3]` → Dalja: `12` (poligoni me perimetrin më të madh ka 5 brinjë: 1,1,2,3,5)

**Detyra 3. [5 pikë]** Duke përdorur algoritmin e **Kruskal-it**, të tregohen hapat e gjetjes së PSHM-së (MST) për grafin e dhënë në vijim. Më tej, çfarë kompleksiteti kohor ka aplikimi i këtij algoritmi? *(Graf me nyje A, B, C, D, E, F, G dhe brinjë të peshuara — shihni foton origjinale `IMG_1101.jpg` në folderin Afate për vlerat e sakta të peshave, pasi transkriptimi manual i një diagrami të fotografuar mund të mos jetë 100% i saktë.)*

**Detyra 4. [7 pikë]** Duke përdor Algoritmin e **Prim-it**, të tregohen hapat e gjetjes së PSHM-së për grafin e dhënë në detyrën 3. Më tej, çfarë kompleksiteti kohor ka aplikimi i këtij algoritmi?
*(Kjo çift pyetjesh Kruskal+Prim mbi TË NJËJTIN graf është model shumë i zakonshëm — praktikoni të dyja algoritmet mbi grafin e njëjtë dhe verifikoni që jepni të njëjtin PSHM total, edhe nëse hapat/renditja e degëve të shtuara ndryshon.)*

**Detyra 5. [8 pikë]**
- Ç'ka nënkuptojmë me **bad character rule** dhe **good suffix rule**?
- Përdor bad character rule dhe good suffix rule për kërkim të Pattern. Text: `GTTATAGCTGATCGCGGCGTAGCGGCGAA` Pattern: `GTAGCGGCG`
- Të tregohen hapat e gjetjes së përputhjes së stringut për mostrën **"AAAAB"** në stringun **"AAAAAAAAAAAAAAAAB"**, duke përdorur algoritmin **KMP (Knuth-Morris-Pratt)**.

**Detyra 6. [6 pikë]**
- a. **[2 pikë]** Të tregohet kompleksiteti kohor për funksionin në vijim:
```java
int fun(int n) {
    int count = 0;
    for (int i = n; i > 0; i /= 2)
        for (int j = 0; j < i; j++)
            count += 1;
    return count;
}
```
*(i zvogëlohet për gjysmë çdo herë → log n kalime të jashtme; loop e brendshme punon 0 deri n herë → shuma gjeometrike n+n/2+n/4+... → **O(n)**.)*
- b. **[2 pikë]** Të tregohet kompleksiteti kohor për funksionin në vijim:
```java
int fun(int n) {
    int count = 0;
    for (int i = 0; i < n; i++)
        for (int j = i; j > 0; j--)
            count = count + 1;
    return count;
}
```
*(Loop e brendshme punon i herë; shuma 0+1+2+...+(n-1) = n(n-1)/2 → **O(n²)**.)*
- c. **[2 pikë]** Cila nga shprehjet më poshtë NUK është O(n²)?
  - (15^10)·n + 12099
  - n^1.98
  - **n³/√n = n^2.5 ← KJO NUK ËSHTË O(n²)** (n^2.5 rritet më shpejt se n²)
  - (2^20)·n

**Detyra 7. [4 pikë]** Të implementohet Metoda e **Horner-it** për [vlerësimin e një polinomi — teksti u pre në foto, shih skedarin 06, §1 për algoritmin e plotë].

**Detyra 8. [5 pikë]** Të shpjegohet koncepti **P, NP dhe NP-complete**, si dhe **Back-Tracking**. *(Shih skedarin 09, Pjesa II, §2-4 dhe fundin e skedarit për shpjegimin e Backtracking-ut.)*

---

## EXAM 7: Provimi Final — DAA (10.05.2025) — 90 minuta

**Detyra 1. [10 pikë]** Njëjtë si Detyra 1 e Exam 1 (katrori magjik n×n, zgjidhje **divide-and-conquer**, analizë kompleksiteti).

**Detyra 2. [15 pikë]** Njëjtë si Detyra 3 e Exam 1 (verifikimi i kllapave të balancuara — valid parentheses).

**Detyra 3. [5 pikë]** Shpjego se si funksionon algoritmi i **Winograd** dhe **Strassen**. Më tej, krahaso kompleksitetin kohor të aplikimit të tyre dhe përmend dallimet kryesore në çasje.
*(Shih skedarin 06, §2 — Winograd mbetet O(N³) me konstante ~gjysmë; Strassen arrin O(N^2.81) përmes 7 formulave rekursive për blloqe 2×2.)*

**Detyra 4. [4 pikë]** Duke ditur se kompleksiteti kohor i një algoritmi është **T(n) = 3 × 2ⁿ**, dhe një makinë i duhen `t` sekonda të procesoj `n` hyrje, sa hyrje do të mund të procesohen në `t` sekonda në një makinë të re **64 herë më shpejtë**?
*(Këshillë zgjidhjeje: 64 = 2⁶, pra makina e re mund të bëjë 2⁶ herë më shumë punë në të njëjtën kohë `t` → 3×2^(n+m) = 64×3×2ⁿ ⇒ 2^m=64=2⁶ ⇒ **m=6** → makina e re mund të procesojë **n+6** hyrje në po atë kohë `t`.)*

**Detyra 5. [4 pikë]** Cilat janë dallimet kryesore midis algoritmeve të kërkimit **Knuth-Morris-Pratt (KMP)** dhe **Boyer-Moore**?
*(Shih skedarin 07, §4 — tabela e plotë krahasuese: drejtimi i krahasimit, informacioni i përdorur, garancia teorike, performanca praktike.)*

---

## EXAM 8: Provimi_DAA_Janar2021 (75 minuta)
- **Detyra 1. [20p]** a) Analiza kohore e algoritmit që gjen vlerën maksimale + verifikim korrektësie (15p). b) Definoni Big-O, me graf (5p).
- **Detyra 2. [20p]** V=[2,3,7,10,90]: a) pseudokod kërkim sekuencial (5p) b) hapat kërkim binar për 90 + parakushti (10p) c) kompleksiteti i kërkimit binar (5p).
- **Detyra 3. [10p]** Avantazhet e Insertion Sort, QuickSort, HeapSort, MergeSort — rasti më i mirë/keq.
- **Detyra 4. [30p] — Grafet dhe kërkimi në stringje:** a) Kruskal → PSHM për grafin e dhënë. b) KMP: gjeni përputhjen e mostrës "AAAAB" në stringun "AAAAAAAAAAAAAAAAB".
- **Detyra 5. [20p]** Shpjegoni P, NP, NP-complete, dhe algoritmin Backtracking.

## EXAM 9: Provimi_DAA_Nentor2020 (75 minuta)
Struktura identike me Exam 8 (Detyra 1-3), por:
- **Detyra 4. [30p] — Grafet dhe pemët:** a) Kruskal → PSHM. b) **Dijkstra**: shtegu më i shkurtër prej nyjes 1 te të gjitha nyjet tjera, për të NJËJTIN graf.
- **Detrya 5. [20p]** Kompleksiteti kohor për 2 funksione të dhëna (grafika e kodit s'u nxor nga skanimi — kontrolloni origjinalin nëse e keni).

## EXAM 10: Provimi_DAA_Qershor-2 (75 minuta)
Struktura e njëjtë Detyra 1-2, por:
- **Detyra 3. [10p]** *(skenari i bibliotekës)*: Keni koleksion librash pothuajse të renditur, me 1 libër në vend të gabuar. Cilin algoritëm aplikoni: a.Insertion b.Merge c.Radix d.Heap? *(Përgjigje: Insertion Sort — afrohet te rasti më i mirë O(n) për listë pothuajse të sortuar.)*
- **Detyra 4. [30p]:** a) Kruskal → PSHM. b) **Dijkstra** shtegu më i shkurtër, nyja 1, i njëjti graf.
- **Detrya 5. [20p]** Kompleksiteti kohor për 2 funksione (grafika mungon nga skanimi).

## EXAM 11: Provimi_DAA_Shtator-1 (75 minuta)
- **Detyra 1. [20p]** a) Renditja e trendit të rritjes për f1=2ⁿ, f2=n^(3/2), f3=n log n, f4=n^(log n) — MC me 4 opsione (15p). b) Definoni **Big O** (5p).
- **Detyra 2. [20p]** V=[2,3,7,10,92,55] *(shënim: OCR-i origjinal tregon "92. 55" — mund të jetë artefakt skanimi i dy vlerave 92 dhe 55, kontrolloni origjinalin)*: a) pseudokod kërkim sekuencial (5p). b) hapat kërkim binar për 92 (10p). c) **Cila është zgjidhja e relacionit të rekurrencës: an = -an-1 + 4an-2 + 4an-3, me a0=8, a1=6 dhe a2=26?** (5p) — *(⭐ ky është saktësisht Shembulli 2 i skedarit 03 §7 — përgjigja: an = 2·(-1)ⁿ + (-2)ⁿ + 5·2ⁿ)*
- **Detyra 3. [10p]** Avantazhet e Insertion/Quick/Heap/MergeSort.
- **Detyra 4. [30p]:** a) Kruskal → PSHM. b) KMP: "AAAAB" në "AAAAAAAAAAAAAAAAB".
- **Detrya 5. [20p]** a) Kompleksiteti fun() (grafika mungon). b) Përshkruani si të implementohet, ashtu që fshirja e elementit të i-të (i) në varg të pasortuar, dhe (ii) në varg të sortuar (duke ruajtur sortimin), të mos varet nga madhësia n. *(Shih skedarin 13, §1.11 për diskutimin e plotë.)*

## EXAM 12: Provimi_DAA_Shtator2021 (75 minuta)
- **Detyra 1. [20p]** a) max+BigO analizë (15p). b) Definoni **Big Omega (Ω)** — *(shënim: ndryshe nga shumica e provimeve që kërkojnë Big-O, ky kërkon Ω — mos supozoni gjithmonë O!)* (5p).
- **Detyra 2. [20p]** V=[2,3,7,10,20], target=20 — sekuencial + binar + kompleksiteti.
- **Detyra 3. [10p]** *(varg me veti k-distancë)*: çdo element ka distancë maksimale k nga pozicioni i sortuar. Cili algoritëm modifikohet lehtë për ta sortuar, dhe kompleksiteti? a. Insertion O(kn) b. Heap O(nLogk) c. Quick O(kLogk) d. Merge O(kLogk) — *(përgjigje e sugjeruar: **Heap Sort me O(n log k)**, duke përdorur një min-heap të madhësisë k+1 — teknikë klasike "sort a nearly-sorted/k-sorted array".)*
- **Detyra 4. [30p]:** a) Kruskal → PSHM. b) **Dijkstra** shtegu më i shkurtër, i njëjti graf.
- **Detrya 5. [20p]** a) Kompleksiteti `fun()` me `for(i=n;i>0;i/=2) for(j=0;j<i;j++)` → **O(n)**. b) Kompleksiteti `fun()` me `for(i=0;i<n;i++) for(j=i;j>0;j--)` → **O(n²)**. c) Cila shprehje NUK është O(n²): (15^10)·n+12099 / n^1.98 / **n³/√n** ← kjo / (2^20)·n.

## EXAM 13: DAA — Kolokviumi 1 — 2023 (PDF, 90 minuta)
- **Detyra 1. [5p]** Dijkstra: shtegu më i shkurtër prej nyjes 1 te të gjitha nyjet, + kompleksiteti.
- **Detyra 2. [10p]** a) Kruskal → PSHM + kompleksiteti. b) **Prim** → PSHM për TË NJËJTIN graf (2a) + kompleksiteti.
- **Detrya 3. [4p]** P, NP, NP-complete + Backtracking.
- **Detrya 4. [7p]** a) Bad character rule + good suffix rule (koncepte). b) Zbatim mbi Text: `GTTATAGCTGATCGCGGCGTAGCGGCGAA`, Pattern: `GTAGCGGCG`. c) KMP: "AAAAB" në "AAAAAAAAAAAAAAAAB".
- **Detrya 5. [4p]** Kodi për vlerësim polinomi (Horner): vlerësoni **2x³ - 6x² + 2x - 1** për **x=3** + kompleksiteti. *(Zgjidhje: 2(27)-6(9)+2(3)-1 = 54-54+6-1 = **5**.)*

## EXAM 14: DAA — Kolokviumi 2 — 2023 (PDF, 90 minuta)
- **1. [8p]** *(reasoning i kërkuar!)* Zgjidhni algoritmin më të përshtatshëm + arsyetoni pse: diferenca maksimale mes elementeve fqinje pas sortimit. `[9,18,27,3]`→`9`. *(Përgjigje: **Radix Sort**, sepse arrin O(n) pa krahasime — nëse çelësat janë numra të plotë me gamë të kufizuar.)*
- **2. [7p]** **Elementi i Shumicës** (Majority Element): elementi që shfaqet >⌊n/2⌋ herë. *(Shih skedarin 13, §1.3 — Algoritmi i Votimit Boyer-Moore, O(n)/O(1).)*
- **3. [5p]** Avantazhet Bubble/Quick/Heap/Radix/MergeSort — best/worst case.
- **4. [15p]** **Sort Characters By Frequency**: rendit karakteret zbritës sipas frekuencës. `"tree"`→`"eert"`. *(Shih skedarin 13, §1.2 — vini re kujdesin: "cacaca" është e pasaktë!)*
- **5. [7p]** Kruskal → PSHM + kompleksiteti.
- **6. [8p]** Prim → PSHM për grafin e detyrës 5 + kompleksiteti.
- **7. [10p]** Bad-character/good-suffix + zbatim (i njëjti Text/Pattern si Exam 13) + KMP "AAAAB"/"AAAA...B".

## EXAM 15: DAA — Provimi — Janar 2024 / EN_Provimi_DAA_Janar2024 (PDF, versione shqip DHE anglisht, 90 minuta, "DAA Shkurt 2024")
*(Kjo është E NJËJTA provim si "EXAM 6" (Shkurt 2024) e dokumentuar më lart nga fotot — tani e kemi të konfirmuar fjalë-për-fjalë, në 2 gjuhë, që vërteton saktësinë e transkriptimit të mëparshëm nga imazhe.)*
- **Q1./Detyra 1. [10p]** Rrënja katrore floor pa built-in.
- **Q2./Detyra 2. [15p]** Perimetri më i madh i poligonit.
- **Q3./Detyra 3. [5p]** Kruskal → PSHM + kompleksiteti.
- **Q4./Detyra 4. [7p]** Prim → PSHM (i njëjti graf si Q3) + kompleksiteti.
- **Q5./Detyra 5. [8p]** Bad-character/good-suffix (Text: `GTTATAGCTGATCGCGGCGTAGCGGCGAA`, Pattern: `GTAGCGGCG`) + KMP "AAAAB"/"AAAA...B".
- **Q6./Detyra 6. [6p]** a) `fun()` me i/=2 nested → O(n). b) `fun()` me i++/j-- nested → O(n²). c) Cila NUK është O(n²) → n³/√n.
- **Q7./Detyra 7. [4p]** Horner's Method (implementim i përgjithshëm).
- **Q8./Detrya 8. [5p]** P, NP, NP-complete + Backtracking.

## EXAM 16: Provimi_DAA_Nentor2024 (variant tjetër/më i plotë i Exam 1, 90 minuta)
*(Ky dokument përmban të njëjtat Detyra 1-5 si "EXAM 1" më lart, POR shton edhe Detyra 6-7 që s'ishin të dukshme në foto — pra ky ËSHTË po ai provim (Kolokviumi 1, 20 Nëntor 2024), vetëm më i plotë. Detyra 1-5 identike (growth funcs/T(n) speedup/min k/anagram/valid parens) — SHTESAT E REJA:)*
- **Detyra 6. [15p]** a) Kruskal → PSHM. b) KMP: "AAAAB" në "AAAAAAAAAAAAAAAAB".
- **Detrya 7. [10p]** a) Kompleksiteti i `fun()` (5p). b) Përshkruani si operacionet vijuese në varg të mos varen nga n: (i) fshirja e elementit të i-të pa kërkesë renditjeje, (ii) fshirja e elementit të i-të duke ruajtur sortimin (5p). *(Shih skedarin 13, §1.11.)*

---

## Përmbledhje: Llojet e Problemeve që Përsëriten (praktikoni secilën!)

| Lloji i Problemit | Shfaqet në | Zgjidhje/Referencë |
|---|---|---|
| Katrori Magjik (Divide & Conquer) | Exam 1, Exam 7, Exam 16 | Skedari 01, §7 |
| Grupimi i Anagrameve | Exam 1, Exam 16 | Hashing/sortim i shkronjave si çelës |
| Kllapa të Balancuara (Valid Parentheses) | Exam 1, Exam 7, Exam 16 | Stack-based |
| Krahasimi i shpejtësisë së rritjes (n!, 2ⁿ, etj.) | Exam 1, Exam 16 | Skedari 02, §4 |
| Renditja e f1..f4 sipas rritjes (2ⁿ, n^1.5, nlogn, n^logn) — MC | Exam 11, Exam 12 | Skedari 02, §4 |
| Rasti mesatar Θ për kod të dhënë | Exam 1 | Skedari 02, §1 |
| Vlera minimale k, O(nᵏ) | Exam 1, Exam 6, Exam 16 | Skedari 02, §6 |
| Rekurrenca në formë të mbyllur (back-substitution) | Exam 1 | Skedari 03, §5 |
| Zgjidhja e rekurrencës me ekuacion karakteristik (an=-an-1+4an-2+4an-3) | Exam 11 | Skedari 03, §7, Shembulli 2 |
| Analiza e kompleksitetit të fragmenteve kodi (bubble/insertion/quicksort/binary search/nested loops) | Exam 2, Exam 3, Exam 6, Exam 9, Exam 10, Exam 12, Exam 16 | Skedarët 02, 05 |
| Krahasime teorike sortesh (Shell/Insertion, Quick/Merge, Heap) | Exam 3, Exam 5, Exam 8, Exam 12, Exam 14 | Skedari 05, §9 |
| Maximum Subarray Sum | Exam 3 | Kadane's Algorithm |
| Longest Increasing Subsequence | Exam 3 | DP O(n²) ose O(n log n) |
| Parsimi i Formulës Kimike (Number of Atoms) | Exam 4 | Parsim rekursiv/stack |
| Radix Sort — diferenca maksimale mes fqinjëve (Maximum Gap) | Exam 5, Exam 14 | Skedari 05, §4; Skedari 13, §1.7 |
| Elementi i Shumicës (Majority Element) | Exam 14 | Boyer-Moore Voting, skedari 13 §1.3 |
| Rendit Karakteret sipas Frekuencës (Sort Chars By Frequency) | Exam 14 | Skedari 13, §1.2 |
| Kërkimi Sekuencial/Binar (pseudokod + kompleksitet) | Exam 5, Exam 1, Exam 8-12 (të gjitha) | Skedari 04 |
| Zgjedhja e algoritmit të duhur sipas skenarit (libri i vendit të gabuar) | Exam 5, Exam 10 | Njohuri e thellë e karakteristikave të sorteve |
| Varg me veti k-distancë — cili sort modifikohet | Exam 12 | Heap Sort me min-heap madhësi k+1, O(n log k) |
| 132 Pattern (array) | Exam 5 | Monotonic stack O(n) |
| Rrënja Katrore pa built-in (Floor Sqrt) | Exam 6, Exam 15 | Kërkim binar mbi hapësirën e përgjigjeve |
| Perimetri më i madh i Poligonit | Exam 6, Exam 15 | Sortim + greedy nga fundi |
| Kruskal + Prim mbi të njëjtin graf | Exam 6, Exam 13, Exam 14, Exam 15 | Skedari 08, §4 |
| Kruskal + Dijkstra mbi të njëjtin graf | Exam 9, Exam 10, Exam 12 | Skedari 08, §4-5 |
| Kruskal + KMP (jo graf i njëjtë, dy pyetje të pavarura) | Exam 8, Exam 11, Exam 12, Exam 16 | Skedarët 07-08 |
| Dijkstra vetëm (shteg më i shkurtër) | Exam 13 | Skedari 08, §5 |
| Bad Character Rule / Good Suffix Rule (Boyer-Moore) | Exam 6, Exam 13, Exam 14, Exam 15 | Skedari 07, §4 |
| KMP aplikim manual ("AAAAB" në "AAAA...B") | Exam 6, Exam 8, Exam 11, Exam 12, Exam 13, Exam 14, Exam 15, Exam 16 | Skedari 07, §3 — **PYETJA MË E PËRSËRITUR NGA TË GJITHA!** |
| Horner's Method (implementim) | Exam 6, Exam 7, Exam 13, Exam 15 | Skedari 06, §1 |
| Fshirja në O(1) (varg i sortuar vs i pasortuar) | Exam 9, Exam 11, Exam 16 | Skedari 13, §1.11 |
| Definimi Big-Ω (jo vetëm Big-O!) | Exam 9 | Skedari 01, §5 |
| P, NP, NP-complete, Backtracking (koncepte) | Exam 6 | Skedari 09, Pjesa II |
| Winograd vs Strassen | Exam 7 | Skedari 06, §2 |
| Rritja eksponenciale + shpejtësia e re e makinës | Exam 7 | Algjebër me fuqi të 2-shit |
| KMP vs Boyer-Moore (krahasim teorik) | Exam 7 | Skedari 07, §4 |
