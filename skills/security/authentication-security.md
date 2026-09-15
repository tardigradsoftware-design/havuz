# Authentication Hardening, Rate Limiting & Session Defense

## Purpose
Hardens authentication systems against credential stuffing, brute-force dictionary attacks, session hijacking, timing attacks, and account takeover vulnerabilities.

## When To Use
- When engineering login, signup, password reset, MFA, session token generation, and account recovery flows.
- When configuring authentication cookies and session verification middleware.

## When Not To Use
- For completely public read-only static content that contains zero authenticated state.

## Core Principles
1. **Rate Limiting by Default**: Every authentication endpoint must be aggressively rate-limited by IP and username.
2. **Timing-Attack Immunity**: Cryptographic operations and credential comparisons must run in constant time.
3. **Session Revocability**: Provide an immediate mechanism to revoke all active sessions across devices upon password change.

## Rules
- **Rule 1 (Brute-Force Rate Limiting Standard)**:
  Configure sliding-window rate limiting on sensitive auth endpoints:
  - Login attempts: Max **5 failed attempts per IP / Account per 15 minutes**.
  - Password reset requests: Max **3 requests per email per hour**.
  - Verification codes / OTP: Max **3 attempts per code** before invalidation.
- **Rule 2 (Constant-Time Comparisons)**:
  When verifying API keys, reset tokens, or webhook signatures, use constant-time byte comparisons to prevent side-channel timing attacks:
  ```typescript
  import crypto from 'node:crypto';

  export function safeCompare(a: string, b: string): boolean {
    const bufA = Buffer.from(a);
    const bufB = Buffer.from(b);
    if (bufA.length !== bufB.length) return false;
    return crypto.timingSafeEqual(bufA, bufB);
  }
  ```
- **Rule 3 (Cookie Flag Hardening)**:
  Session cookies must strictly enforce:
  `HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=604800`
- **Rule 4 (No Account Enumeration)**:
  Authentication and recovery endpoints must return identical timing and messages regardless of whether an account exists:
  "If an account exists for this email, a password reset link has been sent."
- **Rule 5 (Session Invalidation on Password Change)**:
  When a user changes their password or updates security settings, invalidate all existing sessions and refresh tokens in the database immediately.

## Decision Criteria
```text
IF user attempts 5 failed logins within 15 minutes:
  Block subsequent attempts with HTTP 429 and require CAPTCHA or email verification.
IF password reset token is used:
  Invalidate token immediately; single-use only with 15-minute expiration.
IF session inactivity exceeds 30 days:
  Expire session and require re-authentication.
```

## Recommended Workflow
1. Set up Redis-backed rate limiting middleware (Upstash Ratelimit or similar).
2. Configure password hashing using Argon2id with salt.
3. Enforce password complexity policies: minimum 8 characters, checked against HaveIBeenPwned common passwords.
4. Issue secure cookies with `HttpOnly` and `SameSite` flags.
5. Implement unit tests verifying rate-limiting triggers after threshold breaches.

## Best Practices
- Encourage multi-factor authentication (TOTP or WebAuthn/Passkeys).
- Send automated security alert emails whenever a login occurs from a new device, browser, or geolocation.
- Store session IDs in database or Redis cache with user-agent and IP metadata to allow users to view and revoke active sessions.

## Anti-Patterns
- **Informative Error Messages**: Responding with "Incorrect password" vs "User not found", allowing attackers to harvest valid email lists.
- **Eternal Sessions**: Issuing JWT tokens with 1-year expiration and no revocation mechanism.
- **Unthrottled Login**: Leaving the `/api/login` endpoint open to automated bot scripts attempting 1,000 passwords per second.

## Validation Checklist
- [ ] Login and password reset endpoints enforce rate limiting.
- [ ] Error messages do not leak account existence.
- [ ] Authentication cookies enforce `HttpOnly`, `Secure`, and `SameSite`.
- [ ] Token comparisons use constant-time operations (`timingSafeEqual`).

## Related Skills
- `skills/backend/authentication.md`
- `skills/security/owasp.md`
- `skills/security/api-security.md`

## References
- OWASP Credential Management Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Credential_Management_Cheat_Sheet.html
- NIST Special Publication 800-63B: Digital Identity Guidelines

## Last Reviewed
2026-09-15
