# KitchenMetrics — Sources / Fonti

Every number KitchenMetrics prints is traceable to a published source: a national
regulation, an international standard, or the peer-reviewed literature. This document maps
each figure to its origin so that **anyone — a chef, an IT reviewer, a food-safety inspector —
can verify it independently**, without taking the app's word for anything.

> **This is not a certification.** No public body certifies calculation apps on request.
> What this document offers is stronger than a badge: full traceability. The app cites the
> source; you check the source. Where a figure is an estimate rather than a regulation, it is
> labelled as such — see *Honest limitations* at the end.

*Ogni valore stampato da KitchenMetrics è riconducibile a una fonte pubblicata: una norma
nazionale, uno standard internazionale o la letteratura scientifica. Questo documento collega
ogni dato alla sua origine, così che chiunque — cuoco, revisore IT o ispettore — possa
verificarlo in autonomia.*

Last reviewed: **16 July 2026**. On that date four primary legal texts were read directly
(Switzerland, Spain, Argentina, Chile) and **eight clarification requests** were sent to the bodies
that own the remaining figures:

| Sent (CEST) | Body | Channel |
|---|---|---|
| 11:36 | 🇮🇹 Ministry of Health — **DGISAN** | email · vigency of Circular 1/1991 |
| 12:37 | 🇩🇪 **DGF** — Deutsche Gesellschaft für Fettwissenschaft | email · **replied the same day**: forwarded to their experts |
| 14:07 | 🇲🇽 **COFEPRIS** | email |
| 14:37 | 🇮🇪 **FSAI** | email · Advice Line (staffed by food scientists) |
| 14:38 | 🇨🇦 **CFIA** | email |
| 14:48 | 🇬🇧 **FSA** | **FOI request** (Freedom of Information Act 2000) · **acknowledged in 12 minutes** by the Information Governance Manager · statutory reply due within 20 working days |
| — | 🇺🇸 **FDA** | web form 3907 (FCIC / FSMA Technical Assistance Network), category *Retail Food* |
| — | 🇦🇺🇳🇿 **FSANZ** | web enquiry form |

*(The two web-form submissions carry no timestamped copy — unlike an email, they leave the sender
no receipt. Both were submitted the same afternoon.)*

Every request asks the same thing and refuses to ask anything else: **whether the norm exists and
what it says**. None asks for an assessment, endorsement or approval of the application — no public
body certifies calculation software, and asking would only earn a refusal worth quoting against us.

Each entry below carries the date it was checked. **A pending request is not an answer**, and this
table will not pretend otherwise until the replies arrive.

---

## 1. Deep-fryer oil — statutory TPC discard limits

Total Polar Compounds (TPC) limits are **national, and they disagree**. There is **no** EU,
EFSA, FDA or Codex Alimentarius limit — a claim the app deliberately corrects, because citing
a non-existent threshold in a HACCP tool is something an inspector can falsify on the spot.

The app always applies the **stricter** of the fat's own chemistry limit and the local legal
ceiling; a jurisdiction can only ever tighten the threshold, never relax it.

Where the "read" column is ticked, the primary legal text was consulted directly on the date shown,
not taken from a review paper. A citation without a consultation date is worth little: laws change.

| Country | Limit | Status | Legal basis (as cited in-app) | Primary text read |
|---|---|---|---|---|
| Italy | 25% | statutory | Ministry of Health Circular no. 1 of 11/01/1991 | ⏳ vigency query sent to the Ministry (DGISAN) **2026-07-16, 11:36 CEST** |
| Switzerland | 27% | statutory | **ODOV/VLpH art. 6 para. 4** — DFI Ordinance of 16.12.2016 (RS 817.022.17) | ✅ **2026-07-16** (Fedlex; version in force 01.01.2026) |
| Spain | 25% | statutory | **Orden de 26 de enero de 1989, art. 6.3** (BOE-A-1989-2265) | ✅ **2026-07-16** (boe.es) |
| Argentina | 25% | statutory | **Código Alimentario Argentino art. 552 bis** (Res. Conjunta 17/2021); method ISO 8420:2002 | ✅ **2026-07-16** (boletinoficial.gob.ar) |
| Chile | 25% | statutory | **Reglamento Sanitario de los Alimentos (D.S. 977/96) art. 266 let. c** | ✅ **2026-07-16** (consolidation to Aug 2017 — see caveat below) |
| Mexico | — | no maximum | NMX-F-068 defines *how to measure*, sets no maximum (verify with COFEPRIS) | ⏳ query sent to COFEPRIS **2026-07-16, 14:07 CEST** |
| United Kingdom | 24%* | no statutory limit | FSA — general food-safety law; *industry practice | ⏳ **FOI request** (Freedom of Information Act 2000) sent to the FSA **2026-07-16, 14:48 CEST** — acknowledged the same day by the FSA's Information Governance Manager; statutory reply due within 20 working days |
| Ireland | 25%* | no statutory limit | FSAI; *industry benchmark | ⏳ query sent to the FSAI Advice Line **2026-07-16, 14:37 CEST** |
| United States | 25%* | no statutory limit | No federal TPC limit **found** (FDA); *voluntary benchmark, some states differ | ⏳ query sent to the FDA (FCIC/FSMA TAN, form 3907, category *Retail Food*) **2026-07-16** |
| Canada | 25%* | no statutory limit | CFIA; *industry benchmark | ⏳ query sent to the CFIA **2026-07-16, 14:38 CEST** |
| Australia | 27% | disputed | Reviews cite 27%; industry says TPM is not a legal limit — confirm with state regulator | ⏳ query sent to FSANZ **2026-07-16** |
| New Zealand | 25%* | no statutory limit | FSANZ Food Standards Code; *industry benchmark | ⏳ covered by the FSANZ query **2026-07-16** |

