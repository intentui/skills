# Styling Rules

## NEVER use raw Tailwind colors

Never use raw Tailwind color utilities. This includes ANY color from: red, orange, amber, yellow, lime, green, emerald, teal, cyan, sky, blue, indigo, violet, purple, fuchsia, pink, rose, slate, gray, zinc, neutral, stone.

### Forbidden patterns

```
❌ text-blue-500, text-red-600, text-gray-400, text-green-500
❌ bg-blue-500, bg-red-100, bg-gray-50, bg-slate-200
❌ border-blue-500, border-gray-300, border-red-200
❌ ring-blue-500, ring-red-500
❌ from-blue-500, to-red-500, via-green-500
❌ divide-gray-200, placeholder-gray-400
❌ shadow-blue-500/50
```

## Use semantic color tokens instead

### Text colors

| For | Use |
|---|---|
| Default text | `text-fg` |
| Muted/secondary text | `text-muted-fg` |
| Primary colored text | `text-primary` |
| Primary subtle text | `text-primary-subtle-fg` |
| Success text | `text-success` or `text-success-subtle-fg` |
| Danger/error text | `text-danger` or `text-danger-subtle-fg` |
| Warning text | `text-warning` or `text-warning-subtle-fg` |
| Info text | `text-info` or `text-info-subtle-fg` |
| Foreground on primary bg | `text-primary-fg` |
| Foreground on secondary bg | `text-secondary-fg` |
| Foreground on overlay bg | `text-overlay-fg` |
| Foreground on accent bg | `text-accent-fg` |
| Foreground on navbar bg | `text-navbar-fg` |
| Foreground on sidebar bg | `text-sidebar-fg` |

### Background colors

| For | Use |
|---|---|
| Page background | `bg-bg` |
| Primary background | `bg-primary` |
| Secondary background | `bg-secondary` |
| Muted background | `bg-muted` |
| Accent background | `bg-accent` |
| Overlay/modal background | `bg-overlay` |
| Success background | `bg-success` or `bg-success-subtle` |
| Danger background | `bg-danger` or `bg-danger-subtle` |
| Warning background | `bg-warning` or `bg-warning-subtle` |
| Info background | `bg-info` or `bg-info-subtle` |

### Border & ring colors

| For | Use |
|---|---|
| Default border | `border-border` |
| Input border | `border-input` |
| Focus ring | `ring-ring` |

### All available semantic tokens

`primary`, `primary-fg`, `primary-subtle`, `primary-subtle-fg`, `secondary`, `secondary-fg`, `accent`, `accent-fg`, `success`, `success-fg`, `success-subtle`, `success-subtle-fg`, `danger`, `danger-fg`, `danger-subtle`, `danger-subtle-fg`, `warning`, `warning-fg`, `warning-subtle`, `warning-subtle-fg`, `info`, `info-fg`, `info-subtle`, `info-subtle-fg`, `muted`, `muted-fg`, `overlay`, `overlay-fg`, `navbar`, `navbar-fg`, `sidebar`, `sidebar-fg`, `sidebar-primary`, `sidebar-primary-fg`, `sidebar-accent`, `sidebar-accent-fg`, `sidebar-border`, `sidebar-ring`, `bg`, `fg`, `border`, `input`, `ring`, `chart-1` through `chart-5`

## Tailwind shorthand utilities

When `width` and `height` have the same value, always use the `size-*` shorthand instead of writing both `w-*` and `h-*`.

```tsx
// ✅ Correct
<div className="size-5" />
<Avatar className="size-10" />
<span className="size-8 rounded-full" />

// ❌ Wrong — use size-* when width and height are equal
<div className="w-5 h-5" />
<Avatar className="w-10 h-10" />
<span className="h-8 w-8 rounded-full" />
```

This also applies to arbitrary values:

```tsx
// ✅ size-[32px]
// ❌ w-[32px] h-[32px]
```

## className utility

Use the right utility depending on whether the component is a react-aria component or a regular HTML element:

- **`cx`** from `@/lib/primitive` — ONLY for react-aria-components that need `composeRenderProps` (e.g. `Button`, `TextField`, `Select`, etc. from `react-aria-components`)
- **`twMerge`** from `tailwind-merge` — for regular HTML elements (`div`, `p`, `span`, `strong`, `code`, etc.)
- **`twJoin`** from `tailwind-merge` — when you just need to join classes without conflict resolution

```tsx
// ✅ cx — for react-aria components
import { cx } from "@/lib/primitive"
import { Button as ButtonPrimitive } from "react-aria-components"

<ButtonPrimitive className={cx("base-classes", className)} />

// ✅ twMerge — for regular HTML elements
import { twMerge } from "tailwind-merge"

<p className={twMerge("text-base/6 text-muted-fg sm:text-sm/6", className)} />
<strong className={twMerge("font-medium", className)} />
<code className={twMerge("rounded-sm border bg-muted px-0.5", className)} />

// ✅ twJoin — when no class conflicts, just joining
import { twJoin } from "tailwind-merge"

<div className={twJoin("flex items-center", isActive && "bg-accent")} />

// ❌ Wrong — never use these
import { cn } from "@/lib/utils"
import clsx from "clsx"
import classNames from "classnames"
```

## Variant naming

Use `intent` for color variant props, not `variant` or `color`.

```tsx
// ✅ Correct
<Button intent="primary">Save</Button>
<Badge intent="danger">Error</Badge>
<Note intent="warning">Careful</Note>

// ❌ Wrong
<Button variant="primary">Save</Button>
<Badge color="danger">Error</Badge>
```

## Component styling with tailwind-variants

Use `tv` from `tailwind-variants` for component variant styles:

```tsx
import { tv } from "tailwind-variants"

const styles = tv({
  base: "...",
  variants: {
    intent: {
      primary: "...",
      danger: "...",
    },
  },
  defaultVariants: {
    intent: "primary",
  },
})
```
