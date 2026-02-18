# GitHub Actions LLM Rules

## 1. Resilience

- Always use `response_format={"type": "json_object"}` when interacting with `gpt-oss-120b` for data extraction.
- Implement `try/except` blocks around the LLM call to prevent a single bad generation from crashing the CI pipeline.

## 2. Dependency Management

- Keep scripts self-contained within the YAML (using `cat <<EOF`) for simple tasks, or use a dedicated `scripts/` folder for complex ones.
- Prioritize standard libraries (`openai`, `pydantic`) over experimental ones to ensure stability in the CI runner.
