# Migration Guide: 4-Table → 6-Dimension Ontology

**Version:** 1.0.0
**Date:** 2025-10-10
**Status:** Active

---

## Overview

This guide helps you migrate from the old "4-table ontology" terminology to the new "6-dimension ontology" architecture.

**IMPORTANT:** The database schema has NOT fundamentally changed. This is primarily a documentation and conceptual upgrade that makes Organizations and People explicit first-class dimensions.

### What Changed and Why

**Old Model:** "4-table ontology" (things, connections, events, tags/knowledge)
- Implied organizations and people existed
- No explicit multi-tenant isolation
- Authorization was ad-hoc
- Hard to explain to children or enterprises

**New Model:** "6-dimension ontology" (organizations, people, things, connections, events, knowledge)
- Explicit multi-tenant isolation via Organizations
- Clear authorization model via People
- Scales from children's lemonade stands to enterprise SaaS
- AI agents can reason about complete reality model

**Why This Matters:**
- **For Children:** "I own a lemonade stand (Organization), I'm the owner (Person), I sell lemonade (Things), customers buy it (Connections), sales happen (Events), and I learn what works (Knowledge)"
- **For Enterprises:** Multi-tenant SaaS platforms with clear ownership, governance, data isolation, and intelligence

---

## What Changed

### 1. Terminology

| Old Term | New Term | Description |
|----------|----------|-------------|
| 4-table ontology | 6-dimension ontology | Complete reality model |
| Things, connections, events, knowledge | Organizations, people, things, connections, events, knowledge | All six dimensions explicit |
| Implicit org context | Explicit organizationId | Every resource belongs to an org |
| Ad-hoc authorization | Role-based people dimension | Clear permission hierarchy |

### 2. Conceptual Model

**Old:**
```
Things → Connections → Events → Knowledge
```

**New:**
```
Organizations (isolation boundary)
    ↓
People (authorization & governance)
    ↓
Things (what exists)
    ↓
Connections (how they relate)
    ↓
Events (what happened)
    ↓
Knowledge (what it means)
```

### 3. Database Schema Changes

**Added:**
- `organizations` table (explicit first-class dimension)
- `organizationId` field on all tables (for multi-tenant scoping)
- `actorId` required on events (always references a person/thing)
- Org-scoped indexes: `by_org`, `by_org_type`

**People Representation:**
- People are `things` with `type: 'creator'` and `properties.role`
- No separate `people` table needed (avoids duplication)
- Roles: `platform_owner`, `org_owner`, `org_user`, `customer`

**Schema Updates:**

```typescript
// Organizations table (new first-class dimension)
organizations: defineTable({
  name: v.string(),
  slug: v.string(),
  status: v.string(),
  plan: v.string(),
  limits: v.any(),
  usage: v.any(),
  billing: v.any(),
  settings: v.any(),
  createdAt: v.number(),
  updatedAt: v.number(),
})
  .index("by_slug", ["slug"])
  .index("by_status", ["status"]),

// Things table (updated with org scoping)
things: defineTable({
  thingType: v.string(),
  name: v.string(),
  organizationId: v.id("organizations"),  // NEW: Every thing belongs to an org
  description: v.optional(v.string()),
  properties: v.any(),
  status: v.string(),
  createdAt: v.number(),
  updatedAt: v.number(),
})
  .index("by_type", ["thingType"])
  .index("by_org", ["organizationId"])  // NEW: Query things by org
  .index("by_org_type", ["organizationId", "thingType"]),  // NEW: Efficient org + type queries

// Connections table (updated with org scoping)
connections: defineTable({
  fromThingId: v.id("things"),
  toThingId: v.id("things"),
  relationshipType: v.string(),
  organizationId: v.id("organizations"),  // NEW: Connections scoped to org
  metadata: v.any(),
  createdAt: v.number(),
})
  .index("by_from", ["fromThingId"])
  .index("by_to", ["toThingId"])
  .index("by_org", ["organizationId"])  // NEW: Query connections by org
  .index("by_relationship", ["relationshipType"]),

// Events table (updated with org scoping and actor)
events: defineTable({
  thingId: v.optional(v.id("things")),
  eventType: v.string(),
  actorId: v.id("things"),  // REQUIRED: Actor is always a person (thing with role)
  organizationId: v.id("organizations"),  // NEW: Events scoped to org
  metadata: v.any(),
  timestamp: v.number(),
})
  .index("by_thing", ["thingId"])
  .index("by_actor", ["actorId"])  // NEW: Query by who did it
  .index("by_org", ["organizationId"])  // NEW: Query events by org
  .index("by_type", ["eventType"])
  .index("by_timestamp", ["timestamp"]),

// Knowledge table (updated with org scoping)
knowledge: defineTable({
  knowledgeType: v.string(),
  text: v.optional(v.string()),
  embedding: v.optional(v.array(v.number())),
  embeddingModel: v.optional(v.string()),
  embeddingDim: v.optional(v.number()),
  sourceThingId: v.optional(v.id("things")),
  sourceField: v.optional(v.string()),
  organizationId: v.id("organizations"),  // NEW: Knowledge scoped to org
  chunk: v.optional(v.any()),
  labels: v.optional(v.array(v.string())),
  metadata: v.optional(v.any()),
  createdAt: v.number(),
  updatedAt: v.number(),
})
  .index("by_type", ["knowledgeType"])
  .index("by_source", ["sourceThingId"])
  .index("by_org", ["organizationId"])  // NEW: Query knowledge by org
  .index("by_created", ["createdAt"]),
```

