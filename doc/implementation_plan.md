# Implementation Plan - Docker Compose Setup

## Goal Description
Create a `docker-compose.yml` file in the root of the project (`c:\Proyectos\reto relampago`) to orchestrate the following services:
- **Frontend**: Angular (existing in `reto-frontend`)
- **Backend**: Laravel (to be mapped to `./reto-backend`)
- **Database**: Postgres
- **Microservices**: Python (to be mapped to `./microservices`)

## User Review Required
> [!IMPORTANT]
> **Directory Creation**: The `reto-backend` and `microservices` directories do not currently exist. The `docker-compose.yml` will reference them as volumes.
>
> **Frontend Dockerfile**: The `reto-frontend` directory exists. I will configure the frontend service to use a base Node.js image and map the volume to run locally.
> *Current Proposal*: Use a `node:18` image for frontend, mapping `./reto-frontend:/app` and running `npm start`.

## Proposed Changes

### Root Directory
#### [NEW] [docker-compose.yml](file:///c:/Proyectos/reto%20relampago/docker-compose.yml)
- **Frontend Service**:
  - Image: `node:20-alpine`
  - Working Dir: `/app`
  - Volumes: `./reto-frontend:/app`
  - Command: `npm start -- --host 0.0.0.0`
  - Ports: `4200:4200`
- **Backend Service (Laravel)**:
  - Image: `bitnami/laravel:latest`
  - Volumes: `./reto-backend:/app`
  - Ports: `8000:8000`
  - Depends on: `db`
  - Environment variables for DB connection.
- **Database Service (Postgres)**:
  - Image: `postgres:15`
  - Environment: `POSTGRES_DB=app_db`, `POSTGRES_USER=user`, `POSTGRES_PASSWORD=password`
  - Ports: `5432:5432`
  - Volumes: `db_data:/var/lib/postgresql/data`
- **Microservices Service (Python)**:
  - Image: `python:3.11-slim`
  - Working Dir: `/app`
  - Volumes: `./microservices:/app`
  - Command: `python main.py` (placeholder)

## Verification Plan
### Manual Verification
1.  **Execution**: User can run `docker-compose up` to verify containers start.
