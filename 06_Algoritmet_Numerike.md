# 06 — Algoritmet Numerike (Numerical Algorithms)

*(Java 8 e syllabusit + programimi dinamik i shumëzimit të matricave)*

---

## 1. Vlerësimi i Polinomeve (Polynomial Evaluation)

$$p(x) = a_nx^n + \dots + a_1x + a_0$$

### Standard
```
result = a[0] + a[1]*x; xPower = x
for i = 2 to n do xPower *= x; result += a[i]*xPower
```
→ **2N−1 shumëzime, N mbledhje**

### Metoda e Horner-it *(del shpesh si detyrë kodimi në provim!)*
$$p(x) = (\dots((a_nx + a_{n-1})x + a_{n-2})x + \dots)x + a_0$$
```
result = a[n]
for i = n-1 down to 0 do result = result*x + a[i]
```
→ **N shumëzime, N mbledhje** (gjysma e shumëzimeve krahasuar me standardin!)

### Koeficientë të Preprocesuar (edhe më efikas, por i ndërlikuar)
- Faktorizon p(x) në dy polinome të shkallës më të ulët: p(x) = (xʲ+b)·q(x) + r(x), j=2^(k-1)
- **N/2 + lg N shumëzime, (3N−1)/2 mbledhje** — më pak shumëzime, më shumë mbledhje (trade-off që zakonisht ia vlen sepse shumëzimi është më "i shtrenjtë").

| Metoda | Shumëzime | Mbledhje |
|---|---|---|
| Standarde | 2N−1 | N |
| Horner | **N** | N |
| Koeficientë preprocesuar | N/2+lg N | (3N−1)/2 |

---

## 2. Shumëzimi i Matricave (Matrix Multiplication)

- I mundur nëse #kolona(A) = #rreshta(B). **Jo komutativ**: AB ≠ BA.
- **Standard**: matrica (a×b)·(b×c) → **a·b·c shumëzime, a·(b−1)·c mbledhje** → **O(N³)**.

### Algoritmi i Winograd-it
- Faktorizon produktin pikë (dot product) për të lejuar **paraprocesim** të termeve që varen vetëm nga rreshti/kolona.
- Redukton shumëzimet në **~abc/2** (gjysma), me rritje të mbledhjeve.
- **Ende O(N³)**, por me konstante më të vogël.

### Algoritmi i Strassen-it *(krahasimi Winograd vs Strassen del shpesh në provim!)*
- Funksionon vetëm mbi matrica **katrore** (zgjerohen me zero nëse jo).
- 7 formula të pazakonshme për shumëzim 2×2 (M₁...M₇), që **NUK supozojnë komutativitet** → mund të aplikohen **rekursivisht** mbi nën-matrica.
- Për 2×2: **7 shumëzime, 18 mbledhje** (dukshëm keq lokalisht, por fitimprurës rekursivisht).
- **Kompleksiteti asimptotik: O(N^2.81)** (2.81 = log₂7) — **i pari algoritëm më i shpejtë se O(N³)**.
- Në praktikë përdoret rrallë (bookkeeping rekursiv kompleks).

**Dallimi kryesor Winograd vs Strassen**: Winograd mbetet **O(N³)** por me konstante ~gjysmë (fiton nëpërmjet paraprocesimit të termave të përsëritur në produktin pikë); Strassen arrin **rend më të ulët asimptotik O(N^2.81)** duke ndryshuar vetë algoritmin e ndarjes rekursive (7 formula në vend të 8 shumëzimeve standarde për blloqe 2×2), me çmimin e më shumë mbledhjeve dhe kompleksitet të lartë implementimi.

| Metoda | Rendi |
|---|---|
| Standarde | O(N³) |
| Winograd | O(N³), konstante ~½ |
| Strassen | **O(N^2.81)** |

---

## 3. Sistemet e Ekuacioneve Lineare

### Metoda e Zëvendësimit — teorikisht e thjeshtë, por e vështirë për programim, rritet me numrin e ekuacioneve.

### Metoda Gauss-Jordan (Eliminimi Gaussian)
1. Matrica e zgjeruar (N rreshta, N+1 kolona).
2. Pjesëto rreshtin me pivotin e tij (bëje 1), zbrit shumëfishe nga rreshtat e tjerë (bëje kolonën [1,0,0,...]ᵀ).
3. Përsërit për çdo rresht deri sa N kolonat e para = matrica identitet.
4. Kolona e fundit = zgjidhja.

