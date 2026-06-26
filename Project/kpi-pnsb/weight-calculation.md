# ⚖️ PNSB KPI — Weight Calculation Reference

Authoritative formula for KPI weight auto-rescale in the PNSB KPI Library. Reworked 2026-06-26 to the PER-CATEGORY model. See `decision-log.md` for the module decision log.

## The two weights
- **Big weight / Pemberat Besar (`bsc_perspectives.weight`)** = the perspective's share of the scorecard. Fixed 4 perspectives, ALP-locked, super-admin only. Must total **100%** across the 4.
- **Small weight / Pemberat Kecil (`kpis.weight`)** = each KPI's share. Bounded by its perspective's besar.

## Per-category rule (the core idea)
Each KPI has a `category_scope`: `null` (All) / `ceo` / `executive` (NO support — support is 100% competency, 0% KPI).
A given category's SCORECARD only receives a SUBSET of the library: KPIs scoped `null` **plus** that category's own.
So weight balance is checked **PER CATEGORY**, never as one global sum:
```
categoryWeightTotal(cat) = Σ kpis.weight WHERE category_scope IN (null, cat)
balanced(cat)            = categoryWeightTotal(cat) == besar   (tolerance < 0.01)
```
Each category's eligible set must independently equal the besar. A "shared" (null) KPI counts toward BOTH ceo and executive totals. The raw sum of ALL a perspective's KPI weights is NOT meaningful (it double-counts shared KPIs — e.g. rawSum = 1.5×besar when shared+ceo+exec all present).

## AUTO-RESIZE on big-weight change (the calculation Amirul asked to note)
When super-admin edits a perspective's besar from `oldBesar` → `newBesar` (Kpis/Index BSC Weights editor), EVERY KPI in that perspective is rescaled by the SAME ratio:
```
factor      = newBesar / oldBesar
new kpiWeight = round(oldKpiWeight × factor, 2)   // floored at 0
```
**Why same-ratio works:** if a category's set summed to `oldBesar`, multiplying every member by `newBesar/oldBesar` makes it sum to `newBesar`. So BOTH the ceo set and the executive set stay balanced automatically — no per-category special-casing needed on rescale.
- Edge: `oldBesar <= 0` → skip (no ratio); leave weights at 0 for manual entry.
- **Worked example:** Financial besar 40→35, two KPIs at 20/20. factor = 35/40 = 0.875 → each becomes 20×0.875 = 17.50. New set sums to 35. ✓
- **WRONG approach (the bug that was fixed):** `factor = newBesar / globalSum`. Once scopes mix, globalSum ≠ besar (it double-counts shared KPIs), so this shrank everything incorrectly and broke per-category balance. Do NOT reintroduce a global-sum rescale.

## Inline small-weight editing (Kpis/Show)
- `saveWeights()` blocks if ANY category's `categoryWeightTotal` exceeds besar (not the global sum). Under-besar is allowed but flagged red.
- UI shows one chip PER category: e.g. `CEO: 40%/40% ✓ · Executive: 25%/40%`.

## Tier/score formula (separate, unchanged — from PNSB Excel)
Not weight-resize but related scoring: `Skor = (Pencapaian Sebenar / 100) × Pemberat Kecil`, where Pencapaian Sebenar tier index = Below 0 / Ambangan 60 / Setuju 80 / Lebihan 100, hard-capped at 100.
