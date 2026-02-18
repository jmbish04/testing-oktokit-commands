---
description: This plan deploys the self-contained "LLM-as-a-Judge" workflow to GitHub Actions.
---

# Implementation Plan: GitHub Actions Research Judge

This plan deploys the self-contained "LLM-as-a-Judge" workflow to GitHub Actions.

## User Intent

Create a robust GitHub Action that uses Cloudflare Workers AI (`@cf/openai/gpt-oss-120b`) to orchestrate, execute, and evaluate GitHub repository searches before syncing them to a Cloudflare Worker.

## Technical Context

- **Infrastructure**: GitHub Actions (`ubuntu-latest`)
- **Language**: Python 3.11
- **AI Provider**: Cloudflare AI Gateway (OpenAI Compatible Endpoint)
- **Model**: `gpt-oss-120b` (128k context)

## Execution Steps

### 1. Create Workflow File

- **Path**: `.github/workflows/research-judge.yml`
- **Content**: Copy the provided YAML exactly.
- **Key Features**:
  - Embeds `research_judge.py` directly (no extra file management).
  - Uses `pydantic` for strict JSON schema validation from the LLM.
  - Implements a `TinyAgent` class to wrap the OpenAI SDK interactions.

### 2. Configure Secrets (Manual)

You must add the following secrets to your GitHub Repository:

- `CLOUDFLARE_ACCOUNT_ID`: Your CF Account ID.
- `CLOUDFLARE_GATEWAY_ID`: The ID of your AI Gateway.
- `CLOUDFLARE_API_TOKEN`: Token with Workers AI permissions.
- `WORKER_API_KEY`: Token to authenticate with your Hono Worker.

### 3. Usage

- **Manual**: Go to "Actions" -> "Deep Research Judge" -> "Run workflow" -> Enter a prompt.
- **Automated**: Send a POST request from your Worker:
  ```typescript
  await fetch("https://api.github.com/repos/OWNER/REPO/dispatches", {
    method: "POST",
    body: JSON.stringify({
      event_type: "deep-research",
      client_payload: {
        query: "Find react agents",
        callback_url: "https://your.worker/callback",
      },
    }),
  });
  ```
