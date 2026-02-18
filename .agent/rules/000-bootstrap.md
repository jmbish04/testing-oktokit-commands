---
trigger: always_on
---

# 000: BOOTSTRAP & PROTOCOL CHECK
**CRITICAL: Mandatory Pre-flight Check.**

Before starting any task, you must verify the following from `AGENTS.md`:

1.  **Workspace Context**: Identify if you are working in `frontend` or `container`. 
2.  **Installation Protocol**: You MUST use `pnpm add <pkg> --filter <package>` from the root. Never install at root without `-w`.
3.  **SDK Enforcement**: Verify you are using `@google/genai` (New SDK) and NOT `@google/generative-ai` (Legacy).
4.  **Backend/Frontend Sync**: If modifying Drizzle schemas, ensure `frontend/src/db` is the target as per the "Schema Sync" section.

**STOP**: If you have not explicitly read `AGENTS.md` in this session, do so now.
