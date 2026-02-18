---
trigger: always_on
---

# AGENT & WORKFLOW GOVERNANCE

## 1. The Manager-Worker Pattern
*   **Rule**: Agents (`extends Agent`) **MUST NOT** perform long-running blocking tasks (>10s).
*   **Rule**: Long-running tasks (Cloning, Vectorizing, Scraping) **MUST** be offloaded to Cloudflare Workflows (`extends WorkflowEntrypoint`).
*   **Rule**: Agents act as "Managers" (State/Decision); Workflows act as "Workers" (Execution).

## 2. Tool Integration (MCP)
*   **Constraint**: Do NOT import Node.js-exclusive packages (e.g., `fs`, `child_process`) directly into the Worker.
*   **Strategy**: Adapt the *logic* of official tools into the Agents SDK `sql`-backed state or stateless `Octokit` calls.
*   **Schema**: All tools must strictly define Zod schemas for the Agents SDK to generate valid MCP interfaces.

## 3. Sandbox Usage
*   **Lifecycle**: Sandboxes are ephemeral. Data must be extracted (to R2, D1, or Vectorize) before the `step` completes.
*   **Security**: Never pass raw user input directly to `sandbox.exec()`. Sanitize command arguments.

## 4. Vectorization & RAG
*   **Chunking**: Code must be chunked (e.g., by function/class) before embedding.
*   **Model**: Use `@cf/baai/bge-large-en-v1.5` for embeddings (1024 dimensions).
*   **Metadata**: Upserts MUST include `{ repo, filepath, commit_sha }`.