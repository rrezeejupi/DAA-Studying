# 10 — Analiza e Ushtrimeve Ekzistuese (Kodi Juaj Java "Ushtrime DAA")

*Ky skedar rishikon kodin që keni shkruar tashmë në projektin "Ushtrime DAA", për t'ju treguar çka keni praktikuar dhe ku ka boshllëqe.*

---

## 1. `Detyra.java` — Numërimi i Shkronjave
Numëron shkronja të mëdha/vogla dhe zanore/bashkëtingëllore në një string të fiksuar ("random VarG"). Komenti përmend "+ binary search" por s'ka kod të kërkimit binar këtu ende — **rekomandim: shtoni implementimin e binary search këtu ose kontrolloni `Detyra2.java` ku e keni tashmë**.

## 2. `InsertionSort.java`
Insertion sort standard mbi `{1, 24, -5, 89}` (shih skedarin 05, §1 për pseudokodin/analizën teorike të plotë — kompleksiteti O(N²) rasti më i keq/mesatar, O(N) rasti më i mirë).

## 3. `ushtrimeKllkf/CPU_TaskScheduler.java`
`leastInterval(char[] tasks, int n)` — numëron frekuencën e detyrave, aplikon formulën greedy:
```java
Math.max(tasks.length, (maxFreq-1)*(n+1) + numMaxTasks)
```
Testuar me `{'A','A','A','B','B','B'}`, n=2. Ky është problem klasik greedy/matematikor (jo drejtpërdrejt nga syllabusi DAA, por i lidhur me analizën e kompleksitetit dhe të menduarit algoritmik — mirë praktikë shtesë).

## 4. `ushtrimeKllkf/Detyra1.java` — Kompleksiteti Kohor
Gjen vlerën maksimale në varg `{1,2,4,6,3,100,8,9,34,76}`; komenti thotë saktë: **kompleksiteti kohor O(n), hapësinor O(1)**. ✅ Korrekte — ky ushtrim mbulon direkt skedarin 01/02 (analiza bazë e kompleksitetit).

## 5. `ushtrimeKllkf/Detyra2.java` — Kërkimi Sekuencial dhe Binar + Krahasimi i Sorteve
- `kerkimSekuencial(int num)` mbi `{2,3,7,10,90}` — shiko skedarin 04, §2 për analizën e plotë teorike.
- Komentet gjurmojnë manualisht Binary Search për target=90, saktë identifikojnë **parakushtin (array i sortuar)** dhe kompleksitetin **O(log n) worst/avg, O(1) best** — ky është saktësisht rezultati teorik i skedarit 04, §3. ✅ Shumë mirë i kuptuar.
- **"Detyra 3"** (koment-only, pa kod): tabelë krahasimi Insertion/QuickSort/HeapSort/MergeSort — përputhet plotësisht me tabelën përmbledhëse të skedarit 05, §9. Nëse doni ta praktikoni më tej: **implementoni realisht** këto katër algoritme këtu (jo vetëm koment) — kjo do t'ju përgatiste direkt për pyetje kodimi provimi.

## 6. `ushtrimeKllkf/Detyra4.java` — Radix Sort (vetëm koment, klasë bosh)
Komentet përshkruajnë mekanikën e Radix Sort saktë, por **shënoni pasiguri mbi hapësirën** ("spe kuptoj" = "s'e kuptoj"). **Sqarim direkt nga skedari 05, §4**: kompleksiteti hapësinor i Radix Sort është **O(N) deri O(kN)** — jo O(1)! — sepse algoritmi ka nevojë për **kova (buckets)** shtesë (10 kova për shifra, ose 26+ për alfabet), të cilat kërkojnë hapësirë proporcionale me N (numrin e elementeve) shumëzuar me numrin e kovave, PLUS hapësirën për vetë kopjimin e elementeve nga/në kova gjatë çdo kalimi (2M herë lëvizje/element, M=gjatësia e çelësit). Kjo ndryshon nga sortet in-place (si Heapsort, O(1)) — Radix Sort **e blen shpejtësinë e tij lineare O(N) me koston e hapësirës shtesë të konsiderueshme**.

## 7. `ushtrimeKllkf/Detyra5.java` — DP: Katrori më i Madh i Qelizave të Lira (Maximal Square)
`findLargestSquare(int[][] matrix)` — zgjidhje DP funksionale për problemin klasik "katrori më i madh i 0-ave" (analog te LeetCode "Maximal Square"). **Ky është shembulli juaj kryesor praktik i Programimit Dinamik** — krahasojeni konceptualisht me shembullin e "Shumëzimit të Vargut të Matricave" në skedarin 06, §5: të dy përdorin **tabelë DP** dhe **nënstrukturë optimale** (zgjidhja e nënproblemit të madhësisë (i,j) ndërtohet nga nënproblemet më të vogla).
- **"Detyra 6"** (koment-only): përshkruan problemin e CPU Task Scheduler-it (i implementuar veç në `CPU_TaskScheduler.java`).

---

## Rekomandim për Praktikë Shtesë (bazuar në boshllëqet e gjetura)

1. **Implementoni realisht** algoritmet e sortimit që i keni vetëm si koment teorik (QuickSort, MergeSort, HeapSort) te `Detyra2.java`/`Detyra3` — kjo lidhet direkt me pyetjet e kodimit që dalin në provim (shih skedarin 11, ku "funksioni_3"/"funksioni_4"/"quickSort" janë dhënë si kod për analizë kompleksiteti — praktika e shkrimit të tyre nga zeri do t'ju ndihmojë të njihni menjëherë strukturën e tyre në provim).
2. **Shtoni Horner's Method** (skedari 06, §1) — del si detyrë implementimi e drejtpërdrejtë në të paktën 1 provim të shqyrtuar.
3. **Implementoni KMP ose Boyer-Moore** njëherë vetë (skedari 07) — që të mos jetë hera e parë ta shihni kodin gjatë provimit.
4. **Praktikoni Kruskal/Prim** mbi një graf të vogël të vizatuar me dorë (skedari 08, §4) — kjo është pothuajse e garantuar si pyetje.
5. **Shkruani sqrt() pa built-in** (binary search mbi hapësirën e përgjigjeve) — del si problem konkret në provimet e kaluara (shih skedarin 11).
