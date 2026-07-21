# Changelog

All notable changes to KitchenMetrics. The application source is private; this file records what
changed and, where it matters, **why** — because most of the entries below are corrections, and a
correction without its reason teaches nobody anything.

Dates are the date of the work, not of the Play rollout.

---

## [Unreleased]

Everything in this section is **committed but not yet built into a release**. Version 1.0
(versionCode 4) was approved and distributed to closed testing on **18 July 2026** and is frozen;
the changes below will ship in the next build, versionCode 5.

> ⚠️ **This file has a gap.** Work done on 18 and 19 July — the radiative term in the thermal
> engine, the acrylamide cap, the FDA Food Code cooking temperatures, the per-jurisdiction
> temperature registry, the Choi–Okos verification against the ASHRAE Handbook, and the
> demotion of Italy's 25% TPC figure from *statutory* to *disputed* — is recorded in the private
> repository and in `SOURCES.md`, but was never written up here. It is listed as missing rather
> than backfilled from memory: an entry written days later, from recollection, is exactly the kind
> of claim the rest of this file exists to avoid.

### Added — finite shapes in the thermal model *(2026-07-21)*

- **The food can now be a three-dimensional body.** The engine offered slab, cylinder and sphere,
  and the first two are *infinite* in the textbook sense: an infinite slab loses heat through one
  pair of faces, an infinite cylinder only radially. A joint of meat is neither, so modelling it
  flat over-predicts the cook. Two shapes were added — **Block (3D)** and **Finite Cylinder (3D)** —
  using the classical **product solution (Newman's rule)**: the centre solution of a finite body is
  the product of the 1-D solutions whose intersection forms it. For a 10 cm cube in a 180 °C oven
  to a 72 °C core the difference is **145.0 min as an infinite slab against 75.2 min as a real
  cube**. Dimensions may be left blank and are completed from mass ÷ density.
  *The code had been contradicting itself here: with no dimension typed, the fallback was
  documented as "treat the volume as a cube" and then solved as an infinite slab.*

- **⚠️ These shapes return SHORTER times**, which is the correct direction and also the unsafe one.
  So the three original geometries are unchanged, the conservative slab remains the default, and
  the panel states that a shorter time is only right if the food really has that shape — a block is
  a terrine or a tray bake, not a joint that is thick at one end.

### Fixed — the long dimension that made food cook slower *(2026-07-21)*

- **A limit test caught what inspection did not.** Stretch one dimension of a block far enough and
  it must converge on the infinite slab, because a dimension that long cannot matter. It did not: a
  block 40× longer than its thickness came out at **249 min against the slab's 145**. The one-term
  series is valid for `Fo > 0.2`; a very long member has `Fo ≈ 0`, far outside that, and there the
  truncated series tends not to θ = 1 as the physics requires but to A₁ — up to 1.27 for a slab.
  That surplus multiplied into the product and slowed the whole solve. **Left in, the new model
  would have been worse than the one it replaced, and wrong in the direction that overcooks.**
  Fixed with the physical bound θ ≤ 1: a member whose one-term θ comes out ≥ 1 is out of domain,
  not slow, so it is dropped and the product re-solved. A long loin now collapses onto exactly the
  infinite cylinder.

### Tested *(2026-07-21)*

- `thermal_geometry` **102 → 164 assertions**; the original 102 pass unchanged, which is what
  establishes that nothing already shipped moved. Fifteen suites, **4943 assertions**, all green.
- **What the new assertions are, stated plainly:** analytic identities (a one-member product must
  equal the single-geometry solver exactly) and limit cases. They are **not** checked against a
  published worked example, because none was read at source. Recorded as owed, in the test file and
  here. The older 102 remain pinned to Çengel's published one-term table.
- Documentation counts corrected across both repositories. Four figures had rotted — the TPC suite
  was quoted as 138 and is 173, the load-factor suite as 45 and is 90 — and are now taken from the
  test runner rather than from memory.

### Changed — the fryer no longer states more than the law does *(2026-07-16)*

- **Germany's 24% is no longer called a limit.** It was the only country figure in the app quoted
  without a status and without a legal basis, while all twelve registry entries carry both — and
  the fryer legend called it *"the strictest known limit anywhere"*. The figure originates from a
  **DGF recommendation** (Deutsche Gesellschaft für Fettwissenschaft, 3rd International Symposium
  on Frying Fats, 2000), used by German Länder inspectors as one assessment criterion among
  several, not as a statute. The legend now reads *"strictest known figure anywhere: Germany, 24%
  (DGF recommendation)"* — true whether or not German law also adopts it. A clarification has been
  requested from the DGF; this entry will be updated with their answer.
  *This is the same failure shape as the "EFSA/FDA threshold" removed in July 2025: a plausible
  number attributed to an authority that never set it. It was found in five places outside the
  fryer code, including the Play listing copy.*

- **"Discard oil when TPC exceeds X%" → "Discard oil at or above X% TPC".** Spain requires the
  polar content to be *"inferior al 25 por 100"* (art. 6.3) and Argentina condemns oil at a content
  *"igual o superior al 25 %"* (art. 552 bis). 25.0% is therefore already non-compliant, not the
  last acceptable value. The arithmetic had always treated the threshold as inclusive; only the
  sentence a cook reads was a hair too generous.

- **An absence can no longer be marked "official".** The registry's `confidence` field — the one
  that records whether anyone actually checked a claim — defined `official` as *"checked against
  the primary legal text"*. The United States sat at `official` for the claim that **no such text
  exists**. You cannot read a text that does not exist: nothing had been read, because there was
  nothing to read. It is now `literature`, and the basis says what is true — *"no federal TPC
  limit **found**"*, rather than asserting the FDA has set none.
  The taxonomy now states the distinction plainly: a positive claim is established by **reading**
  a text; an absence must be **attested** by the body itself. There is deliberately no confidence
  value for an attested absence yet — if FSANZ or the DGF answer with one, it will be added as a
  deliberate act with the reply archived, rather than by stretching `official` over it again.
  *`confidence` was also the only field in the registry with no test: 35 assertions were added,
  and mutation-checked by reintroducing the bug to confirm they catch it.*

- **Spain's registry note was backwards and is corrected.** It claimed the law *"bans selling food
  that touched oil at 25% TPC or above — the offence is the dish, not the fryer"*. The primary text
  says the opposite: art. 3 binds whoever uses and handles heated frying oils, art. 6.3 requires
  the oil **in use** to be below 25%, and art. 9.2 prohibits commercialising the used **oil**. The
  offence is the fryer.

### Added *(2026-07-16)*

- **Geometry selector in the Thermal Calculator — slab, cylinder or sphere.** A whole chicken is
  not an infinite slab. The eigenvalue λ₁, the coefficient A₁ and the characteristic length now
  follow the chosen shape (`λ tan λ = Bi` · `λ J₁/J₀ = Bi` · `1 − λ cot λ = Bi`), with Bessel
  functions from Abramowitz & Stegun 9.4.1/9.4.4. All twelve λ₁/A₁ pairs were verified against
  Cengel's published one-term table at Bi = 0.5, 1, 5 and 100 to better than 5·10⁻⁴.
  The slab remains the default and is unchanged: at equal size it is the slowest to heat, so it is
  the conservative choice.

- **The fryer now says when the fat itself is the problem.** If a fat cannot safely reach the
  temperature a food needs, the app used to lower the recommendation silently and still print
  *"conform to HACCP guidelines"*; the red warning tested a condition the clamp made arithmetically
  impossible. It now names the cause: *this fat cannot safely reach the temperature this food
  needs — choose a fat with a higher smoke point.* (Re-checked afterwards: still 0 hits across all
  49,284 oil × food combinations, so nothing changes for users today. The branch is now silent
  because the data is good, not because no data could ever reach it.)

- **Legal citations down to the article.** Switzerland: ODOV/VLpH **art. 6 para. 4** (RS
  817.022.17) — previously just "Swiss federal food legislation (FDHA)", which named the department
  and not the act. Chile: RSA **art. 266 let. c** (D.S. 977/96). Spain: **art. 6.3**
  (BOE-A-1989-2265). Each was read in the primary text, not taken from a review paper.

### Removed *(2026-07-16)*

- **The phantom Prime Cost % in Ingredient mode.** With no labour input the figure counted food
  only, so labelling it *"Prime Cost % (USAR · target < 65%)"* stated something it wasn't. The row
  is now omitted rather than shown under a warning: a warning under a wrong number still leaves the
  wrong number on screen, and the number is what a chef reads.

### Tested *(2026-07-16)* — the two places that had no net

- **The thermal engine had no tests at all.** It is the part of the app that tells a cook when
  chicken is safe, and it was the only area without a suite. `thermal_geometry` now pins the
  eigenvalues, the A₁ coefficients and the Bessel functions with **102 assertions against
  published tables** — Cengel's one-term approximation (12 λ₁/A₁ pairs at Bi = 0.5/1/5/100 across
  all three geometries) and Abramowitz & Stegun 9.4.1/9.4.4 for J₀/J₁. The references are
  deliberately external: *a suite that pins yesterday's numbers only proves the code still does
  what it did.*
  It also pins the app's worst historical bug as what it actually was — `A1 = 4/π` and `λ1 = π/2`
  are asserted to be the **Bi → ∞ limit** of the correct solution (a surface reaching oven
  temperature instantly), not constants wrong in themselves — and asserts that a real fan oven
  (Bi ≈ 0.69) sits nowhere near that limit, which is why the fix mattered.
  Plus invariants no table covers: λ₁ monotonic in Bi and never reaching its ceiling, A₁ positive
  and bounded, and the ordering **slab > cylinder > sphere** at equal Bi — which is *why* the slab
  default is the conservative one. If that ever inverts, the default has stopped being safe.

