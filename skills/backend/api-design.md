# Robust API Design (REST & Type-Safe RPC)

## Purpose
Establishes standardized conventions for designing predictable, secure, scalable, and versioned APIs across RESTful endpoints and type-safe RPC systems (tRPC / Next.js Server Actions).

## When To Use
- When designing backend endpoints, Next.js Route Handlers (`app/api/*/route.ts`), or tRPC routers.
- When creating public APIs, webhook receivers, or third-party integration endpoints.

## When Not To Use
- For purely internal, co-located Server Component data fetches where direct database queries (`db.query`) are preferable.

## Core Principles
1. **Predictable Semantics**: Use HTTP methods (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) strictly according to their RFC specifications.
2. **Contract-First Safety**: Every endpoint must have an immutable request/response schema validated via Zod or OpenAPI.
3. **Idempotency by Design**: Ensure mutation endpoints supporting financial or critical operations support `Idempotency-Key` headers to prevent duplicate execution.

## Rules
- **Rule 1 (HTTP Status Code Determinism)**:
  - `200 OK`: Successful retrieval or mutation with returned body.
  - `201 Created`: Resource successfully created (must return `Location` header or created object).
  - `204 No Content`: Successful mutation with no response body (e.g. `DELETE`).
  - `400 Bad Request`: Validation failure (must return structured error details).
  - `401 Unauthorized`: Missing or invalid authentication token/session.
  - `403 Forbidden`: Authenticated user lacks permission to access resource.
  - `404 Not Found`: Resource does not exist (or concealed for security).
  - `409 Conflict`: Unique constraint violation or state collision.
  - `422 Unprocessable Entity`: Syntactically valid request failing business rules.
  - `429 Too Many Requests`: Rate limit exceeded (must return `Retry-After` header).
  - `500 Internal Server Error`: Unhandled server exception.
- **Rule 2 (Standardized Error Envelope - RFC 7807)**:
  API error responses must conform to Problem Details format:
  ```json
  {
    "type": "https://api.example.com/errors/validation-failed",
    "title": "Validation Failed",
    "status": 400,
    "detail": "The payload contains invalid field values.",
    "instance": "/api/v1/projects",
    "errors": {
      "title": ["String must contain at least 3 character(s)"]
    }
  }
  ```
- **Rule 3 (Cursor-Based Pagination for Large Collections)**:
  Never use offset-based pagination (`OFFSET 1000 LIMIT 20`) for high-scale dynamic tables. Use cursor-based pagination:
  ```text
  GET /api/v1/orders?limit=20&cursor=ord_8923fd7a
  ```
  Response includes `nextCursor: string | null` and `hasMore: boolean`.
- **Rule 4 (Request Body Schema Validation)**:
  Every incoming API request must be validated immediately at the entry boundary with Zod:
  ```typescript
  import { NextResponse } from 'next/server';
  import { z } from 'zod';

  const CreateProjectSchema = z.object({
    name: z.string().min(1).max(100),
    description: z.string().max(500).optional(),
  });

  export async function POST(req: Request) {
    try {
      const body = await req.json();
      const validated = CreateProjectSchema.parse(body);
      const project = await db.project.create({ data: validated });
      return NextResponse.json(project, { status: 201 });
    } catch (err) {
      if (err instanceof z.ZodError) {
        return NextResponse.json({ status: 400, errors: err.flatten().fieldErrors }, { status: 400 });
      }
      return NextResponse.json({ status: 500, detail: 'Internal Server Error' }, { status: 500 });
    }
  }
  ```

## Decision Criteria
```text
IF application is monolithic TypeScript (Next.js frontend + backend):
  Prefer Server Actions or tRPC for full end-to-end type safety without manual API contracts.
IF API is consumed by external third parties, mobile apps, or webhooks:
  Use RESTful endpoints with Zod schemas and auto-generated OpenAPI/Swagger specs.
IF mutating payment or order creation:
  Enforce Idempotency-Key header to prevent duplicate charges.
```

## Recommended Workflow
1. Define request and response schemas in `@/schemas/api/`.
2. Implement route handler with authentication and authorization check first.
3. Validate payload with `schema.safeParse()`.
4. Execute business logic inside transactional database boundary if multiple tables mutate.
5. Return standardized HTTP status code and response payload.

## Best Practices
- Pluralize resource names (`/api/v1/users`, `/api/v1/teams`).
- Version public APIs in URL path (`/api/v1/...`).
- Always set appropriate caching headers (`Cache-Control: private, no-cache, no-store`).

## Anti-Patterns
- **The "200 OK with Error Message"**: Returning `{ status: 200, success: false, error: "Unauthorized" }` (Destroys HTTP caching, breaks client error hooks).
- **Leaking Database Models**: Exposing internal database columns (e.g., `hashedPassword`, `internalTenantId`) directly to API consumers.
- **Unbounded Collections**: Querying `SELECT * FROM users` without mandatory `LIMIT` or pagination constraints.

## Validation Checklist
- [ ] Endpoints use correct semantic HTTP verbs (GET, POST, PUT, DELETE).
- [ ] Every request body is validated via Zod before database access.
- [ ] Status codes conform to REST specifications.
- [ ] Large collection queries require cursor or limit pagination.

## Related Skills
- `skills/backend/error-handling.md`
- `skills/security/api-security.md`
- `skills/frontend/typescript.md`

## References
- RFC 7807 — Problem Details for HTTP APIs — https://datatracker.ietf.org/doc/html/rfc7807
- Google Cloud API Design Guide — https://cloud.google.com/apis/design

## Last Reviewed
2026-09-15
