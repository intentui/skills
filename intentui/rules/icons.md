# Icons Rules

Intent UI uses Heroicons (`@heroicons/react`). These icons already include `data-slot="icon"` by default.

## NEVER add `data-slot="icon"` to Heroicons

Heroicons already have `data-slot="icon"` built in. Adding it manually is redundant.

```tsx
// ✅ Correct
import { TrashIcon } from "@heroicons/react/20/solid"
<MenuItem>
  <TrashIcon />
  Delete
</MenuItem>

// ❌ Wrong — data-slot="icon" is already included by Heroicons
<MenuItem>
  <TrashIcon data-slot="icon" />
  Delete
</MenuItem>
```

## NEVER set icon size inside Intent UI components

Components in `src/components/ui/` already define icon sizes via CSS selectors like `*:data-[slot=icon]:size-4`. Do NOT override these with explicit size classes.

### Complete list of components that control icon sizing

All of these components handle icon sizing automatically via `data-[slot=icon]` CSS selectors. Do NOT add `size-*`, `w-*`, `h-*`, or similar size classes to icons inside them:

- **Badge** — `*:data-[slot=icon]:size-3`
- **Breadcrumbs** — `*:data-[slot=icon]:size-5 sm:*:data-[slot=icon]:size-4`
- **Button** — sizes vary by button size variant (xs: `size-3.5/3`, sm: `size-4.5/4`, md: `size-5/4`, lg: `size-5/4.5`, sq-*: similar)
- **ButtonGroup** — `size-5 sm:size-4`
- **Calendar** — `**:data-[slot=icon]:text-fg`
- **Chart** — `*:data-[slot=icon]:size-2.5`
- **ChoiceBox** — `**:data-[slot=icon]:w-5`
- **DisclosureGroup** — `size-5 sm:size-4`
- **DropdownItem** — `size-5 sm:size-4` (also handles positioning and color)
- **GridList items** — `**:data-[slot=icon]:size-5 sm:size-4`
- **Input** (prefix/suffix) — `*:data-[slot=icon]:size-5 sm:*:data-[slot=icon]:size-4`
- **ListBox items** — inherits from DropdownItem styles
- **MenuItem / MenuContent** — inherits from DropdownItem styles (`size-5 sm:size-4`)
- **Navbar items** — `*:data-[slot=icon]:size-5 md:*:data-[slot=icon]:size-4`
- **Note** — icon positioning handled via grid layout
- **SelectTrigger** — `*:data-[slot=icon]:size-5 sm:*:data-[slot=icon]:size-4`
- **SelectItem** — same as DropdownItem
- **Sidebar items** — `size-5 sm:size-4` (SidebarItem, SidebarLink, SidebarNav, SidebarSection header)
- **Slider** — `*:data-[slot=icon]:size-5`
- **Table** (column header) — `*:data-[slot=icon]:size-3.5`
- **Tabs** — `*:data-[slot=icon]:size-4`
- **TagGroup** — `*:data-[slot=icon]:size-3`
- **Toggle** — sizes vary by toggle size variant (same pattern as Button)
- **ToggleGroup** — sizes vary by size variant (same pattern as Button)
- **Tree items** — `**:data-[slot=icon]:size-5 sm:size-4`

```tsx
// ✅ Correct — let the component handle icon sizing
<MenuItem>
  <TrashIcon />
  <MenuLabel>Delete</MenuLabel>
</MenuItem>

<Button>
  <PlusIcon />
  Add item
</Button>

<SelectItem>
  <GlobeIcon />
  <SelectLabel>English</SelectLabel>
</SelectItem>

<Badge>
  <StarIcon />
  Featured
</Badge>

<Tabs.Tab>
  <HomeIcon />
  Home
</Tabs.Tab>

// ❌ Wrong — do NOT add size classes inside these components
<MenuItem>
  <TrashIcon className="size-4" />
  <MenuLabel>Delete</MenuLabel>
</MenuItem>

<Button>
  <PlusIcon className="h-5 w-5" />
  Add item
</Button>

<Badge>
  <StarIcon className="size-3" />
  Featured
</Badge>

<SelectItem>
  <GlobeIcon className="size-4" />
  <SelectLabel>English</SelectLabel>
</SelectItem>
```

## When you CAN set icon size

Only set icon size when using icons **outside** of Intent UI components, in custom layouts:

```tsx
// ✅ OK — icon is in a custom div, not inside an Intent UI component
<div className="flex items-center gap-2">
  <CheckIcon className="size-4 text-success" />
  <span>Completed</span>
</div>
```

## Heroicons import sizes

Use the appropriate Heroicons size variant:

- `@heroicons/react/24/outline` — 24px outline icons (for larger UI elements)
- `@heroicons/react/24/solid` — 24px solid icons
- `@heroicons/react/20/solid` — 20px solid icons (most common inside components)
- `@heroicons/react/16/solid` — 16px solid icons (for compact/small elements)
