# Effect.ts Integration: 100-Inference Roadmap

**Feature:** Effect.ts + Convex Components + Backend-Agnostic Frontend with Better Auth
**Version:** 1.0.0
**Status:** Planning Ready
**Stack:** Astro 5 + React 19 + Effect.ts + Convex Components + Better Auth + DataProvider
**Target:** Production-ready multi-tenant applications with functional error handling, dependency injection, and flexible backend support

---

## Overview

This roadmap sequences the complete integration of **Effect.ts** with the ONE Platform's existing architecture, combining:

1. **Effect.ts** - Functional error handling, dependency injection, composition
2. **Convex Components** - Agent, Workflow, RAG, Rate Limiting, Retrier, Workpool
3. **Better Auth** - Adapter-based authentication (Convex, WordPress, Notion, Supabase)
4. **DataProvider Interface** - Backend-agnostic frontend (Astro content → Effects services)
5. **6-Dimension Ontology** - Groups, People, Things, Connections, Events, Knowledge

### Why Effect.ts?

- **Type-safe error handling** - Error channels track all possible failures
- **Dependency injection** - Context and Layer patterns enable testability
- **Resource management** - Scoped resources with automatic cleanup
- **Composition** - Combine services without tight coupling
- **No lock-in** - Works with ANY backend via DataProvider

---

## Inference 1-10: Foundation & Setup

**1. [INFER-001]** Validate Effect.ts + DataProvider against 6-dimension ontology
   - Things: Services (Context.Tag definitions)
   - Connections: Service dependencies (Effect.provide)
   - Events: Error tracking and monitoring
   - Knowledge: Service documentation and schemas
   - Groups: Isolated Effect contexts per group
   - People: Role-based service access (Effect.guard)

**2. [INFER-002]** Map service architecture
   - LLMService (generative AI with providers)
   - ThingService (entity CRUD via DataProvider)
   - AuthService (Better Auth adapter abstraction)
   - WorkflowService (Convex workflow orchestration)
   - RAGService (retrieval-augmented generation)
   - MonitoringService (usage tracking and errors)

**3. [INFER-003]** List service dependencies
   - DataProvider (Convex, WordPress, Notion implementations)
   - Better Auth adapters (ConvexAdapter, WordPressAdapter, etc.)
   - Convex components (Agent, Workflow, RAG, etc.)
   - External services (OpenAI, Stripe, Sendgrid, etc.)

**4. [INFER-004]** Design error hierarchy
   - Domain errors (UserNotFound, ValidationError)
   - Infrastructure errors (AgentError, DatabaseError)
   - Authorization errors (UnauthorizedError, ForbiddenError)
   - Rate limit errors (RateLimitError, QuotaExceededError)
   - Tagged unions with full type safety

**5. [INFER-005]** Define service layers
   - Context Layer: Define service interfaces (Context.Tag)
   - Implementation Layer: Provide concrete implementations (Layer.effect)
   - Composition Layer: Combine services (Layer.merge)
   - Access Layer: Guard services by role/group

**6. [INFER-006]** Plan DataProvider connection to Effects
   - DataProvider<T> interface for backend agnosticism
   - Effect.tryPromise wrapper for async DataProvider calls
   - Service layer abstracts specific provider implementations
   - Frontend components use Effect services, not providers directly

**7. [INFER-007]** Identify Astro content integration points
   - Static content loaded via Astro collections
   - Effect services handle dynamic data fetching
   - Content connections (relationships between content)
   - Server-side rendering (SSR) with Effect context

**8. [INFER-008]** Design frontend component hierarchy
   - Islands: React components requiring hydration (client:load)
   - Layouts: Astro pages using Effect services server-side
   - Content: Static markdown with dynamic data augmentation
   - Services: Effect hooks for client-side state management

**9. [INFER-009]** Create implementation plan breakdown
   - Phase 1 (Infer 1-10): Foundation and design
   - Phase 2 (Infer 11-20): Core Effect services
   - Phase 3 (Infer 21-30): DataProvider implementations
   - Phase 4 (Infer 31-40): Better Auth integration
   - Phase 5 (Infer 41-50): Astro content integration
   - Phase 6 (Infer 51-60): Frontend component migration
   - Phase 7 (Infer 61-70): Convex component integration
   - Phase 8 (Infer 71-80): Testing and validation
   - Phase 9 (Infer 81-90): Performance optimization
   - Phase 10 (Infer 91-100): Documentation and deployment

