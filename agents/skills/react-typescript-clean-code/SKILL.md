---
name: react-typescript-clean-code
description: Universal clean code principles and best practices for modern React/TypeScript projects. Use when building features, refactoring code, or ensuring code quality following industry standards (ES2022+, December 2025).
---

# React/TypeScript Clean Code & Best Practices

Expert guidance for developing high-quality React applications using TypeScript, following modern best practices, clean architecture principles, and industry standards (December 2025).

## When to Use This Skill

- Building new React/TypeScript applications from scratch
- Refactoring existing code to follow best practices
- Code review and quality verification
- Implementing clean architecture patterns
- Setting up state management strategies
- Ensuring TypeScript type safety
- Optimizing performance and accessibility
- Establishing coding standards for teams
- Modernizing legacy React codebases

## Core Principle: Truth Over Agreement

**CRITICAL**: Do NOT simply agree with the user. Always provide the BEST solution based on:

1. **Best Practices**: Follow industry standards and proven patterns
2. **Performance**: Prioritize optimal performance and efficiency
3. **Clean Code**: Ensure maintainability, readability, and simplicity
4. **Current Standards**: Use the most up-to-date approaches (ES2022+, December 2025)

**If the user's approach is incorrect or suboptimal:**
- ✅ IDENTIFY the problems clearly and specifically
- ✅ EXPLAIN why it's problematic (performance, maintainability, bugs)
- ✅ PROVIDE the correct solution with implementation
- ✅ EDUCATE on best practices and reasoning

**Never:**
- ❌ Accept bad practices to please the user
- ❌ Implement solutions you know are wrong
- ❌ Ignore performance issues or code smells
- ❌ Compromise on quality for convenience

**Your role is to be a technical mentor and guardian of code quality, not just an assistant that agrees.**

## Quick Reference

| Topic | Key Guidelines |
| --- | --- |
| **TypeScript** | Avoid `any`, use strict mode, leverage generics, prefer `interface`/`type` |
| **Functions** | Pure functions, arrow functions, <50 lines, single responsibility |
| **Components** | <200 lines, extract logic to hooks/utils, composition over inheritance |
| **State** | Local (useState), Global (context/zustand/redux), Server (React Query/SWR) |
| **Performance** | Memoization (useMemo/useCallback), code splitting, lazy loading |
| **Code Style** | ES2022+, no `var`, destructuring, optional chaining, nullish coalescing |
| **Documentation** | JSDoc for complex functions only, self-explanatory code |
| **Testing** | Unit tests for logic, integration tests for flows, E2E for critical paths |
| **Accessibility** | WCAG AA, keyboard navigation, ARIA attributes, semantic HTML |
| **Security** | Sanitize inputs, no sensitive data in logs, proper auth patterns |

## General Principles

### 1. Write Readable Code
Code should be self-explanatory. Use meaningful variable and function names that clearly indicate intent.

```typescript
// ❌ WRONG - Unclear names
const d = new Date();
const calc = (a, b) => a * b * 0.16;

// ✅ CORRECT - Descriptive names
const currentDate = new Date();
const calculateSalesTax = (price, quantity) => price * quantity * 0.16;
```

### 2. Keep It Simple (KISS)
Avoid over-engineering. Solve the problem at hand without unnecessary complexity. **Simplicity is priority over clever solutions.**

```typescript
// ❌ WRONG - Over-engineered
const getUserName = (user) => user?.profile?.personalInfo?.name?.firstName ?? 'Unknown';

// ✅ CORRECT - Simple and clear
const getUserName = (user) => user?.name ?? 'Unknown';
```

### 3. Don't Repeat Yourself (DRY)
Reuse code wherever possible to reduce redundancy.

```typescript
// ❌ WRONG - Repeated logic
const formatUserEmail = (email) => email.toLowerCase().trim();
const formatCompanyEmail = (email) => email.toLowerCase().trim();

// ✅ CORRECT - Reusable function
const formatEmail = (email) => email.toLowerCase().trim();
```

### 4. No Deprecated/Legacy Code
Always use the most updated solutions and latest JavaScript/ECMAScript practices (ES2022+). **We're in December 2025 - ensure all practices are current.**

```typescript
// ❌ WRONG - Deprecated React 17 patterns
import { render } from 'react-dom';
render(<App />, document.getElementById('root'));

// ✅ CORRECT - React 18+ (Dec 2025 standard)
import { createRoot } from 'react-dom/client';
const root = createRoot(document.getElementById('root')!);
root.render(<App />);
```