- **`confidence` — the field recording whether anyone actually checked a claim — had no
  assertions either**, and had rotted exactly where you would expect (see the USA entry above).
  35 new assertions (138 → 173): every entry carries a known confidence; an `official` statute
  must cite an article, so a basis naming only the act cannot pass as a citation; no absence and
  no disputed entry may claim `official`.

- **Both suites were mutation-checked**, because a green suite can be a vacuous one. Re-introducing
  the USA bug produces `FAIL no absence claims 'official': US`. A 0.25% drift in the sphere's A₁
  produces 5 failures — including the `A1 ≤ 2` invariant, which the table check alone would have
  missed. One wrong Bessel coefficient produces 11, cascading into the cylinder's eigenvalue
  because it solves J₁/J₀.

Suites now: `cost_engine 31 · smoke 64 · i18n 1852 · db_i18n 920 · tpc_limits 173 · vault 15 ·
thermal_geometry 102`. Still untested: the thermal engine's DOM-coupled half (Choi–Okos
properties, results panel).

### Known — found today, not yet fixed *(2026-07-16)*

- **The costing model is European; the legal registry is not.** The app now cites the frying-oil
  law of twelve countries to the article — and prints **`€` for all of them**. Nine of the twelve
  do not use the euro, and the Spanish and English interfaces serve *only* non-euro countries
  besides Spain. Nothing is miscalculated (the arithmetic is currency-agnostic; only the label is
  wrong), but it is recorded here rather than left to be found. Same for the 10% VAT default
  (Italian, and coincidentally Spanish) and for "menu price incl. VAT", which has no meaning in the
  United States. See [SOURCES.md](SOURCES.md) §4.

  **The design was decided before writing any code, and the rejected option is worth recording:**
  using the **currency symbol** as the selector that drives the applicable law was proposed and
  turned down, because *currency does not identify jurisdiction*. The euro alone spans Italy (25%),
  Germany (24% — and a DGF recommendation, not an established statute) and Ireland (no statutory
  limit at all); the `$` sign spans seven countries in this registry whose rules range from 25% to
  none to disputed. It would have inferred the specific from the generic — and it would have worked
  perfectly in Italy, which is where it would have been tested.
  The country will be the selector and the currency will follow from it. Amounts will never be
  converted, only relabelled: conversion needs exchange rates → the network → the end of the
  zero-permission guarantee.

