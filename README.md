# Is Antimatter Cheap Yet?

Antimatter Cost Calculator

This project contains a single-file, zero-dependency Monte Carlo based estimator utility for the **cost of antimatter per gram**. It reports the cost as a *distribution* (not a point estimate) with output in nine currencies, compares it to a metric tonne of gold, separates **production** of antimatter from the cost of **shipment**, ranks which assumption owns the uncertainty, and includes a speculative **20-invention sandbox** that lets you apply cost-reduction technologies — individually or as an ordered, cumulative stack — and watch the estimate move. You can snapshot any state to track it over time, and visitors can opt in to a once-a-year email update.

Live page: `https://alex987654.github.io/is-antimatter-cheap-yet/`

It is intended to be a reasoning and communication tool, **not as an authoritative or definitive source**. Every number is a transparent reconstruction of public estimates with editable assumptions; the value is in seeing *which assumption moves the answer*, not in any single figure.

---

## Understanding the estimate — three readings

The same calculation can be explained from three angles. 

### Physics

The estimate is an **energy-floor** cost. To create one gram of antimatter you must supply at least its rest energy, `E = mc² = 9×10¹³ J` (per gram, species-independent — a gram is a gram). Real production is staggeringly inefficient: the wall-plug-to-stored-antimatter energy efficiency `η` is about `10⁻⁹` for antiprotons. (CERN's Antiproton Decelerator yields on the order of `10⁷` antiprotons per cycle, roughly `10¹⁵` per year ≈ 1.7 nanograms per year.) So you spend about a billion times the rest energy, ~`9×10²² J/g`, and at CERN's ~€0.10/kWh that lands near €2.5×10¹⁵/g — the Stefan Ulmer / Physics Girl figure. The model *is* this chain:

```
cost/g = (mc² / η) × price_per_joule × overhead × projection × invention_stack
```

`η` is the dominant uncertain input — it spans about two orders of magnitude across the literature — which is why the tool samples it log-normally and why the sensitivity ("tornado") panel shows η owning the error bar. Species differ only through η and overhead: positrons are far cheaper (effectively higher η; PET-grade sources already exist), antihydrogen is dearer (antiproton + positron + recombination and trap-assembly losses). The presets encode these as different η priors and overhead multipliers.

**Convention note.** `mc²` here is a *single* rest mass. The strict thermodynamic floor for *creating* antimatter is `≥ 2mc²`, because antimatter is pair-produced alongside an equal mass of ordinary matter — which also caps η near 0.5, not 1. That factor of two (≈0.3 dex) is folded into η to match the published convention; it is negligible against the ~4-order-of-magnitude spread. The green "energy floor (η = 1)" marker is therefore a *conventional* floor. A per-draw clamp enforces `cost ≥ floor` so that even the speculative learning-curve projection cannot push a draw below its own η = 1 energy cost — you cannot learn your way below the energy floor.

### Economics

There is **no market** for antimatter — no buyer, no seller, no transaction — so this is a *constructed* (shadow) price: an engineering cost estimate, not an observed equilibrium. The model is a bottom-up, activity-based cost: a variable energy cost multiplied by an **overhead** factor standing in for capital amortization, labor, cooling, and storage. That overhead is where a facility-accounting view enters, and it is deliberately a single editable knob because the underlying question — how much of a multi-program laboratory's budget to *allocate* to antimatter — has no objective answer (the classic cost-allocation problem; it is the most manipulable input in any such estimate).

Two cost structures are kept separate on purpose. **Production** is a *variable* cost scaling with mass. **Shipment** is a *fixed* cost per trip with near-zero marginal cost per added antiproton until trap capacity binds — so today's astronomical per-gram shipment figure is an artifact of a femtogram payload, and the honest unit for shipment is dollars-per-trip, not dollars-per-gram. The **projection** lever is a Wright's-law-style learning curve; note that cost decline in real technologies is exponential (a roughly constant percentage per doubling), not linear, and is highly sensitive to the assumed rate over many years — the tool lets you see that fragility, and floors it at the physical energy floor so the extrapolation cannot run away to nonsense.

Everything is reported as a **distribution** (P5–P95), not a point, because a single figure falsely implies precision the underlying physics does not support; the structural uncertainty (which method, which species) dwarfs the parameter uncertainty. The nine-currency conversion (two of them USD-pegged) is a ~1% layer sitting on a base uncertain by ~10⁴×; it is there for relatability, not accuracy. The gold comparison restates the cost as an opportunity cost in a familiar store of value — tonnes of gold, and multiples of *all the gold ever mined*.

### In plain language

Antimatter is the most expensive material humans know how to make, and this page estimates what a single gram would cost. The key thing it does differently from a headline article is **show the answer as a range**, because nobody actually knows the exact price — there's no shop that sells it.

Why is it so expensive? When you make antimatter in a machine, almost all the electricity you pour in is wasted; you capture only about one-billionth of that energy as actual antimatter. So you're essentially paying a colossal power bill to collect a microscopic speck. The result is somewhere in the range of *tens of trillions to thousands of trillions of dollars per gram*, depending on the assumptions — which is exactly why the tool draws a band instead of one number. To put it in perspective, one gram can be worth more than all the gold ever dug out of the Earth, many times over.

Drag the sliders to change the assumptions and watch the range shift. The **inventions** section lets you pretend humanity has invented better technology — pick one or stack several — and see how much cheaper antimatter could get. But there's a green line that no invention can cross: a hard limit set by physics, because making antimatter will always cost at least the energy stored inside it. Separately, there's a **shipment** section, because moving antimatter is its own challenge — CERN recently drove 92 antiparticles across its campus in what amounts to a one-tonne fridge on a truck. Per gram that looks absurd, but only because the cargo is unimaginably tiny; it's really just a fixed cost per trip.


---

## The model and its constants

```
cost/g = (E_REST / η) × (electricity / J_PER_KWH) × overhead × projection × invention_stack
```

All physical constants are audited and documented inline in `index.html`:

| Constant | Value | Meaning |
|---|---|---|
| `E_REST` | 9.0×10¹³ J | `m·c²` for 1 g (exact 8.988×10¹³; 9e13 by convention, 0.13% high) |
| `J_PER_KWH` | 3.6×10⁶ | 1 kWh = 1000 W × 3600 s |
| `MP_G` | 1.6726×10⁻²⁴ g | antiproton (≡ proton) rest mass in grams — **not** the atomic mass unit (1.6605×10⁻²⁴ g) |
| `G_PER_TONNE` | 1×10⁶ | grams per metric tonne |
| `TROY_OZ_PER_G` | 1/31.1035 | 1 troy ounce = 31.1035 g |

`η` (production efficiency), `overhead`, and `electricity` are sampled **log-normally** and propagated through 12,000 Monte Carlo draws, giving the P5 / P25 / median / P75 / P95 band shown under the headline. The σ used for overhead (0.3 dex) and electricity (0.15 dex) match the perturbations in the sensitivity panel, so the tornado bars are consistent with the distribution that produced them. Presets (`positron`, `antiproton`, `antihydrogen`) set priors that reproduce the literature spread by construction — a transparent reconstruction, **not a measurement.**

A note on two derived readouts: the hero's "N inventions · ±X OOM" badge and the stack waterfall's cumulative figure use the *same* baseline (your current slider values, no stack) and therefore always agree. Both deliberately isolate the **invention stack** and exclude the speculative projection lever, which only affects the headline value.

## Reading the interface

- **Headline band** — median cost/g in USD over a log axis, with the P25–P75 and P5–P95 bands, the published anchors as hairlines (linked to source), and the green energy-floor marker.
- **Inputs** — species preset; η and its uncertainty; overhead; electricity price; and an optional forward projection (years × annual decline).
- **Currencies** — the median and P5–P95 in nine currencies; rates editable in place.
- **Gold** — the cost expressed as tonnes of gold, and as a multiple of all gold ever mined.
- **Sensitivity** — a tornado ranking of how much each input's ±1σ move swings the result (η usually dominates).
- **Distribution** — a histogram of `log10(cost)`.
- **Inventions sandbox** — see below.
- **Shipment** — the BASE-STEP fixed-cost-per-trip model and its crossover point.
- **Snapshots** — save the current state to track how the estimate moves; persists via `localStorage`.

## Production vs. shipment

Grounded in CERN's **BASE-STEP** result: on 24 March 2026 a ~1-tonne cryogenic Penning trap carried **92 antiprotons** by truck across CERN's Meyrin site — the first controlled, reversible transport of antimatter. 92 antiprotons weigh ~`1.5×10⁻²² g`, so at a few hundred thousand dollars per run the *per-gram* shipment cost is ~`10²⁷ $/g` — about twelve orders of magnitude above production. The takeaway baked into the UI: **shipment is a per-trip fixed cost, not a per-gram cost.** Raise the trap capacity and the per-gram figure collapses; the panel shows the crossover payload at which shipment would equal production.

## The 20-invention sandbox

Each invention is a **parameter delta** (e.g. `η ×8`, `overhead ×0.35`, `electricity ×0.35`, trap-capacity `×10⁷`, or shipment `→ 0`), tagged by horizon (**Near** ≤5 yr / **Mid** 5–20 yr / **Far** 20 yr+) and flagged `spec` when speculative. The catalog shows each item's **standalone** effect; clicking adds it to an **ordered stack** whose waterfall shows the cumulative path.

- The **endpoint is order-independent** (multipliers commute); the **path** depends on order, so reorder with ▲▼ to read each step's marginal contribution.
- The green **energy-floor** marker (η → 1) is the limit no stack can cross — even maximally optimistic, grid-priced energy keeps antiprotons near `$10⁵–10⁶ /g`.
- Applying the full stack to antiprotons drops the median ~8 orders of magnitude, into "most expensive real isotope" territory (≈ Cf-252) rather than commodity pricing — consistent with the literature view that kilogram-scale needs ~`10⁹×` improvement.
- The two **shipment** inventions (high-capacity traps, in-situ production) act only on the shipment panel, never on the production headline — keeping the two cost structures cleanly separated.

Inventions are illustrative deltas mapped onto real research directions (collection optics, beam cooling, plasma-wakefield drivers, dedicated facilities, high-capacity traps, laser pair production). They are **not forecasts**, and the multipliers are estimates calibrated to land the full-stack endpoint in a defensible range.

## Sources

The cost anchors in the page link to their canonical origins:

- **NASA $62.5T/g** — Gerrish & Schmidt, *Antimatter Production for Near-Term Propulsion Applications*, NASA NTRS (1999): https://ntrs.nasa.gov/citations/19990080056
- **Physics Girl $2700T/g** — *Why This Stuff Costs $2700 Trillion Per Gram – Antimatter at CERN* (2019): https://www.youtube.com/watch?v=PCuyCJocJWg
- **PBS $3 quadrillion/g** — the same episode on PBS: https://www.pbs.org/video/why-this-stuff-costs-2700-trillion-per-gram-jllccc/
- **Shipment / BASE-STEP** — CERN, *BASE experiment at CERN succeeds in transporting antimatter* (24 Mar 2026): https://home.cern/base-experiment-cern-succeeds-transporting-antimatter/

## Editing the live data

Click **edit rates** to update the nine FX rates and the gold price inline. Defaults are ~18 June 2026 (gold ≈ $4,150/oz; AED and SAR hard-pegged to USD).

## Caveats

This is a reconstruction of public estimates, not an authority. The numbers carry ~4 orders of magnitude of uncertainty, dominated by η and by *which* “antimatter” is meant. Treat the invention deltas as scenario inputs, not predictions. The point of the tool is to make the uncertainty and its drivers legible — not to assert or predict a price.



