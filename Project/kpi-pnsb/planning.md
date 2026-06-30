# 1🎯 PNSB BSC KPI - Planning Document

**PNSB** = Permodalan Negeri Selangor Berhad
**BSC** = Balanced Scorecard (Kad Skor Imbangan)
**KPI** = Key Performance Indicator (Petunjuk Prestasi Utama)
**ALP** = Ahli Lembaga Pengarah (Board of Directors)
**KPE** = Ketua Pegawai Eksekutif (Chief Executive Officer / CEO)
**HR** = Human Resources (Bahagian Sumber Manusia)
**KB** = Kakitangan Biasa (Regular/General Staff group — pre-moderation baseline)
**KIV** = Keep In View (pending — to be confirmed in a later phase)
**Full System Name**: PNSB BSC KPI (Balanced Scorecard & KPI Performance Management System)

---

## 1. Core Concept

A performance management system for PNSB and its subsidiaries that evaluates employees on two pillars — depending on their category:

- **KPI (Quantitative)**: *WHAT* was achieved — measured against BSC-aligned targets
- **Penilaian Keperibadian / Competency (Qualitative)**: *HOW* they achieved it — personality & behavioural assessment

The **KPI pillar** is structured using the 4 BSC perspectives with fixed company-level weightage:

| BSC Perspective                                   | Weightage |
| ------------------------------------------------- | --------- |
| 💰 Kewangan (Financial)                           | 40%       |
| 🤝 Pelanggan (Customer)                           | 30%       |
| ⚙️ Proses Dalaman (Internal Business Process)   | 20%       |
| 🧠 Pembelajaran & Peningkatan (Learning & Growth) | 10%       |

---

## 2. Employee Categories & Scoring Split

This is the most critical design decision — scoring formula differs **per employee category**:

| Kategori                                | KPI Weight | Competency (Penilaian Keperibadian) Weight |
| --------------------------------------- | ---------- | ------------------------------------------ |
| **Ketua Pegawai Eksekutif (CEO)** | 100%       | 0%                                         |
| **Eksekutif → Pengurusan Kanan** | 80%        | 20%                                        |
| **Sokongan / Teknikal**           | 0%         | 100%                                       |

> Key insight: Support/Technical staff are evaluated purely on personality/competency — no KPI scoring.
> CEO is purely KPI-driven.
> Middle layer (Executive to Senior Management) is the hybrid 80/20 split.
> ⚠️ **Important**: These scoring splits are formal **ketetapan ALP (ALP Board resolutions)** — not HR or system admin preferences. They can only be changed through a new ALP Board decision. In the system, only Super Admin can modify these values, and only when instructed by an ALP resolution.

---

## 3. KPI Target Structure (3-Tier)

Each KPI has **three target levels**, not just one:

| Tier            | Malay Term          | Meaning                              |
| --------------- | ------------------- | ------------------------------------ |
| Below Threshold | Ambangan (Kurangan) | Minimum acceptable — scored low     |
| Meet Target     | Setuju              | Hit the agreed target                |
| Stretched       | Lebihan             | Exceeded target — bonus performance |

**Example from KPI form (Financial - Revenue KPI, 20% weight):**

- Threshold: RM 100,000
- Meet Target: RM 150,000
- Stretched: RM 200,000

KPI scores are computed as: `KPI weight × achievement score` → summed across all KPIs within each BSC perspective.

---

## 4. Process Flow (Proses PMS)

10-step process for PNSB and subsidiaries:

