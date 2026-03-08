# Intent UI Skill

A skill that enforces [Intent UI](https://intentui.com) component library conventions when writing or reviewing React/TSX code.

## What it does

The **intentui** skill ensures your code follows Intent UI patterns:

- Uses components from `src/components/ui/` instead of raw HTML elements
- Uses semantic color tokens (`text-muted-fg`, `bg-primary`) instead of raw Tailwind colors (`text-blue-500`, `bg-red-600`)
- Uses Heroicons correctly — no redundant `data-slot="icon"`, no manual icon sizing inside UI components
- Uses proper form structure — `Fieldset`, `Legend`, `FieldError`, `Description`
- Installs missing components from the registry via the shadcn CLI

## Structure

```
intentui/
├── SKILL.md              # Main skill definition
└── rules/
    ├── styling.md        # Semantic colors, cx/twMerge, size-* shorthand, intent prop
    ├── icons.md          # Heroicons rules, 25 components that control icon sizing
    ├── forms.md          # Fieldset, Legend, TextField, FieldError, Description patterns
    ├── components.md     # HTML → Intent UI component mapping, Text/TextLink/Strong/Code
    └── cli.md            # Search & install missing components from the registry
```

## Rules overview

### Styling (`rules/styling.md`)
- Never use raw Tailwind colors (e.g. `text-blue-500`) — use semantic tokens (`text-primary`)
- Use `size-*` instead of `w-* h-*` when width and height are equal
- Use `cx` for react-aria components, `twMerge` for regular HTML elements
- Use `intent` prop for variants, not `variant` or `color`

### Icons (`rules/icons.md`)
- Never add `data-slot="icon"` — Heroicons already include it
- Never set icon size inside UI components — they handle sizing via CSS

### Forms (`rules/forms.md`)
- Use `Fieldset` + `Legend` to group fields, not `<div>` + `<h1>`
- Use `Text` for descriptions, not `<p>`
- When `isRequired` is set, always include `<FieldError />`
- Add `data-slot="control"` to custom divs inside Fieldset for proper spacing

### Components (`rules/components.md`)
- Use Intent UI components instead of raw HTML (`Button` not `<button>`, `Link` not `<a>`)
- Use `TextLink` for links inside `Text`, `Strong` for bold, `Code` for inline code

### CLI (`rules/cli.md`)
- If a component doesn't exist in `src/components/ui/`, install it from the registry
- Detects package runner from lockfile (`bun.lock` → `bunx --bun`, etc.)
- Reads registry name from `components.json`, defaults to `@intentui` if not configured
- Never modifies `components.json`
