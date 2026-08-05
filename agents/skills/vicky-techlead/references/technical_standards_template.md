# Plantilla de Artefacto: Estándares Técnicos (`technical_standards.md`)

```markdown
# Estándares Técnicos de Código y Scaffolding

**Proyecto**: [Nombre del Producto]
**Fecha**: [Fecha Actual]
**Tech Lead / Arquitecta**: Vicky (Technical Architect)

---

## 1. Screaming Architecture & Estructura de Carpetas

```text
src/
├── modules/
│   ├── [feature_1]/
│   │   ├── components/
│   │   ├── services/
│   │   ├── hooks/
│   │   └── types/
│   └── [feature_2]/
└── shared/
    ├── ui/
    ├── lib/
    └── utils/
```

---

## 2. Convenciones de Código y Patrones
- **Result Pattern**: Las funciones asíncronas devuelven `{ ok: boolean, data?: T, error?: String }`.
- **Early Returns**: Validación previa de casos límite antes de la ejecución principal.
- **SOLID**: Single Responsibility por archivo y módulo.

---

## 3. Calidad de Código y Linters
- **Linter & Formatter**: ESLint + Prettier / Biome.
- **Chequeo de Tipos**: TypeScript en modo estricto (`strict: true`).

---

## 4. Estrategia de Testing y Scaffolding
- **Pruebas Unitarias**: Vitest / Jest para servicios del dominio.
- **Pruebas de Componentes**: React Testing Library para componentes compartidos.
```
