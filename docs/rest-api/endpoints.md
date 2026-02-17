# Documentación de API REST - Sistema de Gestión de Usuarios y Productos

## Descripción General
API RESTful diseñada para la gestión completa de usuarios y productos en un sistema de comercio electrónico. Implementa operaciones CRUD con autenticación JWT y sigue las mejores prácticas de diseño de APIs REST.

## Base URL
https://api.sistema.com/v1

## Autenticación
La API utiliza tokens JWT (JSON Web Tokens) para autenticar y autorizar las peticiones.

### Obtención de Token
POST /auth/login
Content-Type: application/json

{
"email": "usuario@email.com",
"password": "contraseña123"
}

**Response 200:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expiresIn": 3600
}
Uso del Token
Incluir el token en el header de todas las peticiones autenticadas:

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Endpoints de Usuarios
1. Listar Usuarios
Obtiene una lista paginada de todos los usuarios del sistema.

GET /users

Headers:
Authorization: Bearer <token>
Parámetros Query (opcionales):

Parámetro	Tipo	Descripción
page	number	Número de página (default: 1)
limit	number	Items por página (default: 10, max: 100)
role	string	Filtrar por rol (admin/user)
search	string	Búsqueda por nombre o email
Response 200 - Éxito:

json
{
  "success": true,
  "data": [
    {
      "id": "usr_123abc",
      "name": "Juan Pérez",
      "email": "juan.perez@email.com",
      "role": "admin",
      "active": true,
      "createdAt": "2024-01-15T10:30:00Z",
      "updatedAt": "2024-01-15T10:30:00Z"
    },
    {
      "id": "usr_456def",
      "name": "María García",
      "email": "maria.garcia@email.com",
      "role": "user",
      "active": true,
      "createdAt": "2024-01-16T14:20:00Z",
      "updatedAt": "2024-01-16T14:20:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 45,
    "totalPages": 5
  }
}
Response 401 - No autorizado:

json
{
  "success": false,
  "error": "Token inválido o expirado",
  "code": "UNAUTHORIZED"
}
2. Obtener Usuario por ID
Retorna los detalles de un usuario específico.

GET /users/{id}

Headers:


Authorization: Bearer <token>
Parámetros URL:

Parámetro	Tipo	Descripción
id	string	ID único del usuario
Response 200 - Éxito:

json
{
  "success": true,
  "data": {
    "id": "usr_123abc",
    "name": "Juan Pérez",
    "email": "juan.perez@email.com",
    "role": "admin",
    "active": true,
    "addresses": [
      {
        "street": "Av. Principal 123",
        "city": "Ciudad de México",
        "country": "México",
        "zipCode": "12345"
      }
    ],
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z"
  }
}
Response 404 - No encontrado:

json
{
  "success": false,
  "error": "Usuario no encontrado",
  "code": "USER_NOT_FOUND"
}
3. Crear Usuario
Registra un nuevo usuario en el sistema.

POST /users

Headers:


Authorization: Bearer <token>
Content-Type: application/json
Request Body:

json
{
  "name": "Carlos López",
  "email": "carlos.lopez@email.com",
  "password": "SecurePass123!",
  "role": "user",
  "active": true
}
Validaciones:

name: Requerido, mínimo 3 caracteres

email: Requerido, formato email válido, único en sistema

password: Requerido, mínimo 8 caracteres, al menos 1 mayúscula y 1 número

role: Opcional, valores permitidos: "admin" | "user" (default: "user")

Response 201 - Creado:

json
{
  "success": true,
  "message": "Usuario creado exitosamente",
  "data": {
    "id": "usr_789ghi",
    "name": "Carlos López",
    "email": "carlos.lopez@email.com",
    "role": "user",
    "active": true,
    "createdAt": "2024-01-20T09:15:00Z"
  }
}
Response 400 - Error de validación:

json
{
  "success": false,
  "error": "Error de validación",
  "details": [
    {
      "field": "email",
      "message": "El email ya está registrado"
    },
    {
      "field": "password",
      "message": "La contraseña debe tener al menos 8 caracteres"
    }
  ],
  "code": "VALIDATION_ERROR"
}
4. Actualizar Usuario
Modifica los datos de un usuario existente.

PUT /users/{id}

Headers:


Authorization: Bearer <token>
Content-Type: application/json
Request Body:

json
{
  "name": "Carlos López Martínez",
  "email": "carlos.lopez.nuevo@email.com",
  "role": "admin",
  "active": true
}
Nota: El campo password no se actualiza mediante este endpoint (usar endpoint específico para cambio de contraseña)

Response 200 - Actualizado:

json
{
  "success": true,
  "message": "Usuario actualizado exitosamente",
  "data": {
    "id": "usr_789ghi",
    "name": "Carlos López Martínez",
    "email": "carlos.lopez.nuevo@email.com",
    "role": "admin",
    "active": true,
    "updatedAt": "2024-01-20T10:30:00Z"
  }
}
5. Eliminar Usuario
Realiza un borrado lógico (soft delete) del usuario.

DELETE /users/{id}

Headers:


Authorization: Bearer <token>
Response 200 - Eliminado:

json
{
  "success": true,
  "message": "Usuario eliminado exitosamente"
}
Endpoints de Productos
1. Listar Productos
Obtiene el catálogo de productos con filtros avanzados.

GET /products

Headers:


Authorization: Bearer <token>
Parámetros Query:

Parámetro	Tipo	Descripción
page	number	Número de página
limit	number	Items por página
category	string	Filtrar por categoría
minPrice	number	Precio mínimo
maxPrice	number	Precio máximo
inStock	boolean	Solo productos con stock
Response 200:

json
{
  "success": true,
  "data": [
    {
      "id": "prod_123xyz",
      "name": "Laptop Pro X1",
      "description": "Laptop de alta gama para profesionales",
      "price": 1299.99,
      "category": "electronics",
      "stock": 15,
      "images": ["laptop1.jpg", "laptop2.jpg"],
      "createdAt": "2024-01-10T08:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 150,
    "totalPages": 15
  }
}
2. Crear Producto
Agrega un nuevo producto al catálogo.

POST /products

Headers:


Authorization: Bearer <token>
Content-Type: application/json
Request Body:

json
{
  "name": "Smartphone Z5",
  "description": "Teléfono inteligente con cámara de 108MP",
  "price": 699.99,
  "category": "electronics",
  "stock": 50,
  "images": ["phone1.jpg", "phone2.jpg"],
  "specs": {
    "brand": "TechBrand",
    "model": "Z5",
    "color": "Negro"
  }
}
Response 201:

json
{
  "success": true,
  "message": "Producto creado exitosamente",
  "data": {
    "id": "prod_456abc",
    "name": "Smartphone Z5",
    "price": 699.99,
    "stock": 50,
    "createdAt": "2024-01-20T11:00:00Z"
  }
}
3. Actualizar Producto
Modifica los datos de un producto existente.

PUT /products/{id}

Headers:


Authorization: Bearer <token>
Content-Type: application/json
Request Body:

json
{
  "name": "Smartphone Z5 Pro",
  "price": 799.99,
  "stock": 35,
  "category": "electronics"
}
Response 200:

json
{
  "success": true,
  "message": "Producto actualizado exitosamente",
  "data": {
    "id": "prod_456abc",
    "name": "Smartphone Z5 Pro",
    "price": 799.99,
    "stock": 35,
    "updatedAt": "2024-01-20T11:30:00Z"
  }
}
4. Eliminar Producto
Elimina un producto del catálogo.

DELETE /products/{id}

Headers:


Authorization: Bearer <token>
Response 200:

json
{
  "success": true,
  "message": "Producto eliminado exitosamente"
}
Códigos de Estado HTTP
Código	Descripción	Casos de uso
200	OK	Petición exitosa (GET, PUT, DELETE)
201	Created	Recurso creado exitosamente (POST)
400	Bad Request	Error de validación en datos enviados
401	Unauthorized	Token no proporcionado o inválido
403	Forbidden	Token válido pero sin permisos suficientes
404	Not Found	Recurso no encontrado
409	Conflict	Conflicto con estado actual (ej. email duplicado)
422	Unprocessable Entity	Datos válidos pero no procesables
429	Too Many Requests	Límite de peticiones excedido
500	Internal Server Error	Error interno del servidor