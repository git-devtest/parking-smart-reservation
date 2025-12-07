# 📐 Arquitectura del Proyecto — Parking Smart Reservation System

## Versión: 1.0
## Última actualización: (completa cuando hagas el commit)

### 🧩 1. Descripción general de la arquitectura

Este sistema implementa una plataforma inteligente para reservas de parqueaderos, compuesta por tres subsistemas principales:

Frontend (Angular 17/18):   
Interfaz web responsiva, con dashboard, flujo de autenticación, administración y módulo cliente de reservas.

Backend API (Node.js 20 LTS + Express):    
Maneja autenticación JWT, control de acceso por roles, CRUD completo, gestión de reservas, cálculos de tarifas y conexión con ML.

Módulo de Machine Learning (Python):    
Servicio independiente que predice ocupación y patrones de uso mediante modelos de series temporales o regresión.

La arquitectura se basa en un modelo cliente → API → servicios y usa prácticas modernas como separación por capas, comunicación REST, y contenedores (futuro opcional).

### 🏗 2. Diagrama General de Arquitectura

```mermaid
┌──────────────────────────┐
│        FRONTEND          │
│     Angular 17/18        │
│  • UI / UX               │
│  • Auth con JWT          │
└────────────┬─────────────┘
             │ HTTPS (REST)
             │
┌────────────▼─────────────┐
│        BACKEND API       │
│     Node.js 20 LTS       │
│  • Rutas y controladores │
│  • Servicio Auth / Roles │
│  • CRUD Usuarios / Admin │
│  • Gestión Reservas      │
│  • Integración ML        │
└────────────┬─────────────┘
             │ MySQL Driver
             │
┌────────────▼──────────────┐
│         DATABASE          │
│         MySQL 8           │
│  • Tablas normalizadas    │
│  • Índices de rendimiento │
│  • Vistas y SP opcionales │
└────────────┬──────────────┘
             │ HTTP / gRPC (opcional)
             │
┌────────────▼──────────────┐
│     ML SERVICE (Python)   │
│  • Predicción ocupación   │
│  • Entrenamiento modelos  │
│  • Servido vía FastAPI    │
└───────────────────────────┘
```

### 📁 3. Estructura de carpetas (general)
🔷 Nivel Raíz
```mermaid
/parking-smart-reservation
│
├── backend/                # API REST Node.js
├── frontend/               # App Angular
├── ml/                     # Servicio ML en Python
│
├── docs/                   # Documentación del proyecto
│   ├── architecture.md
│   ├── roadmap.md
│   ├── endpoints.md
│   └── db-schema.md
│
├── .github/                # Workflows CI/CD
├── .editorconfig
├── README.md
└── LICENSE
```

### 🧱 4. Arquitectura del Backend
1. Patrón utilizado: Clean Architecture ligera + Modularización por dominio

2. Capas principales   
a. Routes: recibe la petición y la envía al controlador.   
b. Controllers: validación básica → llama al servicio.   
c. Services: lógica de negocio.   
d. Repositories: consultas a la base de datos.   
e. Middlewares: autenticación, manejo de errores.   
f. Utils: helpers, formatos, configuración.   
g. Config: conexión DB, variables .env.

3. Estructura
```mermaid
backend/
├── src/
│   ├── routes/
│   ├── controllers/
│   ├── services/
│   ├── repositories/
│   ├── middlewares/
│   ├── config/
│   ├── utils/
│   └── app.js
└── package.json
```

### 🎨 5. Arquitectura del Frontend
1. Basada en:    
a. Angular Standalone   
b. Arquitectura modular   
c. Servicios inyectables   
d. Interceptores para JWT

2. Estructura
```mermaid
frontend/
├── src/
│   ├── app/
│   │   ├── core/             # Interceptores, guards, servicios globales
│   │   ├── modules/          # Módulos como auth, reservas, admin
│   │   ├── shared/           # Componentes reutilizables
│   │   └── app.config.ts
│   └── assets/
└── angular.json
```

### 🤖 6. Arquitectura del módulo ML
1. Características   
a. Servicio independiente   
b. Entrenamiento y predicción por API   
c. Posible Dockerización   
d. Comunicación desde el backend vía HTTP

2. Estructura
```mermaid
ml/
├── notebooks/               # Exploración inicial (no va a producción)
├── src/
│   ├── models/              # Modelos entrenados (.pkl)
│   ├── training/            # Scripts de entrenamiento
│   ├── service/             # FastAPI para predicción
│   └── utils/
└── requirements.txt
```
### 🔄 7. Flujo de autenticación (JWT)
1. Usuario envía credenciales al backend
2. Backend valida y genera token firmado
3. Frontend almacena token (localStorage/secure storage)
4. Todas las peticiones posteriores agregan el token en el header
5. Backend valida token → da acceso según rol
6. Para Admin se habilitan endpoints bloqueados para usuarios estándar

### 🧪 8. Conexión entre módulos
```mermaid
Módulo	                    Conexión	    Protocolo
Frontend → Backend	        REST API	    HTTPS
Backend → Base de datos	    Driver MySQL	TCP
Backend → ML	            REST	        HTTPS (o HTTP interno)
ML → Modelos entrenados	    acceso local	filesystem
```
### ⚙ 9. Tecnologías principales
#### Frontend
1. Angular 17/18
2. Bootstrap/Tailwind
3. RxJS
4. NgRx (opcional)

#### Backend
1. Node.js 20 LTS
2. Express
3. MySQL2
4. JWT
5. bcryptjs
6. Helmet, CORS, Rate limiting

#### ML
1. Python 3.11+
2. scikit-learn / Prophet / XGBoost
3. FastAPI

### 🚀 10. Futuras ampliaciones
1. Docker Compose para 3 servicios (FE, BE, ML)
2. Cache Redis para mejorar respuesta
3. WebSockets para disponibilidad en tiempo real
4. Integración CI/CD con GitHub Actions

### 📦 11. Estado actual del entregable
Este documento corresponde al Entregable 4: Arquitectura del sistema, listo para ser agregado a Git.