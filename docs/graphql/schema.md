# Esquema GraphQL

## Tipos Principales

```graphql
type User {
  id: ID!
  name: String!
  email: String!
  role: UserRole!
  active: Boolean!
  products: [Product!]!
  createdAt: String!
  updatedAt: String!
}

type Product {
  id: ID!
  name: String!
  description: String
  price: Float!
  category: ProductCategory!
  stock: Int!
  images: [String!]
  owner: User!
  createdAt: String!
  updatedAt: String!
}

type Category {
  id: ID!
  name: String!
  description: String
  active: Boolean!
  products: [Product!]!
}
```

## Enums

```graphql
enum UserRole {
  ADMIN
  USER
}

enum ProductCategory {
  ELECTRONICS
  CLOTHING
  BOOKS
  FOOD
  OTHER
}
```

## Inputs

```graphql
input CreateUserInput {
  name: String!
  email: String!
  password: String!
  role: UserRole = USER
}

input UpdateUserInput {
  name: String
  email: String
  role: UserRole
  active: Boolean
}

input CreateProductInput {
  name: String!
  description: String
  price: Float!
  category: ProductCategory!
  stock: Int!
  images: [String!]
}

input UpdateProductInput {
  name: String
  description: String
  price: Float
  category: ProductCategory
  stock: Int
}
```

## Queries

```graphql
type Query {
  # Usuarios
  user(id: ID!): User
  users(limit: Int = 10, offset: Int = 0, role: UserRole): [User!]!
  
  # Productos
  product(id: ID!): Product
  products(
    category: ProductCategory,
    minPrice: Float,
    maxPrice: Float,
    inStock: Boolean
  ): [Product!]!
  
  # Búsqueda
  search(term: String!): [SearchResult!]!
}

union SearchResult = User | Product
```

## Mutations

```graphql
type Mutation {
  # Usuarios
  createUser(input: CreateUserInput!): User!
  updateUser(id: ID!, input: UpdateUserInput!): User!
  deleteUser(id: ID!): Boolean!
  
  # Productos
  createProduct(input: CreateProductInput!): Product!
  updateProduct(id: ID!, input: UpdateProductInput!): Product!
  deleteProduct(id: ID!): Boolean!
}
```

## Subscriptions

```graphql
type Subscription {
  newProduct: Product!
  stockUpdated(productId: ID!): Product!
}
```

## Ejemplos de Consultas

**Query usuarios:**
```graphql
query GetUsers {
  users(limit: 5) {
    id
    name
    email
    products {
      name
      price
    }
  }
}
```

**Mutation crear producto:**
```graphql
mutation CreateProduct {
  createProduct(input: {
    name: "Smartphone Z5"
    price: 699.99
    category: ELECTRONICS
    stock: 50
  }) {
    id
    name
    price
    createdAt
  }
}
```