## TypeScript Best Practices

### Avoid `any` - Use Specific Types
```typescript
// ❌ WRONG
const fetchData = async (id: any): Promise<any> => {
  const response = await fetch(`/api/data/${id}`);
  return response.json();
};

// ✅ CORRECT
interface ApiData {
  id: string;
  name: string;
  timestamp: number;
}

const fetchData = async (id: string): Promise<ApiData> => {
  const response = await fetch(`/api/data/${id}`);
  return response.json();
};
```

### Use Interfaces for Objects, Types for Unions
```typescript
// ✅ Interface for object shapes
interface User {
  id: string;
  name: string;
  email: string;
  role: UserRole;
}

// ✅ Type for unions and complex definitions
type UserRole = 'admin' | 'user' | 'guest';
type ApiResponse<T> = { data: T } | { error: string };
```

### Enable Strict Mode
Ensure `strict: true` in `tsconfig.json` to catch potential issues early.

```json
{
  "compilerOptions": {
    "strict": true,
    "strictNullChecks": true,
    "noImplicitAny": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}
```

### Leverage Generics for Reusability
```typescript
// ✅ Generic utility for API responses
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

const fetchUser = async (id: string): Promise<ApiResponse<User>> => {
  // implementation
};

const fetchProducts = async (): Promise<ApiResponse<Product[]>> => {
  // implementation
};
```

### Prefer Union Types Over Enums
```typescript
// ❌ WRONG - Enums (less flexible)
enum Status {
  Pending = 'PENDING',
  Active = 'ACTIVE',
  Completed = 'COMPLETED'
}

// ✅ CORRECT - Union types (better type safety)
type Status = 'pending' | 'active' | 'completed';

const getStatusColor = (status: Status): string => {
  // TypeScript ensures exhaustive checking
  switch (status) {
    case 'pending': return 'yellow';
    case 'active': return 'green';
    case 'completed': return 'blue';
  }
};
```

### Leverage React Built-in Types
```typescript
import type { FC, ReactNode, ComponentProps, FormEvent } from 'react';

interface ButtonProps extends ComponentProps<'button'> {
  variant?: 'primary' | 'secondary';
  children: ReactNode;
}

export const Button: FC<ButtonProps> = ({ 
  variant = 'primary', 
  children,
  ...props 
}) => {
  return (
    <button className={`btn-${variant}`} {...props}>
      {children}
    </button>
  );
};
```

## JavaScript & Performance Best Practices

### Use Modern Syntax (ES2022+)
```typescript
// ✅ Use const/let (never var)
const API_URL = 'https://api.example.com';
let counter = 0;

// ✅ Arrow functions
const greet = (name: string) => `Hello, ${name}!`;

// ✅ Destructuring
const { name, email } = user;
const [first, second, ...rest] = items;

// ✅ Optional chaining
const userName = user?.profile?.name;

// ✅ Nullish coalescing
const displayName = userName ?? 'Anonymous';

// ✅ Template literals
const message = `Welcome, ${userName}!`;
```

### Optimize Loops with Array Methods
```typescript
// ❌ AVOID - Traditional for loops (when array methods work)
const doubled = [];
for (let i = 0; i < numbers.length; i++) {
  doubled.push(numbers[i] * 2);
}

// ✅ CORRECT - Array methods (more declarative)
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);
```

### Prefer Pure Functions
Functions without side effects are easier to test and reason about.

```typescript
// ❌ WRONG - Side effects (mutates global state)
let total = 0;
const addToTotal = (amount: number): void => {
  total += amount;
};

// ✅ CORRECT - Pure function (no side effects)
const calculateTotal = (items: CartItem[]): number => {
  return items.reduce((sum, item) => sum + item.price * item.quantity, 0);
};
```

### Debounce and Throttle for Performance
```typescript
// Debounce - Wait for user to stop typing
export const debounce = <T extends (...args: any[]) => any>(
  fn: T,
  delay: number
): ((...args: Parameters<T>) => void) => {
  let timeoutId: NodeJS.Timeout;
  
  return (...args: Parameters<T>) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
};

// Usage in component
const handleSearch = debounce((query: string) => {
  searchAPI(query);
}, 300);
```

