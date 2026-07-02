# ⚠️ PNSB KPI — Watch-Outs

## 🚀 DEPLOY-TO-LIVE gate — seeder password (decided 2026-07-01)

The super-admin/demo seeder password is the literal string `password` (audit finding **M2**). Amirul's call: **ALLOWED for dev + staging** — it's a testing convenience, easy to remember. **NOT a bug to fix now.**

**Lucy's job:** this is a **deploy reminder, not a code change.** When Amirul talks about **deploying to production / going live**, LOUDLY flag: rotate/env-gate the seeder password (and strip/guard the other TEMP dev helpers — Quick Login, Fill-targets, Fill-results, demo seeders) BEFORE the live release. Do NOT touch it before then.

## Phase-gate rules (LOCKED 2026-06-29 — don't regress)

The phase boundaries are now airtight; two rules are load-bearing and easy to break by "tidying":

1. **Per-card KPI work gates allow `active` AND `appraisal`** — block only `{draft, moderation, closed}` (`Scorecards\Show::canEdit()`, `Appraisals\Show::canVerify()`, and the existing `canSelfAssessCompetency()`). **Never narrow these to `=== 'active'`** — cards legitimately catch up during the appraisal phase, and a strict active-only check re-creates the "stranded scorecard" bug.
2. **Cycle advance: appraisal = warn-but-allow, moderation = HARD block** (`Cycles\Show::scorecardsReadyForPhase()`). Moderation is the point of no return (all per-card gates close), so it must stay a hard block until every card is `moderation`+. Don't "simplify" both transitions to the same behaviour.

Full detail: `decision-log.md` → "Phase-Gate Hardening + Bell Curve Summary".

## Library completeness gap (raised 2026-06-26, address LATER)

A scorecard only totals 100% if the employee has ≥1 KPI in EVERY one of the 4 BSC perspectives (pre-fill normalises each perspective's assigned KPIs to its besar — see `decision-log.md` "PRE-FILL WEIGHT NORMALISATION FIX"). In the SEEDED demo this is guaranteed because every perspective has an active **company-level** KPI (company cascades to everyone). But in the REAL system, if an admin authors the KPI Library WITHOUT a company KPI in some perspective (and an employee has no dept/individual KPI there either), that employee can't reach 100%.

**Current safety:** the scorecard submit gate already BLOCKS submission when total ≠ 100% (`Scorecards/Show::submitBlockers()`), so it can't be submitted broken — but it's a dead-end with no guidance.

**Why:** Amirul flagged this when fixing the 38.89% pre-fill bug; agreed to handle the warning later, not now.

**How to apply (when we build it):** Add a **library-completeness warning** — e.g. on `Kpis/Index` or the cycle pre-flight panel, flag any BSC perspective that has NO active company-level KPI (the one level that guarantees universal coverage). Optionally also warn per-department/designation. Surface it as a soft warning (not a hard block), consistent with the Option-B "warn don't bulldoze" pattern.

## Multi-period cycle "hanging" flow — KIV / known gaps (raised 2026-06-29, leave as-is for now)

Amirul flagged it: in a **2- or 4-period cycle**, a staff keys in Period 1, hits done, then must come back later to fill the next period(s). It *feels* like the scorecard hangs in limbo between periods.

**Verdict after tracing the code: NOT a bug — intentional, but silent.** Scorecard has ONE status for the whole cycle (`draft → kpi_submitted → kpi_approved → appraisal → moderation → completed`); period scores have no status of their own. `Scorecards/Results::finalize()` is gated by `allPeriodsScored()` — the "Finalise Results" button stays disabled until EVERY KPI in EVERY period is scored, only then does the card move `kpi_approved → appraisal`. So between periods the card legitimately **waits at `kpi_approved`** with partial data. Correct, but nothing tells the staff to return.

**Decision:** leave the behaviour exactly as-is this session. Record the rough edges as KIV.

**KIV-A — No "come back" nudge for staff** *(the core of the worry)*. Staff get zero proactive signal to return when the next period opens — `Dashboard/Index` staff branch renders a static `_own-scorecard.blade.php` card (no "Period 2 open now", no "Period 3 opens 1 Jul", no notification). *Future fix:* staff dashboard banner of open/upcoming periods + button; optional scorecard-list badge; email/notification is a bigger lift (needs scheduler + mail).

**KIV-B — Future periods aren't locked until `opens_at`.** `Results::periodLocked()` only checks `status==='closed'` and `closes_at` in the past — it ignores `opens_at`, so a staff can fill a future period early; the "one period at a time" cadence isn't enforced. *Future fix:* also lock when `opens_at` is null or in the future. *Check first:* whether HR actually sets `opens_at`/`closes_at` (`Cycles/EditPeriods` allows blank dates) — if blank, both gating and date-based nudges are no-ops.

**KIV-C — Period `status` enum is decorative.** `review_periods.status` (`upcoming/open/scored/closed`) is set to `'upcoming'` once at creation (`EditPeriods` ~108-121) and never updated — no scheduler/observer/job flips it. Real gating is purely the passive `closes_at` date check. *Future fix:* a scheduled command to advance period status by date, OR an HR manual "open/close period" control — only worth it if we lean on `status` for gating/nudges.