### Documented *(2026-07-16)*

- **[SOURCES.md](SOURCES.md)** — every figure the app prints, mapped to its published origin, plus
  an honest list of what is *estimated* rather than sourced.
- **The number is never the whole law.** Four primary texts were read, and not one sets the
  polar-compound figure on its own — each embeds it in a package of further requirements: Spain
  (6.1/6.2/6.3, **sensory**), Argentina (smell/taste listed *first*, joined by "y/o", condemning
  the oil on its own), Chile (acidity, smoke point, polar — **chemical**), Switzerland (acidity,
  trans fatty acids, polar — **chemical**). KitchenMetrics models the number only — which is the
  one leg of the stool a cook cannot judge by eye, and precisely why an instrument earns its
  place. It does not replace looking, smelling and tasting the oil.
  *(Corrected 2026-07-18: as first published this entry said every text pairs the figure with a
  **sensory** test, and listed Germany — not a legal text — in place of Switzerland. See
  [SOURCES.md](SOURCES.md).)*

---

## [1.0] — 2026-07-15 — first release, closed testing

`com.kitchenmetrics.app` · versionCode 3 · targetSdk 36 · minSdk 26 · ~2.4 MB AAB

The first public build, submitted for review on 15 July 2026. Two and a half months earlier this
was a Python exercise written while learning to program.

