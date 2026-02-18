---
description: Run the full health suite and review results
---

# Run Health Suite

// turbo-all

## Steps

1. Trigger the health check via API:

```bash
curl -s -X POST https://core-github-api.126colby.workers.dev/api/health/run \
  -H "Content-Type: application/json" \
  -H "x-api-key: $API_KEY" | jq .
```

2. Check the latest results:

```bash
curl -s https://core-github-api.126colby.workers.dev/api/health/latest \
  -H "x-api-key: $API_KEY" | jq '.results[] | {category, name, status, ai_suggestion}'
```

3. View run history:

```bash
curl -s "https://core-github-api.126colby.workers.dev/api/health/history?limit=5" \
  -H "x-api-key: $API_KEY" | jq '.runs[] | {id: .run.id, status: .run.status, created: .run.created_at}'
```

4. List dynamic test definitions:

```bash
curl -s https://core-github-api.126colby.workers.dev/api/health/tests \
  -H "x-api-key: $API_KEY" | jq .
```
