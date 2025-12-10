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
#### [MODIFY] [docker-compose.yml](file:///c:/Proyectos/reto%20relampago/docker-compose.yml)
- **Backend Service**:
  - **[Fix]** Add `user: root` to bypass `chown` permission errors on Windows Docker volumes for the Bitnami image.
- **Frontend Service**:
  - **[Fix]** Add anonymous volume `/app/node_modules` to prevent host Windows modules from breaking the Linux container.
  - **[Fix]** Update initialization command to `sh -c "npm install && npm start -- --host 0.0.0.0"` to ensure Linux dependencies are installed.

## Verification Plan
### Manual Verification
1.  **Execution**: User can run `docker-compose up` to verify containers start.
2.  **Access**: Verify `localhost:4200` is accessible.
