# 05 — Algoritmet e Sortimit (Sorting Algorithms)

*(Java 5-7 e syllabusit — kjo temë del pothuajse në ÇDO provim, si pyetje krahasimi teorik ose implementim.)*

---

## 0. Koncepte të Përgjithshme

- **Inversion** = çift elementesh jashtë renditjes së duhur. Lista në renditje krejt të kundërt ka **(N²−N)/2** inversione (maksimumi).
- Sortet O(N²) (bubble, insertion) heqin **1 inversion/krahasim**; sortet më efikase (Shellsort, Quicksort) mund të heqin **>1** inversion/krahasim.

---

## 1. INSERTION SORT (Rradhitja me Futje)

```
InsertionSort( list, N )
  for i = 2 to N do
    newElement = list[i]; location = i - 1
    while (location ≥ 1) and (list[location] > newElement) do
      list[location+1] = list[location]; location = location-1
    end while
    list[location+1] = newElement
  end for
```

| Rasti | Kompleksiteti |
|---|---|
| Më i miri | O(N) — lista tashmë e sortuar |
| Mesatar | **O(N²)** ≈ N²/4 |
| Më i keqi | **O(N²)** ≈ (N²−N)/2 — lista e kundërt |

**Stabil?** Po. **Hapësira**: O(1), in-place.

---

## 2. BUBBLE SORT

```
BubbleSort( list, N )
  numberOfPairs = N; swappedElements = true
  while swappedElements do
    numberOfPairs -= 1; swappedElements = false
    for i = 1 to numberOfPairs do
      if list[i] > list[i+1] then Swap(list[i],list[i+1]); swappedElements=true
    end for
  end while
```

| Rasti | Kompleksiteti |
|---|---|
| Më i miri | O(N) — N−1 krahasime, ndalet herët (flamuri) |
| Mesatar | O(N²) |
| Më i keqi | O(N²) |

**Stabil?** Po. **Hapësira**: O(1).

---

## 3. SHELL SORT

Zbuluesi: Donald L. Shell. Sort shumëkalimësh — trajton listën si nënlista të ndërthurura me **increment** (largësi) që zvogëlohet çdo kalim, deri increment=1 (=Insertion Sort standard).

```
Shellsort( list, N )
  passes = lg N
  while passes ≥ 1 do
    increment = 2^passes - 1
    for start = 1 to increment do InsertionSort(list, N, start, increment)
    passes -= 1
  end while
```

**Kompleksiteti varet nga sekuenca e increment-eve** — s'ka sekuencë optimale e njohur:

| Sekuenca | Rendi (worst-case) |
|---|---|
| 2^k−1 (klasike) | O(N^(3/2)) |
| hⱼ=(3ʲ−1)/2 | O(N^(3/2)) |
| 2ⁱ3ʲ zbritëse | **O(N(lg N)²)** — më e mirë, por overhead më i lartë |

**Stabil?** JO. **Hapësira**: O(1).

**Dallimi kryesor nga Insertion Sort** *(pyetje e shpeshtë provimi)*: Shell Sort bën shumë "pak-kalime paraprake" mbi nënlista të shpërndara (increment>1) përpara kalimit final me increment=1 (Insertion Sort i plotë), duke e "para-sortuar" pjesërisht listën dhe kështu duke reduktuar numrin e vërtetë të lëvizjeve krahasuar me Insertion Sort të pastër mbi listë krejt të pasortuar — prandaj Shellsort ka kompleksitet më të mirë (O(N^1.5) ose O(N(logN)²)) se Insertion Sort i pastër (O(N²)).

---

## 4. RADIX SORT

Sort shumëkalimësh që **NUK krahason** çelësat — përdor "kova" (buckets) sipas shifrave, nga shifra me peshë më të vogël te ajo më e madhe.

```
RadixSort( list, N )
  shift = 1
  for loop = 1 to keySize do
    for entry = 1 to N do
      bucketNumber = (list[entry].key / shift) mod 10
      Append( bucket[bucketNumber], list[entry] )
    end for
    list = CombineBuckets()
    shift = shift * 10
  end for
```

| Metrikë | Vlera |
|---|---|
| Kohor | M shifra, N çelësa → O(M·N); M konstante → **O(N)** linear |
| Hapësinor | **I lartë**: 10N (numerikë) deri 26N (alfabetikë) me vargje; 2N-4N me lista të lidhura |

**Stabil?** Po (i domosdoshëm për korrektësi). **Kompromis kyç**: shpejtësi O(N) por hapësirë e shtrenjtë.

---

## 5. HEAPSORT

**Heap (Pirg)**: pemë binare e plotë, max-heap (rrënja ≥ fëmijët). Nyja `i` ka fëmijë te `2i` dhe `2i+1`.

```
FixHeap( list, root, key, bound )
  vacant = root
  while 2*vacant ≤ bound do
    largerChild = 2*vacant
    if (largerChild<bound) and (list[largerChild+1]>list[largerChild]) then largerChild += 1
    if key > list[largerChild] then break
    else list[vacant] = list[largerChild]; vacant = largerChild
  end while
  list[vacant] = key
```

```
# Ndërtimi i Heap-it (Build-Heap) — O(N), jo O(N log N) siç mund të supozohet!
for i = N/2 down to 1 do FixHeap( list, i, list[i], N )
# Nxjerrja e elementeve — O(N log N)
for i = N down to 2 do
  max = list[1]; FixHeap( list, 1, list[i], i-1 ); list[i] = max
```

