# 07 — Përputhja e Stringjeve (String Matching Algorithms)

*(Java 9 e syllabusit — KMP dhe Boyer-Moore dalin praktikisht në ÇDO provim të shqyrtuar, si aplikim manual mbi tekst/mostër konkrete.)*

---

## 1. Hyrje

- **T** (ose n) = gjatësia e tekstit; **P** (ose m/S) = gjatësia e mostrës (pattern).
- Përdorime: editorë teksti, spell-checkers, antivirus (kërkim "mostër bajtash").

---

## 2. Brute-Force / Naiv

```
BruteForceMatch(T, P)
  for i = 0 to n-m do
      j = 0
      while j<m and T[i+j]=P[j] do j += 1
      if j = m then return i
  return -1
```

- **Rasti më i keq: O(n·m)** — shembull i keq: T="aaa...a", P="aaah" (tipik në ADN/imazhe binare, jo në tekst natyral).
- **Në praktikë (tekst natyral): shumë më mirë** — mesatarisht ~n krahasime.
- **Problemi themelor**: "harron" informacionin e krahasimeve të mëparshme, rifillon nga zero.

---

## 3. Knuth-Morris-Pratt (KMP)

**Ideja**: kur ndodh mospërputhje, **mos u kthe prapa në tekst** — përdor **funksionin e dështimit (failure function)** të para-llogaritur nga vetë-ngjashmëria e mostrës.

**Funksioni i Dështimit F(j)**: gjatësia e prefiksit më të gjatë të P[0..j] që është edhe sufiks i P[1..j].

```
FunksioniDeshtimit(P)
  F[0] = 0; i=1; j=0
  while i < m do
      if P[i] = P[j] then F[i]=j+1; i+=1; j+=1
      else if j>0 then j = F[j-1]
      else F[i]=0; i+=1
```

```
KMPMatch(T, P)
  F = FunksioniDeshtimit(P); i=0; j=0
  while i < n do
    if T[i]=P[j] then
      if j=m-1 then return i-j
      else i+=1; j+=1
    else
      if j>0 then j = F[j-1]
      else i += 1
  return -1
```

### Shembull i punuar: P = "abacab" (indeksuar 0..5)

| j | P[0..j] | F(j) |
|---|---|---|
| 0 | a | 0 |
| 1 | ab | 0 |
| 2 | aba | 1 |
| 3 | abac | 0 |
| 4 | abaca | 1 |
| 5 | abacab | 2 |

**Kompleksiteti**: ndërtimi i F = **O(m)**; kërkimi = **O(n)** (kurrë s'kthehet prapa: max 2n hapa). **Total: O(n+m)** — optimal.

**Përparësi**: kurrë s'kthehet prapa në T → i mirë për streaming (rrjetë, fajlla të mëdhenj). **Dobësi**: më pak efikas me alfabet të madh (mospërputhjet priren të ndodhin herët).

---

## 4. Boyer-Moore (BM)

**Dallimi themelor**: krahason mostrën **djathtas-majtas**; përdor **2 heuristika**:
1. Looking-glass — krahasim djathtas-majtas.
2. Character-jump (bad character rule) — kur mospërputhet T[i]=c: nëse c∈P, zhvendos që rastisja e fundit e c në P të vijë te T[i]; përndryshe zhvendos gjithë P.

**Funksioni i Rastisjes së Fundit L(c)**: indeksi më i madh i ku P[i]=c, ose -1.

```
BoyerMooreMatch(T, P, Σ)
  L = LastOccurrenceFunction(P, Σ); i=m-1; j=m-1
  repeat
    if T[i]=P[j] then
      if j=0 then return i
      else i-=1; j-=1
    else
      l = L[T[i]]; i += m - min(j, 1+l); j = m-1
  until i > n-1
  return -1
```

### Bad Character Rule + Good Suffix Rule (rregulli i plotë, nga materiali)
- **slide[ch]** (bad-character): sa duhet zhvendosur për të radhitur karakterin e papërputhur me rastisjen e tij të fundit në P.
- **jump[i]** (good-suffix): bazuar mbi sekuenca të fundit të P që përsëriten diku tjetër në P.
- **Rregulli i vendimit: zhvendosje = MAX(slide[karakteri], jump[pozita])** (jo shuma!).

### Shembull klasik: "they" te "there they are"
- `y` vs `r` → `r` s'ekziston në mostër → zhvendos 4 pozita (gjithë mostra).
- `y` vs `h` → `h` ekziston → zhvendos 2 pozita.
- **Total: 6 krahasime** (kundrejt 13 te brute-force).

**Kompleksiteti**: ndërtimi bad-character O(m+alfabet); good-suffix deri O(m²) rasti më i keq; kërkimi rasti më i keq O(n·m), **por praktikisht (tekst natyral) krahason vetëm ~25-40% të karaktereve**. **BM është më i shpejti praktikisht** me alfabet të madh; i dobët me alfabet të vogël (binar/ADN).

**Dallimi kryesor KMP vs Boyer-Moore** *(pyetje e shpeshtë provimi)*:
| | KMP | Boyer-Moore |
|---|---|---|
| Drejtimi i krahasimit | Majtas → Djathtas | **Djathtas → Majtas** |
| Info e përdorur | Vetë-ngjashmëria e P (failure function) | Karakteri i tekstit që s'përputhet (bad-char) + sufikset e P (good-suffix) |
| Garanci teorike | O(n+m) gjithmonë | O(n·m) rasti më i keq, por sub-linear praktikisht |
| Më i mirë me | Alfabet të vogël, ripërsëritje në mostër | **Alfabet të madh** — kërcime më të mëdha |
| A kthehet prapa në T? | KURRË | Po (implicit, por lëviz gjithsesi djathtas nëpër T) |

---

## 5. Përshtatja e Përafërt e Stringjeve (Approximate Matching) — Programim Dinamik

**3 lloje gabimesh**: (1) zëvendësim karakteri, (2) fshirje (deletion), (3) shtim (insertion).

**Matrica `diffs[i,j]`** = numri minimal dallimesh për të përshtatur i karakteret e para të P me tekstin që përfundon në j.

**Rekurrenca**: diffs[i,j] = min i:
1. diffs[i-1,j-1] (+1 nëse P[i]≠T[j]) — zëvendësim/përputhje
2. diffs[i-1,j] + 1 — fshirje
3. diffs[i,j-1] + 1 — shtim

**Kompleksiteti: O(S·T)** — njësoj si brute-force, por trajton shumë më tepër raste. Hapësira mund të reduktohet në **O(S)** (vetëm 2 kolona njëkohësisht).

---

## 6. Tabela Përmbledhëse

| Algoritmi | Parapërpunim | Kërkimi | Total |
|---|---|---|---|
| Brute-Force | — | — | O(n·m) worst |
| **KMP** | O(m) | O(n) | **O(n+m)** — optimal |
| **Boyer-Moore** | O(m+alfabet) deri O(m²) | O(n·m) worst, sub-linear praktik | Më i shpejti praktikisht |
| Approx. Matching (DP) | — | O(S·T) | O(S·T) |
