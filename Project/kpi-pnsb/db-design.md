# PNSB BSC KPI — Database Design
> Follows locked ALP guidelines. See Change Impact section for B3/B4 contingency columns.
> Last updated: 2026-06-24

---

## Design Principles

- All scoring rules are **ALP Board ketetapan** — stored as data, not hardcoded logic
- `manager_id` chain on `users` drives the entire review hierarchy
- Both self-rating and manager rating always saved — immutable audit trail
- Status progression is one-way and logged on every change
- Enums used for ALP-locked values; VARCHAR for anything HR might extend later

---

## Tables

---

### 1. `departments`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
name_my         VARCHAR(150) NOT NULL,               -- e.g. Bahagian Sumber Manusia
name_en         VARCHAR(150) NOT NULL,               -- e.g. Human Resources
parent_id       BIGINT UNSIGNED NULL,                -- NULL = top-level
head_user_id    BIGINT UNSIGNED NULL,                -- FK users (set after users created)
created_at      TIMESTAMP,
updated_at      TIMESTAMP
```

---

### 2. `users`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
name            VARCHAR(150) NOT NULL,
employee_no     VARCHAR(50) UNIQUE NOT NULL,
email           VARCHAR(150) UNIQUE NOT NULL,
password        VARCHAR(255) NOT NULL,
job_title       VARCHAR(150) NULL,                   -- D3: pending exact titles
department_id   BIGINT UNSIGNED NOT NULL,            -- FK departments
manager_id      BIGINT UNSIGNED NULL,                -- FK users (NULL = KPE)
category        ENUM(
                  'kpe',
                  'eksekutif_pengurusan_kanan',
                  'sokongan_teknikal'
                ) NOT NULL,                          -- drives scoring template
role            ENUM(
                  'super_admin',
                  'hr_admin',
                  'kpe',
                  'ketua_bahagian',
                  'kakitangan'
                ) NOT NULL,
is_active       BOOLEAN DEFAULT TRUE,
created_at      TIMESTAMP,
updated_at      TIMESTAMP
```

> A user has both `category` (determines scoring formula) and `role` (determines system permissions).
> A Ketua Bahagian is `category = eksekutif_pengurusan_kanan` and `role = ketua_bahagian` — they have their own scorecard AND review their team.

---

### 3. `appraisal_cycles`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
year            YEAR NOT NULL UNIQUE,
status          ENUM(
                  'draft',
                  'active',           -- KPI setting phase open
                  'appraisal',        -- year-end scoring phase
                  'moderation',       -- bell curve moderation
                  'closed'
                ) DEFAULT 'draft',
opened_by       BIGINT UNSIGNED NOT NULL,            -- FK users (HR Admin)
opened_at       TIMESTAMP NULL,
closed_at       TIMESTAMP NULL,
created_at      TIMESTAMP,
updated_at      TIMESTAMP
```

---

### 4. `bsc_perspectives`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
name_my         VARCHAR(100) NOT NULL,               -- e.g. Kewangan
name_en         VARCHAR(100) NOT NULL,               -- e.g. Financial
weight          DECIMAL(5,2) NOT NULL,               -- 40.00, 30.00, 20.00, 10.00
sort_order      TINYINT UNSIGNED NOT NULL,
created_at      TIMESTAMP,
updated_at      TIMESTAMP
```

**Seed data (fixed — ALP ketetapan):**

| sort_order | name_my | name_en | weight |
|---|---|---|---|
| 1 | Kewangan | Financial | 40.00 |
| 2 | Pelanggan | Customer | 30.00 |
| 3 | Proses Dalaman | Internal Business Process | 20.00 |
| 4 | Pembelajaran & Peningkatan | Learning & Growth | 10.00 |

---

### 5. `kpis`

```sql
id                  BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
title_my            VARCHAR(255) NOT NULL,
title_en            VARCHAR(255) NULL,
bsc_perspective_id  BIGINT UNSIGNED NOT NULL,        -- FK bsc_perspectives
uom                 VARCHAR(100) NOT NULL,            -- e.g. RM, Tarikh, Bil, %
level               ENUM('company', 'department', 'individual') NOT NULL,
category_scope      VARCHAR(50) NULL,                -- B4: NULL = all, or 'kpe' / 'eksekutif_pengurusan_kanan'
is_active           BOOLEAN DEFAULT TRUE,
created_by          BIGINT UNSIGNED NOT NULL,        -- FK users
created_at          TIMESTAMP,
updated_at          TIMESTAMP
```

