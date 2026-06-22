# 🧠 Project Context - laravel-vue-app
*Persistent workspace memory and technical references for the Laravel Vue SPA project*

## 🎯 Project Overview
* **Stack:** Laravel 13 + Vue 3 (Inertia.js) + Tailwind CSS + MySQL + Spatie Laravel MediaLibrary.
* **Database Connection:** MySQL (`DB_CONNECTION=mysql`), Host: `127.0.0.1`, Port: `3306`, Database: `laravel_vue_app`, User: `root`, Password: `""` (empty) in Laragon.
* **Vite Config:** Compiles Vue SPA assets. Runs hot-reloading dev server via `npm run dev` and builds production bundle via `npm run build`.

## ⚙️ Key Technical Solutions & Gotchas

### 1. Vite Build / Axios CSRF Initialization
Starter templates for Laravel 13 may omit the default Axios setup, leading to build-time or runtime import errors when making requests via Inertia or Axios.
* **Solution:** Create or update `resources/js/bootstrap.js` with the following Axios configuration to properly declare the standard CSRF and XMLHttp request headers:
  ```js
  import axios from 'axios';
  window.axios = axios;
  window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
  ```

### 2. Preventing Browser Double Scrollbars (Windows/Chrome/Edge)
Applying page-wide `overflow-x-hidden` or forcing scroll restrictions on the `html` or `body` selectors triggers secondary browser scrollbars. This is particularly noticeable on Windows where scrollbars take layout space.
* **Solution:** 
  1. Keep body/html overflow styles default (e.g. dynamic/auto).
  2. Clip absolute or floating background shapes/glares in a dedicated wrapper container:
     ```html
     <div class="absolute inset-0 overflow-hidden pointer-events-none z-0">
         <!-- Absolute decorative elements go here -->
     </div>
     ```
  3. Ensure the outer container elements do not have conflicting overflow styles.

### 3. Spatie MediaLibrary Configuration & Symbolic Link
* **Storage Symlink:** To make media attachments publicly accessible in the browser, a symbolic link must be created from `public/storage` to `storage/app/public` via:
  ```bash
  php artisan storage:link
  ```
* **Non-Queued Media Conversions:** To avoid waiting for background queue workers in local environments (like Laragon), media conversions are registered to run immediately on-the-fly (`nonQueued()`):
  ```php
  public function registerMediaConversions(Media $media = null): void
  {
      $this->addMediaConversion('thumb')
          ->width(300)
          ->height(300)
          ->fit(\Spatie\Image\Enums\Fit::Crop, 300, 300)
          ->nonQueued();
  }
  ```

### 4. Vue Real-time Upload Progress Bar (Axios)
* **Axios custom handler:** Direct file uploads using Inertia can sometimes lack individual file progress details. Using window.axios.post with `onUploadProgress` allows tracking individual file percentages dynamically:
  ```js
  window.axios.post(route('tasks.media.store', task_id), formData, {
      onUploadProgress: (progressEvent) => {
          const percent = Math.round((progressEvent.loaded * 100) / progressEvent.total);
          queueItem.progress = percent;
      }
  })
  ```

## 🗺️ Registered Routes & Pages
* **Landing Page (`/`):** Routes to `Landing.vue` (Premium modern dark mode showcase).
* **Vue Feature Showcase (`/showcase`):** Routes to `VueShowcase.vue` (Local sandbox widgets).
* **Ideas Board (`/ideas`):** Routes to `Ideas/Index.vue` (Simple voting board, MySQL CRUD).
* **Kanban Task & Media Board (`/tasks`):** Routes to `Tasks/Index.vue`. Integrates:
  * **Model:** `app/Models/Task.php` (implements `HasMedia`, uses `InteractsWithMedia`).
  * **Controllers:** `app/Http/Controllers/TaskController.php` with:
    * `index`: Maps tasks and returns associated Spatie media attachments (original URL & thumbnail conversion URL).
    * `store`: Creates new Kanban cards.
    * `update`: Shifts cards between statuses (`todo`, `in-progress`, `done`).
    * `uploadMedia`: Handles Axios file drops and attaches them to task media collection.
    * `deleteMedia`: Removes media attachments by ID.
    * `destroy`: Deletes tasks and automatically wipes all associated Spatie media storage folders.
  * **View features:** Modal overlay, drag-and-drop file uploader zone, upload queue progress bar animations, card image cover rendering, zoom lightbox views, custom warning confirmation modal for delete actions, and dynamic corner toast alert feedback.
* **Live Analytics Dashboard (`/analytics`):** Routes to `Analytics/Index.vue`. Powered by:
  * **Library:** `vue3-apexcharts` + `apexcharts` (npm installed).
  * **Controller:** `app/Http/Controllers/AnalyticsController.php` with:
    * `index`: Returns Inertia view.
    * `stats`: JSON endpoint — returns live DB stats (task counts, task status breakdown, 7-day task creation trend, top ideas by upvotes, activity feed).
  * **Vue Concepts Showcased:**
    * `onMounted` / `onUnmounted` lifecycle hooks for safe setInterval polling.
    * `setInterval` polling every 5 seconds with automatic cleanup on unmount.
    * `requestAnimationFrame` animated counter transitions (`easeOutCubic`).
    * `computed` reactive chart series/options that update automatically when data changes.
    * `reactive` objects for granular state tracking (totals, taskStatus, display).
  * **Charts:** Area chart (7-day task trend), Donut chart (task status), Bar chart (top ideas by votes).
  * **Other features:** Loading skeleton, live activity feed, "why this is impossible in Blade" explainer panel, responsive nav with active state highlighting.

## 🧪 Testing Status
* Successfully verified Breeze auth and Inertia navigation tests.
* Verified that Spatie MediaLibrary generates thumbnails and registers uploads locally without errors.
* Vite build compiles all pages (including ApexCharts analytics) in ~1.5s with zero errors.
