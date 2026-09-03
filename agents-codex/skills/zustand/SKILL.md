---
name: zustand
description: Patrones avanzados y mejores prácticas para la gestión de estado global y de UI con Zustand 5+. Abarca arquitectura funcional, selector hygiene con useShallow, prevención de bucles infinitos de render, slices modulares, aislamiento de almacenamiento seguro (sessionStorage / createAppStore) y acceso fuera de React.
---

# Zustand 5: Best Practices & Architecture

Guía integral para gestionar estado global y de UI con Zustand 5 de forma predecible, funcional y de alto rendimiento.

---

## 1. Reglas Fundamentales de Arquitectura

1. **Estado de Servidor vs Estado de UI**:
   - ❌ **No usar Zustand para datos del servidor**: Utilizar TanStack Query para fetching, caching, sincronización e invalidación.
   - ✅ **Usar Zustand para estado de UI / Cliente**: Modales, wizards, filtros activos de UI, sidebar colapsado, preferencias de sesión.
2. **Código Funcional Puro**:
   - Prohibido el uso de clases y `this`. Todos los stores se crean con funciones puras y tipos TypeScript explícitos.
3. **Nomenclatura y Estructura**:
   - Nombrar los hooks de los stores con el prefijo `use` (ej: `useAuthStore`, `useFilterStore`).
   - Siempre exponer una acción `reset()` para restaurar el estado inicial de manera limpia.

---

## 2. Creación de Stores & Persistencia Segura

### A. Store en Memoria (No persistente)
Ideal para estado volátil que debe reiniciarse al refrescar la página.

```typescript
import { create } from 'zustand';

interface ModalState {
  isOpen: boolean;
  activeId: string | null;
  openModal: (id: string) => void;
  closeModal: () => void;
  reset: () => void;
}

const initialState = {
  isOpen: false,
  activeId: null,
};

export const useModalStore = create<ModalState>((set) => ({
  ...initialState,
  openModal: (id) => set({ isOpen: true, activeId: id }),
  closeModal: () => set({ isOpen: false, activeId: null }),
  reset: () => set(initialState),
}));
```

### B. Store Persistente (Aislamiento de Sesión)
**ADVERTENCIA DE SEGURIDAD**: Evitar persistir directamente en `localStorage` datos de sesión o tokens sin aislamiento, ya que puede provocar fugas de datos entre usuarios en el mismo navegador. Preferir `sessionStorage` o el wrapper seguro del proyecto (`createAppStore`).

```typescript
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

interface WizardState {
  currentStep: number;
  draftData: Record<string, unknown>;
  setStep: (step: number) => void;
  updateDraft: (data: Record<string, unknown>) => void;
  reset: () => void;
}

const initialState = {
  currentStep: 1,
  draftData: {},
};

export const useWizardStore = create<WizardState>()(
  persist(
    (set) => ({
      ...initialState,
      setStep: (step) => set({ currentStep: step }),
      updateDraft: (data) =>
        set((state) => ({ draftData: { ...state.draftData, ...data } })),
      reset: () => set(initialState),
    }),
    {
      name: 'app-wizard-session',
      storage: createJSONStorage(() => sessionStorage), // Aislamiento por pestaña/sesión
      partialize: (state) => ({ currentStep: state.currentStep }), // Persistir solo lo necesario
    }
  )
);
```

---

## 3. Consumo Seguro & Prevención de Re-renders (`useShallow`)

### Regla Crítica en Zustand 5:
- ❌ **PROHIBIDO destructurar el store completo**: `const { prop1, prop2 } = useStore();` (Re-renderiza el componente ante **CUALQUIER** cambio en el store).
- ❌ **PROHIBIDO retornar objetos/arrays literales sin memoización**: Provoca un nuevo puntero de memoria en cada evaluación y causa bucles infinitos (`Maximum update depth exceeded`).

