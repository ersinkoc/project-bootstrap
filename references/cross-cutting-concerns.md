# Cross-Cutting Concerns

Rules and patterns that span ALL generated skills. Every skill must honor these. When generating any skill, check this document for applicable cross-cutting rules and embed them.

---

## 1. Defense in Depth

No single skill "owns" security. Every skill must embed security thinking:

- `project-architecture`: Module isolation prevents blast radius
- `{language}-standards`: Type safety prevents type confusion attacks
- `data-validation`: Input validation prevents injection
- `error-handling`: Error messages don't leak internals
- `api-design`: Rate limiting, auth on every endpoint
- `database-design`: Parameterized queries, least privilege
- `ui-engineering`: XSS prevention, CSP compliance
- `testing-strategy`: Security test cases
- `devops-pipeline`: Security scanning in CI
- `observability`: Security event logging
- `privacy-compliance`: PII protection everywhere

**Rule**: Every generated skill must include at least one security-relevant rule, even if the domain seems non-security-related.

---

## 2. Error Handling Uniformity

All errors across the entire project must follow the same shape and propagation rules.

**Shared Error Contract** (adapt to language):

```
BaseError
├── code: string          # Structured code: DOMAIN_CATEGORY_SPECIFIC
├── message: string       # Human-readable, safe to show to users
├── statusCode: number    # HTTP status or equivalent exit code
├── isOperational: bool   # true = expected, false = programmer error
├── context: object       # Structured metadata for debugging
└── cause: Error?         # Original error (error chaining)
```

