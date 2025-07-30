# Copilot Instructions for Covalience Next.js + shadcn/ui Project

## Project Overview

This is a Next.js 15 project with React 19, combining MUI Material components with TailwindCSS v4 and shadcn/ui components. The hybrid approach uses MUI for complex components, shadcn/ui for consistent UI patterns, while leveraging Tailwind's utility-first styling.

## Architecture & Key Patterns

### Tech Stack Integration

- **Next.js 15** with App Router (`src/app/` directory structure)
- **React 19** with latest features and concurrent rendering
- **shadcn/ui** component library for consistent UI patterns and accessibility
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

# Add shadcn/ui components
npx shadcn@latest add [component-name]
```

## File Structure Conventions

### Pages & Routing

- Use App Router pattern: `src/app/[route]/page.tsx`
- Example: `src/app/about/page.tsx` creates `/about` route
- Default exports for page components: `export default function PageName()`

### Component Structure

- **shadcn/ui components**: `src/components/ui/` (auto-generated via CLI)
- **Custom components**: `src/components/` organized by feature
- **Layout components**: Combine shadcn/ui with MUI as needed

### Styling Architecture

- **Global styles**: `src/app/globals.css` with CSS custom properties
- **Theme system**: CSS variables for light/dark mode in `:root` and `@media (prefers-color-scheme: dark)`
- **Font loading**: Geist fonts loaded in layout with CSS variables (`--font-geist-sans`, `--font-geist-mono`)
- **shadcn/ui theming**: CSS variables for component styling (`--primary`, `--secondary`, etc.)

### Component Patterns

- **shadcn/ui components**: Primary choice for UI components (buttons, forms, dialogs, etc.)
- **MUI components**: For complex components not available in shadcn/ui
- **Hybrid approach**: Use shadcn/ui styling with MUI functionality when needed
- **TailwindCSS utilities**: For layout, spacing, and custom styling
- CSS custom properties for theming: `--background`, `--foreground`, `--color-*`

## Development Guidelines

### Component Creation Priority

1. **First choice**: Use shadcn/ui components for standard UI elements
2. **Second choice**: Extend shadcn/ui components with custom variants
3. **Third choice**: Use MUI components for complex functionality
4. **Custom components**: Only when neither shadcn/ui nor MUI provides the needed functionality

### Component Development

- Use functional components with TypeScript
- Import shadcn/ui components: `import { ComponentName } from '@/components/ui/component-name'`
- Import MUI components: `import { ComponentName } from '@mui/material'`
- Apply Tailwind classes alongside component styling
- Use `@/*` path alias for internal imports

### Styling Best Practices

- Combine shadcn/ui components with Tailwind utilities for layout
- Use shadcn/ui's variant system for component customization
- Reference CSS custom properties for consistent theming
- Use `cn()` utility function for conditional classes
- Dark mode handled automatically via CSS media queries and shadcn/ui theme variables

### shadcn/ui Best Practices

For comprehensive shadcn/ui implementation guidelines, patterns, and best practices, refer to:
**`.github/instructions/shadcn-ui-best-practices-instructions.md`**

This file contains detailed instructions for:

- Component installation and setup
- Form implementation with react-hook-form and Zod
- Data table patterns with TanStack Table
- Chart implementation with Recharts
- Layout patterns and sidebar components
- State management patterns
- Error handling and accessibility guidelines
- Performance optimization techniques
- Testing strategies

### Configuration Files

- **TypeScript**: Path mapping configured for `@/*` alias
- **ESLint**: Uses Next.js recommended config with TypeScript support
- **PostCSS**: TailwindCSS v4 plugin only
- **shadcn/ui**: Configured with CSS variables and Tailwind integration
- **Next.js**: Minimal config, relies on defaults for App Router

## Key Integration Points

- shadcn/ui components use CSS variables defined in `globals.css`
- MUI cache provider must wrap app content in `layout.tsx`
- Font variables applied via className in body element
- TailwindCSS imported as single `@import "tailwindcss"` statement
- Theme tokens defined inline in CSS using `@theme inline` directive
- shadcn/ui CLI configured for `@/components/ui` component
