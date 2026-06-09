# API Documentation - Pharmacy Management System

## Base URL

```
http://localhost:5000/api
```

## Authentication

Tous les endpoints (sauf login/register) nécessitent un JWT token:

```
Header: Authorization: Bearer <token>
```

## Response Format

**Success (2xx):**
```json
{
  "success": true,
  "data": { ... },
  "message": "Operation successful"
}
```

**Error (4xx/5xx):**
```json
{
  "success": false,
  "error": "ERROR_CODE",
  "message": "Error description",
  "details": { ... }
}
```

## Endpoints

### Auth Endpoints

#### Register User
```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+33612345678"
}

Response: 201
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "John",
    "role": "user"
  },
  "token": "jwt_token"
}
```

#### Login
```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "SecurePass123!"
}

Response: 200
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "John",
    "role": "admin"
  },
  "token": "jwt_token"
}
```

#### Get Profile
```
GET /auth/profile
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "role": "admin",
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

### Products (Médicaments)

#### List Products
```
GET /products?page=1&limit=20&search=aspirin
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "name": "Aspirin 500mg",
      "sku": "ASP-500-001",
      "quantity": 150,
      "minQuantity": 50,
      "price": 2.50,
      "expiryDate": "2025-12-31",
      "supplier": { "id": "uuid", "name": "Supplier Inc" },
      "createdAt": "2024-01-01T00:00:00Z"
    }
  ],
  "pagination": {
    "total": 100,
    "page": 1,
    "pages": 5,
    "limit": 20
  }
}
```

#### Get Product
```
GET /products/:id
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Aspirin 500mg",
    "description": "Pain reliever and fever reducer",
    "sku": "ASP-500-001",
    "quantity": 150,
    "minQuantity": 50,
    "price": 2.50,
    "expiryDate": "2025-12-31",
    "supplier": { ... },
    "createdAt": "2024-01-01T00:00:00Z"
  }
}
```

#### Create Product
```
POST /products
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Aspirin 500mg",
  "description": "Pain reliever",
  "sku": "ASP-500-001",
  "quantity": 100,
  "minQuantity": 50,
  "price": 2.50,
  "expiryDate": "2025-12-31",
  "supplierId": "uuid"
}

Response: 201
{ "success": true, "data": { ... } }
```

#### Update Product
```
PUT /products/:id
Authorization: Bearer <token>

{ "quantity": 200, "price": 2.75 }

Response: 200
{ "success": true, "data": { ... } }
```

#### Delete Product
```
DELETE /products/:id
Authorization: Bearer <token>

Response: 204
```

### Sales (Ventes)

#### Create Sale
```
POST /sales
Authorization: Bearer <token>
Content-Type: application/json

{
  "customerId": "uuid" (optional),
  "items": [
    {
      "productId": "uuid",
      "quantity": 2,
      "unitPrice": 2.50
    }
  ],
  "paymentMethod": "cash", // cash, card, check
  "notes": "Customer notes"
}

Response: 201
{
  "success": true,
  "data": {
    "id": "uuid",
    "saleNumber": "SALE-2024-001",
    "items": [ ... ],
    "totalAmount": 5.00,
    "paymentMethod": "cash",
    "status": "completed",
    "createdAt": "2024-01-01T10:30:00Z"
  }
}
```

#### Get Sales
```
GET /sales?page=1&limit=20&from=2024-01-01&to=2024-01-31
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": [ ... ],
  "pagination": { ... }
}
```

#### Get Sale Details
```
GET /sales/:id
Authorization: Bearer <token>

Response: 200
{ "success": true, "data": { ... } }
```

### Customers

#### Create Customer
```
POST /customers
Authorization: Bearer <token>

{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+33612345678",
  "address": "123 Main St",
  "city": "Paris",
  "zipCode": "75001"
}

Response: 201
```

#### List Customers
```
GET /customers?page=1&limit=20&search=john
Authorization: Bearer <token>

Response: 200
```

### Reports

#### Sales Report
```
GET /reports/sales?from=2024-01-01&to=2024-01-31
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": {
    "totalSales": 1500.00,
    "totalTransactions": 45,
    "averageTransaction": 33.33,
    "topProducts": [ ... ],
    "paymentMethods": { ... },
    "dailySales": [ ... ]
  }
}
```

#### Inventory Report
```
GET /reports/inventory
Authorization: Bearer <token>

Response: 200
{
  "success": true,
  "data": {
    "totalProducts": 120,
    "lowStockProducts": 15,
    "expiredProducts": 2,
    "stockValue": 25000.00
  }
}
```

### Suppliers

#### Create Supplier
```
POST /suppliers
Authorization: Bearer <token>

{
  "name": "Pharma Supplier Inc",
  "email": "contact@pharma.com",
  "phone": "+33123456789",
  "address": "456 Business Ave",
  "city": "Lyon"
}

Response: 201
```

#### List Suppliers
```
GET /suppliers?page=1&limit=20
Authorization: Bearer <token>

Response: 200
```

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| `INVALID_CREDENTIALS` | 401 | Email ou mot de passe incorrect |
| `TOKEN_EXPIRED` | 401 | Le token JWT a expiré |
| `UNAUTHORIZED` | 403 | Accès non autorisé |
| `NOT_FOUND` | 404 | Ressource non trouvée |
| `VALIDATION_ERROR` | 400 | Données invalides |
| `DUPLICATE_EMAIL` | 409 | Email déjà utilisé |
| `INSUFFICIENT_STOCK` | 400 | Stock insuffisant |
| `SERVER_ERROR` | 500 | Erreur serveur |

## Rate Limiting

```
100 requests per 15 minutes per IP
Header: X-RateLimit-Remaining
```

## API Documentation Interactive

La documentation Swagger est disponible à:
```
http://localhost:5000/api/docs
```
