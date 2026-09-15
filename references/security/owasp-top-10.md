# OWASP Top 10 Security Cheat Sheet

## 1. A01: Broken Access Control
- Never trust client IDs. Always verify `user.tenantId === record.tenantId`.
- Block unauthorized requests at server middleware with HTTP 403/404.

---

## 2. A02: Cryptographic Failures
- Enforce HTTPS and HSTS headers.
- Hash passwords with Argon2id or bcrypt (>= 12 rounds).
- Store session tokens in `HttpOnly; Secure; SameSite=Lax` cookies.

---

## 3. A03: Injection (SQL & XSS)
- Parameterize all SQL queries via Drizzle/Prisma.
- Never render raw user strings via `dangerouslySetInnerHTML`.
- Enforce strict Content Security Policy (CSP).

---

## 4. A05: Security Misconfiguration
- Disable directory browsing.
- Configure security headers (`X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`).
- Remove verbose debug stack traces from production error responses.

---

## 5. A07: Identification and Authentication Failures
- Rate limit authentication endpoints: max 5 failed attempts per 15 mins.
- Implement timing-safe string comparisons (`crypto.timingSafeEqual`).
- Provide multi-factor authentication (MFA/Passkeys).
