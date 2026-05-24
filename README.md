# MaklaTime

Système Web de Gestion de Buvette Universitaire
Projet réalisé dans le cadre du module :
Développement Web – Contrôle Pratique
Université Chouaib Doukkali – ENSA El Jadida
2ITE S2 – Année Universitaire 2025-2026

---

# Description du Projet

MaklaTime est une application web moderne permettant la gestion d’une buvette universitaire à travers une architecture Full Stack basée sur ReactJS et Laravel API REST.

L’application permet :

* la consultation des produits disponibles
* la gestion des commandes étudiantes
* l’authentification sécurisée multi-rôles
* la gestion complète des produits et catégories via un dashboard administrateur

Le projet respecte la séparation Front-End / Back-End à travers des requêtes HTTP sécurisées via Axios.

---

# Technologies Utilisées

## Front-End

* ReactJS
* Vite
* Axios
* React Router
* Bootstrap / CSS

## Back-End

* Laravel 13
* Laravel Sanctum
* API REST
* Eloquent ORM

## Base de Données

* SQLite / MySQL

---

# Fonctionnalités Principales

## Authentification

* Inscription et connexion sécurisées
* Gestion des rôles :

  * Admin
  * User

---

## Espace Utilisateur

* Consultation du catalogue
* Recherche de produits
* Ajout au panier
* Passage de commandes
  

---

## Espace Administrateur

* Dashboard administrateur
* Gestion des produits (CRUD)
* Gestion des catégories (CRUD)
* Gestion des commandes
* Gestion des utilisateurs
* Gestion du stock et disponibilité

---

# Architecture du Projet

```bash id="d9g0gf"
maklaTime/
│
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── services/
│   │   ├── context/
│   │   └── assets/
│   └── package.json
│
├── backend/
│   ├── app/
│   ├── routes/
│   ├── database/
│   └── composer.json
│
└── README.md
```

---

# Installation et Lancement du Projet

# 1. Cloner le Repository

```bash id="yr4gr4"
git clone <repository-link>
```

---

# 2. Installation du Backend Laravel

## Accéder au dossier backend

```bash id="3sjlwm"
cd backend
```

## Installer les dépendances

```bash id="4s7n5f"
composer install
```

## Copier le fichier .env

```bash id="c76wkt"
cp .env.example .env
```

## Générer la clé Laravel

```bash id="44b4v2"
php artisan key:generate
```

## Configurer la base de données dans `.env`

```env id="nnv17m"
DB_CONNECTION=sqlite
```

ou

```env id="6vjlwm"
DB_CONNECTION=mysql
DB_DATABASE=maklatime
DB_USERNAME=root
DB_PASSWORD=
```

## Exécuter les migrations

```bash id="10kzjz"
php artisan migrate
```

## Lancer le serveur Laravel

```bash id="8m0u0u"
php artisan serve
```

Backend disponible sur :

```bash id="svlfrn"
http://127.0.0.1:8000
```

---

# 3. Installation du Frontend React

## Accéder au dossier frontend

```bash id="ib6z8w"
cd frontend
```

## Installer les dépendances

```bash id="uwm8ah"
npm install
```

## Configurer le fichier `.env`

```env id="5r1x8f"
VITE_API_URL=http://127.0.0.1:8000/api
```

## Lancer le serveur React

```bash id="1j8lsj"
npm run dev
```

Frontend disponible sur :

```bash id="r34e67"
http://localhost:5173
```

---

# Structure des Fonctionnalités

## Front-End

* Pages publiques
* Pages utilisateur
* Dashboard administrateur
* Composants réutilisables
* Gestion d’état avec Context API
* Communication API via Axios

---

## Back-End

* Architecture MVC Laravel
* API REST sécurisée
* Contrôleurs CRUD
* Middleware d’authentification
* Validation des données
* Gestion des rôles

---

# Sécurité

Le projet implémente :

* Laravel Sanctum
* Authentification par token
* Protection des routes administrateur
* Validation des formulaires
* Gestion des erreurs côté client et serveur

---

# Routes API Principales

```bash id="2b4t2d"
GET    /api/products
POST   /api/products
PUT    /api/products/{id}
DELETE /api/products/{id}

GET    /api/categories
POST   /api/categories

POST   /api/login
POST   /api/register
POST   /api/logout
```

---

# Comptes de Test

## Admin

```text id="0r2c9q"
Email: admin@maklatime.com
Password: password123
```

## User

```text id="hxb0p2"
Email: user@maklatime.com
Password: password123
```

---

# Déploiement

## Front-End

Compatible avec :

* Vercel
* Netlify

## Back-End

Compatible avec :

* VPS
* Laravel Forge
* Shared Hosting

---

# Équipe

Projet développé dans le cadre du contrôle pratique du module Développement Web.

Université Chouaib Doukkali – ENSA El Jadida
2ITE S2 – 2025/2026


