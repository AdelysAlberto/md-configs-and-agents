---
applyTo: "src/components/**, src/pages/**/components/**"
---

# Componentes — r365-backoffice

## Regla fundamental: Sin librerías de UI de terceros

❌ No usar MUI, shadcn, Ant Design, Chakra, Radix ni similares.
✅ Todos los componentes son `Base*` custom en `src/components/`.
✅ Usar solo TailwindCSS + variables CSS del design system.

## Base Components disponibles

| Componente | Ubicación |
|---|---|
| `BaseAlert` | `src/components/BaseAlert/` |
| `BaseBadge` | `src/components/BaseBadge/` |
| `BaseButton` | `src/components/BaseButton/` |
| `BaseCard` | `src/components/BaseCard/` |
| `BaseInput` | `src/components/BaseInput/` |
| `BaseModal` | `src/components/BaseModal/` |
| `BasePagination` | `src/components/BasePagination/` |
| `BaseSelect` | `src/components/BaseSelect/` |
| `BaseTable` | `src/components/BaseTable/` |
| `ErrorBoundary` | `src/components/ErrorBoundary/` |

Barrel export: `src/components/index.ts`

## Props estándar de referencia

**BaseButton**:
```typescript
type ButtonVariant = 'primary' | 'secondary' | 'ghost' | 'danger';
type ButtonSize = 'sm' | 'md' | 'lg';
interface BaseButtonProps {
  variant?: ButtonVariant;
  size?: ButtonSize;
  loading?: boolean;
  disabled?: boolean;
  onClick?: () => void;
  children: React.ReactNode;
}
```

**BaseBadge**:
```typescript
type BadgeVariant = 'success' | 'danger' | 'warning' | 'info' | 'neutral';
interface BaseBadgeProps { variant: BadgeVariant; label: string; }
```

**BaseTable**:
```typescript
interface Column<T> {
  key: keyof T;
  header: string;
  render?: (value: T[keyof T], row: T) => React.ReactNode;
  sortable?: boolean;
}
interface BaseTableProps<T> {
  columns: Column<T>[];
  data: T[];
  loading?: boolean;
  onRowClick?: (row: T) => void;
}
```

## Reglas de diseño de componentes

- ✅ Componentes < 200 líneas — si excede, refactorizar
- ✅ Extraer lógica a custom hooks — componentes solo UI
- ✅ Barrel exports (`index.ts`) en cada carpeta de componente
- ✅ Nombres en `PascalCase` (`BaseButton`, `TransactionFilters`)
- ❌ No inline styles — siempre clases de Tailwind o CSS vars
- ❌ No lógica de negocio dentro del componente

## Ejemplo correcto vs incorrecto

```tsx
// ✅ CORRECTO — UI pura, lógica en hook
const UsersPage = () => {
  const { data, isLoading, error } = useUsers(filters);
  if (isLoading) return <BaseSkeleton />;
  if (error) return <ErrorState />;
  return <BaseTable columns={columns} data={data} />;
};

// ❌ INCORRECTO — fetch directo en componente
const UsersPage = () => {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch('/api/users').then(r => r.json()).then(setUsers);
  }, []);
  return <table>...</table>;
};
```
