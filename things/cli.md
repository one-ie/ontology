# ONE CLI - Ontology-Driven Project Bootstrap

**Version:** 1.0.0
**Package:** `npx oneie` (ONE Intelligent Environment)
**Purpose:** Bootstrap production-ready, AI-first projects with full ontology integration

---

## Core Concept

The ONE CLI creates **living, ontology-driven codebases** that AI agents can understand and extend:

```
npx oneie
    ↓
Downloads /one/ → Generates Frontend → Generates Backend → Configures AI → Ready!
```

**Result:** A complete Astro + Effect.ts + Backend-of-Choice project where:

- Every organization, person, thing, connection, event, and knowledge maps to the 6-dimension ontology
- **Backend-agnostic frontend** works with Convex, WordPress, Notion, Supabase, or custom APIs
- AI agents (Claude, GPT, MCPs) can generate features using the DSL
- Code is type-safe, tested, and production-ready
- Organizations can use their **existing infrastructure**

---

## Bootstrap Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  1. CLONE ONTOLOGY & DOCS                                   │
│                                                              │
│  Creates 2 directories:                                     │
│                                                              │
│  /one/ - Ontology documentation (from source repo):         │
│    • Ontology.md (6-dimension universe)                    │
│    • ontology-frontend.md (backend-agnostic patterns)      │
│    • ontology-backend.md (Convex + Effect.ts)              │
│    • Rules.md, Patterns.md, Workflow.md                    │
│    • Protocol specs (A2A, ACP, AP2, X402, AGUI)            │
│    • DSL specs, Architecture docs                          │
│                                                              │
│  /docs/ - 3rd party documentation (cloned for AI):          │
│    • Astro docs                                             │
│    • Convex docs                                            │
│    • Effect.ts docs                                         │
│    • React docs                                             │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│  2. GENERATE FRONTEND (Astro 5 + React 19 + Effect.ts)    │
│     📖 See: one/knowledge/ontology-frontend.md            │
│                                                              │
│  frontend/                                                   │
│  ├── src/                                                    │
│  │   ├── pages/*.astro          # SSR pages                │
│  │   ├── components/ui/*        # shadcn/ui (50+)          │
│  │   ├── components/features/*  # Feature components        │
│  │   ├── providers/             # Backend-agnostic layer   │
│  │   │   ├── DataProvider.ts    # Universal ontology API   │
│  │   │   ├── convex/            # Convex implementation    │
│  │   │   ├── wordpress/         # WordPress provider       │
│  │   │   └── notion/            # Notion provider          │
│  │   ├── services/              # Effect.ts client layer   │
│  │   │   ├── ThingClientService.ts  # Generic thing ops    │
│  │   │   └── ConnectionClientService.ts  # Connection ops  │
│  │   ├── styles/global.css      # Tailwind v4              │
│  │   └── layouts/Layout.astro   # Base layout              │
│  ├── public/                    # Static assets            │
│  └── astro.config.mjs           # Provider selection       │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│  3. GENERATE BACKEND (Convex + Effect.ts)                 │
│     📖 See: one/knowledge/ontology-backend.md             │
│                                                              │
│  backend/                                                    │
│  ├── convex/                                                 │
│  │   ├── schema.ts              # 6-dimension schema       │
│  │   ├── services/              # Effect.ts services       │
│  │   ├── mutations/             # Convex mutations         │
│  │   ├── queries/               # Convex queries           │
│  │   ├── actions/               # Convex actions           │
│  │   └── providers/             # External integrations    │
│  └── package.json               # Backend dependencies     │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│  4. CREATE IMPORT UTILITIES                                 │
│                                                              │
│  import/                                                     │
│  └── ...                        # Data import scripts       │
└─────────────────────────────────────────────────────────────┘
    ↓
┌─────────────────────────────────────────────────────────────┐
│  5. CONFIGURE AI INTEGRATION                                │
│                                                              │
│  • CLAUDE.md (instructions for Claude Code)                 │
│  • AGENTS.md (Convex patterns)                              │
│  • .claude/hooks/ (pre/post hooks)                          │
│  • .mcp.json (MCP server config)                            │
│  • scripts/mcp-ontology-server.js (ontology API)            │
└─────────────────────────────────────────────────────────────┘
```

---

## Usage

### Bootstrap New Project

```bash
npx oneie create my-creator-platform
cd my-creator-platform
```

**Generated structure:**

```
my-creator-platform/
├── one/                # ⭐ Ontology (cloned from source)
│   ├── people/         # Dimension 2: Authorization & intent
│   ├── things/         # Dimension 3: Entities (66 types)
│   ├── connections/    # Dimension 4: Relationships (25 types)
│   │   └── ontology-frontend.md  # 🔑 Frontend implementation guide
│   ├── events/         # Dimension 5: Behavior patterns (67 types)
│   └── knowledge/      # Dimension 6: Embeddings & search
│
├── frontend/           # ⭐ Generated Astro + React app
│   ├── src/
│   │   ├── pages/              # Astro pages
│   │   ├── components/         # React components
│   │   ├── providers/          # Backend-agnostic layer
│   │   ├── services/           # Effect.ts client services
│   │   ├── hooks/              # React hooks
│   │   ├── ontology/           # UI config
│   │   └── styles/             # CSS/Tailwind
│   ├── public/                 # Static assets
│   └── astro.config.mjs        # Config (provider selection)
│
├── backend/            # ⭐ Generated Convex backend
│   ├── convex/
│   │   ├── schema.ts           # 6-dimension schema
│   │   ├── services/           # Effect.ts services
│   │   ├── queries/            # Convex queries
│   │   ├── mutations/          # Convex mutations
│   │   ├── actions/            # Convex actions
│   │   └── providers/          # External services
│   └── package.json
│
├── docs/               # ⭐ 3rd party docs (cloned)
│   ├── astro/                  # Astro documentation
│   ├── convex/                 # Convex documentation
│   ├── effect/                 # Effect.ts documentation
│   └── react/                  # React documentation
│
├── import/             # ⭐ Import utilities
│   └── ...                     # Data import scripts
│
├── .claude/            # Claude Code config
│   └── hooks/
│
├── .env                # Environment variables
├── CLAUDE.md           # AI agent instructions
├── .mcp.json           # MCP server config
└── package.json        # Root dependencies
```

**Key Features:**

- **5 Core Directories:** `/one/`, `/frontend/`, `/backend/`, `/docs/`, `/import/`
- **Self-Contained:** All docs (ontology + 3rd party) included for offline AI access
- **Backend-Agnostic:** Frontend works with Convex, WordPress, Notion, or custom backends
- **AI-Ready:** MCP server, Claude Code config, complete documentation
- **Portable:** Clone and deploy anywhere

---

## Frontend Architecture (Backend-Agnostic)

**See: `one/knowledge/ontology-frontend.md` for complete implementation guide.**

### DataProvider Pattern (Inspired by Astro Content Layer)

The CLI generates a **backend-agnostic frontend** using the DataProvider pattern:

```typescript
// astro.config.ts - Swap backends by changing ONE line
import { defineConfig } from "astro/config";
import { one } from "@one/astro-integration";
import { convexProvider } from "./src/providers/convex";
// import { wordpressProvider } from './src/providers/wordpress'
// import { notionProvider } from './src/providers/notion'

export default defineConfig({
  integrations: [
    one({
      // ✅ Change this ONE line to swap backends
      provider: convexProvider({
        url: import.meta.env.PUBLIC_BACKEND_URL,
      }),
    }),
  ],
});
```

### Universal Ontology API

Every provider implements the same 6-dimension interface:

```typescript
// src/providers/DataProvider.ts
export interface DataProvider {
  organizations: {
    get: (id: string) => Effect.Effect<Organization, Error>;
    list: (params) => Effect.Effect<Organization[], Error>;
    update: (id: string, updates) => Effect.Effect<void, Error>;
  };
  people: {
    get;
    list;
    create;
    update;
    delete;
  };
  things: {
    get;
    list;
    create;
    update;
    delete;
  };
  connections: {
    create;
    getRelated;
    getCount;
    delete;
  };
  events: {
    log;
    query;
  };
  knowledge: {
    embed;
    search;
  };
}
```

**Result:**

- Frontend works with **any backend** (Convex, WordPress, Notion, Supabase)
- Organizations can use their **existing infrastructure**
- Swap backends without changing frontend code
- ONE ontology = universal API

### Generated Providers

The CLI generates provider implementations for popular backends:

#### Convex Provider (Default)

```typescript
// src/providers/convex/ConvexProvider.ts
export class ConvexProvider implements DataProvider {
  things = {
    get: (id) =>
      Effect.tryPromise(() =>
        this.client.query(api.queries.things.get, { id })
      ),
    list: (params) =>
      Effect.tryPromise(() =>
        this.client.query(api.queries.things.list, params)
      ),
    // ... all 6 dimensions
  };
}
```

#### WordPress Provider

```typescript
// src/providers/wordpress/WordPressProvider.ts
export class WordPressProvider implements DataProvider {
  things = {
    get: (id) =>
      Effect.gen(function* () {
        // Map ONE thing → WordPress post
        const response = yield* fetch(
          `${this.baseUrl}/wp-json/wp/v2/posts/${id}`
        );
        const post = yield* response.json();
        return transformToThing(post);
      }),
  };
}
```

#### Notion Provider

```typescript
// src/providers/notion/NotionProvider.ts
export class NotionProvider implements DataProvider {
  things = {
    get: (id) =>
      Effect.gen(function* () {
        // Map ONE thing → Notion page
        const page = yield* this.notion.pages.retrieve({ page_id: id });
        return transformToThing(page);
      }),
  };
}
```

**All providers implement the same interface** → Frontend code stays identical.

---

## Feature Generation (DSL-Driven)

### Create Feature from Plain English

```bash
# Write feature spec
cat > features/token-purchase.english << 'EOF'
FEATURE: Let fans buy creator tokens

INPUT:
  - fan: who is buying
  - token: which token
  - amount: how many

OUTPUT:
  - payment ID: Stripe transaction
  - tx hash: blockchain confirmation

FLOW:

CHECK fan exists
CHECK token exists
CHECK amount is greater than 0

DO TOGETHER:
  - CALL Stripe to charge payment
  - CALL Blockchain to mint tokens

RECORD tokens_purchased
  BY fan
  WITH amount and payment ID

GIVE payment ID and tx hash
EOF

# Generate code
npx oneie generate features/token-purchase.english
```

**Generated files:**

```
convex/services/tokens/token-purchase.ts    # Effect.ts service
convex/mutations/tokens.ts                   # Convex wrapper
src/components/features/TokenPurchase.tsx    # React component
tests/unit/services/token-purchase.test.ts   # Tests
```

---

### Conversational Mode

```bash
npx oneie chat
```

**Interactive session:**

```
Welcome to ONE! 🎉

What would you like to build?
> Let fans chat with my AI clone

Great! A few questions:

1. Should fans need tokens? (yes/no)
> yes

2. Tokens per message?
> 10

3. Train AI on videos? (yes/no)
> yes

┌───────────────────────────────┐
│ Feature: Chat with AI Clone   │
├───────────────────────────────┤
│ ✓ Token-gated                 │
│ ✓ 10 tokens per message       │
│ ✓ AI trained on videos        │
│ ✓ Real-time chat              │
└───────────────────────────────┘

Sound good? (yes/edit/cancel)
> yes

Building...
✓ Training AI personality... (3s)
✓ Creating chat system... (2s)
✓ Building UI... (2s)
✓ Testing... (1s)

🎉 Done! Try it: https://localhost:4321/chat
```

---

## MCP Integration

The CLI generates `.mcp.json` with ontology server and local docs access:

```json
{
  "mcpServers": {
    "ontology": {
      "command": "node",
      "args": ["scripts/mcp-ontology-server.js"]
    },
    "shadcn": {
      "command": "npx",
      "args": ["-y", "@shadowsu/mcp-shadcn"]
    },
    "convex": {
      "command": "npx",
      "args": ["@convex-dev/mcp-server"]
    }
  }
}
```

**MCP Tools:**

- `ontology.read` → Full Ontology.md from `/one/`
- `ontology.query` → Query entities/connections/events/knowledge from `/one/`
- `ontology.validate` → Validate feature specs
- `dsl.parse` → Plain English → Technical DSL
- `dsl.compile` → Technical DSL → TypeScript
- `docs.read` → Read 3rd party docs from `/docs/` (Astro, Convex, Effect.ts, React)

**AI Usage (Offline):**

```typescript
// Claude Code reads from local /one/ directory:
await mcp.ontology.query("entities", { filter: "token" });
// → Returns: token entity type with properties (no web fetch)

await mcp.dsl.parse(plainEnglishSpec);
// → Returns: Technical DSL structure

await mcp.ontology.validate(technicalDSL);
// → Returns: { valid: true, errors: [] }

// Read 3rd party docs locally from /docs/:
await mcp.docs.read("convex", "mutations");
// → Returns: Convex mutation docs (no web fetch)
```

**Key Benefit:** All documentation (ontology + 3rd party) is **cloned into the project**, enabling fast offline AI operation.

---

## Command Reference

### `npx oneie create <name>`

Bootstrap new project with 5-directory structure.

**Options:**

- `--backend <type>` - Backend: `convex` (default), `wordpress`, `notion`, `supabase`, `none`
- `--no-docs` - Skip cloning 3rd party docs to `/docs/`
- `--no-install` - Skip `bun install`
- `--ai <provider>` - AI provider: `claude`, `gpt`, `cursor`

```bash
# Full stack with Convex backend (default)
npx oneie create my-platform

# Frontend + WordPress backend
npx oneie create my-platform --backend wordpress

# Frontend only (bring your own backend)
npx oneie create my-platform --backend none

# Skip docs cloning (smaller project size)
npx oneie create my-platform --no-docs
```

**Generated Structure:** Always creates 5 directories (`/one/`, `/frontend/`, `/backend/`, `/docs/`, `/import/`)

---

### `npx oneie generate <feature>`

Generate feature from DSL.

**Options:**

- `--validate-only` - Only validate
- `--dry-run` - Show what would be generated
- `--output <dir>` - Custom output directory

```bash
npx oneie generate features/token-purchase.english --dry-run
```

---

### `npx oneie chat`

Interactive conversational mode.

```bash
npx oneie chat

> Let fans buy tokens
> $0.10 per token
> Accept crypto
> Building...
> ✓ Done!
```

---

### `npx oneie ontology`

Query/update ontology.

**Commands:**

- `npx oneie ontology show` - Show full ontology
- `npx oneie ontology query <type>` - Query types
- `npx oneie ontology sync` - Sync from GitHub
- `npx oneie ontology stats` - Statistics

```bash
npx oneie ontology query entities
# Returns: 66 entity types

npx oneie ontology sync
# ✓ Synced with one@latest (v1.0.0)
```

---

### `npx oneie add <component>`

Add shadcn/ui component.

```bash
npx oneie add button card dialog
npx oneie add --all
```

---

### `npx oneie validate`

Validate project against ontology.

```bash
npx oneie validate
npx oneie validate --fix
```

---

## Generated Files

### Convex Schema (6-Dimension Ontology)

```typescript
// convex/schema.ts
export default defineSchema({
  // DIMENSION 1: ORGANIZATIONS (multi-tenancy)
  organizations: defineTable({
    name: v.string(),
    slug: v.string(),
    status: v.union(
      v.literal("active"),
      v.literal("suspended"),
      v.literal("trial")
    ),
    plan: v.union(
      v.literal("starter"),
      v.literal("pro"),
      v.literal("enterprise")
    ),
  }).index("by_slug", ["slug"]),

  // DIMENSION 2: PEOPLE (authorization, intent)
  people: defineTable({
    email: v.string(),
    username: v.string(),
    displayName: v.string(),
    role: v.union(
      v.literal("platform_owner"),
      v.literal("org_owner"),
      v.literal("org_user"),
      v.literal("customer")
    ),
    organizationId: v.id("organizations"),
  }).index("by_email", ["email"]),

  // DIMENSION 3: THINGS (all entities)
  things: defineTable({
    type: v.union(
      v.literal("course"),
      v.literal("lesson"),
      v.literal("product"),
      v.literal("token"),
      v.literal("ai_clone")
      // ... 66 total types
    ),
    name: v.string(),
    organizationId: v.id("organizations"),
    properties: v.any(), // JSON blob for type-specific fields
    status: v.string(),
    createdAt: v.number(),
    updatedAt: v.number(),
  })
    .index("by_type", ["type"])
    .index("by_org", ["organizationId"]),

  // DIMENSION 4: CONNECTIONS (all relationships)
  connections: defineTable({
    fromThingId: v.id("things"),
    toThingId: v.id("things"),
    relationshipType: v.union(
      v.literal("part_of"),
      v.literal("enrolled_in"),
      v.literal("owns"),
      v.literal("following"),
      v.literal("member_of")
      // ... 25 total types
    ),
    metadata: v.optional(v.any()),
    createdAt: v.number(),
  })
    .index("from_type", ["fromThingId", "relationshipType"])
    .index("to_type", ["toThingId", "relationshipType"]),

  // DIMENSION 5: EVENTS (all behavior)
  events: defineTable({
    type: v.union(
      v.literal("thing_created"),
      v.literal("connection_created"),
      v.literal("tokens_purchased"),
      v.literal("chat_interaction"),
      v.literal("course_completed")
      // ... 67 total types
    ),
    actorId: v.id("people"), // Person who did it
    targetId: v.optional(v.string()), // Thing/Person/Connection ID
    organizationId: v.id("organizations"),
    timestamp: v.number(),
    metadata: v.optional(v.any()),
  })
    .index("by_actor", ["actorId"])
    .index("by_org", ["organizationId"])
    .index("by_type", ["type"]),

  // DIMENSION 6: KNOWLEDGE (embeddings, search)
  knowledge: defineTable({
    text: v.string(),
    embedding: v.array(v.float64()),
    sourceThingId: v.optional(v.id("things")),
    sourcePersonId: v.optional(v.id("people")),
    organizationId: v.id("organizations"),
    labels: v.optional(v.array(v.string())),
  })
    .vectorIndex("by_embedding", {
      vectorField: "embedding",
      dimensions: 1536,
    })
    .index("by_org", ["organizationId"]),
});
```

---

### Effect.ts Service Pattern

```typescript
// convex/services/tokens/token-purchase.ts
import { Effect } from "effect";
import { ConvexDatabase } from "../core/database";
import { StripeProvider, BlockchainProvider } from "../providers";

export class TokenPurchaseService extends Effect.Service<TokenPurchaseService>()(
  "TokenPurchaseService",
  {
    effect: Effect.gen(function* () {
      const db = yield* ConvexDatabase;
      const stripe = yield* StripeProvider;
      const blockchain = yield* BlockchainProvider;

      return {
        execute: (
          fanId: Id<"entities">,
          tokenId: Id<"entities">,
          amount: number
        ) =>
          Effect.gen(function* () {
            // Validation
            const fan = yield* Effect.tryPromise(() => db.get(fanId));
            if (!fan)
              return yield* Effect.fail({ _tag: "NotFound", entity: "fan" });

            const token = yield* Effect.tryPromise(() => db.get(tokenId));
            if (!token)
              return yield* Effect.fail({ _tag: "NotFound", entity: "token" });

            if (amount <= 0)
              return yield* Effect.fail({ _tag: "InvalidAmount" });

            // Parallel execution
            const [payment, mintTx] = yield* Effect.all([
              stripe.charge({ amount: amount * 100 }),
              blockchain.mint({ tokenId, toAddress: fanId, amount }),
            ]);

            // Record event
            yield* Effect.tryPromise(() =>
              db.insert("events", {
                entityId: tokenId,
                eventType: "tokens_purchased",
                timestamp: Date.now(),
                actorType: "user",
                actorId: fanId,
                metadata: {
                  amount,
                  paymentId: payment.id,
                  txHash: mintTx.hash,
                },
              })
            );

            return { paymentId: payment.id, txHash: mintTx.hash };
          }),
      };
    }),
    dependencies: [
      ConvexDatabase.Default,
      StripeProvider.Default,
      BlockchainProvider.Default,
    ],
  }
) {}
```

---

### Convex Mutation Wrapper

```typescript
// convex/mutations/tokens.ts
import { confect } from "confect";
import { v } from "convex/values";
import { TokenPurchaseService } from "../services/tokens/token-purchase";
import { MainLayer } from "../layers";

export const purchase = confect.mutation({
  args: {
    fanId: v.id("entities"),
    tokenId: v.id("entities"),
    amount: v.number(),
  },
  handler: (ctx, args) =>
    Effect.gen(function* () {
      const service = yield* TokenPurchaseService;
      return yield* service.execute(args.fanId, args.tokenId, args.amount);
    }).pipe(Effect.provide(MainLayer)),
});
```

---

### React Component

```typescript
// src/components/features/TokenPurchase.tsx
import { useMutation } from "convex/react";
import { api } from "@/convex/_generated/api";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { useState } from "react";

export function TokenPurchase({ tokenId }: { tokenId: Id<"entities"> }) {
  const [amount, setAmount] = useState(100);
  const purchase = useMutation(api.tokens.purchase);

  const handlePurchase = async () => {
    try {
      const result = await purchase({ tokenId, amount });
      alert(`Success! TX: ${result.txHash}`);
    } catch (error) {
      alert(`Error: ${error.message}`);
    }
  };

  return (
    <div className="flex gap-2">
      <Input
        type="number"
        value={amount}
        onChange={(e) => setAmount(Number(e.target.value))}
      />
      <Button onClick={handlePurchase}>Buy {amount} Tokens</Button>
    </div>
  );
}
```

---

## Environment Variables

Generated `.env.example`:

```bash
# Convex
CONVEX_DEPLOYMENT=<your-deployment>
CONVEX_URL=https://<your-deployment>.convex.cloud

# Better Auth
BETTER_AUTH_SECRET=<generate-with-openssl>
BETTER_AUTH_URL=http://localhost:4321

# OAuth
GITHUB_CLIENT_ID=<your-github-oauth>
GITHUB_CLIENT_SECRET=<your-github-secret>
GOOGLE_CLIENT_ID=<your-google-oauth>
GOOGLE_CLIENT_SECRET=<your-google-secret>

# External Services (optional)
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
STRIPE_SECRET_KEY=sk_test_...
BLOCKCHAIN_RPC_URL=https://mainnet.base.org
```

---

## Claude Code Integration

Generated `.claude/settings.local.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "bash .claude/hooks/pre.sh"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "bash .claude/hooks/post.sh"
          }
        ]
      }
    ]
  },
  "permissions": {
    "allow": ["Bash(bun:*)", "Bash(bunx:*)", "Bash(git:*)", "mcp__*"]
  }
}
```

**Hooks track ontology changes (inference score):**

```bash
# .claude/hooks/pre.sh
# Calculates hash of one/knowledge/ontology.md
# If changed → increment inference score
# Logs to one/things/inference_score.md
```

---

## Why This Works

**Traditional approach:**

1. Manual setup (hours)
2. Configure 10+ tools (days)
3. Learn patterns (weeks)
4. Write boilerplate (hours per feature)
5. Locked into one backend forever

**ONE CLI approach:**

1. `npx oneie create` (5 minutes)
2. Everything pre-configured ✅
3. AI learns from patterns ✅
4. `npx oneie generate` (1 minute)
5. Backend-agnostic (swap anytime) ✅

**Key Innovations:**

1. **5-Directory Structure**

   - `/one/` - Ontology docs cloned into project (AI can read)
   - `/frontend/` - Backend-agnostic Astro + React app
   - `/backend/` - Convex backend (or generate for any backend)
   - `/docs/` - 3rd party docs cloned locally (offline AI access)
   - `/import/` - Data import utilities

2. **Universal Ontology API**

   - Frontend only knows 6 dimensions (organizations, people, things, connections, events, knowledge)
   - Backend provider pattern = swap Convex ↔ WordPress ↔ Notion
   - Same frontend code works with ANY backend

3. **Self-Contained & Portable**
   - All documentation included (ontology + 3rd party)
   - AI agents work offline (no web fetches needed)
   - Clone project = clone complete knowledge base

**Result:** 100x faster, higher quality, AI-ready, backend-flexible, fully portable.

---

## Roadmap

### Phase 1: Core Bootstrap (Q1 2025)

- ✅ `npx oneie create` - Full scaffolding
- ✅ 5-directory structure (`/one/`, `/frontend/`, `/backend/`, `/docs/`, `/import/`)
- ✅ Ontology cloning from GitHub
- ✅ 3rd party docs cloning (Astro, Convex, Effect.ts, React)
- ✅ Backend-agnostic frontend (Astro + React + DataProvider)
- ✅ Convex backend generation (6-dimension schema)
- ✅ shadcn/ui installation
- ✅ Better Auth configuration
- ✅ MCP server setup

### Phase 2: Feature Generation (Q2 2025)

- ⏳ `npx oneie generate` - DSL compilation
- ⏳ Plain English parser
- ⏳ Ontology validator
- ⏳ Claude Code SDK integration

### Phase 3: Conversational CLI (Q2 2025)

- ⏳ `npx oneie chat` - Interactive mode
- ⏳ Intent recognition
- ⏳ Smart questions
- ⏳ Progress feedback

### Phase 4: Advanced (Q3 2025)

- 📋 `npx oneie deploy` - One-command deployment
- 📋 `npx oneie upgrade` - Update ontology
- 📋 `npx oneie migrate` - Schema migrations

---

## Success Metrics

**For Developers:**

- ⏱️ 5 minutes → Project bootstrap to first feature
- 🎯 100% type-safe → Generated code passes strict TypeScript
- ✅ Tests included → Every feature has unit tests
- 📖 AI-ready → Claude, GPT, Cursor understand it

**For Non-Technical Users:**

- 💬 Conversational → No commands to memorize
- 🚀 1 minute → Feature idea to working code
- 📱 Browser preview → See it immediately
- ❌ No code shown → Never see TypeScript

---

## Related Documentation

### Architecture Guides

- **[structure.md](../connections/structure.md)** - 📁 Repository & project structure (5-directory architecture)
- **[ontology-frontend.md](../knowledge/ontology-frontend.md)** - 🔑 Frontend implementation (backend-agnostic)
- **[ontology-backend.md](../knowledge/ontology-backend.md)** - Backend implementation (Convex + Effect.ts)
- **[Ontology.md](./ontology.md)** - 6-dimension data model (source of truth)

### Development Guides

- **[dsl.md](./dsl.md)** - Technical DSL specification
- **[dsl-english.md](./dsl-english.md)** - Plain English DSL
- **[Rules.md](./rules.md)** - Golden rules for AI agents
- **[Patterns.md](./patterns.md)** - Proven code patterns
- **[Workflow.md](../connections/workflow.md)** - Ontology-driven development flow
- **[CLAUDE.md](../CLAUDE.md)** - Instructions for Claude Code

---

## Summary: The 5-Directory Architecture

Every generated project has exactly **5 core directories**:

```
my-project/
├── one/        → 📖 Ontology documentation (AI knowledge base)
├── frontend/   → 🎨 Backend-agnostic UI (Astro + React + DataProvider)
├── backend/    → ⚙️ Data & logic (Convex by default, swap to WordPress/Notion/etc)
├── docs/       → 📚 3rd party docs (Astro, Convex, Effect.ts, React)
└── import/     → 📥 Data import utilities
```

**Why This Structure?**

1. **`/one/`** - AI agents read ontology to understand your domain
2. **`/frontend/`** - Works with ANY backend via DataProvider pattern
3. **`/backend/`** - Implements 6-dimension schema (organizations, people, things, connections, events, knowledge)
4. **`/docs/`** - AI agents reference official docs without web fetches
5. **`/import/`** - Utilities to import existing data

**Result:** Self-contained, portable, AI-ready projects where frontend ↔ backend communication follows the universal 6-dimension ontology API.

---

**Core Principle:** The CLI makes it **easier for AI agents to build features** than humans. When that's true, we've succeeded.