**10. [INFER-010]** Assign specialists and dependencies
   - Backend specialist: Effect services, layers, error handling
   - Frontend specialist: Components, hooks, client state
   - Integration specialist: DataProvider, Better Auth, Convex components
   - Quality specialist: Testing, validation, monitoring
   - Documenter: Architecture, patterns, API docs

---

## Inference 11-20: Core Effect Services

**11. [INFER-011]** Design error classes and tagged unions
   - UserNotFoundError, ValidationError (domain)
   - AgentError, DatabaseError (infrastructure)
   - RateLimitError, QuotaExceededError (quota)
   - AuthorizationError, ForbiddenError (security)
   - All as Data.TaggedError for type safety

**12. [INFER-012]** Create service context definitions
   - `class ThingService extends Context.Tag("ThingService")`
   - `class AuthService extends Context.Tag("AuthService")`
   - `class WorkflowService extends Context.Tag("WorkflowService")`
   - `class RAGService extends Context.Tag("RAGService")`
   - Each with typed interface and operations

**13. [INFER-013]** Implement ThingService (CRUD via DataProvider)
   - create: (groupId, type, data) → Effect<Id, ValidationError>
   - getById: (groupId, id) → Effect<Thing, NotFoundError>
   - list: (groupId, type, filter) → Effect<Thing[], DatabaseError>
   - update: (groupId, id, data) → Effect<Thing, NotFoundError | ValidationError>
   - delete: (groupId, id) → Effect<void, NotFoundError>

**14. [INFER-014]** Create ThingService layer implementations
   - ConvexThingServiceLive (via Convex API)
   - WordPressThingServiceLive (via WordPress REST API)
   - NotionThingServiceLive (via Notion API)
   - CompositeThingServiceLive (multi-provider fallback)

**15. [INFER-015]** Implement AuthService with Better Auth
   - signUp: (email, password) → Effect<User, ValidationError>
   - signIn: (email, password) → Effect<Session, InvalidCredentialsError>
   - signOut: (sessionId) → Effect<void>
   - getCurrentUser: () → Effect<User | null>
   - changePassword: (oldPassword, newPassword) → Effect<void>

**16. [INFER-016]** Create AuthService layer with Better Auth adapters
   - ConvexAuthServiceLive (via ConvexAdapter)
   - WordPressAuthServiceLive (via WordPressAdapter)
   - NotionAuthServiceLive (via NotionAdapter)
   - SupabaseAuthServiceLive (via Drizzle adapter)

**17. [INFER-017]** Implement WorkflowService (Convex workflows)
   - execute: <A>(workflow, args) → Effect<A, ExecutionError>
   - getStatus: (runId) → Effect<WorkflowStatus>
   - cancel: (runId) → Effect<void>
   - list: (filter) → Effect<Workflow[]>

**18. [INFER-018]** Implement RAGService (retrieval + augmentation)
   - addDocument: (namespace, content) → Effect<DocumentId, IndexingError>
   - search: (namespace, query, limit) → Effect<SearchResults, SearchError>
   - delete: (namespace, docId) → Effect<void>
   - listNamespaces: () → Effect<Namespace[]>

**19. [INFER-019]** Implement MonitoringService (observability)
   - trackUsage: (data) → Effect<void>
   - trackError: (error) → Effect<void>
   - getMetrics: (groupId, period) → Effect<Metrics>
   - reportHealth: () → Effect<HealthStatus>

**20. [INFER-020]** Write unit tests for all service interfaces
   - Mock implementations for testing
   - Verify Effect type signatures
   - Test error handling paths
   - Benchmark Effect overhead

---

## Inference 21-30: DataProvider Implementations

**21. [INFER-021]** Design DataProvider<T> generic interface
   - T = resource type (Thing, User, Document, etc.)
   - Methods: create, read, update, delete, list
   - Returns: Effect<T, DataProviderError>
   - Handles provider-specific implementation details

**22. [INFER-022]** Implement ConvexProvider
   - Wrap ConvexHttpClient in DataProvider interface
   - Convert Convex queries/mutations to Effect
   - Handle Convex-specific errors
   - Support streaming responses

**23. [INFER-023]** Implement WordPressProvider
   - Use WordPress REST API
   - Map custom post types to Thing types
   - Handle WordPress authentication (JWT tokens)
   - Cache responses efficiently

