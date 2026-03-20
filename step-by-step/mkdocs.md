# MkDocs y Documentación Automática

`MkDocs` crea sitios web estáticos para documentar tu proyecto, y con `mkdocstrings`, puedes extraer documentación directamente de los comentarios de tu código Python.

## 1. Instalación

Añade las dependencias necesarias:

```bash
poetry add --group dev mkdocs mkdocs-material mkdocstrings[python]
```

## 2. Configuración (`mkdocs.yml`)

Archivo principal en la raíz del proyecto:

```yaml
site_name: "Mi Proyecto"
theme:
  name: material

plugins:
  - mkdocstrings:
      handlers:
        python:
          paths: ["."]  # Ruta al código fuente
```

## 3. Crear una página con Docstrings

En `docs/api.md`, puedes usar `:::` seguido de la ruta del módulo para importar los docstrings automáticamente:

```markdown
# Referencia de API

::: mi_paquete.modulo
```

## 4. Servidor de Previsualización

Ejecuta el servidor local para ver cambios en tiempo real:

```bash
poetry run mkdocs serve
```

## 5. Construir para producción

Crea una carpeta `site/` con los archivos HTML listos para desplegar.

```bash
poetry run mkdocs build
```
