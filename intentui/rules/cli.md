# CLI Rules

When a required component does NOT exist in `src/components/ui/`, use the shadcn CLI to search and install it from the registry.

## Step 1: Detect the package runner

Check which lockfile exists in the project root to determine the correct runner:

| Lockfile | Runner | Flag |
|---|---|---|
| `bun.lock` or `bun.lockb` | `bunx --bun` | |
| `pnpm-lock.yaml` | `pnpx` | |
| `package-lock.json` | `npx` | |
| `yarn.lock` | `npx` | |

## Step 2: Detect the registry name

Read `components.json` and check if a `registries` key exists.

- **If `registries` exists** — use the key defined there (e.g. `@irsyad`, `@acme`, etc.)
- **If `registries` does NOT exist** — use `@intentui` as the default. Do NOT modify `components.json` to add a registry.

```bash
# Default (no registries in components.json) → use @intentui
bunx --bun shadcn@latest add @intentui/button

# Custom registry defined (e.g. @irsyad) → use @irsyad
bunx --bun shadcn@latest add @irsyad/button
```

IMPORTANT: Never add or modify the `registries` key in `components.json`. Just read it — if it's there, use it. If it's not, default to `@intentui`.

## Step 3: Search for the component

If you need a component that doesn't exist in `src/components/ui/`, first search the registry:

```bash
# Pattern: <runner> shadcn@latest search <registry>
bunx --bun shadcn@latest search @irsyad
```

This will list all available components in the registry.

## Step 4: Install the component

Once confirmed the component exists in the registry, install it:

```bash
# Pattern: <runner> shadcn@latest add <registry>/<component-name>
bunx --bun shadcn@latest add @irsyad/menu
```

## Full example

Suppose you need `<Menu>` but `src/components/ui/menu.tsx` doesn't exist:

1. Check lockfile → `bun.lock` exists → use `bunx --bun`
2. Read `components.json` → registries key is `@irsyad`
3. Search: `bunx --bun shadcn@latest search @irsyad`
4. Install: `bunx --bun shadcn@latest add @irsyad/menu`
5. Now import from `@/components/ui/menu`

## Important

- Always check `src/components/ui/` first — only install if the component file doesn't exist
- Always read `components.json` for the registry name — never hardcode it
- Always check the lockfile for the correct runner — never assume `npx` or `bunx`
- After installing, verify the component file was created before importing it
