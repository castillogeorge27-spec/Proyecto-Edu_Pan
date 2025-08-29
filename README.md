# Proyecto-Edu_Pan
Centificados
# Proyecto DocuTrack Certificados (React + Node + PostgreSQL)

Primero que todo darles las gracias por la oportunidad de ser tomado encuenta para la prueba, 
este es el proyecto que realize la verdad no está bien terminado debido que es la primera 
vez utilizando estas herramientay por falta de conocimiento quedó sin que me pudiera registrar los usuarios,
el proyecto en si lo hice con ayuda de la IA y videos proporcionado de YouTube sobre las programas 
ya que al no saber sobre las herramientas solicitada me vi a buscar soluciones
aún sabiendo que tal vez no cumpla con las espectativa de la vacante,
pero bueno esto también me ayuda a mi probar horizontes sobre mi área de estudio.

## 1) Prerrequisitos
- node-v22.18.0-x64
- PostgreSQL 17+
- Visual Studio Code con extensiones: **ESLint**, **Prettier**, **PostgreSQL** (o SQLTools)

## 2) Clonar/copy el código
Descarga este zip y extrae la carpeta `DocuTrack-certificados`.

## 3) Crear base de datos
Desde VS Code (Terminal Integrado) o tu terminal:

```bash
psql -U postgres -h localhost -c "CREATE DATABASE certificadosdb;"
```

## 4) Backend
```bash
cd backend
cp .env.example .env
# edita .env si cambiaste usuario/clave de Postgres
npm install
npm run migrate
# (opcional) crear admin inicial
ADMIN_EMAIL=admin@demo.com ADMIN_PASSWORD=admin123 npm run seed:admin
npm run dev
```
API en `http://localhost:4000/api`

## 5) Frontend
Abrir nueva terminal:
```bash
cd ../fronted
cp .env.example .env
npm install
npm run dev
```
Web en `http://localhost:5173`

## 6) Flujo
- Regístrate / inicia sesión.
- Crea una solicitud de certificado.
- Como admin, entra al panel `/admin`, cambia estados y genera PDF.
- El usuario ve el seguimiento en tiempo real y puede descargar cuando esté **ready**.

## Estructura de datos (tablas principales)
- `users(id, email, password, full_name, role)`
- `certificates(id, user_id, content, requester_name, requester_email, tracking_code, status, pdf_path)`
- `certificate_events(id, certificate_id, status, note)`

## Sincronizar VS Code con PostgreSQL
1. Instala la extensión **PostgreSQL** 
2. Crea una conexión con:
   - Host: `localhost`
   - Port: `5432`
   - User: `postgres` 
   - Password: `gc13`
   - Database: `certificadosdb`
3. Abre `server/migrations/001_init.sql`, haz clic derecho y **Run Query** para ejecutar desde VS Code (opcional si ya corriste la migración).
4. Verifica tablas en el panel de la extensión.

## Seguridad
- JWT (Bearer) con expiración 2h, middleware de autenticación y `requireRole('admin')` para rutas de admin.
- Descarga de PDF validada por **rol** o **propiedad** del certificado.
- Helmet, CORS restringido al `CLIENT_ORIGIN`.

## Rutas API (resumen)
- **Auth**: `POST /auth/register`, `POST /auth/login`, `GET /auth/me`
- **Usuario**: `POST /certificates`, `GET /certificates`, `GET /certificates/:id`, `GET /certificates/:id/events`, `GET /certificates/:id/download`
- **Admin**: `GET /admin/dashboard`, `GET /admin/requests`, `PATCH /admin/requests/:id/status`, `POST /admin/requests/:id/generate-pdf`
- 
