# Copilot Instructions for Covalience Next.js + shadcn/ui Project

## Project Overview

This is a Next.js 15 project with React 19, combining MUI Material components with TailwindCSS v4 and shadcn/ui patterns. The hybrid approach uses MUI for complex components while leveraging Tailwind's utility-first styling.

## Architecture & Key Patterns

### Tech Stack Integration

- **Next.js 15** with App Router (`src/app/` directory structure)
- **React 19** with latest features and concurrent rendering
- **MUI Material v7** integrated via `AppRouterCacheProvider` in layout
- **TailwindCSS v4** with inline theme configuration in `globals.css`
- **TypeScript** with strict mode and path aliases (`@/*` → `./src/*`)

### Development Workflow

```bash
# Development with Turbopack (faster builds)
npm run dev

# Production build
npm run build && npm start

# Linting with Next.js ESLint config
npm run lint
```

## File Structure Conventions

### Pages & Routing

- Use App Router pattern: `src/app/[route]/page.tsx`
- Example: `src/app/about/page.tsx` creates `/about` route
- Default exports for page components: `export default function PageName()`

### Styling Architecture

- **Global styles**: `src/app/globals.css` with CSS custom properties
- **Theme system**: CSS variables for light/dark mode in `:root` and `@media (prefers-color-scheme: dark)`
- **Font loading**: Geist fonts loaded in layout with CSS variables (`--font-geist-sans`, `--font-geist-mono`)

### Component Patterns

- MUI components wrapped in `AppRouterCacheProvider` for SSR compatibility
- TailwindCSS utilities for layout and spacing
- CSS custom properties for theming: `--background`, `--foreground`, `--color-*`

## Development Guidelines

### Component Creation

- Use functional components with TypeScript
- Import MUI components: `import { ComponentName } from '@mui/material'`
- Apply Tailwind classes alongside MUI styling
- Use `@/*` path alias for internal imports

### Styling Best Practices

- Combine MUI's component library with Tailwind utilities
- Use CSS custom properties for consistent theming
- Reference existing color variables: `var(--background)`, `var(--foreground)`
- Dark mode handled automatically via CSS media queries

### Configuration Files

- **TypeScript**: Path mapping configured for `@/*` alias
- **ESLint**: Uses Next.js recommended config with TypeScript support
- **PostCSS**: TailwindCSS v4 plugin only
- **Next.js**: Minimal config, relies on defaults for App Router

## Key Integration Points

- MUI cache provider must wrap app content in `layout.tsx`
- Font variables applied via className in body element
- TailwindCSS imported as single `@import "tailwindcss"` statement
- Theme tokens defined inline in CSS using `@theme inline` directive
