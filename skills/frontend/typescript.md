# Strict TypeScript Standards & Type Safety

## Purpose
Establishes unyielding type safety standards, eliminating runtime type errors, enforcing predictable contracts, and maximizing IDE code intelligence.

## When To Use
- In every TypeScript file across the frontend, backend, test suites, and configurations.
- When defining API request/response payloads, database schemas, and component prop types.

## When Not To Use
- Never. All project code must be strictly typed.

## Core Principles
1. **Zero Any Policy**: The `any` type is completely banned. Use `unknown` when types are truly dynamic, then narrow defensively.
2. **Single Source of Truth**: Define runtime validation schemas with Zod, then infer static TypeScript types (`z.infer<typeof Schema>`).
3. **Make Impossible States Unrepresentable**: Use discriminated union types to prevent conflicting or invalid state combinations.

## Rules
- **Rule 1 (Strict Compiler Settings)**:
  `tsconfig.json` must enforce:
  ```json
  {
    "compilerOptions": {
      "strict": true,
      "noImplicitAny": true,
      "strictNullChecks": true,
      "strictFunctionTypes": true,
      "noImplicitThis": true,
      "alwaysStrict": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true,
      "noImplicitReturns": true,
      "noFallthroughCasesInSwitch": true,
      "exactOptionalPropertyTypes": true
    }
  }
  ```
- **Rule 2 (Discriminated Unions for Async State)**:
  Never maintain loose booleans (`isLoading`, `isError`, `data`). Use discriminated unions:
  ```typescript
  type AsyncState<T> =
    | { status: 'idle' }
    | { status: 'loading' }
    | { status: 'success'; data: T }
    | { status: 'error'; error: Error };
  ```
- **Rule 3 (Type Narrowing Over Casting)**:
  Never use type assertions (`as MyType` or `as any`) to bypass compiler checks. Use custom type guards or Zod parsing:
  ```typescript
  function isApiError(error: unknown): error is { message: string; code: number } {
    return (
      typeof error === 'object' &&
      error !== null &&
      'message' in error &&
      typeof (error as Record<string, unknown>).message === 'string'
    );
  }
  ```
- **Rule 4 (Component Props Contracts)**:
  Component prop types must be explicitly declared as `interface` or `type`. Extend native HTML element attributes cleanly when creating design system primitives:
  ```typescript
  interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
    variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'destructive';
    size?: 'sm' | 'md' | 'lg';
    isLoading?: boolean;
  }
  ```

## Decision Criteria
```text
IF schema validation is needed at runtime (API boundaries, form inputs):
  Define Zod schema and export: type MyEntity = z.infer<typeof MyEntitySchema>;
IF data model is static domain logic:
  Define pure TypeScript interface.
IF handling third-party external JSON payload:
  Type incoming variable as unknown; validate with Zod safeParse before using.
```

## Recommended Workflow
1. Define shared schemas and interfaces in `@/types/` or adjacent to module logic.
2. Configure compiler options to strict mode in `tsconfig.json`.
3. Code functions and components with explicit parameter and return signatures.
4. Run `npx tsc --noEmit` locally and in CI to verify zero compilation errors.

## Best Practices
- Use `readonly` arrays and properties where state must not be mutated in-place.
- Use utility types (`Pick`, `Omit`, `Partial`, `Record`) to avoid duplicate type definitions.
- Leverage the `satisfies` operator to validate that an expression matches a type without widening it.

## Anti-Patterns
- **The Catch-All Any**: `function processData(data: any): any` (Defeats TypeScript entirely).
- **Type Casting Lies**: `const user = response.data as User;` (If API sends invalid shape, app crashes at runtime).
- **Optional Field Overuse**: Marking every field in an interface as optional (`?`), making consumer null-checks painful.

## Validation Checklist
- [ ] `tsc --noEmit` exits with status code 0.
- [ ] No instances of `any` exist in the codebase.
- [ ] No `@ts-ignore` or `@ts-nocheck` directives exist without documented exceptions.
- [ ] External data payloads are validated via Zod schemas.

## Related Skills
- `skills/frontend/component-architecture.md`
- `skills/backend/api-design.md`
- `skills/core/verification.md`

## References
- TypeScript Official Handbook — https://www.typescriptlang.org/docs/handbook/
- Matt Pocock’s Total TypeScript Best Practices — https://www.totaltypescript.com/

## Last Reviewed
2026-09-15