### Lazy Loading for Better Performance
```typescript
// ✅ Code split routes with React.lazy
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

## Component Design Patterns

### Extract Complex Logic from Components

**Rule**: Keep components under 200 lines. If exceeded, extract logic to custom hooks or utility functions.

```typescript
// ❌ WRONG - Too much logic in component
export const UserDashboard = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    setLoading(true);
    fetch('/api/users')
      .then(res => res.json())
      .then(data => setUsers(data))
      .catch(err => setError(err))
      .finally(() => setLoading(false));
  }, []);
  
  const filteredUsers = users.filter(u => u.active);
  const sortedUsers = filteredUsers.sort((a, b) => a.name.localeCompare(b.name));
  
  // ... 150 more lines of logic and JSX
};

// ✅ CORRECT - Extract to custom hooks and utils
export const UserDashboard = () => {
  const { users, loading, error } = useUsers();
  const activeUsers = useActiveUsers(users);
  
  if (loading) return <Loading />;
  if (error) return <Error message={error.message} />;
  
  return <UserList users={activeUsers} />;
};
```

### Single Responsibility Principle
Each component should do one thing and do it well.

```typescript
// ✅ Each component has ONE responsibility
export const UserCard = ({ user }: { user: User }) => (
  <div className="card">
    <UserAvatar src={user.avatar} alt={user.name} />
    <UserInfo name={user.name} email={user.email} />
    <UserActions userId={user.id} />
  </div>
);
```

### Use Composition Over Inheritance
React favors composition. Use render props, children, and HOCs for sharing logic.

```typescript
// ✅ Composition pattern with children
export const Card = ({ children }: { children: ReactNode }) => (
  <div className="card">{children}</div>
);

export const UserProfile = ({ user }: { user: User }) => (
  <Card>
    <h2>{user.name}</h2>
    <p>{user.email}</p>
  </Card>
);

