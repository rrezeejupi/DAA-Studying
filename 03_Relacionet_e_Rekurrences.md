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

---

## 7. Shtojcë: 6 Shembuj Shtesë të Plotë (nga materiali i asistentit) — PRAKTIKOJINI!

*Këto janë nxjerrë nga një ligjëratë shtesë "Relacionet e Rekurrencës" (49 slide). I rëndësishëm: shembulli #2 (an=-an-1+4an-2+4an-3) është KONFIRMUAR si pyetje reale provimi (Provimi_DAA_Qershor-2, Detyra 2c — atje me a0=8, a1=6, a2=26 saktësisht si këtu)!*

### Shembulli 1 — Rrënjë reale të dallueshme (shkallë 2)
`an = an-1 + 2an-2`, `a0=2, a1=7`
```
Ekuacioni karakteristik: r² - r - 2 = 0 → (r+1)(r-2)=0 → r1=2, r2=-1
Zgjidhja e përgjithshme: an = α1·2ⁿ + α2·(-1)ⁿ
a0: α1+α2=2
a1: 2α1-α2=7
⟹ α1=3, α2=-1
```
**Përgjigja: an = 3·2ⁿ - (-1)ⁿ**

### Shembulli 2 — Rrënjë reale të dallueshme (shkallë 3) — ⭐ KONFIRMUAR NË PROVIM
`an = -an-1 + 4an-2 + 4an-3`, `a0=8, a1=6, a2=26`
```
Ekuacioni karakteristik: r³+r²-4r-4=0 → (r+1)(r+2)(r-2)=0 → r1=-1, r2=-2, r3=2
Zgjidhja e përgjithshme: an = α1(-1)ⁿ + α2(-2)ⁿ + α3·2ⁿ
Sistemi:
a0: α1+α2+α3=8
a1: -α1-2α2+2α3=6
a2: α1+4α2+4α3=26
⟹ α1=2, α2=1, α3=5
```
**Përgjigja: an = 2·(-1)ⁿ + (-2)ⁿ + 5·2ⁿ**

### Shembulli 3 — Rrënjë e përsëritur, shumëfishmëri 2
`an = 6an-1 - 9an-2`, `a0=1, a1=6`
```
Ekuacioni karakteristik: r²-6r+9=0 → (r-3)²=0 → r=3, shumëfishmëri 2
Zgjidhja: an = (β0+β1·n)·3ⁿ
a0: β0=1
a1: 3β0+3β1=6 ⟹ β1=1
```
**Përgjigja: an = 3ⁿ + n·3ⁿ**

### Shembulli 4 — Rrënjë e përsëritur, shumëfishmëri 3
`an = -3an-1 - 3an-2 - an-3`, `a0=1, a1=-2, a2=-1`
```
Ekuacioni karakteristik: r³+3r²+3r+1=0 → (r+1)³=0 → r=-1, shumëfishmëri 3
Zgjidhja: an = (β0+β1·n+β2·n²)(-1)ⁿ
Sistemi ⟹ β0=1, β1=3, β2=-2
```
**Përgjigja: an = (1+3n-2n²)(-1)ⁿ**

### Shembulli 5 — Dy rrënjë të përsëritura (shkallë 4)
`an = 8an-2 - 16an-4` (n≥4), `a0=1, a1=4, a2=28, a3=32`
```
Ekuacioni karakteristik: r⁴-8r²+16=0 → (r²-4)²=(r-2)²(r+2)²=0 → r1=2, r2=-2, secila shumëfishmëri 2
Zgjidhja: an = (β10+β11·n)·2ⁿ + (β20+β21·n)·(-2)ⁿ
Sistemi ⟹ β10=1, β11=2, β20=0, β21=1
```
**Përgjigja: an = (1+2n)·2ⁿ + n·(-2)ⁿ**

### Shembulli 6 — Jo-homogjene, f(n) eksponenciale që përputhet me rrënjë homogjene
`an = 2an-1 - an-2 + 2ⁿ`, `a0=1, a1=2`
```
Hapi 1 — zgjidhja e veçantë bn: provo bn=c·2ⁿ (por 2 s'përputhet me rrënjën homogjene r=1, kështu
   që s'nevojitet faktor shtesë n këtu — provo direkt):
   c·2ⁿ = 2(c·2ⁿ⁻¹) - c·2ⁿ⁻² + 2ⁿ ⟹ zgjidh për c ⟹ c=4
   bn = 4·2ⁿ

Hapi 2 — zgjidhja homogjene hn: hn=2hn-1-hn-2 → r²-2r+1=0 → (r-1)²=0 → r=1 (shumëfishmëri 2)
   hn = β1 + β2·n

Hapi 3 — kombino dhe përshtat kushtet fillestare: an = 4·2ⁿ + β1 + β2·n
   a0: 4+β1=1 ⟹ β1=-3
   a1: 8+β1+β2=2 ⟹ β2=-3
```
**Përgjigja: an = 4·2ⁿ - 3n - 3** (verifikohet: a0=4-0-3=1 ✓, a1=8-3-3=2 ✓)

### Teoremat Formale (referuara si "Teorema 1-4" në disa ligjërata/provime)
- **Teorema (kombinim linear)**: nëse disa sekuenca plotësojnë një rekurrencë lineare homogjene, çdo **kombinim linear** i tyre gjithashtu e plotëson.
- **Teorema 2**: `r` është rrënjë e ekuacionit karakteristik **atëherë dhe vetëm atëherë** kur `rⁿ` plotëson rekurrencën.
- **Teorema 3 (rrënjë e përsëritur)**: nëse `r1` shfaqet me shumëfishmëri `m+1` te ekuacioni karakteristik, atëherë jo vetëm `r1ⁿ`, por edhe `n·r1ⁿ, n²·r1ⁿ, ..., nᵐ·r1ⁿ` janë secila zgjidhje — prandaj forma `(β0+β1n+...+βmnᵐ)r1ⁿ`.
- **Teorema 4 (struktura jo-homogjene)**: nëse `bn` plotëson rekurrencën jo-homogjene, atëherë `an` e plotëson **atëherë dhe vetëm atëherë** kur `hn=an-bn` plotëson rekurrencën homogjene përkatëse. Prandaj: **an = bn + hn**.
