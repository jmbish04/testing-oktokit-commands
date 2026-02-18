---
trigger: always_on
---

### PNPM Workspace Commands
This project is a pnpm monorepo with packages: `frontend` and `container`.
- **Installing Dependencies:** Never install dependencies at the root unless they are project-wide dev tools (e.g., turbo, prettier).
- **Targeted Install:** Use the `--filter` flag to target specific packages from the root:
  - Example: `pnpm add zod --filter frontend`
- **Root Install:** If a package *must* go to the root, use the `-w` flag:
  - Example: `pnpm add -Dw typescript`
- **Internal Dependencies:** When adding one workspace package to another, use the `workspace:*` protocol.
  - Example: `pnpm add @workspace/common --filter frontend`

### State Management & Sync
- When updating schemas in `frontend/src/db`, ensure the backend remains the source of truth if shared.
- Always run `pnpm install` from the root after manual `package.json` edits to update the lockfile.
