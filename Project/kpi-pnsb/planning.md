# 🎯 PNSB BSC KPI - Planning Document

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
8. **Sesi penilaian prestasi** — Appraisal session based on Ahli Lembaga Pengarah (ALP)-set guidelines
9. **Sesi Moderasi** — Moderation session by Department Head and Ketua Pegawai Eksekutif (KPE / CEO)
10. **Kategori Pemarkahan** — Final grade assignment

---

## 5. Performance Rating Categories (Bell Curve)

5-tier grading system (in order, lowest to highest):

| # | Kategori         | Meaning           |
| - | ---------------- | ----------------- |
| 1 | Perlu Diperbaiki | Needs Improvement |
| 2 | Memuaskan        | Satisfactory      |
| 3 | Baik             | Good              |
| 4 | Sangat Baik      | Very Good         |
| 5 | Cemerlang        | Excellent         |

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

- **Roles**:
  1. **Super Admin**: Full system access, system configuration, user role assignment
  2. **Admin — Bahagian Sumber Manusia (HR)**: Manages PNSB appraisal operations, receives KPI forms, runs moderation
  3. **Ketua Pegawai Eksekutif (KPE / CEO)**: Approves individual KPIs, leads moderation, top-level view
  4. **Ketua Bahagian / Jabatan (Department Head)**: Approves department staff KPIs, participates in moderation
  5. **Kakitangan (Staff)**: Prepares own KPIs, self-assesses, views own results

> **Role Architecture**: Hierarchy-driven — a Department Head is both reviewer (for their reports) and reviewee (reviewed by the Ketua Pegawai Eksekutif). The `manager_id` chain controls who reviews whom.

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

---

## 7. Database Schema (High-Level)

```
users                   (id, name, category, manager_id, department_id, job_title)
appraisal_cycles        (id, year, status: draft/active/appraisal/moderation/closed)
bsc_perspectives        (id, name, weight)                              -- 4 fixed perspectives
kpis                    (id, title, bsc_perspective_id, uom, level: company/dept/individual)
scorecard_templates     (id, category, kpi_weight, competency_weight)   -- per employee category
scorecards              (id, user_id, cycle_id, template_id, status, final_score, grade)
scorecard_kpis          (scorecard_id, kpi_id, bsc_weight, kpi_weight, threshold, meet_target, stretched, actual, score)
competency_items        (id, title, description, type: core/functional)
scorecard_competencies  (scorecard_id, competency_id, self_rating, manager_rating, final_rating)
bell_curve_targets      (cycle_id, grade_category, target_count, target_pct)
moderation_logs         (scorecard_id, cycle_id, before_grade, after_grade, moderated_by, notes)
```

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
| 12 | **Competency scoring**      | Manager sees self-rating first, then enters own rating — supervisor's rating is the final score (overrides, not averaged) |
| 13 | **KPI rejection flow**      | Rejected KPI → staff edits and resubmits — manager does not edit staff KPI directly                                      |

---

## 9. ❓ Open Questions (Keep In View)

| # | Topic                               | Status                                                                                                                                                         |
| - | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | **Competency scoring method** | ⚠️**Needs decision** — See Section 10 for full method comparison with pros & cons                                                                     |
| 2 | **KPI rejection flow**        | ✅**Decided** — Manager rejects with a written reason. Staff receives rejection + reason, edits the KPI, and resubmits. Manager does not edit directly. |
| 3 | **Bell curve enforcement**    | Hard cap per category, or target/guideline only? TBC                                                                                                           |
| 4 | **Subsidiary handling**       | Same system for anak syarikat? Separate instances or shared? TBC                                                                                               |
| 5 | **WhatsApp integration**      | Keep In View (KIV) — notification via WhatsApp for submission reminders — pending scope                                                                      |
| 6 | **Final sign-off layers**     | Exact flow after moderation: HR only, or additional layer? TBC                                                                                                 |

---

com

## 10. Competency Scoring Method — Options Comparison

When a staff member submits their Penilaian Keperibadian (Competency Assessment), there are **two ratings** on the table — the staff's self-rating and the manager's rating. The question is: **how does the system compute the final competency score?**

> Both ratings are always saved regardless of which method is chosen — for transparency and audit trail.

---

### Method A — Manager Override (Supervisor Replaces Self-Rating)

**How it works:** Manager sees the staff's self-rating, then enters their own score. The manager's score completely replaces the self-rating as the final score. Self-rating is stored for reference only.

**Example:**

| Competency    | Self-Rating | Manager Rating | Final Score   |
| ------------- | ----------- | -------------- | ------------- |
| Teamwork      | 85%         | 70%            | **70%** |
| Communication | 90%         | 80%            | **80%** |

**Pros:**

- Simple — no formula complexity
- Manager has full accountability for the final score
- Reduces grade inflation from over-confident self-raters
- The reference system (AIROD PMS PDF) uses this method

**Cons:**

- Staff may feel their self-reflection is pointless if it has zero impact
- Risk of biased managers giving unfairly low scores with no check
- No incentive for staff to take self-assessment seriously

---

### Method B — Simple Average (50/50)

**How it works:** Final score = (Self-Rating + Manager Rating) ÷ 2. Both carry equal weight.

