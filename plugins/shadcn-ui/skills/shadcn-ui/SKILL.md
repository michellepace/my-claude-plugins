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
npx shadcn@latest search @shadcn -q "nav" # Search components by query (-q filters results)
npx shadcn@latest view button card        # Preview code before installing
npx shadcn@latest add <component>         # Add component to project
npx shadcn@latest add button --overwrite  # Overwrite existing component
npx shadcn@latest add @v0/<block>         # Add from v0.dev registry
npx shadcn@latest diff                    # Check for upstream registry updates
```

## Best Practices

1. **Compose Primitives**: Build complex UIs by composing simple components
2. **Use Form Validation**: Use `<Field />` component + TanStack Form + Zod
3. **Tailwind**: Utility-first, mobile-first (use responsive utilities like `sm:`, `md:`)

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
