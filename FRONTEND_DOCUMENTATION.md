# 📱 Documentation Frontend - Buvette Universitaire

**Version**: 1.0.0  
**Framework**: React 19.2.6  
**Date**: Mai 2026

---

## 📋 Table des Matières

1. [Architecture Générale](#architecture-générale)
2. [Structure du Projet](#structure-du-projet)
3. [Installation & Configuration](#installation--configuration)
4. [Pages et Composants](#pages-et-composants)
5. [Services API](#services-api)
6. [Gestion d'État](#gestion-détat)
7. [Authentification](#authentification)
8. [Design System](#design-system)
9. [Déploiement](#déploiement)

---

## 🏗️ Architecture Générale

### Stack Technologique

```
Frontend: React 19.2.6
├── Routing: React Router DOM 7.15.0
├── HTTP Client: Axios 1.16.0
├── UI Framework: Bootstrap 5.3.8
├── Charting: Recharts 3.8.1
├── Notifications: React Hot Toast 2.6.0
└── Build Tool: React Scripts 5.0.1
```

### Flux d'Application

```
┌─────────────────────┐
│   React Components  │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  AuthContext (State)│
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│   API Service       │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  Backend API (Axios)│
│ http://localhost:8000
└─────────────────────┘
```

---

## 📁 Structure du Projet

```
my-frontend/
├── public/
│   ├── index.html          # Point d'entrée HTML
│   ├── manifest.json       # PWA manifest
│   └── robots.txt          # SEO
├── src/
│   ├── App.jsx             # Composant racine
│   ├── App.css             # Styles globaux
│   ├── index.js            # Point d'entrée React
│   ├── assets/
│   │   └── buv_background.jpg
│   ├── components/
│   │   ├── Navbar.jsx      # Navigation principale
│   │   ├── Footer.jsx      # Pied de page
│   │   ├── Loader.jsx      # Spinner de chargement
│   │   ├── Toast.jsx       # Notifications
│   │   └── ProductCard.jsx # Carte produit
│   ├── context/
│   │   └── AuthContext.jsx # Context Auth globale
│   ├── pages/
│   │   ├── public/
│   │   │   ├── Home.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── Catalogue.jsx
│   │   │   └── ProductDetails.jsx
│   │   ├── user/
│   │   │   ├── Dashboard.jsx
│   │   │   ├── Cart.jsx
│   │   │   ├── Checkout.jsx
│   │   │   ├── Profile.jsx
│   │   │   └── StudentOrders.jsx
│   │   └── admin/
│   │       ├── AdminDashboard.jsx
│   │       ├── AdminUsers.jsx
│   │       ├── AdminProducts.jsx
│   │       ├── AdminCategories.jsx
│   │       ├── AdminOrders.jsx
│   │       ├── AdminSlots.jsx
│   │       └── AdminStyles.css
│   └── services/
│       ├── api.jsx         # Client HTTP + endpoints
│       └── authService.js  # Auth helpers
├── package.json
└── .env                    # Variables d'environnement
```

---

## 🚀 Installation & Configuration

### Prérequis

- Node.js 16.x ou supérieur
- npm ou yarn
- Backend Laravel actif sur `http://localhost:8000`

### Installation

```bash
cd my-frontend
npm install
```

### Configuration Environnement

Créez un fichier `.env` :

```env
REACT_APP_API_URL=http://localhost:8000/api
```

### Démarrage

```bash
# Mode développement (port 3000 ou 3001)
npm start

# Build production
npm build

# Tests
npm test
```

---

## 📄 Pages et Composants

### Pages Publiques

#### **Home** (`/`)
- Hero section avec CTA
- Présentation du service
- Appel à l'action (Login/Register)
- Design: Palette Foodzand (rouge #e8272a, orange, vert)

#### **Login** (`/login`)
- Formulaire email/password
- Validation en temps réel
- Design cohérent avec les pages
- Redirection dynamique post-login (admin → `/admin`, étudiant → `/user`)

#### **Register** (`/register`)
- Formulaire d'inscription (nom, email, mot de passe)
- Validation
- Création compte + token auto-sauvegardé

#### **Catalogue** (`/catalogue`)
- Affichage des produits par catégorie
- Filtrage et recherche
- Pagination
- Ajout au panier
- Détails produit au clic

#### **ProductDetails** (`/produit/:id`)
- Détail complet du produit
- Image + description + prix
- Bouton ajouter au panier

### Pages Utilisateur

#### **Dashboard** (`/user`)
- Vue d'ensemble (commandes récentes, panier)
- Lien vers mes commandes et profil

#### **Cart** (`/user/cart`)
- Affichage articles du panier
- Modification quantités
- Calcul total
- Procédure de commande

#### **Checkout** (`/user/checkout`)
- Sélection créneau de récupération
- Confirmation finale
- Placement de commande

#### **StudentOrders** (`/user/commandes`)
- Historique des commandes
- État (en cours, prête, livrée, annulée)
- Détails par commande

#### **Profile** (`/user/profil`)
- Infos utilisateur
- Modification profil
- Déconnexion

### Pages Admin

#### **AdminDashboard** (`/admin`)
- Stats globales : total commandes, revenu, commandes/jour, panier moyen
- Graphiques et KPIs
- Quick links vers gestion

#### **AdminProducts** (`/admin/products`)
- Liste tous produits
- CRUD : créer, modifier, supprimer
- Recherche et filtrage
- Statut stock disponible/rupture

#### **AdminCategories** (`/admin/categories`)
- Gestion catégories
- CRUD complet
- Couleurs personnalisées

#### **AdminOrders** (`/admin/orders`)
- Toutes les commandes
- Filtrage par statut
- Modification statut
- Détails complet

#### **AdminSlots** (`/admin/slots`)
- Gestion créneaux de récupération
- CRUD

#### **AdminUsers** (`/admin/users`)
- Liste utilisateurs
- Suppression/modification

---

## 🔌 Services API

### Fichier : `src/services/api.jsx`

#### Configuration de Base

```javascript
const BASE_URL = "http://localhost:8000/api";

function authHeaders(isFormData = false) {
  const token = localStorage.getItem("auth_token");
  return {
    Authorization: `Bearer ${token}`,
    "Content-Type": isFormData ? undefined : "application/json",
  };
}
```

#### Endpoints Disponibles

### Auth

```javascript
loginUser(credentials)          // POST /auth/login
registerUser(userData)          // POST /auth/register
logoutUser()                    // POST /auth/logout
```

### Produits

```javascript
productsApi.getAll()            // GET /products
productsApi.create(data)        // POST /products (admin)
productsApi.update(id, data)    // PUT /products/:id (admin)
productsApi.remove(id)          // DELETE /products/:id (admin)
```

### Catégories

```javascript
categoriesApi.getAll()          // GET /categories
categoriesApi.create(data)      // POST /categories (admin)
categoriesApi.update(id, data)  // PUT /categories/:id (admin)
categoriesApi.remove(id)        // DELETE /categories/:id (admin)
```

### Commandes

```javascript
ordersApi.getAll()              // GET /orders
ordersApi.create(data)          // POST /orders
ordersApi.show(id)              // GET /orders/:id
ordersApi.updateStatus(id, status)  // PATCH /orders/:id/status (admin)
ordersApi.cancel(id)            // DELETE /orders/:id
```

### Créneaux

```javascript
slotsApi.getAll()               // GET /slots
slotsApi.create(data)           // POST /slots (admin)
slotsApi.update(id, data)       // PUT /slots/:id (admin)
slotsApi.remove(id)             // DELETE /slots/:id (admin)
```

### Utilisateurs

```javascript
usersApi.getAll()               // GET /users (admin)
usersApi.update(id, data)       // PUT /users/:id (admin)
usersApi.remove(id)             // DELETE /users/:id (admin)
```

---

## 🗂️ Gestion d'État

### AuthContext (`src/context/AuthContext.jsx`)

Gère l'authentification globale :

```javascript
{
  user: { id, name, email, role },
  token: string,
  loading: boolean,
  login: (email, password) => Promise,
  logout: () => void,
  isAdmin: () => boolean,
  isStudent: () => boolean
}
```

**Usage** :

```javascript
import { useAuth } from '../context/AuthContext';

function MyComponent() {
  const { user, login, logout, isAdmin } = useAuth();
  // ...
}
```

---

## 🔐 Authentification

### Flow

1. **Login/Register**
   - Envoi credentials à `/auth/login` ou `/auth/register`
   - Backend retourne `token` + `user`
   - Token stocké dans `localStorage`

2. **Headers Request**
   - Tous les appels incluent : `Authorization: Bearer {token}`

3. **Token Validation**
   - Backend valide via Laravel Sanctum
   - Routes protégées avec `auth:sanctum`
   - Routes admin avec middleware `admin`

4. **Logout**
   - Suppression token du localStorage
   - Appel `/auth/logout` (optionnel)

### Rôles

- **student** : Accès pages user uniquement
- **admin** : Accès pages admin + toutes les routes

---

## 🎨 Design System

### Palette de Couleurs (Foodzand)

```
Rouge Principal        : #e8272a (buttons, active states)
Rouge Foncé           : #c01f22 (hover, gradients)
Rouge Clair           : #ff5558
Rouge Glow            : rgba(232,39,42,0.25)
Jaune                 : #f9c021
Vert                  : #22c55e
Orange                : #f59e0b
Violet                : #8b5cf6

Fonds
Crème Principal       : #FFF7ED
Crème Alt             : #FFE8CC
Blanc                 : #FFFFFF
Gris Léger            : #f8f9fa

Textes
Noir Presque          : #1F1F1F
Gris Foncé            : #2d2d2d
Gris Doux             : #4A4A4A
Gris Moyen            : #6b7280
Gris Clair            : #9CA3AF
```

### Typographie

- **Font** : Poppins (weights: 300, 400, 500, 600, 700, 800, 900)
- **Import** : `@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap')`

### Animations

```css
/* Transition standard */
transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);

/* Ombres */
box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
```

### Boutons

```
Primary Button
├── Gradient: 135deg, red → darkRed
├── Color: White
├── Padding: 0.85rem 1.4rem
├── Border-radius: 12px
└── Shadow: 0 6px 20px redGlow

Outline Button
├── Background: Transparent
├── Border: 1.5px solid border
├── Color: textSoft
└── Hover: border red, bg redLight
```

---

## 📦 Dépendances Principales

| Package | Version | Utilité |
|---------|---------|---------|
| react | 19.2.6 | Framework principal |
| react-router-dom | 7.15.0 | Routing |
| axios | 1.16.0 | HTTP requests (non utilisé, fetch à la place) |
| bootstrap | 5.3.8 | Framework CSS (optionnel) |
| recharts | 3.8.1 | Graphiques et charts |
| react-hot-toast | 2.6.0 | Notifications toast |

---

## 🚀 Déploiement

### Build Production

```bash
npm run build
```

Génère un dossier `build/` optimisé.

### Options de Déploiement

- **Vercel** : Connecter GitHub repo
- **Netlify** : Drop `build/` ou connecter GitHub
- **Docker** :
  ```dockerfile
  FROM node:16
  WORKDIR /app
  COPY . .
  RUN npm install && npm run build
  CMD ["serve", "-s", "build"]
  ```

### Variables d'Environnement Production

```env
REACT_APP_API_URL=https://api.yourdomain.com/api
```

---

## 🔗 Intégration avec Backend

### CORS Configuration

Backend accepte les origins :
- `http://localhost:3000` (dev)
- `http://localhost:3001` (dev alt port)

### Authentification Backend

```
Method: Bearer Token (Laravel Sanctum)
Header: Authorization: Bearer {token}
```

---

## 📝 Convention de Code

### Nommage

- Composants : PascalCase (`AdminDashboard.jsx`)
- Fonctions : camelCase (`handleSubmit()`)
- Variables : camelCase (`isLoading`)
- Constants : UPPER_SNAKE_CASE (`MAX_ITEMS`)

### Structure Composant

```javascript
import { useState } from 'react';
import { useAuth } from '../context/AuthContext';
import { apiMethod } from '../services/api';

export default function MyComponent() {
  const [state, setState] = useState(null);
  const { user } = useAuth();

  const handleAction = async () => {
    // logic
  };

  return (
    <div>
      {/* JSX */}
    </div>
  );
}
```

---

## 🐛 Debugging

### Browser DevTools

1. Ouvrir F12
2. Console : Erreurs et logs
3. Network : Requêtes API
4. Application → LocalStorage : Vérifier token

### React DevTools

- Extension Chrome : `React Developer Tools`
- Inspector l'état et props

### Logs Utiles

```javascript
// Vérifier token
console.log(localStorage.getItem('auth_token'));

// Vérifier user
console.log(localStorage.getItem('auth_user'));

// API URL
console.log(import.meta.env.VITE_API_URL);
```

---

## ✅ Checklist Avant Déploiement

- [ ] Vérifier variables env correctes
- [ ] Test login/register fonctionne
- [ ] Admin dashboard charge stats
- [ ] Panier fonctionne
- [ ] CORS configuré
- [ ] Token Sanctum valide
- [ ] Build sans erreurs
- [ ] Images optimisées

---

## 📞 Support & Ressources

- **React Docs** : https://react.dev
- **React Router** : https://reactrouter.com
- **Bootstrap** : https://getbootstrap.com
- **Recharts** : https://recharts.org

---

**Dernière mise à jour** : 20 Mai 2026
