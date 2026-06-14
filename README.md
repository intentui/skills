# Intent UI Skill

A skill that enforces [Intent UI](https://intentui.com) component library conventions when writing or reviewing React/TSX code.

## What it does

The **intentui** skill ensures your code follows Intent UI patterns:

- Uses components from `src/components/ui/` instead of raw HTML elements
- Uses semantic color tokens (`text-muted-fg`, `bg-primary`) instead of raw Tailwind colors (`text-blue-500`, `bg-red-600`)
- Uses proper form structure — `Fieldset`, `Legend`, `FieldError`, `Description`
- Installs missing components from the registry via the shadcn CLI

## Structure

```
intentui/
├── SKILL.md                 # Main skill definition
└── rules/
    ├── styling.md           # Semantic colors, cx/twMerge/twJoin, size-* shorthand, intent prop
    ├── forms.md             # Fieldset, Legend, TextField, Select, Checkbox, Radio, FieldError patterns
    ├── components.md        # HTML → Intent UI component mapping, Text/TextLink/Strong/Code
    ├── overlays.md          # Modal, Sheet, Drawer, Popover, Menu, and Tooltip composition
    ├── collections.md       # Table, ListBox, GridList, Tree, TagGroup: items + render fn, selection props
    └── cli.md               # Search & install missing components from the registry
```

## Rules overview

### Styling (`rules/styling.md`)
- Never use raw Tailwind colors (e.g. `text-blue-500`) — use semantic tokens (`text-primary`)
- Use `size-*` instead of `w-* h-*` when width and height are equal
- Use `cx` for react-aria components, `twMerge`/`twJoin` for regular HTML elements
- Use `intent` prop for variants, not `variant` or `color`
- Use `tv` from `tailwind-variants` for component variant styles

### Forms (`rules/forms.md`)
- Use `Fieldset` + `Legend` to group fields, not `<div>` + `<h1>`
- Use `Text` for descriptions, not `<p>`
- When `isRequired` is set, always include `<FieldError />`
- Add `data-slot="control"` to custom divs inside Fieldset for proper spacing
- `Checkbox`, `Radio`, and `Switch` hold only the label — all props go on `CheckboxField`/`RadioField`/`SwitchField`
- Use `value`/`onChange` for controlled Select/ComboBox — `selectedKey`/`onSelectionChange` are deprecated
- Never use `<Input type="number">` or `<Input type="date">` — use `NumberField` and `DatePicker`

### Components (`rules/components.md`)
- Use Intent UI components instead of raw HTML (`Button` not `<button>`, `Link` not `<a>`)
- Use `TextLink` for links inside `Text`, `Strong` for bold, `Code` for inline code

### Overlays (`rules/overlays.md`)
- Compose `Modal`/`Sheet`/`Drawer`/`Popover`/`Menu`/`Tooltip` with their matching subcomponents — never raw `<div>` or `<dialog>`
- Trigger state is managed by the root wrapper — do not wire up your own `isOpen`
- `TooltipTrigger`/`MenuTrigger`/`DrawerTrigger` are the button — style with `buttonStyles(...)`, never nest a `<Button>` inside
- Set `role="alertdialog"` on `ModalContent` for destructive confirmations

### Collections (`rules/collections.md`)
- Never `.map()` inside a collection — use `items={...}` + a render function `(item) => <Item id={item.id} />`
- Collections use `selectedKeys`/`onSelectionChange` (a `Set`) — not the `value`/`onChange` of `Select`/`ComboBox`
- Selection checkboxes render automatically in `selectionMode="multiple"` — do not add your own
- Every item needs an `id`; `Table`, `ListBox`, `GridList`, and `Tree` require `aria-label` or a visible label

### CLI (`rules/cli.md`)
- If a component doesn't exist in `src/components/ui/`, install it from the registry
- Detects package runner from lockfile (`bun.lock` → `bunx --bun`, etc.)
- Reads registry name from `components.json`, defaults to `@intentui` if not configured
- Never modifies `components.json`