1. **Penyediaan KPI Syarikat** — Prepare Company-level KPI
2. **Kelulusan KPI Syarikat oleh ALP** — Ahli Lembaga Pengarah (ALP / Board of Directors) approves Company KPI
3. **Penyediaan KPI Ketua Bahagian / Jabatan / Kakitangan** — Department Heads and staff prepare individual KPIs
4. **Kelulusan oleh KPE / Ketua Bahagian / Jabatan** — Ketua Pegawai Eksekutif (KPE / CEO) or Department Head approves individual KPIs
5. **KPI lengkap dikemukakan kepada HR** — Completed KPI forms submitted to Bahagian Sumber Manusia (HR)
6. **Perlaksanaan KPI (Januari - Disember)** — KPIs are live and tracked for the full year
7. **KPI lengkap dikemukakan kepada HR untuk penilaian** — Year-end: completed KPIs resubmitted to Bahagian Sumber Manusia (HR) for appraisal
8. **Sesi penilaian prestasi** — Appraisal session conducted by supervisors/managers, strictly following Ahli Lembaga Pengarah (ALP) formal guidelines and ketetapan (Board resolutions). The scoring splits per employee category (100% KPI / 80%+20% / 100% Competency) are ALP Board decisions — not HR configurations. Any change to these weights requires a new ALP resolution before the system can be updated.
9. **Sesi Moderasi** — Moderation session by Department Head and Ketua Pegawai Eksekutif (KPE / CEO)
10. **Kategori Pemarkahan** — Final grade assignment

---

## 5. Performance Rating Categories (Bell Curve)

5-tier grading system (in order, lowest to highest):

| # | Kategori         | Meaning           | Score Range |
| - | ---------------- | ----------------- | ----------- |
| 1 | Perlu Diperbaiki | Needs Improvement | < 50        |
| 2 | Memuaskan        | Satisfactory      | 50 – 59     |
| 3 | Baik             | Good              | 60 – 75     |
| 4 | Sangat Baik      | Very Good         | 76 – 89     |
| 5 | Cemerlang        | Excellent         | 90 – 100    |

### Bell Curve Data (actual PNSB data — 91 staff total):

| Kategori         | Sasaran (Target) | KB           | MOD1 (KB/HR) | MOD2 (KPE)   | % over target |
| ---------------- | ---------------- | ------------ | ------------ | ------------ | ------------- |
| Perlu Diperbaiki | 4                | 3            | 0            | 0            | 0%            |
| Memuaskan        | 7                | 7            | 17           | 13           | 186%          |
| Baik             | 61               | 12           | 54           | 52           | 85%           |
| Sangat Baik      | 16               | 35           | 19           | 24           | 150%          |
| Cemerlang        | 3                | 34           | 1            | 2            | 67%           |
| **Jumlah** | **91**     | **91** | **91** | **91** |               |

> **KB** = Kakitangan Biasa (Regular Staff) — raw scores before any moderation
> **MOD1 (KB/HR)** = Moderasi Pertama — after 1st moderation round by Bahagian Sumber Manusia (HR)
> **MOD2 (KPE)** = Moderasi Kedua — after 2nd moderation round by Ketua Pegawai Eksekutif (KPE / CEO)
> Bell curve moderation is **confirmed in use** — distribution shifts significantly between rounds

---

## 6. Key Modules & Features

### 👥 User & Organization Management

#### Organisational Hierarchy (Hierarki Organisasi)

```
Ahli Lembaga Pengarah - ALP (Board of Directors)
│  Sets KPI guidelines & scoring ketetapan. Approves Company KPI.
│  Not a system user — decisions are configured by Super Admin.
│
└── Ketua Pegawai Eksekutif - KPE (Chief Executive Officer)
    │  Approves all individual KPIs. Leads moderation. 100% KPI scoring.
    │  Reviewed by: ALP (outside system)
    │
    ├── Ketua Bahagian / Jabatan (Department Head)
    │   │  Approves dept staff KPIs. Participates in moderation.
    │   │  Scoring: 80% Petunjuk Prestasi Utama (KPI) + 20% Kompetensi (Competencies)
    │   │  Reviewed by: Ketua Pegawai Eksekutif (KPE)
    │   │
    │   ├── Eksekutif / Pengurusan Kanan (Executive / Senior Management)
    │   │       Submits own KPIs. Self-assesses competencies.
    │   │       Scoring: 80% Petunjuk Prestasi Utama (KPI) + 20% Kompetensi (Competencies)
    │   │       Reviewed by: Ketua Bahagian
    │   │
    │   └── Kakitangan Sokongan / Teknikal (Support / Technical Staff)
    │           No KPI form — Kompetensi (Competencies) only.
    │           Scoring: 100% Kompetensi (Competencies)
    │           Reviewed by: Ketua Bahagian
    │
    └── Bahagian Sumber Manusia - HR (Human Resources)
            Administers the appraisal cycle. Runs moderation.
            Not in the appraisal chain — system administrator role.
```