| Rasti | Kompleksiteti |
|---|---|
| Më i miri | **O(N log N)** |
| Mesatar | **O(N log N)** |
| Më i keqi | **O(N log N)** |

**I vetmi sort ku best=avg=worst.** **Stabil?** JO. **Hapësira**: **O(1)** — in-place, pa pemë eksplicite.

---

## 6. MERGE SORT

Rekursiv, "puna bëhet gjatë kthimit lart" (bottom-up gjatë kthimit të rekursionit).

```
MergeSort( list, first, last )
  if first < last then
    middle = (first+last)/2
    MergeSort(list, first, middle)
    MergeSort(list, middle+1, last)
    MergeLists(list, first, middle, middle+1, last)
```

**Relacionet e rekurrencës:**
- Rasti më i keq: W(N) = 2W(N/2) + N − 1 → **O(N log N)** (zgjidhur me zëvendësim të përsëritur)
- Rasti më i mirë: B(N) = 2B(N/2) + N/2 → **O(N log N)**

| Rasti | Kompleksiteti |
|---|---|
| Të gjitha rastet | **O(N log N)** (garantuar) |

**Stabil?** Po (`<` jo `≤` në krahasim). **Hapësira**: **O(N) shtesë** — kërkon listë ndihmëse `result` — kjo është "problemi" kryesor i tij.

---

## 7. QUICKSORT

```
Quicksort( list, first, last )
  if first < last then
    pivot = PivotList(list, first, last)
    Quicksort(list, first, pivot-1)
    Quicksort(list, pivot+1, last)

PivotList( list, first, last )
  PivotValue = list[first]; PivotPoint = first
  for index = first+1 to last do
    if list[index] < PivotValue then
      PivotPoint += 1; Swap(list[PivotPoint], list[index])
  Swap(list[first], list[PivotPoint])
  return PivotPoint
```

| Rasti | Kompleksiteti | Kushti |
|---|---|---|
| Më i miri | O(N log N) | Pivot ndan në gjysma të barabarta |
| Mesatar | **O(N log N)** | Rasti tipik |
| Më i keqi | **O(N²)** | **Lista tashmë e sortuar** (ose pivot gjithmonë ekstrem) |

**Ironi kyçe:** rasti "i lehtë" për njeriun (listë tashmë e sortuar) është pikërisht **rasti më i keq** për Quicksort naiv (me pivot=elementi i parë).

**Stabil?** JO. **Hapësira**: O(lg N) mesatarisht (stack rekursioni), O(N) rasti më i keq.

**Dallimi QuickSort vs MergeSort** *(pyetje e shpeshtë provimi)*: QuickSort bën punën (ndarjen sipas pivotit) **para** rekursionit (top-down, "puna në zbritje"), MergeSort e bën **pas** (bottom-up, "puna në ngjitje", gjatë bashkimit). QuickSort sortim **in-place** (pa hapësirë shtesë domethënëse); MergeSort kërkon **O(N) hapësirë shtesë**. QuickSort ka rast më të keq O(N²) (i ndjeshëm ndaj hyrjeve të renditura); MergeSort ka **O(N log N) garantuar në çdo rast**.

---

## 8. SORTI I JASHTËM POLIFAZOR (External Polyphase Merge Sort)

Për të dhëna shumë të mëdha (s'hyjnë në memorie) — punohet me fajlla në disk.

- **Faza 1 — CreateRuns**: lexo S rekorde, sortoji (intern), shkruaj alternativisht në fajllin A/B → **R=⌈N/S⌉ "rende" (runs)**.
- **Faza 2 — PolyphaseMerge**: bashko çift-çift, madhësia e rendeve dyfishohet çdo kalim → **⌈lg R⌉ kalime bashkimi**.
- **Kompleksiteti**: Krijimi O(N lg S); Bashkimi O(N lg R); **Lexime blloqesh totale: O(R lg R)**.
- Kostoja reale dominohet nga **I/O në disk**, jo nga krahasimet logjike.

---

## 9. TABELA PËRMBLEDHËSE — MEMORIZOJE PËR PROVIM

| Algoritmi | Best | Average | Worst | Hapësirë | Stabil? |
|---|---|---|---|---|---|
| **Insertion Sort** | O(N) | O(N²) | O(N²) | O(1) | ✅ Po |
| **Bubble Sort** | O(N) | O(N²) | O(N²) | O(1) | ✅ Po |
| **Shell Sort** | — | varet | O(N^1.5) deri O(N(lgN)²) | O(1) | ❌ Jo |
| **Radix Sort** | O(N) | O(N) | O(N) | O(N) deri O(kN) | ✅ Po |
| **Heapsort** | O(N lg N) | O(N lg N) | O(N lg N) | **O(1)** | ❌ Jo |
| **Merge Sort** | O(N lg N) | O(N lg N) | O(N lg N) | **O(N)** | ✅ Po |
| **Quicksort** | O(N lg N) | O(N lg N) | **O(N²)** | O(lg N) avg | ❌ Jo |

**5 pikat kyçe për t'i mbajtur mend:**
1. Heapsort = konsistenca e plotë (best=avg=worst=O(N log N)), in-place, jo-stabil.
2. Merge Sort = garanci e plotë O(N log N), por kushton O(N) hapësirë.
3. Quicksort = mesatarisht më i shpejti në praktikë, por rrezik O(N²) me hyrje të renditura.
4. Radix Sort = i vetmi jo bazuar në krahasime — O(N), por hapësirë e shtrenjtë.
5. Shellsort = kompleksiteti i tij VARET nga zgjedhja e sekuencës së increment-eve (kjo është unike ndër sortet).
