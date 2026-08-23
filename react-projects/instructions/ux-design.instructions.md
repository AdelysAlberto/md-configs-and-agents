---
applyTo: "src/pages/**, src/layouts/**"
---

# UX & Design — r365-backoffice

## Principios UX del Backoffice

Este es un **panel de administración fintech** — priorizar:
1. **Claridad** — Datos legibles, jerarquía visual clara
2. **Eficiencia** — Mínimos clics para completar tareas
3. **Feedback** — Loading states, confirmaciones, errores claros
4. **Consistencia** — Mismos patrones en todas las páginas

## Patrones de página estándar

### Página de listado (CRUD)
```
┌─ Header: título + botón de acción principal ──────┐
│ Filtros (inline o collapsible)                     │
├────────────────────────────────────────────────────┤
│ BaseTable con datos                                │
│  - Loading → skeleton rows                         │
│  - Empty → empty state con mensaje                 │
│  - Error → error state con retry                   │
├────────────────────────────────────────────────────┤
│ BasePagination                                     │
└────────────────────────────────────────────────────┘
```

### Página de detalle
```
┌─ Header: breadcrumb + título + actions ────────────┐
│ BaseCard con secciones de información               │
│ Tabs si hay múltiples secciones                     │
└─────────────────────────────────────────────────────┘
```

## Estados obligatorios en toda página

| Estado | Componente | Cuándo |
|---|---|---|
| **Loading** | Skeleton / Spinner | `isLoading === true` |
| **Empty** | Empty state con icono | `data.length === 0` |
| **Error** | Error state con retry | `error !== null` |
| **Success** | Datos renderizados | data disponible |

```tsx
// ✅ Patrón obligatorio
const Page = () => {
  const { data, isLoading, error } = usePageData();
  if (isLoading) return <LoadingSkeleton />;
  if (error) return <ErrorState onRetry={refetch} />;
  if (!data?.length) return <EmptyState message="No hay datos" />;
  return <BaseTable columns={columns} data={data} />;
};
```

## Modales y confirmaciones

- Acciones destructivas (eliminar, desactivar) → **BaseModal de confirmación**
- Formularios de creación/edición → **BaseModal o página dedicada**
- Feedback de éxito/error → **Toast notification**

## Accesibilidad básica

- ✅ HTML semántico (`<main>`, `<nav>`, `<section>`, `<button>`)
- ✅ Atributos ARIA cuando sea necesario
- ✅ Navegación por teclado funcional
- ✅ Alt text en imágenes
- ❌ No `<div onClick>` cuando debería ser `<button>`

## Responsive

- Dashboard layout: sidebar colapsable en mobile
- Tablas: scroll horizontal en pantallas pequeñas
- Formularios: stack vertical en mobile
