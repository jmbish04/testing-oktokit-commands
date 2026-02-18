---
description: System-Wide RPC & WebSocket Refactor
---

# Workflow: System-Wide RPC & WebSocket Refactor

## Phase 1: Backend RPC Preparation

- [ ] In `backend/src/index.ts`, group all routes into a `routes` constant.
- [ ] Export `type AppType = typeof routes`.
- [ ] Verify `zValidator` is used for all inputs to ensure RPC type inference.

## Phase 2: Frontend Client Setup

- [ ] Install `hono` in frontend: `pnpm add hono`.
- [ ] Create `frontend/src/lib/api-client.ts` to initialize `hc<AppType>`.

## Phase 3: WebSocket Implementation

- [ ] Create a Hono WebSocket route in the backend using `c.env.MY_DURABLE_OBJECT.fetch(c.req.raw)`.
- [ ] Implement the `onmessage` and `onclose` handlers in `LandingPageGenerator.tsx`.

## Phase 4: Cleanup

- [ ] Remove all manual `fetch()` calls and custom `Response` types in the frontend.
- [ ] Ensure all API errors are caught by `sonner` toasts via the RPC proxy.
