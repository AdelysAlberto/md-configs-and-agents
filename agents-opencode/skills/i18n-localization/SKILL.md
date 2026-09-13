---
name: i18n-localization
description: Internationalization (i18n) standards, react-i18next setup, namespaced translation keys, pluralization, and RTL logical CSS properties. Use when localizing UI text or adding multi-language support.
license: MIT
compatibility: opencode
metadata:
  domain: frontend
  tooling: react-i18next
---

# Internationalization (i18n) & Localization Standards

Master guide for full internationalization and localization using **react-i18next** and logical CSS properties.

## 1. Core i18n Invariants

1. **Zero Hardcoded User-Facing Strings**: All labels, headings, placeholders, toasts, and buttons must be wrapped in `t('namespace.key')`.
2. **Namespaced Organization**: Keys must be structured logically by feature domain (`auth.login.submit`, `routes.hud.turn_left`).
3. **Bilingual Parity**: Any new translation key introduced must exist with values in all supported locales (e.g. `es` and `en`).
4. **Logical CSS Properties**: Support bidirectional layouts (LTR / RTL) by using CSS logical properties (`margin-inline-start`, `padding-inline-end`) rather than physical directions (`left`, `right`).

---

## 2. Locale File Layout

```text
public/locales/
├── en/
│   ├── common.json
│   ├── auth.json
│   └── routes.json
└── es/
    ├── common.json
    ├── auth.json
    └── routes.json
```

---

## 3. Usage Pattern in React Components

```tsx
import { useTranslation } from 'react-i18next';

export const WelcomeBanner = ({ userName, unreadCount }: Props) => {
  const { t } = useTranslation('dashboard');

  return (
    <section>
      <h1>{t('welcome_back', { name: userName })}</h1>
      <p>{t('unread_messages', { count: unreadCount })}</p>
    </section>
  );
};
```

JSON translation file (`dashboard.json`):
```json
{
  "welcome_back": "¡Bienvenido de nuevo, {{name}}!",
  "unread_messages_one": "Tienes 1 mensaje sin leer",
  "unread_messages_other": "Tienes {{count}} mensajes sin leer"
}
```
