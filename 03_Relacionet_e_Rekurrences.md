# 03 — Relacionet e Rekurrencës (Recurrence Relations)

*(Plotëson skedarin 02, §2-3 — këtu janë teknikat "manuale" për të kthyer një relacion rekurence në formë të mbyllur, kur Master Theorem-i NUK aplikohet, p.sh. T(n)=c·T(n-1)+f(n).)*

---

## 1. Çka është një Relacion Rekurence

Përkufizime rekursive i kemi parë tashmë te: seritë numerike, Fibonacci, funksionet e plota, Algoritmi i Euklidit, koeficientët binomialë, bashkësitë/vargjet e stringjeve.

**Dy forma ekuivalente të shprehjes:**
- Forma me raste bazë të veçanta (T(0)=..., T(1)=..., pastaj formula e përgjithshme).
- Forma me kusht (`if n≤k then ... else ...`).

---

## 2. Metoda e Teleskopimit / Zëvendësimit të Përsëritur (Back-Substitution)

**Shembull 1**: aₙ = 2aₙ₋₁, a₀ = 3
- Zëvendëso në mënyrë të përsëritur: aₙ = 2(2aₙ₋₂) = 4aₙ₋₂ = 8aₙ₋₃ = ... = 2ᵏaₙ₋ₖ
- Kur k=n: aₙ = 2ⁿ·a₀ = **3·2ⁿ**

**Shembulli klasik — Kullat e Hanoit (Towers of Hanoi)**: M(n) = 2M(n-1)+1, M(1)=1
- Zgjidhje me teleskopim: M(n) = 2ⁿ⁻¹·M(1) + (2ⁿ⁻¹-1) = 2ⁿ⁻¹ + 2ⁿ⁻¹ - 1 = **2ⁿ - 1**
- **M(n) ∈ Θ(2ⁿ)** — "Katastrofë" — rritje eksponenciale (paralajmërim klasik në ligjëratë: nëse relacioni juaj del kështu, algoritmi juaj do jetë JASHTËZAKONISHT i ngadalshëm për n të mëdha).

---

## 3. Ekuacioni Karakteristik (Characteristic Equation) — Rekurrencat Lineare Homogjene

**Përkufizim:** rekurrencë lineare homogjene e shkallës k:
$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k}$$

**Ekuacioni karakteristik:** $r^k - c_1 r^{k-1} - \dots - c_k = 0$

**Teoremë kyçe:** nëse `r` është rrënjë e ekuacionit karakteristik, atëherë `rⁿ` plotëson relacionin e rekurrencës. Zgjidhja e përgjithshme = kombinim linear i fuqive të rrënjëve.

### Shembulli klasik: Fibonacci
- Relacioni: aₙ = aₙ₋₁ + aₙ₋₂
- Ekuacioni karakteristik: **r² = r + 1** → rrënjët: $r_{1,2} = \frac{1 \pm \sqrt{5}}{2}$ (numri i artë/golden ratio!)
- Zgjidhja e përgjithshme: aₙ = A·r₁ⁿ + B·r₂ⁿ
- Duke zgjidhur për A, B me kushtet fillestare a₀=0, a₁=1: **A = 1/√5, B = -1/√5** → **Formula e Binet-it**

### Shembuj të tjerë të zgjidhur

**aₙ = aₙ₋₁ + 2aₙ₋₂** → ekuacioni karakteristik r² - r - 2 = 0 → rrënjët **r=2, r=-1** → zgjidhja: **aₙ = 3·2ⁿ - (-1)ⁿ**

**aₙ = -aₙ₋₁ + 4aₙ₋₂ + 4aₙ₋₃** → tri rrënjë reale: **r = -1, -2, 2**

**Rrënjë e përsëritur** (shkallë 2): (r-3)² = 0 → rrënja r=3 me shumëfishmëri 2 → zgjidhja ka formën: **aₙ = (α₀ + α₁n)·3ⁿ** (shto faktor `n` për çdo shumëfishmëri shtesë)

**Rrënjë të përsëritura shkallë 4**: (r²-4)² = (r-2)²(r+2)² = 0 → r=2 (shumëfishmëri 2), r=-2 (shumëfishmëri 2) → zgjidhja: (α₀+α₁n)2ⁿ + (β₀+β₁n)(-2)ⁿ

