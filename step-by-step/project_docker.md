# Configuración de Docker en Lumen Bridge

Lumen Bridge utiliza una arquitectura de contenedores para asegurar que el entorno de ejecución sea idéntico en desarrollo, pruebas y producción.

## 1. El Dockerfile (Multi-stage)

El `Dockerfile` está dividido en varias etapas para optimizar el tamaño de la imagen y la seguridad.

- **Etapa Base (`base`)**:
  - Instala Python 3.13-slim.
  - Configura variables esenciales como `PYTHONUNBUFFERED=1` (ver logs en tiempo real) y `PYTHONPATH=/app`.
  - Instala `Poetry` y las dependencias del sistema para PostgreSQL (`libpq-dev`).
- **Etapa de Test (`test`)**:
  - Se basa en la etapa `base`.
  - Ejecuta `pytest` automáticamente al construir. Si los tests fallan, la imagen no se construye.
- **Etapa de Producción (`production`)**:
  - Expone el puerto `8000`.
  - Ejecuta la aplicación usando `uvicorn`.

## 2. Docker Compose (`docker-compose.yaml`)

Permite orquestar los servicios necesarios para el proyecto.

- **etl-app**: El contenedor principal de la aplicación.
  - Carga variables de entorno desde el archivo `.env`.
  - Mapea el puerto `8000` de tu máquina al puerto `8000` del contenedor.
- **test-app**: Un servicio auxiliar diseñado exclusivamente para ejecutar pruebas.
  - Utiliza el `target: test` del Dockerfile para validar el código.

## 3. Docker Ignore (`.dockerignore`)

Este archivo es crucial para evitar que archivos innecesarios "ensucien" la imagen Docker o aumenten su tamaño.

- **Qué se excluye**:
  - Entornos virtuales locales (`.venv/`).
  - Cachés de Python (`__pycache__`).
  - Archivos de configuración de IDEs (`.vscode/`).
  - Logs y carpetas de exportación locales.
  - Archivos del propio Docker como el `Dockerfile` y `docker-compose.yaml` (una vez usados no son necesarios dentro del contenedor).

## Comandos Rápidos

```bash
# Levantar el proyecto completo
docker-compose up --build

# Ejecutar solo los tests en un contenedor
docker-compose run test-app
```
