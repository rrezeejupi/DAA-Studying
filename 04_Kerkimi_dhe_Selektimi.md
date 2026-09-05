# 04 — Kërkimi dhe Selektimi (Search and Selection Algorithms)

*(Java 3-4 e syllabusit)*

---

## 1. Koncepte Themelore

- **Kërkimi (Search)** — gjetja e një vlere (çelës/key) në një **listë (array)**, indeksuar 1..N.
- **E pasortuar** vs **e sortuar** — nëse lista është e pasortuar, kërkimi sekuencial është i vetmi opsion.
- Konvencion: nëse caku (target) s'gjendet, algoritmi kthen **0**; supozohet se vlerat e çelësit janë **unike**.
- **Problemi i selektimit**: gjetja e elementit që plotëson një kriter (p.sh. vlera e K-të më e madhe/mesatarja/mediana).

---

## 2. Kërkimi Sekuencial (Sequential/Linear Search)

```
SequentialSearch( list, target, N )
  for i = 1 to N do
    if (target = list[i]) return i
  end for
  return 0
```

- **Rasti më i keq**: caku në pozitën e fundit OSE caku s'ekziston → **N krahasime → O(N)**.
- **Rasti mesatar** (caku gjithmonë në listë): T_avg = (1+2+...+N)/N = **(N+1)/2**
- **Rasti mesatar** (duke përfshirë mundësinë e mos-gjetjes, N+1 mundësi të barabarta): rritet vetëm ~½ krahasim shtesë → i parëndësishëm për N të mëdha.
- **T_avg(N) = Θ(N)**

---

## 3. Kërkimi Binar (Binary Search)

Kërkon **listë të sortuar**. Strategji **Divide and Conquer**.

```
BinarySearch( list, target, N )
  start = 1; end = N
  while start ≤ end do
    middle = (start + end) / 2
    if list[middle] = target then return middle
    else if list[middle] < target then start = middle + 1
    else end = middle - 1
  end while
  return 0
```

- **Rasti më i keq**: për N=2^k-1, maksimumi **k = log₂(N+1)** kalime → **O(log N)**
- **Shembull**: N=1,000,000 → maksimumi <20 krahasime!
- **Rasti mesatar**: përmes pemës së vendosjes (decision tree) → **Θ(log N)**. Shembull numerik: N=1,048,575 (2²⁰-1) → ~19 krahasime (caku në listë) ose ~19.5 (duke përfshirë mos-gjetjen).

---

## 4. Selektimi — Gjetja e K-birit më të Madh (Kth Largest)

### Qasje Naive #1: Sortim i plotë + merr pozitën K — joefikase, punë e tepërt.

### Qasje Naive #2: FindKthLargest (gjetje e përsëritur e max-it)
```
FindKthLargest( list, N, K )
  for i = 1 to K do
    largest = list[1]; largestLocation = 1
    for j = 2 to N-(i-1) do
      if list[j] > largest then largest=list[j]; largestLocation=j
    end for
    Swap( list[N-(i-1)], list[largestLocation] )
  end for
  return largest
```
- **Kompleksiteti: O(K·N)** — nëse K>N/2, kërko (N-K)-birin më të vogël në vend (simetri).

### Qasja Efikase: Quickselect (bazuar në Partition)
```
KthLargestRecursive( list, start, end, K )
  if start < end then
    Partition( list, start, end, middle )
    if middle = K then return list[middle]
    else if K < middle then return KthLargestRecursive( list, middle+1, end, K )
    else return KthLargestRecursive( list, start, middle-1, K-middle )
```
- Duke supozuar ndarje ~në gjysmë mesatarisht: N + N/2 + N/4 + ... ≈ **2N krahasime → O(N)**, i **pavarur nga K**.

---

## 5. Tabela Përmbledhëse

| Algoritmi | Kërkon listë të sortuar? | Rasti më i keq | Rasti mesatar |
|---|---|---|---|
| Kërkimi Sekuencial | Jo | O(N) | Θ(N) ≈ (N+1)/2 |
| Kërkimi Binar | Po | O(log N) | Θ(log N) |
| FindKthLargest (naiv) | Jo | O(K·N) | O(K·N) |
| KthLargestRecursive (quickselect) | Jo | O(N²) | **O(N)** (~2N, i pavarur nga K) |

**Shënim provimi:** dallimi ndërmjet **kufirit të epërm** (varet nga vetë problemi) dhe **rastit më të keq** (varet nga algoritmi specifik) — për kërkimin sekuencial, të dyja përputhen (=N), por kjo dallesë konceptuale mund të pyetet teorikisht.
