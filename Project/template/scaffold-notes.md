# Project: `template` — Reusable Laravel Admin Starter

**Location:** `C:\laragon\www\template` · serves at `http://template.test`
**Created:** 2026-07-02

## Stack (as installed — latest, per standing rule)
- Laravel **13.18.0**, PHP **8.4.14**
- **Livewire 4.3.3** (starter kit: Livewire + Volt + **Flux** UI, built-in auth incl. 2FA + passkeys)
- **Tailwind v4** (`@tailwindcss/vite`) — tokens in `resources/css/app.css` `@theme`, NOT `tailwind.config.js`
- **PHPUnit**, **MySQL** (DB `template`, migrated), Vite 8
- Git initialised on `main` (no GitHub remote yet)

## Blue Executive Dashboard scaffold (2026-07-02)
Custom hand-rolled admin UI (no Flux dependency for the new views).

### Design tokens (`resources/css/app.css` `@theme` — authoritative)
- `primary` `#1E40AF` (`bg-primary`) · accent `brand-500` `#3B82F6`
- full `brand-50…950` blue ramp
- `canvas` `#F8FAFC` (page bg) · `sidebar` `#0F172A` (+`-hover` `#1E293B`, `-active` `#1D4ED8`) · `hairline` `#E2E8F0` (card border)
- `radius-card` 12px (`rounded-card`) · `radius-btn` 8px (`rounded-btn`)
- Font: **Inter** (loaded via bunny.net `<link>` in `partials/head.blade.php`)
- `tailwind.config.js` mirrors the palette and is wired via `@config '../../tailwind.config.js';` in app.css (belt-and-suspenders; `@theme` is what really drives it)

### Files
| File | Purpose |
|---|---|
| `resources/views/layouts/app.blade.php` | Full standalone shell: navy sidebar + icon nav (active=blue), sticky white header (search + notifications dropdown + avatar dropdown), loads Inter + Chart.js CDN. Guards `Route::has('logout')`. |
| `resources/views/dashboard.blade.php` | 4 KPI stat-cards (Users/Revenue/Orders/Growth) + Chart.js gradient line chart + dept-progress panel + recent-activity table. |
| `resources/views/users.blade.php` | Demo user-management table (so `users` route resolves). |
| `components/ui/stat-card.blade.php` | label/value/change/trend + `icon` slot; up/down colouring. |
| `components/ui/button.blade.php` | variants primary/secondary/ghost/danger; sizes sm/md/lg; renders `<a>` or `<button>`. |
| `components/ui/data-table.blade.php` | card-wrapped table; `title`, `:headers`, `actions` + `footer` slots. |
| `routes/web.php` | `dashboard`, `users` (+ existing `settings`). |

All demo data is real Malay/English (Nurul Aina, Tan Wei Ming, Kewangan/Operasi, RM values) — no lorem ipsum.

## Watch-outs / gotchas
- **Component namespace:** starter registers `layouts` as a component NAMESPACE → views MUST use `<x-layouts::app>` (double-colon). The dot form `<x-layouts.app>` fails: `ComponentTagCompiler::componentClass("layouts.app")`. Learned the hard way via `view:cache`.
- Replaced the Flux **placeholder** dashboard + layout shell. Auth + settings pages still render inside the new shell (Flux still installed for their form inputs) — verify settings pages look OK in-browser; reconcile if off.
- Tailwind v4: no `tailwind.config.js`-driven build by default; `@theme` is authoritative.
- Chart.js loaded via CDN in the layout `<head>`; dashboard init script guards `el.dataset.rendered` + `typeof Chart`.

## Verification done
`npm run build` ✅ · blue utilities present in compiled `app-*.css` ✅ · `php artisan view:cache` (compiles all Blade) ✅ · `route:list` shows dashboard/users/settings ✅
