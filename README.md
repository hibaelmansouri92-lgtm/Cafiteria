# 🍽️ Buvette Universitaire - Documentation Projet

**Système de Commande en Ligne pour Cafétéria Universitaire**

**Version**: 1.0.0  
**Date**: Mai 2026  
**Stack**: React 19 + Laravel 13 + SQLite

---

## 📚 Documentation Complète

Cette documentation est divisée en deux fichiers :

### 📱 [FRONTEND_DOCUMENTATION.md](./FRONTEND_DOCUMENTATION.md)
Documentation complète du frontend React :
- Architecture & structure du projet
- Installation et configuration
- Pages et composants
- Services API & AuthContext
- Design System Foodzand
- Guide de déploiement

### 🔧 [BACKEND_DOCUMENTATION.md](./BACKEND_DOCUMENTATION.md)
Documentation complète du backend Laravel :
- Architecture MVC & structure
- Installation et configuration
- Modèles de données (ER diagram)
- Routes API REST
- Contrôleurs & business logic
- Authentification (Sanctum)
- Migrations & seeders
- Guide de déploiement

---

## 🚀 Quick Start

### Frontend (React)

```bash
cd my-frontend
npm install
npm start
# → http://localhost:3001
```

### Backend (Laravel)

```bash
cd buvette-backend
composer install
php artisan migrate
php artisan db:seed
php artisan serve --port=8000
# → http://localhost:8000
```

---

## 📦 Vue d'Ensemble du Projet

```
buvette-universitaire/
├── my-frontend/          # Application React
│   ├── src/
│   │   ├── pages/        # Pages (public, user, admin)
│   │   ├── components/   # Composants réutilisables
│   │   ├── services/     # API client
│   │   ├── context/      # State management (Auth)
│   │   └── assets/       # Images & ressources
│   └── package.json
│
├── buvette-backend/      # API Laravel
│   ├── app/
│   │   ├── Models/       # Modèles Eloquent
│   │   └── Http/
│   │       ├── Controllers/  # Logique métier
│   │       └── Middleware/   # Auth, CORS
│   ├── routes/           # Définition des routes API
│   ├── database/
│   │   ├── migrations/   # Schéma DB
│   │   └── seeders/      # Données de test
│   └── composer.json
│
├── FRONTEND_DOCUMENTATION.md   # 📱 Doc frontend détaillée
├── BACKEND_DOCUMENTATION.md    # 🔧 Doc backend détaillée
└── README.md                    # 📖 Ce fichier

```

---

## 🎯 Fonctionnalités Principales

### 👥 Pour les Étudiants

- ✅ **Inscription & Connexion**
  - Créer compte avec email/mot de passe
  - Authentification sécurisée (Sanctum)
  - Session persistante

- 🛒 **Commande de Repas**
  - Parcourir catalogue produits par catégorie
  - Recherche et filtrage
  - Ajouter au panier + modifier quantités
  - Sélectionner créneau de récupération
  - Passer commande

- 📊 **Suivi Commandes**
  - Historique complet
  - État en temps réel (en cours → prête → livrée)
  - Détails de chaque commande

- 👤 **Profil Utilisateur**
  - Voir infos personnelles
  - Modifier profil
  - Déconnexion

### 👨‍💼 Pour l'Administration

- 📦 **Gestion Produits**
  - CRUD : créer, modifier, supprimer
  - Gérer stock & disponibilité
  - Ajouter images & descriptions

- 📂 **Gestion Catégories**
  - CRUD catégories
  - Associer produits

- 📋 **Gestion Commandes**
  - Voir toutes les commandes
  - Filtrer par statut
  - Modifier statut (en cours → prête → livrée)
  - Détails complets par commande

- ⏰ **Gestion Créneaux**
  - Créer/modifier créneaux de récupération
  - Lundi à vendredi
  - Limites de commandes par créneau

- 👥 **Gestion Utilisateurs**
  - Voir tous les utilisateurs
  - Supprimer/modifier rôles
  - Statistiques

