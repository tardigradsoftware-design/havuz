# API Security, CORS, CSRF & Server-Side Request Forgery (SSRF)

## Purpose
Protects web APIs, Server Actions, and webhook processors against cross-site exploitation, request forgery, server-side request forgery, and unbounded resource consumption.

## When To Use
- When configuring API route handlers, CORS middleware, external webhook listeners, and URL fetching services.
- When managing server-side outbound requests (`fetch`) based on user input.

## When Not To Use
- For purely static, client-side UI rendering lacking backend route handlers.

## Core Principles
1. **Strict Origin Validation**: Explicitly whitelist permitted origins in CORS configurations; never use wildcard `*` with credentials.
2. **Server-Side Request Forgery (SSRF) Defense**: Never allow arbitrary user-supplied URLs to be fetched directly by the server without strict IP and protocol validation.
3. **Payload Size Boundaries**: Enforce strict body size limits on all incoming requests to defend against Denial of Service (DoS).

## Rules
- **Rule 1 (CORS Whitelist Standard)**:
  Configure CORS with an explicit origin array, never reflection or wildcard with credentials:
  ```typescript
  const ALLOWED_ORIGINS = new Set([
    'https://example.com',
    'https://app.example.com',
  ]);

  export function getCorsHeaders(requestOrigin: string | null) {
    if (requestOrigin && ALLOWED_ORIGINS.has(requestOrigin)) {
      return {
        'Access-Control-Allow-Origin': requestOrigin,
        'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
        'Access-Control-Allow-Headers': 'Content-Type, Authorization',
        'Access-Control-Allow-Credentials': 'true',
      };
    }
    return {};
  }
  ```
- **Rule 2 (SSRF Protection for Webhooks & Link Unfurling)**:
  When fetching a user-provided URL on the server (e.g., custom webhooks, link previews):
  - Enforce `https:` protocol only.
  - Resolve DNS and block private/loopback IP ranges (`127.0.0.1`, `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`, `169.254.169.254` AWS metadata).
  - Set strict timeout (max 3000ms) and response size limit (max 1MB).
- **Rule 3 (CSRF Protection for Mutations)**:
  - Modern Next.js Server Actions automatically enforce Origin header verification against CSRF attacks.
  - For custom Route Handlers handling state-changing requests (`POST`, `PUT`, `DELETE`), verify the `Origin` or `Referer` header matches the host, or require a custom CSRF token header (`X-CSRF-Token`).
- **Rule 4 (Request Body Size Limits)**:
  Enforce explicit payload size caps:
  - Standard JSON APIs: Max **100 kB**.
  - File uploads: Max **10 MB** (or route through direct S3 presigned URLs).

## Decision Criteria
```text
IF endpoint processes third-party webhook (e.g. Stripe, GitHub):
  Verify cryptographic HMAC signature using raw request body buffer before parsing JSON.
IF endpoint accepts URL from user to fetch:
  Validate through SSRF IP filter; NEVER fetch directly without loopback check.
IF endpoint is standard REST mutation:
  Verify Origin header matches application host.
```

## Recommended Workflow
1. Apply global CORS and body-parser middleware with size limits.
2. Implement cryptographic webhook signature verification (`stripe.webhooks.constructEvent`).
3. Add SSRF filter to any outbound link crawler or webhook dispatcher.
4. Configure edge rate limiting per IP / API token.
5. Test against automated security scanner.

## Best Practices
- Use direct client-to-cloud presigned S3/R2 upload URLs to avoid routing large file streams through server memory.
- Always read the raw body buffer when verifying webhook signatures (`await req.text()`).
- Return HTTP 413 Payload Too Large when incoming request bodies exceed size thresholds.

## Anti-Patterns
- **Wildcard CORS with Credentials**: `Access-Control-Allow-Origin: *` combined with `Access-Control-Allow-Credentials: true` (Browser blocks it, but signifies severe misconfiguration).
- **Unchecked Outbound Fetch**: `await fetch(req.body.webhookUrl)` (Allows an attacker to hit `http://169.254.169.254/latest/meta-data/` and steal AWS IAM cloud keys).
- **JSON Parsing Before Signature Verification**: Parsing `await req.json()` before checking Stripe signature (Tampered JSON payload can bypass signature).

## Validation Checklist
- [ ] CORS enforces an explicit origin whitelist.
- [ ] All outbound user-provided URL fetches are protected against SSRF.
- [ ] Webhook endpoints verify cryptographic signatures.
- [ ] Request body payloads are capped to prevent memory exhaustion DoS.

## Related Skills
- `skills/security/owasp.md`
- `skills/backend/api-design.md`
- `skills/security/secrets-management.md`

## References
- OWASP Server-Side Request Forgery Prevention Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html
- OWASP Cross-Site Request Forgery (CSRF) Prevention Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html

## Last Reviewed
2026-09-15