> ⚠️ Exact job titles per category still pending — see **D3** in Questions Tracker.

---

#### System Roles & Permissions (Peranan Sistem)

| System Role                                   | Maps To                 | Key Permissions                                                                                                   |
| --------------------------------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Super Pentadbir (Super Admin)**       | System owner / IT       | Full access, configure ALP ketetapan (scoring rules), manage all users                                            |
| **Pentadbir Sumber Manusia (HR Admin)** | Bahagian Sumber Manusia | Open/close appraisal cycles, run moderation, view all scorecards, lock final grades                               |
| **Ketua Pegawai Eksekutif (KPE)**       | CEO                     | Approve/reject all individual KPIs, lead final moderation round, view company-wide results                        |
| **Ketua Bahagian (Department Head)**    | Dept Head               | Approve/reject dept staff KPIs, first moderation round, view dept results — also has own scorecard as a reviewee |
| **Kakitangan (Staff)**                  | All other employees     | Submit own KPI form, submit self-assessment (Penilaian Kendiri), view own scorecard only                          |

---

#### Appraisal Review Chain (Rantaian Semakan)

| Employee                      | KPI Approved By                               | Competency Rated By       | Moderation By        |
| ----------------------------- | --------------------------------------------- | ------------------------- | -------------------- |
| Ketua Pegawai Eksekutif (KPE) | Ahli Lembaga Pengarah (ALP) — outside system | — (no competency)        | ALP (outside system) |
| Ketua Bahagian (Dept Head)    | Ketua Pegawai Eksekutif (KPE)                 | KPE rates them            | KPE + HR             |
| Eksekutif / Pengurusan Kanan  | Ketua Bahagian                                | Ketua Bahagian rates them | Ketua Bahagian + KPE |
| Sokongan / Teknikal           | — (no KPI form)                              | Ketua Bahagian rates them | Ketua Bahagian + KPE |

> **Key design note**: `manager_id` on the `users` table drives the review chain — the system checks who your manager is to determine who reviews your KPI and competency. A Ketua Bahagian has `manager_id = KPE`. An Eksekutif has `manager_id = Ketua Bahagian`.

### 📚 KPI Library (BSC-Aligned)

- Company-level KPIs cascaded down to individual level
- KPIs tagged to BSC perspective (Financial / Customer / Internal / Learning)
- Each KPI has 3-tier targets: Threshold / Meet Target / Stretched
- Unit of Measure (UOM) defined per KPI (RM, Date, %, Bil/Count, etc.)

### 🧠 Competency / Penilaian Keperibadian

- Applies to: Executives, Senior Management, Support/Technical staff
- Does NOT apply to: CEO (100% KPI only)
- Sole measure for Support/Technical staff (100% weight)
- Self-rating + Supervisor rating

### 🔄 The Appraisal Workflow

1. Bahagian Sumber Manusia (HR) opens appraisal cycle for the year
2. Company KPI prepared → submitted to Ahli Lembaga Pengarah (ALP / Board of Directors) for approval
3. Staff prepare individual KPIs → submit to Ketua Pegawai Eksekutif (KPE) / Department Head for approval
4. KPIs locked after approval
5. Year runs (Jan–Dec) — staff track and log achievements
6. Year-end: staff submit final KPI achievement data to Bahagian Sumber Manusia (HR)
7. Appraisal session conducted per employee category rules
8. Sesi Moderasi (Moderation session) by Department Head + Ketua Pegawai Eksekutif (KPE) — bell curve applied
9. Final grades assigned and communicated

### 📊 Dashboards & Analytics (Vue + ApexCharts)

- **Staff Dash**: KPI progress per BSC (Balanced Scorecard) perspective, competency radar chart
- **Ketua Bahagian (Department Head) Dash**: Team KPI completion, distribution preview
- **Bahagian Sumber Manusia (HR) / Admin Dash**: Company-wide bell curve, moderation controls, grade distribution

### 🔒 Audit Trail & Activity Log  *(built 2026-06-30)*

