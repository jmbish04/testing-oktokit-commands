---
description: Implement Agentic Research Team
---

# Workflow:

## Phase 1: Infrastructure & MCP Layer
1.  **Wrangler Config**:
    *   Edit `wrangler.jsonc` to add `[workflows]`, `[vectorize_indexes]`, and `[send_email]`.
    *   Run `npx wrangler types` to update `worker-configuration.d.ts`.
2.  **Vectorize Setup**:
    *   Run `npx wrangler vectorize create research-index --dimensions 1024 --metric cosine`.
3.  **MCP Adapter**:
    *   Create `src/mcp/github-official-adapter.ts`.
    *   Implement standard GitHub tools (`list_files`, `read_file`, `search_repositories`) using `src/octokit` logic but matching official tool names/schemas.
    *   Import and register this in `src/tools/index.ts` to combine with custom tools.

## Phase 2: The Research Workflow (The Muscle)
1.  **Scaffold Workflow**:
    *   Create `src/workflows/DeepResearchWorkflow.ts` extending `WorkflowEntrypoint`.
2.  **Sandbox Integration**:
    *   Implement `step.do('clone')`:
        ```typescript
        import { Sandbox } from '@cloudflare/sandbox-sdk';
        // ...
        const sandbox = await Sandbox.create({ assets: env.BROWSER });
        await sandbox.run(`git clone ${repoUrl}`);
        ```
3.  **Analysis & RAG**:
    *   Implement `step.do('process')`:
        *   Read file tree.
        *   Split code files.
        *   `env.AI.run('@cf/baai/bge-large-en-v1.5')`.
        *   `env.RESEARCH_INDEX.upsert()`.

## Phase 3: The Orchestrator (The Brain)
1.  **Create Agent**:
    *   Create `src/agents/ResearchAgent.ts` extending `Agent`.
2.  **Logic Implementation**:
    *   **Plan**: `onMessage` -> LLM generates research plan.
    *   **Execute**: Call `env.DEEP_RESEARCH_WORKFLOW.create()`.
    *   **Monitor**: Expose a `reportProgress` RPC method that the Workflow calls to update the Agent.
    *   **HITL**: If the plan involves "Create Issue" or "PR", pause and send `type: 'approval_request'` to WebSocket.

## Phase 4: Daily Discovery & Email
1.  **Cron Handler**:
    *   Update `src/index.ts` to export a `scheduled` handler.
    *   Logic: Fetch "trending" -> Call `DeepResearchWorkflow` with `mode: 'discovery'` -> Aggregate Findings.
2.  **Email**:
    *   Install `mimetext`.
    *   Generate HTML report.
    *   Send via `env.EMAIL_SENDER`.

## Phase 5: Verification
1.  Deploy: `npx wrangler deploy`.
2.  Test MCP: Connect generic MCP client to the Agent.
3.  Test Full Loop: Trigger `ResearchAgent` via Chat UI.