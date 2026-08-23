---
name: react-typescript-clean-code
description: Guía maestra y estándares de ingeniería para React moderno (18/19+) y TypeScript. Enfatiza simplicidad sobre complejidad, código funcional puro, uso juicioso de hooks (useEffect, useMemo, useCallback), patrones de rendimiento reales (virtualización, code-splitting), Clean Architecture y prevención de antipatrones.
---

# React & TypeScript: Clean Architecture, Simplicity & Performance

Guía de ingeniería para el desarrollo de aplicaciones frontend en React con TypeScript. Prioriza la **simplicidad radical**, la previsibilidad funcional y el rendimiento medible sobre la sobreingeniería y la optimización prematura.

---

## 🧭 1. Principio Rector: Simplicidad sobre Complejidad

> *"El código más rápido y con menos bugs es el que no se escribe o el que se mantiene simple."*

1. **Evitar la optimización prematura**: No envolver cada variable o función en `useMemo`/`useCallback` por defecto. El costo de memoria y sobrecarga cognitiva suele superar el supuesto beneficio.
2. **Priorizar funciones puras**: Extraer la lógica de cálculo fuera de los componentes React en funciones puras de TypeScript fáciles de probar.
3. **Componentes pequeños y con responsabilidad única**: Máximo ~200 líneas por componente. Si crece, extraer lógica a custom hooks o subcomponentes de presentación.
4. **Prohibición de `class` y `this`**: 100% código funcional con TypeScript estricto.

---

## 🔍 2. Diagnóstico y Uso Juicioso de Hooks

### A. `useEffect`: ¿Cuándo Usar y Cuándo EVITAR?

`useEffect` es una compuerta de escape para sincronizarse con **sistemas externos**. **NO** es un mecanismo para manejar eventos de usuario ni para sincronizar estado interno.

| Caso de Uso | ¿Usar `useEffect`? | Alternativa Correcta (Simplicidad) |
| :--- | :---: | :--- |
| **Calcular estado derivado** | ❌ **NUNCA** | Calcular directamente en el cuerpo del componente durante el render. |
| **Resetear estado al cambiar un prop** | ❌ **EVITAR** | Usar una `key` única en el componente para forzar el reinicio natural de React. |
| **Manejar acciones de usuario (clicks, submit)** | ❌ **NUNCA** | Ejecutar la lógica dentro del event handler (`onClick`, `onSubmit`). |
| **Data Fetching manual** | ❌ **EVITAR** | Usar **TanStack Query** (maneja caché, deduplicación, abort y reintentos). |
| **Suscripción a eventos externos (DOM global, WebSockets, Canvas, Timers)** | ✅ **SÍ (Con Cleanup)** | Sincronización externa con función de limpieza obligatoria en el `return`. |

#### ❌ Antipatrón: Estado derivado con `useEffect` (Doble Render + Lag)
```tsx
// MAL: Provoca un render extra con estado inconsistente
const ProductSummary = ({ price, discount }: Props) => {
  const [total, setTotal] = useState(0);

  useEffect(() => {
    setTotal(price - discount);
  }, [price, discount]);

  return <div>Total: ${total}</div>;
};
```

#### ✅ Solución Limpia: Cálculo en Render
```tsx
// BIEN: Sin hooks adicionales, síncrono y sin bugs
const ProductSummary = ({ price, discount }: Props) => {
  const total = price - discount;
  return <div>Total: ${total}</div>;
};
```

#### ✅ Uso Legítimo: Sincronización Externa con Cleanup
```tsx
export const useWindowWidth = () => {
  const [width, setWidth] = useState(() => window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);

    // OBLIGATORIO: Limpieza para evitar memory leaks
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return width;
};
```

---

### B. `useMemo`: ¿Cuándo es Realmente Necesario?

`useMemo` almacena en caché el resultado de un cálculo entre re-renders.

