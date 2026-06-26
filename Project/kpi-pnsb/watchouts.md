# ⚠️ PNSB KPI — Watch-Outs

## Library completeness gap (raised 2026-06-26, address LATER)

A scorecard only totals 100% if the employee has ≥1 KPI in EVERY one of the 4 BSC perspectives (pre-fill normalises each perspective's assigned KPIs to its besar — see `decision-log.md` "PRE-FILL WEIGHT NORMALISATION FIX"). In the SEEDED demo this is guaranteed because every perspective has an active **company-level** KPI (company cascades to everyone). But in the REAL system, if an admin authors the KPI Library WITHOUT a company KPI in some perspective (and an employee has no dept/individual KPI there either), that employee can't reach 100%.

**Current safety:** the scorecard submit gate already BLOCKS submission when total ≠ 100% (`Scorecards/Show::submitBlockers()`), so it can't be submitted broken — but it's a dead-end with no guidance.

**Why:** Amirul flagged this when fixing the 38.89% pre-fill bug; agreed to handle the warning later, not now.

**How to apply (when we build it):** Add a **library-completeness warning** — e.g. on `Kpis/Index` or the cycle pre-flight panel, flag any BSC perspective that has NO active company-level KPI (the one level that guarantees universal coverage). Optionally also warn per-department/designation. Surface it as a soft warning (not a hard block), consistent with the Option-B "warn don't bulldoze" pattern.
