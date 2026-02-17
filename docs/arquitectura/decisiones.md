# Decisiones Arquitectónicas (ADR)

## ADR 001: API Híbrida (REST + GraphQL)

**Estado:** Aceptado

**Contexto:**
El sistema debe soportar casos de uso simples (CRUD básico) y consultas complejas con múltiples recursos.

**Decisión:**
Implementar ambas APIs con un API Gateway unificado:
- REST para operaciones CRUD estándar
- GraphQL para consultas complejas y flexibles

**Consecuencias:**
- ✅ Mayor flexibilidad para diferentes tipos de clientes
- ✅ Curva de aprendizaje gradual
- ❌ Mayor complejidad en mantenimiento
- ❌ Duplicación de lógica en algunos casos

---

## ADR 002: Autenticación JWT

**Estado:** Aceptado

**Contexto:**
Necesidad de un sistema stateless, escalable y sin sesiones en servidor.

**Decisión:**
Usar JWT (JSON Web Tokens) con refresh tokens:
- Access token: 15 minutos de validez
- Refresh token: 7 días, almacenado en Redis

**Consecuencias:**
- ✅ Escalabilidad horizontal
- ✅ Sin estado en servidor
- ❌ Tokens no revocables inmediatamente
- ❌ Mayor payload en cada request

---

## ADR 003: Base de Datos Relacional

**Estado:** Aceptado

**Contexto:**
Datos estructurados con relaciones claras entre usuarios, productos y categorías.

**Decisión:**
PostgreSQL como base de datos principal:
- Esquema definido
- Integridad referencial
- Soporte para JSON cuando sea necesario

**Consecuencias:**
- ✅ Integridad de datos garantizada
- ✅ Consultas complejas con JOINS
- ❌ Menos flexible para cambios de esquema
- ❌ Escalado vertical principalmente

---

## ADR 004: Estrategia de Caché

**Estado:** Aceptado

**Contexto:**
Alta lectura de productos y necesidad de reducir carga en base de datos.

**Decisión:**
Redis para caché distribuida:
- Productos más vistos: TTL 1 hora
- Sesiones de refresh tokens
- Rate limiting

**Consecuencias:**
- ✅ Reducción de latencia
- ✅ Menos carga en BD
- ❌ Consistencia eventual
- ❌ Complejidad operacional

---

## ADR 005: API Gateway

**Estado:** Aceptado

**Contexto:**
Múltiples servicios (REST, GraphQL) necesitan un punto de entrada único.

**Decisión:**
NGINX como API Gateway:
- Enrutamiento basado en path
- Rate limiting por IP/usuario
- SSL termination
- Logging centralizado

**Consecuencias:**
- ✅ Punto único de entrada
- ✅ Seguridad centralizada
- ❌ Punto único de fallo (con redundancia)
- ❌ Latencia adicional mínima

---

## Línea de Tiempo de Decisiones

| Fecha | ADR | Decisión |
|-------|-----|----------|
| 2024-01-10 | 001 | API Híbrida |
| 2024-01-10 | 002 | JWT |
| 2024-01-11 | 003 | PostgreSQL |
| 2024-01-11 | 004 | Redis |
| 2024-01-12 | 005 | API Gateway |