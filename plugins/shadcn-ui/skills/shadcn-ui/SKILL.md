---
name: shadcn-ui
description: Adds and configures shadcn/ui components. Use when building UI with React/Next.js, adding or updating shadcn components, working with Tailwind CSS styling, or using Radix UI primitives.
---

# shadcn/ui Skill

shadcn/ui is a collection of beautifully-designed, accessible components and a code distribution platform built with TypeScript, Tailwind CSS, and Radix UI primitives.

## Reference

- Latest Components index: <https://ui.shadcn.com/llms.txt>
- Latest Change log: <https://ui.shadcn.com/docs/changelog>

## Important Changes

**Current practices:**

- OKLCH colours (not HSL)
- `data-slot` attributes on primitives
- Forms: `<Field />` + TanStack Form + Zod

**Deprecated:**

- Toast → use **Sonner**
- `<Form />` → use `<Field />` pattern

## New Utility Components (Oct 2025)

Framework-agnostic primitives (work with Radix, Base UI, React Aria):

| Component | Purpose |
| :-------- | :------ |
| `<Spinner />` | Loading state indicator |
| `<Kbd />` | Keyboard key display (with KbdGroup for combinations) |
| `<ButtonGroup />` | Related buttons, split button support |
| `<InputGroup />` | Icons, buttons, labels around inputs |
| `<Item />` | Flex container for lists/cards (with ItemMedia, ItemContent, ItemTitle) |
| `<Empty />` | Empty state component (with EmptyMedia, EmptyTitle, EmptyDescription) |

## CLI Commands

Commands for an already-initialised project.

```bash
npx shadcn@latest --help                  # CLI help
npx shadcn@latest list @shadcn            # List all available components
npx shadcn@latest search @shadcn -q "nav" # Search components by query
npx shadcn@latest view button card        # Preview code before installing
npx shadcn@latest add <component>         # Add component to project
npx shadcn@latest add button --overwrite  # Overwrite existing component
npx shadcn@latest add @v0/<block>         # Add from v0.dev registry
npx shadcn@latest diff                    # Check for upstream registry updates
```

## Best Practices

### Styling

Apply Tailwind mobile-first and utility-first design.

- Utility classes for layout (margin, padding, flex, grid, etc)
- **Variants** for visual styling (colours, borders, shadows)
- Never hardcode colours — use semantic tokens (`bg-primary`, not `bg-blue-600`)

**Good:** `<Button variant="secondary" className="w-full gap-2">`
**Avoid:** `<Button className="bg-blue-600 text-white">`

### Extending with Variants

Add variants directly via CVA with semantic tokens:

```tsx
const buttonVariants = cva("inline-flex items-center justify-center ...", {
  variants: {
    variant: {
      default: "bg-primary text-primary-foreground hover:bg-primary/90",
      soft: "bg-card text-card-foreground hover:opacity-90", // Custom
    },
  },
});
```

**Key:** Variants belong in component files via CVA, not as className logic at call sites.

### Composition Pattern

Use compound components (Radix style). Compose primitives, don't wrap them:

```tsx
<DropdownMenu>
  <DropdownMenuTrigger>Open</DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuItem>Profile</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

### Pure UI Components

UI components handle rendering only. Keep API calls and business logic in hooks or server actions.

### Server Components

Interactive shadcn/ui components include `"use client"` automatically. Push client boundaries down — wrap only the interactive parts.

**Interleave server content inside client wrappers:**

```tsx
// page.tsx (Server Component)
export default async function Page() {
  const user = await getUser();
  return (
    <Sheet>
      <SheetContent>
        <UserProfile user={user} /> {/* Server-rendered */}
      </SheetContent>
    </Sheet>
  );
}
```

Children passed to client components can still be server-rendered.

**Config:** `"rsc": true` in `components.json` (default).

### Forms

Use `<Field />` component + TanStack Form + Zod for validation

## Component Architecture

```text
components/
├── ui/                  # Design system primitives (from shadcn)
│   ├── button.tsx       # Modify for: variants, default styles, global behaviour
│   └── sheet.tsx
├── auth/                # Domain components (compose ui/ primitives)
│   └── login-form.tsx
└── nav/
    └── mobile-menu.tsx
```

- **Use composition** over customisation
- **Modify `ui/` directly** for global styling, new variants, behaviour changes
- **Create domain components** outside `ui/` for business logic and composition

## Theming

**Principles:**

- Centralised tokens in `app/globals.css` — no hard-coded colours
- Tailwind utilities only — no custom CSS
- Light/dark mode implemented with `next-themes`
