```markdown
AI, necesito que crees un proyecto React + TypeScript siguiendo ESTRICTAMENTE las instrucciones técnicas que te proporcionaré al final de este mensaje.

## 📱 DESCRIPCIÓN DE LA APLICACIÓN

Necesito una web app de comparación de precios siguiendo el patrón responsive-first, primordial mobile.

**Funcionalidades principales:**
- Búsqueda de productos por nombre (ej: "Mayonesa Ligeresa")
- Comparación de precios entre supermercados (Mercadona, Día, Alcampo, Lidl)
- Escaneo de código de barras para búsqueda automática
- Lista de resultados con precios ordenados de menor a mayor
- Botón para nueva consulta
- Historial de búsquedas recientes

## 🎨 COLORES DE MARCA / BRANDING

**IMPORTANTE:** Genera el sistema de variables CSS con estos colores siguiendo las reglas de las instrucciones técnicas.

### Colores Principales:
- **Color Primario:** #FF6B35
  - Descripción: Naranja vibrante para botones principales, CTA, y elementos interactivos
  
- **Color Secundario:** #004E89
  - Descripción: Azul corporativo para headers, navegación, y elementos secundarios
  
- **Color Terciario/Acento:** #FFB563
  - Descripción: Naranja claro para badges de descuentos y highlights

### Estética del Tema:
- **Tema:** Claro con soporte para modo oscuro
- **Estilo:** Moderno y amigable, orientado a móvil
- **Escala de grises:** Neutra (ni muy fría ni muy cálida)

### Colores de Estado (usar defaults):
- Usar los colores de estado por defecto del sistema (verde para éxito, rojo para error, etc.)

---

# Technical Project Initialization Prompt

**Context**: This is the technical foundation prompt for initializing a new React + TypeScript project following our established architecture patterns, coding standards, and best practices. This prompt should be used **FIRST** before any business logic or feature implementation.

---


## Core Principle: Truth Over Agreement

*CRITICAL RULE: Do NOT simply agree with the user. Always provide the BEST solution based on:*

1.⁠ ⁠*Best Practices*: Follow industry standards and proven patterns
2.⁠ ⁠*Performance*: Prioritize optimal performance and efficiency
3.⁠ ⁠*Clean Code*: Ensure maintainability, readability, and simplicity
4.⁠ ⁠*Current Standards*: Use the most up-to-date and recommended approaches

*If the user's approach is incorrect or suboptimal:*
•⁠  ⁠✅ *IDENTIFY the problems* clearly and specifically
•⁠  ⁠✅ *EXPLAIN why* it's problematic (performance, maintainability, bugs, etc.)
•⁠  ⁠✅ *PROVIDE the correct solution* with implementation
•⁠  ⁠✅ *EDUCATE* on best practices and reasoning

*Never:*
•⁠  ⁠❌ Accept bad practices to please the user
•⁠  ⁠❌ Implement solutions you know are wrong
•⁠  ⁠❌ Ignore performance issues or code smells
•⁠  ⁠❌ Compromise on quality for convenience

*Your role is to be a technical mentor and guardian of code quality, not just an assistant that agrees.*

## 🎯 Project Setup Objective

Create a complete, production-ready React + TypeScript project with the following technical specifications:

---

## 📋 Core Technology Stack

### Primary Technologies
- **React**: 19+ (latest stable)
- **TypeScript**: 5.9+ with strict mode enabled
- **Build Tool**: Vite 7+ 
- **Node.js**: 24+ with ESM modules exclusively
- **Package Manager**: **pnpm** (version 10+)
- **ECMAScript**: ES2022+ features 

### State Management & Data Fetching
- **Global State**: **Zustand** (5.0+) for persistent global state
- **Server State/Cache**: **TanStack Query** (React Query 5.90+) for API caching and server state
- **State Persistence**: 
  - **CookieStore API** (https://developer.mozilla.org/en-US/docs/Web/API/CookieStore) for persistent storage
  - **sessionStorage** for temporary session-based storage
  - ❌ **Never use localStorage** - use CookieStore instead

### Routing & Navigation
- **React Router**: 7.10+ (React Router v7)

### Internationalization
- **i18next**: 25.7+
- **react-i18next**: 16.4+

### Development Tools
- **Code Quality**: **Biome** (2.3+) for linting and formatting
- **Git Hooks**: **Husky** (9.1+) for pre-commit and pre-push validation
- **Testing**: Vitest (4.0+) with React Testing Library
- **TypeScript Validation**: Built-in TypeScript compiler

### Additional Libraries
- **Date Handling**: Luxon (3.7+)
- **Token Validation**: react-jwt (2.0+)
- **Schema Validation**: Zod (4.1+)
- **HTTP Client**: Native fetch API with custom wrapper

### React Router Configuration
- **React Router**: 7.10+ with `createBrowserRouter`
- **Lazy Loading**: Use `React.lazy()` for code splitting on routes
- **Nested Routes**: Organize routes with layouts and nested children
- **Route Guards**: Implement private/public route protection
- **Suspense Boundaries**: Wrap lazy-loaded routes with Suspense and loading fallbacks
- **Centralized Paths**: Define all route paths in `navigation/paths.ts` to avoid hardcoding and circular dependencies

---

## 🏗️ Project Architecture

### 📁 Project Root Structure

**CRITICAL - Configuration Files Location:**

All configuration files **MUST** be placed in the **project root directory**:

```
project-root/
├── package.json           # ✅ Root - Package configuration
├── tsconfig.json          # ✅ Root - TypeScript configuration
├── tsconfig.app.json      # ✅ Root - App-specific TS config
├── tsconfig.node.json     # ✅ Root - Node-specific TS config
├── vite.config.ts         # ✅ Root - Vite configuration
├── biome.json             # ✅ Root - Biome linting/formatting config
├── .gitignore             # ✅ Root - Git ignore rules
├── .env                   # ✅ Root - Environment variables
├── .env.example           # ✅ Root - Environment variables template
├── README.md              # ✅ Root - Project documentation
│
├── .husky/                # ✅ Root - Git hooks
│   ├── pre-commit
│   └── pre-push
│
├── docs/                  # ✅ Root - Additional documentation
│   ├── ARCHITECTURE.md    # Architecture documentation
│   ├── API.md             # API documentation
│   ├── DEPLOYMENT.md      # Deployment guide
│   └── CONTRIBUTING.md    # Contribution guidelines
│
├── public/                # Public static assets
│   └── favicon.ico
│
└── src/                   # Source code (see structure below)
    ├── main.tsx
    ├── App.tsx
    └── ...
```

**Documentation Rules:**
- ✅ **ALL documentation files** (except README.md) go in `docs/` folder
- ✅ Create `docs/` folder at project root
- ✅ Use Markdown (.md) for all documentation
- ❌ **NEVER** place documentation inside `src/` directory

---

### 📦 Package Management & Versions

**CRITICAL - Version Requirements:**

❌ **ABSOLUTELY FORBIDDEN:**
- Using deprecated npm packages
- Using packages with known security vulnerabilities
- Using outdated major versions when stable newer versions exist
- Using packages marked as "unmaintained" or "archived"

✅ **REQUIRED:**
- Always use **latest stable versions** (as of January 2025)
- Check package.json for deprecated dependencies
- Verify all packages are actively maintained
- Use exact versions or caret (^) for dependencies
- Run `pnpm outdated` to check for updates

**Before installing any package:**
```bash
# Check if package is deprecated
pnpm info <package-name>

# Check for security vulnerabilities
pnpm audit

