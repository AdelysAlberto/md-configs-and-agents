---
description: Estándar estricto para el Sistema de Diseño y Librería de Componentes Reutilizables de React Frontend (DRY, Variables CSS, Estructura por carpetas)
applyTo: src/**/*.tsx, src/components/**/*
---

# 🎨 Reglas del Sistema de Diseño & Librería de Componentes Reutilizables (React)

## 📌 1. Principio DRY y Origen Único (Single Source of Truth)
- **Cero Duplicación de Elementos Interactivos**: Queda estrictamente prohibido maquetar botones, campos de entrada, etiquetas, modales, toasts o dropdowns inline directamente en las páginas o vistas de módulos.
- **Librería Centralizada**: Todo elemento visual reutilizable DEBE pertenecer a `src/components/` y consumirse desde allí. Si se modifica la apariencia o comportamiento de un componente (ej. `Button`), el cambio debe verse reflejado en el 100% de la aplicación al modificar ese único origen.

---

## 📁 2. Estructura Obligatoria por Carpeta de Componente

Cada componente de la librería DEBE residir dentro de su propia carpeta dedicada en `src/components/<ComponentName>/`:

```text
src/components/<ComponentName>/
├── <ComponentName>.tsx        # Código del componente funcional React (pure functional)
├── <ComponentName>.types.ts   # Definición estricta de Props y tipos TypeScript
└── index.ts                   # Exportación barril (barrel export) limpia
```

### Ejemplo de Exportación en `index.ts`:
```typescript
export * from "./Button";
export * from "./Button.types";
```

---

## 🎨 3. Invariante Obligatorio de Variables CSS Globales (`var(--...)`)

- **Prohibido Hardcodear Colores en Código o Clases en Línea**: Todos los colores (fondos, bordes, textos, acentos y estados) DEBEN utilizar variables CSS globales o tokens del tema (`var(--color-...)` o variantes configuradas centralmente en `index.css`).
- Si la marca cambia una tonalidad primaria o secundaria, el cambio DEBE realizarse exclusivamente en la definición de la variable en `index.css` sin necesidad de modificar el código de los componentes.

### Tokens Estándar Design System:
- `--color-primary`: Color primario institucional o de marca
- `--color-secondary`: Color secundario o de acento
- `--color-danger`: Estado crítico, errores o eliminaciones
- `--color-success`: Estado exitoso o confirmación
- `--color-bg-main`: Fondo principal de la aplicación
- `--color-bg-card`: Superficie y tarjetas elevadas
- `--color-border-card`: Bordes de separación y delimitadores

---

## 🧩 4. Catálogo Obligatorio de Componentes Base

1. **`Button`**:
   - Variantes requeridas: `primary`, `secondary`, `tertiary`, `danger`, `ghost`, `outline`.
   - Soporte para icono a la izquierda/derecha y estado de carga (`loading`).
   - Respetar `border-radius` estándar del sistema de diseño.
2. **`Input`**:
   - Campos de texto, contraseña, búsqueda, correo.
   - Soporte para `label`, `error`, `helperText` e `icon`.
3. **`Dropdown` / `Select`**:
   - Selector desplegable unificado para filtros y formularios.
4. **`Modal`**:
   - `InformationModal`: Modal de avisos, créditos o detalles.
   - `ConfirmationModal`: Modal interactivo de confirmación (aceptar/cancelar).
5. **`Toast`**:
   - `ToastProvider` & hook `useToast()` para notificaciones flotantes con variantes `success`, `error`, `warning`, `info`.
6. **`Image`**:
   - Componente de imagen con carga perezosa (`lazy`), esqueleto de carga (`skeleton`) y fallback antierrores.
7. **`Badge`**:
   - Etiquetas de estado e indicadores métricos.
8. **`Card`**:
   - Contenedor de superficie elevado con borde adaptativo.

---

## ⚙️ 5. Simplicidad & Código Funcional Puro
- Prohibidas las clases u orientaciones orientadas a objetos (`class`, `this`).
- Componentes sencillos, minimalistas y sin sobreingeniería.
