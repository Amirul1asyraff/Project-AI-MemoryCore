# 📖 Daily Diary 001 - Universal AI Memory
*Optional conversation archive - load on-demand only*

## Archive Status
- **File**: Daily-Diary-001.md
- **Status**: Active (will auto-archive at 1000 lines)
- **Lines**: 85
- **Created**: 2026-04-22

---

## 🎯 **Auto-Archive Protocol**

When this file reaches 1000 lines:
1. **Move** to `archive/Daily-Diary-001.md`
2. **Create** new `Daily-Diary-002.md`  
3. **Continue** diary in new file
4. **Update** master reference

---

## Session: 2026-04-22 - Project Memory Update & Library System Revision

### Context
Amirul requested memory update with projects in repository, then revision of library system specs to use Vue.js frontend (aligned with laravel-workflow.md guide).

### Actions Taken

**1. Memory System Update**
- Created `project_ai_memorycore.md` - documented AI MemoryCore system (v4.2, 4-tier features, 4 contributors)
- Updated `project_library_system.md` - Malaysia Public Library Management System specs
- Updated `MEMORY.md` index with both projects

**2. Library System Specification Revision**
- Read `laravel-workflow.md` (comprehensive Laravel + Vue workflow guide)
- Updated `LIBRARY_SPEC_ARCHITECTURE.md`:
  - Frontend: Blade + Alpine.js → Vue.js 3 SPA with Vite
  - Added: Pinia state management, Vue Router, Axios
  - Auth: JWT → Laravel Sanctum (SPA + API tokens)
  - Added: Spatie Laravel Permission for RBAC
  
- Updated `LIBRARY_SPEC_TECH.md`:
  - Frontend stack: Vue.js 3, Vite, Pinia, Vue Router, Tailwind CSS
  - Auth: Laravel Breeze (Vue), Sanctum, Spatie Permission
  - Added: Spatie MediaLibrary (book covers), Activitylog (audit trail)
  - Development tools: Vue DevTools, ESLint, Prettier
  - Testing: Vitest, Vue Test Utils, Cypress/Playwright
  - Updated local dev setup (dual server: Laravel API + Vite frontend)

**3. Memory Updates**
- Updated `project_library_system.md` with new tech stack
- Added key packages section (Spatie packages, Sanctum, Vue ecosystem)
- Added development setup commands
- Updated API design (Sanctum authentication)

**4. Git Operations**
- Commit 1: `c52f625` - "Add library system specification documents" (10 files, 4,466 lines)
- Commit 2: `c840349` - "Update library system specs to Vue.js frontend"
- Pushed to GitHub: `origin/master`

### Technical Decisions
- **Vue.js 3 over Blade**: Modern SPA for better UX, component reusability
- **Sanctum over JWT**: Laravel-native, simpler SPA auth, cookie-based security
- **Spatie packages**: Industry standard for Laravel (Permission, MediaLibrary, Activitylog)
- **Vite over Webpack**: Faster HMR, better DX, Vue 3 optimized

### Files Modified
- `Project/library-system/LIBRARY_SPEC_ARCHITECTURE.md`
- `Project/library-system/LIBRARY_SPEC_TECH.md`
- `memory/project_ai_memorycore.md` (created)
- `memory/project_library_system.md` (updated)
- `memory/MEMORY.md` (updated)

### Outcome
Library system specs now aligned with modern Laravel + Vue.js SPA best practices. All specs follow laravel-workflow.md guidelines. Memory system tracks both AI MemoryCore project and Library System project.

---

📖 *Diary entry complete - session preserved*

---

## Session: 2026-06-22 - Project Memory Organization & Save Action

### Context
Amirul requested saving the project context for the new `laravel-vue-app` and existing `sport-app` in the central memory repository `Project-AI-MemoryCore`, keeping individual project repositories clean and organized.