# Check for outdated packages
pnpm outdated
```

---

### Architecture Patterns
This project **MUST** follow these architectural principles:

1. **Vertical Slicing**: Organize code by feature/domain, NOT by technical layer
2. **Screaming Architecture**: Directory structure should scream what the application does, not what frameworks it uses
3. **Clean Architecture**: Separate concerns into distinct layers (Domain → Repository → Use Cases → Hooks → UI)
4. **Functional Programming Only**: ❌ No classes, constructors, `this`, or inheritance. Use functions, hooks, and composable patterns

### Service Layer Architecture
**MANDATORY** pattern for all API integrations:

```
UI Component <-> Custom Hook (React Query + Mapper/Adapter) <-> Service Layer
```

- **UI Components**: Pure presentation, consume hooks, NO business logic
- **Custom Hooks**: Use `useQuery`/`useMutation` from TanStack Query, handle loading/error states
- **Mapper/Adapter**: Transform API responses to prevent UI breaks when backend changes
- **Service Layer**: HTTP calls, business logic, error handling

### !CRITICAL IMPORTANT! Project Structure (Vertical Slicing Example)

```
src/
├── main.tsx                    # Application entry point
├── App.tsx                     # Root application component
├── routes.tsx                  # Application routes definition
├── vite-env.d.ts              # Vite environment types
│
├── assets/                     # Static assets (images, icons, fonts)
│   ├── icons/
│   ├── images/
│   └── fonts/
│
├── components/                 # Reusable UI components (BaseButton, BaseInput, etc.)
│   ├── BaseButton/
│   │   ├── BaseButton.tsx
│   │   ├── BaseButton.module.css    # Only if using CSS Modules (not Tailwind)
│   │   └── index.ts
│   ├── BaseInput/
│   └── index.ts               # Barrel export for all components
│
├── styles/                     # Global styles and CSS utilities
│   ├── index.css              # Main CSS entry (imports all partials)
│
├── hooks/                      # Reusable custom hooks
│   ├── useAuth.ts
│   ├── useDebounce.ts
│   └── useLocalStorage.ts
│
├── utils/                      # Pure utility functions
│   ├── envs.ts                # Environment variables helper
│   ├── token.ts               # Token validation utilities
│   ├── dateUtils.ts           # Date manipulation
│   └── validationUtils.ts     # Validation helpers
│
├── store/                      # Zustand global stores
│   ├── authStore.ts
│   ├── errorStore.ts
│   └── storage.ts             # CookieStore/sessionStorage helpers
│
├── constants/                  # Application constants
│   ├── routes.ts              # Route paths
│   ├── apiEndpoints.ts        # API endpoint URLs
│   └── config.ts              # App configuration
│
├── enums/                      # TypeScript enums (use sparingly, prefer unions)
│
├── types/                      # Shared TypeScript types/interfaces
│   ├── api.types.ts
│   ├── user.types.ts
│   └── common.types.ts
│
├── interfaces/                 # TypeScript interfaces
│   ├── components.interface.ts
│   └── services.interface.ts
│
├── validators/                 # Zod schemas and validators
│   ├── authSchema.ts
│   └── userSchema.ts
│
├── providers/                  # React context providers
│   ├── QueryProvider.tsx      # TanStack Query provider
│   ├── I18nProvider.tsx       # i18next providerhttp/
│   ├── http.ts                # HTTP client wrapper (fetch)
│       └── httpClient.ts      # HTTP configuration
│   └── ThemeProvider.tsx      # Theme context provider
│
├── navigation/                 # Navigation components and guards
│   ├── PrivateRoute.tsx
│   └── PublicRoute.tsx
│
├── adapters/                   # Data adapters/mappers (API response transformers)
│   ├── userAdapter.ts
│   └── authAdapter.ts
│
├── services/                   # API service functions
│   ├── authService.ts
│   └── userService.ts
│
├── pages/                      # FEATURE-BASED PAGE ORGANIZATION (Vertical Slicing)
│   ├── Auth/                  # Authentication domain
│   │   ├── Login/
│   │   │   ├── Login.tsx
│   │   │   ├── Login.module.css     # Only if using CSS Modules (not Tailwind)
│   │   │   ├── useLogin.ts          # Custom hook for login logic
│   │   │   └── index.ts
│   │   ├── Register/
│   │   └── ForgotPassword/
│   │
│   ├── Dashboard/             # Dashboard domain
│   │   ├── Dashboard.tsx
│   │   ├── Dashboard.module.css     # Only if using CSS Modules (not Tailwind)
│   │   ├── components/              # Dashboard-specific components
│   │   │   ├── StatsCard/
│   │   │   └── ActivityFeed/
│   │   ├── hooks/                   # Dashboard-specific hooks
│   │   │   └── useDashboardData.ts
│   │   └── index.ts
│   │
│   └── Profile/               # User profile domain
│       ├── Profile.tsx
│       ├── Profile.module.css       # Only if using CSS Modules (not Tailwind)
│       ├── components/
│       │   ├── ProfileForm/
│       │   └── AvatarUpload/
│       ├── hooks/
│       │   └── useProfileUpdate.ts
│       ├── services/
│       │   └── profileService.ts
│       └── index.ts
│
└── test/                       # Test utilities and mocks
    ├── mocks/
    ├── utils/
    └── setup.ts
```

**Key Principles:**
- Each **feature/domain** has its own directory under `pages/`
- Domain directories contain: components, hooks, services, types specific to that feature
- **Shared** components go in `/components/`
- **Shared** hooks go in `/hooks/`
- **Shared** utilities go in `/utils/`
- **Respect domain boundaries** - don't mix concerns between features

---

### Security
- **Sanitize User Inputs**: Prevent XSS attacks by validating and escaping all user inputs.
- **Validate Data**: Escape data before rendering.
- **Use HTTPS**: For all external API calls.
- **Proper Authentication**: Implement proper authentication and authorization patterns.
- **Content Security Policy**: Use CSP headers.
- **Never Expose Sensitive Data**: Never log tokens, user IDs, or metadata in console.
- **Remove Debug Logging**: Remove all `console.log`, `console.error`, `console.warn` before production deployment.

### Accessibility
- **Semantic HTML**: Use appropriate HTML elements (header, nav, main, article, section, etc.).
- **ARIA Attributes**: Implement proper ARIA attributes and roles.
- **Keyboard Navigation**: Ensure all interactive elements work with keyboard.
- **Alt Text**: Provide alt text for images and descriptive text for icons.
- **Color Contrast**: Implement proper color contrast ratios (WCAG AA/AAA).
- **Screen Reader Testing**: Test with screen readers and accessibility tools.

## UI/UX Design Principles

**CRITICAL**: When designing or implementing ANY new UI feature, component, or layout, these principles are MANDATORY and NON-NEGOTIABLE.

### Mobile-First Approach
- **Design for Mobile FIRST**: Always start with mobile (320px-375px) and progressively enhance for larger screens.
- **Touch-Friendly Targets**: Minimum 44x44px touch targets for all interactive elements (buttons, links, inputs).
- **Responsive by Default**: EVERY component must work seamlessly across all screen sizes (mobile, tablet, desktop).
- **Test on Real Devices**: Verify behavior on actual mobile devices, not just browser DevTools.
- **Mobile Performance**: Optimize for mobile networks - lazy load images, minimize bundle size, reduce re-renders.

### Responsive Design Standards
- **Breakpoints**: Use Tailwind's standard breakpoints:
  - `sm: 640px` (small tablets)
  - `md: 768px` (tablets)
  - `lg: 1024px` (small desktops)
  - `xl: 1280px` (large desktops)
  - `2xl: 1536px` (extra large screens)
- **Fluid Typography**: Use responsive font sizes that scale with viewport.
- **Flexible Layouts**: Use flexbox and grid for fluid, adaptive layouts.
- **Images**: Always responsive with `max-w-full h-auto`, proper `srcset` for different resolutions.
- **Spacing**: Use responsive spacing utilities (e.g., `px-4 md:px-6 lg:px-8`).

### UX Best Practices
- **User Feedback**: Provide immediate visual feedback for all user actions (loading states, success/error messages, hover states).
- **Loading States**: Show skeleton loaders, spinners, or progress indicators for async operations.
- **Error Handling**: Display clear, actionable error messages (what went wrong + how to fix it).
- **Empty States**: Design meaningful empty states with clear call-to-action.
- **Confirmation Dialogs**: Require confirmation for destructive actions (delete, cancel, discard).
- **Progressive Disclosure**: Show only essential information first, reveal details on demand.
- **Consistent Patterns**: Reuse UI patterns across the app (same modals, buttons, forms style).
- **Microinteractions**: Add subtle animations for state changes (button press, menu open, etc.).

### UI Design Standards
- **Visual Hierarchy**: Use size, weight, color, and spacing to establish clear hierarchy.
- **Consistent Spacing**: Follow 4px/8px grid system (spacing-1 = 4px, spacing-2 = 8px, etc.).
- **Color System**: Use design tokens from theme (primary, secondary, accent, text, background).
- **Typography Scale**: Use predefined text sizes from design system (text-sm, text-base, text-lg, etc.).
- **Icon Consistency**: Use same icon library (lucide-react) with consistent sizing (w-4, w-5, w-6).
- **Shadows & Elevation**: Use design system shadows to create depth and hierarchy.
- **Border Radius**: Consistent corner radius across similar components (buttons, cards, inputs).
- **Whitespace**: Generous padding and margins for breathing room, especially on mobile.

### Accessibility Requirements (MANDATORY)
- **Keyboard Navigation**: ALL interactive elements must be keyboard accessible (Tab, Enter, Esc, Arrow keys).
- **Focus Management**: Visible focus indicators on all interactive elements.
- **Screen Readers**: Proper ARIA labels, roles, and live regions for dynamic content.
- **Color Independence**: Don't rely solely on color to convey information (use icons, text, patterns).
- **Contrast Ratios**: Minimum 4.5:1 for normal text, 3:1 for large text (WCAG AA).
- **Form Accessibility**: Labels for all inputs, error messages associated with fields.
- **Skip Links**: Provide skip navigation links for long pages.
- **Alt Text**: Descriptive alt text for all meaningful images (empty alt="" for decorative).

### Performance Considerations
- **Lazy Load**: Code-split routes and lazy load heavy components.
- **Image Optimization**: Use next-gen formats (WebP, AVIF), proper compression.
- **Debounce/Throttle**: Optimize expensive operations (search, scroll, resize handlers).
- **Memoization**: Use `useMemo` and `useCallback` for expensive calculations and callbacks.
- **Virtual Scrolling**: For long lists (>100 items), implement virtualization.
- **Bundle Size**: Monitor and minimize JavaScript bundle size.

### Design Verification Checklist
Before completing ANY UI task, verify:
- [ ] Works perfectly on mobile (320px-375px width)
- [ ] Scales properly to tablet (768px-1024px)
- [ ] Looks great on desktop (1280px+)
- [ ] Touch targets are minimum 44x44px
- [ ] All interactive elements have hover/active/focus states
- [ ] Loading states implemented for async operations
- [ ] Error states designed and implemented
- [ ] Empty states designed with clear CTA
- [ ] Keyboard navigation works flawlessly
- [ ] Screen reader accessible (test with VoiceOver/NVDA)
- [ ] Color contrast meets WCAG AA standards
- [ ] Animations respect `prefers-reduced-motion`
- [ ] Works in both light and dark themes
- [ ] No layout shift (CLS) during loading
- [ ] Fast interaction (< 100ms response time)

## 🎨 CSS & Styling Standards

### ⚠️ CRITICAL STYLING RULES

**IMPORTANT:** You must choose ONE styling approach for the project. **DO NOT MIX** CSS Modules with Tailwind.

#### Decision Tree:

**IF user explicitly requests Tailwind:**
✅ Use **Tailwind CSS ONLY**
✅ Use global CSS for design tokens (variables.css)
❌ **DO NOT** create `.module.css` files
❌ **DO NOT** use BEM methodology
✅ Apply Tailwind utility classes directly in components

**IF user does NOT request Tailwind (DEFAULT):**
✅ Use **CSS Modules** (`.module.css`) with **BEM methodology**
✅ Use global CSS for design tokens (variables.css)
❌ **DO NOT** install or use Tailwind CSS
❌ **DO NOT** use utility classes in HTML

**ALWAYS FORBIDDEN (regardless of approach):**
❌ **DO NOT** use CSS-in-JS libraries (styled-components, emotion, etc.)
❌ **DO NOT** use inline styles
❌ **DO NOT** mix CSS Modules with Tailwind in the same project

---

## 🎨 CSS Variable System - DESIGN TOKENS

### 📌 Core Philosophy

The CSS variable system is designed to be **brand-agnostic** and **reusable**. All color variables must use **generic, semantic names** that allow complete rebranding by simply changing hexadecimal values.

**🚨 CRITICAL PRINCIPLES:**

1. **Generic Variable Names Only** - Variables describe what they are (colors, spacing), NOT where they're used
2. **Component Independence** - Components reference generic variables, never component-specific ones
3. **Brand Portability** - Changing brand colors = changing hex values only, NOT variable names
4. **Light & Dark Support** - All color schemes must support both themes

---

### ❌ ABSOLUTELY FORBIDDEN Variable Names

**NEVER create variables with these patterns:**

```css
/* ❌ FORBIDDEN - Component-specific variables */
--bg-background
--bg-card
--bg-popover
--bg-modal
--bg-sidebar

