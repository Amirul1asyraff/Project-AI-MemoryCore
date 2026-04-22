# 🚀 Laravel Full Development Workflow

> A complete step-by-step guide: Installation → Auth → Migration → Seeder/Factory → CRUD → Unit Test

---

## 📦 1. Base Installation

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

## 🔐 2. Authentication Setup

### Option A — Laravel UI (Bootstrap Auth)

```bash
composer require laravel/ui

# Generate Bootstrap scaffold with Auth
php artisan ui bootstrap --auth

# Install & compile frontend assets
npm install
npm run dev
```

> **Note:** `npm run dev` compiles assets once. Use `npm run watch` for live reloading during development.

### Option B — Laravel Breeze (Recommended for Tailwind)

```bash
composer require laravel/breeze --dev
php artisan breeze:install blade   # or: react / vue / api

npm install
npm run dev
```

### Option C — Laravel Jetstream (Full-featured)

```bash
composer require laravel/jetstream
php artisan jetstream:install livewire   # or: inertia

npm install
npm run dev
```

### Run Auth Migrations

```bash
php artisan migrate
```

> This creates: `users`, `password_reset_tokens`, `sessions`, `cache`, `jobs` tables.

---

## 🗃️ 3. Migration & Model

### Create Model + Migration Together

```bash
php artisan make:model Post -m
# -m = generate migration file at the same time
```

### Edit Migration File

`database/migrations/xxxx_xx_xx_create_posts_table.php`

```php
public function up(): void
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->foreignId('user_id')->constrained()->onDelete('cascade');
        $table->string('title');
        $table->string('slug')->unique();
        $table->text('body');
        $table->enum('status', ['draft', 'published'])->default('draft');
        $table->timestamps();
        $table->softDeletes(); // optional: for soft delete
    });
}
```

### Edit Model File

`app/Models/Post.php`

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;
use Illuminate\Database\Eloquent\Factories\HasFactory;

class Post extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'user_id',
        'title',
        'slug',
        'body',
        'status',
    ];

    // Relationship: Post belongs to User
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

### Run Migration

```bash
php artisan migrate

# Rollback if needed
php artisan migrate:rollback

# Fresh migration (drops all tables)
php artisan migrate:fresh
```

---

## 🌱 4. Seeder & Factory

### Create Factory

```bash
php artisan make:factory PostFactory --model=Post
```

`database/factories/PostFactory.php`

```php
<?php

namespace Database\Factories;

use App\Models\User;
use Illuminate\Database\Eloquent\Factories\Factory;
use Illuminate\Support\Str;

class PostFactory extends Factory
{
    public function definition(): array
    {
        $title = $this->faker->sentence(6);

        return [
            'user_id'  => User::factory(),
            'title'    => $title,
            'slug'     => Str::slug($title),
            'body'     => $this->faker->paragraphs(3, true),
            'status'   => $this->faker->randomElement(['draft', 'published']),
        ];
    }
}
```

### Create Seeder

```bash
php artisan make:seeder PostSeeder
```

`database/seeders/PostSeeder.php`

```php
<?php

namespace Database\Seeders;

use App\Models\Post;
use App\Models\User;
use Illuminate\Database\Seeder;

class PostSeeder extends Seeder
{
    public function run(): void
    {
        // Create 5 users, each with 3 posts
        User::factory(5)->create()->each(function ($user) {
            Post::factory(3)->create(['user_id' => $user->id]);
        });
    }
}
```

### Register in DatabaseSeeder

`database/seeders/DatabaseSeeder.php`

```php
public function run(): void
{
    $this->call([
        PostSeeder::class,
    ]);
}
```

### Run Seeder

```bash
# Run all seeders
php artisan db:seed

# Run specific seeder
php artisan db:seed --class=PostSeeder

# Fresh migrate + seed (useful in development)
php artisan migrate:fresh --seed
```

---

## ⚙️ 5. CRUD (Full Resource)

### Create Controller & Request

```bash
php artisan make:controller PostController --resource --model=Post
php artisan make:request StorePostRequest
php artisan make:request UpdatePostRequest
```

### Form Request Validation

