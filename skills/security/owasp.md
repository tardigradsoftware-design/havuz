# OWASP Top 10 Web Security Architecture & Defense

## Purpose
Establishes comprehensive, proactive defenses against the most critical web application security risks defined by the Open Web Application Security Project (OWASP Top 10).

## When To Use
- In every web application, API endpoint, server action, authentication flow, and database integration.
- During security architecture reviews and pre-release audits.

## When Not To Use
- Never. Security policies apply globally across all code.

## Core Principles
1. **Zero Trust Architecture**: Never trust incoming client data. Validate and sanitize all inputs at every system boundary.
2. **Principle of Least Privilege**: Every module, database user, and service account must operate with the minimum permissions necessary.
3. **Defense in Depth**: Implement layered security controls—if one layer fails (e.g., client validation), secondary layers (server validation, DB constraints, CSP) prevent exploitation.

## Rules
- **Rule 1 (A01: Broken Access Control)**:
  - Enforce tenant and user ownership checks on every database read and write.
  - Disable directory listing and restrict sensitive administrative routes (`/admin/*`) via server middleware.
  - Never expose sequential numeric database IDs; use UUIDv7 or CUID2 to mitigate IDOR enumeration.
- **Rule 2 (A02: Cryptographic Failures)**:
  - Enforce HTTPS strictly via HTTP Strict Transport Security (HSTS):
    `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`
  - Store sensitive data at rest using AES-256-GCM.
  - Hash passwords using Argon2id or bcrypt (>= 12 rounds).
- **Rule 3 (A03: Injection Defenses - SQL & XSS)**:
  - Never concatenate user strings directly into SQL queries. Always use parameterized queries via Drizzle, Prisma, or prepared statements.
  - Mitigate Cross-Site Scripting (XSS) by avoiding `dangerouslySetInnerHTML`. If HTML rendering is mandatory, sanitize using `DOMPurify` or `sanitize-html`.
  - Enforce a strict Content Security Policy (CSP):
    `Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-...'; object-src 'none'; base-uri 'self';`
- **Rule 4 (A05: Security Misconfiguration - Security Headers)**:
  Configure mandatory HTTP security headers in Next.js middleware or config:
  ```typescript
  const securityHeaders = [
    { key: 'X-DNS-Prefetch-Control', value: 'on' },
    { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' },
    { key: 'X-Frame-Options', value: 'DENY' },
    { key: 'X-Content-Type-Options', value: 'nosniff' },
    { key: 'Referrer-Policy', value: 'origin-when-cross-origin' },
    { key: 'Permissions-Policy', value: 'camera=(), microphone=(), geolocation=()' },
  ];
  ```
- **Rule 5 (A06: Vulnerable and Outdated Components)**:
  Run automated dependency vulnerability audits (`npm audit` or Dependabot) in CI. Block deployments containing high or critical severity CVEs.

## Decision Criteria
```text
IF rendering raw HTML from external source:
  Pass through DOMPurify with strict tag allowlist; NEVER render unsanitized HTML.
IF querying database with dynamic input:
  Use ORM parameter binding; NEVER use raw string interpolation (e.g. `WHERE id = '${input}'`).
IF handling file upload:
  Validate MIME type, enforce file size limits, randomize filename, and store on isolated object storage (S3/R2).
```

## Recommended Workflow
1. Run static application security testing (SAST) in CI.
2. Implement centralized security headers middleware.
3. Validate every input using Zod schemas at controller/action entry.
4. Execute `checklists/security-audit.md` before production deployment.
5. Review OWASP ASVS verification levels.

## Best Practices
- Never include sensitive API keys or credentials in client bundles or public repositories.
- Keep dependencies updated with automated tooling (Renovate or Dependabot).
- Log all authorization failures with timestamp and IP address to detect active probes.

## Anti-Patterns
- **Raw SQL Concatenation**: `db.raw(`SELECT * FROM users WHERE email = '${email}'`)` (Classic SQL Injection).
- **Unsanitized HTML Injection**: `<div dangerouslySetInnerHTML={{ __html: userBio }} />` (Instant XSS).
- **Disabled CORS**: Setting `Access-Control-Allow-Origin: *` with credentials enabled.

## Validation Checklist
- [ ] No raw SQL string interpolation exists anywhere in the codebase.
- [ ] Content Security Policy and security headers are active.
- [ ] Dependencies have zero critical or high vulnerabilities in `npm audit`.
- [ ] All database mutations verify ownership/tenant bounds.

## Related Skills
- `skills/security/authentication-security.md`
- `skills/security/api-security.md`
- `skills/security/secrets-management.md`

## References
- OWASP Top 10 Standard — https://owasp.org/www-project-top-ten/
- OWASP Cheat Sheet Series — https://cheatsheetseries.owasp.org/

## Last Reviewed
2026-09-15
