# Estándares de Código y Screaming Architecture (Vicky - Tech Lead)

Este documento establece las reglas técnicas y convenciones de código defendidas por **Vicky** (Technical Architect).

---

## 1. Screaming Architecture (Vertical Slicing)

El proyecto debe organizarse siempre por módulos de negocio bajo la carpeta `modules/` o `features/`.

```text
src/
├── modules/
│   ├── auth/
│   │   ├── components/       # UI específica de autenticación
│   │   ├── hooks/            # Hooks/Lógica reactiva de auth
│   │   ├── services/         # Adaptadores HTTP / Clientes API
│   │   └── types/            # Tipos y esquemas de validación (Zod/DTOs)
│   ├── products/
│   │   ├── components/
│   │   ├── services/
│   │   └── types/
└── shared/                   # Solo utilidades transversales (UI primitives, logger, HTTP client)
```

---

## 2. Patrones de Diseño Obligatorios

1. **Result Pattern**: Evitar `try/catch/throw` para el control de flujo de negocio. Retornar siempre objetos estructurados `{ ok: true, data }` o `{ ok: false, error }`.
2. **KISS & Clean Code**: Funciones cortas (< 50 líneas), early returns, nombres descriptivos de variables y funciones.
3. **Adapter / Mapper Pattern**: Desacoplar la UI de las respuestas de API externas. Mapear DTOs externos a modelos de dominio antes de consumirlos en componentes UI.
4. **Selector Pattern en Estado Global**: Al usar Zustand o Redux, exigir el uso de selectores individuales para evitar re-renders innecesarios.
