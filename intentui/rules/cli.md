# CLI Rules

When a required component or block does NOT exist locally, use the shadcn CLI to search and install it from the registry.

## Step 1: Detect the package runner

Check which lockfile exists in the project root to determine the correct runner:

| Lockfile | Runner |
|---|---|
| `bun.lock` or `bun.lockb` | `bunx --bun` |
| `pnpm-lock.yaml` | `pnpx` |
| `package-lock.json` | `npx` |
| `yarn.lock` | `npx` |

## Step 2: Detect the registry name

Read `components.json` and check if a `registries` key exists.

- **If `registries` exists** — use the first key defined there (e.g. `@irsyad`, `@acme`, `@intentui`, etc.)
- **If `registries` does NOT exist** — use `@intentui` as the default. Do NOT modify `components.json` to add a registry.

IMPORTANT: Never add or modify the `registries` key in `components.json`. Just read it — if it's there, use it. If it's not, default to `@intentui`.

## Step 3: Search the registry

The registry has two types of items:

- **`registry:ui`** — foundational components (button, tabs, modal, etc.) installed to `src/components/ui/`
- **`registry:block`** — ready-made page sections and patterns (sign-in forms, settings pages, dashboards, shopping carts, etc.)

### Searching for components

If you need a component that doesn't exist in `src/components/ui/`, search the registry directly with what you need:

```bash
# Pattern: <runner> shadcn@latest search <registry> -q "<query>"
bunx --bun shadcn@latest search @intentui -q "modal"
```

### Searching for blocks (pages, sections, features)

When the user asks for a page or feature (e.g. "login page", "dashboard", "settings page"), **search the registry using their exact terms**:

```bash
# User asks for "dashboard" → search "dashboard"
bunx --bun shadcn@latest search @intentui -q "dashboard"

# User asks for "login page" → search "login"
bunx --bun shadcn@latest search @intentui -q "login"

# User asks for "settings" → search "settings"
bunx --bun shadcn@latest search @intentui -q "settings"
```

Look at the results — items with `"type": "registry:block"` are page blocks, items with `"type": "registry:ui"` are components. Pick the one that best matches what the user needs.

If the first search doesn't return relevant results, try broader or alternative keywords. You can also list all available blocks by searching with a broad term and filtering results by `"type": "registry:block"`.

## Step 4: Install

Once you've found the right item, install it:

```bash
# Install a component
bunx --bun shadcn@latest add @intentui/menu

# Install a block
bunx --bun shadcn@latest add @intentui/sign-in-and-registration-05
```

## Full example — installing a component

Suppose you need `<Menu>` but `src/components/ui/menu.tsx` doesn't exist:

1. Check lockfile → `bun.lock` exists → use `bunx --bun`
2. Read `components.json` → registries key is `@irsyad` → use `@irsyad`
3. Search: `bunx --bun shadcn@latest search @irsyad -q "menu"`
4. Find `menu` with `"type": "registry:ui"` in results
5. Install: `bunx --bun shadcn@latest add @irsyad/menu`
6. Import from `@/components/ui/menu`

## Full example — installing a block

User asks: "I need a login page"

1. Check lockfile → `bun.lock` exists → use `bunx --bun`
2. Read `components.json` → registries key is `@irsyad` → use `@irsyad`
3. Search: `bunx --bun shadcn@latest search @irsyad -q "sign"`
4. Browse results for `"type": "registry:block"` items like `sign-in-and-registration-05`
5. Install: `bunx --bun shadcn@latest add @irsyad/sign-in-and-registration-05`

## Important

- Always check `src/components/ui/` first — only install components if the file doesn't exist
- Always read `components.json` for the registry name — never hardcode it
- Always check the lockfile for the correct runner — never assume `npx` or `bunx`
- After installing, verify the file was created before importing it
- When searching for blocks, try multiple keyword variations if the first search doesn't match