**Example:**

| Competency    | Self-Rating | Manager Rating | Final Score     |
| ------------- | ----------- | -------------- | --------------- |
| Teamwork      | 85%         | 70%            | **77.5%** |
| Communication | 90%         | 80%            | **85%**   |

**Pros:**

- Fair and transparent — both parties contribute equally
- Staff feel their self-assessment matters
- Easy to explain to employees

**Cons:**

- A dishonest staff member who inflates their self-rating pulls the final score up
- Manager and staff may "negotiate" scores rather than rate honestly
- Does not reflect real-world practice where the manager's view usually carries more weight

---

### Method C — Weighted Average (Manager Heavier)

**How it works:** Final score = (Self-Rating × W1) + (Manager Rating × W2), where W2 > W1. Common split: 30% self + 70% manager.

**Example (30/70 split):**

| Competency    | Self-Rating | Manager Rating | Final Score                            |
| ------------- | ----------- | -------------- | -------------------------------------- |
| Teamwork      | 85%         | 70%            | (85×0.3) + (70×0.7) =**74.5%** |
| Communication | 90%         | 80%            | (90×0.3) + (80×0.7) =**83%**   |

**Pros:**

- Self-rating still has influence — encourages honest self-reflection
- Manager's view dominates the outcome — aligns with organisational authority
- Reduces impact of inflated self-ratings compared to Method B
- Weights are configurable (can adjust per category or cycle)

**Cons:**

- Slightly more complex to explain to staff
- Weight ratio needs a decision (30/70? 20/80?) — adds another open question
- Still vulnerable to extreme self-inflation if weight is too high

---

### Method D — Capped Deviation (Manager Can Only Adjust Within a Range)

**How it works:** Manager cannot deviate more than a set range (e.g. ±15%) from the staff's self-rating without providing a written justification. If deviation exceeds the threshold, HR is notified.

**Example (±15% cap without justification):**

| Competency    | Self-Rating | Manager Rating | Within Cap?                                  | Final Score   |
| ------------- | ----------- | -------------- | -------------------------------------------- | ------------- |
| Teamwork      | 85%         | 70%            | ✅ Yes (diff = 15%)                          | **70%** |
| Communication | 90%         | 60%            | ❌ No (diff = 30%) — justification required | Pending       |

**Pros:**

- Protects staff from unfair/vindictive managers
- Forces managers to justify large score differences — creates accountability
- Encourages honest rating from both sides

**Cons:**

- Most complex to implement — needs justification workflow and HR notification
- Can slow down the appraisal process if many justifications are triggered
- May not suit PNSB's current maturity level for a first-phase system

---

### Method E — Blind Rating (Both Rate Independently, Then Compare)

**How it works:** Staff submits self-rating first. Manager rates independently **without seeing** the self-rating. After both are submitted, the system reveals both scores side-by-side. Manager's score is used as final, but the gap is visible to both parties for discussion.

**Example:**

| Competency    | Self-Rating | Manager Rating | Gap  | Final Score   |
| ------------- | ----------- | -------------- | ---- | ------------- |
| Teamwork      | 85%         | 70%            | -15% | **70%** |
| Communication | 90%         | 80%            | -10% | **80%** |

**Pros:**

- Eliminates anchoring bias — manager is not influenced by seeing self-rating first
- Gap report is useful for development conversations
- Completely honest ratings from both sides

**Cons:**

- Harder to implement (must hide self-rating from manager until they submit)
- Manager loses context — may rate something lower simply because they forgot an incident the staff documented
- Unusual for Malaysian HR practices — may confuse users

---

### Summary & Recommendation

| Method                | Complexity | Staff Fairness | Manager Authority | Grade Inflation Risk | Recommended For                     |
| --------------------- | ---------- | -------------- | ----------------- | -------------------- | ----------------------------------- |
| A — Override         | Low        | Low            | High              | Low                  | Simple first-phase system           |
| B — 50/50 Average    | Low        | High           | Medium            | High                 | Collaborative cultures              |
| C — Weighted Average | Medium     | Medium         | High              | Medium               | **Balanced — good for PNSB** |
| D — Capped Deviation | High       | High           | Medium            | Low                  | Mature HR systems                   |
| E — Blind Rating     | High       | Medium         | High              | Low                  | Research / academic orgs            |

> **Suggested starting point for PNSB**: **Method C (Weighted Average, 30% self / 70% manager)** — gives staff a voice while keeping the manager in control. Simple enough for a first-phase system. The weights can be made configurable so PNSB can adjust later without code changes.
>
> **Decision needed from PNSB HR**: Which method do they want to use?

---

## 12. Files Reference

| File                                            | Description                                                     |
| ----------------------------------------------- | --------------------------------------------------------------- |
| `flow-diagram.md`                             | Full Mermaid flow diagram — all phases + rejection cases       |
| `files/AIROD_PMS.pdf`                         | Reference PMS slide deck                                        |
| `files/908b7001f2ffdffaf1cc0693c17a88a2.xlsx` | PNSB KPI form (KPI1) + Process flow (Proses PMS)                |
| `files/image.png`                             | Bell curve summary — actual PNSB staff distribution (91 staff) |
