---
trigger: model_decision
description: 'UI Components and design principles standards'
applyTo: 'src/baseComponents/**, src/pages/**, src/providers/**, src/components/**'
---

# Components

## Priority Rule: Use `@sice/frontend-components` First

Before creating any UI element, check `@sice/frontend-components` for an existing component. Using them ensures design consistency.

```typescript
// ❌ WRONG — creating a custom button
<button onClick={handleClick} className="my-btn">Submit</button>

// ✅ CORRECT — using library Button
import { Button } from '@sice/frontend-components';
<Button label="Submit" onClick={handleClick} />
```

## Component Import Priority

1. **`@sice/frontend-components`** — primary UI library (Button, Table, Input, Icons, DatePicker, useToaster, etc.)
2. **`src/baseComponents/`** — project-specific wrappers and composite components

```typescript
// 1st choice — library component
import { Button, Icons, Input, useToaster } from '@sice/frontend-components';

// 2nd choice — project base component (wraps library or project-specific logic)
import { AccessDenied, PageNotFound } from 'src/baseComponents';
```

Always import project base components from the barrel file:
```typescript
import { AccessDenied, PageNotFound, UnderConstruction } from 'src/baseComponents';
```

## Creating New Base Components

When a required component doesn't exist in `@sice/frontend-components`, create a wrapper in `src/baseComponents/` before writing inline UI. Follow the pattern:

```typescript
// src/baseComponents/MyWidget/MyWidget.tsx
interface IMyWidget {
  value: string;
  onChange: (value: string) => void;
}

const MyWidget = ({ value, onChange }: IMyWidget) => {
  return <div className={styles.widget}>{value}</div>;
};

export default MyWidget;
```

Then export from the barrel: `src/baseComponents/index.ts`.

## Component Design Principles

- **Functional only** — no class components
- **Single responsibility** — one component = one concern
- **Max ~20 lines** of JSX logic — extract sub-components if larger
- **No inline styles** — use CSS files in `src/styles/`
- **Typed props** — always define an `interface I<ComponentName>` for props
- **No `React.FC`** — this project avoids it; use direct function declarations

```typescript
// ❌ WRONG
const Card: React.FC<{ title: string }> = ({ title }) => <div>{title}</div>;

// ✅ CORRECT
interface ICard { title: string }
const Card = ({ title }: ICard) => <div>{title}</div>;
```

## Composition Patterns

Prefer composition over prop drilling:

```typescript
// ✅ Compound components for related UI
const Form = ({ children }: { children: React.ReactNode }) => <form>{children}</form>;
Form.Field = ({ label, input }: IFormField) => <div><label>{label}</label>{input}</div>;
```

## If a New Component Is Missing

1. Check `@sice/frontend-components` first — if it exists there, use it
2. Create it in `src/baseComponents/<ComponentName>/<ComponentName>.tsx`
3. Define its interface in the same file or `src/interfaces/`
4. Export from `src/baseComponents/index.ts`
5. Add docs to `docs/` if the component is complex