> `category_scope` is pre-added for B4. NULL means KPI applies to all KPI-scoring categories.

---

### 6. `scorecard_templates`

```sql
id                  BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
category            ENUM(
                      'kpe',
                      'eksekutif_pengurusan_kanan',
                      'sokongan_teknikal'
                    ) NOT NULL UNIQUE,
kpi_weight          DECIMAL(5,2) NOT NULL,           -- 100.00 / 80.00 / 0.00
competency_weight   DECIMAL(5,2) NOT NULL,           -- 0.00 / 20.00 / 100.00
competency_method   VARCHAR(50) NULL,                -- B3: 'override' / 'weighted' / etc — NULL until decided
created_at          TIMESTAMP,
updated_at          TIMESTAMP
```

**Seed data (fixed — ALP ketetapan):**

| category | kpi_weight | competency_weight |
|---|---|---|
| kpe | 100.00 | 0.00 |
| eksekutif_pengurusan_kanan | 80.00 | 20.00 |
| sokongan_teknikal | 0.00 | 100.00 |

---

### 7. `scorecards`

```sql
id                  BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
user_id             BIGINT UNSIGNED NOT NULL,        -- FK users
cycle_id            BIGINT UNSIGNED NOT NULL,        -- FK appraisal_cycles
template_id         BIGINT UNSIGNED NOT NULL,        -- FK scorecard_templates
status              ENUM(
                      'draft',                       -- not yet submitted
                      'kpi_submitted',               -- staff submitted KPI targets
                      'kpi_approved',                -- manager approved KPI targets
                      'appraisal',                   -- year-end scoring in progress
                      'moderation',                  -- under bell curve moderation
                      'completed',                   -- final grade assigned
                      'locked'                       -- immutable, communicated to staff
                    ) DEFAULT 'draft',
kpi_score           DECIMAL(5,2) NULL,               -- computed from scorecard_kpis
competency_score    DECIMAL(5,2) NULL,               -- computed from scorecard_competencies
final_score         DECIMAL(5,2) NULL,               -- kpi_score * kpi_weight + competency_score * competency_weight
grade               ENUM(
                      'cemerlang',
                      'sangat_baik',
                      'baik',
                      'memuaskan',
                      'perlu_diperbaiki'
                    ) NULL,
locked_at           TIMESTAMP NULL,
locked_by           BIGINT UNSIGNED NULL,            -- FK users (HR Admin)
created_at          TIMESTAMP,
updated_at          TIMESTAMP,

UNIQUE KEY uq_user_cycle (user_id, cycle_id)
```

**Grade auto-assignment rule (C5):**

| final_score | grade |
|---|---|
| >= 90 | cemerlang |
| >= 76 | sangat_baik |
| >= 60 | baik |
| >= 50 | memuaskan |
| < 50 | perlu_diperbaiki |

---

### 8. `scorecard_kpis`

```sql
id                      BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
scorecard_id            BIGINT UNSIGNED NOT NULL,    -- FK scorecards
kpi_id                  BIGINT UNSIGNED NOT NULL,    -- FK kpis
bsc_perspective_id      BIGINT UNSIGNED NOT NULL,    -- FK bsc_perspectives (denormalised for query speed)
kpi_weight              DECIMAL(5,2) NOT NULL,       -- Jumlah Kecil Pemberat e.g. 20.00
threshold               VARCHAR(100) NOT NULL,       -- Ambangan — VARCHAR supports RM, date, text
meet_target             VARCHAR(100) NOT NULL,       -- Setuju
stretched               VARCHAR(100) NOT NULL,       -- Lebihan
actual                  VARCHAR(100) NULL,           -- Pencapaian Sebenar (raw value)
pencapaian_sebenar      DECIMAL(5,2) NULL,           -- Converted tier score: 0 / 60 / 80 / 100
skor                    DECIMAL(5,2) NULL,           -- pencapaian_sebenar / 100 * kpi_weight
notes                   TEXT NULL,                   -- Catatan
created_at              TIMESTAMP,
updated_at              TIMESTAMP
```