--text-title
--text-body
--text-caption
--text-muted

--border-input
--border-card
--border-divider

--button-primary-bg
--button-secondary-bg
--card-foreground
--popover-foreground
--muted-foreground
--accent-foreground
--destructive-foreground

/* ❌ FORBIDDEN - Generic semantic names without color prefix */
--background
--foreground
--muted
--accent
--destructive
--border
--input
--ring
```

**Why forbidden?** These tie variables to specific UI components, making them impossible to reuse across different branding without renaming everything.

---

### ✅ REQUIRED Variable Naming Convention

**ONLY use these variable name patterns:**

```css
/* Color Variables */
--color-primary           /* Main brand color */
--color-primary-light     /* Lighter variant */
--color-primary-dark      /* Darker variant */

--color-secondary         /* Secondary brand color */
--color-secondary-light
--color-secondary-dark

--color-tertiary          /* Accent/tertiary color */
--color-tertiary-light
--color-tertiary-dark

/* Gray Scale (1 = lightest/white, 6 = darkest/black) */
--color-gray-1            /* Lightest gray / white */
--color-gray-2            /* Very light gray */
--color-gray-3            /* Light gray */
--color-gray-4            /* Medium gray */
--color-gray-5            /* Dark gray */
--color-gray-6            /* Darkest gray / black */

/* Base Colors */
--color-white             /* Pure white */
--color-black             /* Pure black */

/* Status/Feedback Colors */
--color-success           /* Success state */
--color-success-light
--color-success-dark

--color-error             /* Error state */
--color-error-light
--color-error-dark

--color-warning           /* Warning state */
--color-warning-light
--color-warning-dark

--color-info              /* Info state */
--color-info-light
--color-info-dark

--color-disabled          /* Disabled state */

```

---

### 📋 How to Define Colors Based on User Brand

**When the user provides a brand color or design direction:**

1. **Ask for brand colors** if not provided:
   - Primary color (main brand color)
   - Secondary color (optional)
   - Tertiary/accent color (optional)

2. **Generate color variants** automatically:
   - Light variant: 20-30% lighter than base
   - Dark variant: 20-30% darker than base

3. **Create gray scale** based on the overall aesthetic:
   - For light themes: gray-1 = #ffffff, gray-6 = #1a1a1a
   - For dark themes: Invert the scale

4. **Keep status colors consistent** unless specified:
   - Success: Green spectrum (#10b981, #059669, etc.)
   - Error: Red spectrum (#ef4444, #dc2626, etc.)
   - Warning: Yellow/Orange spectrum (#f59e0b, #d97706, etc.)
   - Info: Blue spectrum (#3b82f6, #2563eb, etc.)

---

### � Typography - Google Fonts (Poppins)

**Default Font Family: Poppins**

**REQUIRED:** Use **Poppins** from Google Fonts as the primary font family for all projects (unless user specifies otherwise).

#### Installation

**Method 1: HTML Link (Recommended)**

Add to `index.html` in the `<head>` section:

```html
<!-- Google Fonts - Poppins (All weights) -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
```

**Method 2: CSS Import**

Add to `src/styles/index.css`:

```css
/* Google Fonts - Poppins */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap');
```

#### Font Weights Included:

- **300** - Light
- **400** - Regular (Normal)
- **500** - Medium
- **600** - Semi-Bold
- **700** - Bold
- **800** - Extra-Bold
- **900** - Black

#### CSS Configuration

Add to `src/styles/reset.css` or `src/styles/index.css`:

```css
body {
  font-family: 'Poppins', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-weight: 400; /* Regular */
  line-height: 1.5;
}

/* Font weight utilities (optional) */
.font-light { font-weight: 300; }
.font-regular { font-weight: 400; }
.font-medium { font-weight: 500; }
.font-semibold { font-weight: 600; }
.font-bold { font-weight: 700; }
.font-extrabold { font-weight: 800; }
.font-black { font-weight: 900; }
```

#### Usage Examples:

```css
/* Headings */
h1, h2, h3 {
  font-family: 'Poppins', sans-serif;
  font-weight: 600; /* Semi-Bold */
}

/* Body text */
body, p {
  font-family: 'Poppins', sans-serif;
  font-weight: 400; /* Regular */
}

/* Buttons */
button {
  font-family: 'Poppins', sans-serif;
  font-weight: 500; /* Medium */
}

/* Labels */
label {
  font-family: 'Poppins', sans-serif;
  font-weight: 500; /* Medium */
}
```

**Why Poppins?**
- ✅ Modern, clean, and professional
- ✅ Excellent readability on screens
- ✅ Free and open source (SIL Open Font License)
- ✅ Wide range of weights for design flexibility
- ✅ Great for both headings and body text
- ✅ Works well in mobile and desktop interfaces

---

### �📄 CSS Variables Template (src/styles/variables.css)

**This file is MANDATORY - create it in `src/styles/variables.css`**

```css
/**
 * Design Tokens - Color Variables
 * 
 * RULES:
 * 1. All variables use generic, semantic names (--color-primary, --color-gray-3)
 * 2. NEVER create component-specific variables (--bg-card, --text-title)
 * 3. Components reference these generic variables directly
 * 4. To rebrand: change hex values only, NOT variable names
 * 
 * Gray Scale Convention:
 * - gray-1 = lightest (white or near-white)
 * - gray-6 = darkest (black or near-black)
 */

:root {
  /* ========================================
     PRIMARY BRAND COLORS
     Replace these hex values with your brand colors
     ======================================== */
  --color-primary: #0066cc;
  --color-primary-light: #3385d6;
  --color-primary-dark: #0052a3;

  /* ========================================
     SECONDARY BRAND COLORS
     ======================================== */
  --color-secondary: #6366f1;
  --color-secondary-light: #818cf8;
  --color-secondary-dark: #4f46e5;

  /* ========================================
     TERTIARY/ACCENT COLORS
     ======================================== */
  --color-tertiary: #ec4899;
  --color-tertiary-light: #f472b6;
  --color-tertiary-dark: #db2777;

  /* ========================================
     GRAY SCALE (Light Theme)
     1 = lightest/white, 6 = darkest/black
     ======================================== */
  --color-gray-1: #ffffff;
  --color-gray-2: #f5f5f5;
  --color-gray-3: #e0e0e0;
  --color-gray-4: #9e9e9e;
  --color-gray-5: #424242;
  --color-gray-6: #1a1a1a;

  /* ========================================
     BASE COLORS
     ======================================== */
  --color-white: #ffffff;
  --color-black: #000000;

  /* ========================================
     STATUS/FEEDBACK COLORS
     ======================================== */
  --color-success: #10b981;
  --color-success-light: #34d399;
  --color-success-dark: #059669;

  --color-error: #ef4444;
  --color-error-light: #f87171;
  --color-error-dark: #dc2626;

  --color-warning: #f59e0b;
  --color-warning-light: #fbbf24;
  --color-warning-dark: #d97706;

  --color-info: #3b82f6;
  --color-info-light: #60a5fa;
  --color-info-dark: #2563eb;

  --color-disabled: #9e9e9e;
}

/* ========================================
   DARK THEME
   Apply with: <html data-theme="dark">
   ======================================== */
[data-theme="dark"] {
  /* Primary colors (may stay same or adjust for dark mode) */
  --color-primary: #3385d6;
  --color-primary-light: #5ca3e0;
  --color-primary-dark: #0066cc;

  --color-secondary: #818cf8;
  --color-secondary-light: #a5b4fc;
  --color-secondary-dark: #6366f1;

  --color-tertiary: #f472b6;
  --color-tertiary-light: #f9a8d4;
  --color-tertiary-dark: #ec4899;

  /* Gray Scale INVERTED for dark theme */
  /* 1 = lightest (now dark backgrounds), 6 = darkest (now light text) */
  --color-gray-1: #1a1a1a;
  --color-gray-2: #2d2d2d;
  --color-gray-3: #424242;
  --color-gray-4: #9e9e9e;
  --color-gray-5: #e0e0e0;
  --color-gray-6: #ffffff;

  /* Base colors */
  --color-white: #ffffff;
  --color-black: #000000;

  /* Status colors (may adjust for better contrast in dark mode) */
  --color-success: #34d399;
  --color-success-light: #6ee7b7;
  --color-success-dark: #10b981;

  --color-error: #f87171;
  --color-error-light: #fca5a5;
  --color-error-dark: #ef4444;

  --color-warning: #fbbf24;
  --color-warning-light: #fcd34d;
  --color-warning-dark: #f59e0b;

  --color-info: #60a5fa;
  --color-info-light: #93c5fd;
  --color-info-dark: #3b82f6;

  --color-disabled: #6b7280;
}

```

---

### ✅ CORRECT Component Usage Examples

**Components MUST reference generic variables, never component-specific ones:**

```css
/* ✅ CORRECT - Button Component */
.button {
  background-color: var(--color-primary);
  color: var(--color-white);
  border: 1px solid var(--color-primary-dark);
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: 0.375rem;
}