**24. [INFER-024]** Implement NotionProvider
   - Use Notion API for database access
   - Map properties to Thing properties
   - Handle rich text content
   - Support filtering and sorting

**25. [INFER-025]** Implement SupabaseProvider
   - Use Supabase PostgREST API
   - Leverage Supabase Auth for sessions
   - Support real-time subscriptions
   - Handle RLS (Row-Level Security)

**26. [INFER-026]** Create provider factory pattern
   - Detect provider from environment/config
   - Return appropriate DataProvider implementation
   - Support provider switching at runtime
   - Fallback to local mock provider in tests

**27. [INFER-027]** Add provider detection and initialization
   - Read GROUP_PROVIDER config
   - Load provider-specific credentials
   - Initialize provider client
   - Validate connectivity

**28. [INFER-028]** Implement provider error mapping
   - Map provider-specific errors to domain errors
   - Standardize error messages
   - Preserve error context
   - Enable provider-agnostic error handling

**29. [INFER-029]** Write provider integration tests
   - Test with actual providers (integration env)
   - Test error scenarios
   - Test performance
   - Test provider switching

**30. [INFER-030]** Document provider integration guide
   - How to add new provider
   - Provider-specific configuration
   - Supported features per provider
   - Troubleshooting guide

---

## Inference 31-40: Better Auth Integration

**31. [INFER-031]** Create Better Auth adapter factory
   - Detect provider from environment
   - Return appropriate adapter (ConvexAdapter, WordPressAdapter, etc.)
   - Support dynamic adapter selection
   - Handle adapter switching

**32. [INFER-032]** Implement ConvexAdapter for Better Auth
   - Implement DatabaseAdapter interface
   - Wrap Convex mutations/queries
   - Handle authentication state
   - Support sessions and tokens

**33. [INFER-033]** Implement WordPressAdapter for Better Auth
   - Map WordPress tables to Better Auth models
   - Use WordPress REST API
   - Handle JWT token management
   - Support WordPress user roles

**34. [INFER-034]** Implement NotionAdapter for Better Auth
   - Use Notion database for users
   - Map properties to Better Auth schema
   - Handle token storage
   - Support email as unique identifier