**Tier scoring logic (C7 — locked):**

| Tier achieved | pencapaian_sebenar |
|---|---|
| Below Threshold | 0 |
| Threshold (Ambangan) | 60 |
| Meet Target (Setuju) | 80 |
| Stretched (Lebihan) | 100 |

`kpi_score` on `scorecards` = SUM(skor) across all scorecard_kpis for that scorecard.

---

### 9. `competency_items`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
title_my        VARCHAR(255) NOT NULL,
title_en        VARCHAR(255) NULL,
description     TEXT NULL,
type            ENUM('core', 'functional') NOT NULL,
is_active       BOOLEAN DEFAULT TRUE,
created_at      TIMESTAMP,
updated_at      TIMESTAMP
```

> D2: Actual PNSB competency items pending from HR — needed for seeding.

---

### 10. `scorecard_competencies`

```sql
id                      BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
scorecard_id            BIGINT UNSIGNED NOT NULL,    -- FK scorecards
competency_item_id      BIGINT UNSIGNED NOT NULL,    -- FK competency_items
self_rating             DECIMAL(5,2) NULL,           -- Penilaian Kendiri (0–100)
manager_rating          DECIMAL(5,2) NULL,           -- Penilaian Pengurus (0–100) — manager override, FINAL
final_rating            DECIMAL(5,2) NULL,           -- derived per B3 method
manager_justification   TEXT NULL,                   -- B3: used if deviation threshold exceeded
created_at              TIMESTAMP,
updated_at              TIMESTAMP,

UNIQUE KEY uq_scorecard_competency (scorecard_id, competency_item_id)
```

> `manager_rating` is the override value — always becomes `final_rating` under current ALP guidelines (C1).
> `manager_justification` pre-added for B3 deviation safeguard scenario.

`competency_score` on `scorecards` = AVG(final_rating) across all scorecard_competencies for that scorecard.

---

### 11. `bell_curve_targets`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
cycle_id        BIGINT UNSIGNED NOT NULL,            -- FK appraisal_cycles
grade           ENUM(
                  'cemerlang',
                  'sangat_baik',
                  'baik',
                  'memuaskan',
                  'perlu_diperbaiki'
                ) NOT NULL,
target_count    INT UNSIGNED NOT NULL,               -- e.g. 3
target_pct      DECIMAL(5,2) NOT NULL,               -- e.g. 3.30
created_at      TIMESTAMP,
updated_at      TIMESTAMP,

UNIQUE KEY uq_cycle_grade (cycle_id, grade)
```

**Seed data per cycle (based on 91 staff — scales per headcount):**

| grade | target_count | target_pct |
|---|---|---|
| cemerlang | 3 | 3.30 |
| sangat_baik | 16 | 17.58 |
| baik | 61 | 67.03 |
| memuaskan | 7 | 7.69 |
| perlu_diperbaiki | 4 | 4.40 |

---

### 12. `moderation_logs`

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
scorecard_id    BIGINT UNSIGNED NOT NULL,            -- FK scorecards
cycle_id        BIGINT UNSIGNED NOT NULL,            -- FK appraisal_cycles
round           ENUM('MOD1', 'MOD2') NOT NULL,       -- MOD1 = HR, MOD2 = KPE
before_score    DECIMAL(5,2) NULL,
after_score     DECIMAL(5,2) NULL,
before_grade    ENUM('cemerlang','sangat_baik','baik','memuaskan','perlu_diperbaiki') NULL,
after_grade     ENUM('cemerlang','sangat_baik','baik','memuaskan','perlu_diperbaiki') NULL,
moderated_by    BIGINT UNSIGNED NOT NULL,            -- FK users
notes           TEXT NULL,
moderated_at    TIMESTAMP NOT NULL,
created_at      TIMESTAMP
```

---

### 13. `score_overrides`

> Audit trail — every time a manager overrides a KPI or Competency score, a record is written here.

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
scorecard_id    BIGINT UNSIGNED NOT NULL,            -- FK scorecards
override_type   ENUM('kpi', 'competency') NOT NULL,
field_ref_id    BIGINT UNSIGNED NULL,                -- FK scorecard_kpis.id or scorecard_competencies.id
before_value    DECIMAL(5,2) NULL,
after_value     DECIMAL(5,2) NOT NULL,
overridden_by   BIGINT UNSIGNED NOT NULL,            -- FK users (manager)
notes           TEXT NULL,
overridden_at   TIMESTAMP NOT NULL,
created_at      TIMESTAMP
```

