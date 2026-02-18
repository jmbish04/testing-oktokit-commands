# Rule: Real-time Communication Standards

- **WebSocket First**: Any UI component requiring a "progress bar" or "agent status" must use WebSockets via Cloudflare Durable Objects.
- **RPC Only**: Standard REST calls MUST use the Hono RPC client. Manual `fetch('/api/...')` is deprecated and will be flagged as an error.
- **Shared Schemas**: Input validation schemas (Zod) must live in a shared `packages/schema` or `backend/src/schemas` and be used by both the Hono validator and the RPC client.