.button--secondary {
  background-color: var(--color-gray-2);
  color: var(--color-gray-6);
  border-color: var(--color-gray-3);
}

.button--danger {
  background-color: var(--color-error);
  color: var(--color-white);
  border-color: var(--color-error-dark);
}

.button:disabled {
  background-color: var(--color-disabled);
  color: var(--color-gray-3);
  cursor: not-allowed;
}

/* ✅ CORRECT - Card Component */
.card {
  background-color: var(--color-white);
  border: 1px solid var(--color-gray-3);
  border-radius: 0.5rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

[data-theme="dark"] .card {
  background-color: var(--color-gray-2);
  border-color: var(--color-gray-3);
}

.card__title {
  color: var(--color-gray-6);
  font-size: 1.25rem;
  font-weight: 600;
}

.card__body {
  color: var(--color-gray-5);
}

/* ✅ CORRECT - Form Inputs */
.input {
  background-color: var(--color-white);
  border: 1px solid var(--color-gray-3);
  color: var(--color-gray-6);
  padding: var(--spacing-sm) var(--spacing-md);
}

.input:focus {
  border-color: var(--color-primary);
  outline: 2px solid var(--color-primary-light);
}

.input--error {
  border-color: var(--color-error);
}

.input__error-message {
  color: var(--color-error);
  font-size: 0.875rem;
}

/* ✅ CORRECT - Typography */
.heading-1 {
  color: var(--color-gray-6);
  font-size: 2.5rem;
}

.text-body {
  color: var(--color-gray-5);
  font-size: 1rem;
}

.text-muted {
  color: var(--color-gray-4);
}

/* ✅ CORRECT - Alerts/Notifications */
.alert {
  padding: var(--spacing-md);
  border-radius: 0.375rem;
  border: 1px solid transparent;
}

.alert--success {
  background-color: var(--color-success-light);
  border-color: var(--color-success);
  color: var(--color-success-dark);
}

.alert--error {
  background-color: var(--color-error-light);
  border-color: var(--color-error);
  color: var(--color-error-dark);
}

.alert--warning {
  background-color: var(--color-warning-light);
  border-color: var(--color-warning);
  color: var(--color-warning-dark);
}
```

---

### ❌ WRONG Component Usage Examples

```css
/* ❌ WRONG - Creating component-specific variables */
:root {
  --bg-button-primary: #0066cc;    /* FORBIDDEN */
  --bg-card: #ffffff;              /* FORBIDDEN */
  --text-heading: #1a1a1a;         /* FORBIDDEN */
  --border-input: #e0e0e0;         /* FORBIDDEN */
}

.button {
  background-color: var(--bg-button-primary);  /* FORBIDDEN */
}

.card {
  background-color: var(--bg-card);            /* FORBIDDEN */
}

.heading {
  color: var(--text-heading);                  /* FORBIDDEN */
}
```

**Why wrong?** These variables are tied to specific components, making rebranding require renaming variables across the codebase
---

### 🎨 Styling Methodology

**🚨 CRITICAL DECISION:** Choose ONE approach for the entire project:

#### Option 1: CSS Modules + BEM (DEFAULT - If user does NOT request Tailwind)

**When to use:**
- User does NOT mention Tailwind
- Project is a new greenfield project
- User wants full control over CSS

**What to do:**
- Create `.module.css` files for components
- Use BEM naming convention
- Reference CSS variables: `var(--color-primary)`
- NO utility classes in HTML

#### Option 2: Tailwind CSS (ONLY if user explicitly requests it)

**When to use:**
- User explicitly says "use Tailwind", "with Tailwind", or "Tailwind CSS"
- User shows Tailwind config in existing project
- User asks to keep existing Tailwind setup

**What to do:**
- Install and configure Tailwind
- Use utility classes in components
- Map Tailwind to CSS variables in config
- NO `.module.css` files
- NO BEM methodology

**🚫 NEVER MIX BOTH APPROACHES**

---

### 📘 Option 1: CSS Modules + BEM Methodology

**Use this approach when the user does NOT request Tailwind (DEFAULT).**

#### BEM Naming Convention

**BEM = Block__Element--Modifier**

```
.block              /* Component/block */
.block__element     /* Child element of block */
.block--modifier    /* Variation of block */
.block__element--modifier  /* Variation of element */
```

#### CSS Modules + BEM Examples

```css
/* Button.module.css - ✅ CORRECT */

/* Block (component root) */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-sm) var(--spacing-md);
  background-color: var(--color-primary);
  color: var(--color-white);
  border: 1px solid var(--color-primary-dark);
  border-radius: 0.375rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.button:hover {
  background-color: var(--color-primary-dark);
}

/* Modifiers (variations of button) */
.button--secondary {
  background-color: var(--color-gray-2);
  color: var(--color-gray-6);
  border-color: var(--color-gray-3);
}

.button--danger {
  background-color: var(--color-error);
  border-color: var(--color-error-dark);
}

.button--outline {
  background-color: transparent;
  color: var(--color-primary);
  border-color: var(--color-primary);
}

.button--large {
  padding: var(--spacing-md) var(--spacing-lg);
  font-size: 1.125rem;
}

.button--small {
  padding: var(--spacing-xs) var(--spacing-sm);
  font-size: 0.875rem;
}

.button--disabled {
  background-color: var(--color-disabled);
  color: var(--color-gray-3);
  border-color: var(--color-disabled);
  cursor: not-allowed;
  opacity: 0.6;
}

/* Elements (children of button) */
.button__icon {
  margin-right: var(--spacing-xs);
  display: inline-flex;
}

.button__text {
  display: inline-block;
}

.button__loader {
  margin-left: var(--spacing-xs);
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
```

```tsx
// Button.tsx - Using CSS Modules with BEM
import styles from './Button.module.css';
import type React from 'react';

interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'danger' | 'outline';
  size?: 'small' | 'medium' | 'large';
  disabled?: boolean;
  loading?: boolean;
  icon?: React.ReactNode;
  children: React.ReactNode;
  onClick?: () => void;
}

export const Button = ({
  variant = 'primary',
  size = 'medium',
  disabled = false,
  loading = false,
  icon,
  children,
  onClick,
}: ButtonProps) => {
  const buttonClasses = [
    styles.button,
    variant !== 'primary' && styles[`button--${variant}`],
    size !== 'medium' && styles[`button--${size}`],
    disabled && styles['button--disabled'],
  ]
    .filter(Boolean)
    .join(' ');

  return (
    <button
      type="button"
      className={buttonClasses}
      onClick={onClick}
      disabled={disabled || loading}
    >
      {icon && <span className={styles.button__icon}>{icon}</span>}
      <span className={styles.button__text}>{children}</span>
      {loading && <span className={styles.button__loader}>⏳</span>}
    </button>
  );
};
```

#### More BEM Examples

```css
/* Card.module.css */
.card {
  background-color: var(--color-white);
  border: 1px solid var(--color-gray-3);
  border-radius: 0.5rem;
  padding: var(--spacing-lg);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.card--bordered {
  border: 2px solid var(--color-primary);
}

.card--elevated {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: var(--spacing-md);
  padding-bottom: var(--spacing-sm);
  border-bottom: 1px solid var(--color-gray-3);
}

.card__title {
  color: var(--color-gray-6);
  font-size: 1.25rem;
  font-weight: 600;
}

.card__subtitle {
  color: var(--color-gray-4);
  font-size: 0.875rem;
}

.card__body {
  color: var(--color-gray-5);
  line-height: 1.6;
}

.card__footer {
  margin-top: var(--spacing-md);
  padding-top: var(--spacing-md);
  border-top: 1px solid var(--color-gray-3);
  display: flex;
  justify-content: flex-end;
  gap: var(--spacing-sm);
}

/* Dark theme support */
[data-theme="dark"] .card {
  background-color: var(--color-gray-2);
  border-color: var(--color-gray-3);
}
```

```css
/* Input.module.css */
.input-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.input-group__label {
  color: var(--color-gray-6);
  font-size: 0.875rem;
  font-weight: 500;
}

.input-group__label--required::after {
  content: '*';
  color: var(--color-error);
  margin-left: 0.25rem;
}

.input {
  padding: var(--spacing-sm) var(--spacing-md);
  background-color: var(--color-white);
  border: 1px solid var(--color-gray-3);
  border-radius: 0.375rem;
  color: var(--color-gray-6);
  font-size: 1rem;
  transition: border-color 0.2s ease;
}

.input:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px var(--color-primary-light);
}

.input--error {
  border-color: var(--color-error);
}

.input--error:focus {
  box-shadow: 0 0 0 3px var(--color-error-light);
}

.input--disabled {
  background-color: var(--color-gray-2);
  color: var(--color-disabled);
  cursor: not-allowed;
}

.input-group__helper-text {
  color: var(--color-gray-4);
  font-size: 0.75rem;
}

.input-group__error-message {
  color: var(--color-error);
  font-size: 0.75rem;
  display: flex;
  align-items: center;
  gap: 0.25rem;
}
```

---

### 🌍 2. Global CSS (SECONDARY - Design Tokens Only)

**Global styles should ONLY contain:**
- CSS variable definitions
- CSS resets/normalization
- Global utility classes (use sparingly)

**Structure:**

```
src/styles/
├── index.css           # Main entry point (imports all partials)
├── variables.css       # Color and spacing variables
├── reset.css           # CSS reset/normalization
└── utilities.css       # Optional utility classes (minimal)
```

#### src/styles/index.css

```css
/**
 * Global Styles Entry Point
 * Import order matters!
 */

/* 1. Variables (must come first) */
@import './variables.css';

/* 2. Reset/Normalization */
@import './reset.css';

/* 3. Optional utilities (use sparingly) */
@import './utilities.css';
```

#### src/styles/reset.css

```css
/**
 * CSS Reset
 */

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: 'Poppins', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-weight: 400;
  line-height: 1.5;
  background-color: var(--color-gray-1);
  color: var(--color-gray-6);
}

img,
picture,
video,
canvas,
svg {
  display: block;
  max-width: 100%;
}

