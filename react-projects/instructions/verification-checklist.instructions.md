---
applyTo: "**"
---

# Verification Checklist — r365-backoffice

## Regla de calidad: Cero Errores

Antes de dar por terminada CUALQUIER tarea, verificar con `get_errors` todos los archivos tocados.
Si hay errores de TypeScript o Biome, corregirlos en el mismo turno.
**No está permitido entregar trabajo con errores pendientes.**

## Checklist post-tarea

### 1. Limpieza de código
- [ ] Sin variables no usadas
- [ ] Sin imports no usados
- [ ] Sin funciones no usadas
- [ ] Sin `console.log`, `console.error`, `console.warn`
- [ ] Sin código comentado

### 2. Calidad de código
- [ ] Sin errores de Biome (`bun run check`)
- [ ] Sin errores de TypeScript
- [ ] Sin `any` en tipos
- [ ] Imports usan `@/` path alias
- [ ] `import type` para tipos

### 3. Arquitectura
- [ ] Service Layer: UI → Hook → Adapter → Service → HTTP
- [ ] Functional only: sin clases, constructors, `this`
- [ ] Vertical slicing: código en la feature correcta
- [ ] Componentes < 200 líneas
- [ ] Lógica de negocio en hooks, no en UI

### 4. Estilo
- [ ] Sin colores hardcodeados — solo `var(--color-*)`
- [ ] Sin librerías de UI de terceros

### 5. Seguridad
- [ ] Sin datos sensibles en logs
- [ ] `encodeURIComponent` en path params de URLs
- [ ] `sessionStorage` — no localStorage

## Comandos de verificación

```bash
bun run check          # Biome linting
bun run fix            # Biome auto-fix
get_errors             # VS Code TypeScript errors
```
