---
name: commit
description: Analiza los cambios en git (staged o en working tree), genera un título y descripción detallada (estilo Conventional Commits y plantilla de Merge Request), realiza el commit y hace push a GitLab/GitHub.
---

# Skill Global: Commit & Push con Descripción de MR

Esta skill se activa cuando el usuario escribe `/commit` o solicita realizar un commit y push de sus cambios en cualquier proyecto.

## Instrucciones de ejecución

1. **Revisar los cambios en Git**:
   - Obtener la rama actual: `git branch --show-current`
   - Comprobar los archivos en staging y modificados: `git status -s`
   - Si no hay archivos en staging pero sí modificados y el usuario quiere incluirlos todos, hacer `git add .` (o preguntar si aplica).

2. **Analizar las diferencias (Diff)**:
   - Inspeccionar las diferencias con `git diff --cached` (o `git diff`).
   - Identificar refactorizaciones, funciones nuevas, corrección de errores, estilos o configs.

3. **Redactar Mensaje de Commit / Descripción de MR**:
   - **Título**: Debe comenzar obligatoriamente con el ID (6 dígitos precedidos de `#`, ej: `#262316 `) al inicio absoluto del mensaje de commit (ej: `#262316 feat(scope): ...`, `#262316 fix: ...`, `#262316 refactor: ...`).
   - **Cuerpo (Descripción Detallada)**:
     - **Resumen**: Explicación general del cambio.
     - **Cambios realizados**: Lista detallada archivo por archivo o por módulo.
     - **Motivo / Contexto**: Razón del cambio.

4. **Ejecutar Commit y Push**:
   - Ejecutar `git commit -m "<Título>" -m "<Descripción detallada>"`
   - Ejecutar `git push origin <rama-actual>` (o `git push -u origin <rama-actual>` si la rama no existe en el remoto).

5. **Confirmación**:
   - Confirmar al usuario con el título y la descripción generados.