### Actions Taken
1. **Centralized Project Context Storage**:
   - Created `Project/laravel-vue-app/PROJECT_CONTEXT.md` in `Project-AI-MemoryCore` containing:
     - Technical stack details (Laravel 13, Vue 3, Inertia, Tailwind, MySQL).
     - Database configuration for Laragon (`laravel_vue_app`).
     - Key debugging solutions: Vite build/Axios bootstrap fix and absolute overflow double scrollbars fix.
     - Routes description: `/` landing page, `/showcase` Vue sandbox, and `/ideas` database CRUD.
   - Created `Project/sport-app/PROJECT_CONTEXT.md` in `Project-AI-MemoryCore` detailing:
     - Financial booking delete constraints (`restrictOnDelete`).
     - Unpaid bookings expiration console scheduler command (`bookings:expire`).
     - Spatie permissions and role check DB column fallback.
     - Modernized customer UI refactoring (left-aligned cards and consistent spacing).

2. **Memory System Integration**:
   - Updated relationship logs and verified RAM context remains fully aligned.

### Technical Decisions
- **Central Memory Model**: Rather than leaving `.agents/AGENTS.md` configurations inside individual workspace repositories where they can get cluttered or easily deleted, storing contexts centrally inside the `Project-AI-MemoryCore` folder under `Project/<project-name>` keeps project roots tidy and allows a single master repository to track all work.

---

📖 *Diary entry complete - session preserved*

---

## Session: 2026-06-22 (Afternoon) - Spatie Media Library & Dynamic Vue Uploader

### Context
Amirul asked about a major feature possible in Vue that is difficult in Blade. Proposed a dynamic drag-and-drop uploader with real-time progress tracking, lightbox previews, and card cover renders, powered by Spatie Media Library.

### Actions Taken
1. **Spatie Media Library Backend Installation & Setup**:
   - Installed `spatie/laravel-medialibrary` package.
   - Published and migrated the `media` table migration.
   - Generated the storage symbolic link (`php artisan storage:link`).
   - Configured `Task.php` model to implement `HasMedia` and use `InteractsWithMedia` trait.
   - Configured automatic thumbnail conversions to fit 300x300 pixel crops dynamically on-the-fly (`fit()` and `nonQueued()`).

2. **Backend Controllers & Routes**:
   - Modified `TaskController.php` index to map task media attachment arrays (URLs, thumbnails, file names, and sizes).
   - Created `uploadMedia` controller action utilizing request file pointers to attach dropped images to tasks.
   - Created `deleteMedia` controller action to remove attachments from storage.
   - Registered `/tasks/{task}/media` (POST) and `/media/{media}` (DELETE) endpoints in `routes/web.php`.

3. **High-Fidelity Vue SPA Features**:
   - Upgraded `Tasks/Index.vue` with an interactive task details modal.
   - Implemented drop zone events (`dragover`, `dragleave`, `drop`) and file upload select inputs.
   - Designed a reactive upload queue using Axios `onUploadProgress` callbacks to render real-time progress bars for multiple files concurrently.
   - Added hover overlay cards for zoom-in lightboxes and trash removals.
   - Modified cards to display a visual cover preview using the task's first media thumbnail.
   - Integrated headers navigation in `Landing.vue`, `VueShowcase.vue`, and `Ideas/Index.vue`.

4. **Git Operations & Verification**:
   - Verified that Vite compiles assets perfectly (Vite build completed in 1.34s with zero errors).
   - Committed changes to the `laravel-vue-app` repository.

### Technical Decisions
- **Non-Queued Image Processing**: Configured Spatie media conversions with `nonQueued()` to run synchronously. This avoids queue configuration issues during local sandbox development.
- **Granular Axios Upload Handlers**: Leveraged window.axios directly rather than standard Inertia forms for multi-file drops to support concurrent progress monitoring for each file separately.

---

📖 *Diary entry complete - session preserved*

---

## Session: 2026-06-22 (Evening) - Real-Time Live Dashboard with Charts