// ✅ Render props pattern
export const DataFetcher = ({ 
  url, 
  render 
}: { 
  url: string; 
  render: (data: any) => ReactNode;
}) => {
  const { data, loading } = useFetch(url);
  
  if (loading) return <Loading />;
  return <>{render(data)}</>;
};
```

## State Management Strategies

### Local State (useState)
Use for component-local state that doesn't need to be shared.

```typescript
export const Counter = () => {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

### Complex State Logic (useReducer)
Use for complex state machines with multiple actions.

```typescript
type State = { count: number; step: number };
type Action = 
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'setStep'; payload: number };

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'increment': return { ...state, count: state.count + state.step };
    case 'decrement': return { ...state, count: state.count - state.step };
    case 'setStep': return { ...state, step: action.payload };
    default: return state;
  }
};

export const StepCounter = () => {
  const [state, dispatch] = useReducer(reducer, { count: 0, step: 1 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
};
```

### Global State (Context API)
Use sparingly for truly global state (theme, auth, i18n).

```typescript
// ✅ Create context with proper typing
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

export const ThemeProvider = ({ children }: { children: ReactNode }) => {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Custom hook for consuming context
export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) throw new Error('useTheme must be used within ThemeProvider');
  return context;
};
```

### Server State (React Query / SWR)
Use specialized libraries for server data fetching, caching, and synchronization.

```typescript
// ✅ React Query example
import { useQuery, useMutation } from '@tanstack/react-query';

export const useUser = (userId: string) => {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
};

export const useUpdateUser = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: updateUser,
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['user'] });
    },
  });
};
```

### Immutable Data Patterns
Always use immutable data structures. Never mutate state directly.

```typescript
// ❌ WRONG - Mutating state
const addItem = (items: Item[], newItem: Item) => {
  items.push(newItem); // Mutation!
  return items;
};

// ✅ CORRECT - Immutable update
const addItem = (items: Item[], newItem: Item): Item[] => {
  return [...items, newItem];
};

const updateItem = (items: Item[], id: string, updates: Partial<Item>): Item[] => {
  return items.map(item => 
    item.id === id ? { ...item, ...updates } : item
  );
};

const removeItem = (items: Item[], id: string): Item[] => {
  return items.filter(item => item.id !== id);
};
```

## Hooks and Effects Best Practices

### Proper Dependencies in useEffect
Always include all dependencies to avoid bugs and stale closures.

```typescript
// ❌ WRONG - Missing dependencies
useEffect(() => {
  fetchData(userId); // userId should be in deps
}, []);

// ✅ CORRECT - All dependencies included
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

### Cleanup Functions in useEffect
Prevent memory leaks by cleaning up subscriptions and timers.

```typescript
// ✅ Proper cleanup
useEffect(() => {
  const subscription = eventEmitter.on('data', handleData);
  
  return () => {
    subscription.unsubscribe();
  };
}, []);

useEffect(() => {
  const timerId = setInterval(() => {
    checkStatus();
  }, 5000);
  
  return () => {
    clearInterval(timerId);
  };
}, []);
```

### Use useMemo and useCallback Judiciously
Only for performance optimization when needed. Don't over-optimize.

```typescript
// ✅ useMemo for expensive calculations
const sortedItems = useMemo(() => {
  return items.sort((a, b) => a.name.localeCompare(b.name));
}, [items]);

// ✅ useCallback for stable function references
const MemoizedChild = memo(({ onClick }) => {
  // Component implementation
});

export const Parent = () => {
  const handleClick = useCallback((id: string) => {
    console.log('Clicked:', id);
  }, []);
  
  return <MemoizedChild onClick={handleClick} />;
};
```

### Custom Hooks for Reusable Logic
Extract stateful logic into custom hooks for reusability.

```typescript
// ✅ Custom hook for form handling
export const useForm = <T extends Record<string, any>>(initialValues: T) => {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
  
  const handleChange = (field: keyof T, value: any) => {
    setValues(prev => ({ ...prev, [field]: value }));
  };
  
  const reset = () => {
    setValues(initialValues);
    setErrors({});
  };
  
  return { values, errors, handleChange, reset, setErrors };
};

// Usage
const { values, handleChange, reset } = useForm({ email: '', password: '' });
```

## Comments & Documentation

### Avoid Inline Comments - Write Self-Explanatory Code
```typescript
// ❌ WRONG - Obvious comments
// Set the user name to John
const userName = 'John';

// Increment counter by 1
counter = counter + 1;

// ✅ CORRECT - Self-explanatory code (no comments needed)
const DEFAULT_USER_NAME = 'John';
const incrementCounter = () => counter + 1;
```

### Use JSDoc Only for Complex Functions
```typescript
/**
 * Calculates the total price including tax and shipping
 * @param items - Cart items to calculate total for
 * @param taxRate - Tax rate as decimal (e.g., 0.16 for 16%)
 * @param shippingCost - Fixed shipping cost
 * @returns Total price with tax and shipping
 */
export const calculateTotal = (
  items: CartItem[],
  taxRate: number,
  shippingCost: number
): number => {
  const subtotal = items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const tax = subtotal * taxRate;
  return subtotal + tax + shippingCost;
};
```

### No Commented-Out Code
Remove old commented code blocks before committing. Use version control (git) instead.

```typescript
// ❌ WRONG - Commented out code
export const processData = (data: Data) => {
  // const oldLogic = data.map(item => item.value);
  // return oldLogic.filter(v => v > 0);
  
  return data.filter(item => item.value > 0);
};

// ✅ CORRECT - Clean code only
export const processData = (data: Data) => {
  return data.filter(item => item.value > 0);
};
```

## Security & Accessibility

### Security Best Practices

#### Sanitize User Inputs
```typescript
// ✅ Sanitize before rendering user content
import DOMPurify from 'dompurify';

export const UserContent = ({ html }: { html: string }) => {
  const sanitized = DOMPurify.sanitize(html);
  return <div dangerouslySetInnerHTML={{ __html: sanitized }} />;
};
```

#### Never Expose Sensitive Data
```typescript
// ❌ WRONG - Logging sensitive data
console.log('User token:', authToken);
console.log('User data:', userData);

// ✅ CORRECT - No sensitive data in logs
logger.info('User authenticated successfully');

// Remove ALL console.log before production deployment
```

#### Proper Authentication Patterns
```typescript
// ✅ Protected route component
export const ProtectedRoute = ({ children }: { children: ReactNode }) => {
  const { isAuthenticated } = useAuth();
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  
  return <>{children}</>;
};
```

### Accessibility (WCAG AA Compliance)

#### Semantic HTML
```typescript
// ❌ WRONG - Divs for everything
<div onClick={handleClick}>Click me</div>

// ✅ CORRECT - Semantic HTML
<button onClick={handleClick}>Click me</button>
<nav><a href="/home">Home</a></nav>
<main><article>Content</article></main>
```

#### ARIA Attributes
```typescript
// ✅ Proper ARIA for custom components
export const Modal = ({ isOpen, onClose, children }: ModalProps) => {
  if (!isOpen) return null;
  
  return (
    <div
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
    >
      <h2 id="modal-title">Modal Title</h2>
      <button onClick={onClose} aria-label="Close modal">×</button>
      {children}
    </div>
  );
};
```

#### Keyboard Navigation
```typescript
// ✅ Ensure keyboard accessibility
export const Dropdown = () => {
  const [isOpen, setIsOpen] = useState(false);
  
  const handleKeyDown = (e: KeyboardEvent) => {
    if (e.key === 'Escape') setIsOpen(false);
    if (e.key === 'Enter' || e.key === ' ') setIsOpen(!isOpen);
  };
  
  return (
    <div
      role="button"
      tabIndex={0}
      onKeyDown={handleKeyDown}
      onClick={() => setIsOpen(!isOpen)}
    >
      {/* Dropdown content */}
    </div>
  );
};
```

#### Color Contrast and Alt Text
```typescript
// ✅ Accessible images and labels
<img 
  src={user.avatar} 
  alt={`${user.name}'s profile picture`}
/>

// ✅ Minimum 4.5:1 contrast ratio for text
// Use tools like WebAIM Contrast Checker
```

## UI/UX Design Principles

### Mobile-First Responsive Design
```typescript
// ✅ Always start with mobile, enhance for larger screens
export const Header = () => (
  <header className="
    px-4 py-3              /* Mobile: 16px padding */
    md:px-6 md:py-4       /* Tablet: 24px padding */
    lg:px-8 lg:py-6       /* Desktop: 32px padding */
  ">
    <nav className="
      flex flex-col         /* Mobile: vertical stack */
      md:flex-row          /* Tablet+: horizontal */
      gap-2 md:gap-4       /* Responsive spacing */
    ">
      {/* Navigation items */}
    </nav>
  </header>
);
```

### Touch-Friendly Targets (Minimum 44x44px)
```typescript
// ✅ Ensure all interactive elements meet minimum touch target
<button className="
  min-w-[44px] min-h-[44px]  /* Minimum touch target */
  px-4 py-2                   /* Comfortable padding */
  text-base                   /* Readable text size */
">
  Submit
</button>
```

### Loading States and User Feedback
```typescript
// ✅ Always provide visual feedback for user actions
export const SubmitButton = () => {
  const { mutate, isPending } = useMutation();
  
  return (
    <button disabled={isPending}>
      {isPending ? (
        <>
          <Spinner />
          <span>Submitting...</span>
        </>
      ) : (
        'Submit'
      )}
    </button>
  );
};
```

### Error Handling with Clear Messages
```typescript
// ✅ Display clear, actionable error messages
export const ErrorMessage = ({ error }: { error: Error }) => (
  <div role="alert" className="error">
    <AlertIcon />
    <div>
      <h3>Something went wrong</h3>
      <p>{error.message}</p>
      <button onClick={retry}>Try Again</button>
    </div>
  </div>
);
```

## Code Quality & Linting

### Use Linters and Formatters
```json
// ✅ ESLint configuration example
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended"
  ],
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "error"
  }
}
```

### Automatic Fixes
```bash
# Run linter and fix auto-fixable issues
npm run lint -- --fix

