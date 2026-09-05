# Plani i Studimit — Dizajni dhe Analiza e Algoritmeve (DAA)

**Lënda:** Dizajni dhe Analiza e Algoritmeve, FIEK, Viti III, Semestri V, Zgjedhore, 2+2 orë/javë, 5 ECTS
**Profesori:** Avni Rexhepi (avni.rexhepi@uni-pr.edu, +38344174374)
**Literatura kryesore:** Jeffrey J. McConnell, *Analysis of Algorithms: An Active Learning Approach*, Jones and Bartlett, 2001
**Literatura shtesë:** D.S. Malik, *C++ Programming*; Steven Skiena, *The Algorithm Design Manual*

Ky folder është krijuar duke kaluar nëpër **të gjitha** ligjëratat (21 PDF), skedarët e provimeve të kaluara (folderi "Afate"), dhe ushtrimet ekzistuese në kodin Java ("Ushtrime DAA"). Përmban shënime të plota studimi, të organizuara sipas temave, plus një bankë pyetjesh nga provimet e kaluara që tregojnë **saktësisht çfarë lloj pyetjesh vijnë në provim**.

---

## ⚡ PLANI I SHPEJTË — 5 DITË (Provimi: 10 Shtator)

*Meqë provimi është shumë afër, ky është rendi i prioriteteve bazuar në frekuencën e temave në 7 provimet e kaluara të analizuara (skedari 11). Kjo ZËVENDËSON planin 10-ditor më poshtë për këtë raund — nëse keni më shumë kohë, kthehuni te plani i plotë.*

