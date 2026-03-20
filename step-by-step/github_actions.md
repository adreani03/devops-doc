# CI/CD con GitHub Actions

El CI/CD (Integración Continua y Despliegue Continuo) permite automatizar pruebas, análisis de código y despliegues cada vez que subes código a GitHub.

## 1. Estructura de archivos

Las configuraciones se guardan en `.github/workflows/`.

```text
.github/
└── workflows/
    └── main_ci.yml
```

## 2. Workflow Básico (`main_ci.yml`)

Este ejemplo ejecuta pruebas y análisis de código en cada Pull Request.

```yaml
name: CI Proyecto

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.13'

      - name: Install Poetry
        run: |
          curl -sSL https://install.python-poetry.org | python3 -
          export PATH="$HOME/.local/bin:$PATH"

      - name: Install dependencies
        run: poetry install

      - name: Lint with Ruff
        run: poetry run ruff check .

      - name: Test with Pytest
        run: poetry run pytest
```

## 3. Secretos de GitHub

Para despliegues (ej. Docker Hub), usa secretos:

1. Ve a tu repositorio en GitHub.
2. Settings -> Secrets and variables -> Actions.
3. Crea `DOCKERHUB_USERNAME` y `DOCKERHUB_TOKEN`.

## 4. Flujo sugerido

- **CI**: En cada Pull Request. Asegura que el código no rompe nada.
- **CD**: En cada `push` a `main` o al crear un `release`. Despliega a producción.