input,
button,
textarea,
select {
  font: inherit;
}

button {
  background: none;
  border: none;
  cursor: pointer;
}

a {
  color: inherit;
  text-decoration: none;
}
```

#### src/styles/utilities.css (Optional - use sparingly)

```css
/**
 * Utility Classes
 * Use ONLY when CSS Modules would be overkill
 */

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

.container {
  width: 100%;
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
}

.text-center {
  text-align: center;
}
```

---

### ⚡ Option 2: Tailwind CSS Configuration (ALTERNATIVE APPROACH)

**🚨 CRITICAL DECISION POINT:**

**IF user explicitly requests Tailwind:**
- ✅ Install and configure Tailwind
- ✅ Use ONLY Tailwind utility classes for styling
- ✅ Map Tailwind colors to our generic CSS variables
- ❌ **DO NOT** create `.module.css` files for components
- ❌ **DO NOT** use BEM methodology
- ❌ **DO NOT** mix with CSS Modules

**IF user does NOT mention Tailwind:**
- ❌ **DO NOT** install Tailwind
- ✅ Use CSS Modules + BEM instead (Option 1)

**Configuration (ONLY if user requests Tailwind):**

#### Installation

```bash
npm install tailwindcss @tailwindcss/vite
```
### CONFIGURE VITE PLUGIN TAILWIND
Add the @tailwindcss/vite plugin to your Vite configuration.

```bash
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({
  plugins: [
    tailwindcss(),
  ],
})
```

#### Import Tailwind in src/styles/index.css

**CRITICAL:** This is your SINGLE source of truth for design tokens. Configure it properly:

```css
/* src/styles/index.css - REQUIRED STRUCTURE */

/* 1. Import Tailwind CSS */
@import "tailwindcss";

/* 2. Configure @theme to map YOUR variables to Tailwind */
@theme {
  /* Primary Colors */
  --color-primary: var(--color-primary);
  --color-primary-light: var(--color-primary-light);
  --color-primary-dark: var(--color-primary-dark);
  
  /* Secondary Colors */
  --color-secondary: var(--color-secondary);
  --color-secondary-light: var(--color-secondary-light);
  --color-secondary-dark: var(--color-secondary-dark);
  
  /* Tertiary Colors */
  --color-tertiary: var(--color-tertiary);
  --color-tertiary-light: var(--color-tertiary-light);
  --color-tertiary-dark: var(--color-tertiary-dark);
  
  /* Gray Scale */
  --color-gray-1: var(--color-gray-1);
  --color-gray-2: var(--color-gray-2);
  --color-gray-3: var(--color-gray-3);
  --color-gray-4: var(--color-gray-4);
  --color-gray-5: var(--color-gray-5);
  --color-gray-6: var(--color-gray-6);
  
  /* Base Colors */
  --color-white: var(--color-white);
  --color-black: var(--color-black);
  
  /* Status Colors */
  --color-success: var(--color-success);
  --color-success-light: var(--color-success-light);
  --color-success-dark: var(--color-success-dark);
  
  --color-error: var(--color-error);
  --color-error-light: var(--color-error-light);
  --color-error-dark: var(--color-error-dark);
  
  --color-warning: var(--color-warning);
  --color-warning-light: var(--color-warning-light);
  --color-warning-dark: var(--color-warning-dark);
  
  --color-info: var(--color-info);
  --color-info-light: var(--color-info-light);
  --color-info-dark: var(--color-info-dark);
  
  --color-disabled: var(--color-disabled);
}

/* 3. Import your variables definitions */
@import './variables.css';

/* 4. Import reset and utilities (optional) */
@import './reset.css';
```

**What this enables:**
- ✅ Use Tailwind classes: `bg-primary`, `text-gray-6`, `border-error`
- ✅ Opacity modifiers: `bg-primary/90`, `text-gray-6/80`
- ✅ Hover/Focus states: `hover:bg-primary-dark`, `focus:border-primary`
- ✅ Single source of truth: Change colors in `variables.css` only

**❌ NEVER create:**
```css
/* ❌ FORBIDDEN - theme.css */
@theme {
  --color-primary: oklch(0.5 0.2 200);  /* FORBIDDEN */
  --bg-card: #ffffff;                   /* FORBIDDEN */
}
```

#### ✅ CORRECT Tailwind Usage

```tsx
// ✅ CORRECT - Using our mapped variables
<button className="bg-primary text-white hover:bg-primary-dark px-md py-sm rounded">
  Click me
</button>

<div className="bg-gray-1 text-gray-6 p-lg">
  <h1 className="text-gray-6 text-2xl font-bold">Title</h1>
  <p className="text-gray-5">Content</p>
</div>

<div className="border border-gray-3 rounded-lg">
  <span className="text-error">Error message</span>
  <span className="text-success">Success message</span>
</div>
```

#### ❌ FORBIDDEN Tailwind Usage

```tsx
// ❌ WRONG - Never use Tailwind's default colors
<button className="bg-blue-500 text-white">  {/* FORBIDDEN */}
  Click me
</button>

<div className="bg-slate-100 text-gray-900">  {/* FORBIDDEN */}
  Content
</div>

<span className="text-red-600">Error</span>  {/* FORBIDDEN - use text-error */}
```

**Why forbidden?** Using Tailwind's default palette (`blue-500`, `slate-100`, `red-600`) bypasses our variable system and makes rebranding impossible.

---

### � TAILWIND PROJECT BEST PRACTICES (MANDATORY)

**🚨 CRITICAL RULES when using Tailwind CSS:**

#### 1. ✅ Component Reusability Priority

**ALWAYS use existing components instead of raw HTML:**

```tsx
// ❌ WRONG - Using raw HTML elements
<button className="bg-primary text-white px-4 py-2 rounded">
  Click me
</button>

<input 
  type="text" 
  className="border border-gray-3 px-3 py-2 rounded"
  placeholder="Enter text"
/>

// ✅ CORRECT - Using our components from src/components/ui
import { Button } from '@/components/ui/Button';
import { Input } from '@/components/ui/Input';

<Button variant="primary">Click me</Button>
<Input placeholder="Enter text" />
```

**Rule:** If a component exists in `src/components/ui/`, **ALWAYS** import and use it. Only create new components when necessary.

#### 2. ❌ NO theme.css Files

**FORBIDDEN - Never create these:**

```css
/* ❌ FORBIDDEN - theme.css with oklch colors */
@theme {
  --color-primary: oklch(0.5 0.2 200);
  --color-secondary: oklch(0.6 0.15 250);
}

/* ❌ FORBIDDEN - Any theme.css file */
```

**✅ CORRECT - Use ONLY index.css for design tokens:**

```css
/* src/styles/index.css - Your single source of truth */
@import "tailwindcss";
@import './variables.css';

@theme {
  /* Map Tailwind to YOUR variables only */
  --color-primary: var(--color-primary);
  --color-secondary: var(--color-secondary);
  /* etc... */
}
```

#### 3. ✅ Use ONLY Your CSS Variables

**FORBIDDEN - Do NOT invent colors:**

```tsx
// ❌ WRONG - Hardcoded hex colors
<div className="bg-[#0066cc] text-[#ffffff]">

// ❌ WRONG - Tailwind default colors
<div className="bg-blue-500 text-slate-900">

// ❌ WRONG - oklch values
<div className="bg-[oklch(0.5_0.2_200)]">
```

**✅ CORRECT - Use mapped variables:**

```tsx
// ✅ CORRECT - Using your design tokens
<div className="bg-primary text-white">
<div className="bg-gray-1 text-gray-6">
<div className="text-error border-error">

// ✅ CORRECT - With opacity (if @theme configured)
<div className="bg-primary/90 text-gray-6/80">
```

#### 4. 🎨 SVG and Icon Management

**❌ FORBIDDEN - Inline SVG code:**

```tsx
// ❌ WRONG - SVG code directly in component
export const MyComponent = () => (
  <div>
    <svg width="20" height="20" viewBox="0 0 20 20">
      <path d="M10 5L5 15h10L10 5z" fill="currentColor"/>
    </svg>
  </div>
);
```

**✅ CORRECT - Proper SVG/Icon handling:**

**Option 1: Save SVG as asset file**
```tsx
// 1. Save file: src/assets/icons/arrow-down.svg
// 2. Import and use:
import ArrowIcon from '@/assets/icons/arrow-down.svg';

export const MyComponent = () => (
  <div>
    <img src={ArrowIcon} alt="Arrow" className="w-5 h-5" />
  </div>
);
```

**Option 2: Use icon library (Preferred)**
```tsx
// Use Lucide React or similar
import { ChevronDown, Search, User } from 'lucide-react';

export const MyComponent = () => (
  <div>
    <ChevronDown className="w-5 h-5 text-primary" />
    <Search className="w-6 h-6 text-gray-4" />
  </div>
);
```

**Option 3: Create dedicated icon component (for reusable custom icons)**
```tsx
// src/components/icons/ArrowDownIcon.tsx
export const ArrowDownIcon = ({ className }: { className?: string }) => (
  <svg 
    width="20" 
    height="20" 
    viewBox="0 0 20 20"
    className={className}
  >
    <path d="M10 5L5 15h10L10 5z" fill="currentColor"/>
  </svg>
);

// Use in components:
import { ArrowDownIcon } from '@/components/icons/ArrowDownIcon';
<ArrowDownIcon className="text-primary" />
```

#### 5. 🚫 NO Mass Component Generation

**❌ FORBIDDEN - Creating components upfront:**

```
DON'T create entire component libraries like:
src/components/ui/
  ├── Button/
  ├── Input/
  ├── Card/
  ├── Modal/
  ├── Dropdown/
  ├── Tooltip/
  ├── Badge/
  ├── Avatar/
  └── ... (20+ components)
```

**✅ CORRECT - Generate components on demand:**

```
ONLY create components when needed for the current screen:

Step 1: Working on Login page
  → Create: Button, Input (needed now)

Step 2: Working on Dashboard
  → Create: Card (needed now)
  → Reuse: Button, Input (already exist)

