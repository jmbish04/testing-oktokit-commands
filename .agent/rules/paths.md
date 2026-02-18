---
trigger: always_on
---

# Rule: Absolute Pathing & Alias Standards

## 1. Path Definition
- ALWAYS use defined path aliases for cross-module imports. 
- **Forbidden**: `import { ... } from "../../../db"`
- **Required**: `import { ... } from "@db/schema"`

## 2. Standard Aliases
- `@/*`: Maps to the local `src` directory.
- `@db/*`: Maps to the Drizzle/D1 data layer in the backend.
- `@api/*`: Maps to the Hono RPC definitions/AppType.
- `@ui/*`: Maps to the Shadcn component registry.
- `@shared/*`: Maps to shared Zod schemas and utility functions.

## 3. Configuration Maintenance
- If the agent creates a new top-level directory (e.g., `services/`), it MUST immediately update `tsconfig.json` and the corresponding bundler config (Vite/Astro) to include the new alias.
- All path aliases must be reflected in the `backend/` and `frontend/` tsconfigs to ensure the RPC client (`hc`) resolves types without internal relative path leakage.