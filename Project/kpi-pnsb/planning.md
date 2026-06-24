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

| #  | Topic                             | Decision                                                                                         |
| -- | --------------------------------- | ------------------------------------------------------------------------------------------------ |
| 1  | **Company Name**            | PNSB = Permodalan Negeri Selangor Berhad                                                         |
| 2  | **BSC Weights**             | Financial 40%, Customer 30%, Internal 20%, Learning 10% — fixed                                 |
| 3  | **Scoring Split**           | CEO: 100% KPI / Exec→Senior Mgmt: 80% KPI + 20% Competency / Support+Technical: 100% Competency |
| 4  | **KPI Target Tiers**        | 3-tier: Threshold / Meet Target / Stretched                                                      |
| 5  | **Performance Categories**  | 5 levels: Perlu Diperbaiki / Memuaskan / Baik / Sangat Baik / Cemerlang                          |
| 6  | **Bell Curve Moderation**   | Confirmed — 2-round moderation (Bahagian Sumber Manusia / HR then Ketua Pegawai Eksekutif / KPE) |
| 7  | **Appraisal Cycle**         | Annual — January to December                                                                      |
| 8  | **Company KPI Approval**    | Ahli Lembaga Pengarah (ALP / Board of Directors) approves company KPI                             |
| 9  | **Individual KPI Approval** | Ketua Pegawai Eksekutif (KPE / CEO) or Department Head approves individual KPIs                   |
| 10 | **Role Architecture**       | Hierarchy-driven (manager_id chain)                                                              |
| 11 | **Rating Scale**            | Percentage-based (0–100%)                                                                       |

---

## 9. ❓ Open Questions (KIV)

| # | Topic                               | Status                                                           |
| - | ----------------------------------- | ---------------------------------------------------------------- |
| 1 | **Competency scoring method** | Self + Manager average? Or weighted? TBC                         |
| 2 | **KPI rejection flow**        | Does staff revise & resubmit, or Dept Head edits directly? TBC   |
| 3 | **Bell curve enforcement**    | Hard cap per category, or target/guideline only? TBC             |
| 4 | **Subsidiary handling**       | Same system for anak syarikat? Separate instances or shared? TBC |
| 5 | **WhatsApp integration**      | Keep In View (KIV) — notification via WhatsApp for submission reminders — pending scope |
| 6 | **Final sign-off layers**     | Exact flow after moderation: HR only, or additional layer? TBC   |

---

## 10. Files Reference

| File                                            | Description                                                     |
| ----------------------------------------------- | --------------------------------------------------------------- |
| `flow-diagram.md`                             | Full Mermaid flow diagram — all phases + rejection cases        |
| `files/AIROD_PMS.pdf`                         | Reference PMS slide deck                                        |
| `files/908b7001f2ffdffaf1cc0693c17a88a2.xlsx` | PNSB KPI form (KPI1) + Process flow (Proses PMS)                |
| `files/image.png`                             | Bell curve summary — actual PNSB staff distribution (91 staff) |