Step 3: Working on Profile
  → Create: Avatar (needed now)
  → Reuse: Button, Input, Card
```

**Rule:** Start with **Button** and **Input** as base components. Create others **only when required** by the feature you're implementing.

#### 6. ⚙️ Tailwind @theme Configuration

**REQUIRED - Configure @theme in index.css:**

```css
/* src/styles/index.css */
@import "tailwindcss";

@theme {
  /* Map YOUR variables to Tailwind utilities */
  --color-primary: var(--color-primary);
  --color-primary-light: var(--color-primary-light);
  --color-primary-dark: var(--color-primary-dark);
  
  --color-secondary: var(--color-secondary);
  --color-secondary-light: var(--color-secondary-light);
  --color-secondary-dark: var(--color-secondary-dark);
  
  --color-gray-1: var(--color-gray-1);
  --color-gray-2: var(--color-gray-2);
  --color-gray-3: var(--color-gray-3);
  --color-gray-4: var(--color-gray-4);
  --color-gray-5: var(--color-gray-5);
  --color-gray-6: var(--color-gray-6);
  
  --color-error: var(--color-error);
  --color-success: var(--color-success);
  --color-warning: var(--color-warning);
  
  /* etc... map all your variables */
}

@import './variables.css';
```

**This enables opacity modifiers:**
```tsx
<div className="bg-primary/90">       {/* 90% opacity */}
<div className="text-gray-6/80">     {/* 80% opacity */}
<div className="border-error/50">    {/* 50% opacity */}
```

#### 📋 Tailwind Project Checklist

Before starting development, ensure:

- [ ] ✅ Created `Button` component in `src/components/ui/Button.tsx`
- [ ] ✅ Created `Input` component in `src/components/ui/Input.tsx`
- [ ] ✅ Configured `@theme` in `src/styles/index.css` with ALL your variables
- [ ] ✅ Imported `variables.css` in `index.css`
- [ ] ❌ NO `theme.css` file exists
- [ ] ❌ NO `.module.css` files in components
- [ ] ❌ NO inline SVG code in components
- [ ] ❌ NO hardcoded colors or hex values
- [ ] ✅ Icon library installed (e.g., `lucide-react`)

---

### �🚫 4. FORBIDDEN: Inline Styles

❌ **NEVER use inline `style` attribute:**

```tsx
// ❌ WRONG
<div style={{ backgroundColor: '#0066cc', color: '#fff' }}>
  Content
</div>

// ✅ CORRECT - Use CSS Modules
<div className={styles.container}>
  Content
</div>

// ✅ CORRECT - Use Tailwind (if enabled)
<div className="bg-primary text-white">
  Content
</div>

---

## ⚙️ Configuration Files

### 1. package.json

```json
{
  "name": "project-name",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "author": {
    "name": "Your Name",
    "email": "your.email@example.com"
  },
  "scripts": {
    "dev": "vite --open",
    "build": "pnpm check && vite build",
    "fix": "pnpm biome check --fix ./src",
    "test": "vitest",
    "check": "pnpm biome check --error-on-warnings ./src",
    "typecheck": "tsc --noEmit -p tsconfig.src.json"
  },
  "dependencies": {
    "@tanstack/react-query": "^5.90.12",
    "i18next": "^25.7.2",
    "react": "^19.2.1",
    "react-dom": "^19.2.1",
    "react-i18next": "^16.4.0",
    "react-jwt": "^2.0.0",
    "react-router": "^7.10.1",
    "react-router-dom": "^7.10.1",
    "zod": "^4.1.13",
    "zustand": "^5.0.9"
  },
  "devDependencies": {
    "@biomejs/biome": "2.3.11",
    "@tanstack/react-query-devtools": "5.91.1",
    "@types/node": "25.0.9",
    "@types/react": "19.2.8",
    "@types/react-dom": "19.2.3",
    "@vitejs/plugin-react": "4.3.4",
    "@vitejs/plugin-react-swc": "4.2.2",
    "typescript": "5.9.3",
    "vite": "7.3.1"
  },
  "packageManager": "pnpm@10.25.0",
  "lint-staged": {
    "src/**/*.{ts,tsx,js,jsx}": [
      "pnpm biome check --write --unsafe --files-ignore-unknown=true"
    ],
    "src/**/*.{css,scss,json}": [
      "pnpm biome check --write --files-ignore-unknown=true"
    ]
  }
}
```

### 2. biome.json

```json
{
  "$schema": "https://biomejs.dev/schemas/2.3.11/schema.json",
  "assist": {
    "actions": {
      "source": {
        "organizeImports": "off"
      }
    }
  },
  "css": {
    "parser": {
      "tailwindDirectives": true
    }
  },
  "formatter": {
    "attributePosition": "multiline",
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 120
  },
  "javascript": {
    "formatter": {
      "arrowParentheses": "asNeeded",
      "bracketSpacing": false,
      "jsxQuoteStyle": "double",
      "quoteStyle": "double",
      "semicolons": "always",
      "trailingCommas": "es5"
    }
  },
  "json": {
    "formatter": {
      "trailingCommas": "none"
    }
  },
  "linter": {
    "enabled": true,
    "rules": {
      "a11y": {
        "noLabelWithoutControl": "off",
        "useFocusableInteractive": "off",
        "useKeyWithClickEvents": "off",
        "useSemanticElements": "off",
        "useValidAnchor": "off"
      },
      "complexity": {
        "noForEach": "off",
        "noImportantStyles": "off"
      },
      "correctness": {
        "useExhaustiveDependencies": "off",
        "useImportExtensions": "off",
        "useUniqueElementIds": "off"
      },
      "recommended": true,
      "security": {
        "noDangerouslySetInnerHtml": "off"
      },
      "style": {
        "noInferrableTypes": "off",
        "useImportType": "error"
      },
      "suspicious": {
        "noExplicitAny": "error",
        "noUnknownAtRules": {
          "level": "error",
          "options": {
            "ignore": ["theme", "custom-variant", "variant"]
          }
        }
      }
    }
  },
  "overrides": [
    {
      "assist": {
        "actions": {
          "source": {
            "organizeImports": "off"
          }
        }
      },
      "formatter": {
        "enabled": false
      },
      "includes": ["**/*.test.{ts,tsx,js,jsx}", "**/*.spec.{ts,tsx,js,jsx}", "**/__tests__/**/*.{ts,tsx,js,jsx}"]
    }
  ],
  "vcs": {
    "clientKind": "git",
    "enabled": true,
    "useIgnoreFile": true
  }
}

```

### 3. tsconfig.json (Root)

```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ],
  "exclude": [
    "node_modules",
    "dist",
    ".env",
    "coverage",
    "build"
  ]
}
```

### 4. tsconfig.app.json

```json
{
  "compilerOptions": {
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.app.tsbuildinfo",
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    
    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"],
  "exclude": [
    "node_modules",
    "dist",
    "build",
    "coverage",
    "**/*.test.ts",
    "**/*.test.tsx",
    "**/*.spec.ts",
    "**/*.spec.tsx"
  ]
}
```

### 5. tsconfig.node.json

```json
{
  "compilerOptions": {
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.node.tsbuildinfo",
    "target": "ES2022",
    "lib": ["ES2023"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "noEmit": true
  },
  "include": ["vite.config.ts"]
}
```

### 6. vite.config.ts

```typescript
import react from "@vitejs/plugin-react-swc";
import {defineConfig} from "vite";
import tsconfigPaths from "vite-tsconfig-paths";

export default defineConfig({
  plugins: [
    react(),
    tsconfigPaths(),
  ],
  css: {
    modules: {
      scopeBehaviour: "local",
      generateScopedName: "[name]_[local]_[hash:base64:5]",
    },
  },
  server: {
    port: 3000,
    open: true,
  },
  build: {
    outDir: "build",
    chunkSizeWarningLimit: 1000,
    sourcemap: false,
  },
});
```

### 7. .gitignore

```
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pnpm-debug.log*
lerna-debug.log*

node_modules
dist
dist-ssr
*.local
build

# Environment variables
.env
.env.local
.env.development.local
.env.staging.local
.env.production.local

# Editor directories and files
.vscode/*
!.vscode/extensions.json
.idea
.DS_Store
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?

# Coverage
coverage
.coverage
coverage-report
coverage-final.json

# CI/CD
.pnpm-store
biome-report.xml
junit.xml

# Backup files
*.bak
```

### 8. Husky Configuration

#### Install Husky
```bash
pnpm add -D husky
pnpm exec husky init
```

#### .husky/pre-commit
```bash
echo "🔍 Running pre-commit checks..."

# Run lint-staged (applies Biome fixes to staged files)
echo "📋 Processing staged files with lint-staged..."
npx lint-staged

echo "✅ Pre-commit checks passed!"
```

#### .husky/pre-push
```bash
echo "🔍 Running pre-push checks..."
echo ""

# 1. Run Biome checks
echo "🔧 Running Biome code quality checks..."
pnpm check

BIOME_EXIT_CODE=$?

if [ $BIOME_EXIT_CODE -ne 0 ]; then
  echo ""
  echo "❌ Biome checks failed!"
  echo ""
  echo "💡 Fix the issues with: pnpm fix"
  echo "   Or review them with: pnpm biome check src"
  echo ""
  exit 1
fi

# 2. Run TypeScript type checking
echo ""
echo "🔧 Running TypeScript type checking..."
pnpm typecheck

TSC_EXIT_CODE=$?

if [ $TSC_EXIT_CODE -ne 0 ]; then
  echo ""
  echo "❌ TypeScript type checking failed!"
  echo "   Please fix the type errors before pushing."
  echo ""
  exit 1
fi

echo ""
echo "✅ All pre-push checks passed! Ready to push."
exit 0
```

---

## 📝 Coding Standards & Best Practices

### General Principles
1. **Keep It Simple**: Avoid over-engineering. Simplicity is priority over clever solutions
2. **No Deprecated/Legacy Code**: Always use ES2022+ features (December 2025 standards)
3. **Functional Programming Only**: ❌ No `class`, `constructor`, `this`, inheritance
4. **Pure Functions**: Prefer functions without side effects
5. **Arrow Functions**: Use as default function syntax

