# OWASP Top 10 Security Audit Checklist

> **Stage**: Security & Compliance  
> **Mandatory**: Non-negotiable gate for all deployments.

---

## 1. Access Control & Injection
- [ ] Every database query and mutation enforces tenant/user ownership boundaries.
- [ ] Zero raw SQL string concatenation exists; all queries parameterized via ORM.
- [ ] Zero unescaped user inputs rendered via `dangerouslySetInnerHTML`.
- [ ] Content Security Policy (CSP) and HTTP security headers configured.

## 2. Authentication & Data Protection
- [ ] Session cookies enforce `HttpOnly; Secure; SameSite=Lax`.
- [ ] Authentication and password reset endpoints protected by sliding rate limits.
- [ ] Passwords hashed with Argon2id or bcrypt (>= 12 rounds).
- [ ] All environment secrets validated via Zod on startup; zero secrets in Git or client bundles.

---
*Sign-off: Security Engineer / Lead Agent*