---

## Migration Checklist

### For Developers

- [ ] Read updated [ontology.md](./ontology.md) specification
- [ ] Read updated [architecture.md](../things/architecture.md) for schema changes
- [ ] Update all queries to include `organizationId` filter
- [ ] Add authorization checks using `requireOrgAccess()` middleware
- [ ] Update frontend components to use organization context
- [ ] Test multi-tenant data isolation thoroughly
- [ ] Update API routes with org scoping
- [ ] Add `actorId` to all event logging
- [ ] Update tests to cover org isolation and authorization

### For AI Agents

- [ ] Update system prompts to reference "6 dimensions" instead of "4 tables"
- [ ] Include organizations and people in feature mapping
- [ ] Generate org-scoped queries by default
- [ ] Include authorization checks in all mutations
- [ ] Log events with `actorId` (who performed the action)
- [ ] Check organization context before querying data
- [ ] Generate role-based permission checks

### For Documentation

- [ ] Search and replace "4-table" → "6-dimension" in all docs
- [ ] Update diagrams to show all 6 dimensions
- [ ] Add examples showing organizations and people dimensions
- [ ] Update README files with new architecture
- [ ] Create examples for both children and enterprises
- [ ] Update API documentation with org parameters
- [ ] Document role-based authorization patterns

---

## Code Changes

### 1. Query Patterns

#### Old Query Pattern (4-Table)

```typescript
// Get all blog posts (NO org scoping)
const posts = await ctx.db
  .query("things")
  .withIndex("by_type", (q) => q.eq("thingType", "blog_post"))
  .collect();

// PROBLEM: Returns posts from ALL organizations!
// No multi-tenant isolation
// Security risk: users can see other orgs' data
```

#### New Query Pattern (6-Dimension)

```typescript
// Get all blog posts in organization (WITH org scoping)
const posts = await ctx.db
  .query("things")
  .withIndex("by_org_type", (q) =>
    q.eq("organizationId", orgId)
     .eq("thingType", "blog_post")
  )
  .collect();

// BENEFIT: Returns only posts from specified organization
// Perfect multi-tenant isolation
// Optimized index: by_org_type
```

### 2. Mutation Patterns

#### Old Mutation Pattern (4-Table)

```typescript
export const createBlogPost = mutation({
  args: {
    title: v.string(),
    content: v.string(),
  },
  handler: async (ctx, args) => {
    // NO authorization check
    // NO organization scoping

    const postId = await ctx.db.insert("things", {
      thingType: "blog_post",
      name: args.title,
      properties: { content: args.content },
      status: "draft",
      createdAt: Date.now(),
      updatedAt: Date.now(),
    });

    // NO event logging
    return postId;
  },
});

// PROBLEMS:
// 1. No authorization check
// 2. No organization scoping
// 3. No audit trail (events)
// 4. Can't track who created the post
```

#### New Mutation Pattern (6-Dimension)

