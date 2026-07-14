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

## How the app is built

- **Architecture:** a single self-contained HTML/JS application wrapped in an Android WebView shell, with a JS↔Android bridge for persistence (SharedPreferences on device, localStorage in the browser).
- **Costing engine:** one engine aligned with USAR (Uniform System of Accounts for Restaurants): true ingredient yields, ancillary costs, VAT-aware pricing. Recipes support sub-recipes and menu-wide repricing when supplier prices change.
- **i18n:** full English / Italian / Spanish runtime translation, driven by a language order constant so that a fourth language is a dictionary plus one array entry, not a rewrite. An automated check enforces key parity across all three dictionaries and catches duplicate keys — a duplicate is silently dropped by the JS object literal, so it cannot be caught by reading the file, and it has been the recurring bug class in that subsystem.
- **Regulatory data:** the fryer's oil-discard limits are held in a per-country registry rather than a single global number. There is no EU, EFSA, FDA or Codex limit for total polar compounds — the limits are national and they disagree — so the app names the jurisdiction it is quoting, flags the countries where sources contradict each other, and never presents an industry benchmark as if it were law.
- **Testing:** the costing engine, the TPC registry and the Vault's per-client data scoping are each covered by automated suites (31, 138 and 15 assertions green at release), alongside a UI smoke suite and the i18n parity check. Every physical constant in the app is either traced to a published source or explicitly flagged as an estimate in the internal audit log.

## Contact

Andrea Petrucci — [apetrucci90@gmail.com](mailto:apetrucci90@gmail.com)
Learning journal: [java-exercises](https://github.com/apetrucci90-rgb/java-exercises)
