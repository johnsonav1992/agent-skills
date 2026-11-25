---
name: remix-app-builder
description: The code conventions this app must follow. Use whenever writing or changing app code.
---

## Code style

- Style with the theme. Add repeated values to the theme; small one-off spacing is fine.
- The theme should never get bloated and only contain values that are the core of the app's styling and design patterns
- Inline `css()` in the JSX `mix` prop unless it is large or reused.
- Size in `rem` unless `px` or another unit is truly needed.
- No comments ever, except JSDoc on utils that need it.
- No barrel `index` files, ever.

## Components

- Each component lives in its own PascalCase folder: `.../Name/Name.tsx`.
- Put props in a named type above the component and pass it to `Handle<>`.
- A component file holds only the component and its props types. Simple display text stays in JSX; utils, constants, label builders and style helpers go in separate modules.
- Split components into focused pieces when that reads better, but don't extract trivial fragments.
- Standalone icons go in `app/ui/icons/<Name>Icon/`. Don't put inline SVG in other components.
- Always reach for a UI component from the ui library first when building. When a shared component is missing and justified, build it to library quality: composable API, accessible, keyboard support, sensible states, themed, tested and documented.


## Pages and loading

- Every page is its own page component, wrapped by a shared layout (`AppLayout`). The home page is only the home page. Shared page pieces live in reusable UI.
- Use proper loaders, either spinners or skeletons. Skeletons are preferred for layout-based and spinners for buttons.
- Prefer using `Frame` components for parts of the UI that must reload separately based on user actions after the initial page has loaded

## Structure

```txt
app/
├── routes.ts          # one nested route map per area
├── router.ts          # middleware + router.map(routes.<area>, <area>Controller)
├── actions/
│   ├── controller.tsx         # root: assets route only
│   ├── controller.test.ts     # router test covering every route and page
│   ├── public/                # browser-reachable: entry.ts + app-wide browser scripts
│   └── <area>/                # kebab-case, named after the route area
│       ├── controller.tsx
│       ├── components/<Name>Page/<Name>Page.tsx
│       ├── public/            # islands and browser scripts only this area uses
│       ├── types/
│       └── utils/
├── middleware/        # request middleware (auth, session, database, logging)
├── data/              # backend only: tables, relations, queries, mutations
├── types/             # types shared across areas
├── utils/             # helpers shared across areas
├── ui/                # shared components, icons/, public/ islands
└── theme/
db/                    # migrations/ and seed.sql (Remix CLI defaults for `remix db`)
linting/               # custom lint plugin and rules
test/fixtures/         # shared test fixtures
```

- A new area needs a nested map in `routes.ts`, one controller, and a `router.map` line in `router.ts`.
- Give distinct form mutations distinct methods and paths. Use `methodOverride()` after `formData()` for PUT, PATCH or DELETE from native forms.
- Code used by one area stays in its local `utils/` or `types/`. Move it to `app/utils/` or `app/types/` only when something else needs it.
- `app/data/` has no copy, labels, formatters or UI types.
- Keep simple copy in the JSX. Extract it only when it's shared or has logic.
- No empty folders and no `.gitkeep` files.
- Tests sit beside the module they test. Shared fixtures go in `test/fixtures/`.
