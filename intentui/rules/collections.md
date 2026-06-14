# Collections Rules

Collection components — `Table`, `ListBox`, `GridList`, `Tree`, `TagGroup`, and `Menu` — are built on `react-aria-components`. They share two patterns that the agent gets wrong most often: **dynamic data via `items` + a render function**, and **selection props** that are *different from `Select`/`ComboBox`*.

All of these are interactive — add `"use client"` to the file.

## 1. Static vs dynamic children

For a fixed, small set, list children directly. For data arrays, pass `items={...}` to the collection and a **single render-function child** `(item) => <Item />`. Every item needs a stable `id` (falls back to `key`).

```tsx
// ✅ Static
<ListBox aria-label="Sizes">
  <ListBoxItem id="s">Small</ListBoxItem>
  <ListBoxItem id="m">Medium</ListBoxItem>
</ListBox>

// ✅ Dynamic — items + render function, each item has an id
<ListBox aria-label="Users" items={users}>
  {(user) => <ListBoxItem id={user.id}>{user.name}</ListBoxItem>}
</ListBox>

// ❌ Wrong — .map() inside a collection (breaks RAC's collection tracking,
//    keyboard nav, and selection)
<ListBox aria-label="Users">
  {users.map((u) => <ListBoxItem id={u.id}>{u.name}</ListBoxItem>)}
</ListBox>
```

**Rule: never `.map()` inside a collection.** Use `items` + a render function. (For nested collections inside Tree/Section, use `<Collection items={...}>` from `react-aria-components/Collection`.)

## 2. Selection props — NOT the same as Select/ComboBox

Collections use **`selectedKeys`** (plural, a `Set`/`Selection`) with **`onSelectionChange`** — this is correct and required here. Do **not** apply the `Select`/`ComboBox` `value`/`onChange` rule to collections.

| Prop | Type | Notes |
|---|---|---|
| `selectionMode` | `"none" \| "single" \| "multiple"` | default `"none"` |
| `selectedKeys` | `Selection` (`Set` or `"all"`) | controlled |
| `defaultSelectedKeys` | `Selection` | uncontrolled |
| `onSelectionChange` | `(keys: Selection) => void` | |
| `disabledKeys` | `Iterable<Key>` | |

```tsx
import { useState } from "react"
import type { Selection } from "react-aria-components/GridList"

const [selectedKeys, setSelectedKeys] = useState<Selection>(new Set())

<Table aria-label="Books" selectionMode="multiple"
  selectedKeys={selectedKeys} onSelectionChange={setSelectedKeys}>
  ...
</Table>
```

Keys in `selectedKeys` reference each item's `id`. Selection checkboxes (Table/GridList/Tree) render **automatically** when `selectionMode="multiple"` — do not add your own `<Checkbox>` column/cell.

## 3. Every collection needs an accessible name

`Table`, `ListBox`, `GridList`, and `Tree` must have a visible label or an `aria-label`. (Same rule as form fields — see forms.md.)

## Table

```tsx
import { Table, TableHeader, TableColumn, TableBody, TableRow, TableCell } from "@/components/ui/table"

<Table aria-label="Books" selectionMode="multiple"
  selectedKeys={keys} onSelectionChange={setKeys}>
  <TableHeader>
    <TableColumn className="w-0">#</TableColumn>
    <TableColumn isRowHeader>Title</TableColumn>
    <TableColumn>Author</TableColumn>
  </TableHeader>
  <TableBody items={books}>
    {(book) => (
      <TableRow id={book.id}>
        <TableCell>{book.id}</TableCell>
        <TableCell>{book.title}</TableCell>
        <TableCell>{book.author}</TableCell>
      </TableRow>
    )}
  </TableBody>
</Table>
```

