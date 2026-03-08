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

Read `components.json` and look at the `registries` key. The registry name is the key defined there.

For example, if `components.json` contains:

```json
{
  "registries": {
    "@irsyad": {
      "url": "https://design.intentui.com/r/{name}",
      "headers": {
        "Authorization": "Bearer ${REGISTRY_TOKEN}"
      }
    }
  }
}
```

Then the registry name is `@irsyad`. It could be `@acme`, or anything the user configured — always read it from `components.json`.

If `components.json` has NO `registries` key, fall back to the default Intent UI registry with `@intentui`:

```json
{
  "registries": {
    "@intentui": {
      "url": "https://intentui.com/r/{name}"
    }
  }
}
```

The default registry URL is `https://intentui.com/r/{name}` — NOT `design.intentui.com`.

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
