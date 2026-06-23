# 🎯 PMS: KPI & Competency Balanced Scorecard (BSC) - Brainstorming

## 1. Core Concept
A comprehensive Performance Management System that evaluates employees on two main pillars:
- **KPIs (Quantitative)**: "WHAT" the employee achieved (e.g., revenue generated, bugs fixed, project delivery).
- **Competencies (Qualitative)**: "HOW" they achieved it (e.g., teamwork, leadership, problem-solving).

Both are structured using the **Balanced Scorecard (BSC)**'s 4 perspectives to ensure alignment with company goals:
1. 💰 Financial
2. 🤝 Customer
3. ⚙️ Internal Business Processes
4. 🧠 Learning & Growth

---

## 2. Key Modules & Features

### 👥 User & Organization Management
- **Roles**:
  1. **Super Admin**: Master of the system. Can access all system configurations, the upcoming WhatsApp integration setup (KIV), and has the authority to assign 'Admin' roles to others.
  2. **Admin**: Manages standard PMS operations, HR duties, and company-wide appraisal data.
  3. **User**: Everyone else (CEO, Managers, Staff, etc.). Their exact permissions and views will be dictated by the reporting hierarchy.
- **Hierarchy**: Departments, Job Titles, and Reporting Lines (vital for knowing who reviews whom).

> *(TBC)* **Role Architecture** — Two options under consideration:
> - **Option A (Role-based)**: Explicit roles — `Super Admin` → `Admin` → `Manager` → `Employee`. Cleaner with Spatie permissions.
> - **Option B (Hierarchy-driven)**: Everyone is a `User`. System checks `manager_id` to unlock manager-level views/actions. More flexible — a person can be both a reviewer (has direct reports) AND a reviewee (has their own manager) simultaneously. Example: CEO reviews C-Suite but is also reviewed by the Board.
> - **Pending confirmation**: Does the org have people who are both reviewers AND reviewees at the same time? If yes, Option B is the right call.

### 📚 Assessment Libraries
- **KPI Dictionary**: Reusable KPIs mapped to the 4 BSC perspectives.
- **Scorecard Templates**: Defines the weightage **dynamically per job role**. For example, a Sales Rep might have 70% KPI / 30% Competency, while a Manager might have 50% KPI / 50% Competency. Each template holds its own KPI/Competency split, allowing full flexibility across the organization.

### 🧠 Competency Framework

**Competencies = the "HOW"**
While KPIs measure *what* was achieved (numbers, targets, results), competencies measure *how* the employee behaved and operated to get there.

> Two employees both hit 100% of their sales KPI — but one did it by lying to clients, the other by building genuine relationships. KPIs can't tell the difference. Competencies can.

#### Two Types of Competencies

**1. Core Competencies** — Applied to *everyone* regardless of role:
- Integrity & Professionalism
- Teamwork & Collaboration
- Communication Skills
- Initiative & Problem Solving

**2. Functional / Technical Competencies** — Role-specific skills:
- Example (Developer): Code Quality, System Design Thinking, Debugging Ability
- Example (Finance): Financial Reporting Accuracy, Regulatory Compliance Knowledge

#### How Competencies Are Scored

Using the **0–100% rating scale**. Both employee and manager rate each competency independently:

| Competency | Self Rating | Manager Rating |
|---|---|---|
| Teamwork | 80% | 75% |
| Communication | 90% | 85% |
| Technical Skills | 70% | 72% |

Scores are weighted and rolled up into an overall **Competency Score**, which then combines with the **KPI Score** based on the Scorecard Template split (e.g. 50% KPI / 50% Competency).

### 🔄 The Appraisal Workflow (The Engine)
1. **KPI Proposal**: Employee sets and submits their own KPIs for the cycle.
2. **KPI Approval**: KPIs go through an approval process before being locked.
   - *(KIV)* Who approves — Manager only, or HR also?
   - *(KIV)* Rejection flow — does employee revise & resubmit, or Manager edits directly?
   - *(KIV)* Target-setting — does employee set their own targets, or Manager sets after approval?
3. **KPIs Locked ✅**: Once approved, KPIs and targets are frozen for the cycle.
4. **Continuous Feedback**: A journal/diary feature to log achievements throughout the year (so we don't forget by December!).
5. **Year-End Evaluation**:
   - **Self-Assessment**: Employee rates themselves against their locked KPIs.
   - **Manager Assessment**: Manager reviews and provides the final rating.
   - **Moderation** *(KIV)*: HR adjusts scores to fit a company bell curve — forces grade distribution (e.g. ~5% Outstanding, ~60% Average, ~5% Poor) to prevent grade inflation. Will require a dedicated moderation module where HR can override final scores before sign-off. To be scoped in a later phase.
6. **Final Sign-off** *(KIV — exact approval layers TBC)*: Management-driven sign-off. Exact flow (e.g. Employee ➡️ Manager only, or additional Dept Head / HR layer) to be confirmed before development.

### 📊 Dashboards & Analytics (Vue + ApexCharts)
- **Employee Dash**: Spider-web/Radar charts for competencies, gauge charts for KPI targets.
- **Manager Dash**: Team completion status, performance spread.
- **Admin Dash**: Company-wide analytics, bell curve distribution.

---

## 3. Database Schema Ideas (High-Level)

Here is a rough sketch of how the data will relate in Laravel:

- `users` (id, name, role, manager_id, department_id)
- `appraisal_cycles` (id, year, status: planning/active/review/closed)
- `kpis` (id, title, bsc_perspective, uom)
- `competencies` (id, title, description)
- `scorecards` (id, user_id, cycle_id, status, final_score, grade)
- `scorecard_kpis` (scorecard_id, kpi_id, weight, target, actual, score)
- `scorecard_competencies` (scorecard_id, competency_id, weight, self_rating, manager_rating)

---

## 4. ✅ Decisions Made

| # | Topic | Decision |
|---|-------|----------|
| 1 | **Weightage Rules** | **Dynamic per job role** — each Scorecard Template defines its own KPI/Competency split |
| 2 | **Approval Flow** | **Management-driven** — exact layers (2-layer or 3-layer) TBC *(KIV)* |
| 3 | **Rating Scale** | **Percentage-based (0–100%)** — all scores expressed as percentages, not a 1–5 scale |
| 4 | **Bell Curve Moderation** | **KIV** — will be used but scoped to a later phase |
