# Secrets Management, Environment Variables & Credential Sanitization

## Purpose
Eliminates hardcoded credentials, prevents accidental secret leaks to client bundles or Git repositories, and ensures secure runtime injection of API keys and database credentials.

## When To Use
- In every project configuring environment variables (`.env`, `.env.local`), database connections, and third-party API integrations.
- Before committing any code to Git or pushing to public remotes.

## When Not To Use
- For public configuration constants (e.g., standard layout max-width, default pagination limits).

## Core Principles
1. **Zero Hardcoded Secrets**: No API keys, passwords, database URLs, or webhook secrets may ever exist in source code files.
2. **Schema-Validated Environment**: Validate all environment variables at build/runtime launch using Zod. Fail fast if a required variable is missing.
3. **Client vs Server Isolation**: Maintain a strict boundary between public environment variables (`NEXT_PUBLIC_`) and private server secrets.

## Rules
- **Rule 1 (Environment Schema Validation via Zod)**:
  Define a centralized `src/env.ts` file that validates environment variables upon server startup:
  ```typescript
  import { z } from 'zod';

  const serverSchema = z.object({
    DATABASE_URL: z.string().url(),
    AUTH_SECRET: z.string().min(32),
    STRIPE_SECRET_KEY: z.string().startsWith('sk_'),
    NODE_ENV: z.enum(['development', 'test', 'production']).default('development'),
  });

  const clientSchema = z.object({
    NEXT_PUBLIC_APP_URL: z.string().url(),
    NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY: z.string().startsWith('pk_'),
  });

  export const env = {
    ...serverSchema.parse(process.env),
    ...clientSchema.parse({
      NEXT_PUBLIC_APP_URL: process.env.NEXT_PUBLIC_APP_URL,
      NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY: process.env.NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY,
    }),
  };
  ```
- **Rule 2 (The Public Prefix Barrier)**:
  - Private server secrets must NEVER start with `NEXT_PUBLIC_`.
  - Only truly public values (e.g., Google Analytics ID, public Stripe key) may use `NEXT_PUBLIC_`.
- **Rule 3 (Git Ignore Enforcement)**:
  `.gitignore` must contain:
  ```text
  .env
  .env.local
  .env.development.local
  .env.test.local
  .env.production.local
  *.pem
  *.key
  ```
  Provide an annotated `.env.example` file containing dummy placeholders for team onboarding.
- **Rule 4 (No Leaking Secrets in Logs or Client Traces)**:
  Ensure logging utilities scrub sensitive fields (`password`, `token`, `secret`, `authorization`, `creditCard`) before emitting log strings.

## Decision Criteria
```text
IF an API key is needed only by the server:
  DO NOT add NEXT_PUBLIC_ prefix. Access via process.env.MY_SECRET in Server Component/Action.
IF a required environment variable is missing on app boot:
  Throw explicit fatal error immediately; DO NOT silently proceed with undefined credentials.
IF secret was accidentally committed to Git:
  Consider the secret compromised immediately. Revoke and rotate key in provider dashboard; rewrite Git history.
```

## Recommended Workflow
1. Create `.env.example` with documented variable keys and dummy values.
2. Create `.env.local` for local development secrets (verified in `.gitignore`).
3. Implement `src/env.ts` schema validation.
4. Reference variables via `import { env } from '@/env'` to guarantee type safety.
5. Run automated secret scanner (`trufflehog` or `git-secrets`) in CI pre-commit hooks.

## Best Practices
- Rotate production API secrets on a scheduled 90-day cadence.
- Use cloud secret managers (AWS Secrets Manager, HashiCorp Vault, Vercel Environment Variables) for production deployments.
- Use distinct, isolated API keys for staging and production environments.

## Anti-Patterns
- **The Accidental Prefix**: Naming a database connection string `NEXT_PUBLIC_DATABASE_URL`, broadcasting production database credentials to the world.
- **Committed .env Files**: Forgetting to add `.env` to `.gitignore` on initial commit, leaking live credentials to GitHub history.
- **Silent Undefined**: Falling back to empty strings (`process.env.API_KEY || ''`), causing cryptic downstream runtime crashes.

## Validation Checklist
- [ ] `.gitignore` contains all `.env*` files except `.env.example`.
- [ ] No hardcoded secrets or API tokens exist in source files.
- [ ] Environment variables are validated on startup via Zod.
- [ ] Private secrets are free of `NEXT_PUBLIC_` prefixes.

## Related Skills
- `skills/security/owasp.md`
- `skills/backend/authentication.md`
- `skills/core/verification.md`

## References
- The Twelve-Factor App: Config — https://12factor.net/config
- OWASP Secrets Management Cheat Sheet — https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html

## Last Reviewed
2026-09-15
