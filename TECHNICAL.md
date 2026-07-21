# KitchenMetrics — Engineering Notes

The source of KitchenMetrics is private, but the engineering behind it doesn't have to be. This is a short case study of the most important bug found and fixed during the app's pre-release audit, plus an outline of how the app is built and tested.

## Case study: the thermal model that was wrong in the dangerous direction

KitchenMetrics' Thermal Calculator predicts cooking and pasteurisation times from physics — transient heat conduction into the food — rather than from lookup tables.

The first version used the classic one-term series solution for transient conduction with hard-coded coefficients `A1 = 4/π` and `λ1 = π/2`. Those constants are correct — but only in the **Bi → ∞ limit**, which physically means the food's surface jumps to oven temperature *instantly*. Real ovens transfer heat by convection (h ≈ 12–50 W/m²K), giving Biot numbers around 0.3–0.7, and the surface lags far behind.

The consequence, for a 3 cm chicken breast going from 4 °C to a 74 °C core in a 200 °C oven:

| Model | Predicted time |
|---|---|
| Original (Bi → ∞ assumption) | **7.3 min** |
| Fixed, fan oven (Bi ≈ 0.69) | **25.1 min** — matches published data |
| Fixed, static oven (Bi ≈ 0.33) | 43.5 min |

The original model under-predicted by **3.4×, in the unsafe direction**: it declared chicken pasteurised long before it actually was. A bug like this doesn't crash. It just quietly tells a cook that unsafe food is safe — which is why it was treated as the highest-severity finding of the audit.

**The fix.** The eigenvalue λ₁ is now computed properly by solving `λ1·tan(λ1) = Bi` with bisection, and `A1 = 4·sin(λ1) / (2λ1 + sin(2λ1))`. An oven-mode selector supplies the convective coefficient *h*. The app displays the Biot and Fourier numbers it computed, and flags results where `Fo < 0.2` — the regime where the one-term series itself stops being valid, so the user is warned rather than silently misled.

**How it was caught.** Not by a crash or a test failure — by auditing every number in the app against published sources and refusing to trust output that couldn't be traced to one. The same audit fixed the Choi–Okos food-property coefficients (a wrong carbohydrate density; missing quadratic terms in the specific-heat polynomials) and, just as importantly, *declined* to "fix" one counter-intuitive result: the fryer model shows oil absorption **rising** with temperature, which contradicts kitchen folk wisdom but is what the literature supports (more moisture loss → more porous crust → more uptake). Verifying before changing goes in both directions.

## Case study: the test that failed for the right reason

The same engine models the food's *shape*. It offered three geometries — slab, cylinder, sphere — and two of those are, in the textbook sense, **infinite**: an infinite slab loses heat through one pair of faces, an infinite cylinder only radially. A joint of meat is neither. It is a finite body and it loses heat from every side at once, so modelling it as an infinite slab over-predicts the cook.

The code had also drifted into contradicting itself. When no dimension was typed, the fallback was documented as *"treat the volume as a cube → L = V^⅓/2"* and then solved as an infinite slab: it called it a cube and computed it flat. For a 10 cm cube of meat in a 180 °C oven to a 72 °C core, that is **145.0 min as a slab against 75.2 min as a real cube.**

The fix is the classical product solution (Newman's rule): the centre temperature of a finite body is the product of the 1-D solutions whose intersection forms it — a block is three infinite slabs, a finite cylinder is a cylinder times a slab. It is exact under the assumptions the engine already made, and because every exponent is linear in time it closes in one step instead of iterating.

**What made it worth writing up is how it went wrong.** Two of the new tests were limit cases — the sort that look like box-ticking, because they assert something obvious: stretch one dimension of a block far enough and it must converge on the infinite slab, because a dimension that long cannot matter. Both failed. The elongated block came out at **249 min against the slab's 145** — the long dimension was making the food take *longer* to cook.

The cause is a domain boundary. The one-term series is valid for `Fo > 0.2`; a very long member has `Fo ≈ 0`, far outside it, and there the truncated series does not tend to θ = 1 as the physics requires. It tends to A₁, which is up to 1.27 for a slab. That surplus multiplied into the product and slowed the whole solve. Left in, the more sophisticated model would have been **worse than the one it replaced**, and wrong in the direction that overcooks.

The repair is a physical bound rather than a fudge factor: θ ≤ 1 always, because the centre of a piece of food put in a hot oven cannot be colder than it started. A member whose one-term θ comes out ≥ 1 is not slow — it is out of domain — so it is dropped and the product re-solved without it. A long loin now collapses onto exactly the infinite cylinder, which is the classical answer.

Two notes on discipline, since they are the actual lesson. First, the new shapes return **shorter** times, which is the correct direction and also the unsafe one; so the three original geometries were left byte-for-byte unchanged, the conservative slab remains the default, and the panel states plainly that a shorter time is only right if the food really is that shape. Second, the older tests pin λ₁ and A₁ against Çengel's published table, and these new ones cannot: they assert analytic identities and limits instead. That distinction is written into the test file, and a worked product-solution example read at source is recorded as **still owed** rather than quietly assumed.

## How the app is built

- **Architecture:** a single self-contained HTML/JS application wrapped in an Android WebView shell, with a JS↔Android bridge for persistence (SharedPreferences on device, localStorage in the browser).
- **Costing engine:** one engine aligned with USAR (Uniform System of Accounts for Restaurants): true ingredient yields, ancillary costs, VAT-aware pricing. Recipes support sub-recipes and menu-wide repricing when supplier prices change.
- **i18n:** full English / Italian / Spanish runtime translation, driven by a language order constant so that a fourth language is a dictionary plus one array entry, not a rewrite. An automated check enforces key parity across all three dictionaries and catches duplicate keys — a duplicate is silently dropped by the JS object literal, so it cannot be caught by reading the file, and it has been the recurring bug class in that subsystem.
- **Regulatory data:** the fryer's oil-discard limits are held in a per-country registry rather than a single global number. There is no EU, EFSA, FDA or Codex limit for total polar compounds — the limits are national and they disagree — so the app names the jurisdiction it is quoting, flags the countries where sources contradict each other, and never presents an industry benchmark as if it were law.
- **Testing:** fifteen automated suites, **4943 assertions green** (21 July 2026), run from a single runner. The costing engine, the TPC registry, the thermal geometry and the Vault's per-client data scoping each have their own (31, 173, 164 and 15), alongside a UI smoke suite and the i18n parity check. The counts are quoted from the runner rather than from memory — an earlier version of this page said 138 for the TPC registry, which had been true and had stopped being true. Every physical constant in the app is either traced to a published source or explicitly flagged as an estimate in the internal audit log.

## Contact

Andrea Petrucci — [apetrucci90@gmail.com](mailto:apetrucci90@gmail.com)
Learning journal: [java-exercises](https://github.com/apetrucci90-rgb/java-exercises)