---

## 4. Rekurrencat Lineare Jo-Homogjene (Non-Homogeneous)

**Teorema 4:** zgjidhja e përgjithshme = **zgjidhja e veçantë (particular solution) bₙ** (forma e ngjashme me f(n), termin jo-homogjen) **+ zgjidhja homogjene hₙ** (nga ekuacioni karakteristik i pjesës homogjene).

### Shembuj të zgjidhur

**aₙ = aₙ₋₁ + aₙ₋₂ + 3n + 1**
- Pjesa homogjene: aₙ = aₙ₋₁ + aₙ₋₂ (Fibonacci-like)
- Zgjidhja e veçantë: forma polinomiale (pasi f(n)=3n+1 është lineare) — zëvendëso bₙ=An+B dhe zgjidh për A, B.
- Zgjidhja e plotë = bₙ + hₙ

**aₙ = 2aₙ₋₁ - aₙ₋₂ + 2ⁿ**
- Pjesa homogjene: r² - 2r + 1 = 0 → (r-1)²=0 → rrënjë e përsëritur r=1 → hₙ = (α₀+α₁n)·1ⁿ = α₀+α₁n
- Zgjidhja e veçantë: meqë f(n)=2ⁿ **NUK përputhet** me rrënjën homogjene (r=1≠2), provo bₙ = c·2ⁿ
- Zgjidhja e plotë = c·2ⁿ + α₀ + α₁n

**Kujdes (rregull i rëndësishëm):** nëse forma e propozuar për zgjidhjen e veçantë **përputhet** me një rrënjë të pjesës homogjene, duhet shumëzuar me `n` (ose `n²`, etj.) shtesë — njësoj si te rrënjët e përsëritura — përndryshe sistemi i ekuacioneve nuk zgjidhet (koeficientë të papërcaktuar).

---

## 5. Kur Përdoret Cila Metodë? (Vendimmarrja praktike)

| Forma e Relacionit | Metoda e Rekomanduar |
|---|---|
| T(n) = aT(n/b) + f(n) *(pjesëtim, jo zbritje)* | **Master Theorem** (skedari 02, §3) |
| T(n) = c·T(n-1) + f(n) *(zbritje me konstante)* | **Back-substitution / Teleskopim** (§2 këtu) ose **Ekuacioni Karakteristik** nëse f(n)=0 (homogjene) |
| T(n) = c₁T(n-1) + c₂T(n-2) + ... *(shumë terma, zbritje)* | **Ekuacioni Karakteristik** (§3-4 këtu) |
| T(n) = 2T(n/2) + n, etj. | **Master Theorem** ose **Recursion Tree** (skedari 02, §2) |

**Shembull konkret nga provimi** (shih skedarin 11): "Vendosni (shndërroni) relacionin vijues të rekurrencës në formë të mbyllur: T(n) = 3T(n-1) - 15, T(1) = 8" — kjo ËSHTË saktësisht rasti i **zbritjes me konstante** → përdor **back-substitution**:
```
T(n) = 3T(n-1) - 15
     = 3(3T(n-2)-15) - 15 = 9T(n-2) - 3·15 - 15
     = 27T(n-3) - 9·15 - 3·15 - 15
     ...
     = 3ᵏT(n-k) - 15(3^(k-1) + ... + 3 + 1)
```
Kur n-k=1 (k=n-1): T(n) = 3ⁿ⁻¹·T(1) - 15·(3^(n-1)-1)/(3-1) = 3ⁿ⁻¹·8 - 7.5·(3^(n-1)-1)
Thjeshto dhe merr formën e mbyllur në funksion vetëm të n (**pa T() në të djathtë**) — kjo është përgjigja e kërkuar.

---

## 6. Formula të Përdorura Shpesh (Reference)

- Shuma gjeometrike: Σ(i=0..k) rⁱ = (r^(k+1) - 1)/(r - 1), për r≠1
- Σ(i=1..n) i = n(n+1)/2
- Σ(i=0..n) 2ⁱ = 2^(n+1) - 1
- Numri i artë (golden ratio): φ = (1+√5)/2 ≈ 1.618