### Five tools

- **Recipes & menu costing** — ingredient library with supplier prices and price history,
  multi-line dishes, sub-recipes, and menu-wide repricing against the price *actually printed on
  your menu*.
- **Food cost calculator** — USAR-aligned: two yields applied in sequence (butchering AP→EP, then
  cooking loss EP→plated), energy, labour, utilities, overhead and VAT; ratios measured on net
  revenue.
- **HACCP fryer analyzer** — oil degradation, absorption and remaining life against the statutory
  discard limit of the country you cook in, each figure naming its legal basis.
- **Thermal cooking calculator** — ramp and hold times from transient conduction with a finite Biot
  number, Choi–Okos thermophysical properties over 1,073 food profiles, Bigelow pathogen kinetics.
- **Lipid science database** — 148 fats and oils: smoke point, safe temperature, crystallisation,
  density, fatty-acid profile, oxidation risk, allergens.

### Privacy is a property of the build, not a promise

The app requests **zero Android permissions — not even `INTERNET`**, so the operating system itself
prevents it from sending anything off the device. No account, no ads, no analytics. An earlier USDA
nutrition lookup was removed after concluding it could neither scale (a per-key rate limit shared by
every install) nor keep its API key secret client-side.

### English · Italiano · Español

Runtime translation including the 148-fat database. Food names are deliberately **not**
machine-translated: the allergen field hangs off the name, so a mistranslation would be a safety
bug, not a typo. Translated fat labels keep the English name alongside — *"Strutto (Lard)"* — for
the same reason.

### Known limitations, stated rather than buried

- The energy **load factor defaults to 0.45 — an estimate, not a measurement.** It is the only
  number in the app with no published source, and it feeds prices a business acts on.
- The fryer's **cumulative TPC prediction model is switched off**: it contradicts itself and errs
  in the unsafe direction. The per-country legal *limits* it would compare against are correct and
  shipped; the model itself needs replacing, not patching.
- KitchenMetrics is a professional **estimation** tool. Verify critical food-safety parameters
  against your own HACCP plan, your probe thermometer, your oil tester and local regulations.
