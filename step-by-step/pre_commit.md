# Configuración de Pre-commit

`pre-commit` es una herramienta que ejecuta verificaciones automáticas en tu código cada vez que intentas hacer un `git commit`. Asegura que el código cumpla con los estándares de calidad antes de guardarlo.

## 1. Instalación

Añade `pre-commit` como dependencia de desarrollo:

```bash
poetry add --group dev pre-commit
```

## 2. Archivo de Configuración (`.pre-commit-config.yaml`)

Crea un archivo en la raíz de tu proyecto con el nombre `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format
```

## 3. Instalación de los Hooks

Para activar los hooks en tu repositorio local git, ejecuta:

```bash
poetry run pre-commit install
```

## 4. Ejecución Manual

Puedes ejecutar todos los hooks manualmente sin hacer un commit:

```bash
poetry run pre-commit run --all-files
```

## Beneficios

- Evita que código mal formateado llegue al repositorio.
- Automatiza correcciones simples (espacios adicionales, finales de archivo).
- Mejora la consistencia del código entre diferentes desarrolladores.
