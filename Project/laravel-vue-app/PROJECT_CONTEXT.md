# 🧠 Project Context - laravel-vue-app
*Persistent workspace memory and technical references for the Laravel Vue SPA project*

## 🎯 Project Overview
* **Stack:** Laravel 13 + Vue 3 (Inertia.js) + Tailwind CSS + MySQL.
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

## 🗺️ Registered Routes & Pages
* **Landing Page (`/`):** Routes to `Landing.vue` (Premium modern dark mode showcase).
* **Vue Feature Showcase (`/showcase`):** Routes to `VueShowcase.vue`. Demonstrates:
  * Client-side stateful Kanban board (drag-and-drop state).
  * Monthly budget interactive slider.
  * Real-time notes markdown parser.
* **Ideas Board (`/ideas`):** Routes to `Ideas/Index.vue`. Connected to SQLite/MySQL database via:
  * **Model:** `app/Models/Idea.php`
  * **Migration:** `database/migrations/2026_06_22_042825_create_ideas_table.php` (fields: `id`, `title`, `description`, `votes`, `created_at`, `updated_at`)
  * **Controller:** `app/Http/Controllers/IdeaController.php`
  * **Functionality:** Create new ideas, upvote/downvote ideas, and delete ideas reactively through Inertia post/put/delete requests.

## 🧪 Testing Status
* Successfully ran Laravel Breeze authentication and routing tests. All 25 feature/unit tests passed against the local MySQL database.
