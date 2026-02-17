# API REST - Endpoints

## Base URL
`https://api.sistema.com/v1`

## Autenticación
```
Authorization: Bearer <token-jwt>
```

---

## Usuarios

### `GET /users`
Lista usuarios paginados.

**Query params:** `page`, `limit`, `role`, `search`

**Response 200:**
```json
{
  "data": [
    {
      "id": "usr_123",
      "name": "Juan Pérez",
      "email": "juan@email.com",
      "role": "admin"
    }
  ],
  "pagination": {
    "page": 1,
    "total": 50
  }
}
```

### `GET /users/{id}`
Detalle de usuario.

**Response 200:**
```json
{
  "id": "usr_123",
  "name": "Juan Pérez",
  "email": "juan@email.com",
  "role": "admin",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### `POST /users`
Crear usuario.

**Request:**
```json
{
  "name": "Carlos López",
  "email": "carlos@email.com",
  "password": "Secure123",
  "role": "user"
}
```

**Response 201:**
```json
{
  "id": "usr_456",
  "name": "Carlos López",
  "email": "carlos@email.com",
  "role": "user",
  "createdAt": "2024-01-20T09:15:00Z"
}
```

### `PUT /users/{id}`
Actualizar usuario.

**Request:**
```json
{
  "name": "Carlos López Martínez",
  "email": "carlos.nuevo@email.com",
  "role": "admin"
}
```

**Response 200:**
```json
{
  "id": "usr_456",
  "name": "Carlos López Martínez",
  "email": "carlos.nuevo@email.com",
  "role": "admin",
  "updatedAt": "2024-01-20T10:30:00Z"
}
```

### `DELETE /users/{id}`
Eliminar usuario.

**Response 200:**
```json
{
  "message": "Usuario eliminado exitosamente"
}
```

---

## Productos

### `GET /products`
Lista productos.

**Filtros:** `category`, `minPrice`, `maxPrice`, `inStock`

**Response 200:**
```json
{
  "data": [
    {
      "id": "prod_123",
      "name": "Laptop Pro",
      "price": 1299.99,
      "category": "electronics",
      "stock": 15
    }
  ],
  "pagination": {
    "page": 1,
    "total": 45
  }
}
```

### `GET /products/{id}`
Detalle de producto.

**Response 200:**
```json
{
  "id": "prod_123",
  "name": "Laptop Pro",
  "description": "Laptop de alta gama",
  "price": 1299.99,
  "category": "electronics",
  "stock": 15,
  "createdAt": "2024-01-10T08:00:00Z"
}
```

### `POST /products`
Crear producto.

**Request:**
```json
{
  "name": "Smartphone Z5",
  "price": 699.99,
  "category": "electronics",
  "stock": 50,
  "description": "Teléfono inteligente"
}
```

**Response 201:**
```json
{
  "id": "prod_456",
  "name": "Smartphone Z5",
  "price": 699.99,
  "category": "electronics",
  "stock": 50,
  "createdAt": "2024-01-20T11:00:00Z"
}
```

### `PUT /products/{id}`
Actualizar producto.

**Request:**
```json
{
  "name": "Smartphone Z5 Pro",
  "price": 799.99,
  "stock": 35
}
```

**Response 200:**
```json
{
  "id": "prod_456",
  "name": "Smartphone Z5 Pro",
  "price": 799.99,
  "stock": 35,
  "updatedAt": "2024-01-20T11:30:00Z"
}
```

### `DELETE /products/{id}`
Eliminar producto.

**Response 200:**
```json
{
  "message": "Producto eliminado exitosamente"
}
```

---

## Códigos HTTP

| Código | Descripción |
|--------|-------------|
| 200 | OK |
| 201 | Creado |
| 400 | Error de validación |
| 401 | No autorizado |
| 403 | Prohibido |
| 404 | No encontrado |
| 500 | Error interno |

---

## Ejemplos cURL

```bash
# Login
curl -X POST https://api.sistema.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@email.com","password":"123"}'

# Listar productos (autenticado)
curl -X GET https://api.sistema.com/v1/products \
  -H "Authorization: Bearer <token-jwt>"

# Crear usuario
curl -X POST https://api.sistema.com/v1/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Juan","email":"juan@email.com","password":"Secure123"}'
```