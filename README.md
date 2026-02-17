# Sistema de Arquitectura de API - Proyecto Final

## 📋 Descripción General
Repositorio para la documentación técnica de un sistema híbrido que combina REST y GraphQL, siguiendo principios de arquitectura de software moderna. El proyecto simula el entorno de trabajo de un arquitecto de software, organizando la documentación de una API para gestión de usuarios y productos.

## 👤 Datos del Estudiante
- **Nombre:** Roberto Carlos Olguin Ledezma
- **Universidad:** Universidad Simón I. Patiño (USIP) - Escuela de Postgrado
- **Diplomado:** Fullstack Developer - Back End y Front End
- **Módulo:** 3 - Arquitectura de Software y Gestión de Repositorios
- **Docente:** Marco Antonio Avendaño Ajata
- **Fecha:** Febrero 2026

## 📁 Estructura del Proyecto
```
proyecto-arquitectura-software/
├── README.md
└── docs/
    ├── rest-api/
    │   ├── endpoints.md
    │   └── modelos.md
    ├── graphql/
    │   ├── schema.md
    │   └── resolvers.md
    └── arquitectura/
        ├── diagrama.md
        └── decisiones.md
```
---

## 📊 Estado del Proyecto
| Componente | Estado | Rama |
|------------|--------|------|
| REST API Documentation | ✅ Completado | `feature/rest-endpoints` |
| GraphQL Schema | ✅ Completado | `feature/graphql-schema` |
| Arquitectura y Diagramas | ✅ Completado | `feature/architecture-diagram` |

---

## 🔗 Enlaces Rápidos
- [📌 Issues](https://github.com/[usuario]/proyecto-arquitectura-api/issues)
- [🌿 Ramas](https://github.com/[usuario]/proyecto-arquitectura-api/branches)
- [🔄 Pull Requests](https://github.com/[usuario]/proyecto-arquitectura-api/pulls)
- [📁 Documentación REST](./docs/rest-api/)
- [📁 Documentación GraphQL](./docs/graphql/)
- [📁 Documentación Arquitectura](./docs/arquitectura/)

---

## 📚 RESUMEN

### 🟦 REST API

#### `endpoints.md`
Documentación completa de endpoints REST para usuarios y productos.

**Usuarios:**
- `GET /users` - Lista usuarios paginados
- `GET /users/{id}` - Detalle de usuario
- `POST /users` - Crear usuario
- `PUT /users/{id}` - Actualizar usuario
- `DELETE /users/{id}` - Eliminar usuario

**Productos:**
- `GET /products` - Lista productos con filtros
- `GET /products/{id}` - Detalle de producto
- `POST /products` - Crear producto
- `PUT /products/{id}` - Actualizar producto
- `DELETE /products/{id}` - Eliminar producto

**Autenticación:** JWT Bearer Token
**Base URL:** `https://api.sistema.com/v1`

#### `modelos.md`
Modelos de datos con especificaciones de campos, tipos y validaciones.

**Usuario:**
| Campo | Tipo | Requerido |
|-------|------|-----------|
| id | UUID | Sí |
| name | String | Sí |
| email | String | Sí |
| role | Enum | No |

**Producto:**
| Campo | Tipo | Requerido |
|-------|------|-----------|
| id | UUID | Sí |
| name | String | Sí |
| price | Float | Sí |
| category | Enum | Sí |
| stock | Integer | Sí |

---

### 🟩 GraphQL

#### `schema.md`
Esquema GraphQL con tipos, queries, mutations y subscriptions.

**Tipos principales:**
```graphql
type User {
  id: ID!
  name: String!
  email: String!
  products: [Product!]!
}

type Product {
  id: ID!
  name: String!
  price: Float!
  category: ProductCategory!
}
```

**Queries destacadas:**
- `user(id: ID!): User`
- `users(limit: Int, role: UserRole): [User!]!`
- `products(category: ProductCategory, minPrice: Float): [Product!]!`
- `search(term: String!): [SearchResult!]!`

**Mutations:**
- `createUser`, `updateUser`, `deleteUser`
- `createProduct`, `updateProduct`, `deleteProduct`

#### `resolvers.md`
Implementación de resolvers con lógica de negocio y autenticación.

**Características:**
- Resolvers para Queries y Mutations
- DataLoaders para optimización N+1
- Validación de autenticación y autorización
- Manejo de errores

---

### 🟪 Arquitectura

#### `diagrama.md`
Diagramas de arquitectura en formato Mermaid.

**Diagrama de Componentes:**
```
Cliente → API Gateway → (REST API + GraphQL API) → Business Logic → Database
```

**Componentes principales:**
- API Gateway (NGINX)
- REST API (Node.js/Express)
- GraphQL API (Apollo Server)
- Base de datos (PostgreSQL)
- Caché (Redis)
- CDN (CloudFront)

#### `decisiones.md`
Architecture Decision Records (ADR) documentando las decisiones técnicas.

**ADR 001: API Híbrida (REST + GraphQL)**
- *Contexto:* Soporte para casos simples y complejos
- *Decisión:* Implementar ambas APIs con gateway unificado
- *Consecuencias:* Flexibilidad vs complejidad

**ADR 002: Autenticación JWT**
- *Contexto:* Sistema stateless para escalabilidad
- *Decisión:* JWT con refresh tokens
- *Consecuencias:* Sesiones no persistidas

**ADR 003: Base de Datos Relacional**
- *Contexto:* Datos estructurados con relaciones
- *Decisión:* PostgreSQL
- *Consecuencias:* Integridad referencial garantizada

**ADR 004: Estrategia de Caché**
- *Contexto:* Alto volumen de lecturas
- *Decisión:* Redis para caché distribuida
- *Consecuencias:* Reducción de latencia

**ADR 005: API Gateway**
- *Contexto:* Múltiples servicios necesitan punto único
- *Decisión:* NGINX como gateway
- *Consecuencias:* Seguridad centralizada