---
name: git-add
description: Realiza git add ., analiza la petición y los cambios para hacer un git commit respetando el formato obligatorio (#ID_TICKET título) y sin nombrar archivo por archivo ni hacer push.
---

# Skill: /git-add

Esta skill se activa cuando el usuario escribe `/git-add` o solicita realizar un staging y commit rápido de sus cambios sin hacer push.

## Instrucciones de ejecución

1. **Staging de cambios**:
   - Ejecutar el comando `git add .` para incluir todos los cambios en staging.

2. **Obtener la rama y el Ticket ID**:
   - Obtener la rama actual con `git branch --show-current`.
   - Extraer el ID del ticket de 6 dígitos del nombre de la rama (ej. si la rama es `feature/264099` o `264099`, el ID es `#264099`).

3. **Analizar los cambios (Diff)**:
   - Inspeccionar las diferencias con `git diff --cached` y `git status -s`.
   - Analizar el contexto del usuario y las modificaciones en el código.

4. **Redactar el mensaje de commit**:
   - **Formato del Título (OBLIGATORIO)**:
     - **Debe comenzar obligatoriamente con el ID de 6 dígitos precedido de `#` seguido de un espacio** (ej: `#264099 fix(DatePicker): ...` o `#264099 feat: ...`).
     - Seguir la convención Conventional Commits en el resto del título (`#264099 fix(scope): descripción breve`).
   - **Cuerpo (Descripción técnica breve)**:
     - **Resumen**: Explicación general del cambio o motivo.
     - **Descripción técnica breve**: Breve síntesis conceptual de lo que se ha realizado (de 2 a 4 líneas), **SIN NOMBRAR ARCHIVOS UNO POR UNO**.
     - **Ventajas y Fixes**: Lista limpia con las ventajas logradas, problemas corregidos o mejoras de rendimiento/estabilidad obtenidas con los cambios.

5. **Ejecutar el Commit**:
   - Ejecutar `git commit -m "<Título>" -m "<Cuerpo>"`

6. **Finalización y entrega**:
   - **PROHIBIDO hacer `git push`**. El repositorio debe quedar listo para que el usuario realice el push manualmente cuando lo desee.
   - Mostrar al usuario un resumen del commit realizado.