| Escenario | ¿Usar `useMemo`? | Motivo Técnico |
| :--- | :---: | :--- |
| Operaciones simples (`a + b`, `.length`, `toLowerCase()`) | ❌ **NO** | El costo de crear closures y chequear dependencias es mayor que el cálculo. |
| Arrays pequeños (< 500 elementos) | ❌ **NO** | Las CPUs modernas procesan miles de operaciones en menos de 0.1ms. |
| Filtrado/ordenamiento pesado (> 1,000 elementos o transformaciones O(n²)) | ✅ **SÍ** | Evita congelar el hilo principal en renders recurrentes. |
| Preservar igualdad referencial de objetos pasados a `React.memo` o dependencias de hooks | ✅ **SÍ** | Evita invalidar la memoización de componentes hijos optimizados. |

#### ❌ Antipatrón: Memoización trivial innecesaria
```tsx
// MAL: Sobreingeniería para una operación instantánea
const fullName = useMemo(() => `${firstName} ${lastName}`, [firstName, lastName]);
const itemsCount = useMemo(() => items.length, [items]);
```

#### ✅ Solución: Simple y directo
```tsx
// BIEN: Expresivo, sin memoria extra ni overhead
const fullName = `${firstName} ${lastName}`;
const itemsCount = items.length;
```

#### ✅ Uso Legítimo: Filtrado de gran volumen
```tsx
// BIEN: Justificado por volumen de datos
const visibleItems = useMemo(() => {
  return largeDataset
    .filter((item) => item.status === activeFilter)
    .sort((a, b) => b.timestamp - a.timestamp);
}, [largeDataset, activeFilter]);
```

---

### C. `useCallback`: ¿Cuándo Usar y Cuándo Omitir?

`useCallback` preserva la **referencia de una función** entre renders.

| Escenario | ¿Usar `useCallback`? | Motivo Técnico |
| :--- | :---: | :--- |
| Callbacks pasados a elementos HTML nativos (`<button onClick={...}>`) | ❌ **NO** | Los elementos HTML siempre se re-evalúan en el virtual DOM; no aporta beneficio. |
| Callbacks pasados a componentes hijos normales (no envueltos en `React.memo`) | ❌ **NO** | El hijo se re-renderizará de todas formas cuando el padre cambie. |
| Callbacks pasados a componentes hijos optimizados con `React.memo` | ✅ **SÍ** | Evita que el `React.memo` del hijo se invalide en cada render del padre. |
| Funciones que forman parte del array de dependencias de un `useEffect` o custom hook | ✅ **SÍ** | Evita que el efecto se ejecute en cada ciclo de render. |

#### ❌ Antipatrón: `useCallback` en componentes sin memoización
```tsx
// MAL: Gasto innecesario de memoria y sintaxis ruidosa
export const UserList = () => {
  const handleClick = useCallback((id: string) => {
    console.log('Clicked', id);
  }, []);

  return (
    <div>
      {/* NativeButton no está memoizado con React.memo */}
      <NativeButton onClick={handleClick} />
    </div>
  );
};
```

#### ✅ Solución: Función directa estándar
```tsx
// BIEN: Código limpio y directo
export const UserList = () => {
  const handleClick = (id: string) => {
    console.log('Clicked', id);
  };

  return <NativeButton onClick={handleClick} />;
};
```

#### ✅ Uso Legítimo: Callback hacia hijo memoizado
```tsx
// El hijo está explícitamente memoizado
const MemoizedRow = React.memo(({ item, onDelete }: RowProps) => {
  return (
    <div>
      <span>{item.name}</span>
      <button onClick={() => onDelete(item.id)}>Eliminar</button>
    </div>
  );
});

export const DataTable = ({ items }: TableProps) => {
  // BIEN: Mantiene la referencia estable para no invalidar el memo de MemoizedRow
  const handleDelete = useCallback((id: string) => {
    api.deleteItem(id);
  }, []);

  return (
    <div>
      {items.map((item) => (
        <MemoizedRow key={item.id} item={item} onDelete={handleDelete} />
      ))}
    </div>
  );
};
```

