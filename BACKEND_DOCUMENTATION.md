# 🔧 Documentation Backend - Buvette Universitaire

**Version**: 1.0.0  
**Framework**: Laravel 13.0  
**PHP**: 8.3+  
**Date**: Mai 2026

---

## 📋 Table des Matières

1. [Architecture Générale](#architecture-générale)
2. [Installation & Configuration](#installation--configuration)
3. [Structure du Projet](#structure-du-projet)
4. [Modèles de Données](#modèles-de-données)
5. [Routes API](#routes-api)
6. [Contrôleurs](#contrôleurs)
7. [Authentification](#authentification)
8. [Migrations & Seeders](#migrations--seeders)
9. [Validation & Erreurs](#validation--erreurs)
10. [Déploiement](#déploiement)

---

## 🏗️ Architecture Générale

### Stack Technologique

```
Backend: Laravel 13.0
├── Authentication: Laravel Sanctum 4.3
├── Database: SQLite (développement)
├── ORM: Eloquent
├── HTTP: RESTful API
└── Testing: PHPUnit 12.5.12
```

### Architecture MVC

```
HTTP Request
    ↓
Routes (routes/api.php)
    ↓
Controllers (app/Http/Controllers/)
    ↓
Models (app/Models/) → Database
    ↓
JSON Response
```

### Flux API

```
Frontend (React)
    ↓ (Bearer Token)
CORS Middleware (config/cors.php)
    ↓
Auth Middleware (Sanctum)
    ↓
Route → Controller → Model → Database
    ↓
JSON Response → Frontend
```

---

## 🚀 Installation & Configuration

### Prérequis

- PHP 8.3+
- Composer
- SQLite (ou autre DB)
- Node.js (optionnel, pour assets)

### Installation

```bash
cd buvette-backend

# 1. Installer dépendances
composer install

# 2. Copier env
cp .env.example .env

# 3. Générer app key
php artisan key:generate

# 4. Créer base de données
touch database/database.sqlite

# 5. Migrations
php artisan migrate

# 6. Seeders (données de test)
php artisan db:seed
```

### Configuration

**`.env` - Clés importantes** :

```env
APP_NAME="Buvette Universitaire"
APP_ENV=local
APP_KEY=base64:... (auto-généré)
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=sqlite
DB_DATABASE=database/database.sqlite

SANCTUM_STATEFUL_DOMAINS=localhost:3000,localhost:3001

CORS_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001
```

**`config/cors.php`** :

```php
return [
    'paths'              => ['api/*', 'sanctum/csrf-cookie'],
    'allowed_methods'    => ['*'],
    'allowed_origins'    => ['http://localhost:3000', 'http://localhost:3001'],
    'allowed_headers'    => ['*'],
    'exposed_headers'    => [],
    'max_age'            => 0,
    'supports_credentials' => true,
];
```

### Démarrage

```bash
# Serveur développement
php artisan serve --port=8000

# Ou
php artisan serve
# → http://127.0.0.1:8000
```

---

## 📁 Structure du Projet

```
buvette-backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── AuthController.php
│   │   │   ├── ProductController.php
│   │   │   ├── CategoryController.php
│   │   │   ├── OrderController.php
│   │   │   ├── OrderItemController.php
│   │   │   ├── UserController.php
│   │   │   ├── SlotController.php
│   │   │   └── PaymentController.php
│   │   ├── Middleware/
│   │   │   ├── AdminMiddleware.php
│   │   │   └── Cors.php
│   │   └── Requests/
│   ├── Models/
│   │   ├── User.php
│   │   ├── Product.php
│   │   ├── Category.php
│   │   ├── Order.php
│   │   ├── OrderItem.php
│   │   ├── PickupSlot.php
│   │   ├── Payment.php
│   │   └── ...
│   ├── Providers/
│   │   └── AppServiceProvider.php
│   └── ...
├── bootstrap/
│   ├── app.php
│   └── providers.php
├── config/
│   ├── app.php
│   ├── auth.php
│   ├── cors.php
│   ├── database.php
│   └── ...
├── database/
│   ├── migrations/
│   │   ├── *_create_users_table.php
│   │   ├── *_create_products_table.php
│   │   ├── *_create_categories_table.php
│   │   ├── *_create_orders_table.php
│   │   ├── *_create_order_items_table.php
│   │   ├── *_create_pickup_slots_table.php
│   │   ├── *_create_payments_table.php
│   │   └── ...
│   ├── factories/
│   │   └── UserFactory.php
│   └── seeders/
│       ├── DatabaseSeeder.php
│       └── UserSeeder.php
├── routes/
│   ├── api.php         # Routes API
│   ├── web.php         # Routes web
│   └── console.php     # Commandes Artisan
├── storage/
│   ├── app/
│   ├── framework/
│   └── logs/
├── tests/
│   ├── Feature/
│   └── Unit/
├── .env                # Configuration
├── .env.example        # Template .env
├── artisan             # CLI Laravel
├── composer.json       # Dépendances
├── composer.lock       # Versions verrouillées
└── phpunit.xml         # Config tests
```

---

## 🗄️ Modèles de Données

### Diagramme ER

```
Users
├── id (PK)
├── name
├── email (UNIQUE)
├── password
├── role (student|admin)
├── created_at
└── updated_at

    ↓
    ├── 1:N → Orders
    │
    └── 1:N → (future: Wishlist, Reviews)

Products
├── id (PK)
├── name
├── price
├── description
├── emoji
├── category_id (FK)
├── stock (boolean)
├── image (nullable)
├── created_at
└── updated_at

Categories
├── id (PK)
├── name
├── description
├── emoji
├── color
├── created_at
└── updated_at

    ↓
    └── 1:N → Products

Orders
├── id (PK)
├── user_id (FK)
├── total
├── status (en_cours|prete|livree|annulee)
├── pickup_slot_id (FK)
├── notes (nullable)
├── created_at
└── updated_at

    ├── 1:N → OrderItems
    └── 0:1 → Payment

OrderItems
├── id (PK)
├── order_id (FK)
├── product_id (FK)
├── quantity
├── price (snapshot)
├── created_at
└── updated_at

PickupSlots
├── id (PK)
├── day (Monday-Friday)
├── start_time
├── end_time
├── max_orders
├── current_orders
├── active (boolean)
├── created_at
└── updated_at

    ↓
    └── 1:N → Orders

Payments (optionnel)
├── id (PK)
├── order_id (FK)
├── amount
├── method (cash|card|etc)
├── status (pending|completed|failed)
└── ...
```

### Modèles Détaillés

#### **User**

```php
class User extends Authenticatable {
    protected $fillable = ['name', 'email', 'password', 'role'];
    protected $hidden = ['password', 'remember_token'];
    
    public function orders() { return $this->hasMany(Order::class); }
    public function isAdmin(): bool { return $this->role === 'admin'; }
}
```

#### **Product**

```php
class Product extends Model {
    protected $fillable = ['name', 'price', 'description', 'emoji', 
                          'category_id', 'stock', 'image'];
    
    public function category() { return $this->belongsTo(Category::class); }
    public function orderItems() { return $this->hasMany(OrderItem::class); }
}
```

#### **Category**

```php
class Category extends Model {
    protected $fillable = ['name', 'description', 'emoji', 'color'];
    public function products() { return $this->hasMany(Product::class); }
}
```

#### **Order**

```php
class Order extends Model {
    protected $fillable = ['user_id', 'total', 'status', 'pickup_slot_id', 'notes'];
    
    public function user() { return $this->belongsTo(User::class); }
    public function items() { return $this->hasMany(OrderItem::class); }
    public function slot() { return $this->belongsTo(PickupSlot::class, 'pickup_slot_id'); }
    public function payment() { return $this->hasOne(Payment::class); }
}
```

#### **OrderItem**

```php
class OrderItem extends Model {
    protected $fillable = ['order_id', 'product_id', 'quantity', 'price'];
    
    public function order() { return $this->belongsTo(Order::class); }
    public function product() { return $this->belongsTo(Product::class); }
}
```

#### **PickupSlot**

```php
class PickupSlot extends Model {
    protected $fillable = ['day', 'start_time', 'end_time', 'max_orders', 
                          'current_orders', 'active'];
    
    public function orders() { return $this->hasMany(Order::class, 'pickup_slot_id'); }
}
```

---

## 🛣️ Routes API

### Structure des Routes

```
/api/
├── PUBLIC (sans auth)
│   ├── POST   /auth/register
│   ├── POST   /auth/login
│   ├── GET    /products
│   ├── GET    /products/{id}
│   ├── GET    /categories
│   └── GET    /categories/{id}
│
├── AUTHENTICATED (auth:sanctum)
│   ├── POST   /auth/logout
│   ├── GET    /me
│   ├── GET    /slots
│   ├── GET    /orders
│   ├── POST   /orders
│   ├── GET    /orders/{id}
│   └── DELETE /orders/{id}
│
└── ADMIN ONLY (auth:sanctum + admin middleware)
    ├── PRODUCTS
    │   ├── POST   /products
    │   ├── PUT    /products/{id}
    │   └── DELETE /products/{id}
    ├── CATEGORIES
    │   ├── POST   /categories
    │   ├── PUT    /categories/{id}
    │   └── DELETE /categories/{id}
    ├── ORDERS
    │   └── PATCH  /orders/{id}/status
    ├── SLOTS
    │   ├── POST   /slots
    │   ├── PUT    /slots/{id}
    │   └── DELETE /slots/{id}
    └── USERS
        ├── GET    /users
        ├── PUT    /users/{id}
        └── DELETE /users/{id}
```

### Détail des Routes (routes/api.php)

```php
// PUBLIC
Route::post('/auth/register', [AuthController::class, 'register']);
Route::post('/auth/login', [AuthController::class, 'login']);
Route::get('/products', [ProductController::class, 'index']);
Route::get('/products/{product}', [ProductController::class, 'show']);
Route::get('/categories', [CategoryController::class, 'index']);

// AUTHENTICATED
Route::middleware('auth:sanctum')->group(function () {
    Route::post('/auth/logout', [AuthController::class, 'logout']);
    Route::get('/me', [AuthController::class, 'me']);
    Route::get('/slots', [SlotController::class, 'index']);
    Route::get('/orders', [OrderController::class, 'index']);
    Route::post('/orders', [OrderController::class, 'store']);
    Route::get('/orders/{order}', [OrderController::class, 'show']);
    Route::delete('/orders/{order}', [OrderController::class, 'cancel']);
    
    // ADMIN ONLY
    Route::middleware('admin')->group(function () {
        Route::apiResource('products', ProductController::class);
        Route::apiResource('categories', CategoryController::class);
        Route::patch('/orders/{order}/status', [OrderController::class, 'updateStatus']);
        Route::apiResource('slots', SlotController::class);
        Route::apiResource('users', UserController::class);
    });
});
```

---

## 🎛️ Contrôleurs

### AuthController

```php
class AuthController {
    public function register(Request $request)
        // POST /auth/register
        // Body: { name, email, password }
        // Returns: { message, token, user }
        
    public function login(Request $request)
        // POST /auth/login
        // Body: { email, password }
        // Returns: { message, token, user }
        
    public function logout(Request $request)
        // POST /auth/logout
        // Returns: { message }
        
    public function me(Request $request)
        // GET /me
        // Returns: { id, name, email, role }
}
```

### ProductController (CRUD)

```php
class ProductController {
    public function index()
        // GET /products
        // Returns: { data: [...] }
        
    public function show(Product $product)
        // GET /products/{id}
        // Returns: single product
        
    public function store(Request $request) // Admin
        // POST /products
        // Body: { name, price, category_id, description, emoji, stock, image }
        
    public function update(Request $request, Product $product) // Admin
        // PUT /products/{id}
        
    public function destroy(Product $product) // Admin
        // DELETE /products/{id}
}
```

### CategoryController (CRUD)

```php
class CategoryController {
    public function index()
    public function store(Request $request)     // Admin
    public function update(Request $request, Category $category) // Admin
    public function destroy(Category $category) // Admin
}
```

### OrderController

```php
class OrderController {
    public function index()
        // GET /orders
        // Returns: user's orders (ou all si admin)
        
    public function store(Request $request)
        // POST /orders
        // Body: { items: [{product_id, quantity}], pickup_slot_id }
        // Creates Order + OrderItems + calcule total
        
    public function show(Order $order)
        // GET /orders/{id}
        // Returns: order details + items
        
    public function updateStatus(Request $request, Order $order) // Admin
        // PATCH /orders/{id}/status
        // Body: { status: en_cours|prete|livree|annulee }
        
    public function cancel(Order $order)
        // DELETE /orders/{id}
        // Sets status = 'annulee'
}
```

### UserController

```php
class UserController {
    public function index() // Admin
        // GET /users
        // Returns: all users
        
    public function update(Request $request, User $user) // Admin
    public function destroy(User $user) // Admin
}
```

### SlotController

```php
class SlotController {
    public function index()
        // GET /slots
        // Returns: all available slots
        
    public function store(Request $request) // Admin
    public function update(Request $request, PickupSlot $slot) // Admin
    public function destroy(PickupSlot $slot) // Admin
}
```

---

## 🔐 Authentification

### Laravel Sanctum

**Tokens API personnels** :
- Token générés lors du login
- Stockés en DB (table `personal_access_tokens`)
- Envoyés dans header `Authorization: Bearer {token}`

### Flow

```
1. Client POST /auth/register ou /auth/login
2. Backend valide credentials
3. Crée token avec: $user->createToken('auth_token')->plainTextToken
4. Retourne: { token: "xxx...", user: {...} }
5. Client stocke token en localStorage
6. Tous appels suivants incluent: Authorization: Bearer xxx...
7. Middleware auth:sanctum valide token
```

### Middleware

**Admin Middleware** (`app/Http/Middleware/AdminMiddleware.php`) :

```php
public function handle($request, Closure $next) {
    if (!auth()->user()?->isAdmin()) {
        return response()->json(['message' => 'Unauthorized'], 403);
    }
    return $next($request);
}
```

### Rôles

| Rôle | Accès |
|------|-------|
| **student** | Lire produits, créer/voir commandes, voir profil |
| **admin** | CRUD complet, gestion utilisateurs, stats |

---

## 🗃️ Migrations & Seeders

### Migrations (database/migrations/)

```bash
php artisan migrate              # Exécute toutes les migrations
php artisan migrate:status       # État des migrations
php artisan migrate:rollback     # Annule dernière batch
php artisan migrate:fresh        # Reset + re-execute
```

### Migrations Principales

```
*_create_users_table.php
├── id, name, email, password, role, timestamps

*_create_products_table.php
├── id, name, price, category_id, description, emoji, stock, image, timestamps

*_create_categories_table.php
├── id, name, description, emoji, color, timestamps

*_create_orders_table.php
├── id, user_id, total, status, pickup_slot_id, notes, timestamps

*_create_order_items_table.php
├── id, order_id, product_id, quantity, price, timestamps

*_create_pickup_slots_table.php
├── id, day, start_time, end_time, max_orders, current_orders, active, timestamps

*_create_payments_table.php
├── id, order_id, amount, method, status, timestamps
```

### Seeders (database/seeders/)

```bash
php artisan db:seed              # Exécute DatabaseSeeder
php artisan db:seed --class=UserSeeder
```

**DatabaseSeeder** :
- Crée 1 admin (admin@buvette.com / password123)
- Crée 5 étudiants fictifs
- Crée catégories + produits de test
- Crée créneaux de test

---

## ✅ Validation & Erreurs

### Request Validation

```php
$request->validate([
    'name'     => 'required|string|max:255',
    'email'    => 'required|email|unique:users,email',
    'password' => 'required|min:6'
]);
```

### Error Response Format

```json
{
    "message": "Validation failed",
    "errors": {
        "email": ["Email already exists"]
    }
}
```

### HTTP Status Codes

| Code | Usage |
|------|-------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden (Admin) |
| 404 | Not Found |
| 422 | Validation Error |
| 500 | Server Error |

---

## 🚀 Déploiement

### Prérequis Production

- PHP 8.3+ avec extensions (pdo_sqlite ou pdo_mysql)
- Composer
- HTTPS obligatoire (Sanctum)

### Deployment Checklist

```bash
# 1. Pull code latest
git pull origin main

# 2. Installer dépendances
composer install --optimize-autoloader --no-dev

# 3. Configuration production
cp .env.example .env
php artisan key:generate
# Configurer: APP_DEBUG=false, DB_*, SANCTUM_STATEFUL_DOMAINS=yourdomain.com

# 4. Optimiser
php artisan config:cache
php artisan route:cache
php artisan view:cache

# 5. Migrations
php artisan migrate --force

# 6. Storage
chmod -R 775 storage bootstrap/cache

# 7. Restart
php artisan cache:clear
```

### Serveurs Recommandés

- **Shared Hosting** : Avec PHP 8.3+
- **VPS** : DigitalOcean, Linode, AWS
- **Platform** : Laravel Forge, Envoyer, PlanetScale

### .env Production

```env
APP_ENV=production
APP_DEBUG=false
APP_KEY=base64:xxx...
APP_URL=https://yourdomain.com

DB_CONNECTION=mysql
DB_HOST=db.yourdomain.com
DB_DATABASE=buvette_prod
DB_USERNAME=user
DB_PASSWORD=secure_pass

SANCTUM_STATEFUL_DOMAINS=yourdomain.com,www.yourdomain.com
SESSION_DOMAIN=yourdomain.com

CORS_ALLOWED_ORIGINS=https://yourdomain.com

MAIL_FROM_ADDRESS=noreply@yourdomain.com
```

---

## 🧪 Tests

```bash
# Lancer tests
php artisan test

# Test spécifique
php artisan test --filter=LoginTest

# Avec coverage
php artisan test --coverage
```

### Fichiers Test

```
tests/
├── Feature/
│   ├── AuthTest.php
│   ├── ProductTest.php
│   └── OrderTest.php
└── Unit/
    └── ModelTest.php
```

---

## 📊 Commandes Artisan Utiles

```bash
# Serve
php artisan serve --port=8000

# Migrate
php artisan migrate
php artisan migrate:fresh
php artisan migrate:rollback

# Seed
php artisan db:seed
php artisan tinker (REPL interactive)

# Cache
php artisan cache:clear
php artisan config:clear
php artisan view:clear

# Optimize
php artisan optimize
php artisan optimize:clear

# Logs
tail -f storage/logs/laravel.log
```

---

## 🔧 Configuration CORS (Important!)

**File**: `config/cors.php`

```php
return [
    'paths'              => ['api/*', 'sanctum/csrf-cookie'],
    'allowed_methods'    => ['*'],
    'allowed_origins'    => [
        'http://localhost:3000',
        'http://localhost:3001',
        'https://yourdomain.com'
    ],
    'allowed_origins_patterns' => [],
    'allowed_headers'    => ['*'],
    'exposed_headers'    => [],
    'max_age'            => 0,
    'supports_credentials' => true,
];
```

---

## 📝 Convention de Code

### Nommage

- Classes : PascalCase (`ProductController`)
- Méthodes : camelCase (`updateStatus()`)
- Variables : camelCase (`$userId`)
- Constants : UPPER_SNAKE_CASE (`MAX_ITEMS`)
- Tables : snake_case plural (`users`, `orders`)
- Colonnes : snake_case (`user_id`, `created_at`)

### Structure Modèle

```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model {
    use HasFactory;
    
    protected $fillable = ['name', 'price', 'category_id'];
    protected $casts = ['price' => 'float'];
    
    // Relations
    public function category() {
        return $this->belongsTo(Category::class);
    }
    
    // Scopes
    public function scopeActive($query) {
        return $query->where('stock', true);
    }
}
```

---

## 📞 Support & Ressources

- **Laravel Docs** : https://laravel.com/docs
- **Sanctum** : https://laravel.com/docs/sanctum
- **Eloquent** : https://laravel.com/docs/eloquent
- **API Resource** : https://laravel.com/docs/eloquent-resources

---

**Dernière mise à jour** : 20 Mai 2026