```typescript
export const createBlogPost = mutation({
  args: {
    orgId: v.id("organizations"),  // NEW: Require organization context
    title: v.string(),
    content: v.string(),
  },
  handler: async (ctx, args) => {
    // NEW: Check authorization
    await requireOrgAccess(ctx, args.orgId, "org_user");

    // NEW: Get current user (actor)
    const userId = await getUserId(ctx);

    // NEW: Scope to organization
    const postId = await ctx.db.insert("things", {
      thingType: "blog_post",
      name: args.title,
      organizationId: args.orgId,  // NEW: Org scoping
      properties: { content: args.content },
      status: "draft",
      createdAt: Date.now(),
      updatedAt: Date.now(),
    });

    // NEW: Log event with actor
    await ctx.db.insert("events", {
      eventType: "content_created",
      actorId: userId,  // NEW: Who did it
      thingId: postId,
      organizationId: args.orgId,  // NEW: Org scoping
      timestamp: Date.now(),
      metadata: { contentType: "blog_post" },
    });

    return postId;
  },
});

// BENEFITS:
// 1. Authorization check ensures user has access
// 2. Organization scoping prevents data leaks
// 3. Complete audit trail (who, what, when)
// 4. Multi-tenant safe by default
```

### 3. Authorization Middleware

#### Implementation

```typescript
// src/middleware/auth.ts

export async function requireOrgAccess(
  ctx: ConvexContext,
  orgId: Id<"organizations">,
  requiredRole: "org_owner" | "org_user"
) {
  const userId = await getUserId(ctx);

  // Get user's thing (person)
  const user = await ctx.db.get(userId);
  if (!user) {
    throw new Error("User not found");
  }

  // Check if platform owner (can access everything)
  if (user.properties.role === "platform_owner") {
    return true;
  }

  // Check if user belongs to organization
  const membership = await ctx.db
    .query("connections")
    .withIndex("by_from_to_type", (q) =>
      q.eq("fromThingId", userId)
       .eq("toThingId", orgId)
       .eq("relationshipType", "member_of")
    )
    .first();

  if (!membership) {
    throw new Error("User not member of organization");
  }

  // Check role
  const userRole = membership.metadata.role;
  if (requiredRole === "org_owner" && userRole !== "org_owner") {
    throw new Error("Org owner access required");
  }

  return true;
}
```

#### Usage Examples

```typescript
// Require org_user access (any member)
await requireOrgAccess(ctx, args.orgId, "org_user");

// Require org_owner access (admin only)
await requireOrgAccess(ctx, args.orgId, "org_owner");
```

### 4. Frontend Patterns

#### Old Frontend Pattern (4-Table)

```typescript
// No org context
const posts = useQuery(api.queries.posts.list);

// PROBLEM: Which organization's posts?
// PROBLEM: No multi-tenant support
```

#### New Frontend Pattern (6-Dimension)

```typescript
// Get current user and organization
const user = useQuery(api.queries.users.current);
const orgId = user?.properties.organizationId;

// Query posts for user's organization
const posts = useQuery(
  api.queries.posts.list,
  orgId ? { orgId } : "skip"
);

// BENEFIT: Clear organization context
// BENEFIT: Multi-tenant safe
```

#### Organization Selector Component

```tsx
// src/components/admin/OrganizationSelector.tsx

export function OrganizationSelector() {
  const user = useQuery(api.queries.users.current);
  const orgs = useQuery(
    api.queries.orgs.listForUser,
    user ? { userId: user._id } : "skip"
  );
  const [currentOrg, setCurrentOrg] = useState(
    user?.properties.organizationId
  );

  return (
    <Select value={currentOrg} onValueChange={setCurrentOrg}>
      {orgs?.map(org => (
        <SelectItem key={org._id} value={org._id}>
          {org.name} ({org.plan})
        </SelectItem>
      ))}
    </Select>
  );
}
```

### 5. Event Logging Patterns

#### Old Event Pattern (4-Table)

```typescript
// No actor information
await ctx.db.insert("events", {
  eventType: "content_created",
  thingId: postId,
  timestamp: Date.now(),
  metadata: {},
});

// PROBLEM: Who created the content?
// PROBLEM: No audit trail for authorization
```

#### New Event Pattern (6-Dimension)

```typescript
// Always include actor (person who performed action)
const userId = await getUserId(ctx);

await ctx.db.insert("events", {
  eventType: "content_created",
  actorId: userId,  // NEW: Who did it
  thingId: postId,
  organizationId: args.orgId,  // NEW: Which org
  timestamp: Date.now(),
  metadata: { contentType: "blog_post" },
});

// BENEFIT: Complete audit trail
// BENEFIT: Can query "what did user X do?"
// BENEFIT: Compliance and debugging
```

