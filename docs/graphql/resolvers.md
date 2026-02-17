# Resolvers GraphQL

## Estructura de Resolvers

```
/src
  /graphql
    /resolvers
      user.resolvers.js
      product.resolvers.js
      category.resolvers.js
    /schema
      schema.graphql
```

## Resolvers de Usuario

```javascript
const userResolvers = {
  Query: {
    user: async (_, { id }, { dataSources }) => {
      return await dataSources.userAPI.getUserById(id);
    },
    
    users: async (_, { limit, offset, role }, { dataSources }) => {
      return await dataSources.userAPI.getUsers({ limit, offset, role });
    }
  },
  
  Mutation: {
    createUser: async (_, { input }, { dataSources }) => {
      return await dataSources.userAPI.createUser(input);
    },
    
    updateUser: async (_, { id, input }, { dataSources }) => {
      return await dataSources.userAPI.updateUser(id, input);
    },
    
    deleteUser: async (_, { id }, { dataSources }) => {
      return await dataSources.userAPI.deleteUser(id);
    }
  },
  
  // Resolver de campo
  User: {
    products: async (parent, _, { dataSources }) => {
      return await dataSources.productAPI.getProductsByUserId(parent.id);
    }
  }
};
```

## Resolvers de Producto

```javascript
const productResolvers = {
  Query: {
    product: async (_, { id }, { dataSources }) => {
      return await dataSources.productAPI.getProductById(id);
    },
    
    products: async (_, { category, minPrice, maxPrice, inStock }, { dataSources }) => {
      return await dataSources.productAPI.getProducts({
        category, minPrice, maxPrice, inStock
      });
    }
  },
  
  Mutation: {
    createProduct: async (_, { input }, { dataSources, user }) => {
      if (!user) throw new Error('No autenticado');
      return await dataSources.productAPI.createProduct({ ...input, ownerId: user.id });
    },
    
    updateProduct: async (_, { id, input }, { dataSources, user }) => {
      const product = await dataSources.productAPI.getProductById(id);
      if (product.ownerId !== user.id && user.role !== 'ADMIN') {
        throw new Error('No autorizado');
      }
      return await dataSources.productAPI.updateProduct(id, input);
    },
    
    deleteProduct: async (_, { id }, { dataSources, user }) => {
      const product = await dataSources.productAPI.getProductById(id);
      if (product.ownerId !== user.id && user.role !== 'ADMIN') {
        throw new Error('No autorizado');
      }
      return await dataSources.productAPI.deleteProduct(id);
    }
  },
  
  Product: {
    owner: async (parent, _, { dataSources }) => {
      return await dataSources.userAPI.getUserById(parent.ownerId);
    }
  }
};
```

## DataLoaders (Optimización N+1)

```javascript
const createUserLoader = (dataSources) => {
  return new DataLoader(async (userIds) => {
    const users = await dataSources.userAPI.getUsersByIds(userIds);
    return userIds.map(id => users.find(user => user.id === id));
  });
};

// Uso en resolvers
const resolvers = {
  Product: {
    owner: async (parent, _, { loaders }) => {
      return await loaders.userLoader.load(parent.ownerId);
    }
  }
};
```

## Manejo de Autenticación

```javascript
const context = ({ req }) => {
  const token = req.headers.authorization || '';
  const user = verifyToken(token.replace('Bearer ', ''));
  
  return {
    user,
    dataSources: {
      userAPI: new UserAPI(),
      productAPI: new ProductAPI()
    }
  };
};
```