### React 19+ Rules
- ❌ **DO NOT import React for JSX** - Not needed in React 17+ with new JSX Transform
- ✅ **ONLY import specific hooks/types** when needed

```tsx
// ❌ WRONG - Unnecessary in React 19
import React from 'react';

export const MyComponent = () => {
  return <div>Hello</div>;
};

// ✅ CORRECT - No React import needed for JSX
export const MyComponent = () => {
  return <div>Hello</div>;
};

// ✅ CORRECT - Import only what you need
import { useState, useEffect } from 'react';
import type { FC } from 'react';

export const MyComponent: FC = () => {
  const [count, setCount] = useState(0);
  return <div>{count}</div>;
};
```

### TypeScript Rules
- ❌ **Never use `any`** - use specific types or `unknown`
- ✅ Use `interface` for object shapes, `type` for unions
- ✅ Enable strict mode in tsconfig.json
- ✅ Define types first, then implement (Type-Driven Development)
- ✅ Use React built-in types when needed: `FC`, `ComponentProps`, `ReactNode`
- ✅ Import types with `import type { ... }` for better tree-shaking

### Component Design
- ✅ **Components under 200 lines** - if exceeded, refactor
- ✅ **Extract logic to custom hooks** - keep components focused on UI
- ✅ **Extract calculations to utils** - pure functions in `src/utils/`
- ❌ **Avoid multiple `useEffect`** in one component
- ✅ **Single Responsibility Principle** - each component does one thing
- ❌ **No `import React from 'react'`** - unnecessary in React 19

### Comments & Documentation
- ❌ **No inline comments** for obvious code
- ✅ **JSDoc only for complex functions** - document the "why", not the "what"
- ❌ **No commented-out code** - delete before committing
- ✅ **All documentation in English**

### State Management Rules
- **Local State**: Use `useState` for component-local state
- **Complex State**: Use `useReducer` for state machines
- **Global State**: Use **Zustand** stores
- **Server State**: Use **TanStack Query** (React Query)
- **Persistence**: Use **CookieStore API** or `sessionStorage`

### Internationalization (i18n)
- ✅ **All user-facing text MUST use i18n keys**
- ✅ **Before adding translations**: Verify key doesn't already exist
- ✅ **Create keys in ALL language files** (en, es, etc.)
- ❌ **No hardcoded strings in UI components**

### Security
- ✅ Sanitize all user inputs (prevent XSS)
- ✅ Use HTTPS for all API calls
- ❌ **Never log sensitive data** (tokens, user IDs, passwords)
- ❌ **Remove all `console.log` before production**

### Accessibility
- ✅ Use semantic HTML elements
- ✅ Implement ARIA attributes where needed
- ✅ Ensure keyboard navigation works
- ✅ Provide alt text for images
- ✅ Test with screen readers

---

## ✅ Mandatory Post-Task Verification Checklist

**After completing ANY task, the AI MUST verify:**

### Code Cleanliness
- [ ] Remove unused variables
- [ ] Remove unused imports
- [ ] Remove unused functions
- [ ] Remove all `console.log`, `console.error`, `console.warn`
- [ ] Remove commented-out code

### Deprecated Code Check
- [ ] No deprecated npm packages
- [ ] No deprecated React/Node.js APIs
- [ ] Check migration guides for breaking changes
- [ ] Use latest stable versions (December 2025 standards)

### Code Quality
- [ ] No linting errors (`pnpm check`)
- [ ] No TypeScript errors (`pnpm typecheck`)
- [ ] Proper error handling (try/catch for async operations)
- [ ] Accessibility compliance (ARIA, semantic HTML, keyboard navigation)
- [ ] Performance optimization (no unnecessary re-renders)

### Architecture Compliance
- [ ] Clean Architecture: Business logic in domain layer
- [ ] Vertical Slicing: Code organized by feature/domain
- [ ] Service Layer Pattern: UI <-> Hook (React Query + Mapper) <-> Service
- [ ] Functional programming: Pure functions, no classes/constructors/this
- [ ] Component size: Under 200 lines, complex logic in hooks/utils

### Project-Specific
- [ ] Zustand for global state
- [ ] CookieStore API for persistence (not localStorage)
- [ ] react-jwt for token validation
- [ ] i18n translations:
  - All user-facing text uses i18n keys
  - Verified keys don't already exist
  - Created keys in ALL language files
  - No hardcoded strings
- [ ] HTTP calls use `src/infra/http/http.ts`
- [ ] Services consumed via React Query hooks with mappers

### Verification Commands
```bash
# Fix code quality issues
pnpm fix

# Check for errors
pnpm check

# TypeScript validation
pnpm typecheck

# Check for outdated packages
pnpm outdated

# Run tests
pnpm test
```

---

## �️ React Router Configuration & Route Structure

### Router Setup Pattern

Our routing system follows a **centralized, modular pattern** with:
- Centralized route definitions separated by access level (public/private)
- Lazy loading for code splitting and performance
- Nested layouts for consistent UI structure
- Type-safe path constants to prevent typos

### File Structure for Routing

```
src/
├── routes.tsx                    # Main router configuration
├── App.tsx                       # App component with RouterProvider
├── main.tsx                      # Entry point
│
└── navigation/
    ├── paths.ts                  # Centralized path constants (CRITICAL)
    ├── public.routes.tsx         # Public routes configuration
    ├── private.routes.tsx        # Private routes configuration
    ├── PrivateRoute.tsx          # Private route guard component
    └── PublicRoute.tsx           # Public route guard component (optional)
```

### 1. Centralized Path Constants (`navigation/paths.ts`)

**CRITICAL**: Always define paths in a centralized file to avoid circular dependencies and hardcoding.

```typescript
/**
 * Application paths constants
 * Centralized location for all route paths to avoid circular dependencies
 */

export const publicPaths = {
  home: "/",
  login: "/login",
  forgotPassword: "/forgot-password",
  resetPassword: "/reset-password",
  register: "/register",
} as const;

export const privatePaths = {
  dashboard: {
    path: "dashboard",
    link: "/dashboard",
    title: "Dashboard",
  },
  profile: {
    path: "profile",
    link: "/profile",
    title: "My Profile",
  },
  settings: {
    path: "settings",
    link: "/settings",
    title: "Settings",
  },
} as const;

// Export type for autocomplete support
export type PublicPaths = typeof publicPaths;
export type PrivatePaths = typeof privatePaths;
```

### 2. Main Router Configuration (`routes.tsx`)

```typescript
import {createBrowserRouter, Outlet, type RouteObject} from "react-router-dom";
import ErrorBoundary from "./components/ErrorBoundary";
import {usePageTracking} from "./hooks/usePageTracking";
import privateRoutes from "./navigation/private.routes";
import publicRoutes from "./navigation/public.routes";

const allRoutes: RouteObject[] = [...publicRoutes, ...privateRoutes];

function RootLayout() {
  usePageTracking(); // Analytics tracking on route changes
  return <Outlet />;
}

const router = createBrowserRouter(
  [
    {
      element: <RootLayout />,
      errorElement: <ErrorBoundary />,
      children: [
        {
          path: "/",
          children: allRoutes,
        },
      ],
    },
  ],
  {
    basename: "/", // Change if app is served from subdirectory
  }
);

export default router;
```

### 3. Public Routes Configuration (`navigation/public.routes.tsx`)

```typescript
import {lazy, Suspense} from "react";
import type {RouteObject} from "react-router-dom";

// Lazy load layouts
const PublicLayout = lazy(() => import("../pages/layout/Public/Home"));

// Lazy load pages
const Login = lazy(() => import("../pages/public/Auth/Login"));
const Register = lazy(() => import("../pages/public/Auth/Register"));
const ForgotPassword = lazy(() => import("../pages/public/Auth/ForgotPassword"));
const ResetPassword = lazy(() => import("../pages/public/Auth/ResetPassword"));

import RouteLoading from "../components/RouteLoading";
import {publicPaths} from "./paths";

// Reusable Suspense wrapper
const withSuspense = (Component: React.ComponentType) => (
  <Suspense fallback={<RouteLoading />}>
    <Component />
  </Suspense>
);

const publicRoutes: RouteObject[] = [
  {
    element: withSuspense(PublicLayout),
    children: [
      {
        element: withSuspense(Login),
        index: true, // Default route
      },
      {
        path: publicPaths.login,
        element: withSuspense(Login),
      },
      {
        path: publicPaths.register,
        element: withSuspense(Register),
      },
      {
        path: publicPaths.forgotPassword,
        element: withSuspense(ForgotPassword),
      },
      {
        path: publicPaths.resetPassword,
        element: withSuspense(ResetPassword),
      },
    ],
  },
];

export default publicRoutes;
```

### 4. Private Routes Configuration (`navigation/private.routes.tsx`)

```typescript
import {lazy, Suspense} from "react";
import type {RouteObject} from "react-router-dom";

// Lazy load layouts
const PrivateLayout = lazy(() => import("../pages/layout/Private/Home"));

// Lazy load pages
const Dashboard = lazy(() => import("../pages/private/Dashboard"));
const Profile = lazy(() => import("../pages/private/Profile"));
const Settings = lazy(() => import("../pages/private/Settings"));

import RouteLoading from "../components/RouteLoading";
import PrivateRoute from "./PrivateRoute";
import {privatePaths} from "./paths";

const withSuspense = (Component: React.ComponentType) => (
  <Suspense fallback={<RouteLoading />}>
    <Component />
  </Suspense>
);

const privateRoutes: RouteObject[] = [
  {
    element: <PrivateRoute />, // Route guard wrapper
    children: [
      {
        element: withSuspense(PrivateLayout),
        children: [
          {
            path: privatePaths.dashboard.path,
            element: withSuspense(Dashboard),
          },
          {
            path: privatePaths.profile.path,
            element: withSuspense(Profile),
          },
          {
            path: privatePaths.settings.path,
            element: withSuspense(Settings),
          },
        ],
      },
    ],
  },
];

export default privateRoutes;
```

