# Backend Architecture, Service Layers & Dependency Decoupling

## Purpose
Establishes clear layered backend architecture, separating presentation, business logic, data access, and third-party integrations to guarantee modular testability and longevity.

## When To Use
- When engineering backend systems, server actions, route handlers, background jobs, and microservices.
- When organizing complex business logic that touches multiple database tables or external services.

## When Not To Use
- For simple static read-only endpoints with zero business logic.

## Core Principles
1. **Layered Separation of Concerns**:
   - **Transport Layer**: Route Handlers, Server Actions (HTTP parsing, headers, cookie auth).
   - **Service Layer**: Pure business logic, validation, domain orchestrations.
   - **Data Access Layer**: Database queries, ORM calls, schema definitions.
   - **Integration Layer**: Third-party APIs (Stripe, Twilio, OpenAI, S3).
2. **Dependency Inversion**: High-level business rules must not depend directly on low-level database drivers; abstract infrastructure behind clean interfaces.
3. **Stateless Service Handlers**: Keep backend service instances completely stateless to allow arbitrary horizontal scaling.

## Rules
- **Rule 1 (The 3-Tier Layering Rule)**:
  Never write raw SQL queries or complex business logic directly inside Next.js Route Handlers or Server Actions:
  ```typescript
  // 1. Transport Layer (Server Action)
  export async function handleSubscribe(formData: FormData) {
    const user = await requireAuth();
    const planId = formData.get('planId') as string;
    return BillingService.createSubscription({ userId: user.id, planId });
  }

  // 2. Service Layer (Business Logic)
  export class BillingService {
    static async createSubscription({ userId, planId }: SubscribeInput) {
      const user = await UserRepository.findById(userId);
      if (!user) throw new NotFoundError('User not found');
      
      const stripeCustomer = await PaymentGateway.ensureCustomer(user);
      const subscription = await PaymentGateway.createSub(stripeCustomer.id, planId);
      
      return SubscriptionRepository.save({ userId, stripeId: subscription.id, planId });
    }
  }

  // 3. Data Access Layer (Repository)
  export class SubscriptionRepository {
    static async save(data: SubscriptionData) {
      return db.subscription.create({ data });
    }
  }
  ```
- **Rule 2 (Domain Exceptions)**:
  Throw explicit domain errors (`InsufficientFundsError`, `DuplicateUserError`) in services; let the transport layer map them to appropriate HTTP status codes (400, 409).
- **Rule 3 (Idempotent Event Handlers)**:
  All webhook receivers and event consumers must track processed message IDs to prevent duplicate processing during network retries.

## Decision Criteria
```text
IF logic touches multiple database tables and sends emails:
  Encapsulate inside a Service class/function; DO NOT execute inline in page.tsx.
IF service requires external third-party API:
  Create an integration client adapter that can be easily mocked in tests.
IF background operation takes > 2 seconds:
  Offload to asynchronous job queue (Inngest, BullMQ, QStash); DO NOT block HTTP response.
```

## Recommended Workflow
1. Define TypeScript domain models and repository interfaces.
2. Implement data access methods in repository modules.
3. Write business logic service functions accompanied by unit tests.
4. Expose services through Server Actions or Route Handlers.
5. Verify complete separation by mocking data access layer during testing.

## Best Practices
- Keep database transactions as short as possible to avoid locking database connections.
- Use dependency injection or modular factory functions to facilitate easy unit test mocking.
- Record structured logs with correlation IDs across all service boundaries.

## Anti-Patterns
- **The God Controller**: A 1500-line Route Handler containing raw SQL, password hashing, Stripe API calls, and email sending mixed together.
- **Leaking HTTP Objects into Services**: Passing Next.js `NextRequest` or `NextResponse` deep into database service functions.
- **Missing Transactions**: Modifying User table and Invoice table in sequence without a transaction; if step 2 fails, database is left corrupted.

## Validation Checklist
- [ ] Route Handlers and Server Actions delegate logic to service layers.
- [ ] Database access is isolated into repository or query helper functions.
- [ ] Third-party vendor calls are wrapped in dedicated adapter modules.
- [ ] Multi-table mutations run inside database transactions.

## Related Skills
- `skills/backend/api-design.md`
- `skills/backend/database.md`
- `skills/architecture/scalability.md`

## References
- Domain-Driven Design — Eric Evans
- Clean Architecture — Robert C. Martin (Uncle Bob)

## Last Reviewed
2026-09-15