**No TPC limit found for:** the European Union / EFSA, the US FDA, and Codex Alimentarius.

*The wording matters. "No limit found" is what the evidence supports; "sets no limit" asserts a
fact about a body's entire corpus, which nobody here has exhaustively searched. The app used to
mark the US claim as verified against the primary legal text — for a claim that no such text
exists. An absence cannot be read; it can only be attested by the body itself, and none has been
asked. Where a negative is only ever "widely repeated and never contradicted", this document says
so.*

### Germany's 24% — a recommendation, not (as far as established) a statute

The 24% figure widely quoted for Germany — including the "strictest anywhere" figure the app
displays — originates from the **DGF (Deutsche Gesellschaft für Fettwissenschaft)**, established
at the *3rd International Symposium on Frying Fats* (2000). German food-control authorities use
it as an **assessment criterion**, alongside sensory findings, polymeric triglycerides (max. 12%)
and acid value — not as a single statutory maximum. Germany is deliberately **absent from the
registry above**, because no primary legal text has been established for it.

**A clarification has been requested directly from the DGF** (16 July 2026); their Managing
Director confirmed receipt and forwarded the question to their experts. This document will be
updated with their answer. Until then the app calls 24% a *reference figure*, never a *limit*.

### ⚠️ The number is never the whole law

Four primary texts were read on 16 July 2026, and every one of them pairs the polar-compound
figure with a **sensory test** — several add more criteria still:

| Jurisdiction | What the law actually requires |
|---|---|
| **Spain** (art. 6) | 6.1 free of substances foreign to frying · 6.2 organoleptic characters must not taint the food · **6.3 polar components below 25%** |
| **Argentina** (art. 552 bis) | *(a)* alterations in **smell or taste** — listed **first**, and joined by "y/o", so it condemns the oil on its own — *and/or* *(b)* **TPC ≥ 25%** |
| **Chile** (art. 266) | *(a)* free acidity > 2.5% · *(b)* smoke point < 170 °C · *(c)* **polar compounds > 25%** |
| **Germany** (DGF) | sensory findings *"im Vordergrund"*, plus polymeric triglycerides (12%), **polar compounds (24%)** and acid value (2%) |

Switzerland's own enforcement laboratory says the same thing from the other direction: the Ticino
report lists the signs that condemn an oil **with no instrument at all** — rancid or scratchy
taste, incipient smoking, foaming, viscosity, a pungent smell over the hot fryer. Four laws and
one regulator, all agreeing that the meter is not the whole story.

**KitchenMetrics models the polar-compound figure only.** That is stated here rather than
glossed over — and it is not, on reflection, a weakness: the TPC number is precisely the leg of
the stool a cook *cannot* judge by eye, which is why an instrument is worth having. But the app
does not replace looking at, smelling and tasting the oil, and in Spain, Argentina and Chile the
number alone does not decide compliance.

**Threshold wording:** Spain requires the content to be *"inferior al 25 por 100"* and Argentina
condemns oil at *"igual o superior al 25 %"* — so **25.0% is already non-compliant**, not the last
acceptable value. The app's arithmetic always treated the threshold as inclusive; its wording was
corrected on 2026-07-16 from "discard when TPC *exceeds* X%" to "**discard at or above X%**".

### Chile — primary text read, and it says more than we model

