# Authentication Architecture & Identity Management

## Purpose
Defines secure, resilient, and standard authentication workflows, session lifecycles, and identity management patterns for web applications.

## When To Use
- When configuring user registration, login, logout, password resets, and session verification.
- When implementing OAuth2/OIDC social logins (Google, GitHub), magic links, passkeys (WebAuthn), or multi-factor authentication (MFA).

## When Not To Use
- For completely static public marketing sites that maintain zero user identity or private data.

## Core Principles
1. **Never Roll Your Own Cryptography**: Always use audited, standard identity libraries (`Auth.js / NextAuth`, `Lucia`, `Clerk`, `WorkOS`).
2. **HttpOnly, Secure, SameSite Cookies**: Store session tokens exclusively in `HttpOnly`, `Secure`, `SameSite=Lax` (or `Strict`) cookies. Never store authentication tokens in `localStorage` or `sessionStorage` (vulnerable to XSS).
3. **Defense in Depth**: Authenticate every request on the server; never trust client-side claims or boolean flags in UI state.

## Rules
- **Rule 1 (Standard Session Cookie Configuration)**:
  Cookies managing authentication must be configured with:
  - `httpOnly: true` (Prevents JavaScript XSS access).
  - `secure: process.env.NODE_ENV === 'production'` (Transmitted only over HTTPS).
  - `sameSite: 'lax'` (Defends against Cross-Site Request Forgery).
  - `path: '/'`
  - `maxAge: 60 * 60 * 24 * 7` (7-day sliding window).
- **Rule 2 (Password Hashing Standards)**:
  If storing local passwords, use **Argon2id** (preferred) or **bcrypt** with a minimum work factor of 12. Never use MD5, SHA-1, SHA-256, or plain text for password storage.
- **Rule 3 (Server-Side Session Verification)**:
  Every protected Server Component, Server Action, and Route Handler must verify the session independently:
  ```typescript
  import { auth } from '@/lib/auth';
  import { redirect } from 'next/navigation';

  export async function requireAuth() {
    const session = await auth();
    if (!session?.user) {
      redirect('/login?callbackUrl=' + encodeURIComponent(headers().get('x-url') || '/dashboard'));
    }
    return session.user;
  }
  ```
- **Rule 4 (Timing-Safe Credential Verification)**:
  When verifying credentials, protect against timing attacks by performing constant-time comparisons (`crypto.timingSafeEqual`) and returning generic error messages ("Invalid email or password", never "User does not exist").

## Decision Criteria
```text
IF app is standard Next.js B2B / B2C:
  Use Auth.js (NextAuth v5) with database sessions or secure encrypted JWTs.
IF app requires Enterprise SSO (SAML/Okta/Azure AD):
  Use WorkOS or Auth0 / Clerk Enterprise connectors.
IF storing user credentials:
  Hash with Argon2id; enforce minimum 8 characters and pass through z.string().min(8).
```

## Recommended Workflow
1. Configure authentication provider in `@/lib/auth.ts`.
2. Set up database adapter (Drizzle/Prisma) for persistent accounts, sessions, and verification tokens.
3. Establish auth routes: `app/api/auth/[...nextauth]/route.ts`.
4. Create authentication gate helper (`requireAuth()`).
5. Build accessible login and registration forms with client and server Zod validation.
6. Verify logout terminates server session and purges cookie.

## Best Practices
- Provide social login options alongside email magic links to eliminate password-related friction.
- Implement rate limiting (5 attempts per 15 minutes) on login and password reset endpoints.
- Send password reset tokens with short expiration windows (max 15 minutes).

## Anti-Patterns
- **Tokens in LocalStorage**: `localStorage.setItem('token', jwt)` (Single XSS vulnerability instantly exposes full session).
- **Client-Side Auth Check Only**: Hiding a UI button with `{isAdmin && <AdminButton />}` without verifying the session on the backend mutation endpoint.
- **User Enumeration**: Telling the user "That email is already registered", allowing attackers to map valid user lists.

## Validation Checklist
- [ ] Authentication tokens are stored strictly in `HttpOnly`, `Secure`, `SameSite` cookies.
- [ ] Protected routes verify sessions on the server.
- [ ] Passwords are hashed with Argon2id or bcrypt (>= 12 rounds).
- [ ] Auth endpoints are protected against brute-force rate limits.

## Related Skills
- `skills/security/authentication-security.md`
- `skills/backend/authorization.md`
- `skills/security/owasp.md`

## References
- OWASP Authentication Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
- Auth.js Official Documentation — https://authjs.dev/

## Last Reviewed
2026-09-15
