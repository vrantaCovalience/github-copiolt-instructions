---
applyTo: "**"
---

Follow these best practices when using Material-UI in this project:

### Essential Setup

```tsx
// app/layout.tsx - Required for Next.js App Router
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";
import { ThemeProvider } from "@mui/material/styles";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <AppRouterCacheProvider options={{ enableCssLayer: true }}>
          <ThemeProvider theme={theme}>{children}</ThemeProvider>
        </AppRouterCacheProvider>
      </body>
    </html>
  );
}
```

### Import Strategy (Critical for Performance)

```tsx
// ✅ Fast development builds - Always use this pattern
import Button from "@mui/material/Button";
import TextField from "@mui/material/TextField";
import { Box, Typography } from "@mui/material"; // Exception: layout components
import DeleteIcon from "@mui/icons-material/Delete";

// ❌ Slow development builds - up to 6x slower
import { Button, TextField } from "@mui/material";
import { Delete } from "@mui/icons-material";
```

## Theme Configuration

### Next.js Font Integration

```tsx
// theme.ts
"use client";
import { createTheme } from "@mui/material/styles";

const theme = createTheme({
  cssVariables: true, // Prevents SSR flickering
  typography: {
    fontFamily: "var(--font-geist-sans)", // Use Next.js font variables
  },
  palette: {
    primary: {
      main: "#1976d2",
    },
  },
});

export default theme;
```

### CSS Theme Variables

```tsx
// Enables CSS custom properties for better performance
const theme = createTheme({
  cssVariables: true, // Generates --mui-palette-primary-main, etc.
  components: {
    // Optional: merge className and style props
    mergeClassNameAndStyle: true,
  },
});
```

## Component Patterns

### Server Components vs Client Components

```tsx
// ✅ Server Component (default in App Router)
export default function Page() {
  return (
    <Box sx={{ p: 2 }}>
      <Typography variant="h4">Server-rendered content</Typography>
    </Box>
  );
}

// ✅ Client Component (when interactivity needed)
("use client");
export default function InteractiveForm() {
  const [value, setValue] = useState("");

  return (
    <TextField
      value={value}
      onChange={(e) => setValue(e.target.value)}
      variant="outlined"
    />
  );
}
```

### Styling Approaches (Priority Order)

#### 1. sx Prop (One-off styles)

```tsx
<Button
  sx={{
    backgroundColor: "primary.main",
    "&:hover": { backgroundColor: "primary.dark" },
    borderRadius: 2,
  }}
>
  Styled Button
</Button>
```

#### 2. Styled Components (Reusable)

```tsx
import { styled } from "@mui/material/styles";

const StyledButton = styled(Button)(({ theme }) => ({
  backgroundColor: theme.palette.primary.main,
  "&:hover": {
    backgroundColor: theme.palette.primary.dark,
  },
}));
```

#### 3. Theme Overrides (Global)

```tsx
const theme = createTheme({
  components: {
    MuiButton: {
      defaultProps: {
        variant: "contained",
      },
      styleOverrides: {
        root: {
          textTransform: "none", // Disable uppercase
        },
      },
    },
  },
});
```

## Advanced Patterns

### Custom Component Integration

```tsx
// Creating theme-aware custom components
import { useThemeProps } from "@mui/material/styles";

interface CustomCardProps {
  elevation?: number;
  children: React.ReactNode;
}

function CustomCard(inProps: CustomCardProps) {
  const props = useThemeProps({
    props: inProps,
    name: "MuiCustomCard",
  });

  const { elevation = 2, children, ...other } = props;

  return (
    <Paper elevation={elevation} {...other}>
      {children}
    </Paper>
  );
}
```

### Responsive Design with MUI

```tsx
// Using theme breakpoints
<Box
  sx={{
    display: "flex",
    flexDirection: { xs: "column", md: "row" },
    gap: { xs: 1, md: 2 },
    p: { xs: 2, md: 3 },
  }}
>
  <Typography variant={{ xs: "h6", md: "h4" }}>
    Responsive Typography
  </Typography>
</Box>;

// Custom breakpoint usage
const theme = createTheme({
  breakpoints: {
    values: {
      xs: 0,
      sm: 600,
      md: 900,
      lg: 1200,
      xl: 1536,
    },
  },
});
```

## Integration with Tailwind CSS

### Proper CSS Layer Setup