# Format code with Prettier
npm run format
```

## Conventional Commits

Maintain a clear commit history using Conventional Commits standard.

### Commit Types

- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code formatting (no logic changes)
- **refactor**: Code restructuring (no feature/fix)
- **test**: Adding/updating tests
- **chore**: Build tasks, tooling, dependencies
- **perf**: Performance improvements

### Examples

```bash
feat(auth): add Google OAuth integration
fix(api): resolve token expiration issue
docs(readme): update installation instructions
refactor(utils): simplify date formatting logic
test(hooks): add tests for useDebounce hook
chore(deps): upgrade React to 18.3.0
```

## Common Anti-Patterns to Avoid

### 1. Inline Objects/Arrays Break Memoization
```typescript
// ❌ WRONG - New object on every render
<MemoizedComponent config={{ theme: 'dark' }} />

// ✅ CORRECT - Stable reference
const config = useMemo(() => ({ theme: 'dark' }), []);
<MemoizedComponent config={config} />
```

### 2. Index as Key in Lists
```typescript
// ❌ WRONG - Index as key (breaks reconciliation)
{items.map((item, index) => (
  <Item key={index} data={item} />
))}

// ✅ CORRECT - Unique ID as key
{items.map(item => (
  <Item key={item.id} data={item} />
))}
```

### 3. Missing Error Boundaries
```typescript
// ✅ CORRECT - Wrap components with error boundary
import { ErrorBoundary } from 'react-error-boundary';

