# Gestión de tareas con el archivo .todo

Un archivo `.todo` en la raíz del proyecto es una forma sencilla y efectiva de gestionar tareas pendientes sin depender de herramientas externas complejas.

## 1. Formato sugerido

Usa Markdown para una visualización clara:

```markdown
# 📝 Tareas Pendientes

## 🔴 Urgente
- [ ] Corregir bug en el login
- [ ] Actualizar credenciales caducadas

## 🟡 En Desarrollo
- [x] Implementar exportación a Excel
- [ ] Optimizar consultas a la base de datos

## 🟢 Futuro
- [ ] Refactorizar el módulo de usuarios
- [ ] Añadir internacionalización (i18n)
```

## 2. Uso en el flujo diario

- **Añadir**: Tan pronto como surja una idea o bug, añádela al archivo.
- **Marcar**: Usa `[x]` para marcar tareas completadas.
- **Limpieza**: Mueve tareas completadas hace tiempo a una sección de "Historial" o elimínalas al iniciar un nuevo ciclo de desarrollo.

## 3. Beneficios

- **Contexto**: Siempre sabes qué sigue sin salir del editor.
- **Control**: Compartido vía Git, permite que todo el equipo esté alineado.
- **Simplicidad**: No requiere login, internet, ni apps extras.