- 📊 **Dashboard Admin**
  - Stats globales (total commandes, revenu, etc.)
  - Graphiques KPI
  - Quick links vers gestion

---

## 🔐 Sécurité

### Authentification

- **Method** : Bearer Token (Laravel Sanctum)
- **Storage** : localStorage (frontend)
- **Validation** : Middleware `auth:sanctum`

### Autorisations

- **Routes publiques** : Produits, catégories, login
- **Routes user** : Commandes personnelles
- **Routes admin** : CRUD complet (protégées par middleware `admin`)

### CORS

Configuré pour accepter :
- `http://localhost:3000`
- `http://localhost:3001`
- Production URL (à configurer)

---

## 🎨 Design System

### Palette Foodzand

| Nom | Couleur | Usage |
|-----|---------|-------|
| Rouge Principal | #e8272a | Buttons, active states, accents |
| Rouge Foncé | #c01f22 | Hover, gradients |
| Jaune | #f9c021 | Secondary elements |
| Vert | #22c55e | Success, availability |
| Crème | #FFF7ED | Main background |

### Typographie

- **Font** : Poppins (Google Fonts)
- **Weights** : 300-900
- **Styles** : Consistant à travers l'app

### Composants

- Buttons avec gradients et ombres
- Cards avec hover effects
- Navbar sticky avec logo
- Forms avec validation
- Modals pour actions
- Badges pour statuts

---

## 🔌 Intégration Frontend ↔ Backend

### Communication API

```
Frontend (Fetch/Axios)
        ↓
POST/GET/PUT/DELETE /api/...
        ↓
Authorization: Bearer {token}
        ↓
Backend Routes (routes/api.php)
        ↓
Controllers → Models → DB
        ↓
JSON Response
        ↓
Frontend → setState/Context
```

### Points de Contact

| Frontend | Backend |
|----------|---------|
| `/` | GET /products, /categories |
| `/login` | POST /auth/login |
| `/catalogue` | GET /products |
| `/user/cart` | GET /slots |
| `/user/checkout` | POST /orders |
| `/admin` | GET /orders, stats |
| `/admin/products` | CRUD /products |
| `/admin/orders` | PATCH /orders/{id}/status |

---

## 📝 Modèles de Données

### Schéma Principal

```
Users (Étudiants + Admins)
  ↓
Orders (Commandes)
  ├── OrderItems (Articles commandés)
  └── PickupSlots (Créneaux)

Products
  ├── Categories
  └── (Images)

Payments (optionnel)
```

**Voir** : BACKEND_DOCUMENTATION.md → Modèles de Données

---

## 🔗 Routes API

### Pattern REST

```
GET    /products              (Lire tous)
POST   /products              (Créer - admin)
GET    /products/{id}         (Lire un)
PUT    /products/{id}         (Modifier - admin)
DELETE /products/{id}         (Supprimer - admin)

POST   /orders                (Passer commande)
GET    /orders                (Mes commandes)
PATCH  /orders/{id}/status    (Modifier statut - admin)
```

**Voir** : BACKEND_DOCUMENTATION.md → Routes API

---

## 🗂️ Structure Fichiers Clés

### Frontend

```
src/
├── App.jsx                          # Routing principal
├── pages/
│   ├── public/Home.jsx
│   ├── public/Login.jsx
│   ├── user/Cart.jsx
│   ├── user/Checkout.jsx
│   └── admin/AdminDashboard.jsx
├── components/
│   ├── Navbar.jsx                  # Navigation
│   └── ProductCard.jsx
├── services/api.jsx                # Client HTTP
├── context/AuthContext.jsx         # State global
└── assets/buv_background.jpg
```

### Backend

```
app/
├── Models/
│   ├── User.php
│   ├── Product.php
│   ├── Order.php
│   └── PickupSlot.php
├── Http/Controllers/
│   ├── AuthController.php
│   ├── ProductController.php
│   └── OrderController.php
└── Http/Middleware/AdminMiddleware.php

routes/api.php                       # Définition routes
database/migrations/                 # Schéma DB
database/seeders/DatabaseSeeder.php  # Données test
```

