# Implementation Plan - Docker Compose Setup

## Goal Description
Create a `docker-compose.yml` file in the root of the project (`c:\Proyectos\reto relampago`) to orchestrate the following services:
- **Frontend**: Angular (existing in `reto-frontend`)
- **Backend**: Laravel (to be mapped to `./reto-backend`)
- **Database**: Postgres
- **Microservices**: Python (to be mapped to `./microservices`)

## User Review Required
> [!IMPORTANT]
> **Backend Image Change**: The Bitnami Laravel image is incompatible with Docker Desktop for Windows volume permissions (it tries to `chown` mounted files and fails).
> **Solution**: Switch to a custom `Dockerfile` based on `php:8.2-fpm` or similar, or use a command that installs dependencies and runs the server directly without the restrictive entrypoint.
> **Proposed implementation**: I will create a `Dockerfile` for the backend in inside `reto-backend` (or root `docker/backend/Dockerfile`) and use that interactively.
> *Simpler Alternative*: Use `bitnami/laravel` but override the `entrypoint` to skip the setup script.
> *Even Simpler*: Use `php:8.2-cli` image directly in compose, install extensions on boot (slow) or use a custom Dockerfile.
>
> **Let's try overriding the command first**: explicitly telling it to just run `php artisan serve` might bypass the initialization script if we change the entrypoint.

## Proposed Changes

### Root Directory
#### [MODIFY] [docker-compose.yml](file:///c:/Proyectos/reto%20relampago/docker-compose.yml)
- **Backend Service**:
  - **[Fix]** Replace `bitnami/laravel` with `php:8.2-apache`.
  - **[Fix]** Map ports and commands.
  - **[Complexity]** Requires installing extensions.

**Better Approach**: Create a simple `Dockerfile` for the backend.

#### [NEW] [reto-backend/Dockerfile](file:///c:/Proyectos/reto%20relampago/reto-backend/Dockerfile)
```dockerfile
FROM php:8.2-apache
RUN apt-get update && apt-get install -y libpq-dev zip unzip \
    && docker-php-ext-install pdo pdo_pgsql
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
WORKDIR /app
COPY . /app
RUN chown -R www-data:www-data /app
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
```

#### [MODIFY] [docker-compose.yml](file:///c:/Proyectos/reto%20relampago/docker-compose.yml)
- Change `image: bitnami/laravel` to `build: ./reto-backend`.

## Verification Plan
### Manual Verification
1.  **Execution**: User can run `docker-compose up --build backend` to verify.
