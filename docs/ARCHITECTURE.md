# Architecture du Système - Pharmacy Management

## Vue d'ensemble

```
┌─────────────────────────────────────────────────────────────┐
│                    Client (React + TypeScript)               │
│              Tailwind CSS + Shadcn UI Components             │
└────────────────────┬────────────────────────────────────────┘
                     │
                     │ HTTP/REST
                     ▼
┌─────────────────────────────────────────────────────────────┐
│           API Gateway (Express.js Middleware)               │
│         Authentication, Validation, Error Handling           │
└────────────────────┬────────────────────────────────────────┘
                     │
        ┌────────────┼────────────┬───────────────┐
        │            │            │               │
        ▼            ▼            ▼               ▼
    ┌────────┐  ┌────────┐  ┌────────┐  ┌────────────┐
    │ Routes │  │ Services│ │Models │  │Middleware │
    └────────┘  └────────┘  └────────┘  └────────────┘
        │            │            │
        └────────────┼────────────┘
                     ▼
    ┌─────────────────────────────────┐
    │      PostgreSQL Database        │
    │   (Sequelize ORM - TypeORM)     │
    │  Tables: Users, Products,       │
    │  Sales, Customers, Suppliers... │
    └─────────────────────────────────┘
```

## Architecture en Couches

### 1. **Couche de Présentation (Frontend)**

```
frontend/
├── src/
│   ├── components/
│   │   ├── common/           # Composants réutilisables
│   │   ├── layouts/          # Layouts principaux
│   │   └── features/         # Composants par feature
│   ├── pages/                # Pages/Routes
│   ├── services/             # API calls
│   ├── hooks/                # Custom React hooks
│   ├── stores/               # State management (Zustand)
│   ├── types/                # TypeScript types
│   ├── utils/                # Fonctions utilitaires
│   ├── styles/               # Styles globaux
│   └── App.tsx               # Root component
```

**Technologies:**
- React 18 avec Hooks
- TypeScript pour la typage
- Tailwind CSS pour le styling
- Shadcn UI pour les composants
- TanStack Query pour le data fetching
- Zod pour la validation
- React Router pour la navigation

### 2. **Couche API (Backend)**

```
backend/
├── src/
│   ├── routes/               # Définition des endpoints
│   ├── controllers/          # Logique des endpoints
│   ├── services/             # Logique métier
│   ├── models/               # Modèles Sequelize
│   ├── middleware/           # Custom middleware
│   ├── validators/           # Validation des données
│   ├── utils/                # Fonctions utilitaires
│   ├── config/               # Configuration
│   ├── database/             # Migrations & seeds
│   └── app.ts                # Express app setup
```

**Endpoints Principaux:**
- `POST /api/auth/register` - Inscription
- `POST /api/auth/login` - Connexion
- `GET /api/auth/profile` - Profil utilisateur
- `GET /api/products` - Lister les médicaments
- `POST /api/sales` - Créer une vente
- `GET /api/reports` - Récupérer les rapports
- `GET /api/inventory` - Gestion des stocks

### 3. **Couche de Base de Données**

**Modèle de Données:**

```sql
users
├── id (PK)
├── email (UNIQUE)
├── password (encrypted)
├── firstName
├── lastName
├── role (Admin, Pharmacist, Cashier, etc.)
└── timestamps

products (Médicaments)
├── id (PK)
├── name
├── description
├── sku (UNIQUE)
├── quantity (Stock actuel)
├── minQuantity (Stock minimum)
├── price
├── expiryDate
├── supplier_id (FK)
└── timestamps

sales (Ventes/Transactions)
├── id (PK)
├── saleNumber (UNIQUE)
├── customer_id (FK)
├── user_id (FK)
├── totalAmount
├── paymentMethod
├── status
└── timestamps

sale_items
├── id (PK)
├── sale_id (FK)
├── product_id (FK)
├── quantity
├── unitPrice
└── subtotal

customers
├── id (PK)
├── name
├── email
├── phone
├── address
├── loyaltyPoints
└── timestamps

suppliers
├── id (PK)
├── name
├── email
├── phone
├── address
└── timestamps

purchase_orders
├── id (PK)
├── poNumber (UNIQUE)
├── supplier_id (FK)
├── totalAmount
├── status
└── timestamps
```

## Patterns et Principes

### 1. **MVC Pattern**
- Routes → Controllers → Services → Models
- Séparation des responsabilités
- Code réutilisable

### 2. **Dependency Injection**
- Services injectés dans les controllers
- Facilite les tests unitaires

### 3. **Error Handling**
- Custom error classes
- Middleware centralisé d'erreurs
- Réponses d'erreur standardisées

### 4. **Authentication & Authorization**
- JWT tokens pour l'authentification
- Role-based access control (RBAC)
- Middleware de vérification des tokens

### 5. **Validation des Données**
- Validateurs Joi/Zod
- Validation côté client et serveur
- Types TypeScript pour la sécurité

## Flux de Données

### Exemple: Créer une Vente

```
1. Frontend: User remplit le formulaire
   ↓
2. Validation côté client (Zod)
   ↓
3. POST /api/sales avec les données
   ↓
4. Backend: Route → Controller
   ↓
5. Validation côté serveur
   ↓
6. Service: Logique métier
   - Vérifier le stock
   - Calculer les totaux
   - Créer la transaction
   ↓
7. Model: Sauvegarde en BD
   ↓
8. Response: Retour au frontend
   ↓
9. Frontend: Update UI + Cache
```

## Sécurité

### 1. **Authentication**
- JWT avec expiration
- Refresh tokens
- Secure password hashing (bcrypt)

### 2. **Authorization**
- Middleware de vérification des rôles
- Protection des routes sensibles
- Audit logging

### 3. **Validation**
- Input validation stricte
- XSS protection
- SQL Injection prevention (Sequelize ORM)

### 4. **Communication**
- HTTPS en production
- CORS configuré
- Rate limiting

## Scalabilité

### Frontend
- Code splitting
- Lazy loading des routes
- Optimization des images
- Cache stratégique

### Backend
- Connection pooling pour la BD
- Caching des données
- Pagination des requêtes
- Indexation des tables

## Monitoring & Logging

```
Logs:
- Application logs (Winston)
- Request logs (Morgan)
- Error tracking (Sentry optional)
- Performance monitoring
```

## Deployment

```
Local Development:
  docker-compose up

Production:
  - Docker images
  - Kubernetes (optional)
  - CI/CD avec GitHub Actions
  - Environment-specific configs
```
