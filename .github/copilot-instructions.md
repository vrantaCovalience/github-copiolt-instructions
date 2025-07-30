# Copilot Instructions

## Project Architecture

This Next.js 15 project uses **App Router** with a **hybrid UI approach**: Material-UI for interactive components + Tailwind CSS v4 for layout/styling.

### Critical Setup Pattern

```tsx
// src/app/layout.tsx - Current implementation missing ThemeProvider
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";
// ⚠️ Add ThemeProvider + enableCssLayer for Tailwind integration
```

### Component Import Strategy (Performance Critical)

```tsx
// ✅ Required pattern - 6x faster dev builds
import Button from "@mui/material/Button";
import { Button } from "@mui/material"; // ❌ Causes slow rebuilds
```

### MUI best practices:

Please refer to `.github/instructions/material-ui-best-practices-instructions.md` for detailed best practices on using Material-UI in this project.

### Hybrid UI Decision Tree

- **MUI**: Interactive components (forms, navigation, feedback) - see `src/app/about/page.tsx`
- **Tailwind**: Layout, spacing, custom styling - see `src/app/page.tsx`
- **Integration**: Current layout missing `enableCssLayer: true` for CSS specificity

## Development Workflow

### Build Commands

```bash
npm run dev --turbopack  # Turbopack enabled by default
npm run build           # Production build
npm run lint           # ESLint flat config
```

### Key Files & Patterns

```
src/app/layout.tsx       # MUI AppRouterCacheProvider setup
src/app/globals.css      # Tailwind v4 inline theme config
src/app/page.tsx         # Pure Tailwind styling
src/app/about/page.tsx   # Hybrid MUI + Tailwind
```

### Styling Architecture

- **Tailwind v4**: Inline `@theme` config in `globals.css`
- **CSS Custom Properties**: `--background`, `--foreground` for dark mode
- **Font Loading**: Geist fonts via Next.js optimization
- **Responsive**: Mobile-first `sm:` breakpoints

## Critical Patterns

### MUI-Tailwind Integration

```tsx
// Current gap: Missing CSS layers setup
<AppRouterCacheProvider options={{ enableCssLayer: true }}>
```

```css
/* globals.css - Add for proper specificity */
@layer mui, tailwind;
```

### Layout Patterns (from existing code)

```tsx
// Full-height grid pattern from src/app/page.tsx
className = "grid grid-rows-[20px_1fr_20px] min-h-screen";

// Component arrangement
className = "flex flex-col gap-4 items-center sm:items-start";
```

### Component State Handling

- Server Components by default (pages)
- `"use client"` only when interactivity needed
- MUI components require client-side for state/events

## Dependencies & Integration Points

- **Next.js 15** + App Router + Turbopack
- **React 19** latest features
- **MUI 7** with Next.js integration package
- **Tailwind 4** PostCSS plugin
- **TypeScript** strict mode, path aliases `@/*`

## Implementation Guidelines

1. **Import MUI individually** for performance
2. **Use Tailwind for layout**, MUI for interaction
3. **Enable CSS layers** for hybrid styling
4. **Follow App Router** file-based routing
5. **Optimize fonts** with Next.js variables
6. **Maintain responsive** mobile-first design