---

## ⚙️ Configuration & Variables

### Frontend (.env)

```env
REACT_APP_API_URL=http://localhost:8000/api
```

### Backend (.env)

```env
APP_NAME="Buvette Universitaire"
DB_CONNECTION=sqlite
SANCTUM_STATEFUL_DOMAINS=localhost:3000,localhost:3001
CORS_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001
```

---

## 🧪 Testing

### Frontend

```bash
cd my-frontend
npm test                    # Run tests
npm run build               # Production build
```

### Backend

```bash
cd buvette-backend
php artisan test            # Run PHPUnit
php artisan test --filter=LoginTest
php artisan test --coverage
```

---

## 📊 Comptes de Test

### Admin

```
Email: admin@univ.com
Password: password123
Role: admin
```

### Étudiant

```
Email: hiba@univ.com
Password: password123
Role: student
```

*(Généré via seeders)*

---

## 🚀 Déploiement

### Frontend (Vercel / Netlify)

```bash
npm run build
# Push to GitHub & connecter Vercel/Netlify
```

### Backend (Laravel Forge / VPS)

```bash
composer install --optimize-autoloader
php artisan config:cache
php artisan migrate --force
```

**Voir** :
- FRONTEND_DOCUMENTATION.md → Déploiement
- BACKEND_DOCUMENTATION.md → Déploiement

---

## 🛠️ Développement

### Workflow

1. **Créer feature branch**
   ```bash
   git checkout -b feature/my-feature
   ```

2. **Frontend changes**
   ```bash
   cd my-frontend
   npm start
   ```

3. **Backend changes**
   ```bash
   cd buvette-backend
   php artisan serve
   ```

4. **Test & commit**
   ```bash
   git add .
   git commit -m "feat: add my-feature"
   git push origin feature/my-feature
   ```

5. **Pull request → review → merge**

---

## 📞 Support

### Besoin d'Aide ?

1. **Frontend** : Voir FRONTEND_DOCUMENTATION.md
2. **Backend** : Voir BACKEND_DOCUMENTATION.md
3. **Erreurs** : Vérifier browser console (F12) & server logs
4. **API** : Tester avec Postman/Insomnia

### Ressources

- [React Docs](https://react.dev)
- [Laravel Docs](https://laravel.com/docs)
- [Sanctum](https://laravel.com/docs/sanctum)
- [Bootstrap 5](https://getbootstrap.com)

---

## ✅ Checklist Qualité

- [ ] Responsive design testé
- [ ] Authentification fonctionne
- [ ] CRUD complet pour admin
- [ ] Messages erreurs clairs
- [ ] Pas de console errors
- [ ] Pagination / loading states
- [ ] Forms validées
- [ ] Images optimisées
- [ ] Env variables configurées
- [ ] Backend + Frontend en sync

---

## 📋 Roadmap Futur

### Phase 2

- [ ] Paiement en ligne (Stripe/Paypal)
- [ ] Notifications email
- [ ] Historique complet utilisateur
- [ ] Reviews/ratings produits
- [ ] Wishlist
- [ ] Dashboard stats avancées
- [ ] Dark mode
- [ ] Mobile app (React Native)

### Phase 3

- [ ] Intégration CMS
- [ ] Analytics avancées
- [ ] Export rapports (PDF)
- [ ] Multi-language support
- [ ] Système de stock auto
- [ ] Prédictions demande

---

## 📄 Licence

Ce projet est la propriété de l'Université. Tous droits réservés.

---

## 👥 Équipe

**Développé par** : Smart Cafeteria Team  
**Dernière mise à jour** : 20 Mai 2026

---

## 📞 Contact

- **Email** : contact@foodzand.ma
- **Phone** : +212 6 XX XX XX XX
- **Localisation** : Université [Nom], Ville

---

**🎉 Merci d'utiliser Buvette Universitaire !**
