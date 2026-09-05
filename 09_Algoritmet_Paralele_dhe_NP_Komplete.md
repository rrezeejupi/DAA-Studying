# 09 — Algoritmet Paralele & Algoritmet Jodeterministike / NP-Plotësia

*(Java 13-14 e syllabusit)*

---

# PJESA I — ALGORITMET PARALELE

## 1. Taksonomia Flynn

| Kategori | Kuptimi |
|---|---|
| **SISD** | 1 instruksion, 1 e dhënë (sekuencial klasik) |
| **SIMD** | 1 instruksion, shumë të dhëna (procesorë vektorialë) |
| **MISD** | Shumë instruksione, 1 e dhënë (rrallë, p.sh. testim primaliteti paralel) |
| **MIMD** | Shumë instruksione, shumë të dhëna (klasterë, sot më e zakonshme) |

## 2. Modeli PRAM (Parallel Random Access Machine)

Të gjithë procesorët ndajnë memorie; cikli **Lexo→Përpuno→Shkruaj**.

| Shkurtim | Lexim | Shkrim |
|---|---|---|
| CRCW | Konkurent | Konkurent |
| CREW | Konkurent | Ekskluziv |
| ERCW | Ekskluziv | Konkurent |
| EREW | Ekskluziv | Ekskluziv |

**Kostoja e algoritmit paralel = koha × numri i procesorëve.** Nëse kostoja > kostoja sekuenciale optimale, algoritmi paralel s'ia vlen ekonomikisht (edhe nëse më i shpejtë në kohë të vërtetë).

## 3. Shembuj Kryesorë (Kohë / Kosto)

| Algoritmi | Kohë | Kosto | Procesorë |
|---|---|---|---|
| Gjetja e max (naive) | O(lg N) | O(N lg N) | N/2 |
| Gjetja e max (optimale) | O(lg N) | **O(N)** | N/lg N |
| Kërkim (p=N) | O(1) | O(N) | N |
| Sorti linear i rrjetës | O(N) | O(N²) | N |
| Shumëzim matricash (Mesh) | O(N) | O(N³) | O(N²) |
| Shumëzim matricash (CRCW) | O(1) | O(N³) | O(N³) |
| MST paralel | O(N²/p) | O(N²) | ~N/lg N optimale |

**Ide kryesore**: shpesh duhen **N/lg N procesorë** (jo N) për të arritur kosto optimale = kompleksitetin sekuencial, ekzekutuar shumë më shpejt (kohë O(lg N) në vend të O(N)).

---

# PJESA II — ALGORITMET JODETERMINISTIKE & NP-PLOTËSIA

## 1. Konteksti

Deri tani: kompleksitet polinomial (linear, kuadratik, kubik). Tani: **O(N!)** dhe **O(xᴺ)** — **s'njihet algoritëm** i shpejtë. E vetmja qasje e njohur: **hamendje + verifikim**.

## 2. Klasa NP

- **P** = zgjidhshme deterministikisht në kohë polinomiale.
- **NP** = "Nondeterministic Polynomial time" — model 2-hapësh: **(1) hamendje jodeterministike** e një zgjidhjeje, **(2) verifikim deterministik** në kohë polinomiale.
- **P ⊆ NP** (çdo problem P mund të "përshkruhet" trivialisht si proces jodeterministik).
- **A është P = NP? — PYETJE E HAPUR** (ende e pazgjidhur).

### Shembulli klasik: TSP (Traveling Salesman Problem)
- Gjej rradhën që viziton çdo qytet 1 herë, minimizo koston, kthehu në fillim.
- **TSP ∈ NP**: Hapi 1 (gjenero rradhë, O(N)) + Hapi 2 (llogarit kosto, O(N²)) — të dyja polinomiale.
- Shembull madhësie: 20 qytete → 1 miliard kompjuterë ~9 muaj për zgjidhje forcë-brutale!

## 3. Reduktimi i Problemeve

Nëse A mund të transformohet (polinomialisht) në B, dhe B zgjidhet në P, atëherë A zgjidhet në P.

## 4. Problemet NP-Complete

- Problemet **më të vështira** në NP: nëse **1** zgjidhet deterministikisht-polinomialisht, **të gjitha** problemet NP zgjidhen kështu (P=NP).
- **Cook (1971)**: **CNF-SAT është i pari problem i vërtetuar NP-complete** (rezultat themelor).
- Që atëherë, qindra probleme janë treguar NP-complete duke i **reduktuar** te SAT ose te ndonjë tjetër i njohur.

## 5. Problemet Tipike NP (mësoji të gjitha emrat dhe formulimin — mund të pyeten drejtpërdrejt)

| Problemi | Optimizim | Vendosje (Decision) |
|---|---|---|
| **Ngjyrosja e Grafeve** | numri minimal ngjyrash χ(G) | a mund me ≤C ngjyra? |
| **Bin Packing** | numri minimal kutish | a futet me ≤B kuti? |
| **Knapsack (Çanta)** | maksimizo vlerën në kapacitet K | a ekziston nënset me vlerë ≥W? |
| **Subset Sum** | shuma maksimale ≤L | a ekziston nënset me shumë =L? |
| **CNF-SAT** | — (vetëm vendosje) | a ekziston kombinim true/false që kënaq shprehjen? |
| **Job Scheduling** | dënim minimal | a ekziston renditje me dënim ≤P? |

**Aplikime praktike**: ngjyrosje grafesh → planifikim provimesh (kurs=nyje, konflikt student=degë); knapsack → strategji investimi; bin packing → paketim/ruajtje disku.

## 6. P vs NP — Thelbi

- Çdo problem P është edhe NP (sortimi ∈ P ∩ NP).
- Dallimi: te NP-complete, numri i kombinimeve për t'u kontrolluar është **eksponencial/faktorial**, dhe **s'ka teknikë eliminimi efikase** (ndryshe nga sortimi, ku çdo krahasim eliminon një fraksion të madh të mundësive).
- Zgjidhje praktike: **algoritme përafrimi (approximation algorithms)** — jo optimale, por të shpejta.

## 7. Verifikimi (shembuj pseudokodi nga materiali)

```
boolean PenaltyLess(list, N, limit)  // verifikon nëse renditja e punëve ka dënim ≤ limit
  # O(N) — polinomial, plotëson kërkesën NP

boolean ValidColoring(graph, N, colors)  // verifikon nëse ngjyrosja është e vlefshme
  # O(numri i degëve) — polinomial
```

---

## Koncepte Teorike që Mund të Pyeten Drejtpërdrejt (shih edhe skedarin 11)
- **"Shpjegoni konceptet P, NP, NP-complete, Backtracking"** — kjo pyetje del verbatim në provime të kaluara!
  - **Backtracking**: teknikë kërkimi që ndërton zgjidhje hap-pas-hapi dhe **kthehet mbrapa (backtrack)** sapo të kuptohet se rruga aktuale s'mund të çojë në zgjidhje të vlefshme — përdoret shpesh si teknikë praktike për probleme NP-complete (p.sh. N-Queens, Sudoku, ngjyrosja e grafeve) kur input-i është mjaftueshëm i vogël për të qenë e realizueshme.
