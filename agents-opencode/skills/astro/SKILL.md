---
name: astro
description: Skill for building with the Astro web framework. Helps create Astro components and pages, configure SSR adapters, set up content collections, deploy static sites, and manage project structure and CLI commands. Use when the user mentions .astro files, SSG, islands architecture, content collections, or deploying Astro.
mode: subagent
license: MIT
---

# Astro Skill (Opencode Adaptation)

You are the **Astro Specialist** for Opencode. Astro is the web framework for content-driven websites.

## Quick Reference

### File Location
CLI looks for `astro.config.js`, `astro.config.mjs`, `astro.config.cjs`, and `astro.config.ts` in: `./`. Use `--config` for custom path.

### CLI Commands
- `npx astro dev` - Start the development server.
- `npx astro build` - Build your project and write it to disk.
- `npx astro check` - Check your project for errors.
- `npx astro add` - Add an integration.
- `npx astro sync` - Generate TypeScript types for all Astro modules.

**Re-run after adding/changing plugins.**

### Project Structure
- `src/*` - Project source code (components, pages, styles, images, etc.)
- `src/pages` - **Required.** Defines all pages and routes.
- `src/components` - Components (convention, not required).
- `src/layouts` - Layout components (convention, not required).
- `src/styles` - CSS/Sass files (convention, not required).
- `public/*` - Non-code, unprocessed assets (fonts, icons, etc.); copied as-is to build output.
- `package.json` - Project manifest.
- `astro.config.{js,mjs,cjs,ts}` - Astro configuration file. (recommended)
- `tsconfig.json` - TypeScript configuration file. (recommended)

### Core Config Options
| Option | Notes |
|--------|-------|
| `site` | Your final, deployed URL. Used to generate sitemaps and canonical URLs. |

### Example `astro.config.ts`
```typescript
import { defineConfig } from 'astro/config';

export default defineConfig({
  site: 'https://example.com',
});
```

### Common Workflows

#### Creating a Basic Page
Add a file to `src/pages/` — the filename becomes the route:

```astro
---
// src/pages/index.astro
const title = 'Hello, Astro!';
---

<html>
  <head><title>{title}</title></head>
  <body>
    <h1>{title}</h1>
  </body>
</html>
```

#### Creating a Component
```astro
---
// src/components/Card.astro
const { title, body } = Astro.props;
---

<div class="card">
  <h2>{title}</h2>
  <p>{body}</p>
</div>
```

#### Deploying with an Adapter
1. Add the adapter: `npx astro add vercel --yes` (or `node`, `cloudflare`, `netlify`)
2. Run `npx astro check` to catch type and configuration errors before building.
3. Run `npx astro build` to produce the deployment artifact.
4. Verify the build output directory (e.g. `dist/`) exists and is non-empty before proceeding.
5. Deploy the output per the adapter's documentation.

### Adapters
Deploy to your favorite server, serverless, or edge host with build adapters.

- **Node.js adapter**: `npx astro add node --yes`
- **Cloudflare adapter**: `npx astro add cloudflare --yes`
- **Netlify adapter**: `npx astro add netlify --yes`
- **Vercel adapter**: `npx astro add vercel --yes`

Other adapters available at: https://astro.build/integrations/

### Resources
- [Docs](https://docs.astro.build)
- [Config Reference](https://docs.astro.build/en/reference/configuration-reference/)
- [GitHub](https://github.com/withastro/astro)