---

## ⚡ 3. Técnicas de Rendimiento de Alto Impacto

### A. Virtualización de Listas Extensas
Cuando se renderizan listas con más de 50-100 elementos, la virtualización es 10x más efectiva que cualquier micro-optimización con `useMemo`.

- **Web**: Utilizar `@tanstack/react-virtual`.
- **React Native**: Utilizar `@shopify/flash-list`.

```tsx
import { useVirtualizer } from '@tanstack/react-virtual';
import { useRef } from 'react';

export const VirtualList = ({ items }: { items: Item[] }) => {
  const parentRef = useRef<HTMLDivElement>(null);

  const rowVirtualizer = useVirtualizer({
    count: items.length,
    getScrollElement: () => parentRef.current,
    estimateSize: () => 48,
    overscan: 5,
  });

  return (
    <div ref={parentRef} style={{ height: '400px', overflow: 'auto' }}>
      <div style={{ height: `${rowVirtualizer.getTotalSize()}px`, position: 'relative' }}>
        {rowVirtualizer.getVirtualItems().map((virtualRow) => (
          <div
            key={virtualRow.key}
            style={{
              position: 'absolute',
              top: 0,
              left: 0,
              width: '100%',
              height: `${virtualRow.size}px`,
              transform: `translateY(${virtualRow.start}px)`,
            }}
          >
            {items[virtualRow.index].name}
          </div>
        ))}
      </div>
    </div>
  );
};
```

### B. Code-Splitting por Rutas con `React.lazy`
Dividir el bundle principal cargando rutas y componentes pesados bajo demanda.

```tsx
import { lazy, Suspense } from 'react';
import { Routes, Route } from 'react-router-dom';

const DashboardModule = lazy(() => import('./modules/Dashboard'));
const SettingsModule = lazy(() => import('./modules/Settings'));

export const AppRoutes = () => (
  <Suspense fallback={<div className="loading-spinner">Cargando...</div>}>
    <Routes>
      <Route path="/dashboard" element={<DashboardModule />} />
      <Route path="/settings" element={<SettingsModule />} />
    </Routes>
  </Suspense>
);
```

---

## 🛡️ 4. TypeScript Estricto & Tipado Limpio

1. **Cero `any`**: Usar `unknown`, genéricos `T` o interfaces específicas.
2. **Discriminated Unions en lugar de Enums**:
   ```typescript
   // ❌ MAL: Enums numéricos o heterogéneos
   enum Status { Active, Inactive }

   // ✅ BIEN: Tipos literales discriminados (type-safe y tree-shakeable)
   type Status = 'active' | 'inactive' | 'pending';
   ```
3. **Props con interfaces descriptivas**:
   ```typescript
   interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
     variant?: 'primary' | 'secondary' | 'danger';
     isLoading?: boolean;
   }
   ```
4. **Result Pattern en Servicios**:
   ```typescript
   type Result<T, E = string> = 
     | { success: true; value: T }
     | { success: false; error: E };
   ```

---

## 📋 5. Checklist de Verificación y Calidad

Antes de dar por completado cualquier componente o módulo:

- [ ] **Simplicidad**: ¿Se calculó el estado derivado durante el render en lugar de usar un `useEffect`?
- [ ] **Hooks justificados**: ¿Todos los `useMemo` y `useCallback` tienen una justificación técnica real (hijos con `React.memo` o datasets pesados)?
- [ ] **Efectos limpios**: ¿Cada `useEffect` que suscribe eventos externos cuenta con su respectiva función de retorno/cleanup?
- [ ] **TypeScript**: ¿Cero `any` y tipos estrictos en todas las props y firmas de función?
- [ ] **Tamaño de componentes**: ¿Los componentes se mantienen bajo ~200 líneas y desacoplados de lógica pesada?
- [ ] **Cero fugas de render**: ¿Los selectores de estado global están protegidos con `useShallow` o selectores atómicos?
