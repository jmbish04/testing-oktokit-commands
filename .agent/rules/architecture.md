---
trigger: always_on
---

# Rule: Architectural Integrity & Communication

## 1. Data Layer Isolation
- **Backend Only**: Drizzle ORM, D1 bindings, and database migrations MUST reside exclusively in the `backend/` or `packages/db` directory.
- **Frontend Prohibition**: The `frontend/` folder is strictly forbidden from importing `drizzle-orm`, `drizzle-kit`, or any direct database drivers. 
- **Action**: If the agent detects a database-related install in the frontend, it must immediately abort and move the logic to a Hono route.

## 2. Communication Standards
- **Primary Protocol**: Use **Hono RPC** (`hc`) for all standard API interactions. This ensures shared type definitions between the Hono `AppType` and the Astro/React frontend.
- **Real-time Status**: For long-running tasks (e.g., AI generation, PR creation), use **WebSockets** (via Durable Objects) or **Server-Sent Events (SSE)**.
- **Type Safety**: Never manually redefine backend response types in the frontend. Always export the `AppType` from the backend and import it into the frontend's API client.

## 3. Operational Flow
- All frontend data fetching must go through a centralized `src/lib/api-client.ts` that initializes the Hono RPC client.
- Status updates for background jobs must be pushed from the backend to the frontend, rather than the frontend polling the database.