`app/Http/Requests/StorePostRequest.php`

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'title'  => ['required', 'string', 'max:255'],
            'body'   => ['required', 'string'],
            'status' => ['required', 'in:draft,published'],
        ];
    }
}
```

### Resource Controller

`app/Http/Controllers/PostController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Models\Post;
use Illuminate\Support\Str;
use App\Http\Requests\StorePostRequest;
use App\Http\Requests\UpdatePostRequest;

class PostController extends Controller
{
    public function index() { ... }
    public function create() { ... }
    public function store(StorePostRequest $request) { ... }
    public function show(Post $post) { ... }
    public function edit(Post $post) { ... }
    public function update(UpdatePostRequest $request, Post $post) { ... }
    public function destroy(Post $post) { ... }
}
```

---

#### 📄 `index()` — List All Posts
**Route:** `GET /posts`

```php
public function index()
{
    $posts = Post::with('user')->latest()->paginate(10);
    return view('posts.index', compact('posts'));
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can view the posts list → `assertStatus(200)` + `assertViewIs('posts.index')`
> - ✅ Guest is redirected to login → `assertRedirect(route('login'))`
> - ✅ Paginated posts are passed to the view → `assertViewHas('posts')`

---

#### ➕ `create()` — Show Create Form
**Route:** `GET /posts/create`

```php
public function create()
{
    return view('posts.create');
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can access the create form → `assertStatus(200)` + `assertViewIs('posts.create')`
> - ✅ Guest is redirected to login → `assertRedirect(route('login'))`

---

#### 💾 `store()` — Save New Post
**Route:** `POST /posts`

```php
public function store(StorePostRequest $request)
{
    Post::create([
        'user_id' => auth()->id(),
        'title'   => $request->title,
        'slug'    => Str::slug($request->title),
        'body'    => $request->body,
        'status'  => $request->status,
    ]);

    return redirect()->route('posts.index')
                     ->with('success', 'Post created successfully.');
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can create a post → `assertDatabaseHas('posts', [...])`
> - ✅ Redirects to index after successful store → `assertRedirect(route('posts.index'))`
> - ✅ Validation fails when fields are empty → `assertSessionHasErrors(['title', 'body', 'status'])`
> - ✅ Validation fails when `status` is not `draft` or `published` → `assertSessionHasErrors(['status'])`
> - ✅ Guest cannot store a post → `assertRedirect(route('login'))`

---

#### 👁️ `show()` — View Single Post
**Route:** `GET /posts/{post}`

```php
public function show(Post $post)
{
    return view('posts.show', compact('post'));
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can view a single post → `assertStatus(200)` + `assertViewIs('posts.show')`
> - ✅ Post data is passed to the view → `assertViewHas('post')`
> - ✅ Returns 404 for a non-existent post → `assertStatus(404)`
> - ✅ Guest is redirected to login → `assertRedirect(route('login'))`

---

#### ✏️ `edit()` — Show Edit Form
**Route:** `GET /posts/{post}/edit`

```php
public function edit(Post $post)
{
    return view('posts.edit', compact('post'));
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can access the edit form → `assertStatus(200)` + `assertViewIs('posts.edit')`
> - ✅ Correct post data is passed to the view → `assertViewHas('post')`
> - ✅ Guest is redirected to login → `assertRedirect(route('login'))`

---

#### 🔄 `update()` — Save Edited Post
**Route:** `PUT /posts/{post}`

```php
public function update(UpdatePostRequest $request, Post $post)
{
    $post->update([
        'title'  => $request->title,
        'slug'   => Str::slug($request->title),
        'body'   => $request->body,
        'status' => $request->status,
    ]);

    return redirect()->route('posts.index')
                     ->with('success', 'Post updated successfully.');
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can update a post → `assertDatabaseHas('posts', ['title' => 'Updated Title'])`
> - ✅ Redirects to index after successful update → `assertRedirect(route('posts.index'))`
> - ✅ Validation fails when required fields are missing → `assertSessionHasErrors([...])`
> - ✅ Guest cannot update a post → `assertRedirect(route('login'))`

---

#### 🗑️ `destroy()` — Delete Post
**Route:** `DELETE /posts/{post}`

```php
public function destroy(Post $post)
{
    $post->delete();

    return redirect()->route('posts.index')
                     ->with('success', 'Post deleted successfully.');
}
```

> 🧪 **Unit Test Required:**
> - ✅ Authenticated user can delete a post → `assertSoftDeleted('posts', ['id' => $post->id])`
> - ✅ Redirects to index after deletion → `assertRedirect(route('posts.index'))`
> - ✅ Deleted post no longer appears in the list → `assertDatabaseMissing` or soft delete check
> - ✅ Guest cannot delete a post → `assertRedirect(route('login'))`

---

### Register Routes

`routes/web.php`

```php
use App\Http\Controllers\PostController;

Route::middleware(['auth'])->group(function () {
    Route::resource('posts', PostController::class);
});
```

### Check Registered Routes

```bash
php artisan route:list
```

---

## 🧪 6. Unit Test

### Create Test

```bash
# Feature test (HTTP / integration)
php artisan make:test PostTest

# Unit test (pure class logic)
php artisan make:test PostUnitTest --unit
```

### Feature Test Example

`tests/Feature/PostTest.php`

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use App\Models\Post;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;

class PostTest extends TestCase
{
    use RefreshDatabase;

    private User $user;

    protected function setUp(): void
    {
        parent::setUp();
        $this->user = User::factory()->create();
    }

    /** @test */
    public function authenticated_user_can_view_posts_index(): void
    {
        $this->actingAs($this->user)
             ->get(route('posts.index'))
             ->assertStatus(200)
             ->assertViewIs('posts.index');
    }

    /** @test */
    public function authenticated_user_can_create_a_post(): void
    {
        $postData = [
            'title'  => 'My First Post',
            'body'   => 'This is the post body content.',
            'status' => 'published',
        ];

        $this->actingAs($this->user)
             ->post(route('posts.store'), $postData)
             ->assertRedirect(route('posts.index'));

        $this->assertDatabaseHas('posts', [
            'title'   => 'My First Post',
            'user_id' => $this->user->id,
        ]);
    }

    /** @test */
    public function authenticated_user_can_update_a_post(): void
    {
        $post = Post::factory()->create(['user_id' => $this->user->id]);

        $this->actingAs($this->user)
             ->put(route('posts.update', $post), [
                 'title'  => 'Updated Title',
                 'body'   => 'Updated body content.',
                 'status' => 'draft',
             ])
             ->assertRedirect(route('posts.index'));

        $this->assertDatabaseHas('posts', ['title' => 'Updated Title']);
    }

    /** @test */
    public function authenticated_user_can_delete_a_post(): void
    {
        $post = Post::factory()->create(['user_id' => $this->user->id]);

        $this->actingAs($this->user)
             ->delete(route('posts.destroy', $post))
             ->assertRedirect(route('posts.index'));

        $this->assertSoftDeleted('posts', ['id' => $post->id]);
        // or: $this->assertDatabaseMissing('posts', ['id' => $post->id]);
    }

    /** @test */
    public function guest_cannot_access_posts(): void
    {
        $this->get(route('posts.index'))
             ->assertRedirect(route('login'));
    }

    /** @test */
    public function post_requires_title_and_body(): void
    {
        $this->actingAs($this->user)
             ->post(route('posts.store'), [])
             ->assertSessionHasErrors(['title', 'body', 'status']);
    }
}
```

### Unit Test Example

`tests/Unit/PostUnitTest.php`

```php
<?php

namespace Tests\Unit;

use Tests\TestCase;
use App\Models\Post;
use App\Models\User;
use Illuminate\Foundation\Testing\RefreshDatabase;

class PostUnitTest extends TestCase
{
    use RefreshDatabase;

    /** @test */
    public function post_belongs_to_a_user(): void
    {
        $post = Post::factory()->create();
        $this->assertInstanceOf(User::class, $post->user);
    }

    /** @test */
    public function post_has_correct_fillable_fields(): void
    {
        $post = new Post();
        $this->assertEquals(
            ['user_id', 'title', 'slug', 'body', 'status'],
            $post->getFillable()
        );
    }
}
```

### Run Tests

```bash
# Run all tests
php artisan test

# Run specific test file
php artisan test tests/Feature/PostTest.php

# Run specific test method
php artisan test --filter=authenticated_user_can_create_a_post

# Run with coverage report (requires Xdebug or PCOV)
php artisan test --coverage
```

---

## 🎨 7. Recommended Packages

> Essential packages for production Laravel apps — Spatie, API auth, developer tooling, and user management.

---

### 📦 Packages Overview

| Package | Purpose |
|---------|---------|
| `spatie/laravel-permission` | Roles & permissions per user |
| `spatie/laravel-medialibrary` | File & image uploads linked to models |
| `spatie/laravel-activitylog` | Auto-logs model create/update/delete |
| `laravel/sanctum` | API token auth & SPA session auth |
| `lab404/laravel-impersonate` | Admin login-as-user impersonation |
| `barryvdh/laravel-debugbar` | In-browser debug toolbar (dev only) |

---

### 🛡️ 7.1 — Roles & Permissions (`laravel-permission`)

#### Install

```bash
composer require spatie/laravel-permission
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
php artisan migrate
```

#### Add Trait to User Model

`app/Models/User.php`

```php
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;
}
```

#### Seed Roles & Permissions

```php
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

// Create permissions
Permission::create(['name' => 'create post']);
Permission::create(['name' => 'edit post']);
Permission::create(['name' => 'delete post']);

// Create roles and assign permissions
$admin  = Role::create(['name' => 'admin']);
$editor = Role::create(['name' => 'editor']);

$admin->givePermissionTo(Permission::all());
$editor->givePermissionTo(['create post', 'edit post']);

// Assign role to user
$user->assignRole('admin');
```

#### Usage in Controller & Blade

```php
// Controller
if (!auth()->user()->can('edit post')) {
    abort(403);
}

// Or use middleware on routes
Route::resource('posts', PostController::class)
     ->middleware(['auth', 'role:admin|editor']);
```

```blade
{{-- Blade --}}
@role('admin')
    <a href="{{ route('posts.destroy', $post) }}">Delete</a>
@endrole

@can('edit post')
    <a href="{{ route('posts.edit', $post) }}">Edit</a>
@endcan
```

> 🧪 **Unit Test Required:**
> - ✅ Admin can access restricted route → `assertStatus(200)`
> - ✅ User without role is forbidden → `assertStatus(403)`
> - ✅ User with `edit post` permission can update → `assertRedirect(route('posts.index'))`
> - ✅ User without permission cannot delete → `assertStatus(403)`
> - ✅ Role is correctly assigned to user → `assertTrue($user->hasRole('admin'))`
> - ✅ Permission is correctly assigned to role → `assertTrue($user->can('create post'))`

```php
/** @test */
public function admin_can_delete_post(): void
{
    $admin = User::factory()->create();
    $admin->assignRole('admin');
    $post = Post::factory()->create();

    $this->actingAs($admin)
         ->delete(route('posts.destroy', $post))
         ->assertRedirect(route('posts.index'));
}

/** @test */
public function editor_cannot_delete_post(): void
{
    $editor = User::factory()->create();
    $editor->assignRole('editor');
    $post = Post::factory()->create();

    $this->actingAs($editor)
         ->delete(route('posts.destroy', $post))
         ->assertStatus(403);
}
```

---

### 🖼️ 7.2 — Media Library (`laravel-medialibrary`)

#### Install

```bash
composer require spatie/laravel-medialibrary
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="medialibrary-migrations"
php artisan migrate
```

#### Add Interface & Trait to Model

`app/Models/Post.php`

```php
use Spatie\MediaLibrary\HasMedia;
use Spatie\MediaLibrary\InteractsWithMedia;
use Spatie\MediaLibrary\MediaCollections\Models\Media;

class Post extends Model implements HasMedia
{
    use InteractsWithMedia;

    // Define media collections
    public function registerMediaCollections(): void
    {
        $this->addMediaCollection('thumbnail')
             ->singleFile(); // only 1 image per post

        $this->addMediaCollection('gallery');
    }

    // Define conversions (auto-resize)
    public function registerMediaConversions(?Media $media = null): void
    {
        $this->addMediaConversion('thumb')
             ->width(300)
             ->height(300);

        $this->addMediaConversion('preview')
             ->width(800);
    }
}
```

#### Usage in Controller

```php
// Upload single thumbnail
public function store(StorePostRequest $request)
{
    $post = Post::create([...]);

    if ($request->hasFile('thumbnail')) {
        $post->addMediaFromRequest('thumbnail')
             ->toMediaCollection('thumbnail');
    }

    // Upload multiple gallery images
    if ($request->hasFile('gallery')) {
        foreach ($request->file('gallery') as $image) {
            $post->addMedia($image)->toMediaCollection('gallery');
        }
    }
}
```

```blade
{{-- Blade --}}
<img src="{{ $post->getFirstMediaUrl('thumbnail', 'thumb') }}" />

@foreach ($post->getMedia('gallery') as $image)
    <img src="{{ $image->getUrl('preview') }}" />
@endforeach
```

> 🧪 **Unit Test Required:**
> - ✅ Post can have a thumbnail uploaded → `assertCount(1, $post->getMedia('thumbnail'))`
> - ✅ Post can have multiple gallery images → `assertCount(3, $post->getMedia('gallery'))`
> - ✅ Thumbnail collection only keeps one file (singleFile) → upload twice, assert count is still 1
> - ✅ Media is deleted when post is deleted → `assertDatabaseMissing('media', [...])`

```php
use Illuminate\Http\UploadedFile;
use Illuminate\Support\Facades\Storage;

/** @test */
public function post_can_have_thumbnail_uploaded(): void
{
    Storage::fake('public');

    $post = Post::factory()->create();
    $file = UploadedFile::fake()->image('thumbnail.jpg');

    $post->addMedia($file)->toMediaCollection('thumbnail');

    $this->assertCount(1, $post->getMedia('thumbnail'));
}
```

---

### 📋 7.3 — Activity Log (`laravel-activitylog`)

#### Install

```bash
composer require spatie/laravel-activitylog
php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="activitylog-migrations"
php artisan vendor:publish --provider="Spatie\Activitylog\ActivitylogServiceProvider" --tag="activitylog-config"
php artisan migrate
```

#### Add Trait to Model

`app/Models/Post.php`

```php
use Spatie\Activitylog\Traits\LogsActivity;
use Spatie\Activitylog\LogOptions;

class Post extends Model
{
    use LogsActivity;

    public function getActivitylogOptions(): LogOptions
    {
        return LogOptions::defaults()
            ->logOnly(['title', 'status'])       // only log these fields
            ->logOnlyDirty()                      // only log changed fields
            ->dontSubmitEmptyLogs()               // skip if nothing changed
            ->setDescriptionForEvent(fn(string $eventName) => "Post {$eventName}");
    }
}
```

#### Manual Logging

```php
// Log a custom activity
activity()
    ->causedBy(auth()->user())
    ->performedOn($post)
    ->withProperties(['custom_key' => 'value'])
    ->log('Post was published manually');

// Retrieve logs
$logs = Activity::all();
$postLogs = activity()->forSubject($post)->get();
$userLogs = activity()->causedBy($user)->get();
```

#### Usage in Blade / Controller

```php
// Controller: show log history for a post
public function history(Post $post)
{
    $activities = $post->activities()->latest()->get();
    return view('posts.history', compact('activities'));
}
```

```blade
@foreach ($activities as $log)
    <p>
        <strong>{{ $log->causer->name }}</strong>
        {{ $log->description }} —
        <small>{{ $log->created_at->diffForHumans() }}</small>
    </p>
@endforeach
```

> 🧪 **Unit Test Required:**
> - ✅ Activity is logged when a post is created → `assertDatabaseHas('activity_log', ['description' => 'Post created'])`
> - ✅ Activity is logged when a post is updated → assert log exists with `event = 'updated'`
> - ✅ Activity is logged when a post is deleted → assert log exists with `event = 'deleted'`
> - ✅ Only dirty (changed) fields are logged → update one field, assert only that field appears in `properties`
> - ✅ Causer is correctly stored → `assertEquals($user->id, $activity->causer_id)`

```php
/** @test */
public function activity_is_logged_when_post_is_created(): void
{
    $post = Post::factory()->create(['title' => 'Hello World']);

    $this->assertDatabaseHas('activity_log', [
        'subject_type' => Post::class,
        'subject_id'   => $post->id,
        'event'        => 'created',
    ]);
}

/** @test */
public function activity_is_logged_when_post_is_updated(): void
{
    $post = Post::factory()->create();
    $post->update(['title' => 'New Title']);

    $log = Activity::where('subject_id', $post->id)
                   ->where('event', 'updated')
                   ->first();

    $this->assertNotNull($log);
    $this->assertArrayHasKey('title', $log->properties['attributes']);
}
```

---

### 📦 Quick Install Reference

```bash
# Spatie — Roles & Permissions
composer require spatie/laravel-permission

# Spatie — Media Library
composer require spatie/laravel-medialibrary

# Spatie — Activity Log
composer require spatie/laravel-activitylog

# API Auth
composer require laravel/sanctum

# Impersonate
composer require lab404/laravel-impersonate

# Debug Bar (dev only)
composer require barryvdh/laravel-debugbar --dev
```

---

## 🔑 7.4 — Laravel Sanctum (API Authentication)

> Sanctum provides lightweight API token auth and cookie-based SPA authentication. Use it when building a REST API or a decoupled frontend (Vue, React, Next.js).

### Install

```bash
composer require laravel/sanctum
php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
php artisan migrate
```

### Add Trait to User Model

`app/Models/User.php`

```php
use Laravel\Sanctum\HasApiTokens;

class User extends Authenticatable
{
    use HasApiTokens, HasFactory, Notifiable;
}
```

### Configure API Middleware

`app/Http/Kernel.php` — inside `api` middleware group:

```php
'api' => [
    \Laravel\Sanctum\Http\Middleware\EnsureFrontendRequestsAreStateful::class,
    'throttle:api',
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
],
```

### Auth Routes (API)

`routes/api.php`

```php
use App\Http\Controllers\Api\AuthController;
use App\Http\Controllers\Api\PostController;

// Public routes
Route::post('/register', [AuthController::class, 'register']);
Route::post('/login',    [AuthController::class, 'login']);

// Protected routes
Route::middleware('auth:sanctum')->group(function () {
    Route::post('/logout', [AuthController::class, 'logout']);
    Route::apiResource('posts', PostController::class);
});
```

### AuthController (API)

`app/Http/Controllers/Api/AuthController.php`

```php
<?php

namespace App\Http\Controllers\Api;

use App\Models\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\Hash;
use Illuminate\Validation\ValidationException;

class AuthController extends Controller
{
    public function register(Request $request)
    {
        $data = $request->validate([
            'name'     => ['required', 'string', 'max:255'],
            'email'    => ['required', 'email', 'unique:users'],
            'password' => ['required', 'min:8', 'confirmed'],
        ]);

        $user  = User::create([...$data, 'password' => Hash::make($data['password'])]);
        $token = $user->createToken('api-token')->plainTextToken;

        return response()->json(['token' => $token, 'user' => $user], 201);
    }

    public function login(Request $request)
    {
        $request->validate([
            'email'    => ['required', 'email'],
            'password' => ['required'],
        ]);

        $user = User::where('email', $request->email)->first();

        if (!$user || !Hash::check($request->password, $user->password)) {
            throw ValidationException::withMessages([
                'email' => ['The provided credentials are incorrect.'],
            ]);
        }

        $token = $user->createToken('api-token')->plainTextToken;

        return response()->json(['token' => $token, 'user' => $user]);
    }

    public function logout(Request $request)
    {
        $request->user()->currentAccessToken()->delete();
        return response()->json(['message' => 'Logged out successfully.']);
    }
}
```

### Calling the API

```bash
# Register
POST /api/register
Content-Type: application/json
{ "name": "Ali", "email": "ali@example.com", "password": "secret123", "password_confirmation": "secret123" }

# Login — returns token
POST /api/login
{ "email": "ali@example.com", "password": "secret123" }

# Authenticated request
GET /api/posts
Authorization: Bearer {your-token-here}
```

> 🧪 **Unit Test Required:**
> - ✅ User can register and receives a token → `assertJsonStructure(['token', 'user'])`
> - ✅ User can login with correct credentials → `assertStatus(200)` + token in response
> - ✅ Login fails with wrong password → `assertStatus(422)`
> - ✅ Authenticated user can access protected routes → pass `Bearer` token, `assertStatus(200)`
> - ✅ Unauthenticated request is rejected → `assertStatus(401)`
> - ✅ User can logout and token is invalidated → logout, retry request, `assertStatus(401)`

```php
/** @test */
public function user_can_login_and_receive_token(): void
{
    $user = User::factory()->create(['password' => bcrypt('secret123')]);

    $this->postJson('/api/login', [
             'email'    => $user->email,
             'password' => 'secret123',
         ])
         ->assertStatus(200)
         ->assertJsonStructure(['token', 'user']);
}

/** @test */
public function unauthenticated_request_is_rejected(): void
{
    $this->getJson('/api/posts')->assertStatus(401);
}

/** @test */
public function authenticated_user_can_access_api(): void
{
    $user  = User::factory()->create();
    $token = $user->createToken('test')->plainTextToken;

    $this->withHeader('Authorization', "Bearer {$token}")
         ->getJson('/api/posts')
         ->assertStatus(200);
}
```

---

## 👤 7.5 — Laravel Impersonate (`lab404/laravel-impersonate`)

> Lets admins log in as any user without knowing their password. Extremely useful for debugging user-specific issues in production.

### Install

```bash
composer require lab404/laravel-impersonate
php artisan vendor:publish --tag=impersonate
```

### Add Trait to User Model

`app/Models/User.php`

```php
use Lab404\Impersonate\Models\Impersonate;

class User extends Authenticatable
{
    use Impersonate;

    // Define who CAN impersonate (only admins)
    public function canImpersonate(): bool
    {
        return $this->hasRole('admin');
    }

    // Define who CANNOT be impersonated (protect other admins)
    public function canBeImpersonated(): bool
    {
        return !$this->hasRole('admin');
    }
}
```

### Register Routes

`routes/web.php`

```php
use Lab404\Impersonate\Controllers\ImpersonateController;

Route::impersonate(); // registers take & leave routes automatically
```

This generates:
- `GET /impersonate/take/{id}` — start impersonating
- `GET /impersonate/leave` — stop impersonating

### Usage in Blade

```blade
@if (auth()->user()->canImpersonate())
    @foreach ($users as $user)
        @if ($user->canBeImpersonated())
            <a href="{{ route('impersonate', $user->id) }}">
                Login as {{ $user->name }}
            </a>
        @endif
    @endforeach
@endif

@impersonating
    <div class="alert alert-warning">
        You are impersonating <strong>{{ auth()->user()->name }}</strong>.
        <a href="{{ route('impersonate.leave') }}">Leave</a>
    </div>
@endImpersonating
```

### Usage in Controller

```php
$manager = app('impersonate');

// Start impersonating
$manager->take(auth()->user(), $targetUser);

// Stop impersonating
$manager->leave();

// Check if currently impersonating
$manager->isImpersonating();
```

> 🧪 **Unit Test Required:**
> - ✅ Admin can impersonate a regular user → `assertAuthenticatedAs($targetUser)`
> - ✅ Non-admin cannot impersonate → `assertStatus(403)` or redirect
> - ✅ Admin cannot impersonate another admin → assert `canBeImpersonated()` returns false
> - ✅ Impersonation can be stopped (leave) → after leave, assert original admin is authenticated
> - ✅ Impersonation is logged in activity log (if using `laravel-activitylog`)

```php
/** @test */
public function admin_can_impersonate_regular_user(): void
{
    $admin  = User::factory()->create();
    $admin->assignRole('admin');

    $target = User::factory()->create();
    $target->assignRole('editor');

    $this->actingAs($admin)
         ->get(route('impersonate', $target->id))
         ->assertRedirect();

    $this->assertAuthenticatedAs($target);
}

/** @test */
public function non_admin_cannot_impersonate(): void
{
    $editor = User::factory()->create();
    $editor->assignRole('editor');

    $target = User::factory()->create();

    $this->actingAs($editor)
         ->get(route('impersonate', $target->id))
         ->assertForbidden();
}
```

---

## 🐛 7.6 — Laravel Debugbar (`barryvdh/laravel-debugbar`)

> Adds an in-browser debug toolbar showing queries, routes, views, memory, timing, logs, and more. **Dev environment only — never enable in production.**

### Install

```bash
composer require barryvdh/laravel-debugbar --dev
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```

### Configuration

`.env`

```env
APP_DEBUG=true        # Debugbar only shows when this is true
DEBUGBAR_ENABLED=true # Explicit toggle
```

`config/debugbar.php` — key options:

```php
'enabled'    => env('DEBUGBAR_ENABLED', false),
'collectors' => [
    'db'        => true,  // SQL queries + bindings
    'views'     => true,  // Blade views rendered
    'route'     => true,  // Current route info
    'log'       => true,  // Laravel log entries
    'memory'    => true,  // Memory usage
    'time'      => true,  // Request timeline
    'exceptions'=> true,  // Exceptions
    'auth'      => true,  // Logged-in user info
],
```

### What Debugbar Shows

| Tab | What You See |
|-----|-------------|
| **Messages** | `Debugbar::info()`, `::warning()` custom logs |
| **Timeline** | Request lifecycle breakdown |
| **Exceptions** | Caught exceptions with stack traces |
| **Views** | Blade templates rendered + data passed |
| **Route** | Controller, middleware, route name |
| **Queries** | Every SQL query + execution time — spot N+1 instantly |
| **Models** | Eloquent models hydrated per request |
| **Auth** | Current user + guard |
| **Mail** | Emails triggered (not sent, just tracked) |

### Usage in Code

```php
// Log custom messages to the Debugbar
\Debugbar::info('User loaded: ' . $user->id);
\Debugbar::warning('Slow query detected');
\Debugbar::error('Something went wrong');

// Time a code block
\Debugbar::startMeasure('heavy-job', 'Running heavy job');
// ... expensive code ...
\Debugbar::stopMeasure('heavy-job');

// Add arbitrary data
\Debugbar::addMessage(['posts_count' => $posts->count()], 'label');
```

### Disable in Production

```env
# .env.production
APP_DEBUG=false
DEBUGBAR_ENABLED=false
```

Or conditionally in `config/debugbar.php`:

```php
'enabled' => env('APP_DEBUG', false) && env('APP_ENV') !== 'production',
```

> ⚠️ **Note:** Debugbar has no unit tests — it is a dev tool only. However, you should verify it is **disabled** in production/testing environments.

> 🧪 **Testing Tip:**
> - ✅ Add `DEBUGBAR_ENABLED=false` to your `.env.testing` to prevent it from interfering with JSON responses in feature tests
> - ✅ Assert it is not injected into API responses → `assertJsonMissing(['__debugbar'])`

```env
# .env.testing
APP_DEBUG=false
DEBUGBAR_ENABLED=false
```

---

| Step | Command |
|------|---------|
| New project | `composer create-project laravel/laravel app-name` |
| Generate key | `php artisan key:generate` |
| Install Bootstrap Auth | `composer require laravel/ui && php artisan ui bootstrap --auth` |
| Install Breeze | `composer require laravel/breeze --dev && php artisan breeze:install blade` |
| Compile assets | `npm install && npm run dev` |
| Create Model + Migration | `php artisan make:model Post -m` |
| Run migrations | `php artisan migrate` |
| Create Factory | `php artisan make:factory PostFactory --model=Post` |
| Create Seeder | `php artisan make:seeder PostSeeder` |
| Run seeder | `php artisan db:seed` |
| Fresh migrate + seed | `php artisan migrate:fresh --seed` |
| Create resource controller | `php artisan make:controller PostController --resource --model=Post` |
| Create form request | `php artisan make:request StorePostRequest` |
| List routes | `php artisan route:list` |
| Create feature test | `php artisan make:test PostTest` |
| Run tests | `php artisan test` |

---

> 💡 **Tip:** Combine model, migration, factory, seeder, controller, and form request in one command:
> ```bash
> php artisan make:model Post -mfsc --requests --resource
> ```
> `-m` migration · `-f` factory · `-s` seeder · `-c` controller · `--requests` form requests · `--resource` resource methods