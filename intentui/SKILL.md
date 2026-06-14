---
name: intentui
description: Enforces Intent UI component library conventions. Use when writing React components, reviewing UI code, or building pages/features that involve UI elements. Ensures semantic color tokens, correct form and overlay patterns, the `intent` variant prop, and that existing components from src/components/ui/ are used.
allowed-tools: Read, Grep, Glob
---

# Intent UI — Component & Style Enforcer

You are a code quality enforcer for the Intent UI component library. When writing or reviewing React/TSX code, you MUST follow all rules defined in the `rules/` directory.

## Rules

Load and enforce all rules from the following files:

- [Styling Rules](${CLAUDE_SKILL_DIR}/rules/styling.md) — Semantic color tokens, className utilities, variant conventions
- [Forms Rules](${CLAUDE_SKILL_DIR}/rules/forms.md) — Form component patterns, TextField, Select, Checkbox, etc.
- [Components Rules](${CLAUDE_SKILL_DIR}/rules/components.md) — Always use Intent UI components instead of raw HTML elements
- [Overlays Rules](${CLAUDE_SKILL_DIR}/rules/overlays.md) — Modal, Sheet, Drawer, Popover, Menu, and Tooltip composition
- [Collections Rules](${CLAUDE_SKILL_DIR}/rules/collections.md) — Table, ListBox, GridList, Tree, TagGroup: items + render function, selection props
- [CLI Rules](${CLAUDE_SKILL_DIR}/rules/cli.md) — How to search and install missing components from the registry

## When reviewing or writing code

1. **Scan for raw HTML elements** — replace with Intent UI components (see components rule)
2. **Scan for raw Tailwind colors** — replace with semantic tokens (see styling rule)
3. **Check form patterns** — use Intent UI form components correctly (see forms rule)
4. **Check overlay composition** — Modal/Sheet/Drawer/Popover/Menu must be paired with their subcomponents (see overlays rule)
5. **Check collection patterns** — Table/ListBox/GridList/Tree/TagGroup must use `items` + a render function (never `.map()`), and `selectedKeys`/`onSelectionChange` for selection (see collections rule)
6. **Check imports** — ensure components come from `@/components/ui/`
7. **Check className utility** — use `cx` from `@/lib/primitive` for react-aria components, `twMerge`/`twJoin` for plain HTML; never `cn` or `clsx` (see styling rule)
8. **Check the `intent` prop** — color variants use `intent`, never `variant` or `color`
9. **Missing components** — if a needed component doesn't exist in `src/components/ui/`, follow the CLI rule to search and install it from the registry

If you find violations, fix them and explain what was changed and why.
