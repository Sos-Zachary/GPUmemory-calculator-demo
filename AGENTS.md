# AGENTS.md — LLM VRAM Calculator

> AI coding agent guide for this project. Read this first before making any changes.

## Project Overview

This is **LLM VRAM Calculator** (Qwen3.5 全系列显存占用计算器), a single-page React web application that estimates GPU VRAM consumption for running Qwen3.5 LLM models under various configurations (quantization method, inference engine, context length, concurrency).

The entire application UI is implemented as a monolithic single-page dashboard with no actual routing. It features interactive charts, real-time calculation updates, dark/light theming, and GPU hardware recommendations.

## Technology Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Framework | React | ^19.2.0 |
| Language | TypeScript | ~5.9.3 |
| Build Tool | Vite | ^7.2.4 |
| Styling | Tailwind CSS | ^3.4.19 |
| UI Components | shadcn/ui (new-york style) | — |
| Component Primitives | Radix UI | Various |
| Icons | Lucide React | ^0.562.0 |
| Charts | Recharts | ^2.15.4 |
| Form Validation | Zod | ^4.3.5 |
| Forms | React Hook Form | ^7.70.0 |

**Node.js version:** 20 (as noted in `info.md`)

## Project Structure

```
src/
├── main.tsx              # Entry point: StrictMode + BrowserRouter (router is mounted but unused)
├── App.tsx               # Root component containing ALL application UI logic (~750 lines)
├── App.css               # Empty — all styles moved to index.css
├── index.css             # Global styles, Tailwind directives, CSS variables, custom scrollbar, theme system
├── components/
│   └── ui/               # 40+ shadcn/ui components (button, select, slider, badge, card, chart, etc.)
├── pages/
│   └── Home.tsx          # Unused boilerplate from Vite template
├── hooks/
│   ├── useTheme.tsx      # Custom theme context (dark/light toggle via CSS classes)
│   └── use-mobile.ts     # useIsMobile hook (768px breakpoint)
├── lib/
│   └── utils.ts          # `cn()` utility (clsx + tailwind-merge)
└── data/
    └── modelData.ts      # Domain models, static data, VRAM calculation logic (~550 lines)
```

### Key Architectural Notes

- **Monolithic UI:** Despite having a `pages/` directory, the entire application UI lives in a single `App.tsx` file. Helper components (`ChartTooltip`, `PresetBtn`, `AccordionCard`, `FormulaBlock`) are defined at the bottom of the same file. `pages/Home.tsx` is dead boilerplate code.
- **No routing:** `BrowserRouter` is mounted in `main.tsx` but no routes are defined anywhere.
- **Business logic in data layer:** `src/data/modelData.ts` contains all domain data (model specs, quantization methods, inference engines) and the VRAM calculation functions (`calculateVRAM`, `getGPURecommendations`).

## Build and Development Commands

```bash
# Install dependencies
npm install

# Start development server (port 3000)
npm run dev

# Type-check and build for production (outputs to dist/)
npm run build

# Preview production build locally
npm run preview

# Run ESLint
npm run lint
```

### Vite Configuration

- `base: './'` — relative paths for static hosting compatibility
- Dev server port: `3000`
- Path alias: `@/` → `./src`
- Plugins: `@vitejs/plugin-react`, `kimi-plugin-inspect-react` (dev inspection only)

## Code Style Guidelines

### TypeScript Configuration

- Target: ES2022, Module: ESNext, JSX: `react-jsx`
- Strict mode enabled with additional checks:
  - `noUnusedLocals: true`
  - `noUnusedParameters: true`
  - `erasableSyntaxOnly: true`
  - `noFallthroughCasesInSwitch: true`
- Path mapping: `@/*` → `./src/*`

### ESLint

- Flat config format (ESLint v9)
- Extends: `@eslint/js` recommended, `typescript-eslint` recommended, `eslint-plugin-react-hooks` recommended, `eslint-plugin-react-refresh` vite config
- Lints `**/*.{ts,tsx}` files; ignores `dist/`
- **No Prettier is configured.** Do not add Prettier without explicit approval.

### Styling Conventions

- Tailwind CSS is used for layout and utility classes.
- **Inline `style` props are heavily used for dynamic theming** (dark vs. light conditional colors via `isDark` boolean). This is an intentional pattern in this project. When adding new themed elements, follow the existing pattern of using inline styles with `isDark ? darkValue : lightValue`.
- The `cn()` utility from `src/lib/utils.ts` should be used for conditional Tailwind class merging.
- shadcn/ui components follow the standard pattern: Radix UI primitives + Tailwind + `class-variance-authority` (CVA) for variants.

### Custom Theme System

The app uses a **custom theme context** (`src/hooks/useTheme.tsx`), not `next-themes` (which is installed but unused). Theme classes (`theme-dark`, `theme-light`) are applied to `document.documentElement`. `index.css` defines extensive CSS variable sets for both themes.

When styling new components:
1. Check if the existing CSS variable in `index.css` covers your need.
2. If not, follow the inline `style` pattern used throughout `App.tsx` for conditional dark/light values.

## Domain Model & Calculation Logic

The VRAM calculation lives in `src/data/modelData.ts` and follows industry-standard simplified estimation:

1. **Model Weights** = `totalParams(B) × bytesPerParam / 2^30`
2. **KV Cache** = `2 × layers × numKVHeads × headDim × contextLength × concurrency × kvBytesPerParam / 2^30`
3. **Activations** ≈ `concurrency × contextLength × hiddenSize × 18 × bytesPerParam / 2^30`
4. **Engine Overhead** = `(weights + KV Cache + activations) × overheadFactor`

**Important MoE behavior:** For MoE models, ALL expert parameters are loaded for weights, but KV Cache is calculated using the model's architecture dimensions (which correspond to activated parameters).

## Testing

**There is no testing infrastructure.** No test files, test directories, or testing dependencies (no Jest, Vitest, Playwright, Cypress, etc.). If you add tests, you will need to set up the testing framework from scratch.

## Deployment

No CI/CD or deployment configuration is present. However:
- `base: './'` in `vite.config.ts` makes the build suitable for static hosting (GitHub Pages, Netlify, Vercel, etc.)
- Build output goes to `dist/` (standard Vite default)
- No Docker, no `vercel.json`, no `netlify.toml`

## Security Considerations

- This is a pure client-side static application with no backend, no API calls, and no user authentication.
- No sensitive environment variables or secrets are used.
- All data (model specs, formulas) is static and embedded in the bundle.

## Adding New shadcn/ui Components

This project uses the shadcn/ui CLI. To add a new component:

```bash
npx shadcn add <component-name>
```

Configuration is in `components.json`:
- Style: `new-york`
- Base color: `slate`
- Aliases: `components` → `@/components`, `utils` → `@/lib/utils`, `ui` → `@/components/ui`

## Notes for AI Agents

- **Do not refactor the monolithic `App.tsx` into multiple files unless explicitly asked.** The current structure is intentional for this single-purpose tool.
- When modifying UI colors, always handle both dark and light themes using the `isDark` boolean pattern.
- If adding new model data, update `src/data/modelData.ts` and ensure the `ModelSpec` interface covers all needed fields.
- The project has `react-router` installed but unused. Avoid adding route-dependent logic.
- Chinese is used extensively in the UI text and data descriptions. Keep Chinese labels and descriptions when modifying user-facing content.
