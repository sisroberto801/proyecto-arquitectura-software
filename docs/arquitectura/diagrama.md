# Diagramas de Arquitectura

## Diagrama de Componentes

```mermaid
graph TB
    Client[Cliente Web/Mobile]
    Gateway[API Gateway]
    
    subgraph REST[REST API]
        RC[Controllers]
        RS[Services]
    end
    
    subgraph GraphQL[GraphQL API]
        GS[Schema]
        GR[Resolvers]
    end
    
    subgraph Core[Core Business]
        BL[Business Logic]
        Auth[Authentication]
        Val[Validation]
    end
    
    subgraph Data[Data Layer]
        DB[(PostgreSQL)]
        Cache[(Redis)]
    end
    
    Client --> Gateway
    Gateway --> REST
    Gateway --> GraphQL
    REST --> Core
    GraphQL --> Core
    Core --> Data
```

## Diagrama de Flujo - REST

```mermaid
sequenceDiagram
    participant C as Cliente
    participant G as Gateway
    participant R as REST API
    participant B as Business Logic
    participant D as Database
    
    C->>G: POST /users
    G->>R: Enrutar
    R->>B: Validar datos
    B->>D: INSERT usuario
    D-->>B: Usuario creado
    B-->>R: Respuesta
    R-->>G: 201 Created
    G-->>C: JSON response
```

## Diagrama de Flujo - GraphQL

```mermaid
sequenceDiagram
    participant C as Cliente
    participant G as Gateway
    participant QL as GraphQL
    participant B as Business Logic
    participant D as Database
    
    C->>G: POST /graphql {query}
    G->>QL: Ejecutar query
    QL->>B: Resolver campos
    B->>D: Múltiples queries
    D-->>B: Datos
    B-->>QL: Resultados
    QL-->>G: JSON formateado
    G-->>C: Response
```

## Diagrama de Despliegue

```mermaid
graph TB
    subgraph AWS[Amazon Web Services]
        subgraph VPC[VPC]
            subgraph Public[Subred Pública]
                LB[Load Balancer]
            end
            
            subgraph Private[Subred Privada]
                API1[API Instance 1]
                API2[API Instance 2]
                
                subgraph Data[Base de Datos]
                    RDS[(PostgreSQL RDS)]
                    EC[(ElastiCache Redis)]
                end
            end
        end
        
        CDN[CloudFront CDN]
    end
    
    User[Usuario] --> CDN
    CDN --> LB
    LB --> API1
    LB --> API2
    API1 --> RDS
    API1 --> EC
    API2 --> RDS
    API2 --> EC
```

## Componentes Principales

| Componente | Tecnología | Descripción |
|------------|------------|-------------|
| API Gateway | NGINX/AWS | Enrutamiento y rate limiting |
| REST API | Node.js/Express | Endpoints tradicionales |
| GraphQL API | Apollo Server | Consultas flexibles |
| Base de Datos | PostgreSQL | Datos relacionales |
| Caché | Redis | Sesiones y caché |
| CDN | CloudFront | Contenido estático |