- One `TableColumn` should be `isRowHeader` (the row's primary cell).
- `Table` props: `selectionMode`, `allowResize` (pair with `isResizable` on columns), `bleed`, `grid` (cell borders), `striped`.
- `TableBody` accepts `renderEmptyState`; a default empty state renders when there are no items.
- For sortable columns set `allowsSorting` and handle `sortDescriptor`/`onSortChange` on `Table`.

## ListBox

A standalone selectable list (the same item primitive powers `Select`/`ComboBox`/`Menu` content).

```tsx
import { ListBox, ListBoxItem, ListBoxSection } from "@/components/ui/list-box"

<ListBox aria-label="Permissions" selectionMode="multiple"
  selectedKeys={keys} onSelectionChange={setKeys} items={perms}>
  {(p) => <ListBoxItem id={p.id}>{p.label}</ListBoxItem>}
</ListBox>
```

- Group with `<ListBoxSection title="...">` (pass nested items via `items` on the section).
- `ListBoxItem` shows a check automatically when selected.

## GridList

Like ListBox but each row is a focusable grid cell, supporting actions/drag inside a row.

```tsx
import { GridList, GridListItem } from "@/components/ui/grid-list"

<GridList aria-label="Files" selectionMode="multiple"
  selectedKeys={keys} onSelectionChange={setKeys} items={files}>
  {(file) => <GridListItem id={file.id} textValue={file.name}>{file.name}</GridListItem>}
</GridList>
```

- Pass `textValue` on `GridListItem` when children aren't a plain string (needed for typeahead).
- Selection checkboxes appear automatically in multiple mode.

## Tree

Hierarchical list. Each node is a `TreeItem` whose label goes in `TreeContent`; nest children directly or with `<Collection items={node.children}>`.

```tsx
import { Tree, TreeItem, TreeContent } from "@/components/ui/tree"

<Tree aria-label="Files" defaultExpandedKeys={["src"]}
  selectionMode="multiple" selectedKeys={keys} onSelectionChange={setKeys}>
  <TreeItem id="src" textValue="src">
    <TreeContent>src</TreeContent>
    <TreeItem id="index" textValue="index.ts">
      <TreeContent>index.ts</TreeContent>
    </TreeItem>
  </TreeItem>
</Tree>
```

- `TreeContent` is required inside each `TreeItem` — it renders the chevron, indentation, and selection checkbox. Don't put the label directly in `TreeItem`.
- Expansion: `expandedKeys`/`defaultExpandedKeys` + `onExpandedChange`.
- Provide `textValue` on each `TreeItem` for typeahead.

## TagGroup

```tsx
import { TagGroup, TagList, Tag } from "@/components/ui/tag-group"
import { Label } from "@/components/ui/field"

<TagGroup selectionMode="multiple" selectedKeys={keys} onSelectionChange={setKeys}>
  <Label>Tags</Label>
  <TagList items={tags}>
    {(tag) => <Tag id={tag.id}>{tag.name}</Tag>}
  </TagList>
</TagGroup>
```

- Selection props live on `TagGroup`; `items` + render function live on `TagList`.
- Removable tags: pass `onRemove` to `TagGroup` — the remove button renders automatically.
- For a tag **input** (typing to add tags), use `TagField` from `@/components/ui/tag-field` instead.

## Menu

See [overlays.md](overlays.md) for full Menu composition. The same dynamic pattern applies — `MenuContent` takes `items` + a render function, and selectable menus use `selectionMode` + `selectedKeys`/`onSelectionChange`.

## Key patterns

1. **Never `.map()` inside a collection** — use `items={...}` + a render function `(item) => <Item id={item.id} />`.
2. **Collections use `selectedKeys`/`onSelectionChange`** (a `Set`) — *not* the `value`/`onChange` of `Select`/`ComboBox`.
3. **Every item needs an `id`**; selection keys reference those ids.
4. **Selection checkboxes are automatic** in `selectionMode="multiple"` for Table/GridList/Tree — don't add your own.
5. **`aria-label` (or visible label) is required** on Table/ListBox/GridList/Tree.
6. **`textValue`** on items whose children aren't plain strings, so keyboard typeahead works.
