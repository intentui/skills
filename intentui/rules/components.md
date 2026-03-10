# Components Rules

Always use Intent UI components from `src/components/ui/` instead of raw HTML elements. Import from `@/components/ui/<component-name>`.

## Available components

area-chart, avatar, badge, bar-chart, bar-list, breadcrumbs, button, button-group, calendar, card, carousel, chart, checkbox, choice-box, color-area, color-field, color-picker, color-slider, color-swatch, color-swatch-picker, color-thumb, color-wheel, combo-box, command-menu, container, context-menu, date-field, date-picker, date-range-picker, description-list, dialog, disclosure-group, drawer, drop-zone, dropdown, field, file-trigger, grid-list, heading, input, input-otp, keyboard, leaderboard, line-chart, link, list-box, loader, menu, meter, modal, multiple-select, native-select, navbar, note, number-field, pagination, pie-chart, popover, progress-bar, progress-circle, radio, range-calendar, scroll-area, search-field, select, separator, sheet, show-more, sidebar, skeleton, slider, snippet, switch, table, tabs, tag-field, tag-group, text, text-field, textarea, time-field, toast, toggle, toggle-group, toolbar, tooltip, tracker, tree

## HTML element → Intent UI component mapping

| Instead of | Use | Import from |
|---|---|---|
| `<button>` | `<Button>` | `@/components/ui/button` |
| `<input>` | `<Input>` inside `<TextField>` for text-like values only | `@/components/ui/input`, `@/components/ui/text-field` |
| `<input type="number">` | `<NumberInput>` inside `<NumberField>` | `@/components/ui/number-field` |
| `<input type="date">` | `<DatePicker>` | `@/components/ui/date-picker` |
| `<select>` | `<Select>` | `@/components/ui/select` |
| `<textarea>` | `<Textarea>` | `@/components/ui/textarea` |
| `<table>` | `<Table>` | `@/components/ui/table` |
| `<a>` (standalone) | `<Link>` | `@/components/ui/link` |
| `<dialog>` | `<Modal>` | `@/components/ui/modal` |
| `<h1>`–`<h6>` | `<Heading>` | `@/components/ui/heading` |
| `<p>`, `<span>` for styled text | `<Text>` | `@/components/ui/text` |
| `<a>` inside `<Text>` | `<TextLink>` | `@/components/ui/text` |
| `<strong>` inside `<Text>` | `<Strong>` | `@/components/ui/text` |
| `<code>` inline | `<Code>` | `@/components/ui/text` |
| `<label>` in forms | `<Label>` | `@/components/ui/field` |
| `<hr>` | `<Separator>` | `@/components/ui/separator` |
| `<nav>` breadcrumbs | `<Breadcrumbs>` | `@/components/ui/breadcrumbs` |
| `<img>` for avatars | `<Avatar>` | `@/components/ui/avatar` |
| Custom spinner | `<Loader>` | `@/components/ui/loader` |
| Custom checkbox | `<Checkbox>` | `@/components/ui/checkbox` |
| Custom radio | `<Radio>` | `@/components/ui/radio` |
| Custom switch/toggle | `<Switch>` | `@/components/ui/switch` |
| Custom tooltip | `<Tooltip>` | `@/components/ui/tooltip` |
| Custom tabs | `<Tabs>` | `@/components/ui/tabs` |
| Custom dropdown | `<Dropdown>` | `@/components/ui/dropdown` |
| Custom popover | `<Popover>` | `@/components/ui/popover` |
| Custom modal/dialog | `<Modal>` / `<Dialog>` | `@/components/ui/modal` / `@/components/ui/dialog` |
| Custom card | `<Card>` | `@/components/ui/card` |
| Custom badge/tag | `<Badge>` | `@/components/ui/badge` |
| Custom skeleton | `<Skeleton>` | `@/components/ui/skeleton` |
| Custom progress | `<ProgressBar>` | `@/components/ui/progress-bar` |

## Text, TextLink, Strong, and Code

The `text.tsx` component exports `Text`, `TextLink`, `Strong`, and `Code`. **CRITICAL: When you need a link inside `<Text>`, you MUST use `<TextLink>` — NEVER use `<Link>` or `<a>` inside `<Text>`.** `<Link>` is only for standalone links outside of `<Text>`.

- **Inside `<Text>`** → use `<TextLink>` (from `@/components/ui/text`)
- **Outside `<Text>` (standalone)** → use `<Link>` (from `@/components/ui/link`)

```tsx
import { Text, TextLink, Strong, Code } from "@/components/ui/text"

// ✅ Correct — TextLink inside Text
<Text>
  By signing up, you agree to our <TextLink href="/terms">Terms of Service</TextLink> and{" "}
  <TextLink href="/privacy">Privacy Policy</TextLink>.
</Text>

// ✅ Strong and Code inside Text
<Text>
  Run <Code>npm install</Code> to get started. See <Strong>Getting Started</Strong> for more info.
</Text>

// ❌ Wrong — Link inside Text (MUST use TextLink instead)
<Text>
  By signing up, you agree to our <Link href="/terms">Terms of Service</Link>.
</Text>

// ❌ Wrong — raw <a> inside Text
<Text>
  By signing up, you agree to our <a href="/terms">Terms of Service</a>.
</Text>
```

## Coding conventions

- Use `cx` from `@/lib/primitive` for composing class names
- Use `tailwind-variants` (`tv`) for component variant styles
- Components are built on `react-aria-components` primitives
- Add `"use client"` directive when using React hooks or interactive components
- Use `intent` prop for color variants (not `variant` or `color`)

## When NOT to replace raw HTML

- Structural `<div>`, `<section>`, `<main>`, `<article>`, `<aside>`, `<header>`, `<footer>` are fine as layout wrappers
- `<span>` for inline non-styled text or wrappers is fine
- `<p>` for basic paragraph text without special styling is fine
- `<ul>`, `<ol>`, `<li>` for basic lists are fine (use `<ListBox>` for interactive/selectable lists)
- `<form>` is fine — Intent UI doesn't replace it
- `<img>` for regular images is fine (use `<Avatar>` only for user/profile avatars)