---

## Testing

### 1. Data Isolation Tests

Test that organizations have completely isolated data:

```typescript
import { describe, it, expect } from "vitest";

describe("Multi-tenant data isolation", () => {
  it("organizations cannot see each other's data", async () => {
    // Create two separate organizations
    const orgA = await createOrg("Org A");
    const orgB = await createOrg("Org B");

    // Create posts in each organization
    const postA = await createPost(orgA, "Post A");
    const postB = await createPost(orgB, "Post B");

    // Query from Org A context
    const postsInA = await queryPosts(orgA);

    // Verify isolation
    expect(postsInA).toContainEqual(postA);
    expect(postsInA).not.toContainEqual(postB);
  });

  it("platform owner can access all organizations", async () => {
    const orgA = await createOrg("Org A");
    const orgB = await createOrg("Org B");

    const platformOwner = await getPlatformOwner();

    // Platform owner can access both orgs
    const postsA = await queryPosts(orgA, platformOwner);
    const postsB = await queryPosts(orgB, platformOwner);

    expect(postsA).toBeDefined();
    expect(postsB).toBeDefined();
  });
});
```

### 2. Authorization Tests

Test that role-based permissions work correctly:

```typescript
describe("Role-based authorization", () => {
  it("org users can create content in their org", async () => {
    const org = await createOrg("Test Org");
    const user = await createUser(org, "org_user");

    // Should succeed
    const post = await createPost(org, user, "My Post");
    expect(post).toBeDefined();
  });

  it("org users cannot create content in other orgs", async () => {
    const orgA = await createOrg("Org A");
    const orgB = await createOrg("Org B");
    const userA = await createUser(orgA, "org_user");

    // Should fail
    await expect(
      createPost(orgB, userA, "Unauthorized Post")
    ).rejects.toThrow("User not member of organization");
  });

  it("org users cannot modify org settings", async () => {
    const org = await createOrg("Test Org");
    const user = await createUser(org, "org_user");

    // Should fail (requires org_owner)
    await expect(
      updateOrgSettings(org, user, { allowSignups: false })
    ).rejects.toThrow("Org owner access required");
  });

  it("org owners can modify org settings", async () => {
    const org = await createOrg("Test Org");
    const owner = await createUser(org, "org_owner");

    // Should succeed
    const updated = await updateOrgSettings(
      org,
      owner,
      { allowSignups: false }
    );
    expect(updated.settings.allowSignups).toBe(false);
  });
});
```

### 3. Event Audit Trail Tests

Test that all actions are properly logged:

```typescript
describe("Event audit trail", () => {
  it("logs actor for all mutations", async () => {
    const org = await createOrg("Test Org");
    const user = await createUser(org, "org_user");

    // Create content
    const postId = await createPost(org, user, "Test Post");

    // Check event was logged
    const events = await ctx.db
      .query("events")
      .withIndex("by_thing", (q) => q.eq("thingId", postId))
      .collect();

    expect(events).toHaveLength(1);
    expect(events[0].actorId).toBe(user._id);
    expect(events[0].eventType).toBe("content_created");
  });

  it("can query all actions by user", async () => {
    const org = await createOrg("Test Org");
    const user = await createUser(org, "org_user");

    // User performs multiple actions
    await createPost(org, user, "Post 1");
    await createPost(org, user, "Post 2");

    // Query all events by this user
    const userEvents = await ctx.db
      .query("events")
      .withIndex("by_actor", (q) => q.eq("actorId", user._id))
      .collect();

    expect(userEvents).toHaveLength(2);
  });
});
```

---

## Rollback Plan

If critical issues arise during migration:

### Stage 1: Documentation Rollback (1 hour)

**Action:** Revert documentation to "4-table" terminology
**Impact:** Minimal - schema changes remain (backward compatible)
**Steps:**
1. Revert all `.md` files to previous commit
2. Update main README
3. No code changes needed

### Stage 2: Query Rollback (4 hours)

**Action:** Temporarily remove organization scoping
**Impact:** Medium - disables multi-tenant isolation temporarily
**Steps:**
1. Comment out `organizationId` filters in queries
2. Disable `requireOrgAccess()` checks
3. Add monitoring to detect cross-org queries
4. Deploy hotfix
5. Monitor for data leaks

