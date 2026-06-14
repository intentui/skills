# Overlays Rules

Overlay components (Modal, Sheet, Drawer, Popover, Menu, Tooltip) are built on `react-aria-components` and must be composed with their own subcomponents — never with raw `<div>`, `<dialog>`, or hand-rolled state.

All of these are interactive and require the `"use client"` directive in the file that uses them.

## Modal

Use `Modal` (the trigger wrapper) → `ModalContent` → `ModalHeader`/`ModalBody`/`ModalFooter`. The trigger element goes directly inside `Modal`.

```tsx
import {
  Modal,
  ModalContent,
  ModalHeader,
  ModalTitle,
  ModalDescription,
  ModalBody,
  ModalFooter,
  ModalClose,
} from "@/components/ui/modal"
import { Button } from "@/components/ui/button"

// ✅ Correct
<Modal>
  <Button>Open</Button>
  <ModalContent size="md" role="dialog">
    <ModalHeader>
      <ModalTitle>Confirm action</ModalTitle>
      <ModalDescription>This cannot be undone.</ModalDescription>
    </ModalHeader>
    <ModalBody>Body content</ModalBody>
    <ModalFooter>
      <ModalClose>Cancel</ModalClose>
      <Button intent="danger">Delete</Button>
    </ModalFooter>
  </ModalContent>
</Modal>

// ❌ Wrong — raw dialog / manual open state
<dialog open={isOpen}>...</dialog>
```

- `ModalContent` props: `size` (`"2xs" | "xs" | "sm" | "md" | "lg" | "xl" | "2xl" | "3xl" | "4xl" | "5xl" | "fullscreen"`, default `"md"`), `role` (`"dialog" | "alertdialog"`, default `"dialog"`), `closeButton` (default `true`), `isDismissable`.
- Set `role="alertdialog"` for destructive confirmations — it becomes non-dismissable by default.
- `ModalHeader`, `ModalTitle`, `ModalDescription`, `ModalBody`, `ModalFooter`, `ModalClose`, and `ModalTrigger` are aliases of the `Dialog*` subcomponents — never substitute raw elements.

## Sheet

Same composition as Modal, but slides in from a side. Use for side panels / mobile-style sheets.

```tsx
import {
  Sheet,
  SheetContent,
  SheetHeader,
  SheetTitle,
  SheetBody,
  SheetFooter,
  SheetClose,
} from "@/components/ui/sheet"

<Sheet>
  <Button>Open sheet</Button>
  <SheetContent side="right">
    <SheetHeader>
      <SheetTitle>Filters</SheetTitle>
    </SheetHeader>
    <SheetBody>...</SheetBody>
    <SheetFooter>
      <SheetClose>Close</SheetClose>
    </SheetFooter>
  </SheetContent>
</Sheet>
```

- `SheetContent` props: `side` (`"top" | "bottom" | "left" | "right"`, default `"right"`), `isFloat` (default `true`), `closeButton` (default `true`), `role`, `isDismissable`.

## Drawer

Touch-draggable overlay (powered by `motion`), typically swiped from the bottom on mobile.

```tsx
import {
  Drawer,
  DrawerContent,
  DrawerTrigger,
  DrawerHeader,
  DrawerTitle,
  DrawerDescription,
  DrawerBody,
  DrawerFooter,
  DrawerClose,
} from "@/components/ui/drawer"
import { buttonStyles } from "@/components/ui/button"

<Drawer>
  <DrawerTrigger className={buttonStyles({ intent: "outline" })}>Open drawer</DrawerTrigger>
  <DrawerContent side="bottom">
    <DrawerHeader>
      <DrawerTitle>Title</DrawerTitle>
      <DrawerDescription>Description</DrawerDescription>
    </DrawerHeader>
    <DrawerBody>...</DrawerBody>
    <DrawerFooter>
      <DrawerClose>Close</DrawerClose>
    </DrawerFooter>
  </DrawerContent>
</Drawer>
```

- `DrawerContent` props: `side` (`"top" | "bottom" | "left" | "right"`, default `"bottom"`), `isFloat` (default `false`), `notch` (default `true` — the grab handle).

## Popover

Use for non-modal floating content anchored to a trigger.

