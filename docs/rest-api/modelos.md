# Modelos de Datos - API REST

## Modelo Usuario

| Campo | Tipo | Requerido | Único | Descripción |
|-------|------|-----------|-------|-------------|
| id | UUID | Sí | Sí | Identificador único |
| name | String | Sí | No | Nombre completo |
| email | String | Sí | Sí | Correo electrónico |
| password | String | Sí | No | Contraseña (hash) |
| role | Enum | No | No | admin / user |
| active | Boolean | No | No | Estado de la cuenta |
| createdAt | DateTime | Sí | No | Fecha de creación |
| updatedAt | DateTime | Sí | No | Fecha de actualización |

**Ejemplo:**
```json
{
  "id": "usr_123abc",
  "name": "Juan Pérez",
  "email": "juan@email.com",
  "password": "$2a$10$Xk89Pf2K",
  "role": "admin",
  "active": true,
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T10:30:00Z"
}
```

## Modelo Producto

| Campo | Tipo | Requerido | Único | Descripción |
|-------|------|-----------|-------|-------------|
| id | UUID | Sí | Sí | Identificador único |
| name | String | Sí | No | Nombre del producto |
| description | Text | No | No | Descripción detallada |
| price | Float | Sí | No | Precio |
| category | Enum | Sí | No | electronics / clothing / books / food / other |
| stock | Integer | Sí | No | Cantidad disponible |
| images | Array | No | No | URLs de imágenes |
| specs | JSON | No | No | Especificaciones técnicas |
| createdAt | DateTime | Sí | No | Fecha de creación |
| updatedAt | DateTime | Sí | No | Fecha de actualización |

**Ejemplo:**
```json
{
  "id": "prod_123abc",
  "name": "Laptop Pro X1",
  "description": "Laptop de alta gama",
  "price": 1299.99,
  "category": "electronics",
  "stock": 15,
  "images": ["img1.jpg", "img2.jpg"],
  "specs": {
    "brand": "TechBrand",
    "model": "X1",
    "ram": "16GB"
  },
  "createdAt": "2024-01-10T08:00:00Z"
}
```

## Modelo Categoría

| Campo | Tipo | Requerido | Único | Descripción |
|-------|------|-----------|-------|-------------|
| id | UUID | Sí | Sí | Identificador único |
| name | String | Sí | Sí | Nombre de categoría |
| description | Text | No | No | Descripción |
| active | Boolean | No | No | Estado |

**Ejemplo:**
```json
{
  "id": "cat_123abc",
  "name": "Electronics",
  "description": "Productos electrónicos",
  "active": true
}
```

## Relaciones

- **Usuario → Productos**: Un usuario puede tener muchos productos (relación 1:N)
- **Categoría → Productos**: Una categoría puede tener muchos productos (relación 1:N)

## Validaciones

### Usuario
- `name`: Mínimo 3 caracteres, máximo 100
- `email`: Formato email válido
- `password`: Mínimo 8 caracteres, 1 mayúscula, 1 número
- `role`: Solo 'admin' o 'user'

### Producto
- `name`: Mínimo 3 caracteres, máximo 200
- `price`: Mayor a 0
- `stock`: Mayor o igual a 0
- `category`: Debe existir en el enum