# shortlink-frontend

Frontend for the short-link system, built with Vue 3 + Vite.

## Tech Stack

### Core

- Vue 3
- Vite 4
- Vue Router 4
- Vuex 4

### UI & Interaction

- Element Plus
- @element-plus/icons-vue
- SortableJS (drag sorting)
- QRCode (QR rendering)

### Data & Visualization

- Axios
- ECharts 4 (including China/World map)

### Effects & Utilities

- Three.js
- Vanta.js (login background effect)
- Lodash
- js-cookie

### Styling & Engineering

- SCSS + Less
- ESLint + Prettier
- pnpm

## Requirements

- Node.js 18+ (recommended LTS)
- pnpm 8+

## Quick Start

```bash
pnpm install
pnpm dev
```

Open the local URL printed by Vite (usually <http://localhost:5173>).

## Available Scripts

- `pnpm dev` - start development server
- `pnpm build` - build production bundle
- `pnpm preview` - preview production build locally
- `pnpm lint` - run ESLint and auto-fix
- `pnpm format` - format source files with Prettier

## API Proxy (Development)

Vite proxy is configured in [vite.config.js](vite.config.js):

- `/api` -> `http://127.0.0.1:8000`

Frontend request base path is configured in [src/api/axios.js](src/api/axios.js):

- `/api/short-link/admin/v1`

If your backend address changes, update these two places.

## Project Structure

```text
src/
    api/            # Axios instance + API modules
    assets/         # Static assets
    components/     # Shared components
    core/           # Auth and core helpers
    router/         # Route definitions and guards
    store/          # Vuex store
    utils/          # Utility functions
    views/          # Page-level views
```