Art. 266 of the RSA sets **three** limits, any one of which condemns the oil:
*"a) acidez libre expresada como ácido oléico superior al 2,5%; b) punto de humo inferior a
170 ºC; c) 25% de compuestos polares como máximo."* Art. 265 caps linolenic acid at 2% for
industrial/institutional frying; art. 267 bans reusing discarded frying oil in other human food.
**KitchenMetrics currently models only the 25%** — an honest gap, not a claim.
Note the wording *"aceites y **mantecas**"*: Chile covers animal fats explicitly.
*Version read: consolidation to August 2017; post-2017 amendments not checked, and article
numbering shifts between consolidations — verify on [BCN](https://www.bcn.cl/leychile/navegar?idNorma=71271).*

### The Swiss limit, independently corroborated — and what enforcement actually finds

The **Laboratorio cantonale del Ticino** publishes its frying-oil checks. Its February 2026 report
on the 2025 campaign (*"Qualità dell'olio per frittura"*, Bellinzona) cites the same provision this
document does — *"Ai sensi dell'Ordinanza del DFI sulle derrate di origine vegetale, i funghi e il
sale commestibile (ODOV) del 16 dicembre 2016 tale valore, nei grassi e negli oli commestibili per
friggere, non deve superare il 27 per cento (270 g/kg)"* — which is an official enforcement body
reading the ordinance exactly as we do.

Its findings are worth stating, because they show the limit is not theoretical:

- **96 samples from 77 businesses; 12 non-compliant (12%)** — restaurants, hotels, takeaways,
  food trucks and other food businesses.
- **In 6 of those 12 cases criminal proceedings were opened** against the person responsible,
  *"a causa del massiccio superamento del valore massimo ammesso di parti polari"*.
- A further **5 samples could not be judged either way** because of measurement uncertainty —
  a reminder that the number itself carries an error bar.

The same report also corroborates two things stated elsewhere in this document: that a frying
oil's smoke point **falls as the oil is used** (*"il quale si abbassa man mano che l'olio viene
utilizzato"* — so a database of *fresh* smoke points cannot stand in for measuring the used oil),
and that degradation is recognisable **without any instrument** — rancid or scratchy taste,
incipient smoking, increased foaming, viscous fat, pungent smell over the hot fryer.

*Source: [Laboratorio cantonale, Ticino — Olio di frittura, campagna 2025](https://m4.ti.ch/fileadmin/DSS/DSP/LC/lcinforma/Rapportini/2026/Olio_frittura_2025.pdf) (PDF, read 2026-07-16).*

### Switzerland — primary text read

Art. 6 para. 4 of the ODOV/VLpH (RS 817.022.17) reads: *"Der polare Anteil in Speisefetten und
Speiseölen zum Frittieren darf 27 Prozent nicht übersteigen."* Version in force: 1 January 2026.
**Open question:** the ordinance's scope (art. 2, art. 1 let. b) covers foodstuffs of **plant**
origin, and art. 6 sits in the chapter on plant oils and fats — so whether the 27% also binds
**animal** frying fats (lard, tallow) is not settled by this text. The app does not assume it does.

*Primary source for the registry:* Song et al., "Feasibility of total polar compound ... aspect
of regulations of various countries," *Food Chemistry* (2022), cross-checked against primary
legal texts where marked `official` in the app (`tpc_limits.js`).

**How to verify:** each `basis` string above is a searchable legal reference. `BOE-A-1989-2265`
resolves on boe.es; the Italian Circular is citable to the Ministero della Salute; the Argentine
article is in the Código Alimentario Argentino online.

---

## 2. Fat & oil database (148 entries)

Smoke point, safe frying temperature, crystallisation, density, fatty-acid composition,
oxidation risk and allergens for 148 fats and oils. **Each record carries its own `src` field** —
the database is a single audited source of truth, and a second registry that consumes it is
test-pinned to agree with it.

Sources cited across the records include:
- **USDA FoodData Central** and USDA Agricultural Research Service (composition, density)
- **International Olive Council (IOC)** (olive oils)
- **AOCS — American Oil Chemists' Society** Lipid Library (refined seed/nut oils)
- **Malaysian Palm Oil Board (MPOB)**, **FAO**, and peer-reviewed journals (*Journal of the
  American Oil Chemists' Society*, *Food Chemistry*, *Journal of Food Composition and Analysis*,
  *European Journal of Lipid Science and Technology*, and others)
- **McGee, *On Food and Cooking*** (2004) for several traditional animal fats

**Allergen policy:** allergen labels are never machine-translated, because the allergen hangs
off the food's name; a mistranslation would be a safety bug, not a typo. Translated fat names
keep the English original alongside (e.g. "Strutto (Lard)") for the same reason.

---

## 3. Cooking-time / pasteurisation model (thermal)

Cook and pasteurisation times are computed from **transient heat-conduction physics**, not
lookup tables:

- **Choi–Okos thermophysical model** — composition-based density, specific heat, conductivity
  and thermal diffusivity across 1,073 food profiles.
- **Finite Biot number** (eigenvalue found by bisection of `λ·tan(λ) = Bi`) — *not* the naïve
  Bi→∞ shortcut, which under-predicts cook time up to 3.4× in the **unsafe** direction. A sanity
  gate refuses to output if any derived property goes non-physical, and flags results where the
  one-term series itself stops being valid (Fo < 0.2).
- **Bigelow pathogen kinetics** — log-reduction / D- and z-value model for the lethality target.

**Regulatory / standards references cited in-app:**
- USDA FSIS **9 CFR 318 / 381** (pathogen lethality performance standards)
- **EFSA BIOHAZ Panel** (target log-reduction guidance)
- **FDA Food Code 2022**
- **EU 853/2004**
- USDA FoodData Central (source composition data)

---

## 4. Food costing model

Aligned with the **Uniform System of Accounts for Restaurants (USAR)**, published by the
National Restaurant Association. (There is no ISO/IEC standard for food costing.)

- **Two yields in sequence** — butchering yield (AP→EP) then cooking loss (EP→plated).
  Conflating them undercosts every cooked item.
- **Ratios measured on net (ex-VAT) revenue** — VAT is collected for the state, not earned.
- **Prime Cost %** is the USAR headline figure (target < 65%).

The engine is a pure function with a public regression test suite (`cost_engine.test.html`).

### ⚠️ The costing model is European. The legal registry is not. *(found 2026-07-16)*

This document holds the app's figures to the primary legal text of twelve countries. The costing
engine, meanwhile, **prints `€` for all of them** — the symbol is welded into the formatter, and
there is no notion of currency or country anywhere in the costing path.

**Nine of those twelve countries do not use the euro** (CH, AR, CL, MX, GB, US, CA, AU, NZ). The
Spanish interface serves Argentina, Chile and Mexico — none of them euro. The English interface
serves the UK, US, Canada, Australia and New Zealand — none of them euro either. A cook in Zurich
is given ODOV art. 6 para. 4 cited to the paragraph, and then a plate cost in euros.

**Nothing is miscalculated:** the arithmetic is currency-agnostic, so only the label is wrong. But
it is stated here rather than left to be discovered.

Two further European assumptions, for the same reason:

- **The VAT default of 10%** is Italian food service (and, by coincidence, Spanish). It is wrong
  for Switzerland (~8.1%), Argentina (21%), Chile (19%), the UK (20%) — though it is an editable
  default, not a claim.
- **"Menu price incl. VAT" has no meaning in the United States**, which has no VAT: sales tax is
  added at the till, varies by state and county, and never appears on the menu. The USAR
  `Prime Cost % < 65%` benchmark is likewise American, applied everywhere.

**The fix, and the design decision behind it:** the country becomes the single selector and the
currency follows from it — *not* the other way round. Using the currency to infer the applicable
law was considered and **rejected**: the euro alone spans Italy (25%), Germany (24%, and a DGF
recommendation rather than an established statute) and Ireland (no statutory limit at all), and
the `$` sign spans seven countries in this very registry whose rules range from 25% to none to
disputed. **Currency does not identify jurisdiction**, and inferring it would err in the unsafe
direction more often than not.
*Amounts will never be converted, only relabelled: conversion needs exchange rates, which needs
the network, which would end the zero-permission guarantee.*

---

## 5. Honest limitations (what is *not* sourced)

Transparency cuts both ways. These figures are estimates, and the app says so:

- **Energy load factor default = 0.45** — the one number in the app with no published source.
  It is an appliance duty-cycle estimate; measure yours with a plug meter for accuracy.
- **Industry benchmarks marked with `*`** in the TPC table are *practice*, not law. The app
  shows them as benchmarks and never presents them as statutory.
- **Cooking times are physics-based estimates.** Core temperature must always be verified with
  a probe thermometer. KitchenMetrics supports HACCP decisions; it does not replace the probe,
  the TPC tester, your kitchen's HACCP plan, or professional judgment.

---

## Independent review — invited

If you work in food science, food-safety regulation, or thermal engineering and want to
scrutinise any figure here, that scrutiny is welcome — it is exactly how the app's worst bug
(the thermal model above) was caught. Open an issue on this repository or get in touch.
