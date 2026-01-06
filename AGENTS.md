# AGENTS.md

This file provides guidance for AI coding agents working in the Quartz codebase.

## Project Overview

Quartz v4 is a static site generator for digital gardens and note publishing, built with TypeScript, Preact, and Node.js. It transforms Markdown content into a static website with features like full-text search, graph visualization, and backlinks.

## Build Commands

```bash
npm ci                      # Install dependencies
npm run check               # Type check + format check (CI validation)
npm run format              # Format code with Prettier
npx quartz build            # Build the site
npx quartz build --serve    # Build and serve with hot reload
```

## Test Commands

```bash
npm test                                                           # Run all tests
npx tsx --test quartz/util/path.test.ts                            # Run specific test file
npx tsx --test --test-name-pattern="transforms" quartz/util/*.test.ts  # Run tests matching pattern
```

Test files use `*.test.ts` naming and are colocated with source files.

## Code Style

### Formatting (Prettier)

- **No semicolons** (`semi: false`)
- **Trailing commas** in all multiline structures (`trailingComma: "all"`)
- **2-space indentation** (`tabWidth: 2`)
- **100 character line width** (`printWidth: 100`)

### TypeScript

- **Strict mode** enabled
- **No unused locals or parameters** - remove or prefix with `_`
- **ES modules** - use `import`/`export`, not `require`

### Import Organization

Order imports by: external packages, then internal modules, then relative paths.

```typescript
import { promises } from "fs"
import path from "path"
import remarkMath from "remark-math"

import { QuartzTransformerPlugin } from "../types"
import { PerfTimer } from "./util/perf"
```

### Type Patterns

Use branded/nominal types for type safety with string-based identifiers:

```typescript
type SlugLike<T> = string & { __brand: T }
export type FilePath = SlugLike<"filepath">
```

Always provide type guard functions for branded types:

```typescript
export function isFilePath(s: string): s is FilePath {
  return !s.startsWith(".") && _hasFileExtension(s)
}
```

### Naming Conventions

- **Files**: `camelCase.ts` for utilities, `PascalCase.tsx` for components
- **Functions/Variables**: `camelCase`; **Types**: `PascalCase`; **Constants**: `SCREAMING_SNAKE_CASE`
- **Test files**: `*.test.ts` colocated with source

### Component Pattern

Quartz components use factory functions returning Preact components:

```typescript
const Body: QuartzComponent = ({ children }: QuartzComponentProps) => {
  return <div id="quartz-body">{children}</div>
}

Body.afterDOMLoaded = clipboardScript
Body.css = clipboardStyle

export default (() => Body) satisfies QuartzComponentConstructor
```

Components can attach `css`, `beforeDOMLoaded`, and `afterDOMLoaded` properties.

### Plugin Pattern

Plugins are factory functions returning plugin instances. Three types: `transformers`, `filters`, `emitters`.

```typescript
export const Latex: QuartzTransformerPlugin<Partial<Options>> = (opts) => {
  return {
    name: "Latex",
    markdownPlugins() {
      return [remarkMath]
    },
  }
}
```

### Error Handling

Use the `trace` function for user-friendly error messages:

```typescript
import { trace } from "./util/trace"

try {
  // risky operation
} catch (err) {
  trace("Failed to process file", err as Error)
}
```

## Architecture

```
quartz/
├── bootstrap-cli.mjs    # CLI entry point
├── build.ts             # Build orchestration
├── cfg.ts               # Configuration types
├── cli/                 # CLI command handlers
├── components/          # Preact UI components
│   ├── scripts/         # Client-side JS (*.inline.ts)
│   └── styles/          # Component SCSS
├── plugins/             # transformers, filters, emitters
├── processors/          # Parse/filter/emit pipeline
└── util/                # Utility functions (must be isomorphic)
```

## Commit Convention

Follow Conventional Commits: `feat:`, `fix:`, `docs:`, `style:`, `refactor:`, `perf:`, `test:`, `chore:`, `build:`, `ci:`

## Key Files

- `quartz.config.ts` / `quartz.layout.ts` - Site and layout configuration
- `quartz/cfg.ts`, `quartz/plugins/types.ts`, `quartz/components/types.ts` - Type definitions

## Important Notes

- Node.js >= 22, npm >= 10.9.2 required
- Uses Preact (not React) for JSX - import from `preact`
- SCSS for styling, bundled with esbuild
- No ESLint - relies on TypeScript strict mode + Prettier
- Files in `quartz/util/` must be isomorphic (no Node.js-specific APIs)
