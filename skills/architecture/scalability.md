# High-Throughput Scalability, Caching & Cloud Resilience

## Purpose
Prepares web applications to effortlessly scale from thousands to millions of concurrent users without downtime, database saturation, or performance degradation.

## When To Use
- When architecting systems for rapid user growth, traffic spikes (product launches, sales events), or high concurrent load.
- When configuring database connection pooling, distributed caching, and background job queues.

## When Not To Use
- For early-stage validation experiments where premature over-engineering delays initial feedback.

## Core Principles
1. **Stateless Compute**: Application servers must maintain zero in-memory session state, allowing instances to be dynamically scaled from 0 to 1,000 nodes.
2. **Protect the Database**: The database is almost always the ultimate bottleneck. Guard it with connection pooling, read replicas, and caching layers.
3. **Asynchronous Decoupling**: Defer non-critical work (emails, notifications, analytics, PDF generation) to background worker queues.

## Rules
- **Rule 1 (Connection Pooling Enforcement)**:
  In serverless environments (Vercel, AWS Lambda), always use connection poolers (Neon, Supabase PgBouncer, Prisma Accelerate, AWS RDS Proxy) to prevent exhausting database connection limits during traffic spikes.
- **Rule 2 (Read/Write Separation)**:
  For read-heavy workloads (90%+ reads), route heavy analytical and search queries to read-replicas, reserving the primary database instance for critical writes.
- **Rule 3 (Distributed Rate Limiting)**:
  Protect APIs against denial of service using distributed token-bucket rate limiting backed by Redis (e.g. `@upstash/ratelimit`):
  ```typescript
  import { Ratelimit } from '@upstash/ratelimit';
  import { Redis } from '@upstash/redis';

  const ratelimit = new Ratelimit({
    redis: Redis.fromEnv(),
    limiter: Ratelimit.slidingWindow(20, '10 s'),
  });
  ```
- **Rule 4 (Asynchronous Background Job Queues)**:
  Never execute long-running operations (> 2000ms) synchronously within an HTTP request lifecycle. Offload tasks to reliable event queues (Inngest, Temporal, BullMQ, AWS SQS).

## Decision Criteria
```text
IF user triggers an email or PDF generation:
  Enqueue background job and return HTTP 202 Accepted immediately.
IF database hits > 80% connection pool capacity:
  Enable serverless connection pooling proxy immediately.
IF query executes repeatedly with identical results:
  Cache query result in Redis / CDN Edge with strict TTL and invalidation tags.
```

## Recommended Workflow
1. Identify high-traffic endpoints and potential bottlenecks.
2. Implement connection pooling proxy on primary PostgreSQL database.
3. Wrap cacheable read operations in Redis or Next.js Data Cache.
4. Migrate synchronous email/webhook tasks to background queues.
5. Conduct load tests using k6 or Artillery to identify breaking thresholds.

## Best Practices
- Implement circuit breakers for flaky external third-party dependencies.
- Use exponential backoff with jitter when retrying failed network requests.
- Store static media assets on global object storage with CDN distribution (Cloudflare R2, AWS S3 + CloudFront).

## Anti-Patterns
- **The Synchronous Email Trap**: Sending a transactional welcome email inside the user signup request; if SMTP times out, signup fails.
- **Unbounded Database Connections**: Connecting directly from 500 serverless functions to a standard Postgres instance that supports max 100 connections.
- **Storing File Uploads on Local Server Disk**: Saving user avatars to local `/tmp` on serverless nodes, losing files on every container teardown.

## Validation Checklist
- [ ] Database uses connection pooling proxy in serverless environments.
- [ ] Long tasks are processed asynchronously via queues.
- [ ] High-volume read queries leverage multi-layer caching.
- [ ] Rate limiting is enforced across public endpoints.

## Related Skills
- `skills/architecture/backend-architecture.md`
- `skills/performance/caching.md`
- `skills/backend/database.md`

## References
- Designing Data-Intensive Applications — Martin Kleppmann
- AWS Well-Architected Framework: Reliability & Performance Pillars

## Last Reviewed
2026-09-15