**Probleme praktike**: **gabimet e rrumbullaksimit** (mund të përhapen/kombinohen); **singulariteti** (rreshta linearisht të varur → rresht 0 → ndarje me zero).

---

## 4. Numra Primar, GCD, Aritmetika Modulare

- **Teorema Fundamentale e Aritmetikës**: çdo n>1 shkruhet unik si prodhim primarësh.
- **Sita e Erathostenit**: shëno shumëfishat e çdo primi të gjetur, duke filluar nga katrori i tij, deri √n.
- **GCD** (Pjesëtuesi më i Madh i Përbashkët): gcd(18,30)=6; gcd(0,20)=20; gcd(-21,49)=7. **Relativisht primar** nëse gcd(a,b)=1.
- **Modulo**: r = a mod n ⟺ a = r+kn.
- **Fakti kyç**: gcd(a,b) = gcd(b, a mod b) — baza e **Algoritmit të Euklidit**.

### Algoritmi i Euklidit (rekursiv)
```
EuclidGCD(a, b)
  if b = 0 then return a
  else return EuclidGCD(b, a mod b)
```
- Shembull: gcd(412,260)=4, sekuenca: 412,260,152,108,44,20,4.
- **Kompleksiteti: O(log max(a,b))** — vërtetim: aᵢ₊₂ ≤ ½aᵢ (zvogëlim eksponencial në 2 raunde) → numri max i thirrjeve = 1+2log(max(a,b)).

### Algoritmi Binar i Euklidit (më i shpejtë në kompjuterë)
```
EuclidBinaryGCD(a,b)
  if a=0 then return b
  if b=0 then return a
  if a,b të dyja çift → return 2*EuclidBinaryGCD(a/2,b/2)
  if a çift, b tek → return EuclidBinaryGCD(a/2,b)
  if a tek, b çift → return EuclidBinaryGCD(a,b/2)
  përndryshe (të dyja tek) → return EuclidBinaryGCD(|a-b|/2, min(a,b))
```

### Inverzet Multiplikative
- x·y mod n = 1 ⟹ y = x⁻¹ (invers multiplikativ i x në Zₙ).
- **Nëse n është primar** (p.sh. 11, 13) → **çdo mbetje jo-zero ka invers**.
- **Nëse n është i përbërë** (p.sh. 12) → **jo të gjitha** vlerat kanë invers (vetëm 1, 5, 7, 11 për n=12).
- Aplikim: enkriptim (shumëzo me x) / dekriptim (shumëzo me x⁻¹) mod n.

---

## 5. Programimi Dinamik: Shumëzimi i Vargut të Matricave (Matrix Chain Multiplication)

**Problemi:** për të shumëzuar një varg matricash A₁×A₂×...×Aₙ, gjej renditjen (kllapëzimin) optimal që minimizon numrin total të shumëzimeve skalare.

- **Qasja Naive**: provo të gjitha kombinimet → **eksponenciale**.
- **Programimi Dinamik**: 
  1. **Vërteto nënstrukturën optimale**: nëse Aᵢ...ⱼ ka zgjidhje optimale e kllapëzuar si (Aᵢ...ₖ)(Aₖ₊₁...ⱼ), atëherë secila nënpjesë duhet gjithashtu optimale (vërtetim me kontradiktë).
  2. **Formula rekursive**: M[i,j] = minimumi mbi të gjitha k (i≤k<j) të: **M[i,k] + M[k+1,j] + (kostoja e shumëzimit të rezultateve në k)**
  3. Mbush tabelën M duke filluar nga diagonalja (i=j, kosto 0) drejt jashtë.

**Shembull numerik** (nga ligjërata, varg 5 matricash): minimumi i gjetur = **1344 operacione shumëzimi** (kundrejt qasjes naive që do të ishte shumë më e shtrenjtë, ~eksponenciale).

*(Ky është shembulli klasik i programimit dinamik në kurs — nëse provimi kërkon "programim dinamik", ky ose problemi i "katrorit më të madh 0-sh" nga ushtrimet tuaja janë shembujt kryesorë referencë.)*
