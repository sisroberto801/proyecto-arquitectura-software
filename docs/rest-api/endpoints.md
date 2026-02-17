markdown
# API REST - Endpoints

## Base URL
`https://api.sistema.com/v1`


## Autenticación
Authorization: Bearer <token-jwt>

text

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
  "pagination": { "page": 1, "total": 50 }
}
GET /users/{id}
Detalle de usuario.

Response 200:

json
{
  "id": "usr_123",
  "name": "Juan Pérez",
  "email": "juan@email.com",
  "role": "admin",
  "createdAt": "2024-01-15T10:30:00Z"
}
POST /users
Crear usuario.

Request:

json
{
  "name": "Carlos López",
  "email": "carlos@email.com",
  "password": "Secure123",
  "role": "user"
}
PUT /users/{id}
Actualizar usuario.

DELETE /users/{id}
Eliminar usuario.

Productos
GET /products
Lista productos.

Filtros: category, minPrice, maxPrice, inStock

Response:

json
{
  "data": [
    {
      "id": "prod_123",
      "name": "Laptop Pro",
      "price": 1299.99,
      "category": "electronics",
      "stock": 15
    }
  ]
}
POST /products
Crear producto.

Request:

json
{
  "name": "Smartphone Z5",
  "price": 699.99,
  "category": "electronics",
  "stock": 50
}
GET /products/{id}
Detalle de producto.

PUT /products/{id}
Actualizar producto.

DELETE /products/{id}
Eliminar producto.

Códigos HTTP
Código	Descripción
200	OK
201	Creado
400	Error de validación
401	No autorizado
403	Prohibido
404	No encontrado
500	Error interno