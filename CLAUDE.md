# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **GitHub Spark Template** - a minimal React + TypeScript starter project pre-configured with GitHub's Spark design system, shadcn/ui components, and Tailwind CSS v4. It serves as a clean foundation for building Spark-based applications.

## Core Technologies

- **Runtime**: Vite 6 with React 19 and SWC
- **Spark**: GitHub's Spark design system (`@github/spark`)
- **UI Components**: shadcn/ui (customized for Spark) with Radix UI primitives
- **Styling**: Tailwind CSS v4 with custom Spark theming
- **TypeScript**: v5.7 with strict null checks
- **Icons**: Phosphor Icons (proxied via Spark), Heroicons, Lucide React
- **State Management**: TanStack Query v5
- **Forms**: React Hook Form with Zod validation

## Development Commands

```bash
# Install dependencies
npm install

# Start development server (default: http://localhost:5173)
npm run dev

# Kill process on port 5000 (if needed)
npm run kill

# Build for production
npm run build

# Preview production build
npm run preview

# Lint code
npm run lint

# Optimize dependencies
npm run optimize
```

## Architecture

### Directory Structure

```
src/
├── components/
│   └── ui/              # shadcn/ui components (50+ pre-built)
├── hooks/               # Custom React hooks (use-mobile.ts)
├── lib/
│   └── utils.ts         # Utility functions (cn, tailwind-merge)
├── styles/
│   └── theme.css        # Spark theme CSS variables
├── App.tsx              # Root application component (currently empty)
├── main.tsx             # React entry point with ErrorBoundary
├── ErrorFallback.tsx    # Error boundary fallback UI
└── main.css             # Global styles + Tailwind imports
```

### Spark Integration

The template includes critical Spark-specific configuration:

1. **Vite Plugin**: `sparkPlugin()` in `vite.config.ts` - DO NOT REMOVE
2. **Icon Proxy**: `createIconImportProxy()` - required for Phosphor Icons
3. **Theme System**: CSS variables in `src/styles/theme.css` mapped to Tailwind
4. **Theme Override**: `theme.json` allows runtime theme customization

### Theming System

Spark uses a dual-theme system with CSS variables:

- **Colors**: `neutral`, `accent`, `accent-secondary` (1-12 scale + alpha variants)
- **Semantic Colors**: `fg`, `bg`, `focus-ring`
- **Dark Mode**: Via `data-appearance="dark"` attribute
- **Custom Themes**: Modify `theme.json` to extend Tailwind config at build time

### Path Aliases

TypeScript and Vite are configured with `@/` alias:

```typescript
import { Button } from "@/components/ui/button"
import { cn } from "@/lib/utils"
```

## UI Component Usage

All UI components follow shadcn/ui patterns with Spark theming:

```tsx
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardContent } from "@/components/ui/card"

function Example() {
  return (
    <Card>
      <CardHeader>Title</CardHeader>
      <CardContent>
        <Button variant="default">Click me</Button>
      </CardContent>
    </Card>
  )
}
```

Available component categories:
- Layout: `sidebar`, `resizable`, `scroll-area`, `separator`
- Forms: `input`, `textarea`, `select`, `checkbox`, `radio-group`, `slider`, `switch`
- Overlays: `dialog`, `sheet`, `drawer`, `popover`, `hover-card`, `tooltip`
- Navigation: `navigation-menu`, `menubar`, `breadcrumb`, `tabs`, `pagination`
- Display: `card`, `badge`, `avatar`, `skeleton`, `progress`, `chart`
- Feedback: `alert`, `alert-dialog`, `sonner` (toast)

## Important Conventions

### React 19 Features

This template uses React 19.0.0:
- Use `createRoot()` (not legacy `ReactDOM.render()`)
- Error boundaries via `react-error-boundary` package
- Forms work with `react-hook-form` + Zod schema validation

### Spark Plugin Requirements

**CRITICAL**: The Vite config includes two required plugins:

```typescript
// vite.config.ts
plugins: [
  react(),
  tailwindcss(),
  createIconImportProxy() as PluginOption,  // DO NOT REMOVE
  sparkPlugin() as PluginOption,            // DO NOT REMOVE
]
```

Removing these will break Spark functionality.

### Build Configuration

The build script uses `tsc -b --noCheck` to skip type checking for faster builds. Type errors should be caught during development via editor tooling.

## Adding New Components

When adding shadcn/ui components, use the configuration in `components.json`:

```bash
npx shadcn@latest add [component-name]
```

This will:
- Install component to `src/components/ui/`
- Use "new-york" style preset
- Apply Spark-compatible theming
- Configure path aliases automatically

## Workspaces

The project is configured for npm workspaces (see `package.json`):

```json
"workspaces": {
  "packages": ["packages/*"]
}
```

Future monorepo packages should be added to the `packages/` directory.

## Development Notes

- The template starts with a blank `App.tsx` - this is intentional
- Error boundaries wrap the app root for production stability
- Phosphor Icons are imported via Spark's proxy plugin (handles optimization)
- Use `lucide-react` or `@heroicons/react` for additional icons
- Forms should use `react-hook-form` + `@hookform/resolvers` with Zod schemas
- Queries should use TanStack Query for data fetching

## GitHub Spark Specifics

This template is designed to work within GitHub's Spark ecosystem:

- `spark.meta.json`: Spark template metadata (version tracking)
- Theme customization via `theme.json` + CSS variables
- Integration with GitHub Octokit (`@octokit/core`, `octokit` packages included)
- Designed for rapid prototyping in GitHub Codespaces

## Testing New Features

When testing new Spark features:

1. Keep the minimal App.tsx structure initially
2. Add components incrementally to `App.tsx`
3. Use the error boundary to catch issues early
4. Test in both light/dark modes via `data-appearance` attribute
5. Verify custom theme changes in `theme.json` apply correctly