**35. [INFER-035]** Wrap Better Auth with Effect service
   - AuthService uses Better Auth internally
   - Effect.tryPromise for async operations
   - Custom error types (not Better Auth's)
   - Session management via Effect context

**36. [INFER-036]** Implement multi-provider auth flow
   - Detect provider from request
   - Use appropriate adapter
   - Share auth state across providers
   - Support OAuth with any provider

**37. [INFER-037]** Add role-based access control (RBAC) to Effect
   - Effect.guard for role checks
   - Service access controlled by role
   - Audit logging for access attempts
   - Fine-grained permission model

**38. [INFER-038]** Implement session management
   - Create/verify sessions
   - Handle session expiration
   - Support cross-tab invalidation
   - Secure cookie handling

**39. [INFER-039]** Add 2FA and passkey support
   - TOTP implementation
   - Passkey registration and verification
   - Recovery codes
   - Device trust

**40. [INFER-040]** Write auth integration tests
   - Test all adapters
   - Test sign-up flow
   - Test sign-in with different providers
   - Test RBAC enforcement

---

## Inference 41-50: Astro Content Integration

**41. [INFER-041]** Design content collection schema
   - Astro content collections for blog, docs, courses
   - Markdown with frontmatter
   - Dynamic data augmentation via Effect services
   - Content versioning and drafts

**42. [INFER-042]** Create content loader service
   - Load Astro collections server-side
   - Enrich with dynamic data (views, comments, related)
   - Cache content metadata
   - Index content for search

**43. [INFER-043]** Implement content layout wrapper
   - Wrap content in Effect context
   - Provide ThingService, RAGService, etc.
   - Handle SSR rendering
   - Client-side hydration for interactive parts

**44. [INFER-044]** Create content connection service
   - Track relationships between content pieces
   - Generate "related articles"
   - Build knowledge graph
   - Support backlinking

**45. [INFER-045]** Implement content search via RAG
   - Index all content into RAG namespace
   - Provide semantic search
   - Show similar content
   - Power recommendation engine

**46. [INFER-046]** Add content metrics tracking
   - Track views, reads, shares
   - Measure engagement
   - Identify trending content
   - Monitor content performance

**47. [INFER-047]** Create content versioning system
   - Store content history
   - Support rollback to previous versions
   - Track authorship and changes
   - Show change diffs

**48. [INFER-048]** Implement content collaboration features
   - Comments on content
   - Collaborative editing (if needed)
   - Content approvals/reviews
   - Feedback collection

**49. [INFER-049]** Add dynamic content rendering
   - Support embedded components in markdown
   - Run Effect operations during rendering
   - Inject dynamic data into static pages
   - Cache rendered output

**50. [INFER-050]** Test content integration end-to-end
   - SSR rendering with Effect context
   - Dynamic data injection
   - Search and discovery
   - Performance benchmarks

---

## Inference 51-60: Frontend Component Migration

**51. [INFER-051]** Create Effect-based React hooks
   - useEffect service (like React's useEffect but for Effect)
   - useThingService for CRUD operations
   - useAuthService for authentication
   - useWorkflow for long-running operations

**52. [INFER-052]** Implement useThingService hook
   - Load things by ID or filter
   - Create/update/delete things
   - Handle loading/error/success states
   - Optimize re-renders

**53. [INFER-053]** Implement useAuthService hook
   - Get current user
   - Sign up / sign in / sign out
   - Update profile
   - Change password

**54. [INFER-054]** Create component integration layer
   - Wrap Effect context in React Context
   - Provide services to component tree
   - Handle context cleanup
   - Support multiple Effect scopes

**55. [INFER-055]** Migrate dashboard components
   - Rewrite dashboard with Effect hooks
   - Replace Convex useQuery with useThingService
   - Add error boundaries
   - Maintain feature parity

**56. [INFER-056]** Migrate auth components
   - Rewrite auth pages with useAuthService
   - Support all auth methods (email, OAuth, passkey)
   - Handle errors gracefully
   - Show loading states

**57. [INFER-057]** Migrate form components
   - Validation with Effect
   - Error messaging
   - Loading during submission
   - Success/failure handling

**58. [INFER-058]** Create component design tokens
   - Color system
   - Typography
   - Spacing
   - Shadows and effects

**59. [INFER-059]** Add accessibility to all components
   - ARIA labels
   - Keyboard navigation
   - Focus management
   - Color contrast

**60. [INFER-060]** Write component tests
   - Unit tests with mocked services
   - Integration tests with test Effect layers
   - E2E tests with real services
   - Visual regression tests

---

## Inference 61-70: Convex Component Integration

**61. [INFER-061]** Wrap Convex Agent component with Effect
   - AgentService uses Agent internally
   - Effect.tryPromise for async operations
   - Thread management via Effect
   - Tool definition with Effect context

**62. [INFER-062]** Implement effect-based tool definitions
   - Tools as Effect services
   - Dependency injection for tool services
   - Error handling within tools
   - Tool validation and schema

**63. [INFER-063]** Integrate Convex Workflow component
   - WorkflowService wraps Workflow component
   - Each step uses Effect context
   - Error recovery with retries
   - Long-running operation tracking

**64. [INFER-064]** Implement Convex RAG component
   - RAGService wraps RAG component
   - Effect-based chunking and embedding
   - Vector search with Effects
   - Semantic search integration

**65. [INFER-065]** Integrate rate limiting
   - RateLimitService wraps rate-limiter component
   - Per-user, per-group, per-API limits
   - Graceful degradation when limited
   - Monitoring and alerting

**66. [INFER-066]** Add retry logic with Effect
   - Exponential backoff schedules
   - Conditional retries (retry on specific errors)
   - Maximum retry attempts
   - Retry metrics tracking

**67. [INFER-067]** Implement workpool for background jobs
   - TaskQueueService wraps workpool
   - Priority-based task execution
   - Task status tracking
   - Completion callbacks

**68. [INFER-068]** Create monitoring and observability
   - Track all Effect operations
   - Measure performance
   - Log errors to external service
   - Export metrics for dashboards

**69. [INFER-069]** Write component integration tests
   - Test agent with tools
   - Test workflows with multiple steps
   - Test RAG search accuracy
   - Test rate limiting enforcement

**70. [INFER-070]** Benchmark component performance
   - Measure Effect overhead
   - Compare to direct Convex calls
   - Optimize critical paths
   - Set performance targets

---

## Inference 71-80: Testing & Validation

**71. [INFER-071]** Create test Effect layers
   - Mock ThingService for testing
   - Mock AuthService for testing
   - Mock WorkflowService for testing
   - Mock all external dependencies

**72. [INFER-072]** Write unit tests for Effect services
   - Test happy path
   - Test error scenarios
   - Test error recovery
   - Test service composition

**73. [INFER-073]** Write integration tests
   - Test with real DataProvider
   - Test with real Better Auth adapter
   - Test multi-service workflows
   - Test error propagation

**74. [INFER-074]** Write end-to-end tests
   - Test full user flows
   - Test across multiple services
   - Test error handling
   - Test performance

**75. [INFER-075]** Validate ontology mapping
   - Things: All service types represented
   - Connections: Service dependencies tracked
   - Events: All operations logged
   - Knowledge: Service schemas documented
   - Groups: Multi-tenant isolation verified
   - People: Role-based access enforced

**76. [INFER-076]** Run type safety checks
   - bunx astro check (frontend)
   - npx convex dev (backend types)
   - No `any` types except in entity properties
   - Full TypeScript strict mode

**77. [INFER-077]** Check code quality
   - ESLint passes
   - Prettier formatting
   - No dead code
   - No unused imports

**78. [INFER-078]** Measure test coverage
   - Unit test coverage > 85%
   - Integration test coverage > 70%
   - Critical paths 100% coverage
   - Generate coverage reports

**79. [INFER-079]** Performance testing
   - Measure service call latency
   - Benchmark DataProvider performance
   - Test under load
   - Profile memory usage

**80. [INFER-080]** Security testing
   - Validate auth flows
   - Test RBAC enforcement
   - Check for injection vulnerabilities
   - Verify data isolation

---

## Inference 81-90: Performance & Optimization

**81. [INFER-081]** Optimize service layer caching
   - Cache service results in-memory
   - Implement cache invalidation
   - TTL-based cache expiration
   - Cache hit/miss metrics

**82. [INFER-082]** Optimize DataProvider queries
   - Batch queries where possible
   - Implement query deduplication
   - Lazy load related data
   - Minimize round-trips

**83. [INFER-083]** Add connection pooling
   - Pool connections to external services
   - Reuse connections efficiently
   - Implement backoff for failures
   - Monitor pool health

**84. [INFER-084]** Optimize React rendering
   - Use React.memo for components
   - Implement virtual lists for large data
   - Optimize re-render patterns
   - Profile with React DevTools

**85. [INFER-085]** Optimize bundle size
   - Code split by route
   - Lazy load heavy components
   - Tree-shake unused code
   - Compress assets

**86. [INFER-086]** Implement streaming responses
   - Stream large data responses
   - Use HTTP/2 push
   - Support response resumption
   - Optimize for slow networks

**87. [INFER-087]** Add server-side caching headers
   - Set appropriate cache control
   - Use ETags for validation
   - Leverage CDN caching
   - Invalidate cache on updates

**88. [INFER-088]** Optimize database queries
   - Add indexes to frequently queried fields
   - Use pagination for large result sets
   - Avoid N+1 queries
   - Use connection pooling

**89. [INFER-089]** Monitor performance metrics
   - Track LCP, FID, CLS
   - Monitor API response times
   - Track error rates
   - Set up performance budgets

**90. [INFER-090]** Establish performance baselines
   - Measure baseline performance
   - Set performance targets
   - Track performance over time
   - Alert on regressions

---

## Inference 91-100: Deployment & Documentation

**91. [INFER-091]** Prepare for production deployment
   - Set up CI/CD pipeline
   - Configure environment variables
   - Set up secrets management
   - Plan rollout strategy

**92. [INFER-092]** Deploy backend services
   - Deploy Convex backend
   - Deploy Better Auth service
   - Deploy DataProvider implementations
   - Test connectivity

**93. [INFER-093]** Deploy frontend
   - Build Astro production bundle
   - Deploy to Cloudflare Pages
   - Configure CDN
   - Set up redirects

**94. [INFER-094]** Run smoke tests in production
   - Test critical user flows
   - Verify all services connected
   - Check performance metrics
   - Monitor error rates

**95. [INFER-095]** Write architecture documentation
   - System design overview
   - Service dependencies diagram
   - DataProvider abstraction
   - Error handling patterns

**96. [INFER-096]** Write API documentation
   - Service interface docs
   - Effect context documentation
   - Error types reference
   - Code examples

**97. [INFER-097]** Create implementation guides
   - How to add new service
   - How to add new DataProvider
   - How to add new component
   - How to test locally

**98. [INFER-098]** Capture lessons learned
   - What worked well
   - What was challenging
   - Design decisions and tradeoffs
   - Future improvements

**99. [INFER-099]** Update knowledge base
   - Add to `/one/knowledge/`
   - Document patterns discovered
   - Update ontology mapping
   - Archive old patterns

**100. [INFER-100]** Mark complete and plan next phase
   - Update feature status
   - Notify stakeholders
   - Share metrics and learnings
   - Plan follow-up features

---

## Architecture Overview

```
┌────────────────────────────────────────────────────────────┐
│                   Astro + React Frontend                    │
│  - Pages with SSR and Effect context                       │
│  - Components with useThingService, useAuthService         │
│  - Client islands with selective hydration                 │
└────────────────────┬─────────────────────────────────────┘
                     │
        ┌────────────┴────────────┐
        │                         │
        ↓                         ↓
   Effect Services         Effect Services
   (Server-side)           (Client-side)
   ├─ ThingService         ├─ useThingService
   ├─ AuthService          ├─ useAuthService
   ├─ WorkflowService      ├─ useWorkflow
   ├─ RAGService           └─ useMonitoring
   └─ MonitoringService
        │
        ├─ Layer.merge(ThingServiceLive, AuthServiceLive, ...)
        │
        └─ Effect.provide() → Effect.runPromise()
                    │
        ┌───────────┼───────────┐
        │           │           │
        ↓           ↓           ↓
   DataProvider  Better Auth  Convex
   Interface     Adapters     Components
   ├─ Convex     ├─ Convex    ├─ Agent
   ├─ WordPress  ├─ WordPress ├─ Workflow
   ├─ Notion     ├─ Notion    ├─ RAG
   └─ Supabase   └─ Supabase  ├─ Rate Limiter
                              └─ Workpool
```

---

## Service Composition Example

```typescript
// Define services with Context.Tag
class ThingService extends Context.Tag("ThingService")<...> {}
class AuthService extends Context.Tag("AuthService")<...> {}
class MonitoringService extends Context.Tag("MonitoringService")<...> {}

// Implement with layers
const ThingServiceLive = Layer.effect(ThingService, Effect.gen(function* () { ... }))
const AuthServiceLive = Layer.effect(AuthService, Effect.gen(function* () { ... }))
const MonitoringServiceLive = Layer.succeed(MonitoringService, { ... })

// Compose in business logic
const createEntity = action({
  handler: (ctx, args) =>
    Effect.gen(function* () {
      const things = yield* ThingService
      const auth = yield* AuthService
      const monitoring = yield* MonitoringService

      // Use services
      const entity = yield* things.create(...)
      yield* monitoring.trackUsage(...)

      return entity
    }).pipe(
      Effect.provide(Layer.merge(ThingServiceLive, AuthServiceLive, MonitoringServiceLive)),
      Effect.catchAll((error) => /* handle errors */)
    )
})
```

---

## DataProvider Pattern

```typescript
// Generic DataProvider interface
interface DataProvider<T> {
  create: (data: Partial<T>) => Effect<T, CreateError>
  getById: (id: string) => Effect<T, NotFoundError>
  list: (filter?: Filter) => Effect<T[], ListError>
  update: (id: string, data: Partial<T>) => Effect<T, UpdateError>
  delete: (id: string) => Effect<void, DeleteError>
}

// Implementations are provider-specific
class ConvexProvider<T> implements DataProvider<T> { ... }
class WordPressProvider<T> implements DataProvider<T> { ... }
class NotionProvider<T> implements DataProvider<T> { ... }

// Select provider based on config
const getProvider = <T>(provider: string): DataProvider<T> => {
  switch (provider) {
    case "convex": return new ConvexProvider<T>(...)
    case "wordpress": return new WordPressProvider<T>(...)
    case "notion": return new NotionProvider<T>(...)
  }
}
```

---

## Error Handling Pattern

```typescript
// Typed errors with Data.TaggedError
class NotFoundError extends Data.TaggedError("NotFoundError")<{
  entityType: string
  id: string
}> {}

class ValidationError extends Data.TaggedError("ValidationError")<{
  field: string
  message: string
}> {}

// Effect guarantees error handling
const createEntity = (data: any): Effect<Entity, ValidationError | CreateError> =>
  Effect.gen(function* () {
    // Errors propagate automatically
    const validated = yield* validate(data) // Error type tracked
    const entity = yield* thingService.create(validated) // Error type tracked
    return entity
  }).pipe(
    // Handle specific error types
    Effect.catchTag("ValidationError", (error) => {
      console.error(`Validation failed on ${error.field}: ${error.message}`)
      return Effect.fail(error)
    }),
    // Handle other errors
    Effect.catchAll((error) => {
      console.error("Unexpected error:", error)
      return Effect.fail(new CreateError({ cause: error }))
    })
  )
```

---

## Ontology Mapping Reference

**Things Dimension:**
- Service definitions as things (type="service")
- Configuration entities (type="backend_config")
- API connection things (type="api_connection")

**Connections Dimension:**
- service_depends_on (service A uses service B)
- group_uses_provider (group configured with provider)
- service_provides (service provides interface)

**Events Dimension:**
- service_started, service_failed
- provider_initialized, provider_switched
- auth_flow_completed, role_granted

**Knowledge Dimension:**
- Service documentation (embeddings)
- Error catalog
- Pattern library
- Performance benchmarks

**Groups Dimension:**
- Each group has isolated Effect context
- Group-specific service configuration
- Provider selection per group

**People Dimension:**
- User roles affect service access
- Effect.guard for authorization
- Audit trail of who did what

---

## How to Use This Roadmap

### Sequential Execution
- Always do the next inference in sequence
- Each builds on previous work
- Dependencies are implicit in ordering

### Parallel Opportunities
After Infer 20 (services defined):
- **Backend:** Infer 21-30 (DataProvider) can run in parallel with Infer 31-40 (Better Auth)
- **Frontend:** Infer 41-50 (content) and Infer 51-60 (components) can run in parallel

After Infer 60 (components ready):
- **Integration:** Infer 61-70 (Convex components) can proceed independently
- **Testing:** Infer 71-80 can start concurrent with component work

### Quality Gates

- **Gate 1 (Infer 20):** All core services defined and tested
- **Gate 2 (Infer 40):** DataProvider + Auth working with multiple backends
- **Gate 3 (Infer 60):** All frontend components migrated to Effect services
- **Gate 4 (Infer 80):** Full test suite passing, 85%+ coverage
- **Gate 5 (Infer 100):** Production deployment complete, monitoring active

---

## Specialist Assignments

| Inferences | Specialist | Focus | Duration |
|-----------|-----------|-------|----------|
| 1-10 | Director | Planning and architecture | 1 week |
| 11-30 | Backend | Core services + DataProvider | 3 weeks |
| 31-40 | Integrator | Better Auth + adapters | 2 weeks |
| 41-50 | Frontend | Astro content integration | 2 weeks |
| 51-60 | Frontend | Component migration | 2 weeks |
| 61-70 | Integrator | Convex component wrapping | 2 weeks |
| 71-80 | Quality | Testing and validation | 2 weeks |
| 81-90 | Backend | Performance optimization | 1 week |
| 91-100 | Documenter | Deployment and docs | 1 week |

---

## Key Files to Create/Modify

**New:**
- `/backend/convex/services/` - Effect service definitions
- `/backend/convex/services/layers.ts` - Layer compositions
- `/backend/convex/domain/` - Domain logic with Effect
- `/web/src/providers/DataProvider.ts` - Generic interface
- `/web/src/providers/implementations/` - ConvexProvider, WordPress, etc.
- `/web/src/lib/hooks/` - useThingService, useAuthService, etc.
- `/web/src/lib/effects/` - Effect context and utilities
- `/one/things/todo-effects.md` - This file
- `/one/knowledge/effect-patterns.md` - Pattern documentation

**Modified:**
- `/backend/convex/schema.ts` - Add service metadata
- `/web/src/pages/` - Use Effect services in layouts
- `/web/src/components/` - Use Effect hooks
- `/web/src/auth/` - Integrate Better Auth adapters

---

## Success Metrics

- **Code Quality:** 100% TypeScript strict mode, 0 `any` types
- **Type Safety:** Full Effect error channel tracking
- **Test Coverage:** 85%+ unit, 70%+ integration
- **Performance:** <50ms average service call latency
- **Backend Flexibility:** 3+ providers (Convex, WordPress, Notion) working
- **Auth Methods:** 6+ methods supported (email, OAuth, passkey, 2FA, etc.)
- **Ontology Compliance:** All 6 dimensions properly mapped
- **Documentation:** Complete API docs, architecture guide, implementation guide

---

**Built with clarity, simplicity, and infinite scale in mind.**

_— ONE Platform Team_