**Propagation rules** (every skill must respect these):
1. Catch at boundaries (API handler, job processor, UI error boundary)
2. Enrich with context at each layer (don't swallow, wrap and re-throw)
3. Log once at the boundary (not at every layer)
4. User-facing messages are generic ("Something went wrong") unless operational
5. Stack traces: full in development, omitted in production responses (but logged)

---

## 3. Logging Contract

All skills that touch logging must use the same format:

```json
{
  "timestamp": "2026-03-09T10:30:00.000Z",
  "level": "info",
  "message": "Short human-readable description",
  "service": "{service-name}",
  "requestId": "{correlation-id}",
  "userId": "{user-id-if-authenticated}",
  "action": "{what-is-happening}",
  "duration_ms": 42,
  "context": {}
}
```

**Rules all skills must follow**:
- NEVER log: passwords, tokens, API keys, credit cards, SSNs, raw PII
- ALWAYS log: requestId, action, duration for async operations
- INFO level for business events, ERROR for failures needing attention
- Structured (JSON) in production, human-readable in development

---

## 4. Validation Boundary

Validation happens at defined boundaries. All skills must agree on WHERE:

```
External Input → [VALIDATE HERE] → Trusted Internal Code → [VALIDATE HERE] → External Output

Boundaries that MUST validate:
1. API request handlers (validate body, params, query, headers)
2. Background job processors (validate job payload)
3. Event consumers (validate event payload)
4. File parsers (validate file content)
5. Configuration loaders (validate env vars at startup)
6. Database query results (validate shape if raw SQL)
7. External API responses (validate response shape)

Inside trusted code (between boundaries):
- NO redundant validation
- Types are the contract
- If the types are right, the data is right
```

---

## 5. Configuration Management

All skills that reference configuration must follow:

**Hierarchy** (later overrides earlier):
1. Defaults in code
2. Configuration file (e.g., `config.toml`, `settings.yaml`)
3. Environment variables
4. CLI arguments (if applicable)
5. Runtime overrides (feature flags)

**Rules**:
- ALL configuration validated at startup (fail fast, not at first use)
- Environment variables: UPPER_SNAKE_CASE with project prefix
- No secrets in configuration files (env vars or vault only)
- Configuration schema defined and versioned
- Different configs per environment (dev, staging, production)
- Sensitive values redacted in logs and error messages

---

## 6. Naming Consistency

All skills must use the same naming conventions. Define once in `project-architecture`, reference everywhere:

| Context | Convention | Example |
|---------|-----------|---------|
| Files (components) | {project convention} | `UserProfile.tsx` or `user-profile.tsx` |
| Files (utilities) | {project convention} | `formatDate.ts` or `format-date.ts` |
| Files (tests) | {project convention} | `UserProfile.test.tsx` |
| Functions | camelCase (JS/TS), snake_case (Python/Rust/Go) | `getUserById` / `get_user_by_id` |
| Classes/Types | PascalCase | `UserProfile`, `CreateUserInput` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Env vars | UPPER_SNAKE_CASE with prefix | `APP_DATABASE_URL` |
| Database tables | snake_case, plural | `user_profiles` |
| Database columns | snake_case | `created_at` |
| API endpoints | kebab-case, plural nouns | `/api/v1/user-profiles` |
| Event names | past-tense, dot-separated | `user.created`, `order.payment.failed` |
| Feature flags | kebab-case | `enable-new-checkout` |
| Git branches | type/description | `feat/user-profiles` |
| Commit messages | Conventional Commits | `feat(auth): add MFA support` |

---

## 7. Async/Concurrent Safety

All skills that touch async or concurrent code must follow:

**Rules**:
- Always handle promise rejections / async errors (no unhandled rejections)
- Always set timeouts on external calls (HTTP, DB, cache, queue)
- Always implement cancellation for long-running operations
- Always limit concurrency for batch operations (no unbounded Promise.all on 10K items)
- Always use structured concurrency where the language supports it
- Never hold locks across async boundaries (or across await points)
- Always implement graceful shutdown (finish in-flight work, stop accepting new work)

**Timeout defaults** (customize per project):
| Operation | Default Timeout | Max Timeout |
|-----------|----------------|-------------|
| HTTP request to external API | 5s | 30s |
| Database query | 5s | 30s |
| Cache operation | 1s | 5s |
| Background job | 5min | 30min |
| File upload processing | 30s | 5min |
| Health check | 2s | 5s |

---

## 8. Backward Compatibility

All skills that touch APIs, schemas, events, or configs must follow:

**Rules**:
- Adding a field: always safe (must have default or be optional)
- Removing a field: NEVER without deprecation period
- Renaming a field: treat as add new + deprecate old
- Changing a type: NEVER (add new field instead)
- Database migrations: must be backward compatible (old code works with new schema during rollout)
- API changes: version bump for breaking changes
- Event schema changes: must be backward compatible (consumers must handle old AND new)
- Config changes: old config format must still work (migration period)

**Deprecation process**:
1. Mark as deprecated (annotation/comment/header)
2. Log usage of deprecated feature (track adoption)
3. Communicate timeline (changelog, migration guide)
4. Remove after deprecation period (minimum 2 versions or 30 days)

---

## 9. Observability by Default

Every skill that produces running code should emit observability signals:

**Minimum signals per domain**:
| Domain | Logs | Metrics | Traces |
|--------|------|---------|--------|
| API endpoint | Request/response | Latency, status codes, throughput | Span per request |
| Database query | Slow queries (>100ms) | Query latency, pool utilization | Span per query |
| External API call | Request/response | Latency, error rate | Span per call |
| Background job | Start/complete/fail | Duration, queue depth, fail rate | Span per job |
| Cache operation | Miss only | Hit/miss ratio, latency | Span if >1ms |
| Auth event | All events | Success/failure rate | Part of request span |
| File operation | Upload/download | Size, duration | Span per operation |

---

## 10. Testing Taxonomy

All skills must know what kind of tests exist and when to write each:

| Test Type | What It Tests | Speed | Isolation | When to Write |
|-----------|--------------|-------|-----------|---------------|
| **Unit** | Single function/class | <10ms | Full (no I/O) | Every pure function, utility, validator |
| **Integration** | Module interactions | <1s | Partial (test DB OK) | API routes, service + DB, event handlers |
| **E2E** | Full user journey | <30s | None (real everything) | Critical flows only (signup, checkout, core feature) |
| **Contract** | API compatibility | <1s | Full (mock) | Every API boundary between services |
| **Visual** | UI appearance | <5s | Browser | Components with complex layouts |
| **Accessibility** | WCAG compliance | <5s | Browser | All interactive components |
| **Performance** | Speed/resource usage | Varies | Controlled | Critical paths, after optimization |
| **Load** | System under stress | Minutes | Staging env | Before launch, before scaling changes |
| **Security** | Vulnerability presence | <1s | Full | Auth flows, input handling, file uploads |
| **Mutation** | Test quality | Minutes | Full | Periodically, to verify test effectiveness |

---

## 11. Documentation Proximity

Documentation should live as close to the code it describes as possible:

| Documentation Type | Location | Format |
|-------------------|----------|--------|
| Function/method docs | In code (JSDoc/docstring) | Language-specific |
| Type/interface docs | In code | Language-specific |
| API endpoint docs | Generated from code + annotations | OpenAPI/Swagger |
| Component docs | Storybook / in code | Framework-specific |
| Architecture decisions | `docs/adr/` | ADR template |
| Setup guide | `README.md` | Markdown |
| Runbooks | `docs/runbooks/` | Markdown |
| API consumer guide | `docs/api/` or hosted | Generated + manual |
| Database schema | Generated from ORM/migrations | Diagram + text |

---

## 12. Graceful Degradation Priority

When things fail, degrade in this order (preserve what matters most):

1. **Data integrity**: NEVER compromise (transactions, constraints, backups)
2. **Security**: NEVER compromise (auth, encryption, validation)
3. **Privacy**: NEVER compromise (PII protection, consent, access control)
4. **Core functionality**: Preserve at all costs (user can do the main thing)
5. **Secondary features**: Can disable temporarily (analytics, recommendations, notifications)
6. **Performance**: Can slow down temporarily (disable cache, use sync fallbacks)
7. **Visual polish**: Can degrade (simpler UI, no animations, default avatars)
8. **Nice-to-haves**: Can remove entirely (onboarding tips, feature announcements)

Every skill should reference this priority when defining error recovery strategies.

---

## 13. Latest Stable Version Enforcement

All generated skills and the bootstrap process itself MUST use the latest stable versions of every technology.

**Rules**:
- NEVER hardcode or memorize version numbers — always verify via web search or documentation tools before proposing
- Use `WebSearch`, `WebFetch`, or Context7 (`resolve-library-id` + `query-docs`) to look up the current stable release of every technology
- Every version referenced in ANY generated skill must match the version confirmed during the tech stack phase
- If a version cannot be verified (tools unavailable), mark it explicitly: `⚠️ version unverified — confirm before use`
- Generated skills must include the verified version in the Project Context table
- When generating code examples, use syntax and APIs from the verified latest version (not deprecated patterns from older releases)
- If the latest version introduces breaking changes from a widely-known older API, document the new way explicitly

**Why**: AI models have a knowledge cutoff. Package ecosystems move fast. A skill generated with stale versions will produce outdated code, reference deprecated APIs, and miss security patches. Real-time verification eliminates this entirely.

**Verification sources (in priority order)**:
1. Official documentation site (e.g., nextjs.org, postgresql.org)
2. Package registry (npm, PyPI, crates.io, Maven Central, pkg.go.dev)
3. GitHub releases page
4. Context7 library documentation lookup

**What to verify for each technology**:
| Check | Example |
|-------|---------|
| Latest stable version | Next.js → 16.1.0 |
| Release date | 2026-02-15 |
| Node/runtime compatibility | Requires Node >= 22 |
| Major breaking changes from previous | App Router is now default |
| Deprecated features to avoid | `getServerSideProps` removed |
| New features to prefer | Server Actions stable |