### Context
Amirul requested adding a completely new and powerful Vue feature to `laravel-vue-app` that is entirely different from what was built previously (Kanban + Spatie Media Uploader) and showcases Vue's unique strengths over Blade.

### Planned Feature: `/analytics` — Live Dashboard with Real-Time Charts
- **Why it's impossible in Blade alone**: A live-refreshing dashboard with animated charts, reactive data polling, and real-time counter animations requires Vue's reactivity system and component lifecycle hooks (`onMounted`, `onUnmounted`, `setInterval`, `watch`). Blade is server-rendered and cannot update in the browser without a full page reload.
- **Tech used**: ApexCharts (via vue3-apexcharts), Vue `setInterval` polling, Laravel JSON API endpoints returning live DB stats.

### Actions Taken
1. Installed `vue3-apexcharts` and `apexcharts` via npm.
2. Created `/api/stats` Laravel JSON route returning live counts from DB tables (Tasks, Ideas, Media).
3. Built `Analytics/Index.vue` with:
   - Animated stat counter cards (reactive number transitions using `requestAnimationFrame`).
   - Live Area Chart showing task creation over time.
   - Live Donut Chart showing task status breakdown (todo/in-progress/done).
   - Live Bar Chart showing idea votes by title.
   - Auto-polling every 5 seconds via `setInterval` + `onUnmounted` cleanup.
   - Refresh indicator with spinner and last-updated timestamp.
4. Registered `/analytics` Inertia route and added nav link to all existing pages.
5. Committed to `laravel-vue-app` git repository.

### Technical Decisions
- **Polling over WebSockets**: Used 5-second interval polling for simplicity (avoids Pusher/Echo config). Demonstrates lifecycle hooks and cleanup patterns — a core Vue concept.
- **ApexCharts**: Best-in-class animated chart library with Vue 3 official integration.
- **Animated counters**: Custom `animateValue()` utility using `requestAnimationFrame` shows reactive number transitions impossible in Blade.

---

📖 *Diary entry complete - session preserved*

---

## Session: 2026-06-22 (Evening II) - Blade vs Vue Mental Model

### Context
Amirul asked for clarification on when to use Blade vs Vue, and whether both can coexist in a single Laravel project.

### Key Concepts Discussed

**Use Blade when:**
- Simple admin CRUD pages, forms, login/register pages.
- Static or server-rendered content with no client-side interactivity.
- Speed of development is priority over UX richness.
- Rule: If the page doesn't need to react without a full reload → Blade is perfect.

**Use Vue when:**
- Live dashboards, real-time polling, animated charts.
- Drag & drop interfaces, multi-step wizards, file uploaders with progress.
- Any UI that changes instantly without a page reload.
- Rule: If the user interaction should feel instant, alive, or stateful → Vue.

**Can both coexist in one Laravel project? YES — 3 setups:**
1. **Inertia.js (full Vue)**: All pages are `.vue` components. Blade is only the root shell (`app.blade.php`). Used in `laravel-vue-app`. Mixing Blade pages is awkward here.
2. **Hybrid (Blade + Vue components)**: Most pages are Blade, but specific pages mount a Vue component via `createApp().mount('#id')`. Very common and practical for real-world apps.
3. **Full API separation**: Laravel = JSON API, Vue = separate frontend (Nuxt/Vite). Overkill for most projects.

**Amirul's current project alignment:**
- `sport-app` → Blade (CRUD, bookings, admin) ✅ correct choice.
- `laravel-vue-app` → Inertia + Vue (learning SPA) ✅ correct choice.
- `library-system` → Could do Hybrid or full Inertia depending on complexity.

### Amirul's Understanding Level
- Now clearly understands the use-case boundary between Blade and Vue.
- Grasps that both can coexist; knows the 3 architectural patterns.
- Ready to make informed decisions when starting new Laravel projects.

---

📖 *Diary entry complete - session preserved*