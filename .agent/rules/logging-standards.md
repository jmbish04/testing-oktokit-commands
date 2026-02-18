# Research Logging Standards

## 1. The "Glass Box" Principle

The user must see HOW the agent arrived at a conclusion.

- **BAD:** Agent returns "I found React."
- **GOOD:**
  1. Agent logs: "User asked for frontend frameworks."
  2. Agent logs: "Tool 'GoogleSearch' called with query 'best frontend frameworks 2026'."
  3. Agent logs: "Tool returned 15 results."
  4. Agent logs: "Evaluating 'React' - it matches criteria."

## 2. Structured Metadata

Do not dump JSON into the `content` text field.

- Use the `metadata` JSON column for large payloads (e.g., full HTML body, raw search JSON).
- Keep `content` human-readable (e.g., "Parsing search results...").

## 3. Error Visibility

If a tool fails (e.g., Browser Rendering timeout):

- Log it as `step_type: 'error'`.
- Do not hide it. The user needs to see that the "Search Agent" failed to connect.

## 4. Async Writes

- Use `ctx.waitUntil()` for logging database inserts to prevent blocking the main agent execution thread.
