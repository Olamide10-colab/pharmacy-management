# Database Setup - Pharmacy Management System

## Vue d'ensemble

- **DBMS:** PostgreSQL 15+
- **ORM:** Sequelize
- **Migrations:** Sequelize CLI

## Installation

### 1. PostgreSQL Installation

**macOS:**
```bash
brew install postgresql@15
brew services start postgresql@15
```

**Ubuntu/Debian:**
```bash
sudo apt-get install postgresql postgresql-contrib
sudo systemctl start postgresql
```

**Windows:**
Télécharger depuis https://www.postgresql.org/download/windows/

### 2. Créer l'Utilisateur et la Base de Données

```sql
-- Connect as postgres user
sudo -u postgres psql

-- Create user
CREATE USER pharmacy_user WITH PASSWORD 'secure_password';

-- Create database
CREATE DATABASE pharmacy_db OWNER pharmacy_user;

-- Grant privileges
ALTER ROLE pharmacy_user WITH SUPERUSER;

-- Exit
\q
```

### 3. Configuration Locale

**Backend/.env:**
```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=pharmacy_db
DB_USER=pharmacy_user
DB_PASSWORD=secure_password
DB_DIALECT=postgres
```

## Migrations

### Exécuter les Migrations

```bash
cd backend
npm install

# Exécuter toutes les migrations
sequelize db:migrate

# Exécuter une migration spécifique
sequelize db:migrate --name 20240101120000-create-users.js

# Annuler la dernière migration
sequelize db:migrate:undo

# Annuler toutes les migrations
sequelize db:migrate:undo:all
```

### Créer une Nouvelle Migration

```bash
sequelize migration:generate --name create-products
```

## Schéma de la Base de Données

### Tables Principales

#### users
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  firstName VARCHAR(100) NOT NULL,
  lastName VARCHAR(100) NOT NULL,
  phone VARCHAR(20),
  role VARCHAR(50) DEFAULT 'user',
  isActive BOOLEAN DEFAULT true,
  lastLogin TIMESTAMP,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

#### products (Médicaments)
```sql
CREATE TABLE products (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  description TEXT,
  sku VARCHAR(50) UNIQUE NOT NULL,
  category VARCHAR(100),
  quantity INTEGER NOT NULL DEFAULT 0,
  minQuantity INTEGER DEFAULT 10,
  maxQuantity INTEGER,
  price DECIMAL(10, 2) NOT NULL,
  costPrice DECIMAL(10, 2),
  expiryDate DATE,
  supplierId UUID REFERENCES suppliers(id),
  isActive BOOLEAN DEFAULT true,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_products_sku ON products(sku);
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_products_supplier ON products(supplierId);
CREATE INDEX idx_products_expiry ON products(expiryDate);
```

#### customers
```sql
CREATE TABLE customers (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  phone VARCHAR(20),
  address TEXT,
  city VARCHAR(100),
  zipCode VARCHAR(20),
  loyaltyPoints INTEGER DEFAULT 0,
  totalPurchases DECIMAL(10, 2) DEFAULT 0,
  isActive BOOLEAN DEFAULT true,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_phone ON customers(phone);
```

#### sales
```sql
CREATE TABLE sales (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  saleNumber VARCHAR(50) UNIQUE NOT NULL,
  customerId UUID REFERENCES customers(id),
  userId UUID NOT NULL REFERENCES users(id),
  totalAmount DECIMAL(10, 2) NOT NULL,
  taxAmount DECIMAL(10, 2) DEFAULT 0,
  paymentMethod VARCHAR(50),
  paymentStatus VARCHAR(50) DEFAULT 'completed',
  status VARCHAR(50) DEFAULT 'completed',
  notes TEXT,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_sales_number ON sales(saleNumber);
CREATE INDEX idx_sales_date ON sales(createdAt);
CREATE INDEX idx_sales_customer ON sales(customerId);
CREATE INDEX idx_sales_user ON sales(userId);
```

