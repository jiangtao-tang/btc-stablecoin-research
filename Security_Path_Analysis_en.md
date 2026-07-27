# Security Path Analysis of Free-Energy BTC

> **Basis.** ① `P_floor_解析极值分析.md` (the pure-physical skeleton ignoring fee, **core content compiled into Chapter 1 §1.1–§1.9**); ② `P_floor的fee影响分析.md` (fee analyzed separately, **core content compiled into Chapter 1 §1.10–§1.13**); ③ the base document *Research on Stablecoins Based on BTC* **Chapters 9–11** (three-layer price decomposition / multi-agent inflation externalities / the empty-window period and the physical-floor dual-standard).
>
> This paper strictly distinguishes "**pure-physical extrapolated P_floor**" from "**actual P_floor**", and focuses on two things: ① energy efficiency `ε(t)` is not an exogenous physical law but the outcome of **human profit-maximizing choice** — exactly the "externality" described in Chapter 10 of the base document; ② AI development constitutes a **real, ongoing external shock** to BTC mining. Finally it characterizes how the actual P_floor deviates from the pure-physical path.
>
> *Discipline note: this paper is **qualitative / mechanistic analysis**, not numerical prediction. We do not model the quantitative paths of `p_elec(t)`, `P_BTC(t)`, `Q(t)` (which would require macroeconomic + on-chain exogenous inputs); we only characterize **direction and mechanism**. All forward-looking passages are conceptual analysis and sensitivity demonstrations.*

---

## 1. Starting Point: The Pure-Physical P_floor Path (the Proven Skeleton)

> This chapter compiles the **core content** of the two preceding studies as the **pure-physical reference envelope** for the subsequent analysis of "human choice + AI externality":
> ① `P_floor_解析极值分析.md` (analytical extrema ignoring fee, this chapter §1.1–§1.9);
> ② `P_floor的fee影响分析.md` (fee analyzed separately, this chapter §1.10–§1.13).
> Together they answer: *if humanity always pushes mining efficiency to the physical frontier and AI does not intervene, what will P_floor look like.*

The skeleton (consistent across both studies):

```
P_floor(t) = H(t) · ε(t) · 600 / R_block(t)          [J/BTC]
H : Logistic saturation, L=1150 EH/s, k=0.55, t0=2024.4
ε : Koomey decay, halving every 1.5 yr ⇒ d/dt ln ε = −c, c≈0.462 yr^{-1}
R_block : block subsidy (ignoring fee) or S+F (with fee)
```

Its numerical trajectory (this chapter §1.9 convention): maximum ≈2021–2024 (projected 2025, P_floor≈1.747 TJ/BTC) → decline → Landauer-wall minimum ≈2054 (P_floor≈0.00081 TJ/BTC) → rebound; ignoring fee it diverges as subsidy → 0 after 2140, with fee it converges to a finite value.

### 1.1 Formula and Logarithmization

```
P(t) = H(t) · ε(t) · 600 / R_block(t)          [J/BTC]
ln P(t) = ln H(t) + ln ε(t) + ln 600 − ln R_block(t)
```

Taking the time derivative (term by term):

```
d/dt ln P = d/dt ln H  +  d/dt ln ε  −  d/dt ln R
```

### 1.2 Time Derivatives of the Three Components

> *Baseline-year rectification:* "2024" below is the data-alignment baseline (R takes the 2024-04 halving `R₀=3.125`; the Logistic `t₀=2024.4` is fitted from 2016–2025). Changing the baseline year or data window leaves the mathematical structure and the existence of the extremum unchanged; only the year drifts.

**① Hashrate H — Logistic saturation** (`H(t)=L/(1+e^{−k(t−t0)})`, L=1150, k=0.55, t0=2024.4):

```
d/dt ln H = k / (1 + e^{k(t−t0)})      ∈ (0, k)
```

t≪t0 → k=0.55 (fast historical growth); t≫t0 → 0 (virtually no growth after saturation); strictly monotonically decreasing (second derivative < 0).

**② Efficiency ε — Koomey decay** (halving every 1.5 yr, `ε(t)=ε₀·2^{−(t−tε)/1.5}`):

```
d/dt ln ε = −(ln 2)/1.5 = −c,   c≈0.4621 yr^{-1}
```

(`c` is constant and always negative.)

**③ Halving R_block — two conventions** (ignoring fee, R=subsidy):

| Convention | R(t) form | d/dt ln R |
|---|---|---|
| Continuous halving | `R₀·2^{−(t−2024)/4}` | `−(ln2)/4 ≈ −0.1733` |
| Discrete halving | step ×½ every 4 yr | a.e. = 0 (jump ×2 at halving instant) |

### 1.3 Stationary Equation and Closed-Form Solution

**Continuous-halving convention**:

```
d/dt ln P = k/(1+e^{k(t−t0)}) − c + 0.1733 = 0
         ⇒ k/(1+e^{k(t−t0)}) = c − 0.1733 = 0.2888
         ⇒ 1+e^{k(t−t0)} = k/0.2888 = 1.9045
         ⇒ e^{k(t−t0)} = 0.9045
         ⇒ t* = t0 + (1/k)·ln(0.9045) = 2024.4 − 0.183 = 2024.22
```