```tsx
// layout.tsx
<AppRouterCacheProvider options={{ enableCssLayer: true }}>
```

```css
/* globals.css */
@import "tailwindcss";
@layer mui, tailwind;
```

### Hybrid Usage Patterns

```tsx
// ✅ MUI for interactive components, Tailwind for layout
<div className="grid grid-cols-1 md:grid-cols-2 gap-4 p-4">
  <Card>
    {" "}
    {/* MUI Card for complex interactions */}
    <CardContent>
      <TextField label="Email" variant="outlined" fullWidth />
      <Button variant="contained" sx={{ mt: 2 }}>
        Submit
      </Button>
    </CardContent>
  </Card>
</div>
```

## Performance Optimization

### Bundle Size Best Practices

```json
// .eslintrc.json - Enforce proper imports
{
  "rules": {
    "no-restricted-imports": [
      "error",
      {
        "patterns": [{ "regex": "^@mui/[^/]+$" }]
      }
    ]
  }
}
```

```json
// .vscode/settings.json - Prevent auto-import from barrel files
{
  "typescript.preferences.autoImportSpecifierExcludeRegexes": ["^@mui/[^/]+$"]
}
```

### Tree Shaking Verification

```tsx
// Check webpack bundle analyzer for:
// - Individual component imports
// - No full @mui/material imports
// - Separate icon chunks
```

## Common Patterns

### Form Components

```tsx
"use client";
import { useState } from "react";
import TextField from "@mui/material/TextField";
import Button from "@mui/material/Button";
import Box from "@mui/material/Box";
import Alert from "@mui/material/Alert";

export default function ContactForm() {
  const [values, setValues] = useState({ email: "", message: "" });
  const [error, setError] = useState("");

  return (
    <Box
      component="form"
      sx={{ display: "flex", flexDirection: "column", gap: 2 }}
    >
      {error && <Alert severity="error">{error}</Alert>}

      <TextField
        label="Email"
        type="email"
        value={values.email}
        onChange={(e) =>
          setValues((prev) => ({ ...prev, email: e.target.value }))
        }
        required
        fullWidth
      />

      <TextField
        label="Message"
        multiline
        rows={4}
        value={values.message}
        onChange={(e) =>
          setValues((prev) => ({ ...prev, message: e.target.value }))
        }
        required
        fullWidth
      />

      <Button variant="contained" type="submit">
        Send Message
      </Button>
    </Box>
  );
}
```

### Navigation Components

```tsx
import AppBar from "@mui/material/AppBar";
import Toolbar from "@mui/material/Toolbar";
import Typography from "@mui/material/Typography";
import Button from "@mui/material/Button";
import IconButton from "@mui/material/IconButton";
import MenuIcon from "@mui/icons-material/Menu";

export default function Navigation() {
  return (
    <AppBar position="static">
      <Toolbar>
        <IconButton edge="start" color="inherit">
          <MenuIcon />
        </IconButton>

        <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
          App Title
        </Typography>

        <Button color="inherit">Login</Button>
      </Toolbar>
    </AppBar>
  );
}
```

## Troubleshooting

### Common SSR Issues

```tsx
// ✅ Prevent hydration mismatches
import NoSsr from "@mui/material/NoSsr";

<NoSsr>
  <ComponentWithClientOnlyLogic />
</NoSsr>;

// ✅ Use InitColorSchemeScript for dark mode
import InitColorSchemeScript from "@mui/material/InitColorSchemeScript";

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <InitColorSchemeScript />
        {children}
      </body>
    </html>
  );
}
```

### TypeScript Integration

```tsx
// Extend theme types
declare module "@mui/material/styles" {
  interface Theme {
    customShadows: {
      light: string;
      medium: string;
      heavy: string;
    };
  }

  interface ThemeOptions {
    customShadows?: {
      light?: string;
      medium?: string;
      heavy?: string;
    };
  }
}
```

## Key Takeaways

1. **Always import individual components** for optimal development performance
2. **Use CSS theme variables** to prevent SSR flickering
3. **Enable CSS layers** when using with Tailwind CSS
4. **Leverage sx prop** for component-specific styling
5. **Use proper cache providers** for Next.js App Router integration
6. **Follow responsive-first design** with MUI breakpoints
7. **Maintain accessibility** with proper ARIA attributes and semantic HTML
