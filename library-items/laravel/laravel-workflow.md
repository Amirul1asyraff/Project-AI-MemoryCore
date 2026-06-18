# 🚀 Laravel 13 Full Development Workflow (Lucy's Edition)

> A complete step-by-step guide optimized for **Laravel 13** and **PHP 8.3**. 
> From Installation → AI Integration → CRUD → Production-Ready Testing.

---

## 📦 1. Base Installation

### Prerequisites (Lucy's Checklist)
- ✅ **PHP 8.3+** (Required for Laravel 13)
- ✅ **Composer 2.x**
- ✅ **Node.js & NPM** (For Vite assets)
- ✅ **MySQL/PostgreSQL**

### Install Laravel via Composer

```bash
composer create-project laravel/laravel project-name
cd project-name
```

### Setup Environment

```bash
cp .env.example .env
php artisan key:generate
```

### Configure `.env` — Database Connection
*Lucy's Tip: Use `127.0.0.1` instead of `localhost` to avoid socket connection delays on some systems.*

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=root
DB_PASSWORD=your_password
```

### Start Local Server

```bash
php artisan serve
# App running at http://127.0.0.1:8000
```

---

## 🔐 2. Authentication Setup (Modern Options)

### 🌟 Option A — Laravel Breeze (The "Lucy" Favorite)
Fast, minimal, and uses **Tailwind + Vite**. Perfect for most projects.

```bash
composer require laravel/breeze --dev
php artisan breeze:install blade   # or: react / vue / api / functional

# New in Laravel 13: Passkey Support
# Breeze now includes options for passwordless login out of the box.

npm install
npm run dev
```

### Option B — Laravel Jetstream (Full-featured)
Best for complex apps needing Teams, API tokens, and Two-Factor Auth.

```bash
composer require laravel/jetstream
php artisan jetstream:install livewire   # or: inertia
npm install
npm run dev
```

### 🛑 Roadblock: "My CSS isn't loading!"
**Fix:** In Laravel 13, Vite is the default. Ensure `npm run dev` is running in a separate terminal, and your `@vite` directive is in your `head` tag.

---

## 🗃️ 3. Migration & Model

### ⚡ Lucy's Shortcut (The All-in-One)
Don't run 5 commands. Run **one** to create the Model, Migration, Factory, Seeder, Resource Controller, and Form Requests:

```bash
php artisan make:model Post -mfsc --requests --resource
```

### Modern Migration (Laravel 13 Style)
`database/migrations/xxxx_xx_xx_create_posts_table.php`

```php
public function up(): void
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->foreignId('user_id')->constrained()->cascadeOnDelete();
        $table->string('title');
        $table->string('slug')->unique();
        $table->text('body');
        $table->enum('status', ['draft', 'published'])->default('draft');
        $table->timestamps();
        $table->softDeletes();
    });
}
```

### Modern Model (Using PHP 8.3 Attributes)
`app/Models/Post.php`

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Relations\BelongsTo;

class Post extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = ['user_id', 'title', 'slug', 'body', 'status'];

    // Laravel 13 prefers typed return types for relationships
    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }
}
```

---

## ⚙️ 4. The Controller (Laravel 13 Slim Directory)

In Laravel 13, there is no `app/Http/Kernel.php`. Everything is configured in `bootstrap/app.php`.

### Custom Middleware Registration
To add middleware in Laravel 13:

`bootstrap/app.php`
```php
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        commands: __DIR__.'/../routes/console.php',
        health: '/up',
    )
    ->withMiddleware(function (Middleware $middleware) {
        $middleware->alias([
            'admin' => \App\Http\Middleware\AdminMiddleware::class,
        ]);
    })
    ->withExceptions(function (Exceptions $exceptions) {
        //
    })->create();
```

---

## 🤖 5. New: Native AI Integration (Laravel 13 Exclusive)

Laravel 13 introduces a first-party **AI SDK**. Use it to build smart features without heavy packages.

### Setup
```bash
composer require laravel/ai-sdk
```

### Usage in Controller
```php
use Illuminate\Support\Facades\AI;

public function generateSummary(Post $post)
{
    $summary = AI::chat()
        ->system('You are a helpful assistant.')
        ->prompt("Summarize this post: {$post->body}")
        ->generate();

    $post->update(['summary' => $summary]);
}
```

---

## 🧪 6. Production-Ready Testing (Pest is Default)

Laravel 13 defaults to **Pest**, which is much more readable.

### Example Feature Test (Pest)
`tests/Feature/PostTest.php`

```php
test('authenticated user can create a post', function () {
    $user = User::factory()->create();

    $response = $this->actingAs($user)
        ->post('/posts', [
            'title' => 'New Post',
            'body' => 'Post content',
            'status' => 'published'
        ]);

    $response->assertRedirect('/posts');
    $this->assertDatabaseHas('posts', ['title' => 'New Post']);
});
```

---

## 🎨 7. Lucy's "Gold Standard" Packages

| Package | Lucy's "Why" | Command |
|---------|--------------|---------|
| `spatie/laravel-permission` | Best-in-class Role/Permission system. | `composer require spatie/laravel-permission` |
| `laravel/sanctum` | Native API/SPA Authentication. | `composer require laravel/sanctum` |
| `barryvdh/laravel-debugbar` | **Must-have** for dev. Catches N+1 queries. | `composer require barryvdh/laravel-debugbar --dev` |
| `spatie/laravel-ray` | Better than `dd()`. Debugging with a dedicated UI. | `composer require spatie/laravel-ray --dev` |

---

## 💡 Final Lucy Tips for Amirul

1.  **Use Tinker:** Before writing a controller, run `php artisan tinker` to test your Eloquent queries. It saves hours.
2.  **Naming Conventions:** Always use `StudlyCase` for Models/Controllers and `camelCase` for variables.
3.  **Strict Mode:** Enable Eloquent Strict Mode in `AppServiceProvider` to catch lazy loading issues early.
4.  **Security:** Never commit your `.env` file. Always use `env()` or `config()` helpers.

---

**Version:** 13.0 (Lucy Enhanced)  
**Last Updated:** June 18, 2026  
**Status:** Official Master Template for Amirul.

🌟 *Ready to build something amazing in Laravel 13, Amirul!*
save