```typescript
// Temporary rollback pattern
const posts = await ctx.db
  .query("things")
  .withIndex("by_type", (q) => q.eq("thingType", "blog_post"))
  // .withIndex("by_org_type", ...) // DISABLED for rollback
  .collect();
```

### Stage 3: Full Rollback (1 day)

**Action:** Revert all code changes completely
**Impact:** High - returns to pre-migration state
**Steps:**
1. Revert all code changes via git
2. Keep `organizations` table (data intact)
3. Remove org-scoped indexes (optional)
4. Run full test suite
5. Deploy to production

**Data Safety:**
- All data remains intact
- No data loss
- Organizations table stays (for future migration)
- Can re-run migration later after fixes

### Backup Strategy

**Before Migration:**
1. Full database backup
2. Documentation backup in git
3. Test rollback in staging environment
4. Document rollback procedures

**During Migration:**
1. Monitor error rates
2. Monitor query performance
3. Monitor cross-org data leaks
4. Have rollback ready at all times

---

## Support

### Documentation

- **Primary Spec:** [ontology.md](./ontology.md) - Complete 6-dimension specification
- **Architecture:** [architecture.md](../things/architecture.md) - How everything fits together
- **Organizations:** [organisation.md](./organisation.md) - Multi-tenant architecture
- **People:** [people.md](./people.md) - Authorization & roles
- **Examples:** [implementation-examples.md](../things/implementation-examples.md) - Code examples

### Examples

**Children's Examples:**
- Lemonade stands
- Toy stores
- Pet care apps

**Enterprise Examples:**
- CRM SaaS platforms
- Multi-tenant marketplaces
- Enterprise content management

### Getting Help

**Questions?**
- Read the [6-dimension ontology specification](./ontology.md)
- Check [implementation examples](../things/implementation-examples.md)
- Review [architecture documentation](../things/architecture.md)

**Found a Bug?**
- Check if it's a multi-tenant isolation issue
- Test in single-org environment
- Review authorization middleware
- Check org-scoped queries

**Performance Issues?**
- Verify org-scoped indexes are being used
- Check query patterns include `organizationId`
- Review index usage with Convex dashboard
- Consider adding composite indexes

---

## Key Takeaways

### The Big Picture

The 6-dimension ontology transforms ONE Platform from a single-purpose system into a complete reality-aware architecture:

1. **Organizations** - Multi-tenant isolation boundary (who owns what at org level)
2. **People** - Authorization & governance (who can do what)
3. **Things** - Entities (what exists)
4. **Connections** - Relationships (how they relate)
5. **Events** - Actions (what happened)
6. **Knowledge** - Intelligence (what it means)

### Why It Matters

**For Children:**
- "I own a lemonade stand (Organization)"
- "I'm the owner (Person)"
- "I sell lemonade (Things)"
- "Customers buy it (Connections)"
- "Sales happen (Events)"
- "I learn what works (Knowledge)"

**For Enterprises:**
- Perfect multi-tenant isolation
- Clear ownership boundaries
- Role-based authorization
- Complete audit trails
- AI-powered intelligence
- Infinite scalability

### Golden Rules

1. **Every resource belongs to an organization** - No exceptions
2. **Every action has an actor** - Events must include `actorId`
3. **Check authorization first** - Use `requireOrgAccess()` in all mutations
4. **Query with org context** - Always use `by_org` or `by_org_type` indexes
5. **Log everything** - Complete audit trail via events
6. **People are things** - No duplicate tables, use role metadata

---

## Conclusion

This migration transforms ONE Platform into a complete 6-dimension reality-aware architecture that:

✅ **Scales from children to enterprises** - Same model works for lemonade stands and SaaS platforms
✅ **Perfect multi-tenant isolation** - Organizations partition all data
✅ **Clear ownership and governance** - People dimension with role-based authorization
✅ **Complete audit trail** - Every action logged with actor
✅ **AI-friendly and future-proof** - AI agents can reason about complete reality model
✅ **Simple, beautiful, complete** - Six dimensions model all of reality

**The 6 dimensions—Organizations, People, Things, Connections, Events, Knowledge—create a complete model of reality that both humans and AI can understand, reason about, and build upon infinitely.**

---

**Migration Guide Version:** 1.0.0
**Last Updated:** 2025-10-10
**Next Review:** After production deployment
**Owner:** Platform Architecture Team
