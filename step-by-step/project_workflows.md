# Workflows de CI/CD en Lumen Bridge

Este tutorial explica los workflows de GitHub Actions que están configurados actualmente en el proyecto para automatizar el ciclo de vida del software.

## 1. Ubicación de los archivos

Todos los archivos de configuración se encuentran en la carpeta `.github/workflows/`.

## 2. CI Pipeline Docker (`docker-image.yml`)

Este flujo se encarga de validar que la imagen de Docker se construya correctamente sin errores.

- **Cuándo se ejecuta**: Cada vez que se hace un `push` o un `pull_request` a la rama `main`.
- **Qué hace**:
  1. **Lint**: Ejecuta `ruff` para asegurar que el código cumple con los estándares.
  2. **Build Docker**: Si el linter pasa, intenta construir la imagen Docker (`Dockerfile`) para verificar que no haya errores de configuración en el entorno de contenedores.
  3. **Caché**: Utiliza el sistema de caché de GitHub Actions para acelerar las construcciones posteriores.

## 3. CI/CD Pipeline Versions (`main.yml`)

Es el flujo principal que gestiona las versiones automáticas del proyecto.

- **Cuándo se ejecuta**: Con cada `push` o `pull_request` a `main`.
- **Puntos clave**:
  1. **Linting**: Validación estática del código.
  2. **Validación de Imagen**: Construye las etapas `base` y `test` del Dockerfile.
  3. **Release Automático**: Si el código está en `main` y las etapas anteriores pasan, utiliza `python-semantic-release` para:
    - Calcular la siguiente versión basada en los mensajes de commit.
    - Crear un Tag en Git.
    - Actualizar el CHANGELOG.md automáticamente.

## 4. Python Application (`python-app.yml`)

Un flujo enfocado exclusivamente en la ejecución de pruebas unitarias en el entorno de GitHub.

- **Qué hace**:
  1. Configura el entorno de Python 3.13.
  2. Instala dependencias con `Poetry`.
  3. Ejecuta `pytest` para asegurar que los cambios no rompan la lógica de negocio existente.
  4. Configura `PYTHONPATH` para que los módulos sean importables correctamente durante los tests.

## Resumen del Ciclo de Vida

1. Subes código -> Se ejecutan **Lint** y **Tests**.
2. Se verifica que el **Dockerfile** compile.
3. Si integras en `main`, se genera una **Nueva Versión** (Release) automáticamente.
