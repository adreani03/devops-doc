# Uso de Pre-commit con Poetry

## Paso 1: Configuración

Crea o edita el archivo `.pre-commit-config.yaml` en la raíz del proyecto:

```yaml
repos:
    # 1. Validaciones específicas de Poetry
    - repo: https://github.com/python-poetry/poetry
      rev: 1.8.0
      hooks:
          - id: poetry-check
          - id: poetry-lock

    # 2. Calidad de código (Ruff)
    - repo: https://github.com/astral-sh/ruff-pre-commit
      rev: v0.3.0
      hooks:
          - id: ruff
            args: [--fix]
          - id: ruff-format

    # 3. Hooks básicos
    - repo: https://github.com/pre-commit/pre-commit-hooks
      rev: v4.5.0
      hooks:
          - id: trailing-whitespace
          - id: end-of-file-fixer
          - id: check-yaml
          - id: check-added-large-files
```

---

### Paso 2: Instalar pre-commit

```bash
poetry add pre-commit --group dev
```

---

### Paso 3: Activar hooks en Git

```bash
poetry run pre-commit install
```

---

### Paso 4: Ejecución manual (opcional)

```bash
poetry run pre-commit run --all-files
```

---

### Truco útil

```bash
git commit -m "mensaje" --no-verify
```