#### sale_items
```sql
CREATE TABLE sale_items (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  saleId UUID NOT NULL REFERENCES sales(id) ON DELETE CASCADE,
  productId UUID NOT NULL REFERENCES products(id),
  quantity INTEGER NOT NULL,
  unitPrice DECIMAL(10, 2) NOT NULL,
  subtotal DECIMAL(10, 2) NOT NULL,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_sale_items_sale ON sale_items(saleId);
CREATE INDEX idx_sale_items_product ON sale_items(productId);
```

#### suppliers
```sql
CREATE TABLE suppliers (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255),
  phone VARCHAR(20),
  address TEXT,
  city VARCHAR(100),
  zipCode VARCHAR(20),
  contactPerson VARCHAR(255),
  isActive BOOLEAN DEFAULT true,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_suppliers_name ON suppliers(name);
```

#### purchase_orders
```sql
CREATE TABLE purchase_orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  poNumber VARCHAR(50) UNIQUE NOT NULL,
  supplierId UUID NOT NULL REFERENCES suppliers(id),
  totalAmount DECIMAL(10, 2) NOT NULL,
  status VARCHAR(50) DEFAULT 'pending',
  expectedDeliveryDate DATE,
  deliveryDate DATE,
  notes TEXT,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_purchase_orders_number ON purchase_orders(poNumber);
CREATE INDEX idx_purchase_orders_status ON purchase_orders(status);
```

## Seeding de Données

### Créer un Seeder

```bash
sequelize seed:generate --name demo-data
```

### Exécuter les Seeders

```bash
# Exécuter tous les seeders
sequelize db:seed:all

# Exécuter un seeder spécifique
sequelize db:seed --seed 20240101120000-demo-data.js

# Annuler les seeders
sequelize db:seed:undo:all
```

## Backup et Restore

### Backup

```bash
# Backup complet
pg_dump -U pharmacy_user pharmacy_db > backup.sql

# Backup avec options
pg_dump -U pharmacy_user -F c -b -v pharmacy_db > backup.dump
```

### Restore

```bash
# Depuis un fichier SQL
psql -U pharmacy_user pharmacy_db < backup.sql

# Depuis un dump binaire
pg_restore -U pharmacy_user -d pharmacy_db -v backup.dump
```

## Performance

### Indexes Recommandés

Déjà créés ci-dessus, mais voici quelques optimisations additionnelles:

```sql
-- Recherche rapide sur les dates
CREATE INDEX idx_sales_date_range ON sales(createdAt DESC);

-- Ventes par client
CREATE INDEX idx_sales_customer_date ON sales(customerId, createdAt);

-- Produits par catégorie et stock
CREATE INDEX idx_products_category_stock ON products(category, quantity);
```

### Analyzing Performance

```sql
-- Voir le plan d'exécution
EXPLAIN ANALYZE
SELECT * FROM sales WHERE createdAt > NOW() - INTERVAL '30 days';

-- Vacuum pour nettoyer
VACUUM ANALYZE;
```

## Monitoring

```sql
-- Voir la taille des tables
SELECT
  relname AS table,
  pg_size_pretty(pg_total_relation_size(relid)) AS size
FROM pg_catalog.pg_statio_user_tables
ORDER BY pg_total_relation_size(relid) DESC;

-- Connexions actives
SELECT count(*) FROM pg_stat_activity WHERE state = 'active';
```

## Troubleshooting

### Connexion Refused

```bash
# Vérifier que PostgreSQL est lancé
sudo systemctl status postgresql

# Relancer
sudo systemctl restart postgresql
```

### Migration Failed

```bash
# Voir l'état des migrations
sequelize db:migrate:status

# Annuler et relancer
sequelize db:migrate:undo
sequelize db:migrate
```

### Reset Complet

```bash
# Attention: Cela supprime tout!
sequelize db:drop
sequelize db:create
sequelize db:migrate
sequelize db:seed:all
```