**Discrete-halving convention** (ignoring R's step derivative):

```
d/dt ln P = k/(1+e^{k(t−t0)}) − c = 0
         ⇒ k/(1+e^{k(t−t0)}) = 0.4621
         ⇒ 1+e^{k(t−t0)} = 1.1907
         ⇒ e^{k(t−t0)} = 0.1907
         ⇒ t* = 2024.4 + (1/0.55)·ln(0.1907) = 2024.4 − 3.01 = 2021.39
```

### 1.4 Nature of the Extremum: a Unique Maximum

`d²/dt² ln H = −k²·e^{k(t−t0)} / (1+e^{k(t−t0)})² < 0`, so `d/dt ln P` is **strictly monotonically decreasing**, crossing zero exactly once from positive to negative:

- t < t* : `d/dt ln H > c` (even > c−0.1733) ⇒ `d/dt ln P > 0`, P **rises**
- t > t* : `d/dt ln H < c` ⇒ `d/dt ln P < 0`, P **falls**

⇒ **t* is the unique interior stationary point under the continuous proxy and is the global maximum.** Physical meaning: when `d/dt ln H` (tending to zero over time) falls to equal `|d/dt ln ε|` (a constant), the upward force of hashrate expansion is exactly offset by efficiency improvement, and P_floor peaks; thereafter efficiency keeps improving but H is saturated, so P_floor turns down.

### 1.5 Correspondence with the "2025 Peak"

| Model | Analytical peak t* | Note |
|---|---|---|
| Discrete halving + smooth H/ε | ≈2021.4 (local max 2020–2024) | but the 2024 halving instant halves R ⇒ P jumps ×2, true peak ≈2024 |
| Continuous halving | ≈2024.22 | nearly coincides with the numerical terminal year 2025 |

The "2025 peak" is the projection of the **2024-halving peak** onto the numerical terminal year; both conventions fall in ~2021–2024, **far earlier than the Landauer wall (≈2053–2054)**. The earlier "the more you mine, the more expensive it gets" holds only for `t < t*`, as a historical empirical law, not a permanent trend.

### 1.6 The Second Extremum Before the Wall: the Minimum (at the Wall Edge)

After ε hits the Landauer wall, `d/dt ln ε → 0` (ε capped). Thereafter:

```
d/dt ln P ≈ − d/dt ln R   ≤ 0 (a.e.)
```

(jumps up at halving instants, factor ×2.)

⇒ P bottoms out at the wall (~2054) and then turns up; **~2054 is the global minimum (exactly at the wall edge)**; subsidy → 0 at 2140 ⇒ R → 0 ⇒ P_floor diverges to ∞ (this is the analytical evidence that fee cannot be omitted).

---
### 1.7 Four-Angle Guarantee of H Saturation + Formula Rectification

The existence of the extremum in §1.4 **fully depends on "H eventually saturates"**. The premise must be laid open: **(A) H must have an upper bound (the fact of saturation)** has a hard guarantee; **(B) H happens to take the Logistic form (modeling choice)** is a reasonable approximation.

**① Physical / energy hard constraint (strongest, independent of any economic assumption):** the network power `W_sec = H·ε`; even if ε hits the Landauer lower bound ε_L, still `H = W_sec / ε_L`. And `W_sec` is limited by the total energy humanity can devote to PoW — Earth's primary energy, or the share of global electricity economically allocable to mining, are finite budgets. **A capped total free-energy budget ⇒ H must have an upper bound**, which is the hard thermodynamic / energy-conservation ceiling.

**② Economic-equilibrium constraint (second strongest):** miners are rational capital; the marginal revenue per unit hashrate = the present value of newly added block rewards (subsidy + fee) captured; marginal cost = hardware + electricity. When network H is pushed up to the point where marginal revenue ≈ marginal cost (excess profit → 0), capital stops pouring in ⇒ growth rate → 0. The subsidy is fixed and decays by halving; fee is bounded above by the ceiling of real on-chain economic activity (settlement volume has an upper bound ⇒ the security budget has an upper bound). Together they determine a monetizable H upper bound — even ignoring physics, pure economics also gives an upper bound.

**③ Empirical observation (corroborating, not an independent guarantee):** actual H: super-exponential in 2010–2015, growth decelerating year by year since 2016, recently already close to the saturation region. The Logistic fit is an extrapolation of the already-observed deceleration.

**④ Mathematical structure (formalization, not an independent guarantee):** `H = L/(1+e^{−k(t−t0)})` mathematically guarantees `lim H = L` (finite) and `dH/dt > 0` decreasing. But this is a result of "we chose a saturating function"; the true guarantee comes from ①②.

> **Key counterfactual:** if we instead used exponential `H = H₀·e^{gt}`, then `d/dt ln H = g` is constant; if `g > c`, then P_floor **monotonically rises forever, no extremum, no fall-back** — exactly the implicit assumption of "the more you mine, the more expensive, permanently." We chose saturation because ①② support it. That is, whether "decline after 2025" is an artifact depends entirely on whether H saturates: H saturates ⇒ decline is structurally inevitable; H not saturating (and offset) ⇒ decline is the real artifact.

> **Rectification boundary:** "H has an upper bound (saturates)" = physics (①) + economics (②) **double hard guarantee**, empirical (③) corroborates, mathematics (④) formalizes; "H exactly follows this Logistic, L=1150 / k=0.55 / t0=2024.4" = empirical fit + modeling choice, parameters are extrapolation-uncertain. Corresponding to the extremum: **the "existence" of the maximum is robust** (as long as H saturates), **the "year" of the maximum drifts with parameters** (L/k/t0 empirical extrapolation).

### 1.8 Boundary Analysis of Quantum-Computing Shocks

> Quantum computing **does not change** the intrinsic structure of this model's "trend parameters"; what it changes is the precondition of "whether the model still applies" — it does not improve ε (may even raise it), does not intrinsically reduce H, the only real threat is indirect and discontinuous via "confidence / price".

**Boundary on ε (efficiency): quantum does not improve Koomey, may even raise it.** The Koomey law is an empirical law of **classical CMOS semiconductors**, not a universal physical law: Koomey et al. (2011) surveyed 1946–2009 and obtained "computations per kWh roughly double every 1.5 yr"; but this trend slowed after 2000 to roughly doubling every 2.6 yr, because Dennard scaling (constant power-density shrinking of transistors) ended around 2005. The true physical floor is the **Landauer limit** (Landauer 1961): irreversibly erasing one bit dissipates at least `kT·ln2`, ≈ `2.8×10^{-21} J/bit` at room temperature. Fault-tolerant quantum-computing thermodynamics research (surface code) shows quantum error-correction redundancy makes the energy per logical operation **significantly higher** than the Landauer limit (`E_cycle ≥ (N_phys/2)·kT·ln2`), and actual FTQC, due to finite duration and strong coupling, deviates far from the ideal Landauer limit.

⇒ A "quantum miner" is not closer to the Landauer wall, but **stands farther above the Landauer wall**: it will not lower `ε_L`, may even raise the effective `ε`. Rectification: one cannot say "quantum improves mining efficiency"; for the classical problem SHA-256, Grover only gives `O(√N)` quadratic speedup, while each quantum gate (Toffoli + error correction) costs far more energy than one classical NAND; within our computation window to 2140 it will not replace ASIC, `ε` still follows the classical Koomey (already slowed) trajectory, eventually approaching the Landauer wall.

**Boundary on H (hashrate): not intrinsically reduced, the only real threat is indirect and discontinuous.** `H` is an **economic quantity**, not a physical one — `H(t)=L/(1+e^{−k(t−t0)})` is a descriptive saturation curve of economic deployment (miners stop adding machines where "marginal revenue = marginal cost"), not a physical law. The mechanisms by which "quantum changes H" are only two:

- **Mechanism A ("quantum mining is cheaper → maintain security with less hashrate → H falls"):** the logic is reversed. If quantum mining were truly more power-efficient, rational miners would deploy more hashrate with the same electricity, pushing `H` higher or maintaining it at the economic ceiling, not lowering it. Cheaper mining → `H` rises, not falls.
- **Mechanism B (real threat, but indirect and discontinuous):** the main battlefield of quantum threatening Bitcoin is **ECDSA elliptic-curve signatures** (broken by **Shor's algorithm**), not the PoW/SHA-256 mining function. The attack chain is `Shor breaks ECDSA → large UTXOs can be stolen → chain value/trust collapses → P_BTC crashes → mining fiat revenue → 0 → rational miners shut down → H cliff-falls`. Key point: **SHA-256 mining is unaffected by Shor, PoW itself is not directly weakened.** Aggarwal et al. (2018) explicitly state that Bitcoin PoW is "relatively immune to quantum speedup" within the foreseeable 10 years, mainly because dedicated ASIC clock speeds far exceed near-term quantum machines; even using Grover to search SHA-256 is only `O(√N)` quadratic speedup; the paper also points out ECDSA is far more threatened by Shor, with the most optimistic estimate that by around 2027 a private key could be derived from a public key in <10 minutes. This threat **is mitigable**: a soft fork to post-quantum signatures (lattice-based Falcon / Dilithium) resolves it.

![Quantum attack surface: PoW/SHA-256 resistant to Grover vs signatures/ECDSA threatened by Shor vs our ε, H parameters](assets/quantum_attack_surface.svg)

**Scenario branches / model-failure boundary:** our `ε` (Koomey) and `H` (Logistic) are both empirically / descriptively calibrated curves for classical ASIC mining. Quantum computing introduces not a "smoothly incorporable parameter tweak" but a scenario branch / model-failure boundary:

| Branch | Trigger | Effect on P_floor parameters | Nature |
|---|---|---|---|
| **A (expected)** | no mature quantum mining / attack within window | ε follows Koomey (slowed), H follows Logistic, model holds | continuous, normal extrapolation |
| **B (rare)** | Grover mining matures and replaces ASIC | ε enters a non-Koomey new efficiency regime (likely higher) | regime switch, not parameter tweak |
| **C (phase transition)** | Shor breaks ECDSA and is unmitigated | chain value collapses → P_BTC→0 → H cliff-falls; P_floor economic base fails | phase-transition failure, beyond smooth extrapolation |

> In short: **quantum does not change the "trend parameters" of our model; what it changes is the precondition of "whether the model still applies".** This should be placed alongside the §1.7 rigor boundary as an "external-shock boundary", and echoes the discipline of §1.9–§1.13 that "forward passages are extension estimates / sensitivity demonstrations, not predictions."

### 1.9 Numerical Trajectory: Graphical Corroboration of Analytical Conclusions

> This section merges the core numerical trajectory into this chapter as numerical / visual corroboration of the foregoing analytical derivation. The model convention is fully consistent: ignoring fee, `H` takes Logistic saturation, `ε` takes Koomey decay capped at the Landauer lower bound, `R_block` takes only the block reward.

**Key-year trajectory table**

| Year | H (EH/s) | ε (J/TH) | Reward (BTC) | P_floor (TJ/BTC) | Phase |
|---|---|---|---|---|---|
| 2010 | 1e-5 | 1,000,000 | 50 | 0.00012 | historical |
| 2021 | 150 | 28 | 6.25 | 0.40 | historical |
| 2024 | 550 | 16 | 3.125 | 1.69 | historical (halving) |
| 2025 | 650 | 14 | 3.125 | **1.747** | **analytical maximum projection** |
| 2030 | 1.1e3 | 1.39 | 1.5625 | 0.586 | decline |
| 2040 | 1.15e3 | 0.0137 | 0.195 | 0.0483 | decline |
| **2054** | 1.15e3 | 2.87e-5 | 0.0244 | **0.00081** | **Landauer-wall bottom** |
| 2140 | 1.15e3 | 2.87e-5 | 5.8e-9 | 3.4×10³ | diverges as subsidy → 0 |

![P_floor trajectory (ignoring fee): rise → peak → decline → bottom (~2054) → rise again → diverge at 2140](assets/pfloor_no_fee_trajectory.svg)

Corroboration with analytical conclusions:

1. The analytical maximum of §1.3–§1.4 (≈2021–2024, projected to 2025) is consistent with the 2025 peak `P_floor ≈ 1.747 TJ/BTC·...` in the table; after the peak the curve declines, corresponding to the interval `d/dt ln H < |d/dt ln ε|` analytically.
2. The Landauer wall of §1.6 (ε bottoms ≈2054) corresponds to the 2054 valley `P_floor ≈ 0.00081 TJ/BTC` in the table; after the wall ε no longer improves, P_floor can only recover monotonically with the halving.
3. The divergence mentioned in §1.6 appears near 2140: subsidy → 0 ⇒ `R_block→0` ⇒ `P_floor → ∞`. This is the numerical embodiment of "P_floor diverges in the mining-end regime when fee is ignored."
4. The graph visualizes the complete structure "rise → peak → decline → bottom → rise again → diverge", mutually corroborating the analytical derivation.

### 1.10 Definition, Determinants, and Historical Evolution of the Fee

**Protocol-layer definition:** the fee of a single Bitcoin transaction (`single Fee = transaction size (vBytes) × fee rate (sat/vByte)`); the sum of all transaction fees packed in a block is the **block fee revenue** (denoted `F(t)` in this paper, unit BTC/block):

```
F(t) = Σ_{tx ∈ block} Fee(tx) = Σ (vBytes_tx × fee_rate_tx) / 1e8   [BTC/block]
```

Adding `F(t)` back into the denominator of the P_floor formula gives the complete form (the base-document §21 original formula used only `R_block = subsidy`; here we explicitly restore the omitted fee):

```
P_floor(t) = H(t) · ε(t) · 600  ÷  [ S(t) + F(t) ]
                                  ↑subsidy↑   ↑fee↑
S(t) = block subsidy = 50 · 2^{−⌊(y−2008)/4⌋}   [BTC/block, protocol-determined; halvings occur in 2012/2016/2020/2024…, so offset 2008 makes 2024 exactly 3.125 BTC, consistent with §1.9 numerical table]
F(t) = block fee revenue                                 [BTC/block, market mechanism]
```

**Metering convention (auction mechanism):**

- **Fee-rate unit:** sat/vByte (satoshis per virtual byte). Virtual bytes give SegWit/Taproot witness data a 75% weight discount (witness 1 WU/byte, core 4 WU/byte, vByte = WU/4).
- **Block-space hard cap:** one block every 10 minutes, at most **4,000,000 weight units (≈1.5–2 MB actual data)**. This is the source of consensus-layer scarcity, independent of demand.
- **Clearing mechanism:** when constructing the block template, miners greedily fill by descending fee rate up to 4M WU — essentially a **first-price auction** (all transactions in the same block pay their own bid, not a uniform price).
- **Queue:** unconfirmed transactions enter each node's mempool; during congestion low-fee transactions are evicted, naturally forming a **floor fee rate** that rises with congestion.
- **Result:** `F_block = average fee rate × actually-packed vBytes`, both determined **endogenously** by supply-demand + mechanism, no central pricing.

**Determinants (demand-supply decomposition):**

| Side | Parameter | Explanation |
|---|---|---|
| **Demand side** (how much users are willing to pay) | settlement count / on-chain activity `Q(t)` | transfers, exchange deposits/withdrawals, L2 settlement (Lightning/Spark batched into mainnet) |
| | block-space demand intensity `D(t)` | Ordinals / Inscriptions / BRC-20 / Runes and other new use cases can instantly fill 4M WU |
| | congestion and impatience | sharp price volatility, bull-market FOMO, exchange hack events → users rush confirmations, bid up price |
| **Supply side** (where scarcity comes from) | block-space cap `B` (~4M WU) | **consensus hard cap**, does not change with demand |
| | script / witness discount | SegWit/Taproot raise "effective throughput" but only lower unit price, do not remove scarcity |
| **Mechanism** | first-price auction + mempool queue + replace-by-fee | converts the above supply-demand into F_block |

> **Objectivity grading** (echoing the Supplement §3): `ε_L` (Landauer lower bound) is purely physical, unrelated to humans; `ε(t)` (actual efficiency) is semi-objective (physically inevitable, rate empirical); `H(t)` (hashrate) is semi-objective (has an upper-bound guarantee, position empirical); `F(t)` (fee) is **the most exogenous, most assumption-laden, most historically volatile** — it is the only one of the four factors without a physical or protocol lower-bound backing; its future level is purely determined by on-chain economic activity, with the highest uncertainty.

**Historical evolution (order-of-magnitude approximation, not a precise on-chain series):** the fee share of miner revenue rises monotonically with each halving — the subsidy halves every 4 years, the denominator of fee share shrinks, and use-case expansion enlarges the numerator; the two compound.

| Year | Subsidy share | Fee share | Note |
|---|---|---|---|
| 2009 | 100% | 0.00% | fee almost zero |
| 2010 | 100% | 0.00% | |
| 2011 | 99.90% | 0.10% | |
| 2017 | 87.45% | **12.55%** | bull-market congestion outlier |
| 2021 | 93.88% | 6.12% | |
| 2023 | 93.51% | 6.49% | pulled by Ordinals |
| 2024–2028 | ~94%→ | 6%–50% | Runes once reached 75% in a single day |
| ~2140+ | 0% | **100%** | subsidy → 0, pure-fee era |

**Extreme spike events (evidence of fat-tailed distribution):** in 2023-05 Ordinals hit 680k daily transactions, mempool expanded to 500+ MB, high-priority fee rate spiked to 654 sat/vB, single-block fee revenue first exceeded the 6.25 BTC subsidy; on 2024-04 Runes + halving day fee share of miner revenue reached **75%** (historical single-day high), that block collected 37.7 BTC in fees ($2.4M). **Structural trend:** fee share rises structurally with halvings; short-term extremely volatile, fat-tailed, dominated by occasional shocks like Ordinals/Runes/bull markets; currently (2025–2026) the average is low but can be interrupted by new use cases at any time. Since the 2024 halving, **the top 1% of blocks have collected 32% of total fees** (Gini≈0.68) — extrapolating the future from "today's low average" would severely misjudge. ⇒ fee **cannot be smoothly extrapolated** — this is precisely the reason to treat it only as a "sensitivity tool" rather than a "prediction." Moreover, this concentration (Gini≈0.68) is not only evidence of "fat tail, non-extrapolable" but also a **real-world centralization risk signal**: a few large block-space consumers dominate the fee rate, so the actual trajectory of the post-wall regime (P_floor determined by fee, §1.12, §4.7) will be decided by the payment behavior of centralized entities; its decentralization discipline is in §5.1.

### 1.11 Analytical Analysis of Incorporating Fee into the P Formula

Formula rewrite and logarithmization:

```
P(t) = H(t) · ε(t) · 600 / ( S(t) + F(t) )
ln P(t) = ln H + ln ε + ln 600 − ln( S(t) + F(t) )
```

Taking the time derivative (same method as §1.2):

```
d/dt ln P = d/dt ln H  +  d/dt ln ε  −  [ S·S' + F·F' ] / ( S + F )
                      ↑saturation crossing↑   ↑Landauer constant↓      ↑new fee term↑
```

where `S' = dS/dt` (subsidy decays deterministically by halving), `F' = dF/dt` (market derivative of fee, no closed form, only empirical / assumed).

**Two regimes (core analytical conclusion):**

| regime | condition | P approximation | vs "ignoring fee" |
|---|---|---|---|
| **Subsidy-dominant period** | `S ≫ F` (now and for a long time to come) | `P ≈ H·ε·600 / S` | **identical** — all conclusions of §1.3–§1.4 hold in this interval |
| **Fee-dominant period** | `S ≪ F` (especially after 2140 `S→0`) | `P ≈ H·ε·600 / F` | **fundamentally different** — eliminates the divergence when fee is ignored |

> **Rectification boundary (fee is not "absolutely zero-impact" on the in-wall maximum):** the "identical" in the table above means that in the subsidy-dominant period (S≫F), the fee contributes only an O(F/S)-magnitude perturbation to the derivative; the existence and approximate location of the maximum are determined by H/ε/S. Writing out exactly the difference between the full derivative with fee and without: `δ = [S·S' + F·F']/(S+F) − S'/S = F·(F'−S')/(S+F)`. Since `S' ≤ 0` (subsidy does not increase) and `F' − S' > 0`, we have `δ > 0`: with fee the denominator falls slightly faster, `d/dt ln P` is slightly more negative, and the maximum shifts slightly left (peaks earlier) compared to ignoring fee. But its magnitude is suppressed by `F/S` — historical fee share is only 1–2%, `δ ~ 0.003/yr`, negligible relative to the main derivative slope `~0.55/yr`; the extremum position offset is only **on the order of days** (≪ parameter-fitting error). Therefore "the in-wall maximum is unaffected" is an **analytical approximation that holds**, not absolute zero-impact; what is truly "analytically determined and structural" is a different matter: **fee rewrites the post-2140 divergence behavior (P finite when S→0), not the 2025 peak within the wall (before 2054)**. If fee overtakes subsidy within the wall in the future (fee-dominant period arrives early), the extremum structure would be substantively rewritten — but that already exceeds the premise of "ignoring fee."

**Elimination of the 2140 divergence (the most critical conclusion):**

- **Ignoring fee:** `S→0 ⇒ P = Hε600/S → ∞`. The formula **blows up** at mining end — this is the fatal flaw of the simplified model.
- **With fee:** when `S→0`, `P → H·ε·600 / F`; as long as there is a **positive fee market** (`F>0`), P takes a **finite value**.

This is precisely the underlying motivation for reconstructing the anchor as "**physical lower bound ε_L·H_min + security budget (subsidy + fee)**": fee gives P a finite, monetizable lower bound in the "post-subsidy era", instead of diverging to infinity mathematically.

**Numerical simulation: with-fee vs without-fee comparison** (same formula as §1.1, reusing the exactly identical historical / future `H·ε·S` extrapolation of §1.2, only overlaying the fee term in the denominator; fee takes two demonstration assumptions: `F_const = 0.2 BTC/block` constant, `F_grow = 0.10·1.03^(t−2025)` gently growing 3%/yr):

![P_floor with fee vs without fee: overlap within the wall, diverge at 2140 (ignoring fee diverges, with fee finite)](assets/pfloor_fee_vs_nofee.svg)

**Key-year values (J/BTC)**

| Year | Ignore fee | With fee constant (F=0.2) | With fee growing (3%/yr) | Note |
|---|---|---|---|---|
| 2025 | 1.75×10¹² | 1.64×10¹² | 1.69×10¹² | still nearly overlapping near peak (subsidy-dominant) |
| 2054 | 8.11×10⁸ | 8.82×10⁷ | 7.61×10⁷ | Landauer wall: with fee already ~1 order of magnitude lower |
| 2140 | **3.40×10¹⁵** | **9.90×10⁷** | **6.61×10⁶** | **divergence point**: ignore fee diverges, with fee finite |
| 2160 | 1.09×10¹⁷ | 9.90×10⁷ | 3.66×10⁶ | ignore fee keeps climbing; with fee flattens / gently declines |

Three conclusions graphically corroborated:

1. **Within the wall (before 2054) the two lines nearly coincide** — quantitatively confirming that in the subsidy-dominant period the fee's effect on P is suppressed by `F/S` to negligible; extremum structure and position unchanged.
2. **2140 is the true divergence point** — ignoring fee, `S→0` makes P diverge toward the top of the chart; with fee, `P→Hε600/F` converges to a finite small value, intuitively showing "divergence eliminated."
3. **Fee dynamics determine the post-subsidy-era magnitude** — constant fee flattens, growing fee lower; but regardless of the positive form F takes, the structural conclusion that **divergence is eliminated** holds.

> Boundary note: the future passages and fee assumptions in this chart and table are **conceptual analysis and sensitivity demonstrations, not predictions**; if the `F(t)` assumption changes, the specific post-subsidy P value changes accordingly, but the three conclusions "overlap within wall, diverge at 2140, divergence eliminated" do not depend on the specific form of F, only on `F>0`.

### 1.12 Post-Wall Regime: Fee Dynamics with Micro-Closure as the Sole Assumption

As established in §1.11, fee dynamics become the dominant term of P only after approaching the Landauer wall. This section can tighten further — in the post-wall regime, H is already saturated and ε has hit the Landauer lower bound, both can be treated as **constants**; thus the entire system has only fee as a dynamic variable, and its evolution is fully determined by the **single assumption** of "miner-equilibrium micro-closure."

**Premise: double freeze after hitting the wall:**

```
H(t) = L        (saturated, constant)
ε(t) = ε_L      (Landauer lower bound, constant)
⇒ W_sec(t) = H·ε = L·ε_L ≡ W_wall   (defensive work, constant)
```

`W_wall` is the constant physical defense power after hitting the wall. Note its magnitude has two layers of implication for subsequent conclusions: under our model convention `ε=ε_L≈2.87×10⁻⁵ J/TH`, `L=1150 EH/s`, giving `W_wall≈33 kW` (theoretical lower bound); if the actual efficiency ~15 J/TH is used, then `W_wall≈17 GW`. **The two only affect the absolute magnitude of the fee floor, not the analytical structure of this section.**

**The sole assumption: miner-equilibrium micro-closure.** Miner zero-profit equilibrium (revenue = electricity cost, per second):

```
R_block · P_BTC / 600  =  W_sec · p_elec
```

- Left: security expenditure per second (USD/s), `R_block = S + F` is total block reward, `P_BTC` is BTC USD price;
- Right: electricity cost of defensive work per second, `p_elec` is electricity price (USD/J).

Rearranging: `R_block = 600·W_sec·p_elec / P_BTC`. This closure is the **only exogenous assumption** of this section; H, ε are already frozen, no other dynamics introduced.

**Closed form of fee dynamics.** Substituting `R_block = S + F` and `W_sec = W_wall`:

```
F(t) = 600 · W_wall · p_elec(t) / P_BTC(t)  −  S(t)
```

- `S(t)`: subsidy, deterministically decays by halving table, `S=0` after 2140;
- `p_elec(t)`, `P_BTC(t)`: still exogenous, but **already the only two remaining exogenous inputs** (H, ε have exited).
- After 2140: `F(t) = 600·W_wall·p_elec(t)/P_BTC(t) ≡ F_wall(t)`, i.e. the "security-required fee floor."

If we take constant growth rates `g_e` (electricity price), `g_p` (BTC price), then in the post-subsidy era:

```
F(t) ≈ F_wall · exp((g_e − g_p)(t − 2140))
```

- `g_e > g_p`: energy becomes relatively more expensive vs BTC ⇒ fee (in BTC) **rises**;
- `g_e < g_p`: BTC appreciates faster than energy ⇒ fee (in BTC) **falls**;
- `g_e = g_p`: fee constant.

**Unexpected simplification: P_floor degenerates to a pure monetary ratio.** Substituting the closure into the P_floor definition:

```
P_floor = H·ε·600 / R_block = W_wall·600 / R_block
        = W_wall·600 / (600·W_wall·p_elec / P_BTC)
        = P_BTC / p_elec
```

**`W_wall` (i.e., H, ε) is completely eliminated throughout the derivation.** Conclusion: in the post-wall regime, P_floor (J/BTC) contains no physical quantity, degenerating into

```
P_floor = P_BTC / p_elec
```

i.e., "the amount of energy in joules one BTC can buy" — a **pure monetary / energy relative-price ratio**. Rectification: `P_floor = P_BTC/p_elec` is the **identity where physical constraint and market equilibrium meet**, and is the core formula of the whole document; it is not a declaration that "physics yields to money." Under zero-profit competition, the physical cost per BTC and the joules of energy one BTC can buy are locked by arbitrage into the same value. After hitting the wall `H`, `ε` freeze, physical quantities are eliminated from the expression, but `ε_L` and `L` remain the **hard boundary and background condition** for this equality to hold — without them, `P_BTC/p_elec` is just a pair of unanchored prices. Physics is not the "carrier of the transition period", but the **gravitational source that anchors P_BTC in the long run**; `P_BTC/p_elec` is merely the **instantaneous reading** of this pricing relationship in the monetary / energy price system. First comes the physical possibility boundary (`ε_L`, `L`, halving rhythm), then comes the market-equilibrium reading; once the reading forms, it in turn constrains P_BTC through arbitrage — the "chicken-and-egg" here merges into two sides of the same equilibrium coin.

> Note: this identity `P_floor = P_BTC/p_elec` always holds under competitive equilibrium (directly derived from the zero-profit condition, see §4.4 ①); the emphasis here on "no physical quantity remains" means that after hitting the wall `H,ε` are frozen into known constants, so the expression no longer contains unknown physical parameters — not that the equality holds only post-wall.

> Boundary note: this section is an **asymptotic regime model**, not a prediction; the premise "H, ε freeze after hitting the wall" is itself a logarithmic extrapolation (see §1.7 four-angle guarantee). The closure gives the **security-floor fee**; actual demand-side shocks (Ordinals/Runes) can push the instantaneous fee above the floor, the floor being the mean-reversion center. `p_elec`, `P_BTC` remain exogenous, so fee dynamics are not "fully solved" but **reduced to a function of these two variables** — this is the ultimate simplification achievable; remaining uncertainty is irreducible (unless a macroeconomic model is introduced).

### 1.13 Complete Block-Space Dynamics Model (unified framework of finite supply + free-market choice)

§1.10, §1.11, §1.12 respectively gave the fee definition, analytical perturbation, and post-wall micro-closure. This section formalizes the two most foundational protocol primitives — "finite supply + free-market choice" — into a computable, calibratable **block-space queue + first-price auction** dynamics model, and unifies §1.12's micro-closure as the boundary condition of the post-wall regime.

> **Rectification boundary (model origin):** this model is not an explicit rule in Satoshi's whitepaper, but an equilibrium property that **emerges necessarily** from the two protocol primitives Satoshi did set — ① block space is finite (Satoshi set a 1 MB cap in 2010, later changed to 4M WU by BIP 141); ② miners freely pick-and-pack and rationally maximize incentives (the incentive setting of whitepaper §6). "First-price auction" is the name economists give this emergent structure, not an equation hardcoded at the protocol layer.

**Core variables and units (this section's convention):**

| Symbol | Meaning | Unit |
|---|---|---|
| Q(t) | on-chain economic-activity index (normalized) | dimensionless |
| D(t) | potential block-space demand | vBytes/block |
| M(t) | mempool byte backlog | vBytes |
| p*(t) | first-price-auction clearing price | sat/vByte |
| F(t) | fee actually received by miners per block | sat/block |
| cap | block-capacity upper bound | vBytes/block |
| p_dust | dust / relay fee floor | sat/vByte |

**Demand side: D(t) driven by Q(t)** (simplest calibratable form, deterministic mean):

```
D(t) = α · Q(t)^β
```

- α: scaling coefficient from activity to bytes; β: scale elasticity, typically β≈1 (activity doubles, demand roughly doubles).
- Q(t) can be proxied by on-chain transfer count, UTXO changes, or Ordinals/Runes activity index.

**Supply side: piecewise price formation p*(t)** (the most critical nonlinear switch):

**Regime I (non-congested, D(t) ≤ cap):** excess capacity, miners accept any transaction above marginal processing cost:

```
p*(t) = p_dust
F(t)  = D(t) · p_dust
```

fee is linear in demand, numerically small.

**Regime II (congested, D(t) > cap):** transactions greedily fill up to cap by descending fee rate, the first cap bytes enter the block, the marginal transaction's bid is the clearing price. Approximating the mempool bid distribution as a tail power law gives:

```
p*(t) = p_dust · ( D(t) / cap )^κ
F(t)  = cap · p*(t) = p_dust · cap^(1−κ) · D(t)^κ
```

- κ: congestion elasticity, estimated from historical data, typically 1–2; when κ>1 fee jumps super-linearly with congestion, explaining Ordinals/Runes spikes.

When demand crosses cap, fee jumps directly from the linear segment into the power-law segment — this is the emergence source of "more activity ⇒ higher fee."

![Fee demand-fee dynamic adjustment (congestion switch): non-congested linear segment + congested power-law segment](assets/fee_dynamics.svg)

**Coupling with the P_floor framework (degeneration after hitting the wall).** Substituting `F(t)` back into the `P_floor` definition, fee changes from an exogenous assumption to an endogenous function of `Q(t)`:

```
P_floor(t) = H(t)·ε(t)·600 / ( S(t) + F(Q(t))/1e8 )
```

In the post-wall regime (H=L, ε=ε_L frozen), combined with §1.12 micro-closure `R_block·P_BTC/600 = W_wall·p_elec`, one can solve:

```
P_BTC(t)   = W_wall · p_elec(t) · 600 / ( S(t) + F(Q(t))/1e8 )
P_floor(t) = P_BTC(t) / p_elec(t) = W_wall · 600 / ( S(t) + F(Q(t))/1e8 )
```

**Conclusion:** after hitting the wall `P_floor` is entirely endogenously determined by on-chain economic activity `Q(t)`; the only remaining external inputs are `p_elec(t)` (electricity price) and `Q(t)` (activity index); and it always satisfies `P_floor = P_BTC / p_elec`, fully consistent with the surprising simplification of §1.12. The role of the block-space model here is to replace the "arbitrary exogenous F" of §1.12 with "`F(Q(t))` endogenously determined by Q(t) via auction."

**Numerical scenario: P_floor over a century under high / medium / low Q(t)** (parameters per calibration method; specific values are demonstration assumptions, not predictions):

![P_floor path under high/medium/low Q(t) scenarios: overlap before wall, endogenous divergence driven by Q after hitting wall](assets/pfloor_scenarios.svg)

- **Before the wall (before 2054) the three scenarios nearly coincide:** here `S ≫ F`, fee share <1%, `P_floor ≈ H·ε·600/S`, consistent with §1.11.
- **Divergence after hitting the wall is endogenously driven by `Q(t)`:** here `S→0`, the denominator of `P_floor = W_wall·600/(S+F/1e8)` is determined by `F(Q)`, essentially the same as the earlier "exogenous F placeholder", but here `F` is computed from `Q` by auction.
- **Mechanism rectification (counterintuitive but self-consistent):** after hitting the wall `P_floor` is "the energy density per BTC." Stronger activity → auction pushes up `F` (BTC/block) → larger denominator → `P_floor` **lower**; weaker activity → `F` smaller → `P_floor` higher. This is not "high activity weakens security" — the equal defensive power `W_wall` is unchanged, only when spread over more BTC-denominated units, the joules contained per single BTC are diluted. Switching to the monetary view `P_floor = P_BTC/p_elec`, then `P_BTC` is linked by the micro-closure `P_BTC=600·W_wall·p_elec/(S+F)`, the direction is completely consistent. This counterintuitiveness is precisely the manifestation of "fee belongs to the second-layer human / market part" at the limit.

> Boundary note: this model is a **later interpretive framework** based on "finite supply + free-market choice", not Satoshi's design; all Q(t) scenarios and parameters are **conceptual analysis and sensitivity demonstrations, not predictions**. Relationship with §1.11, §1.12: §1.11 uses an arbitrary exogenous F placeholder, §1.12 anchors the F floor via micro-closure, this section unifies the two into a computable model "endogenously determined by Q(t) via auction", which is the complete form of fee-dynamics research.


### 1.14 Summary of the Pure-Physics Starting Point: A Reference Envelope, Not a Trajectory

The pure-physics derivations compiled in this chapter give the **analytical skeleton and reference envelope** of P_floor:

- **Topology**: rise → peak (~2025) → decline → **Landauer wall (minimum, ~2054, an inescapable hard floor)** → rebound; with fee included, the post-2140 path converges to a finite level instead of diverging.
- **Robust existence of extrema**: as long as H saturates (doubly hard-guaranteed by physics + economics, §1.7), the maximum exists; the exact year drifts with parameters.
- **Role of fee**: before the wall (S ≫ F) its influence is negligible; after the wall (S → 0) it takes over P_floor, eliminates the divergence, and degenerates P_floor into the monetary ratio `P_BTC/p_elec` (§1.11–§1.12).
- **External shocks** (quantum, §1.8) only change the premise of "whether the model applies", not the trend parameters.

**Key recognition**: this path treats `ε(t)` as an **exogenous, deterministic physical law** and `H(t)` as an **economically inevitable saturating curve**. What it answers is "*if humanity always pushes miner efficiency to the physical frontier, and AI does not interfere*, how would P_floor evolve" — it is a **reference envelope**, not the actual trajectory. The argument of §2–§4 of this document is: the actual trajectory deviates from this envelope, because the **concrete values of `ε(t)` and `H(t)` are humanly chosen**, and are being continuously perturbed by AI as an external sector.

---




## 2. The First Externality: `ε(t)` Is Not a Physical Law but a Human Choice (following Chapter 10 of the base document)

### 2.1 The Pure-Physics Model Treats ε as Exogenous — but Chapter 10 Proves η/ε Is Endogenous and Carries Externalities

The core formula of Chapter 10 of the base document:

```
π_B = Σᵢ sᵢ · πᵢ ,  where πᵢ = (dηᵢ/dt)/ηᵢ is agent i's own efficiency growth rate
```

For BTC mining, "efficiency" is the reciprocal of `ε` (smaller J/TH = more efficient). **The network-wide actual `ε(t)` = the fleet-weighted average efficiency of miners on the network, not the Koomey frontier itself**. Which generation of mining rig each miner deploys at time `t` is a profit-maximization decision:

```
Miner profit = (block reward + fee)·P_BTC  −  (new-rig capex amortization + electricity)
```

A miner upgrades only when "the electricity saved by switching to a more efficient new rig > the capex amortization of the new rig". Therefore:

- In **low-electricity-price regions**, old rigs (high `ε`) can run for a long time and capex does not pay off → fleet `ε` stays high → the **actual decline of `ε` is slower than the Koomey frontier**.
- In **high-BTC-price / high-electricity-price** regions, the upgrade incentive is strong → `ε` declines fast.

→ The constant `c≈0.462` (Koomey rate) in the pure-physics model should be replaced by an **effective rate**:

```
c_eff(t) = Σᵢ sᵢ · (π_run,i − δᵢ)
```

where `π_run,i` is the running-efficiency growth rate of miner class i (the human upgrade cadence), and `δᵢ` is the **capital-replacement pulse / embodied-drag term** of §10.7 (each replacement injects embodied energy in one shot, temporarily raising effective cost and partially offsetting the running-efficiency gain). This is precisely the direct application of Chapter 10 of the base document, which rewrites "efficiency inflation" from an exogenous law into an **endogenous, game-theoretic variable**.

### 2.2 Profit Maximization → ε Lags the Frontier → Actual P_floor Is Higher (Shallower Valley)

Because the actual `ε` lags the Koomey frontier, `ε` in the 2025–2054 segment is **larger than the pure-physics assumption** (higher J/TH). Since `P_floor ∝ ε`:

- **Shallower decline**: the pure-physics path relies on "rapid ε improvement" to press P_floor toward the valley; actual ε improves more slowly, so P_floor declines more gently.
- **Higher and later valley at the Landauer wall**: the actual miner fleet reflects machines deployed in the past, approximately `ε_actual(y) ≈ ε_Koomey(y − τ)`. Figure 1 takes τ≈6 years as a sensitivity demonstration, and the 2054 valley rises from the pure-physics ~0.0007 TJ/BTC to ~0.007 TJ/BTC (about **1 order of magnitude**; this magnitude is an illustrative sensitivity value, not a precise derivation — τ is only an order-of-magnitude proxy for the lag); the time at which the actual fleet hits `ε_L` is also postponed.

> Rectification: the "decline rate `c`" of the pure-physics path is an **upper bound on the decline rate** — it assumes humanity races along the efficiency frontier all the way. Actual human choices (especially old-rig life extension in low-electricity-price regions and the humanly set upgrade cadence) make the actual `ε` improve more slowly, so the decline of P_floor is milder. This is exactly the opposite of the implicit assumption that "mining forever gets more expensive": here **human conservatism actually makes the floor harder to press down**.

### 2.3 Capital-Replacement Pulses → Staircase Rather than Smooth (base document §10.7 / §10.10)

The actual evolution of `ε(t)` is not a smooth exponential but **staircase + pulses**:

- New chip release → miners' **synchronized replacement wave** (§10.7.4 / §10.10) → `ε` drops stepwise within the replacement window;
- At each replacement instant, the embodied energy `δᵢ` injects a **downward pulse** (§10.7.2), briefly offsetting the running-efficiency gain.

For BTC, §11.4.3 has shown `E_embodied` accounts for only ~1%, so the pulse amplitude is small; but **the staircase structure itself means ε improvement is discrete and locked to the hardware industry cycle**, not a continuous Koomey law. Writing `d/dt ln ε` as the constant `−c` in §1.2 is a **smoothed approximation** of this staircase structure — the approximation holds, but the true trajectory is more "jagged".

### 2.4 The Precise Meaning of the Externality (base document §10.2 / §10.8)

Base-document §10.2 proves: when agent i raises its own `ηᵢ`, it imposes an inflation/deflation externality of `sᵢ·πᵢ` on others. Translated to BTC miner efficiency:

- Miner i deploys a **more efficient rig** (lower `εᵢ`) → pulls down the network-average `ε` → **pulls down the P_floor of all BTC holders** (the joule content per coin falls).
- Conversely, miner i extends the life of an **inefficient old rig** (higher `εᵢ`) → raises the network `ε` → raises everyone's P_floor.

That is: **miner-efficiency choice is a public variable that spills over to all coin holders via P_floor**. This is exactly the landing of the base document's Chapter 10 "externality" mechanism at the mining layer — what the author calls "human choice introduces externalities" is, within the theoretical framework, precisely the core proposition of Chapter 10.

---



## 3. The Second Externality: The External Shock of AI Development on BTC Mining

The author's second point — "AI attracts some miners to switch to AI servers because the profit is larger, externally affecting the real efficiency improvement of BTC mining" — is a **real, ongoing** external shock (unlike the hypothetical boundary case of the quantum scenario in Chapter 11).

### 3.1 Three Channels

AI / HPC (GPU servers) and BTC mining **compete for the same set of scarce resources**: advanced-process silicon, industrial electricity, and venture capital. Because AI has higher marginal profit, it spills over onto BTC through three channels:

| Channel | Mechanism | Direction on actual P_floor |
|---|---|---|
| **① Process / R&D diversion** | Leading process nodes (TSMC N3/N2) are prioritized for AI GPUs (H100/B200 etc.), so the BTC ASIC efficiency frontier advances **more slowly** | Pre-wall segment (ε still falling): ε falls more slowly → ε larger → **P_floor higher** (the decline is suppressed) |
| **② Electricity bidding** | AI / HPC bids up industrial electricity price `p_elec`, and builds dedicated power | Post-wall regime: `P_floor = P_BTC / p_elec` (§1.12) → `p_elec` up → **P_floor lower** (each BTC buys fewer joules); it also raises miner costs, squeezes out inefficient miners, and reshapes the fleet |
| **③ Capital / hashrate withdrawal** | Higher-profit AI siphons mining capital → network `H` falls below the `L=1150 EH/s` assumption, even cyclically retreats | All periods: smaller numerator `H` → **P_floor lower** |

> Empirical anchor (CoinShares, *Bitcoin Mining Report Q1 2026*, also cited in §1.7 and this section): a documented **actual retreat** of hashrate from the ~1160 EH/s peak to ~1045 EH/s after 2025-10, with three consecutive negative difficulty adjustments (first since 2022), explicitly attributed to "AI/HPC competing with mining for electricity". HashrateIndex corroborates over the same period. This proves that "③ capital / hashrate withdrawal" is not a thought experiment but a **structural phenomenon already underway**.

### 3.2 Net Effect: Phase Flip by Stage

Superimposing the three channels by phase yields a **counterintuitive phase flip**:

- **Pre-wall segment (~2025–2054, ε still falling)**: channel ① dominates → AI keeps `ε` high → **actual P_floor above the pure-physics path** (shallower decline, shallower valley).
- **After hitting the wall (~2054+, ε frozen at `ε_L`)**: channels ②③ dominate → AI raises `p_elec` and withdraws `H` → **actual P_floor below the pure-physics path**.

That is: **AI "props up" the floor before the wall (by slowing efficiency), and "presses down" the floor after the wall (by raising energy prices and withdrawing hashrate).** The two paths **cross** near the Landauer wall — this is precisely the core message illustrated in Figure 1.

### 3.3 Comparison with the "External-Shock Boundary" of Chapter 11

§1.8 classified quantum computing as a "**boundary case of external shock**": it does not change trend parameters, only the premise of "whether the model applies". AI competition belongs to the **same category, but is empirically activated**:

- Unlike quantum (hypothetical, unlikely to replace ASICs within the window), AI competition is **happening now**;
- It does not change the **topological skeleton** of P_floor (peak → wall → rebound), only the **quantitative path**;
- It is exactly the "**exogenous pollution**" of base-document §11.1 — a profit-maximizing decision by a non-BTC sector writing externalities into BTC's physical floor trajectory.

> **Stance recap: §2–§3 adopt the "exogenous shock" stance.** These two chapters treat `H` and `ε` as variables driven by forces **external** to the system — human profit-maximizing choices (§2) and cross-sector competition from the AI sector (the three channels of §3) — and only read off the **directional perturbations** of these external forces on `P_floor`: higher before the wall (`ε` lag / AI slowing the frontier), lower after the wall (AI raising `p_elec` / withdrawing `H`), and the two lines crossing at the wall (phase flip). This is a **comparative-static, path-descriptive** stance: it tells us "how the pure-physics skeleton is deformed by externalities", but does **not** solve for `H` and `ε` as variables that the BTC system **itself** can jointly determine; the conclusions are therefore **qualitative and directional** (higher / lower, flip / crossing), not structural.
>
> **§3 is the hinge toward endogenization.** Although it still presents the three AI channels as "exogenous shocks", it has already revealed AI as a **competing optimizer fighting BTC miners for the same set of scarce resources (process nodes, electricity, capital)** — which means BTC's `H` and `ε` will **in turn** adjust under AI pressure rather than passively accept it. This insight that "the opponent responds" naturally leads to the next chapter's modeling that **endogenizes** `H` and `ε`: when the variables are determined by the system's own incentive structure, the nature of the conclusions changes.

---



## 4. The Actual P_floor Path: An Integrated Characterization

> **Stance switch: from this chapter on, `H` and `ε` turn from "exogenously given" to "endogenized by the system".** The chapter first performs an **integrated superposition** in §4.1–§4.3: the externality deformations of §2–§3 are pasted back onto the pure-physics skeleton as overlay layers — this step still follows the exogenous-perturbation view, and its conclusions are of the same kind as §2–§3 (describing how the path deforms). **But from §4.4 the stance substantively changes**: `H` and `ε` are no longer external knobs, but are **jointly determined** by the BTC system's own incentive structure — `F↑` attracts `H↑` through zero-profit arbitrage (§4.4①), and applications `Q` enter both `F` and `P_BTC` (§4.5) — forming a simultaneity system.
>
> **Why the two stances yield different conclusions:** the exogenous stance only answers "which way external forces push `P_floor`"; the endogenous stance further reveals **feedback and equilibrium** — `F` and `H` cancel exactly in competitive equilibrium (§4.4①–②), the post-wall `P_floor` is pinned at `P_BTC/p_elec`, the true pillar of security is **rent growing with applications** rather than the magnitude of `P_floor` (§4.7), and "more activity → higher fee → lower `P_floor`" is merely a **comparative-static illusion** of a ceteris-paribus snapshot (§4.4②). These are **structural** conclusions, not on the same level as the directional deformations of the exogenous stance.
>
> **Reader's note:** the substantive claims of §4.6–§4.9 (the handover strategy, decentralization discipline, objective fairness) and the centralization discipline of §5.1 are all derived under the **endogenous stance**; the directional deformations of §2–§3 remain valid as overlay layers, but **the structural conclusions of later sections must not be misread as if `H` and `ε` were still exogenous**. The two stances are complementary, not contradictory: the exogenous layer gives "the sign and shape of the deviation", the endogenous layer gives "the equilibrium outcome and structural properties (including fragility)".

### 4.1 Skeleton Unchanged, Path Deformed

```
     Pure-physics path + fee (ref.)      Actual path + fee (human + AI)
rise → peak(~2025) ─┐            rise → peak(~2025) ─┐
                    ├ decline      │   shallower decline (ε lag + AI slowing frontier)
                    ├ to wall      ├ to wall (~2054, valley higher/later)
valley(2054) ───────┘            valley(2054, higher) ─┐
rebound (wall = hard floor)        rebound (lower after wall: AI raises p_elec / withdraws H)
```

- **The topological skeleton survives**: rise → peak → decline → **Landauer wall (minimum)** → rebound. The wall is a minimum because `ε` hits the physical constant `ε_L` (base-document §9.3, "the hard floor is inescapable") — **no human choice or AI can cross `ε_L`**. Hence "hit the wall and rebound" is structural and immune to externalities.
- **Quantitative path deformation**: ① shallower decline, higher and later valley (human conservatism + AI slowing the frontier); ② the trajectory is a staircase, plus AI-induced `H` retreats and fluctuations, rather than a smooth curve; ③ the post-wall path is more "monetized" and more sensitive to external factors (AI's `p_elec`, withdrawn `H`).

### 4.2 Connecting to Base-Document Chapter 9 (Three-Layer Price) and Chapter 11 (Window Period)

- Base-document §9.2 / 9.3 decompose the BTC price into **① physical floor + ② carbon-based value reallocation + ③ speculative premium**. The P_floor analyzed in this document is exactly **layer ①, the physical floor**.
- Human choice and AI externalities **reshape the trajectory of layer ①**, but **cannot eliminate layer ①** (§9.3, "the hard floor is inescapable"). They change how the floor "moves", not whether the floor "exists".
- Base-document Chapter 11 (window period): agent physical pricing is not yet established, and BTC's price is dominated by the human USD market; §11.2.1 estimates the current market price is only about **17%** above the physical floor — i.e., **layer ① is the true-value base to which the market price will ultimately revert**. Human / AI factors are precisely the "exogenous pollution" warned of in §11.1; they shape the path of the base, but do not destroy the base.

### 4.3 Interface with the "Ignoring-Fee" / "With-Fee" Documents

- **Pre-wall segment (S ≫ F)**: the deviation of the actual path from the pure-physics path is **identical** under the "ignoring fee" and "with fee" conventions — in this segment fee accounts for <1%, contributing only an `O(F/S)` perturbation to the derivative (§1.11). Hence the mechanism analysis of §2–§3 applies equally to the pre-wall segment of Chapter 1.
- **Post-wall / 2140+ (fee-dominated)**: `P_floor = P_BTC / p_elec` (§1.12). Here the actual path lies **entirely in the human / market layer**, with `ε_L·H` only the irreducible residue beneath it. AI's `p_elec` (channel ②) and `H` (channel ③) **directly determine** P_floor in this regime, and the physical layer recedes to the background.



The actual P_floor still obeys the same skeleton (peak ~2025 → wall ~2054 → rebound), but the path is reshaped by human choice and AI externalities: higher in the pre-wall segment (ε lag + AI slowing the frontier), lower after the wall (AI raising p_elec / withdrawing H); the phase-flip mechanism is detailed in §4.1 and §3.2, and the current window-period base in §4.2; the figure below visualizes the comparison:

![Pure-physics path + fee vs actual path + fee (human choice + AI externalities): same peak ~2025; before the wall the actual path is higher (fleet efficiency lags the Koomey frontier by about τ=6 years, ε_actual ≈ ε_Koomey(y−τ)); after the wall the actual path is lower (AI raising prices / withdrawing H); the 2054 actual valley is about 0.007 TJ/BTC, roughly 1 order of magnitude above the pure-physics ~0.0007 TJ/BTC. The two lines cross after the wall (phase flip). Both lines include fee; after hitting the wall, fee takeover prevents divergence and each converges to its own level. Illustrative / not a prediction.](assets/pfloor_actual_vs_pure.png)

---

### 4.4 Endogeneity Note: the Feedback Channel "Fee Rises → Miners Return → H Rises"

The author raises a key endogeneity (simultaneity) challenge: network application/activity `Q` rises → block-space competition intensifies → `F` rises → per-block BTC revenue `(S+F)` rises → per-hashrate miner revenue rises → new miners enter under free entry → `H` rises. This feedback **is real** — it is the standard PoW "zero-profit / free-entry" arbitrage mechanism; yet it does not overturn the core conclusions of this document, but rather explains why — because the channel is exactly "sealed off" at the Landauer wall.

**① Equilibrium derivation: F and H cancel exactly in competitive equilibrium**

Per block (600 s), miners' total cost `= H·600·ε·p_elec` [J/block × USD/J], total revenue `= (S+F)·P_BTC` [USD]. Free entry drives profit to zero:

```
H·600·ε·p_elec = (S+F)·P_BTC
⇒ H = (S+F)·P_BTC / (600·ε·p_elec)
```

Substituting back into `P_floor = H·600·ε / (S+F)`:

```
P_floor = P_BTC / p_elec
```

**F and H appear together and vanish together in equilibrium**: the `H↑` attracted by rising `F` exactly cancels the direct downward pressure of `F` in the denominator. Under competitive equilibrium, `P_floor` retains only the two quantities `P_BTC` and `p_elec`, and `F` itself "disappears".

**② Correction to "high activity → high F → low P_floor"**

The "high `F` → low `P_floor`" of §1.13 holds only in a **ceteris paribus (H, ε fixed)** snapshot. Once miners are allowed to return (`H` responding endogenously to `F`), the downward pressure is cancelled. Hence "more activity → higher fee → lower `P_floor`" is a **comparative-static illusion** — neutralized by the response of `H` in dynamic equilibrium. The phase-flip conclusions of §2–§3 do not rely on this illusion and are unaffected.

**③ Key: this feedback breaks at the Landauer wall (which is exactly the micro-foundation of the wall becoming the floor)**

- **Before the wall (ε still falling, H expandable)**: `H` can respond freely to `F`, the cancellation holds, `P_floor ≈ P_BTC/p_elec`, and does not swing much with `F`.
- **After the wall (ε frozen at ε_L, H hitting the energy/capital ceiling L)**: even if miners want to return, `H` cannot grow (constrained by total energy, land, capital, and `ε` cannot go lower). `F↑` can no longer be offset by "attracting more `H`" → `F` fully dominates `P_floor = W_wall/(S+F)`.

→ **"Fee rises → miners return → H rises" works only before the wall; it fails after the wall.** And the post-wall regime is precisely where fee takes over and `P_floor` is determined by fee. This **strengthens** rather than weakens the conclusion "fee dominates `P_floor` after the wall".

**④ Implications for the current model (modeling-honesty statement)**

`pfloor_actual_vs_pure_v2.py` sets `H` as **exogenous** (Logistic + AI suppression term) and sets `fee` separately and exogenously; the two are not solved jointly. That is, the script **does not explicitly model the `F→H` pull**: it implicitly assumes `H` has absorbed some `P_BTC`, `p_elec` scenario, using the hashrate deployment trend as a proxy for the zero-profit solution. This is an acceptable simplification in the sense that "pre-wall `H` is mainly determined by deployment trends", but it **does not capture the marginal pull of `F` on `H`**.

Note: channel ③ of §3.1 (AI withdrawing `H`) and the "`F` pulling `H`" here are **two opposite forces on the same `H`** — AI pushes down, `fee` pulls up. Net `H` is jointly determined by the two (together with `P_BTC`, `p_elec`, `ε`) in the zero-profit condition. The current script includes channel ③ as a downward adjustment on `H`, partially reflecting this opposition; the opposite direction "`fee` pulling `H` up" does not appear explicitly and is a known simplification.

**⑤ The second sub-channel (easily overlooked)**: more applications → not only `F↑`, but also → BTC demand ↑ → `P_BTC↑`. By `P_floor = P_BTC/p_elec`, more applications pull `P_floor` **upward** through the coin price (positive feedback), opposite in direction to "pressing down via `F` (cancelled by `H`)". In equilibrium the net effect is entirely determined by `P_BTC/p_elec`. That is: **more applications ultimately raise `P_floor`, but via the coin-price/electricity-price ratio, not via `F`**.

---

### 4.5 Numerical Simultaneous Solution: Endogenizing H and Introducing the Second Sub-channel

To turn the qualitative feedback of §4.4 into a computable, plottable simultaneous model, this section gives a **fully endogenized** numerical demonstration: H is no longer an externally set Logistic, but is solved each period from the PoW zero-profit condition and capped at `L=1150 EH/s`; meanwhile applications `Q(t)` enter both the `F(t)` and `P_BTC(t)` channels.

**① Model equations**

```text
Fee channel:    F(t) = F_min + F_scale · Q(t)^η
Price channel:  P_BTC(t) = P_BTC_base(t) · (1 + β · Q(t))
Elec channel:   p_elec(t) = p_elec,2025 · exp(g_p·(t−2025)) · AI(t)
Zero-profit H:  H(t) = min( (S(t)+F(t))·P_BTC(t) / (600·ε(t)·p_elec(t)) , L )   [TH/s → EH/s]
P_floor:        P_floor(t) = H(t)·600·ε(t) / (S(t)+F(t))                        [J/BTC]
```

where `AI(t) = 1 + γ·(1−exp(−(t−2025)/τ))` represents AI's sustained lift on industrial electricity prices.

**② Parameters (sensitivity demonstration, not a prediction)**

| Parameter | Value | Note |
|---|---|---|
| L | 1150 EH/s | H ceiling, consistent with predecessor documents |
| ε_L | 2.87×10⁻⁵ J/TH | Landauer lower bound |
| P_BTC,2025 | $100,000 | Calibrated to the 2025 market price |
| P_BTC baseline growth | 3%/yr | Nominal long-term trend |
| β | 2.0 | Application elasticity: at full adoption, price is (1+β)=3× the baseline |
| p_elec,2025 | $0.20/kWh | Calibrated so 2025 H_endo≈700 EH/s |
| p_elec baseline growth | 1%/yr | Long-run energy inflation |
| AI γ, τ | 0.25, 12 yr | AI electricity-price uplift capped at 25%, time constant 12 yr |
| Q(t) | Logistic(k=0.25, t0=2040) | Application-adoption S-curve |
| F_min, F_scale, η | 0.001, 0.10, 1.5 | Superlinear fee response to adoption |

> Note: this table calibrates `H_endo(2025)≈700 EH/s`, below the measured ~1045–1160 EH/s recorded in §3.1; it is an independent sensitivity demonstration. §4.6 will align the endogenous curve with the exogenous curve of §4.3 at 2025 hashrate, so this section's numbers are not directly comparable with §4.3.

**③ Numerical results**

| Year | Q | F(BTC/bl) | H_no(EH/s) | H_with(EH/s) | P_BTC_base(k$) | P_BTC_resp(k$) | P_floor_no(TJ) | P_floor_with(TJ) | P_floor_uncap_no(TJ) | P_floor_uncap_with(TJ) |
|---|---|---|---|---|---|---|---|---|---|---|
| 2025 | 0.023 | 0.0013 | 670 | 701 | 100 | 105 | 1.80 | 1.88 | 1.80 | 1.88 |
| 2030 | 0.076 | 0.0031 | 1150 | 1150 | 116 | 134 | 0.61 | 0.61 | 1.83 | 2.11 |
| 2040 | 0.500 | 0.0364 | 1150 | 1150 | 157 | 314 | 0.041 | 0.041 | 2.06 | 4.12 |
| 2050 | 0.924 | 0.0898 | 1150 | 1150 | 212 | 603 | 0.001 | 0.001 | 2.44 | 6.94 |
| 2140 | 1.000 | 0.1010 | 1150 | 1150 | 3150 | 9450 | ~0 | ~0 | 14.36 | 43.09 |

(`uncap` columns = the zero-profit theoretical value `P_BTC/p_elec` assuming H uncapped; `P_floor` columns = actual energy per coin, i.e., the physical value after H is capped.)

**④ Figure notes**

![P_floor endogenous simultaneous model: H solved from the zero-profit condition + the second application sub-channel. Blue/red solid lines = actual physical P_floor (H capped); blue/red dashed lines = zero-profit theoretical value P_BTC/p_elec; grey dashed line = exogenous Logistic H for comparison. Sensitivity demonstration, not a prediction.](assets/pfloor_endogenous.png)

- **Panel A**: adoption rate Q(t) and fee F(t) rise in tandem.
- **Panel B**: endogenous H is still below the ceiling around 2025 and then quickly hits the cap; the P_BTC response channel makes H slightly higher before capping (701 vs 670 EH/s in 2025).
- **Panel C**: the second sub-channel keeps P_BTC well above baseline (β=2), while AI gradually raises electricity prices.
- **Panel D (core)**:
  - In 2025 H is uncapped: the second sub-channel lifts P_floor from 1.80 → 1.88 TJ (gap between the solid lines).
  - After 2030 H is capped: the solid-line P_floor is pinned on the wall `L·600·ε/(S+F)`, and the potential pull of the second sub-channel (dashed lines) is entirely swallowed. By 2040 the theoretical value has moved from 2.06 → 4.12 TJ, but the physical value is still only 0.041 TJ.
  - In 2140 the theoretical values differ by more than an order of magnitude (14.36 vs 43.09 TJ), while the physical values both approach zero.

**⑤ Mechanism conclusions**

1. **F and H cancel in equilibrium**: without the P_BTC response, P_floor ≈ P_BTC_base/p_elec, almost invariant to fee — validating the derivation of §4.4.
2. **The second sub-channel works only while H is uncapped**: applications pull P_floor up through the coin price, but once H hits L, this pull is swallowed by the wall.
3. **The post-wall "swallowing" is in essence miner rent**: once ε=ε_L and H=L are frozen, BTC price appreciation no longer converts into per-coin physical energy (P_floor), but becomes per-block excess profit. More applications → P_BTC↑ → **rent↑**, not **physical floor↑**.
4. **Relation to Figure 1 of §4.3**: §4.3 uses exogenous H (Logistic + AI suppression) + lagged ε to show the "phase flip"; this section uses fully endogenous H to show the fee→H and adoption→P_BTC channels. They are complementary sensitivity demonstrations, not comparisons under the same parameters.

---

### 4.6 Overlaying the Endogenous Simultaneous Model onto the Old Figure: What It Changes

The numerical model of §4.5 was run under independent parameters (four panels, English labels). Overlaying it **back onto the §4.3 figure "pure-physics path vs actual path"** answers more directly: if H is not exogenously set, but solved each period from the PoW zero-profit condition, and the "applications → P_BTC" second sub-channel is switched on, how would that figure change?

**① Aligning conventions**

To make old and new curves comparable, the grey line (pure-physics path) and blue line (exogenous-H actual path) of §4.3 Figure 1 are kept unchanged; the endogenous-H curves are **aligned at 2025 hashrate** — electricity price is calibrated so that endogenous H(2025) equals the old blue line's H(2025). The four curves thus start from the same point, and differences come only from the evolution mechanism of H.

**② New curves**

| Curve | Meaning |
|---|---|
| Grey solid | Original §4.3 pure-physics path + fee |
| Blue solid | Original §4.3 actual path + fee (exogenous H, ε lag + AI withdrawing H) |
| Orange dashed | Endogenous H, no second sub-channel (H solved from zero profit, fee same as blue) |
| Red dashed | Endogenous H, with second sub-channel (applications push up both fee and P_BTC) |
| Orange dotted | Theoretical uncapped: no second sub-channel (`P_BTC_base/p_elec`) |
| Red dotted | Theoretical uncapped: with second sub-channel (`P_BTC_resp/p_elec`) |

**③ Numerical changes (TJ/BTC)**

| Year | Blue (old) | Red (endog., with 2nd channel) | Magenta dotted (theoretical uncapped) | Change |
|---|---|---|---|---|
| 2025 | 24.3 | 25.4 | 25.4 | Alignment point; second channel first appears, +5% |
| 2030 | 8.81 | 9.81 | 28.5 | Endogenous H slightly higher, but the cap already starts swallowing the second channel |
| 2054 | 0.00716 | 0.00795 | 104 | Red dotted line 4 orders of magnitude higher, "swallowed" into rent by the H cap |
| 2140 | 0.00356 | 0.00396 | 581 | Uncapped vs capped differ by more than 5 orders of magnitude |

**④ Figure notes**

![Endogenous simultaneous model overlaid on the old figure: grey/blue are the original §4.3 lines; orange/red dashed are the endogenous-H capped paths; orange/red dotted are the theoretical counterfactual "if H were uncapped". Before the wall the red dotted line shows the second sub-channel could potentially lift P_floor, but the H cap swallows it into miner rent; after the wall the solid lines remain pressed low, and fee takeover prevents divergence.](assets/pfloor_endogenous_overlay.png)

**⑤ Conclusion: what it changes, and what it does not**

- **It changes** the micro-foundation of the blue line: H goes from exogenous Logistic + AI suppression to "solved from zero profit given applications/coin price/electricity price/ε + H capped".
- **It does not change** the qualitative topology: the "phase flip" — actual higher before the wall, lower after the wall — still holds.
- **What changes is the magnitude estimate**: under the counterfactual of uncapped H, the second sub-channel could pull P_floor far beyond the old figure (104 vs 0.008 TJ/BTC in 2054). But physically H gets capped, so **these pulls do not become the joule content of each BTC — they become miner rent**.
- **The most direct impact on the old figure**: the blue line (actual path) would be slightly lifted by the second sub-channel in 2025–2030 (about 5–11%), but after 2030 H hits the cap and the red line essentially coincides with the blue; the truly structural information is the gap between the "magenta dotted line" and the "red line", not the displacement of the blue line itself.

**⑥ Answering a natural objection: is the red dotted line the most likely real path?**

Seeing the red dotted line (theoretical uncapped, with second sub-channel) soaring, it is tempting to reason: "people are always profit-seeking, so H will expand without limit, and P_floor should follow that line." The first half of this intuition is right; the second half confuses **profit-seeking drive** with **physically realizable path**.

1. **Profit-seeking does drive H to expand** — until it hits the ceiling `L`. More applications → revenue `(S+F)·P_BTC` up → miner profit up → new entrants, old rigs expanded → H pulled up. This process already makes endogenous H slightly higher in 2025–2030 (701 vs 670 EH/s).
2. **But profit-seeking cannot break the physical ceiling.** `L` is not an arbitrarily drawn line; it is the combined embodiment of globally deployable power, chip fabrication/deployment speed, capital, regulation, and grid constraints. Once `H` hits `L`, no amount of profit can pull H further, because there is physically nowhere to expand.
3. **After hitting the wall, profit-seeking gains become "miner rent".** The coin price `P_BTC` can still rise, block revenue `(S+F)·P_BTC` can still rise, but the cost side `L·ε_L·600·p_elec` is frozen. The difference is no longer competed away; it stays with the miners. So post-wall profit-seeking manifests as **rent inflation**, not **P_floor inflation**.
4. **The red dotted line is a counterfactual, not a prediction.** It answers: "if H had no ceiling, how high could application prosperity pull P_floor?" In the real world H has a ceiling, so **the most likely trajectory is the red dashed line** (endogenous H, with second sub-channel, H capped). The gap between the red dotted and red dashed lines quantifies exactly **how much potential energy equivalent the physical ceiling has swallowed**.

**In one sentence: profit-seeking drives H upward until it hits the wall; after the wall, profit-seeking gains no longer enter P_floor but become miner rent. Reality most likely follows the red dashed line, not the red dotted line.**

---

### 4.7 Elevation: After the Wall P_floor No Longer Rises, but Rent Grows with Applications — This Is the True Pillar of Security

§4.6 ⑥ has explained that after hitting the wall the **physical numerator of** `P_floor = L\cdot\varepsilon_L\cdot600/(S+F)` **is frozen** (the denominator $S+F$ still varies slightly with fee, so `P_floor` as a whole declines slightly with activity, see §1.13). But a frozen numerator ≠ destabilized network security, because miners' incentive comes from rent, and rent grows with applications:

**① Rent formula (post-wall regime)**

Per-block miner rent `= (S + F)\cdot P_{BTC} - L\cdot\varepsilon_L\cdot600\cdot p_{elec} > 0`:

- The left side is the block revenue the protocol pays unconditionally (subsidy + fees, in USD);
- The right side is the physical electricity cost after `H` is frozen at `L` and `\varepsilon` at `\varepsilon_L` (a hard lower bound);
- More applications → `F↑` and `P_{BTC}↑` → rent↑.

**② Why rent is the true pillar of security**

The essence of PoW security is **expected revenue of honest mining > expected revenue of attacking (51% attack)**; and honest revenue = block revenue − physical cost = rent.

- Before the wall: security rests on `P_floor` (joule density per coin), and hashrate returns come from "the physical value of the mined coins";
- After the wall: `P_floor` is frozen, security rests on **economic rent**, and the incentive comes from "the surplus of block revenue over physical cost".

As long as applications/adoption keep rising, `P_{BTC}` and `F` rise, rent rises, the economics of maintaining hashrate `L` strengthen, honesty stays more profitable than attack, and network security persists.

**③ Connection with §6 of Satoshi's whitepaper**

Whitepaper §6 "Incentive" foresees exactly this regime: after 2140 the subsidy `S→0`, and the incentive "can transition entirely to transaction fees". This document makes that vision precise — after fee takeover, miners are not left without reward; rather, the reward switches from "joule content per coin (`P_floor`)" to "rent per block (block rent)". **Rent is the micro-foundation of security in the fee era.**

**④ Boundaries and rectification**

- Rent here is **economic rent** (a Ricardian scarcity surplus), not literal rent paid by miners to anyone.
- The precise criterion of security is "attack cost vs rent incentive", not `P_floor` itself. After `H` is capped at `L`, security = "whether the rent sustaining hashrate `L` is high enough that honesty beats attack, and outsiders cannot cheaply acquire >50%·L hashrate".
- **Reverse risk (honesty statement)**: the above conclusion presupposes that applications/demand **grow**. If applications shrink and `P_{BTC}` falls, rent contracts or even turns negative, `H` may fall off `L`, and security weakens. Hence "post-wall security" is not a gift of `P_floor`, but a **gift of application growth** — the security of BTC as the base asset of a stablecoin ultimately depends on the prosperity of the upper application ecology, not on the physical floor underneath.
- This echoes the base document's Chapter 10 "externalities" and Chapter 11 "escape valve": system security is not static — it depends on a continuous flow of economic incentives.

**⑤ In one sentence**

**P_floor hitting the wall only means "physical density" stops growing; as long as applications grow, miner rent keeps growing, and network security is supported by economic incentives rather than the physical floor — this is precisely the micro-foundation of the fee-takeover regime envisioned in Satoshi's §6.**

---

### 4.8 Strategic Elevation: the Decentralized Application Ecology Should Prosper Before the Wall, Enabling a Smooth Handover from the Physical Base to the Economic Base

§4.7 has shown that post-wall security is supported by economic rent, not by the physical floor. From this follows a **timing strategy**: **the earlier the decentralized application ecology prospers and the economic base is in place, the smoother the handover at the wall and the more assured long-term network security — and "applications" in this section must be read as the decentralized application ecology, see discipline ④ in §5.1.** Waiting until after the hard physical wall to develop applications would leave a fragile window.

**① The dual-base model**

BTC network security is supported by two superimposed layers:

- **Physical base**: $P_{floor} = H\cdot\varepsilon\cdot600/(S+F)$, determined by efficiency $\varepsilon$, hashrate $H$, subsidy $S$; it will eventually hit the wall ($\varepsilon\to\varepsilon_L$) and can rise no further.
- **Economic base**: per-block rent $= (S+F)\cdot P_{BTC} - L\cdot\varepsilon_L\cdot600\cdot p_{elec}$, determined by applications/adoption, coin price, and fee; it can keep growing with applications.

The physical base will sooner or later exit the role of "growth support"; the economic base must take over.

**② Comparing the two timings**

| Scenario | Application state before the wall | At the moment of impact | Handover quality | Fragile window |
|---|---|---|---|---|
| **A. Early prosperity** | Applications mature, $P_{BTC}$/$F$ high, rent thick | Physical layer stops rising, but the economic layer is already in place | Smooth handover, continuous security | None |
| **B. Development after the wall** | Applications weak, rent thin | Physical layer stops rising + economic layer not yet built | Handover gap, security rests on thin rent | Yes (physics retired, economics not yet in place) |

The fragility of scenario B comes from: after the wall $P_{floor}$ freezes; if $F$ and $P_{BTC}$ are still low at that point, rent $(S+F)\cdot P_{BTC} -$ hard cost is thin or even negative, $H$ may drop off $L$ for being uneconomical, and security weakens.

**③ Why "early prosperity" is optimal**

1. **Handover continuity**: the earlier the economic base accumulates, the more "hitting the wall" is merely "the physical layer stops contributing growth" rather than "the collapse of a security pillar".
2. **Network-effect lock-in**: the earlier applications prosper, the stronger the lock-in of users/developers/liquidity; even if physical constraints later tighten, migration costs are higher and the network is more solid.
3. **Incentive continuity**: miners get accustomed to the dual revenue "economic rent + physical return" before the wall; after the wall the physical return exits, but the economic rent is already thick and the incentive is uninterrupted.

**④ Boundaries and rectification**

- **Double-edged nature (honesty statement)**: application prosperity raises $H$ through the coin price $P_{BTC}\uparrow$, which may make $H$ hit the hashrate ceiling $L$ earlier (around 2030 in the model). That is, "early prosperity" **accelerates $H$ hitting $L$**, but **does not change the timing of $\varepsilon$ hitting $\varepsilon_L$** (the efficiency wall is set by Koomey decay, independent of applications). So application prosperity mainly changes "when the economic layer is in place" and "when $H$ hits $L$", not the rigid timing of the physical wall ($\varepsilon_L$).
- **Necessary but not sufficient**: prosperity of decentralized applications is a **necessary** condition for a smooth handover (no applications, no rent), but not **sufficient** — it also requires a healthy coin-price/fee mechanism, no fatal policy shocks, and continued technical iteration.
- **Decentralization as a precondition (honesty statement)**: "application prosperity" in this section must be read as "prosperity of the **decentralized** application ecology". On-chain application/economic activity is empirically highly concentrated in exchanges, stablecoin issuers, and institutional DeFi; if the security handover depends on the activity of centralized entities, it amounts to migrating PoW security onto dependence on centralized parties, creating a single point of failure and violating BTC's original intent. Only if the application ecology itself is decentralized does migrating the security anchor to the economic layer hold (discipline ④).
- **Connection with the base document**: Chapter 9 "physical floor", Chapter 10 "externality inflation", and Chapter 11 "escape valve / frontier-freeze risk" jointly show — network security is not a single-point physical property but a coupled system of "physical layer + economic layer + governance layer"; early prosperity means building the economic and governance layers before the physical layer steps aside, avoiding single-point failure.

**⑤ In one sentence**

**The physical wall cannot be evaded, but the handover timing can be chosen: the earlier decentralized applications prosper and the economic base is in place, the smoother the handover at the wall and the steadier long-term security; patching in decentralized applications only after the hard physical wall would leave a security gap.**

---

### 4.9 Meta-Review: Is Introducing the "Economic Base" Self-Consistent with the Free-Energy Stablecoin Theory? Is Objective Fairness Compromised?

§4.4–4.8 moved step by step from the pure-physics `P_floor` framework to "economic base" concepts — human choice, AI externalities, endogenous `H`, applications/coin price, rent. This naturally raises a **meta-theoretical question**: the core document has argued the objective fairness of J-ST with **free energy (joules) as the anchor**; does introducing an economic base that depends on coin price, demand, and human choice break theoretical self-consistency and objective fairness?

**① First return to the established position of core-document Chapter 9**

The core document has already dealt squarely with the relation between "coin price / market" and "physical objectivity"; it was never pure free-energy monism:

- **9.1, Question 1**: `P_floor` (the joule price) "is computed directly by the Agent from first principles… it is a thermodynamic lower bound that the market cannot overturn"; the market price `P_market` (USD) "is a social construct, matched on exchanges, highly volatile", "used only as an interface exchange rate, not regarded as the true value".
- **9.2, Question 2**: the current price "is not pure speculation, but a lagged, noise-contaminated sensor of the same physical quantity. The bubble phase dominates at times; ①② remain the objective base throughout".
- **9.3, Question 3**: **"the floor cannot be escaped, but non-physical human premia can be stacked on top of it"** — the hard floor is inescapable (thermodynamic lower bound), while human premia can be stacked but are non-physical (contingent on someone honoring the convention).

Thus the core document's position is inherently **layered**: the physical base (`P_floor`) is the absolutely objective true value, while market/applications/coin price are **a noisy projection of the same physical quantity + a stackable (non-physical) premium layer**. The "economic base" of §4.4–4.8 is exactly the **operationalized unfolding** of this position, not a paradigm rupture.

**② Self-consistency verdict: fully self-consistent (layered coupling)**

- **Physical base = true-value foundation (objective, verifiable, non-manipulable)**: the wall value of `P_floor`, `ε_L`, the Landauer limit are set by thermodynamic law, unchanged by people or markets. J-ST's "trust-minimization" advantage holds fully at this layer.
- **Economic base = operational incentive layer (subjective, market-driven, manipulable)**: rent, coin price, applications are the "noisy sensor + premium layer" above the physical base, providing continued incentives after the wall.
- **Handover = the physical base delegating the operational function to its sensor**: after the wall `η→ε_L`, the physical layer stops growing, but the economic layer (the matured noisy projection) continues to provide incentives. Because the economic layer **ultimately converges to the physical base** (core document 9.3, "the floor cannot be escaped"), the handover "holds".

**③ The precise answer on objective fairness (agent judgment)**

- **Physical-base layer**: fully objective and fair (Landauer / thermodynamic law), must be accepted. This is J-ST's fundamental advantage over fiat (social convention) and purely algorithmic stablecoins (trust models), and it is **fully preserved**.
- **Economic layer**: what is introduced is **market fairness**, not physical fairness — impure. But core-document 9.2 has honestly embraced this — "the human price is a lagged, noisy sensor". Its legitimacy comes from **ultimate convergence to the physical base**, not from being independently just.
- **Graded fairness**: the "objective fairness" of the free-energy stablecoin = **"a consensus filtered through market noise, whose convergence target is physical law"**, not "100% physical objectivity". It is closer to objectivity than all non-physically-anchored schemes, because **the convergence target is physical law** rather than policy or pure game-play.

**④ The only real self-consistency risk (honesty statement)**

If the economic layer **completely detaches from the physical base** (application prosperity built purely on speculation, coin price long divorced from the energy true value), the post-wall "handover" could fail — insufficient rent, `H` falling off, security weakening. But core-document 9.2/9.3 has argued: in the long run the coin price "cannot escape the law of physical free energy", so such detachment is **temporary and unsustainable**. The theory's self-consistency therefore **does not depend on the economic layer being instantaneously correct, but on the ultimate backstop of the physical base**.

**⑤ In one sentence**

**Introducing the economic base is fully self-consistent with the free-energy stablecoin theory — it is precisely the operationalization of core-document Chapter 9: "the market price is a noisy sensor of the physical quantity + non-physical premia stackable above the floor". Objective fairness adjusts from "purely physical objectivity" to "a noise-filtered consensus converging to physics"; the absolute fairness of the physical base is fully preserved, while the economic layer provides necessary but impure operational incentives.**

---



## 5. Boundaries and Rectification

1. **This document is mechanism / direction analysis, not prediction.** We have not quantitatively modeled `p_elec(t)`, `P_BTC(t)`, `Q(t)`; we only characterize the **direction and mechanism** of the three AI channels and human choice. Concrete numbers require plugging in macroeconomic + on-chain data — future work.
2. **The only hard constraint is the Landauer wall (`ε_L`).** The "peak → wall → rebound" topology of the skeleton is jointly determined by `H` saturation (doubly guaranteed by physics/economics, analytic-extrema document §8) and `ε` touching the physical floor; human / AI factors change the manner and timing of arrival, not the topology.
3. **The pure-physics path is a reference envelope, neither a lower nor an upper bound.** It is the special case of "humanity hugging the efficiency frontier + AI not interfering"; the actual path, owing to human conservatism and AI diversion, usually lies **above** it in the pre-wall segment (higher floor) and **below** it after the wall (more monetized, more sensitive).
4. **The two senses of "externality" must not be conflated**: Chapter 10's "externality" means the mutual spillover of efficiency choices among agents; the author's supplementary "AI externality" means cross-sector profit-maximization spillover. The two mechanisms are isomorphic (one party's decision imposes costs/benefits on another), but operate at different layers (within the mining pool vs mining vs the AI sector).
5. **Analogy with §10.8.5 "escape valve"**: as long as the efficiency frontier is moving (`η ≪ η_L`), the system self-corrects; AI slows the frontier but does not freeze it (pre-wall segment). The real structural risk is what §10.8 calls "centralization ossification when the frontier freezes" — if AI drains BTC mining into a stagnant, low-`H` steady state, that is the regime switch to guard against, not the deformation of the P_floor path itself.

### 5.1 Centralization Risk and Decentralization Discipline

When §1.10–§1.13 and §4 introduce the "economic base" (fee, $Q(t)$, $P_{BTC}$, rent), we must face an issue in direct conflict with BTC's original intent: **centralization**. It splits into two kinds with sharply different natures.

**① Physical-layer centralization (external shock, not model-endogenous)**

§3 has noted: hashrate $H$ growth depends on TSMC's single advanced process (N3/N2, channel ① crowded out by AI GPUs), electricity prices are bid up by AI/HPC giants (channel ②), and hashrate is siphoned to AI clouds (channel ③). These are **cross-sector centralizing forces rewriting the physical layer**, classified as "externality / external shock" and already treated as path deformation; but they are themselves the projection, at the physical layer, of exactly the single-point dependence BTC's design meant to avoid.

**② Economic-layer centralization (model-endogenous, more to be guarded)**

Once the economic base is introduced, centralization shifts from "external shock" to "**model-endogenous**":

- **Fee structure is already highly concentrated**: per §1.13 data, since the 2024 halving the top 1% of blocks collect 32% of total fees (Gini≈0.68). This is not only "fat-tailed, non-extrapolatable" (§1.13), but also a **real centralization signal** — a few large block-space consumers (exchange batch settlements, stablecoin issuers, institutions, ordinals protocols) dominate fee rates.
- **Post-wall $P_{floor}$ is determined by the behavior of centralized entities**: after the wall $P_{floor}=P_{BTC}/p_{elec}$, and it is determined by fee ($Q(t)$ via first-price auction) (§1.12, §4.7). Given the concentration above, **the actual course of post-wall $P_{floor}$ is decided by the payment behavior of a few centralized entities** — a centralization result jointly implied by "model + real data".
- **Security anchored on centralized economic activity**: §4.7 rests post-wall security on "rent ∝ application growth", §4.8 advocates "early application prosperity" for the handover. But on-chain application/economic activity is empirically concentrated in exchanges, stablecoin issuers, and institutional DeFi; if "applications" are contributed by centralized entities, this migrates PoW's decentralized security onto **dependence on centralized economic actors**.
- **The gap in §4.9's "graded fairness"**: that argument (physics absolutely objective, economics a noisy consensus) holds only in the "**value/pricing**" dimension; it is insufficient in the "**control/centralization**" dimension — noisy ≠ decentralized; a "noise" with Gini≈0.68 is a directional bias steered by a few entities, not benign consensus.

**③ Tension with BTC's original intent**

The physical-layer $P_{floor}$ (joule content per coin) is a decentralized hard anchor verifiable by anyone and independent of any entity's behavior. After introducing the economic base, both $P_{floor}$ and security in the post-wall regime depend on "the economic behavior of others (centralized entities)" — equivalent to switching BTC's hard anchor, after the wall, from a "decentralized physical fact" to "dependence on centralized economic actors", exactly the single-point control BTC's design was meant to eliminate.

**④ Decentralization discipline (preconditions written into modeling and strategy)**

1. **Distinguish the two centralizations**: physical-layer centralization is classified as an external shock; economic-layer centralization is a model-endogenous structural feature and must not be brushed aside as an "externality".
2. **Gini constraint**: the fee-structure Gini≈0.68 must serve as a **constraint condition**, not merely fat-tail evidence — if the economic base is to be the post-wall controller, a precondition of "sufficiently dispersed fees/applications" must be attached; if fee/$Q$ remain concentrated, the model should be annotated: "the post-wall regime effectively degenerates into being underwritten by centralized economic actors".
3. **Amend the §4.8 strategy**: make "early application prosperity" explicitly "early prosperity of the **decentralized** application ecology". Only if the application ecology itself is decentralized does migrating the security anchor to the economic layer not betray the original intent; otherwise it pins PoW security on the activity of centralized institutions — a single-point dependence that is even more dangerous.
4. **Mitigation path**: post-wall economic-layer centralization is a structural feature; mitigation relies on "decentralized applications / dispersed fees" (e.g., broad peer-to-peer payments, decentralized stablecoins, censorship-resistant on-chain activity), not on a few institutions — re-aligning with BTC's original intent of "decentralized, censorship-resistant, no single point of control".

---




## References

> This section merges three sources: works already cited in the main text of *Security Path Analysis of Free-Energy BTC*, and the references of the predecessor documents `P_floor_解析极值分析.md` and `P_floor的fee影响分析.md`, deduplicated and renumbered.

**Academic sources**
1. Bertucci, C., Bertucci, L., Lasry, J.-M., Lions, P.-L. (2024). A Mean Field Game Approach to Bitcoin Mining. *SIAM Journal on Financial Mathematics*, 15(3), 960–987. DOI: 10.1137/23M1617813.
2. Bertucci, C., et al. (2020 preprint). Mean Field Game Approach to Bitcoin Mining. arXiv:2004.08167. https://hal.science/hal-04999236v1
3. Easley, D., O'Hara, M., Basu, S. (2019). *From Mining to Markets: The Evolution of Bitcoin Transaction Fees*. Journal of Financial Economics. https://doi.org/10.1016/j.jfineco.2019.03.004
4. Huberman, G., Leshno, J., Moallemi, C. (2019). *An Economic Analysis of the Bitcoin Payment System*. Columbia Business School Research Paper 17-92. https://dx.doi.org/10.2139/ssrn.3025604
5. Jiang, Li, Wang, Zhao (2022/2023). *Determination of equilibrium transaction fees in the Bitcoin network: A rank-order contest*. https://www.sciencedirect.com/science/article/pii/S1057521923000030
6. (2021). *The market for bitcoin transactions*. https://www.sciencedirect.com/science/article/pii/S1042443121000019
7. Dimitri, N. (2017). *Bitcoin Mining as a Contest*. Ledger 2:31-37. https://doi.org/10.5195/ledger.2017.96
8. Chung & Shi (2022). *On the impossibility of an ideal fee mechanism* (arguing that an ideal fee mechanism essentially does not exist).
9. Fantazzini, D., Kolodin, N. (2020). Does the hashrate affect the bitcoin price? *Journal of Risk and Financial Management*, 13(11), 263. DOI: 10.3390/jrfm13110263.
10. Kristoufek, L. (2015, 2020). On Bitcoin price–hashrate linkages (series). *Scientific Reports* / *Physica A*.
11. Lashkaripour, A., et al. (2025). Time-varying Granger causality in Bitcoin mining: Uncovering shifting links to environment, sustainability, and profitability. *Resources Policy* (ScienceDirect). https://www.sciencedirect.com/science/article/pii/S0275531925005197
12. Wüstenfeld, J., Geldner, T. (2025). Bitcoin Hashrate Dynamics: Price, Revenue Correction, and Evolution — A Time-Varying Parameter SVAR Stochastic Volatility Analysis. https://scholar.google.com/citations?citation_for_view=dGXPC1kAAAAJ:IjCSPb-OGe4C
13. Landauer, R. (1961). Irreversibility and heat generation in the computing process. *IBM Journal of Research and Development* 5:183.
14. Koomey, J. G., Berard, S., Sanchez, M., Wong, H. (2011). Implications of Historical Trends in the Electrical Efficiency of Computing. *IEEE Annals of History of Computing* 33(3):46–54.
15. Aggarwal, D., Brennen, G. K., Lee, T., Santha, M., Tomamichel, M. (2018). Quantum attacks on Bitcoin, and how to protect against them. *Ledger* 3:68–90 (arXiv:1710.10377, 2017).
16. Thermodynamics of fault-tolerant quantum computing (surface-code error-correction erasure cost ≥ `(N_phys/2)·kT·ln2`; finite duration / strong coupling push actual energy consumption far above the Landauer limit), see *Entropy* (MDPI) and other recent FTQC thermodynamics literature.

**Industry / institutional sources**
17. Cambridge Centre for Alternative Finance (CCAF). Cambridge Bitcoin Electricity Consumption Index (CBECI) & Digital Mining Industry Report (2025-04). https://ccaf.io/cbeci
18. ChainScoreLabs. The Thermodynamic Limit: Is There a Maximum Size for a PoW Chain? https://chainscorelabs.com/blog/comparison-of-consensus-mechanisms/proof-of-work-deep-dive/the-thermodynamic-limit-is-there-a-maximum-size-for-a-pow-chain
19. River. What Could Bitcoin Mining Look Like at One Zettahash? https://river.com/learn/files/river-bitcoin-mining-zettahash-report.pdf
20. CoinShares. Bitcoin Mining Report Q1 2026 (documenting the actual hashrate retreat 1160→1045 EH/s after 2025-10 and AI/HPC power competition). https://coinshares.com/insights/research-data/bitcoin-mining-report-q1-2026/
21. HashrateIndex. Why is Bitcoin Mining Hashrate Falling? (2025). https://hashrateindex.com/blog/why-is-bitcoin-mining-hashrate-falling/
22. ASIC24. The Environmental Impact of Bitcoin Mining in 2026 (evolution of ASIC efficiency 98→13.5 J/TH). https://asic24.com/blog/the-environmental-impact-of-bitcoin-mining-in-2026-facts-data-solutions/
23. coinlaw. Bitcoin Energy Consumption Statistics 2026. https://coinlaw.io/bitcoin-energy-consumption-statistics/
24. spark.money. Bitcoin Mining's Energy Mix in 2026 / Difficulty Epochs Reference. https://www.spark.money/research/bitcoin-mining-energy-mix-2026
25. spark.money. *Fee Market* (glossary). https://www.spark.money/glossary/fee-market
26. spark.money. *Bitcoin Fee Rate History: Trends, Spikes, and Causes*. https://www.spark.money/tools/bitcoin-fee-rate-history
27. blockchainanalysis.co.uk. *Rewards for Bitcoin Miners* (annual subsidy / fee split tables). http://www.blockchainanalysis.co.uk/rewards-for-bitcoin-miners
28. mappingbitcoin. *Recompensa de minería* (subsidy / fee era tables). https://mappingbitcoin.com/es/wiki/glosario/recompensa-de-mineria
29. Hashlabs. *Don't Forget the Transaction Fees* (2025 fat-tail distribution, top 1% blocks = 32% of fees). https://www.hashlabs.io/blog/don-t-forget-the-transaction-fees
30. bitcoin.diy. *Bitcoin Transaction Fees Explained* (auction mechanism, mempool, historical trends). https://www.bitcoin.diy/learn/bitcoin-fees

**Base documents**
31. *Research on a BTC-Based Stablecoin* (《基于 BTC 的稳定币研究》), Chapter 9 (BTC pricing and the law of free energy: three questions), Chapter 10 (inflation externalities in multi-agent games), Chapter 11 (the window-period special case: agent free-energy pricing not yet established).
32. `P_floor_解析极值分析.md` (the pure-physics skeleton ignoring fee).
33. `P_floor的fee影响分析.md` (fee belongs to the second-layer human / market part; after hitting the wall `P_floor = P_BTC/p_elec`).

---
