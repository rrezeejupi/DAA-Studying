# 02 — Kompleksiteti Kohor dhe Hapësinor (Big-O për Iterative dhe Rekursive)

*(Ky është ndoshta kapitulli më i rëndësishëm për provim — analiza e kompleksitetit të fragmenteve kodi është pyetje e garantuar në çdo provim. Shih skedarin 11 për shembuj konkretë provimi.)*

---

## 1. Big-O për Algoritmet Iterative (Loops)

**Rregulli bazë:** për çdo unazë (loop), numëro sa herë ekzekutohet trupi i saj si funksion i `n`.

### Shembuj kryesorë (nga ligjërata, F1–F11):

| # | Struktura e loop-ut | Rezultati |
|---|---|---|
| F1 | 1 loop e vetme, `i` nga 1 në n | **O(n)** |
| F2 | 2 loops të ndërthurura (nested), `i,j` të dyja 1→n | **O(n²)** (3 loops nested → O(n³), etj. — çdo nivel shtesë shton një fuqi të n) |
| F2b | `while` me `i` që rritet, `S=S+i`, ndalet kur `S≤n` | Nga formula e shumës k(k+1)/2=n → **O(√n)** |
| F3 | Loop me kusht `i² < n` | **O(√n)** — best=worst=average |
| F4 | 3 loops: `i`(1→n), `j`(1→i), `k`(1→100, konstante) | **O(n²)** — loop-i konstant (100) nuk ndikon rendin |
| F5 | 3 loops: `i`(1→n), `j`(1→i²), `k`(1→n/2) | **O(n⁴)** |
| F6 | Loop me `i=i*2` (dyfishim) | **O(log₂n)** — rregull i përgjithshëm: hap shumëzues m → O(logₘn) |
| F7 | `i`(n/2→n), `j`(1→n/2), `k` dyfishues (1→n) | Loop-e të pavarura shumëzohen → **O(n²log₂n)** |
| F8 | `i`(n/2→n), `j` dyfishues, `k` dyfishues | **O(n(log₂n)²)** |
| F9 | `while(n>1){n=n/2}` | **O(log₂n)** (edhe kur n s'është fuqi e 2-shit, p.sh. n=20 → ⌊log₂20⌋=4) |
| F10 | `i`(1→n), `j`(1→n, hap `i` — i varur) | Shuma n+n/2+n/3+...+n/n = n·log n → **O(n log n)** |
| F11 | n=2^(2^k), `i`(1→n), `j` brenda katrorohet (`j=j²`) derisa `j≤n` | **O(n·log log n)** |

**Këshillë praktike:** kur has një fragment kodi me loops të ndërthurura, identifiko: (a) a janë loops **të pavarura nga njëri-tjetri** (shumëzo kufijtë), (b) a **varet** kufiri i loop-it të brendshëm nga ndryshorja e jashtme (analizo si shumë, jo si produkt i thjeshtë), (c) a rritet/zvogëlohet ndryshorja **në mënyrë additive** (i++, → lineare) apo **multiplikative** (i=i*2 ose i=i/2, → logaritmike).

---

## 2. Big-O për Algoritmet Rekursive

### Metoda e Zëvendësimit (Back-Substitution)

**Shembull 1:** T(n) = 1 + T(n-1), T(1) = 1
- Zëvendëso: T(n) = k + T(n-k). Zgjidh n-k=1 ⇒ k=n-1.
- **T(n) = n → O(n)**

**Shembull 2:** T(n) = n + T(n-1), T(1) = 1
- Shuma: 1+2+...+n = n(n+1)/2
- **T(n) = O(n²)**

### Metoda e Pemës Rekursive (Recursion Tree)

**Shembull 1:** T(n) = 2T(n/2) + c, T(1) = c
- Pema ka **log n** nivele; puna/nivel dyfishohet (c, 2c, 4c, ...), por shuma gjeometrike → **O(n)**

**Shembull 2:** T(n) = 2T(n/2) + n, T(1) = 1
- Çdo nivel bën punë totale = n; ka log n nivele → **O(n log n)**

---

## 3. Master Theorem-i (Teorema Master) — FORMULA E PLOTË

Për **T(n) = aT(n/b) + Θ(nᵏ logᵖn)**, ku **a≥1, b>1, k≥0, p real**:

1. **Nëse a > bᵏ**: T(n) = **Θ(n^(log_b a))**
2. **Nëse a = bᵏ**:
   - (a) p > -1 → T(n) = **Θ(n^(log_b a) · log^(p+1) n)**
   - (b) p = -1 → T(n) = **Θ(n^(log_b a) · log log n)**
   - (c) p < -1 → T(n) = **Θ(n^(log_b a))**
3. **Nëse a < bᵏ**:
   - (a) p ≥ 0 → T(n) = **Θ(nᵏ · logᵖn)**
   - (b) p < 0 → T(n) = **O(nᵏ)**

### Shembuj të Zgjidhur (praktikoni t'i identifikoni rastin shpejt)

| Relacioni | a | b | k | p | Rasti | Rezultati |
|---|---|---|---|---|---|---|
| T(n)=3T(n/2)+n² | 3 | 2 | 2 | 0 | a<bᵏ (3<4), 3a | Θ(n²) |
| T(n)=4T(n/2)+n² | 4 | 2 | 2 | 0 | a=bᵏ (4=4), 2a | Θ(n²log n) |
| T(n)=16T(n/4)+n | 16 | 4 | 1 | 0 | a>bᵏ (16>4) | Θ(n²) — sepse log₄16=2 |
| T(n)=2T(n/2)+n log n | 2 | 2 | 1 | 1 | a=bᵏ (2=2), 2a (p=1>-1) | Θ(n log²n) |
| T(n)=2T(n/2)+n/log n | 2 | 2 | 1 | -1 | a=bᵏ, 2b (p=-1) | Θ(n log log n) |
| T(n)=2T(n/4)+n^0.51 | 2 | 4 | 0.51 | 0 | a<bᵏ (2<4^0.51≈2), 3a | Θ(n^0.51) |

**Rastet kur Master Theorem-i NUK APLIKOHET** (kujdes në provim — kërkojnë metodë tjetër):
- T(n) = 2ⁿT(n/2) + nⁿ — **a nuk është konstante** (varet nga n)
- T(n) = 0.5T(n/2) + 1/n — shkel kushtin **a ≥ 1**
- T(n) = 64T(n/8) - n²log n — ka **ZBRITJE** në vend të mbledhjes, jo forma standarde

**Këshillë provimi:** nëse jepet relacion rekurence dhe kërkohet "forma e mbyllur" (p.sh. T(n)=3T(n-1)-15, T(1)=8 — shih skedarin 11, Detyra 7), ky **NUK është në formën e Master Theorem-it** (është T(n-1), jo T(n/b))! Për këto duhet **zëvendësim i përsëritur** (back-substitution) siç shpjegohet në §2 më lart dhe skedarin 03.

---

## 4. Krahasimi i Algoritmeve për nga Rendi i Rritjes (Growth Order Comparison)

**Teknika 1 — Pjesëtimi me faktor të përbashkët:** p.sh. n² vs n³ → pjesëto me n² → krahaso 1 vs n.

**Teknika 2 — Logaritmimi i të dy anëve:** p.sh. n² vs 2ⁿ → merr log₂ → krahaso 2log n vs n → thjeshtohet te krahasimi log n vs n.

### Shembuj të Punuar (nga ligjërata, me vlera konkrete n=2^128 etj.)

1. **n² vs n log n**: → divido me n² → 1 vs (log n)/n → për n të mëdha, O(n²) > O(n log n)
2. **2ⁿ vs 3ⁿ**: krahasohen direkt me bazë eksponenciale
3. **n vs (log n)¹⁰⁰**: krahasim me crossover shumë vonë (p.sh. n=2^128)
4. **O(n) vs O((log n)^100)**: për n=2^1024, log n=1024, (log n)^100 → 100·log(1024)=100·10=1000, ndërsa log n=1024 → **O(n) > O((log n)^100)** për n mjaftueshëm të madh
5. **(n log n) vs (n log n)** (variante me log log n): teknika e logaritmimit të përsëritur
6. **f(n)=n³ (0≤n<10,000), n² (n≥10,000)** vs **g(n)=n (0≤n<100), n³ (n≥100)**: krahasim **me brez** (piecewise) → duhet identifikuar n₀=10,000 ku g(n)=O(f(n))
7. **f1=2ⁿ, f2=n^(3/2), f3=n log n, f4=n^(log n)**: krahasim çift-nga-çift sistematik →
   - Renditja përfundimtare (nga më i madhi te më i vogli): **f1 > f4 > f2 > f3**
   - Pra: **2ⁿ > n^(log n) > n^(3/2) > n log n**

**Këshillë:** kur krahasoni funksione komplekse, gjithmonë (a) pjesëtoni me faktorin e përbashkët më të thjeshtë të mundshëm, (b) nëse ende s'është e qartë, logaritmoni të dy anët, (c) zëvendësoni me vlerë konkrete shumë të madhe (si n=2^128) për të verifikuar drejtimin e pabarazisë.

---

## 5. Kompleksiteti Hapësinor (Space Complexity) — Detaje

### Algoritme Iterative

- **Shembull 1** (`for i=0;i<10;i++) A[i]=0`): hapësirë shtesë vetëm për `i` → **O(1)**, konstante.
- **Shembull 2** (krijon varg të ri `B[n]` nga `A[n]`): hapësirë shtesë O(1) për `i` + O(n) për `B[n]` → **O(n)**.
- **Shembull 3** (krijon matricë 2D `B[n][n]`): hapësirë shtesë O(n²) → **O(n²)**.

### Algoritme Rekursive — Analiza përmes Stekut

Tre lloje rekursioni:
- **Head-recursion**: thirrja rekursive është urdhëri i **parë** (rekursion "në krye").
- **Tail-recursion**: thirrja rekursive është urdhëri i **fundit** ("në bisht").
- **Body-recursion**: thirrja rekursive është **ndërmjet** urdhërave të tjerë.

**Shembull 1** (head-recursion e thjeshtë): `A(n) { if(n≥1) { A(n-1); P(n); } }`
- Hapësira e stekut: max n+1 thirrje njëkohësisht në stek → **O(n)** hapësirë.
- Kompleksiteti kohor: T(n) = T(n-1) + 1 → me zëvendësim: **T(n) = n+1 → O(n)**.

**Shembull 2** (2 thirrje rekursive brenda): `A(n) { if(n≥1) { A(n-1); P(n); A(n-1); } }`
- Numri i thirrjeve: A(n) = 2^(n+1) - 1 → **O(2ⁿ)** thirrje (eksponenciale!)
- **POR** hapësira e stekut mbetet **O(n)** — sepse steku kurrë s'e kalon lartësinë n+1 (funksionet tërhiqen (pop) para se të shtyhen (push) të tjera).
- Kompleksiteti kohor: T(n) = 2T(n-1) + 1 → me zëvendësim të përsëritur:
  ```
  T(n) = 2ᵏT(n-k) + 2^(k-1) + ... + 2 + 1
  ```
  Kur k=n (deri te T(0)=1): **T(n) = 2ⁿ + 2^(n-1) + ... + 1 = O(2ⁿ)** — eksponencial!
- **Metoda Dinamike (Optimizimi):** duke ruajtur rezultatet e llogaritura më parë (memoization/DP) në vend të rillogaritjes, kompleksiteti kohor bie nga **O(2ⁿ) → O(n)** — sepse çdo A(i) llogaritet vetëm 1 herë. **Kjo është motivimi themelor i Programimit Dinamik.**

**Rregull i përgjithshëm:** nëse funksioni rekursiv thërret veten **1 herë** për thirrje, thellësia (dhe hapësira e stekut) = O(n); nëse thërret veten **≥2 herë** (si Fibonacci naiv, ose shembulli 2 më lart), koha rritet eksponencialisht **por hapësira e stekut mbetet O(n)** sepse steku "zbrazet" përpara se degët tjera të "mbushen".

---

## 6. Përmbledhje e Shpejtë për Provim

- Kur të jepen 2-4 fragmente kodi për analizë kompleksiteti (shpesh shfaqet si Detyra 4 ose 6 në provime — shih skedarin 11): identifiko **numrin e loop-eve të ndërthurura**, **nëse janë të varura apo të pavarura**, **nëse ndryshorja rritet aditivisht apo multiplikativisht**.
- Nëse është rekursion: **shkruaj relacionin e rekurrencës T(n)=...**, pastaj vendos: (a) a është në formën `aT(n/b)+f(n)` → përdor **Master Theorem**; (b) a është në formën `T(n-c)+f(n)` → përdor **back-substitution** (skedari 03).
- Për pyetje të tipit "cili funksion NUK është O(n²)" (shih skedarin 11): kontrollo çdo term veç e veç kundrejt n² (p.sh. n³/√n = n^2.5 s'është O(n²); n^1.98 ËSHTË O(n²) sepse 1.98<2).