```tsx
import {
  Popover,
  PopoverContent,
  PopoverHeader,
  PopoverTitle,
  PopoverBody,
} from "@/components/ui/popover"
import { Button } from "@/components/ui/button"

<Popover>
  <Button intent="outline">Open</Button>
  <PopoverContent placement="bottom" arrow>
    <PopoverHeader>
      <PopoverTitle>Title</PopoverTitle>
    </PopoverHeader>
    <PopoverBody>...</PopoverBody>
  </PopoverContent>
</Popover>
```

- `PopoverContent` props: `arrow` (default `false`), `placement`, `offset` — plus all React Aria `Popover` props.
- `PopoverContent` is also the base for `SelectContent`, `ComboBoxContent`, and `MenuContent` — those already wrap it, so do not nest a `PopoverContent` inside them.

## Menu

Use `Menu` (trigger wrapper) → `MenuTrigger` → `MenuContent` → `MenuItem`. For grouping use `MenuSection`/`MenuHeader`/`MenuSeparator`; for shortcuts use `MenuShortcut`; for nested menus use `MenuSubMenu`.

```tsx
import {
  Menu,
  MenuTrigger,
  MenuContent,
  MenuItem,
  MenuSeparator,
  MenuShortcut,
} from "@/components/ui/menu"

<Menu>
  <MenuTrigger>Options</MenuTrigger>
  <MenuContent placement="bottom start">
    <MenuItem>Edit</MenuItem>
    <MenuItem>
      Duplicate <MenuShortcut>⌘D</MenuShortcut>
    </MenuItem>
    <MenuSeparator />
    <MenuItem intent="danger">Delete</MenuItem>
  </MenuContent>
</Menu>
```

- `MenuItem` accepts `intent` (`"danger"` / `"warning"`) for destructive/cautionary actions.
- For right-click menus, use `@/components/ui/context-menu` instead.

## Tooltip

`TooltipTrigger` **is** the button (it is the React Aria `Button`). Style it directly with `buttonStyles(...)` via `className` — do **not** nest a `<Button>` inside it.

```tsx
import { Tooltip, TooltipTrigger, TooltipContent } from "@/components/ui/tooltip"
import { buttonStyles } from "@/components/ui/button"

// ✅ Correct — TooltipTrigger styled as a button
<Tooltip>
  <TooltipTrigger aria-label="Follow me" className={buttonStyles({ intent: "outline", size: "sq-sm" })}>
    <BrandXIcon />
  </TooltipTrigger>
  <TooltipContent>Follow me @intentui</TooltipContent>
</Tooltip>

// ❌ Wrong — nesting a <Button> inside TooltipTrigger produces a button-in-a-button
<Tooltip>
  <TooltipTrigger>
    <Button intent="outline">Hover me</Button>
  </TooltipTrigger>
  <TooltipContent>Helpful hint</TooltipContent>
</Tooltip>
```

- `TooltipContent` props: `arrow` (default `true`), `inverse`, `offset` (default `10`).
- Tooltips are for supplementary hints only — never put essential or interactive content inside them.

## Which triggers are buttons vs. which wrap a `<Button>`

- **The trigger IS the button** — `TooltipTrigger`, `MenuTrigger`, `DrawerTrigger`. Put your label/icon directly inside and style with `className={buttonStyles({...})}`. Never nest a `<Button>`.
- **The root wraps a separate `<Button>`** — `Modal`, `Sheet`, `Popover` are `DialogTrigger` wrappers; pass a real `<Button>` as the trigger child (as shown in their examples above).

## Key patterns

1. **Trigger lives inside the root wrapper** (`Modal`, `Sheet`, `Drawer`, `Popover`, `Menu`, `Tooltip`) — state is managed by the component, do not wire up your own `isOpen`.
2. **Compose with the matching subcomponents** — `*Header`, `*Title`, `*Body`, `*Footer`, `*Close` — never raw `<div>`/`<h2>`/`<button>`.
3. **`ModalContent`/`SheetContent` sizing and side** come from props (`size`, `side`), not custom `max-w-*` classes.
4. **`alertdialog` for destructive confirmation** — set `role="alertdialog"` so it is not dismissible by clicking the backdrop.
5. **Don't double-wrap** `Select`/`ComboBox`/`Menu` content in `PopoverContent` — they already render it internally.