- General audit trail via **`owen-it/laravel-auditing`** — records create / update / delete on all 16 domain & config models (KPIs, BSC perspectives, competencies, departments, designations, cycles, templates, users, scorecards…), capturing old vs new values, the acting user, IP and URL.
- **Authentication auditing** — login, logout, and **failed login** (attempted email + IP) recorded into the same log.
- Secrets (password / remember_token) are never logged; the existing domain logs (`moderation_logs`, `score_overrides`, `scorecard_status_logs`) are left as-is (no double-auditing).
- **Viewer** at `/audit-trail` — **Super Admin only** (separation of duties): filterable table (model / event / actor, with a searchable user combobox) + per-record change-history timeline.

### 🧮 Calculation Preview Tool (Inspired by AIROD PMS+)

> HR and Super Admin can plug in scores and preview the computed result before saving — useful during moderation and appraisal sessions.

**PNSB Calculation Formula (100% scale — not AIROD's /5 scale):**

**Step 1 — KPI Score per KPI item:**
```
Pencapaian Sebenar = 0 / 60 / 80 / 100  (based on tier achieved)
Skor               = Pencapaian Sebenar / 100 × Jumlah Kecil Pemberat (KPI item weight %)
```

**Step 2 — Total KPI Score:**
```
KPI Score = SUM(Skor) across all KPI items in the scorecard
```

**Step 3 — Competency Score:**
```
Competency Score = AVG(manager_rating) across all competency items  (method pending B3)
```

**Step 4 — Final Score:**
```
Final Score = (KPI Score × kpi_weight) + (Competency Score × competency_weight)

Example (Eksekutif — 80% KPI / 20% Competency):
Final Score = (KPI Score × 0.80) + (Competency Score × 0.20)
```

**Step 5 — Grade auto-assigned from Final Score (C5):**
```
>= 90  → Cemerlang
>= 76  → Sangat Baik
>= 60  → Baik
>= 50  → Memuaskan
< 50   → Perlu Diperbaiki
```

> The preview tool should show a live simulation panel where HR can adjust individual KPI tier selections and competency scores to see the final score and grade update in real time — before committing the manager override.

---

## 7. Database Schema

> Full DB design in `db-design.md`. Tables summary:

| Table | Purpose |
|---|---|
| `departments` | Org structure with parent hierarchy |
| `users` | Staff, roles, category, manager_id chain |
| `appraisal_cycles` | Annual cycle with status progression |
| `bsc_perspectives` | 4 fixed BSC perspectives with weights (ALP ketetapan) |
| `kpis` | KPI library — company / dept / individual level |
| `scorecard_templates` | Scoring formula per employee category (ALP ketetapan) |
| `scorecards` | One per user per cycle — tracks status, scores, grade |
| `scorecard_kpis` | KPI targets & achievements per scorecard |
| `scorecard_competencies` | Competency self + manager ratings per scorecard |
| `competency_items` | PNSB competency items (Core + Functional) |
| `bell_curve_targets` | Target distribution per cycle per grade |
| `moderation_logs` | MOD1 / MOD2 grade changes audit |
| `score_overrides` | Manager override audit trail |
| `scorecard_status_logs` | Full scorecard status change history |
| `audits` | General audit trail (owen-it) — create/update/delete on 16 models + login/logout/failed-login; Super Admin viewer |

---

## 8. ✅ Decisions Confirmed

| #  | Topic                             | Decision                                                                                                                   |
| -- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 1  | **Company Name**            | PNSB = Permodalan Negeri Selangor Berhad                                                                                   |
| 2  | **BSC Weights**             | Financial 40%, Customer 30%, Internal 20%, Learning 10% — fixed                                                           |
| 3  | **Scoring Split**           | CEO: 100% KPI / Exec→Senior Mgmt: 80% KPI + 20% Competency / Support+Technical: 100% Competency                           |
| 4  | **KPI Target Tiers**        | 3-tier: Threshold / Meet Target / Stretched                                                                                |
| 5  | **Performance Categories**  | 5 levels: Perlu Diperbaiki / Memuaskan / Baik / Sangat Baik / Cemerlang                                                    |
| 6  | **Bell Curve Moderation**   | Confirmed — 2-round moderation (Bahagian Sumber Manusia / HR then Ketua Pegawai Eksekutif / KPE)                          |
| 7  | **Appraisal Cycle**         | Annual — January to December                                                                                              |
| 8  | **Company KPI Approval**    | Ahli Lembaga Pengarah (ALP / Board of Directors) approves company KPI                                                      |
| 9  | **Individual KPI Approval** | Ketua Pegawai Eksekutif (KPE / CEO) or Department Head approves individual KPIs                                            |
| 10 | **Role Architecture**       | Hierarchy-driven (manager_id chain)                                                                                        |
| 11 | **Rating Scale**            | Percentage-based (0–100%)                                                                                                 |
| 12 | **Appraisal score method**  | Manager directly overrides both KPI and Competency scores submitted by employee — no rejection back to employee, no resubmission. Manager edits score in-system. Staff self-rating is reference only. Manager is final authority. |

---

## 9. ❓ Questions & Decisions Tracker

> Legend: 🔴 **BLOCKER** — must decide before dev starts | ⚠️ **NEEDS DECISION** — needed before that module is built | 🔵 **KIV** — deferred to later phase | ✅ **DECIDED** — confirmed and locked

---

### 🔴 Blockers (Must Decide First — Pending PNSB HR)

| #  | Topic                                | Question                                                                                                                                                                                                                                                                                                                                                                                                                  | Status     |
| -- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| ~~B1~~ | ~~Score → Grade Boundary~~ | ✅ Resolved — moved to Decided (C5). Grade ranges confirmed from PNSB Excel embedded image. | ✅ Done |
| ~~B2~~ | ~~KPI 3-Tier Score Formula~~ | ✅ Resolved — moved to Decided (C7). Tier mapping confirmed from PNSB KPI1 Excel data. | ✅ Done |
| B3 | **Competency Scoring Method**  | How is self-rating + manager rating combined into one final competency score? See Section 11 for 5 method options with pros & cons.                                                                                                                                                                                                                                                                                       | 🔴 Pending |
| B4 | **KPI Structure Per Category** | Do different employee categories have different KPI sets and BSC structure? Specifically: (1) Do Sokongan/Teknikal staff fill in a KPI form at all, or skip it entirely since their score is 100% Penilaian Keperibadian? (2) Do Eksekutif and KPE share the same KPI items or have different sets? (3) Does the BSC weighting (Financial 40%, Customer 30% etc.) apply to all categories or only those with KPI scoring? | 🔴 Pending |

---

### ⚠️ Needs Decision (Before That Module Is Built)

| #  | Topic                                  | Question                                                                                                                                                                                        | Status   |
| -- | -------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| D1 | **Final Sign-off Layers**        | After moderation, who locks the final grades — HR only, or does KPE also formally sign off? Affects number of approval states in the system.                                                   | ⚠️ TBC |
| D2 | **Actual Competency Items**      | What are PNSB's actual Core and Functional competency items and descriptions? Needed for DB seeding and appraisal form UI.                                                                      | ⚠️ TBC |
| D3 | **Employee Category Boundaries** | Which exact job titles fall under each category (KPE / Eksekutif → Pengurusan Kanan / Sokongan / Teknikal)? Needed to auto-assign the correct scoring template when a user account is created. | ⚠️ TBC |
| D4 | **Subsidiary Handling**          | Same system shared with anak syarikat, or separate instances? Affects overall multi-tenancy architecture decision upfront.                                                                      | ⚠️ TBC |

---

### 🔵 KIV (Deferred — Later Phase)

| #  | Topic                                 | Note                                                                                                                   |
| -- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| K1 | **Bell Curve Hard Enforcement** | Hard system cap vs soft guideline — deferred. Phase 1 shows a warning only; hard enforcement scoped to a later phase. |
| K2 | **WhatsApp Integration**        | Submission reminder notifications via WhatsApp — pending scope definition.                                            |

---

### ✅ Decided (Locked — No Further Action Needed)

| #  | Topic                                  | Decision                                                                                                                                                                                                                    |
| -- | -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| C1 | **Manager Score Override**       | Manager directly overrides KPI and Competency scores submitted by employee — no rejection back to employee, no resubmission flow. Manager edits score in-system. Staff self-rating is reference only. Manager is final authority. |
| C2 | **Competency Rating Visibility** | Manager sees staff self-rating before entering their own score. Both versions saved for audit trail.                                                                                                                        |
| C3 | **ALP Role in Scoring Rules**    | Scoring splits per category (100% KPI / 80%+20% / 100% Competency) are formal ALP Board ketetapan — not HR or admin configurations. Changes require a new ALP resolution. Only Super Admin can update these in the system. |
| C4 | **System Terminology**           | See Section 10 — terms finalised for all modules and UI labels.                                                                                                                                                            |
| C5 | **Score → Grade Boundary**       | Confirmed from PNSB Excel embedded image: Cemerlang 90–100 / Sangat Baik 76–89 / Baik 60–75 / Memuaskan 50–59 / Perlu Diperbaiki < 50. System auto-assigns grade once final score is computed.                         |
| C6 | **Manager Score Override (KPI)** | During appraisal, manager directly overrides the KPI score — no dispute or mediation path. Manager is the higher authority; their score is final. Staff cannot contest the manager's rating in-system.                     |
| C7 | **KPI 3-Tier Score Formula**     | Confirmed from PNSB KPI1 Excel: Threshold (Ambangan) = 60, Meet Target (Setuju) = 80, Stretched (Lebihan) = 100, Below Threshold = 0. Hard cap at 100 — exceeding Stretched does not push score above 100. Formula: `Skor = (Pencapaian Sebenar / 100) × Jumlah Kecil Pemberat`. See `files/kpi1-excel-data.md` for full verification. |

---

## 10. Change Impact Notes

> **Build strategy**: Flow and DB design follow current ALP guidelines (locked architecture). These notes flag exactly what changes if a pending decision comes back differently — so we don't re-think from scratch.

---

### If B4 changes — Sokongan/Teknikal also fill KPI form

**Current assumption**: Sokongan/Teknikal skip KPI form entirely (100% Competency, 0% KPI).

**If HR confirms they DO fill KPI form:**

| What Changes | Impact |
|---|---|
| `scorecard_kpis` | Create rows for Sokongan/Teknikal scorecards too — they fill targets & achievements |
| `kpis` table | Add `category_scope` column to assign different KPI sets per category |
| KPI form routing | Show KPI form to ALL categories, not just KPI-scoring ones |
| Score calculation | No change — `kpi_weight = 0%` in their template already handles it |
| Admin UI | HR/Super Admin needs to manage & assign separate KPI sets per category |

**DB schema change**: Minor — one `category_scope` column on `kpis` table.
**Logic change**: Medium — form routing + KPI library assignment per category.

---

### If B3 changes — Competency scoring method differs per category

**Current assumption**: One competency scoring method applies to all categories that have competency (Eksekutif + Sokongan/Teknikal).

**If HR confirms different methods per category:**

| What Changes | Impact |
|---|---|
| `scorecard_templates` | Add `competency_method` column (e.g. `weighted`, `override`, `average`) |
| Score engine | Branching logic per category when computing `final_rating` on competency |
| Config | Method becomes per-template config, not a global setting |
| UI | No visible change to staff — only affects backend calculation |

**DB schema change**: Minor — one `competency_method` column on `scorecard_templates`.
**Logic change**: Low — swap formula based on template setting.

---

### If B3 changes — Competency method for Sokongan/Teknikal needs stronger protection

**Why**: Sokongan/Teknikal are 100% Competency — a biased manager can fully tank their score with no KPI to balance it out.

**If HR wants a safeguard for this category specifically:**

| Option | What to add |
|---|---|
| Method D (Capped Deviation) | Add deviation check + HR notification logic for Sokongan/Teknikal only |
| Justification field | Add `manager_justification` column on `scorecard_competencies` if deviation exceeds threshold |

**DB schema change**: Minor — one optional `manager_justification` column.
**Logic change**: Medium — deviation check + HR alert workflow.

---

## 11. System Terminology (Finalised)

All UI labels, module names, and field names in the system must follow this convention.

> **Language rule**: Malay label first, English in brackets. Applied consistently across all screens, forms, buttons, and reports.

### Core Module Terms

| Concept                       | System Label                                                     | Notes                                                                         |
| ----------------------------- | ---------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| KPI evaluation section        | **Petunjuk Prestasi Utama (KPI)**                          | Use full Malay name — never just "KPI" alone as a section title              |
| Competency evaluation section | **Kompetensi (Competencies)**                              | Use "Competencies" — not "Penilaian Keperibadian" or "Self Assessment" in UI |
| Balanced Scorecard framework  | **Kad Skor Imbangan (BSC)**                                | Used as the structural framework label                                        |
| Overall appraisal form        | **Borang Penilaian Prestasi (Performance Appraisal Form)** |                                                                               |
| Appraisal cycle               | **Kitaran Penilaian (Appraisal Cycle)**                    |                                                                               |
| Self rating (by staff)        | **Penilaian Kendiri (Self Assessment)**                    | Used inside the Competencies section                                          |
| Manager rating                | **Penilaian Pengurus (Manager Assessment)**                | Used inside the Competencies section                                          |
| Final score                   | **Skor Akhir (Final Score)**                               |                                                                               |
| Performance grade             | **Kategori Prestasi (Performance Grade)**                  |                                                                               |

### BSC Perspective Labels

| Malay                      | English (in brackets)       |
| -------------------------- | --------------------------- |
| Kewangan                   | (Financial)                 |
| Pelanggan                  | (Customer)                  |
| Proses Dalaman             | (Internal Business Process) |
| Pembelajaran & Peningkatan | (Learning & Growth)         |

### KPI Field Labels

| Concept                      | System Label                                      |
| ---------------------------- | ------------------------------------------------- |
| KPI target — minimum        | **Ambangan (Threshold)**                    |
| KPI target — expected       | **Sasaran (Meet Target)**                   |
| KPI target — above expected | **Lebihan (Stretched)**                     |
| Actual achievement           | **Pencapaian Sebenar (Actual Achievement)** |
| KPI weight (small)           | **Pemberat Kecil (Sub-weight)**             |
| KPI weight (total per BSC)   | **Pemberat Besar (BSC Weight)**             |
| Unit of measure              | **Unit Pengukuran (Unit of Measure)**       |

### Status Labels

| Status                | System Label                                |
| --------------------- | ------------------------------------------- |
| Draft / not submitted | **Draf (Draft)**                      |
| Submitted for review  | **Dikemukakan (Submitted)**           |
| Approved              | **Diluluskan (Approved)**             |
| Rejected              | **Ditolak (Rejected)**                |
| Locked / finalised    | **Dikunci (Locked)**                  |
| Under moderation      | **Dalam Moderasi (Under Moderation)** |
| Completed             | **Selesai (Completed)**               |

### Role Labels (as displayed in UI)

| Role            | System Label                                  |
| --------------- | --------------------------------------------- |
| CEO             | **Ketua Pegawai Eksekutif (KPE)**       |
| Department Head | **Ketua Bahagian (Department Head)**    |
| Staff           | **Kakitangan (Staff)**                  |
| HR Admin        | **Pentadbir Sumber Manusia (HR Admin)** |
| Super Admin     | **Super Pentadbir (Super Admin)**       |

---

## 12. Competency Scoring Method

Competency scoring follows **garis panduan dan ketetapan ALP** — the appraisal session is conducted strictly based on ALP Board guidelines and resolutions.

Both ratings (staff self-rating + manager rating) are always saved regardless of outcome — for transparency and audit trail.

> Pending B3 confirmation from PNSB HR. See **Section 10 Change Impact Notes** for what changes in DB and logic if the method differs per category.

---

## 13. Files Reference

| File                                            | Description                                                     |
| ----------------------------------------------- | --------------------------------------------------------------- |
| `flow-diagram.md`                             | Full Mermaid flow diagram — all phases + rejection cases        |
| `db-design.md`                               | Full database design — all tables, columns, indexes, business rules |
| `files/AIROD_PMS.pdf`                         | Reference PMS slide deck                                        |
| `files/908b7001f2ffdffaf1cc0693c17a88a2.xlsx` | PNSB KPI form (KPI1) + Process flow (Proses PMS) — binary      |
| `files/kpi1-excel-data.md`                   | Readable export of the Excel above — KPI table, tier scoring formula, B2 resolution proof |
| `files/image.png`                             | Bell curve summary — actual PNSB staff distribution (91 staff) |