### 5. Private Route Guard (`navigation/PrivateRoute.tsx`)

```typescript
import {Navigate, Outlet} from "react-router-dom";
import {useAuthStore} from "../store/authStore";
import {publicPaths} from "./paths";

const PrivateRoute = () => {
  const {isAuthenticated} = useAuthStore();

  if (!isAuthenticated) {
    return <Navigate to={publicPaths.login} replace />;
  }

  return <Outlet />;
};

export default PrivateRoute;
```

### 6. App Component with RouterProvider (`App.tsx`)

```typescript
import {QueryClient, QueryClientProvider} from "@tanstack/react-query";
import {lazy, Suspense} from "react";
import {RouterProvider} from "react-router-dom";
import routes from "./routes";
import "./styles/index.css";

const LazyDevtools = import.meta.env.DEV
  ? lazy(() => import("@tanstack/react-query-devtools").then(mod => ({default: mod.ReactQueryDevtools})))
  : () => null;

const LoadingComponent = () => {
  return (
    <div style={{height: "100vh", display: "grid", placeItems: "center"}}>
      <span>Loading...</span>
    </div>
  );
};

function App() {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: {
        refetchOnWindowFocus: false,
        retry: 1,
      },
    },
  });

  return (
    <QueryClientProvider client={queryClient}>
      <Suspense fallback={<LoadingComponent />}>
        <RouterProvider router={routes} />
      </Suspense>
      <Suspense fallback={null}>
        <LazyDevtools initialIsOpen={false} />
      </Suspense>
    </QueryClientProvider>
  );
}

export default App;
```

### 7. Route Loading Component (`components/RouteLoading/index.tsx`)

```typescript
const RouteLoading = () => {
  return (
    <div style={{height: "100vh", display: "grid", placeItems: "center"}}>
      <div>Loading page...</div>
    </div>
  );
};

export default RouteLoading;
```

### Key Routing Principles

1. **Centralized Path Constants**: All paths defined in `navigation/paths.ts`
2. **Lazy Loading**: Use `React.lazy()` for code splitting on ALL routes
3. **Suspense Boundaries**: Wrap lazy components with `<Suspense>` and loading fallbacks
4. **Route Guards**: Use `PrivateRoute` wrapper for protected routes
5. **Nested Layouts**: Use layouts for consistent UI structure across related routes
6. **Type Safety**: Export path types for autocomplete support
7. **Separation of Concerns**: Public and private routes in separate files
8. **Error Boundaries**: Use `errorElement` for graceful error handling

### Navigation Examples

```typescript
import {useNavigate, Link} from "react-router-dom";
import {privatePaths, publicPaths} from "../navigation/paths";

const MyComponent = () => {
  const navigate = useNavigate();

  // Programmatic navigation
  const handleLogin = () => {
    navigate(privatePaths.dashboard.link);
  };

  return (
    <div>
      {/* Declarative navigation */}
      <Link to={publicPaths.login}>Login</Link>
      <Link to={privatePaths.profile.link}>{privatePaths.profile.title}</Link>
      
      {/* Programmatic navigation */}
      <button onClick={handleLogin}>Go to Dashboard</button>
    </div>
  );
};
```

---

## �🚀 Initial Setup Commands

```bash
# 1. Create project with Vite
pnpm create vite@latest project-name --template react-swc-ts

# 2. Install dependencies
cd project-name
pnpm install

# 3. Install required packages
pnpm add @tanstack/react-query zustand react-router-dom react-router i18next react-i18next react-jwt luxon zod

# 4. Install dev dependencies
pnpm add -D @biomejs/biome husky lint-staged vitest @vitest/coverage-v8 jsdom @testing-library/react @testing-library/jest-dom @testing-library/user-event vite-tsconfig-paths @tanstack/react-query-devtools @types/luxon @types/node

# 5. Initialize Husky
pnpm exec husky init

# 6. Create Biome config
# (Copy biome.json content from above)

# 7. Create .husky/pre-commit and .husky/pre-push
# (Copy scripts from above)

# 8. Create directory structure
mkdir -p src/{components,hooks,utils,store,constants,types,interfaces,validators,providers,navigation,infra/http,adapters,services,pages,assets,styles,test}
mkdir -p src/pages/{public/Auth,private,layout/Public,layout/Private}
mkdir -p docs  # Documentation folder at root

# 9. Add Google Fonts (Poppins) to index.html
# Add the following in the <head> section of index.html:
# <link rel="preconnect" href="https://fonts.googleapis.com">
# <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
# <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">

# 10. Create routing files
# Create routes.tsx, navigation/paths.ts, navigation/public.routes.tsx, navigation/private.routes.tsx
# (Copy router configuration examples from above)

# 11. Create RouteLoading component
# (Copy RouteLoading component from above)

# 12. Create initial CSS files with Poppins font
# Create src/styles/variables.css (copy CSS variables template from above)
# Create src/styles/reset.css (copy reset CSS with Poppins from above)
# Create src/styles/utilities.css (copy utilities CSS from above)
# Create src/styles/index.css (import all partials)

# 13. Verify no deprecated packages
pnpm outdated
pnpm audit

# 14. Run initial checks
pnpm fix
pnpm check
pnpm typecheck
```

---

## 📚 Additional References

For detailed coding standards and best practices, refer to:
- **`.github/copilot-instructions.md`** - Complete coding standards and best practices
- **`.github/copilot-summary-instructions.md`** - Documentation guidelines
- **`.github/refactoring-instructions.md`** - Refactoring best practices

---

## 🎯 Success Criteria

The project initialization is complete when:

1. ✅ All configuration files are created and valid **in the project root**
   - package.json, tsconfig.json, vite.config.ts, biome.json, .gitignore
2. ✅ Directory structure follows Vertical Slicing pattern
3. ✅ `docs/` folder created at project root for additional documentation
4. ✅ CSS color system is implemented correctly (no generic variable names)
5. ✅ **Google Fonts (Poppins)** integrated in index.html
   - All weights installed (300, 400, 500, 600, 700, 800, 900)
   - Font-family set in reset.css
6. ✅ Husky pre-commit and pre-push hooks are working
7. ✅ Biome linting passes without errors
8. ✅ TypeScript strict mode is enabled and compiling without errors
9. ✅ **All packages are latest stable versions (not deprecated)**
   - Run `pnpm outdated` - no critical outdated packages
   - Run `pnpm audit` - no security vulnerabilities
10. ✅ All dev dependencies are installed
11. ✅ Package manager is set to pnpm
12. ✅ Git repository is initialized with proper .gitignore
13. ✅ Project runs successfully with `pnpm dev`
14. ✅ **If using Tailwind CSS:**
    - Button and Input components created in `src/components/ui/`
    - `@theme` configured in `src/styles/index.css` with all variables mapped
    - NO `theme.css` file exists
    - NO `.module.css` files in components
    - Icon library installed (e.g., `lucide-react`)
    - NO inline SVG code in components

---

## ⚠️ Critical Reminders

- **Configuration files location:** ALL config files MUST be in project root (package.json, tsconfig, vite.config, etc.)
- **Documentation:** Create `docs/` folder at root for additional documentation files
- **No deprecated packages:** Always use latest stable versions, verify with `pnpm outdated` and `pnpm audit`
- **Typography:** Use Poppins from Google Fonts as default font family
- **NO classes, constructors, or `this`** - Functional programming only
- **React 19+ imports:** ❌ DO NOT `import React from 'react'` - unnecessary with new JSX Transform

### Styling Approach (CHOOSE ONE):
- **🚨 CRITICAL DECISION:** Choose CSS Modules + BEM **OR** Tailwind CSS, **NEVER BOTH**
- **CSS Modules + BEM (DEFAULT):** Use when user does NOT mention Tailwind
  - Create `.module.css` files with BEM naming
  - NO utility classes in HTML
  - NO Tailwind installation
- **Tailwind CSS (ALTERNATIVE):** Use ONLY when user explicitly requests it
  - Install and configure Tailwind
  - Use utility classes in components
  - NO `.module.css` files
  - NO BEM methodology
- **DO NOT MIX:** Never use CSS Modules AND Tailwind in the same project

### Color System (SAME FOR BOTH APPROACHES):
- **NO generic color variables** - FORBIDDEN: `--background`, `--foreground`, `--card`, `--primary-foreground`, etc.
- **ONLY use our color system** - All variables MUST start with `--color-` (e.g., `--color-primary`, `--color-gray-1`)
- **NO theme.css files** - Never create separate theme.css files with custom colors
- **If Tailwind is used** - Configure `@theme` in index.css to map YOUR variables

### Tailwind-Specific Rules (IF using Tailwind):
- **✅ Component Reusability:** ALWAYS use existing components from `src/components/ui/` instead of raw HTML
  - ❌ DON'T: `<button className="...">`
  - ✅ DO: `<Button variant="primary">`
- **❌ NO theme.css:** Never create separate theme.css files with oklch() or custom color definitions
- **✅ ONLY index.css:** Your single source of truth for design tokens
- **✅ Use YOUR variables:** Only use mapped variables (bg-primary, text-error), NEVER Tailwind defaults (bg-blue-500)
- **❌ NO inline SVGs:** Save SVGs in `src/assets/` or use icon library (lucide-react)
- **✅ Generate on demand:** Only create components when needed (start with Button + Input)
- **✅ Configure @theme:** Map all your CSS variables in index.css for opacity support

### Other Rules:
- **NO localStorage** - Use CookieStore API or sessionStorage
- **Component logic extraction** - Keep components under 200 lines
- **Vertical Slicing** - Organize by feature/domain, not by technical layer
- **i18n is mandatory** - All user-facing text must use i18n keys
- **Always verify post-task checklist** - Non-negotiable for every task

---

**Remember**: This is the TECHNICAL foundation only. After completing this setup, the next phase will cover business logic, features, and application-specific requirements.