### TIER 1 — Duhet t'i dini me çdo kusht (dalin në PRATIKISHT ÇDO provim)
1. **Analiza e kompleksitetit të fragmenteve kodi** (loops të ndërthurura, rekursion) — skedari 02, §1-2. Praktikoni direkt me fragmentet nga skedari 11 (Exam 2, Exam 3, Exam 6) pa shikuar përgjigjen.
2. **Master Theorem + relacione rekurence në formë të mbyllur** (back-substitution kur s'aplikohet Master Theorem) — skedari 02 §3 dhe skedari 03 §5-6. Kjo është formulë — mësojeni përmendësh.
3. **Tabela krahasuese e sortimeve** (Shell vs Insertion, Quick vs Merge, si funksionon Heapsort) — skedari 05, §9. Praktikoni t'i shpjegoni këto dallime me fjalë tuaja, jo vetëm t'i lexoni.
4. **KMP dhe Boyer-Moore** — gjurmim manual hap-pas-hapi (bad character rule, good suffix rule) — skedari 07, §3-4. Praktikoni mbi shembujt konkretë të dhënë në skedarin 11 (Exam 6).
5. **Kruskal + Prim mbi të njëjtin graf** — skedari 08, §4. Vizatoni një graf të thjeshtë (6-7 nyje) dhe praktikoni të dy algoritmet mbi të njëjtin graf derisa t'ju dalë natyrshëm.
6. **P, NP, NP-complete, Backtracking** (koncepte, shpjegim me fjalë) — skedari 09, Pjesa II + fundi. Kjo është pikë e lehtë/e shpejtë për t'u mësuar, mos e lini pa e mësuar.

### TIER 2 — Shumë gjasa të dalin, praktikojini pas Tier 1
7. **Krahasimi i shpejtësisë së rritjes** (n!, 2ⁿ, n², n log n, gjetja e k minimale për O(nᵏ)) — skedari 02, §4.
8. **Horner's Method** (implementim) — skedari 06, §1. E shpejtë për t'u mësuar, del shpesh.
9. **Winograd vs Strassen** (koncepte + krahasim) — skedari 06, §2.
10. **Probleme kodimi tip "divide-and-conquer"/stringje** (katrori magjik, kllapa të balancuara, anagramet) — praktikoni t'i shkruani nga zeri, jo vetëm t'i lexoni. Shih skedarin 11, Exam 1/Exam 7.

### TIER 3 — Nëse ju mbetet kohë
11. Algoritmi i Euklidit (GCD), numra primar/Sita e Erathostenit — skedari 06, §4.
12. Probleme LeetCode-style specifike (rrënja katrore pa built-in, perimetri i poligonit, max subarray sum, longest increasing subsequence, 132 pattern) — skedari 11 ka shembuj konkretë me zgjidhje të sugjeruara.

### ÇKA TË MOS HUMBNI KOHË (frekuencë e ulët në provimet e shqyrtuara)
- Algoritmet paralele (PRAM, kosto/procesorë) — s'u shfaq asnjëherë si pyetje konkrete provimi.
- Detajet e thelluara të grafeve përtej Kruskal/Prim/Dijkstra (Euler path/circuit, komponentë bikonektuara) — s'u shfaqën në provimet e shqyrtuara.
- Sorti i Jashtëm Polifazor (external merge sort) — teorik, s'u pa në asnjë provim.
- Përshtatja e përafërt e stringjeve (DP diffs matrix) — e mundshme por frekuencë e ulët.
- Ekuacionet lineare (Gauss-Jordan) — teorik, frekuencë e ulët në pyetjet konkrete të gjetura.

### Orari sugjeruar (5 ditë deri më 10 Shtator)
- **Sot/Nesër:** Tier 1, pikat 1-2 (kompleksiteti + rekurrenca/Master Theorem) — themeli i gjithçkaje tjetër.
- **Dita pasnesër:** Tier 1, pikat 3-4 (sortimi + string matching).
- **Dita e katërt:** Tier 1, pikat 5-6 (grafet + P/NP/Backtracking) + Tier 2, pikat 7-9.
- **Dita e pestë (para provimit):** Tier 2, pika 10 (praktikë kodimi) + Tier 3 shpejt + rilexim i plotë i skedarit 11 nga fillimi në fund.
- **Dita e provimit (10 Shtator):** vetëm rishikim i shpejtë i tabelave përmbledhëse (skedari 05 §9, skedari 02 §3) dhe skedarit 11 — mos mësoni material të ri, vetëm konsolidoni.

---

## 1. Struktura e Notimit (nga syllabusi zyrtar)

| Komponenti | Pesha |
|---|---|
| Prag kalimi (minimumi për të kaluar) | 50% |
| Pjesëmarrja (Attendance) | 10% |
| Puna individuale në orë | 10% |
| Detyra shtëpie / Seminar | 20% |
| Testet (kolokviumet) | 60% |
| Provimi final | 60% |

*(Shënim: përqindjet siç janë të dhëna në syllabus nuk mblidhen në mënyrë të pastër në 100% — kjo është ashtu siç del në dokumentin zyrtar, jo gabim imi. Në praktikë, testet/kolokviumet dhe provimi final janë pjesa dërrmuese e notës, kështu që ky plan fokusohet mbi to.)*

## 2. Formati i Provimeve — çfarë duhet të prisni

Nga shqyrtimi i **6 provimeve/kolokviumeve të kaluara** (Qershor 2023, Shkurt 2024 — dy versione, Nëntor 2024, Maj 2025), formati është shumë i qëndrueshëm:

- **Kohëzgjatja:** 75–90 minuta.
- **Struktura tipike:** 5–8 "Detyra" (detyra/pyetje), me pikë të ndryshme (1–20 pikë/detyrë).
- **Ndalohet përdorimi i funksioneve/metodave built-in** për kërkim/sortim (p.sh. `.sort()`, `Array.includes()`, `Math.sqrt()`) — duhet implementim manual!
- **Llojet e pyetjeve që përsëriten çdo provim:**
  1. **Problem kodimi (coding) me divide-and-conquer** — shpesh "katrori magjik" (magic square) ose probleme të ngjashme.
  2. **Problem kodimi me stringje/struktura të dhënash** — grupimi i anagrameve, verifikimi i kllapave të balancuara (valid parentheses), parsimi i formulave kimike (atom counting).
  3. **Analiza e kompleksitetit kohor/hapësinor** për fragmente kodi të dhëna (2-4 funksione, zakonisht nested loops, rekursion, ose kërkim/sortim).
  4. **Krahasimi teorik i dy algoritmeve** (p.sh. Shell vs Insertion Sort, QuickSort vs MergeSort, KMP vs Boyer-Moore, Winograd vs Strassen).
  5. **Master Theorem / relacione rekurence** — kthimi i një relacioni në formë të mbyllur, ose identifikimi i rastit të Master Teoremës.
  6. **Krahasimi i shpejtësisë së rritjes** të funksioneve (n!, 2^n, n², n log n, etj.) ose gjetja e vlerës `k` minimale për të cilën një funksion bie në një klasë të caktuar O(n^k).
  7. **Algoritme grafesh** — Kruskal/Prim, me pyetje "tregoni hapat e gjetjes së PSHM-së" + kompleksiteti.
  8. **String matching** — aplikim manual i KMP ose Boyer-Moore (bad character rule, good suffix rule) mbi tekst/mostër të dhënë.
  9. **Koncepte teorike** — P, NP, NP-complete, Backtracking (shpjegim konceptesh).
  10. **Horner's Method** — implementim.

➡️ **Shiko skedarin `11_Pyetje_nga_Provimet_e_Kaluara.md` për pyetjet e plota, verbatim, nga çdo provim i shqyrtuar.** Ky është ndoshta dokumenti më i vlefshëm në këtë folder — praktikoni këto lloj problemesh drejtpërdrejtë.

## 3. Struktura e Ligjëratave (15 javë, sipas syllabusit zyrtar)

| Java | Tema | Skedari përkatës i shënimeve |
|---|---|---|
| 1 | Dizajnimi i algoritmeve, analiza bazë, klasat hyrëse | `01_Analiza_e_Algoritmeve_Bazat.md` |
| 2 | Kompleksiteti kohor dhe hapësinor | `02_Kompleksiteti_Kohor_dhe_Hapesinor.md` |
| 3 | Kërkimi dhe selektimi, analiza e rasteve (best/avg/worst) | `04_Kerkimi_dhe_Selektimi.md` |
| 4 | Kërkimi sekuencial, kërkimi binar, selektimi | `04_Kerkimi_dhe_Selektimi.md` |
| 5 | Sortimi: insertion, bubble, selection sort | `05_Algoritmet_e_Sortimit.md` |
| 6 | Shell sort, radix sort, heap sort, merge sort | `05_Algoritmet_e_Sortimit.md` |
| 7 | Quicksort, polyphase merge sort | `05_Algoritmet_e_Sortimit.md` |
| 8 | Algoritme numerike: polinome, matrica, ekuacione lineare | `06_Algoritmet_Numerike.md` |
| 9 | String matching: KMP, Boyer-Moore, approximate matching | `07_Perputhja_e_Stringjeve.md` |
| 10 | Grafet: terminologjia, strukturat e të dhënave | `08_Algoritmet_e_Grafeve.md` |
| 11 | DFS dhe BFS | `08_Algoritmet_e_Grafeve.md` |
| 12 | MST (Prim, Kruskal), shtegu më i shkurtër (Dijkstra) | `08_Algoritmet_e_Grafeve.md` |
| 13 | Algoritme jodeterministike, NP, teknika tjera | `09_Algoritmet_Paralele_dhe_NP_Komplete.md` |
| 14 | Optimizimi: TSP, bin-packing, knapsack, graph coloring | `09_Algoritmet_Paralele_dhe_NP_Komplete.md` (§5) |
| 15 | Provimi final, sqarime | — |

**Shënim shtesë:** Materiali gjithashtu përfshin **Relacionet e Rekurrencës** (teleskopim, ekuacioni karakteristik, Master Theorem — `03_Relacionet_e_Rekurrences.md`) dhe **Algoritmet Paralele** (§1-4 e skedarit 09), të cilat janë pjesë e ligjëratave por jo domosdo të etiketuara në një javë specifike të syllabusit — ka gjasa fusha e Master Theorem/rekurrencave të mbulohet brenda javëve 1-2, dhe algoritmet paralele si temë shtesë ("teknika tjera") në javën 13.

## 4. Plan Studimi i Rekomanduar (nëse keni ~2 javë deri provimi)

**Java/Ditët 1-2 — Themelet (kritike, gjithçka tjetër ndërtohet mbi to):**
- Big-O, Big-Θ, Big-Ω — definicionet formale dhe si t'i dallosh (skedari 01, §7)
- Analiza e kompleksitetit kohor për algoritme iterative (nested loops) — skedari 02, §1
- Analiza e kompleksitetit kohor për algoritme rekursive (back-substitution, recursion tree) — skedari 02, §2
- **Master Theorem** — mësoje formulën e plotë, praktiko të gjitha rastet (skedari 02, §3)
- Relacionet e rekurrencës në formë të mbyllur (skedari 03)
- **PRAKTIKO:** merr 3-4 fragmente kodi nga skedari 11 (pyetjet e kaluara) dhe analizo kompleksitetin e tyre pa ndihmë, pastaj kontrollo përgjigjen.

**Java/Ditët 3-4 — Kërkimi, Selektimi, Sortimi:**
- Kërkimi sekuencial vs binar (rasti më i keq, rasti mesatar) — skedari 04
- Krahasimi i plotë i të 7 algoritmeve të sortimit (Insertion, Bubble, Shell, Radix, Heap, Merge, Quick) — kompleksiteti best/avg/worst, stabiliteti, hapësira — skedari 05, §9 (tabela përmbledhëse)
- **Kushtëzim i rëndësishëm:** kupto PSE Quicksort ka rast më të keq O(n²) pikërisht kur lista është e sortuar, dhe pse Heapsort ka të njëjtin kompleksitet në të tri rastet.
- **PRAKTIKO:** shkruaj nga kujtesa pseudokodin e Insertion Sort, Merge Sort, Quick Sort, Heap Sort.

**Java/Ditët 5-6 — Algoritmet Numerike dhe String Matching:**
- Algoritmi i Euklidit (rekursiv dhe binar), numra primar, Sita e Erathostenit — skedari 06
- Vlerësimi i polinomeve: standard vs Horner vs koeficientë të preprocesuar — skedari 06, §2 (**Horner del shpesh në provim si detyrë kodimi!**)
- Shumëzimi i matricave: standard vs Winograd vs Strassen — skedari 06, §3 (**krahasimi Winograd/Strassen del shpesh në provim**)
- KMP (funksioni i dështimit) dhe Boyer-Moore (bad character + good suffix rule) — skedari 07 (**del në çdo provim të shqyrtuar**)
- **PRAKTIKO:** ndiqni manualisht KMP dhe Boyer-Moore mbi shembujt konkretë të dhënë në skedarin 07 dhe në pyetjet e provimeve (skedari 11).

**Java/Ditët 7-8 — Grafet:**
- Terminologjia (shkallë, komponentë, urë, pikë artikulimi) — skedari 08, §1
- DFS/BFS (pseudokodi, kompleksiteti O(N)) — skedari 08, §3
- **Prim dhe Kruskal** — hapat, pseudokodi, kompleksiteti O(E log E) — skedari 08, §4 (**del në ÇDO provim të shqyrtuar, me graf konkret për të gjurmuar hapat**)
- Dijkstra (shtegu më i shkurtër) — skedari 08, §5
- Euler path/circuit (teoremat, algoritmi i Fleury-t) — skedari 08, §6

**Java/Dita 9 — Programimi Dinamik, Paralelizmi, NP:**
- Shumëzimi i vargut të matricave (matrix chain multiplication) — skedari 06, §DP (shih edhe skedarin origjinal `download. (11).txt`)
- Konceptet: P, NP, NP-complete, reduktimi — skedari 09, Pjesa II (**"shpjegoni P, NP, NP-complete, Backtracking" del si pyetje teorike direkte**)
- Përmbledhje e shpejtë e PRAM/paralelizmit (më pak i rëndësishëm për provim bazuar në frekuencën në pyetjet e kaluara, por lexoje njëherë)

**Java/Dita 10 (dita para provimit) — Rishikim final:**
- Rilexo skedarin `11_Pyetje_nga_Provimet_e_Kaluara.md` nga fillimi në fund dhe zgjidh çdo problem pa shikuar zgjidhjen.
- Rishiko tabelën përmbledhëse të kompleksiteteve (skedari 05, §9).
- Rishiko Master Theorem edhe një herë (është formula më lehtë e harrueshme, por përdoret shpesh).
- Rishiko dallimet KMP vs Boyer-Moore, Winograd vs Strassen, Shell vs Insertion, QuickSort vs MergeSort — këto "krahasime teorike" janë pyetje të garantuara pothuajse çdo herë.

## 5. Çfarë keni praktikuar tashmë (nga kodi juaj Java "Ushtrime DAA")

Shiko skedarin `10_Ushtrime_te_Praktikuara_dhe_Kodi_Ekzistues.md` për detaje, por shkurtimisht keni tashmë kod pune/koment për: analizën Big-O, kërkimin sekuencial, kërkimin binar (i komentuar), krahasimin e kompleksitetit të sortimeve, Radix Sort (koncept, por keni shënuar pasiguri mbi hapësirën), problemin DP "katrori më i madh 0-sh" (Maximal Square), dhe problemin "CPU Task Scheduler" (formulë greedy). Këto janë pika të mira fillestare — rilexoji këto skedarë përpara se të kaloni te temat e reja, sepse tregojnë çka keni kuptuar tashmë praktikisht.

## 6. Lista e Skedarëve në këtë Folder

1. `00_PLANI_I_STUDIMIT.md` — ky skedar (përmbledhje, plan, format provimi)
2. `01_Analiza_e_Algoritmeve_Bazat.md` — hyrje, klasat hyrëse, Big-O/Θ/Ω, background matematik, divide & conquer
3. `02_Kompleksiteti_Kohor_dhe_Hapesinor.md` — analiza e detajuar iterative/rekursive, Master Theorem, krahasimi i shpejtësisë së rritjes, kompleksiteti hapësinor
4. `03_Relacionet_e_Rekurrences.md` — teleskopimi, ekuacioni karakteristik, Fibonacci, rekurrencat lineare
5. `04_Kerkimi_dhe_Selektimi.md` — kërkimi sekuencial/binar, gjetja e K-birit më të madh
6. `05_Algoritmet_e_Sortimit.md` — 7 algoritme sortimi + external polyphase merge sort + tabelë krahasuese
7. `06_Algoritmet_Numerike.md` — polinome (Horner), shumëzimi i matricave (Winograd/Strassen), ekuacione lineare (Gauss-Jordan), GCD/Euklidi, numra primar, aritmetika modulare, matrix chain multiplication (DP)
8. `07_Perputhja_e_Stringjeve.md` — brute-force, KMP, Boyer-Moore, approximate matching (DP)
9. `08_Algoritmet_e_Grafeve.md` — terminologjia, DFS/BFS, MST (Prim/Kruskal), Dijkstra, Euler path/circuit
10. `09_Algoritmet_Paralele_dhe_NP_Komplete.md` — PRAM, algoritme paralele, P/NP/NP-complete, probleme optimizimi (TSP, knapsack, bin-packing, graph coloring)
11. `10_Ushtrime_te_Praktikuara_dhe_Kodi_Ekzistues.md` — analizë e kodit tuaj ekzistues Java
12. `11_Pyetje_nga_Provimet_e_Kaluara.md` — **pyetje verbatim nga 6 provime/kolokviume të kaluara** (2023-2025)

---

*Shënim: Ky material është ndërtuar duke skanuar dhe përpunuar automatikisht të gjitha PDF-të e ligjëratave, imazhet e provimeve të kaluara, dhe kodin ekzistues në këtë folder. Kontrolloni gjithmonë me profesorin/asistentin nëse ka ndryshime në syllabus apo material shtesë të pashpërndarë ende.*
