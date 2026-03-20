# Docker en Python con Poetry

Docker empaqueta tu aplicación con todas sus dependencias en un contenedor, asegurando que funcione igual en cualquier máquina.

## 1. Dockerfile para Python y Poetry

Un patrón eficiente es usar una etapa de construcción (`builder`) para instalar dependencias y una etapa final más ligera.

```dockerfile
# Etapa 1: Builder
FROM python:3.13-slim as builder

WORKDIR /app
RUN pip install poetry
COPY pyproject.toml poetry.lock ./
# Instalar dependencias sin crear entorno virtual en Docker
RUN poetry config virtualenvs.create false \
    && poetry install --no-interaction --no-ansi

# Etapa 2: Final
FROM python:3.13-slim
WORKDIR /app
COPY --from=builder /usr/local/lib/python3.13/site-packages /usr/local/lib/python3.13/site-packages
COPY . .
CMD ["python", "src/app/main.py"]
```

## 2. Docker Compose

Permite gestionar múltiples contenedores (ej. App + Base de datos).

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    env_file: .env
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: mi_db
      POSTGRES_USER: usuario
      POSTGRES_PASSWORD: password
```

## 3. Comandos Útiles

```bash
# Construir y levantar
docker-compose up --build

# Detener
docker-compose down
```