---

### 14. `scorecard_status_logs`

> Full audit of every scorecard status change.

```sql
id              BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
scorecard_id    BIGINT UNSIGNED NOT NULL,            -- FK scorecards
from_status     VARCHAR(50) NULL,                    -- NULL = initial creation
to_status       VARCHAR(50) NOT NULL,
changed_by      BIGINT UNSIGNED NOT NULL,            -- FK users
notes           TEXT NULL,
changed_at      TIMESTAMP NOT NULL,
created_at      TIMESTAMP
```

---

## Relationships Summary

```
departments ──< users (department_id)
users ──< users (manager_id — self-referential hierarchy)

appraisal_cycles ──< scorecards
users ──< scorecards
scorecard_templates ──< scorecards

scorecards ──< scorecard_kpis
kpis ──< scorecard_kpis
bsc_perspectives ──< scorecard_kpis

scorecards ──< scorecard_competencies
competency_items ──< scorecard_competencies

appraisal_cycles ──< bell_curve_targets
scorecards ──< moderation_logs
scorecards ──< score_overrides
scorecards ──< scorecard_status_logs

bsc_perspectives ──< kpis
```

---

## Key Indexes

```sql
-- scorecards
INDEX idx_scorecards_user_cycle (user_id, cycle_id)
INDEX idx_scorecards_cycle_status (cycle_id, status)
INDEX idx_scorecards_grade (cycle_id, grade)

-- scorecard_kpis
INDEX idx_scorecard_kpis_scorecard (scorecard_id)
INDEX idx_scorecard_kpis_bsc (bsc_perspective_id)

-- scorecard_competencies
INDEX idx_scorecard_comp_scorecard (scorecard_id)

-- users
INDEX idx_users_manager (manager_id)
INDEX idx_users_department (department_id)
INDEX idx_users_category (category)

-- moderation_logs
INDEX idx_mod_logs_cycle_round (cycle_id, round)
```

---

## Business Rules (enforced at application layer)

| Rule | Where Enforced |
|---|---|
| `sokongan_teknikal` → no `scorecard_kpis` created | Service layer on scorecard creation |
| `kpe` → no `scorecard_competencies` created | Service layer on scorecard creation |
| `manager_rating` always becomes `final_rating` (C1) | Competency score service |
| `pencapaian_sebenar` only ever = 0, 60, 80, 100 (C7) | KPI score service |
| `final_score` = kpi_score × (kpi_weight/100) + competency_score × (competency_weight/100) | Score calculation service |
| Grade auto-assigned from `final_score` thresholds (C5) | Grade assignment service |
| BSC weights must sum to 100% | Validation on `bsc_perspectives` update |
| KPI `kpi_weight` per BSC perspective must sum to that perspective's weight | Validation on scorecard_kpis |
| Only `super_admin` can update `scorecard_templates` weights (ALP ketetapan — C3) | Role gate |
| Scorecard status is one-way — cannot revert | Status transition guard |

---

## Change Impact (B3 / B4)

### If B3 — competency method confirmed
- Set `scorecard_templates.competency_method` value per category
- Update score calculation service to branch by method
- No schema change needed

### If B3 — different method per category with deviation safeguard
- `scorecard_competencies.manager_justification` already exists — just activate the logic
- Add HR notification trigger in service layer

### If B4 — Sokongan/Teknikal also fill KPI form
- Add `category_scope` value to relevant `kpis` rows (column already exists)
- Update scorecard creation service to generate `scorecard_kpis` for `sokongan_teknikal`
- Update KPI form routing in UI
- `kpi_weight` stays 0 in template — final score calculation unchanged
