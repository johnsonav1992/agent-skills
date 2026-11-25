---
name: app
description: The code conventions this app must follow. Use whenever writing or changing app code.
---

## Style

- Style with the theme; add repeated values to it. Small one-off spacing is fine.
- Inline `css()` in the `mix` prop unless large or reused. Size in `rem` unless another unit is needed.
- No comments except JSDoc on utils that need it. No barrel `index` files.
- Lint and format occasionally, not after every change.

## Components

- One component per PascalCase folder (`app/ui/Name/Name.tsx`), with its props type declared above it and passed to `Handle<>`. Nothing else goes in the file: utils, constants, label builders and style helpers get their own modules.
- Split into focused pieces when it reads better; don't extract trivial fragments.
- Icons go in `app/ui/icons/<Name>Icon/`, never as inline SVG in other components.
- Reuse existing UI. Build missing shared components to library quality: composable, accessible, keyboard-friendly, themed, tested and documented, with sensible states.
- Use `app/ui/public/Select` for every select and `app/ui/public/Button` for common actions. Controls with other semantics (checkboxes, combobox triggers) stay native.

## Pages and loading

- Each page is its own component inside the shared `AppLayout` (the home page is only the home page). Shared page pieces go in reusable UI.
- Loading regions use a layout-matched `<Name>Skeleton` (from `app/ui/Skeleton`) as the `<Frame fallback>`; pending submits may use `app/ui/Spinner`. Both respect `prefers-reduced-motion`.
- A region that reloads after a mutation is a `<Frame>` with its own GET route, targeted by its forms via `data-rmx-target`. Its actions return the region's HTML for frame requests (`X-Remix-Frame: true`) and a 303 redirect otherwise.

## Structure

```txt
app/
├── routes.ts              # one nested route map per area
├── router.ts              # middleware + router.map(routes.<area>, <area>Controller)
├── actions/
│   ├── controller.tsx     # root: assets route only
│   ├── controller.test.ts # router test for every route and page
│   ├── public/entry.ts    # browser entry
│   └── <area>/            # kebab-case, named after the route area
│       ├── controller.tsx
│       ├── handlers/      # optional
│       ├── components/<Name>Page/<Name>Page.tsx
│       ├── types/
│       └── utils/
├── data/                  # backend only: tables, queries, mutations (no copy, labels or UI types)
├── types/                 # shared across areas
├── utils/                 # shared across areas
├── ui/                    # shared components, icons/, public/ islands
└── theme/
```

- New area: nested map in `routes.ts`, one controller, one `router.map` line.
- Distinct mutations get distinct methods and paths. Use `methodOverride()` after `formData()` for PUT, PATCH or DELETE from native forms.
- Keep code in its area's `utils/` or `types/` until another area needs it.
- Keep simple copy in JSX; extract it only when shared or logic-bearing.
- No empty folders or `.gitkeep` files. Tests sit beside their module; shared fixtures go in `test/fixtures/`.
