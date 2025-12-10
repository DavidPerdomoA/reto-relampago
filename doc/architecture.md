# Arquitectura Completa del Proyecto Clínico de Cirugía Plástica

## Tecnologías
- **Frontend:** Angular
- **Backend:** Laravel
- **Base de Datos:** PostgreSQL
- **Microservicios IA:** Python (FastAPI)
- **Despliegue:** Servicios gratuitos (Vercel, Render, Neon, Cloudflare R2, Deta Space)

---

# 1. Arquitectura General

```mermaid
graph TD
    User[Portal Público / Frontend Dashboard] -->|HTTPS| API[API Gateway (Laravel)]
    API -->|Auth, CRUD, Logic| DB[(PostgreSQL (Neon))]
    API -->|Microservices| Python[Python Microservices (FastAPI + IA)]
    API -->|Storage| R2[(Storage Cloudflare R2)]
    Python --> DB
    Python --> R2
```

---

# 2. Frontend Angular

## Estructura
- `core/` (auth, guards, interceptors)
- `shared/` (componentes reutilizables)
- `features/`
  - pacientes
  - agenda
  - historia clínica
  - imágenes
  - consentimientos
  - facturación
  - inventario
  - crm
  - portal
  - reportes

## Despliegue
- **Vercel** (recomendado)
- Netlify
- Firebase Hosting

---

# 3. Backend Laravel

## Funciones
- API REST
- Autenticación JWT/Sanctum
- Roles y permisos
- CRUDs completos
- Generación de PDFs
- Middleware clínicos
- Conexión a Python

## Despliegue Gratis
- **Render (Web Service Free)**
- Railway Free Tier
- Fly.io

---

# 4. Base de Datos PostgreSQL

## Opciones Free
- **Neon.tech** (recomendado)
- ElephantSQL Free Tier
- Supabase Free Tier

---

# 5. Microservicios Python IA

## Servicios
### 5.1 IA de Imágenes
- Comparación antes/después
- Ghosting
- Anotaciones automáticas
- Limpieza y normalización

### 5.2 IA de Voz
- Transcripción Whisper
- Resumen automático
- Creación de notas clínicas

### 5.3 IA de CRM
- Clasificación de leads
- Mensajes sugeridos
- Embudo inteligente

## Despliegue Free
- **Deta Space** (ideal)
- Render (Free)
- Railway

---

# 6. Storage Gratuito

## Opciones
- **Cloudflare R2 (gratis + CDN)**
- Supabase Storage
- Firebase Storage

---

# 7. Mensajería

## Gratis o bajo costo
- Brevo (email)
- WhatsApp Cloud API
- Firebase Cloud Messaging

---

# 8. Arquitectura en Capas

```
Capa 1: Presentación (Angular)
Capa 2: API REST (Laravel)
Capa 3: Microservicios IA (Python)
Capa 4: Base de Datos (PostgreSQL)
Capa 5: Almacenamiento (R2 / Supabase)
Capa 6: Notificaciones (Brevo / Whatsapp)
```

---

# 9. Dominio por Módulos

- Pacientes  
- Agenda  
- EHR  
- Consentimientos  
- Imágenes  
- Facturación  
- Inventario  
- CRM  
- Portal del paciente  
- Reportes  

---

# 10. Fases de Desarrollo

1. Autenticación, Pacientes, Agenda  
2. Historia Clínica, Consentimientos  
3. Imágenes Avanzadas  
4. Facturación e Inventario  
5. CRM + Marketing  
6. Portal del Paciente  
7. Reportes y Analítica  
