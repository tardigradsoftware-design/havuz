# Error Domains, Result Types & Fault-Tolerant Architecture

## Purpose
Establishes deterministic, predictable, and resilient error handling across server and client boundaries, preventing unhandled crashes, white screens, and cryptic user experiences.

## When To Use
- In all API route handlers, Server Actions, database queries, and async client workflows.
- When creating React Error Boundaries and fallback recovery screens.

## When Not To Use
- For normal, non-exceptional conditional branching (e.g., checking if a list is empty is not an error).

## Core Principles
1. **Errors are Values, Not Surprises**: Model anticipated domain failures explicitly using Result types or structured return objects rather than throwing untyped exceptions.
2. **Never Swallow Errors Silently**: Every caught exception must either be handled with a fallback, recorded in logging telemetry, or re-thrown.
3. **No Sensitive Leaks**: Never leak internal stack traces, database table names, SQL queries, or third-party API keys in user-facing error responses.

## Rules
- **Rule 1 (Result Pattern for Server Actions)**:
  Server Actions should return predictable Result objects rather than throwing exceptions across the network boundary:
  ```typescript
  type Result<T, E = string> =
    | { success: true; data: T }
    | { success: false; error: E; fieldErrors?: Record<string, string[]> };

  export async function updateUsername(formData: FormData): Promise<Result<{ username: string }>> {
    try {
      const parsed = UsernameSchema.safeParse(Object.fromEntries(formData));
      if (!parsed.success) {
        return { success: false, error: 'Validation failed', fieldErrors: parsed.error.flatten().fieldErrors };
      }
      const updated = await db.user.update({ ... });
      return { success: true, data: updated };
    } catch (err) {
      logger.error('Failed to update username', { error: err });
      return { success: false, error: 'An unexpected server error occurred.' };
    }
  }
  ```
- **Rule 2 (React Error Boundary Fallbacks)**:
  Every major route group in Next.js must include `error.tsx`:
  ```tsx
  'use client';

  export default function ErrorBoundary({
    error,
    reset,
  }: {
    error: Error & { digest?: string };
    reset: () => void;
  }) {
    return (
      <div role="alert" className="flex flex-col items-center justify-center p-8 space-y-4 text-center">
        <h2 className="text-xl font-bold">Something went wrong</h2>
        <p className="text-sm text-muted-foreground">We encountered an issue loading this section.</p>
        <button onClick={() => reset()} className="px-4 py-2 bg-primary text-primary-foreground rounded-md">
          Try Again
        </button>
      </div>
    );
  }
  ```
- **Rule 3 (Structured Logging Context)**:
  Log errors with structured metadata (request ID, tenant ID, route, timestamp) using a logging utility (`pino`, `winston`, or structured console JSON):
  ```typescript
  logger.error('Stripe webhook processing failed', {
    eventId: event.id,
    tenantId: event.data.object.metadata.tenantId,
    errorMessage: err instanceof Error ? err.message : 'Unknown error',
  });
  ```
- **Rule 4 (Graceful Network Degradation)**:
  Client data-fetching calls must handle offline states, network timeouts, and aborts cleanly without freezing the UI.

## Decision Criteria
```text
IF error is an expected user input error:
  Return structured field errors with HTTP 400 / Result failure; DO NOT log as critical server error.
IF error is an unexpected server crash (database down, null pointer):
  Log critical error with stack trace internally; return generic sanitized HTTP 500 message to client.
IF component sub-tree fails in UI:
  Catch via local Error Boundary; DO NOT crash the entire page layout.
```

## Recommended Workflow
1. Create custom application error classes (`AppError`, `NotFoundError`, `UnauthorizedError`, `ValidationError`).
2. Wrap external calls (third-party APIs, database queries) in defensive `try/catch` blocks.
3. Normalize error response shapes into standard Problem Details or Result types.
4. Scaffold `error.tsx` boundaries at layout and route levels.
5. Verify recovery behavior by triggering simulated network errors in staging.

## Best Practices
- Provide an actionable recovery path on every error screen (e.g. "Try again", "Go to Dashboard", "Contact Support").
- Include a unique error tracking ID (`digest` or `correlationId`) in the user-facing message so support teams can correlate user reports with logs.
- Never write empty catch blocks: `try { ... } catch (e) {}`.

## Anti-Patterns
- **The Empty Catch**: Silently ignoring exceptions and leaving the application in a corrupted, half-initialized state.
- **Stack Trace Exposure**: Printing `Error: PrismaClientKnownRequestError: Table 'users' does not exist at...` directly into the public DOM.
- **Untyped Promise Rejection**: Forgetting to catch asynchronous promises in background tasks, causing Node.js process crashes.

## Validation Checklist
- [ ] No unhandled promise rejections or uncaught exceptions exist.
- [ ] User-facing errors are sanitized of sensitive stack traces.
- [ ] Route-level `error.tsx` boundaries provide a "Try Again" reset trigger.
- [ ] Server errors are logged with contextual metadata.

## Related Skills
- `skills/backend/api-design.md`
- `skills/frontend/react.md`
- `skills/security/owasp.md`

## References
- Node.js Best Practices: Error Handling — https://github.com/goldbergyoni/nodebestpractices#2-error-handling-practices
- React Error Boundary Documentation — https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary

## Last Reviewed
2026-09-15
