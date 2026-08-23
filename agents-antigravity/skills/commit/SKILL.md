---
name: commit
description: Analiza los cambios en git (staged o working tree), extrae automáticamente el Ticket ID de la rama (#ID), genera un commit con Conventional Commits y descripción técnica estructurada (resumen + mejoras/fixes) sin listar archivos uno por uno.
---

# Skill: Commit & Staging Inteligente

Esta skill se activa cuando el usuario escribe `/commit` o `/git-add`, o solicita preparar y registrar cambios en Git.

## Instrucciones de ejecución

1. **Revisar estado de Git y Rama**:
   - Obtener la rama actual: `git branch --show-current`.
   - Extraer el ID del ticket de 6 dígitos del nombre de la rama (ejemplo: si la rama es `feature/264099` o `264099-fix-grid`, el ID es `#264099`). Si la rama no contiene un ID de 6 dígitos numérico, omitir el prefijo `#ID` o solicitar aclaración solo si es indispensable.

2. **Staging de Cambios**:
   - Comprobar archivos modificados y en staging: `git status -s`.
   - Si no hay archivos en staging pero existen cambios en el working tree, ejecutar `git add .` (o añadir los archivos relevantes para el cambio solicitado).

3. **Analizar las Diferencias (Diff)**:
   - Inspeccionar las diferencias con `git diff --cached`.
   - Evaluar refactorizaciones, nuevas funcionalidades, correcciones de errores, estilos o configuraciones.

4. **Redactar el Mensaje de Commit**:
   - **Formato del Título (OBLIGATORIO)**:
     - Debe comenzar obligatoriamente con el ID precedido de `#` seguido de un espacio (ej: `#264099 fix(DatePicker): corregir offset en timezone` o `#264099 feat(auth): agregar soporte para refresh token`).
     - Seguir la convención Conventional Commits: `tipo(scope): descripción imperativa y concisa`.
     - Tipos válidos: `feat`, `fix`, `refactor`, `perf`, `style`, `test`, `docs`, `chore`.
   - **Cuerpo del Commit (Descripción Técnica Estructurada)**:
     - **Resumen**: Síntesis conceptual de 2 a 4 líneas de lo que se implementó o corrigió.
     - **Regla estricta**: **PROHIBIDO listar archivos uno por uno** en la descripción.
     - **Mejoras y Fixes**: Lista con viñetas limpias de los beneficios técnicos, problemas resueltos, optimizaciones de rendimiento o contratos de API ajustados.

5. **Ejecutar el Commit**:
   - Ejecutar el commit local:
     ```bash
     git commit -m "<Título>" -m "<Cuerpo estructurado>"
     ```

6. **Control de Push**:
   - **Por defecto**: Mantener el commit en el entorno local para que el usuario tenga control total.
   - **Si el usuario solicitó explícitamente push** (o pasó el flag correspondiente):
     ```bash
     git push origin <rama-actual>
     ```

7. **Confirmación**:
   - Mostrar al usuario un resumen conciso con el título y cuerpo del commit generado.
