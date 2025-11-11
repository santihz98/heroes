# Makeup Billing (Java 21 · Spring Boot · MySQL · Hibernate/Jakarta)

Aplicación **Spring Boot 3 + Java 21** para gestionar un sistema de **facturación de maquillaje**, implementando buenas prácticas de arquitectura:
- API REST limpia (controladores, servicios, repositorios).
- Manejo de errores y validaciones.
- Persistencia con **MySQL + Hibernate/JPA**.
- Uso de **Lombok** para simplificar las entidades.
- Sembrado inicial mediante endpoints (Postman o `curl`).
  
---

## 🚀 Tecnologías utilizadas

- **Java 21**
- **Spring Boot 3 (Web, JPA, Validation)**
- **MySQL 8**
- **Hibernate 6**
- **Lombok**
- **Maven**

---
---

## 🐬 Levantar MySQL con Docker

Ejecuta en tu terminal, esto con el fin de crear la base de datos con sus respectivas entidades:

```bash
docker run -d \
  --name mysql8 \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=Sara \
  -e MYSQL_DATABASE=makeup_billing \
  mysql:8.4
```

## 🧠 Endpoints principales

---

### 👩 **Clientes**

| Método | Endpoint | Descripción |
|:-------|:----------|:-------------|
| **GET** | `/api/clientes` | Listar todos los clientes |
| **GET** | `/api/clientes/{id}` | Obtener cliente por ID |
| **GET** | `/api/clientes?q={texto}` | Buscar cliente por nombre |
| **POST** | `/api/clientes` | Crear nuevo cliente |
| **PUT** | `/api/clientes/{id}` | Actualizar cliente existente |
| **DELETE** | `/api/clientes/{id}` | Eliminar cliente |

**🧩 Ejemplo de creación:**

```bash
curl -X POST http://localhost:8080/api/clientes \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Sara Gonzalez Londono",
    "email": "sara.londono@example.com",
    "telefono": "+57 312 555 0142",
    "direccion": "Calle 82 #12-50, Medellín"
  }'
```

### 💅 **Productos**

| Método | Endpoint | Descripción |
|:-------|:----------|:-------------|
| **GET** | `/api/productos` | Listar todos los productos |
| **GET** | `/api/productos?activos=true` | Listar productos activos |
| **GET** | `/api/productos/{id}` | Obtener producto por ID |
| **POST** | `/api/productos` | Crear nuevo producto |
| **PUT** | `/api/productos/{id}` | Actualizar producto existente |
| **DELETE** | `/api/productos/{id}` | Eliminar producto |

**🧩 Ejemplo de creación:**

```bash
curl -X POST http://localhost:8080/api/productos \
  -H "Content-Type: application/json" \
  -d '{
    "sku": "LIPS-MATTE-ROSE-01",
    "nombre": "Labial Matte Rose",
    "descripcion": "Labial mate de larga duración",
    "precio": 39900,
    "stock": 120,
    "activo": true
  }'
```

### 🧾 **Ventas**

| Método | Endpoint | Descripción |
|:-------|:----------|:-------------|
| **GET** | `/api/ventas` | Listar todas las ventas |
| **GET** | `/api/ventas?clienteId={id}` | Listar ventas por cliente |
| **GET** | `/api/ventas/{id}` | Obtener detalle de venta |
| **POST** | `/api/ventas` | Crear nueva venta |

**🧩 Ejemplo de creación:**

```bash
curl -X POST http://localhost:8080/api/ventas \
  -H "Content-Type: application/json" \
  -d '{
    "clienteId": 1,
    "items": [
      { "productoId": 1, "cantidad": 2 },
      { "productoId": 2, "cantidad": 1 }
    ]
  }'
```

### ⚠️ **Manejo de errores**

| Tipo de error | Código HTTP | Ejemplo de causa |
|:---------------|:-------------|:------------------|
| **Validación de datos** | `400 Bad Request` | Campos vacíos o formato inválido |
| **Recurso no encontrado** | `404 Not Found` | ID inexistente |
| **Duplicado / conflicto** | `409 Conflict` | Email o SKU ya existente |
| **Error interno** | `500 Internal Server Error` | Error inesperado |

**🧩 Ejemplo de respuesta de error:**

```json
{
  "success": false,
  "message": "Email ya existe",
  "path": "/api/clientes",
  "timestamp": "2025-11-10T19:31:25.401Z"
}
```
