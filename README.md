# 💊 Pharmacy Management System

## Un système de gestion de pharmacie professionnel et complet

> Plateforme moderne pour gérer efficacement une pharmacie avec interface utilisateur élégante, API robuste et base de données sécurisée.

### 🚀 Features Principales

- ✅ **Gestion des Stocks** - Suivi en temps réel des médicaments
- ✅ **Gestion des Ventes** - Facturation et historique des transactions
- ✅ **Gestion des Clients** - Profils et historiques d'achats
- ✅ **Gestion des Fournisseurs** - Commandes et réceptions
- ✅ **Rapports & Analytics** - Statistiques détaillées
- ✅ **Authentification Sécurisée** - JWT + Role-based access
- ✅ **Dashboard Interactif** - Vue d'ensemble en temps réel
- ✅ **Responsive Design** - Fonctionne sur tous les appareils

### 🛠️ Stack Technologique

**Backend:**
- Node.js + Express.js
- PostgreSQL
- Sequelize ORM
- JWT Authentication
- Swagger/OpenAPI Documentation

**Frontend:**
- React 18+
- TypeScript
- Tailwind CSS
- Shadcn UI Components
- TanStack Query (React Query)
- Zod Validation

**DevOps:**
- Docker & Docker Compose
- ESLint & Prettier
- Jest Testing
- GitHub Actions CI/CD

### 📁 Structure du Projet

```
pharmacy-management/
├── backend/              # API REST Node.js
├── frontend/             # Application React
├── docker-compose.yml    # Configuration Docker
├── docs/                 # Documentation
└── README.md             # Ce fichier
```

### 🚀 Démarrage Rapide

#### Avec Docker (Recommandé)

```bash
git clone https://github.com/Olamide10-colab/pharmacy-management.git
cd pharmacy-management
docker-compose up -d
```

**Accès:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- API Docs: http://localhost:5000/api/docs

#### Installation Locale

**Backend:**
```bash
cd backend
npm install
# Configurer .env
cp .env.example .env
# Migrations base de données
sequelize db:migrate
# Démarrer le serveur
npm run dev
```

**Frontend:**
```bash
cd frontend
npm install
# Configurer .env
cp .env.example .env
# Démarrer l'application
npm start
```

### 📚 Documentation

- [Architecture du Projet](./docs/ARCHITECTURE.md)
- [Guide API](./docs/API.md)
- [Setup Base de Données](./docs/DATABASE.md)
- [Guide de Contribution](./CONTRIBUTING.md)

### 👨‍💻 Variables d'Environnement

**Backend (.env):**
```env
NODE_ENV=development
PORT=5000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pharmacy_db
DB_USER=pharmacy_user
DB_PASSWORD=secure_password
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRY=7d
```

**Frontend (.env):**
```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_ENV=development
```

### 🔐 Authentification

Le système utilise JWT pour l'authentification:
- Email + Mot de passe pour le login
- Tokens stockés sécurisés en localStorage
- Refresh tokens pour les sessions longues
- Role-based access control (RBAC)

### 📊 Rôles Utilisateurs

- **Admin** - Accès complet au système
- **Pharmacien** - Gestion des stocks et ventes
- **Caissier** - Transactions et facturation
- **Gestionnaire Stock** - Gestion des stocks
- **Visualiseur** - Lecture seule

### 🧪 Tests

```bash
# Backend tests
cd backend
npm run test
npm run test:coverage

# Frontend tests
cd frontend
npm run test
npm run test:coverage
```

### 📋 Scripts Disponibles

**Backend:**
- `npm run dev` - Mode développement avec hot reload
- `npm start` - Mode production
- `npm run test` - Exécuter les tests
- `npm run lint` - Vérifier le linting
- `npm run format` - Formater le code

**Frontend:**
- `npm start` - Démarrer l'application
- `npm run build` - Build pour production
- `npm run test` - Tests
- `npm run lint` - Linting
- `npm run type-check` - Vérifier les types TypeScript

### 🐳 Docker

```bash
# Build images
docker-compose build

# Démarrer les services
docker-compose up -d

# Voir les logs
docker-compose logs -f

# Arrêter les services
docker-compose down
```

### 🤝 Contribution

Les contributions sont bienvenues! Consultez [CONTRIBUTING.md](./CONTRIBUTING.md) pour les détails.

### 📄 Licence

Ce projet est sous licence MIT. Voir [LICENSE](./LICENSE) pour plus de détails.

### 📞 Support

Pour les questions ou problèmes, veuillez créer une [Issue](https://github.com/Olamide10-colab/pharmacy-management/issues).

---

**Créé avec ❤️ pour une meilleure gestion de pharmacie**