```tsx
// ❌ MAL: Retorna nueva referencia en cada render -> Bucle infinito potencial
const { currentStep, draftData } = useWizardStore((state) => ({
  currentStep: state.currentStep,
  draftData: state.draftData,
}));
```

### ✅ Solución 1: Selectores Atómicos (Recomendado para 1 o 2 valores)
```tsx
export const StepIndicator = () => {
  const currentStep = useWizardStore((state) => state.currentStep);
  const setStep = useWizardStore((state) => state.setStep);

  return <div>Paso actual: {currentStep}</div>;
};
```

### ✅ Solución 2: `useShallow` (Obligatorio al seleccionar múltiples propiedades en un objeto/array)
```tsx
import { useShallow } from 'zustand/react/shallow';

export const WizardHeader = () => {
  const { currentStep, reset } = useWizardStore(
    useShallow((state) => ({
      currentStep: state.currentStep,
      reset: state.reset,
    }))
  );

  return (
    <header>
      <h1>Paso {currentStep}</h1>
      <button onClick={reset}>Reiniciar</button>
    </header>
  );
};
```

---

## 4. Patrón de Slices Modulares (Stores Complejos)

Para módulos grandes, dividir el estado en slices independientes y combinarlos en un único store raíz.

```typescript
import { create, StateCreator } from 'zustand';

// Slices
interface UserSlice {
  user: { id: string; name: string } | null;
  setUser: (user: { id: string; name: string } | null) => void;
}

interface CartSlice {
  items: string[];
  addItem: (item: string) => void;
}

const createUserSlice: StateCreator<UserSlice & CartSlice, [], [], UserSlice> = (set) => ({
  user: null,
  setUser: (user) => set({ user }),
});

const createCartSlice: StateCreator<UserSlice & CartSlice, [], [], CartSlice> = (set) => ({
  items: [],
  addItem: (item) => set((state) => ({ items: [...state.items, item] })),
});

// Store unificado
export const useRootStore = create<UserSlice & CartSlice>()((...args) => ({
  ...createUserSlice(...args),
  ...createCartSlice(...args),
}));
```

---

## 5. Acciones Asíncronas & Result Pattern

Manejar errores explícitamente sin lanzar excepciones no controladas:

```typescript
interface ProductsState {
  items: Product[];
  isLoading: boolean;
  error: string | null;
  loadProducts: () => Promise<{ success: boolean; error?: string }>;
}

export const useProductsStore = create<ProductsState>((set) => ({
  items: [],
  isLoading: false,
  error: null,

  loadProducts: async () => {
    set({ isLoading: true, error: null });
    try {
      const response = await api.getProducts();
      if (!response.success) {
        set({ error: response.error, isLoading: false });
        return { success: false, error: response.error };
      }
      set({ items: response.value, isLoading: false });
      return { success: true };
    } catch (err) {
      const message = err instanceof Error ? err.message : 'Error inesperado';
      set({ error: message, isLoading: false });
      return { success: false, error: message };
    }
  },
}));
```

---

## 6. Acceso Fuera de Componentes React

Zustand permite leer o suscribirse al estado en servicios o interceptores de red sin hooks:

```typescript
// Leer estado actual
const token = useAuthStore.getState().token;

// Despachar acción
useAuthStore.getState().logout();

// Suscribirse a cambios
const unsubscribe = useAuthStore.subscribe((state) => {
  console.log('Estado de autenticación actualizado:', state.isAuthenticated);
});
```

---

## 7. Checklist de Calidad Zustand

- [ ] ¿El store maneja solo estado del cliente/UI y no duplica el caché del servidor?
- [ ] ¿Todos los selectores de múltiples propiedades usan `useShallow`?
- [ ] ¿Los stores exponen una acción `reset()` para limpieza?
- [ ] ¿La persistencia utiliza storage seguro (`sessionStorage` o particionado explícito)?
- [ ] ¿Los nombres de acciones son verbos claros y descriptivos?
