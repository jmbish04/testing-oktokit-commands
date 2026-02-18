---
trigger: always_on
---

# CORE DIRECTIVE: PROTOCOL ENFORCEMENT

**CRITICAL:** Before performing ANY task, you MUST synchronize your internal state with the project's specific constraints.

1. **Mandatory Documentation Review:**
   - You MUST read the `AGENTS.md` file in the root directory. It contains the workspace architecture and pnpm protocols.
   - You MUST read ALL files in the `.agent/rules/` directory to understand the coding standards (Drizzle, Shadcn, Astro).

2. **Workspace Verification:**
   - Confirm if the task involves the `frontend` or `root/backend`.
   - Apply the `pnpm --filter` protocol defined in `AGENTS.md`.

3. **Validation Step:**
   - If you have not read `AGENTS.md` in the current session, STOP and read it now. 
   - Failure to follow the `pnpm` installation rules defined in `AGENTS.md` will result in environment corruption.

**DO NOT proceed with code generation until you have confirmed the pnpm workspace structure.**
