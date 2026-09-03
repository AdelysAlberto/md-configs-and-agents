---
name: i18n
description: Internationalization (i18n) and localization standards. Use when working on files matching: src/**/*.ts, src/**/*.tsx, src/providers/lang/**, src/infrastructure/lang/**.
---

# Internationalization (i18n)

## Rule: All User-Facing Text Must Be Translated

Never hardcode text that appears in the UI. Every string visible to users must go through the i18n system.

```typescript
// ❌ WRONG — hardcoded text
<h1>Welcome back</h1>
<BaseButton label="Save changes" />

// ✅ CORRECT — translated key
const { t } = useTranslation();
<h1>{t('dashboard.welcome')}</h1>
<BaseButton label={t('common.saveChanges')} />
```

## File Locations

| File | Language |
|---|---|
| `src/infrastructure/lang/en/` | English (per-domain JSON files) |
| `src/infrastructure/lang/es/` | Spanish (per-domain JSON files) |
| `src/infrastructure/lang/index.ts` | i18n provider setup |

Import the hook directly from `react-i18next`:
```typescript
import { useTranslation } from 'react-i18next';
const { t } = useTranslation();
```

## Adding New Text — Required Steps

1. Add the key to **both** `en.json` and `es.json`
2. Use **dot-notation namespacing** matching the feature/page
3. Never add a key to one file without adding it to the other

```json
// en.json
{
  "account": {
    "title": "Account Summary",
    "balance": "Current Balance",
    "errors": {
      "loadFailed": "Failed to load account data"
    }
  }
}

// es.json
{
  "account": {
    "title": "Resumen de Cuenta",
    "balance": "Saldo Actual",
    "errors": {
      "loadFailed": "No se pudo cargar la información de la cuenta"
    }
  }
}
```

## Key Naming Convention

```
{page/feature}.{section}.{element}
```

Examples:
- `common.save` — shared across pages
- `account.title` — page-level heading
- `account.errors.loadFailed` — error in account context
- `navigation.menu.home` — navigation item

## Dynamic Values in Translations

Use interpolation for dynamic content — never string concatenation:

```typescript
// ❌ WRONG
const message = t('account.greeting') + ' ' + userName;

// ✅ CORRECT
const message = t('account.greeting', { name: userName });
// en.json: { "greeting": "Welcome, {{name}}" }
```

## Rules Summary

- All UI text → i18n key
- Add to **both** language files simultaneously
- Use namespacing to avoid key collisions
- Interpolation over concatenation
- Error messages and validation messages also need translation