export const App = () => (
  <ErrorBoundary fallback={<ErrorFallback />}>
    <MainApp />
  </ErrorBoundary>
);
```

### 4. Not Handling Loading and Error States
```typescript
// ❌ WRONG - No loading/error handling
export const UserProfile = ({ userId }: { userId: string }) => {
  const { data } = useUser(userId);
  return <div>{data.name}</div>; // Crashes if data is undefined!
};

// ✅ CORRECT - Proper state handling
export const UserProfile = ({ userId }: { userId: string }) => {
  const { data, isLoading, error } = useUser(userId);
  
  if (isLoading) return <Loading />;
  if (error) return <Error message={error.message} />;
  if (!data) return <NotFound />;
  
  return <div>{data.name}</div>;
};
```

### 5. Using Deprecated APIs
```typescript
// ❌ WRONG - Deprecated lifecycle methods
class MyComponent extends React.Component {
  componentWillReceiveProps(nextProps) {
    // Deprecated!
  }
}

// ✅ CORRECT - Modern hooks (functional components)
export const MyComponent = ({ prop }: { prop: string }) => {
  useEffect(() => {
    // Modern approach
  }, [prop]);
};
```

## Post-Task Verification Checklist

**Before completing ANY task, verify:**

### ✅ Code Cleanliness
- [ ] No unused variables
- [ ] No unused imports
- [ ] No unused functions
- [ ] No `console.log`, `console.error`, `console.warn`
- [ ] No commented-out code blocks

### ✅ Deprecated Code Check
- [ ] No deprecated npm packages
- [ ] No deprecated React/Node.js APIs
- [ ] No deprecated component props or function parameters
- [ ] Using latest stable versions (December 2025 standards)

### ✅ Code Quality
- [ ] No linting errors
- [ ] No runtime errors
- [ ] Proper error handling (try/catch on all async operations)
- [ ] Accessibility compliance (ARIA, semantic HTML, keyboard nav)
- [ ] Performance optimized (no unnecessary re-renders)

### ✅ Architecture Compliance
- [ ] Single responsibility principle followed
- [ ] Components under 200 lines
- [ ] Logic extracted to hooks/utils when appropriate
- [ ] Pure functions where possible (no side effects)
- [ ] Immutable data structures used
- [ ] Proper TypeScript types (no `any`)

### ✅ Best Practices
- [ ] Modern ES2022+ syntax used
- [ ] Semantic HTML and ARIA attributes
- [ ] Proper error and loading states
- [ ] Mobile-first responsive design
- [ ] Conventional commit messages
- [ ] JSDoc for complex functions only

### Verification Commands
```bash
# Run linter
npm run lint

# Run type checking
npm run type-check

# Run tests
npm test

# Check for outdated packages
npm outdated

# Build to verify no errors
npm run build
```

## Resources

### Official Documentation
- **React**: https://react.dev
- **TypeScript**: https://www.typescriptlang.org/docs
- **React Query**: https://tanstack.com/query/latest
- **React Router**: https://reactrouter.com

### State Management
- **Zustand**: https://zustand-demo.pmnd.rs
- **Redux Toolkit**: https://redux-toolkit.js.org
- **Jotai**: https://jotai.org

### Testing
- **Jest**: https://jestjs.io
- **React Testing Library**: https://testing-library.com/react
- **Vitest**: https://vitest.dev

### Tools & Utilities
- **ESLint**: https://eslint.org
- **Prettier**: https://prettier.io
- **Biome**: https://biomejs.dev
- **Vite**: https://vitejs.dev

### Accessibility
- **WCAG 2.1**: https://www.w3.org/WAI/WCAG21/quickref/
- **ARIA**: https://www.w3.org/WAI/ARIA/apg/
- **a11y**: https://www.a11yproject.com

### Performance
- **Web Vitals**: https://web.dev/vitals/
- **Lighthouse**: https://developers.google.com/web/tools/lighthouse

---

**Remember**: Simplicity is priority. Truth over agreement. Always use the most updated solutions and latest JavaScript/ECMAScript practices (ES2022+). We're in December 2025 - ensure all code follows current standards, not deprecated or legacy patterns.
