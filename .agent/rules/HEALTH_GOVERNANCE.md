# Health Check Governance

## Rule: Every New Module Must Register a Health Check

When adding a new domain module under `backend/src/`, you **MUST**:

1. Create a `health.ts` file co-located with the module
2. Export `checkHealth(env: Env): Promise<HealthStepResult>`
3. Register the check in `backend/src/health/coordinator.ts` → `CODE_CHECKS` array
4. Assign a `HealthCategory` from the union in `backend/src/health/types.ts`

## Rule: Dynamic Tests via D1

Runtime endpoint monitoring uses the `health_test_definitions` table. CRUD is available at:

- `GET /api/health/tests` — list all
- `POST /api/health/tests` — create (Zod-validated)
- `DELETE /api/health/tests/:id` — remove

## Rule: AI Remediation

Failed health checks automatically receive AI-powered remediation hints via `analyzeFailure()`. These are stored in the `ai_suggestion` column of `health_results`.

## Rule: Cron Schedule

Health suite runs weekly via cron `0 3 * * 0` (Sundays 3AM UTC).  
On-demand runs are available via `POST /api/health/run`.

## Rule: Frontend Sync

The frontend at `/health` **must** display all categories defined in `CATEGORY_META` in `Health.tsx`.
When adding a new `HealthCategory` to `types.ts`, also add an entry to the frontend registry.
