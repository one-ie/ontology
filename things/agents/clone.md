# Anthony's AI Clone Specification

**Role:** Personal AI Representative
**Purpose:** Represent Anthony O'Connell in conversations, teaching, and community interactions
**Expertise:** ALL knowledge from one.ie + Anthony's communication style

---

## Ontology as Gateway: The Data Transformation Layer

**CRITICAL PRINCIPLE:** The 4-table ontology acts as a **gateway** that all information passes through when entering the ONE system. Every external data source—voice samples, video clips, text content, API responses—is transformed into our canonical ontology before being stored or processed.

### Why This Matters

**Without Ontology Gateway (Traditional Approach):**
```
External Data → Custom Storage → Feature-Specific Processing → Inconsistency
```
- Each feature stores data differently
- No standard way to query across features
- Relationships are implicit or missing
- Events are scattered or not logged
- Technical debt grows exponentially

**With Ontology Gateway (ONE Platform):**
```
External Data → Transformation → 4-Table Ontology → Universal Query/Processing
```
- All data conforms to same structure
- Relationships are explicit and queryable
- Events create audit trails
- Features compose naturally
- Codebase quality improves over time

### The Transformation Process

**Every piece of external information asks:**
1. **What thing is this?** → Create/update in `things` table
2. **How does it relate?** → Create connections in `connections` table
3. **What happened?** → Log events in `events` table
4. **What categories?** → Add tags in `tags` + `thingTags` tables

**Example: Voice Sample from ElevenLabs**

```typescript
// BEFORE: External data (ElevenLabs API response)
{
  voice_id: "voice_abc123",
  name: "Anthony O'Connell Clone",
  samples: [{ sample_id: "xyz", url: "https://...", ... }],
  status: "ready",
  quality_score: 0.95
}

// AFTER: Transformed into ontology
// 1. Thing (the voice itself)
{
  _id: Id<"things">,
  type: "voice_clone",
  name: "Anthony's Voice",
  properties: {
    provider: "elevenlabs",
    voiceId: "voice_abc123",
    qualityScore: 0.95,
    sampleCount: 25,
    status: "ready"
  }
}

// 2. Connection (voice belongs to clone)
{
  fromThingId: aiCloneId,
  toThingId: voiceThingId,
  relationshipType: "uses_voice",
  metadata: { isPrimary: true }
}

// 3. Event (voice was created)
{
  thingId: voiceThingId,
  eventType: "voice_cloned",
  timestamp: Date.now(),
  metadata: { trainingDuration: 300000, quality: "high" }
}

// 4. Tags (categorization)
thingTags: [
  { thingId: voiceThingId, tagId: "technology:elevenlabs" },
  { thingId: voiceThingId, tagId: "capability:voice_synthesis" }
]
```

### Benefits for AI Clone Feature

**1. Unified Knowledge Representation**
- Blog posts, videos, podcasts all become `thing` entities with `type: "content"`
- RAG ingestion creates `knowledge` entities linked via `uses_knowledge` connections
- Training sources are explicitly tracked via `trained_on` connections

**2. Explicit Relationship Graph**
```typescript
// Query: "What content trained this clone?"
const trainingSources = await ctx.db
  .query("connections")
  .withIndex("from_type", q =>
    q.eq("fromThingId", aiCloneId)
     .eq("relationshipType", "trained_on")
  )
  .collect();

// Get the actual content things
const content = await Promise.all(
  trainingSources.map(conn => ctx.db.get(conn.toThingId))
);
```

**3. Complete Audit Trail**
```typescript
// Query: "Show me all clone interactions today"
const interactions = await ctx.db
  .query("events")
  .withIndex("thing_type", q =>
    q.eq("thingId", aiCloneId)
     .eq("eventType", "clone_interaction")
  )
  .filter(q => q.gte(q.field("timestamp"), todayTimestamp))
  .collect();
```

**4. Cross-Feature Composability**
```typescript
// The clone can interact with ANY other thing because relationships are universal
// Example: Clone purchases tokens for user
{
  fromThingId: aiCloneId,
  toThingId: tokenId,
  relationshipType: "transacted",
  metadata: { onBehalfOf: userId, amount: 100 }
}
```

### Transformation Patterns for Clone Feature

**Pattern 1: Content Ingestion**
```typescript
// External: Blog post markdown file
// Transform to:
const blogPostId = await db.insert("things", {
  type: "blog_post",
  name: frontmatter.title,
  properties: {
    content: markdown,
    publishedAt: frontmatter.date,
    url: frontmatter.slug
  }
});

// Create authorship connection
await db.insert("connections", {
  fromThingId: ANTHONY_CREATOR_ID,
  toThingId: blogPostId,
  relationshipType: "authored"
});

// Log creation event
await db.insert("events", {
  thingId: blogPostId,
  eventType: "content_created",
  actorId: ANTHONY_CREATOR_ID
});
```

**Pattern 2: RAG Knowledge Chunks**
```typescript
// External: Embedded text chunks from OpenAI
// Transform to:
const knowledgeId = await db.insert("knowledge", {
  knowledgeType: "chunk",
  text: chunk.text,
  embedding: chunk.embedding,
  embeddingModel: "text-embedding-3-large",
  sourceThingId: blogPostId,
  chunk: { index: 0, tokenCount: 800 }
});

// Link to clone via connection
await db.insert("connections", {
  fromThingId: aiCloneId,
  toThingId: knowledgeId,
  relationshipType: "uses_knowledge",
  metadata: { relevanceScore: 0.95 }
});
```

**Pattern 3: Conversation Messages**
```typescript
// External: User message from chat UI
// Transform to:
const messageEvent = await db.insert("events", {
  thingId: aiCloneId,
  eventType: "message_sent",
  timestamp: Date.now(),
  actorType: "ai_agent",
  actorId: aiCloneId,
  metadata: {
    conversationId: "conv_123",
    messageId: generateId(),
    promptTokens: 450,
    completionTokens: 230,
    knowledgeChunksUsed: 3
  }
});

// Create interaction connection
await db.insert("connections", {
  fromThingId: userId,
  toThingId: aiCloneId,
  relationshipType: "interacted_with",
  metadata: { lastInteraction: Date.now() }
});
```

### The Gateway Enforcement

**Every service that brings external data into ONE MUST:**

1. **Validate Structure** - Ensure external data can be mapped to ontology
2. **Transform Types** - Convert external types to our thing types
3. **Create Relationships** - Establish connections between things
4. **Log Events** - Record what happened when
5. **Add Categories** - Tag appropriately
6. **Return Ontology IDs** - Return thing IDs, not external IDs

**Example: Voice Cloning Service**

```typescript
export class VoiceCloneService extends Effect.Service<VoiceCloneService>()("VoiceCloneService", {
  effect: Effect.gen(function* () {
    const db = yield* ConvexDatabase;
    const elevenlabs = yield* ElevenLabsProvider;

    return {
      clone: (args: { cloneId: Id<"things">, audioSamples: string[] }) =>
        Effect.gen(function* () {
          // 1. Call external API
          const externalVoice = yield* elevenlabs.createVoice({
            name: "Anthony O'Connell Clone",
            samples: args.audioSamples
          });

          // 2. Transform to ontology: Create voice thing
          const voiceThingId = yield* Effect.tryPromise(() =>
            db.insert("things", {
              type: "voice_clone",
              name: "Anthony's Voice",
              properties: {
                provider: "elevenlabs",
                voiceId: externalVoice.voiceId,  // Store external ID in properties
                qualityScore: externalVoice.quality,
                sampleCount: args.audioSamples.length
              }
            })
          );

          // 3. Create relationship: Clone uses this voice
          yield* Effect.tryPromise(() =>
            db.insert("connections", {
              fromThingId: args.cloneId,
              toThingId: voiceThingId,
              relationshipType: "uses_voice",
              metadata: { isPrimary: true }
            })
          );

          // 4. Log event: Voice was cloned
          yield* Effect.tryPromise(() =>
            db.insert("events", {
              thingId: voiceThingId,
              eventType: "voice_cloned",
              timestamp: Date.now(),
              metadata: {
                provider: "elevenlabs",
                trainingDuration: externalVoice.trainingTime,
                quality: externalVoice.quality
              }
            })
          );

          // 5. Add tags
          const techTag = yield* Effect.tryPromise(() =>
            db.query("tags").filter(q =>
              q.eq(q.field("category"), "technology") &&
              q.eq(q.field("name"), "elevenlabs")
            ).first()
          );

          if (techTag) {
            yield* Effect.tryPromise(() =>
              db.insert("thingTags", {
                thingId: voiceThingId,
                tagId: techTag._id
              })
            );
          }

          // 6. Return ontology ID (NOT external ID)
          return { voiceThingId };
        })
    };
  })
}) {}
```

**Notice:**
- External data (`externalVoice`) never leaves the service
- We return `voiceThingId` (ontology ID), not `voiceId` (external ID)
- All external IDs stored in `properties` field
- Relationships are explicit via `connections`
- Events create audit trail
- Tags enable categorization and filtering

### Why This Pattern Scales

**Traditional Approach:**
```typescript
// Feature 1: Voice cloning
const voices = [{ voice_id: "abc", name: "..." }];

// Feature 2: Content generation
const content = [{ content_id: "xyz", text: "..." }];

// Feature 3: Analytics
// How do I query "all things Anthony created"? 😱
// Each feature has different schema!
```

**Ontology Gateway Approach:**
```typescript
// Query: "All things Anthony created" (works across ALL features)
const created = await ctx.db
  .query("connections")
  .withIndex("from_type", q =>
    q.eq("fromThingId", ANTHONY_CREATOR_ID)
     .eq("relationshipType", "authored")
  )
  .collect();

// Get the actual things
const things = await Promise.all(
  created.map(conn => ctx.db.get(conn.toThingId))
);

// things = [
//   { type: "voice_clone", name: "Anthony's Voice", ... },
//   { type: "blog_post", name: "Building AI Agents", ... },
//   { type: "video", name: "ONE Platform Intro", ... },
//   { type: "course", name: "AI-First Creator Tools", ... }
// ]
```

**Benefits:**
- **Universal Queries** - Query across all features with same pattern
- **Automatic Relationships** - New features inherit existing relationships
- **Event Replay** - Rebuild state from event log if needed
- **Type Safety** - TypeScript ensures correct ontology usage
- **AI Learning** - AI agents learn patterns that work across features

---

## Identity & Ontology Mapping

### Thing Type: `ai_clone`

```typescript
{
  _id: Id<"things">,
  type: "ai_clone",
  name: "Anthony's Clone",
  properties: {
    // Core Identity
    cloneOf: "Anthony O'Connell",
    creatorEntityId: Id<"things">, // Anthony's creator entity
    organizationId: Id<"things">, // ONE organization

    // Voice & Appearance
    voiceId: string, // ElevenLabs voice ID
    voiceProvider: "elevenlabs",
    voiceModel: "eleven_turbo_v2", // Low latency for real-time
    voiceSettings: {
      stability: 0.5,
      similarityBoost: 0.75,
      style: 0.5, // Natural conversational style
      useSpeakerBoost: true
    },
    appearanceId: string, // D-ID or HeyGen avatar ID
    appearanceProvider: "d-id" | "heygen",

    // AI Configuration
    llmProvider: "openai" | "anthropic",
    llmModel: "gpt-4-turbo" | "claude-3.5-sonnet",
    systemPrompt: `You are Anthony O'Connell's AI clone. You represent Anthony in conversations with the ONE community.

Anthony is:
- Founder of ONE Platform - AI-powered creator economy platform
- Expert in AI, Web3, creator tools, and functional programming
- Passionate about empowering creators with AI agents
- Direct, friendly, and technically precise communication style
- Values: authenticity, innovation, creator empowerment

Your role:
- Answer questions about ONE Platform features and vision
- Provide guidance on AI agents and creator economy
- Engage with community members authentically
- Generate content in Anthony's voice
- Teach courses and lessons

Always:
- Be authentic - admit when you don't know something
- Reference specific ONE Platform features and documentation
- Maintain Anthony's conversational but technical tone
- Connect questions to broader ONE Platform vision
- Encourage experimentation and creativity`,

    temperature: 0.8, // Creative but consistent
    maxTokens: 2000,

    // Knowledge Base
    knowledgeBaseSize: 0, // Updated via RAG ingestion
    totalChunks: 0, // Number of embedded chunks
    lastTrainingDate: Date.now(),
    embeddingModel: "text-embedding-3-large",
    embeddingDim: 3072,

    // Training Sources
    trainingSources: {
      website: "https://one.ie",
      contentTypes: [
        "blog_post",
        "video",
        "podcast",
        "course",
        "lesson",
        "social_post",
        "email"
      ],
      includeTranscripts: true,
      includeComments: true,
      includeCodeExamples: true
    },

    // Performance Metrics
    totalInteractions: 0,
    totalMessages: 0,
    averageResponseTime: 0, // milliseconds
    satisfactionScore: 0, // 0-100
    feedbackCount: 0,

    // Capabilities
    capabilities: [
      "natural_conversation",
      "voice_interaction",
      "video_avatar",
      "content_generation",
      "course_teaching",
      "code_assistance",
      "community_engagement"
    ],

    // Status
    status: "training" | "ready" | "active" | "paused",
    publiclyAvailable: false, // Initially private for testing

    // Limitations
    maxConcurrentConversations: 10,
    rateLimit: {
      messagesPerMinute: 10,
      tokensPerDay: 100000
    }
  },
  status: "active",
  createdAt: Date.now(),
  updatedAt: Date.now()
}
```

---

## Connections (Relationships)

### 1. Ownership
```typescript
{
  fromThingId: Id<"things">, // Anthony (creator entity)
  toThingId: Id<"things">, // AI Clone
  relationshipType: "owns",
  createdAt: Date.now()
}
```

### 2. Clone Relationship
```typescript
{
  fromThingId: Id<"things">, // AI Clone
  toThingId: Id<"things">, // Anthony (creator entity)
  relationshipType: "clone_of",
  metadata: {
    fidelity: "high", // How accurate the clone is
    lastSyncDate: Date.now(),
    divergenceScore: 0 // How much clone differs from original
  },
  createdAt: Date.now()
}
```

### 3. Powered By Strategy Agent
```typescript
{
  fromThingId: Id<"things">, // Strategy Agent (director)
  toThingId: Id<"things">, // AI Clone
  relationshipType: "powers",
  metadata: {
    orchestrationRole: "director",
    capabilities: ["rag_retrieval", "personality_modeling", "context_awareness"]
  },
  createdAt: Date.now()
}
```

### 4. Trained On Content
```typescript
// Multiple connections for each piece of content
{
  fromThingId: Id<"things">, // AI Clone
  toThingId: Id<"things">, // Content entity (blog, video, etc.)
  relationshipType: "trained_on",
  metadata: {
    contentType: "blog_post",
    chunksGenerated: 12,
    embeddingCompleted: true,
    lastIngested: Date.now()
  },
  createdAt: Date.now()
}
```

### 5. Knowledge Base Links
```typescript
// Links to knowledge items (chunks)
{
  fromThingId: Id<"things">, // AI Clone
  toThingId: Id<"knowledge">, // Knowledge chunk
  relationshipType: "uses_knowledge",
  metadata: {
    relevanceScore: 0.95,
    retrievalCount: 0, // How often this chunk is retrieved
    lastUsed: Date.now()
  },
  createdAt: Date.now()
}
```

---

## Events (Actions & Interactions)

### Clone Lifecycle Events

#### 1. Clone Created
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "clone_created",
  timestamp: Date.now(),
  actorType: "user",
  actorId: Id<"things">, // Anthony
  metadata: {
    voiceProvider: "elevenlabs",
    appearanceProvider: "d-id",
    initialCapabilities: ["natural_conversation"]
  }
}
```

#### 2. Voice Cloned
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "voice_cloned",
  timestamp: Date.now(),
  actorType: "system",
  metadata: {
    provider: "elevenlabs",
    voiceId: "voice_abc123",
    sampleCount: 25,
    trainingDuration: 300000, // ms
    quality: "high"
  }
}
```

#### 3. Appearance Cloned
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "appearance_cloned",
  timestamp: Date.now(),
  actorType: "system",
  metadata: {
    provider: "d-id",
    avatarId: "avatar_xyz789",
    sourceImages: 10,
    resolution: "1080p",
    animationQuality: "natural"
  }
}
```

#### 4. Knowledge Ingested
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "knowledge_ingested",
  timestamp: Date.now(),
  actorType: "system",
  metadata: {
    sourceThingId: Id<"things">, // Content entity
    contentType: "blog_post",
    chunksCreated: 12,
    tokensProcessed: 9600,
    embeddingModel: "text-embedding-3-large"
  }
}
```

### Interaction Events

#### 5. Clone Interaction
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "clone_interaction",
  timestamp: Date.now(),
  actorType: "user",
  actorId: Id<"things">, // User who interacted
  metadata: {
    conversationId: string,
    messageCount: 5,
    durationMs: 180000,
    voiceUsed: true,
    videoUsed: false,
    satisfaction: 4.5, // 0-5 rating
    topicsDiscussed: ["AI agents", "token economy", "course creation"]
  }
}
```

#### 6. Message Sent
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "message_sent",
  timestamp: Date.now(),
  actorType: "ai_agent",
  actorId: Id<"things">, // AI Clone
  metadata: {
    conversationId: string,
    messageId: string,
    promptTokens: 450,
    completionTokens: 230,
    responseTimeMs: 1200,
    knowledgeChunksUsed: 3,
    confidenceScore: 0.87
  }
}
```

#### 7. Feedback Received
```typescript
{
  thingId: Id<"things">, // AI Clone
  eventType: "feedback_received",
  timestamp: Date.now(),
  actorType: "user",
  actorId: Id<"things">, // User who gave feedback
  metadata: {
    conversationId: string,
    messageId: string,
    rating: 5, // 1-5 stars
    helpful: true,
    accurate: true,
    tone: "authentic",
    comments: "Felt like talking to Anthony!"
  }
}
```

---

## RAG Ingestion Strategy

### Content Sources for Training

**From one.ie (Anthony's existing content):**
- Blog posts (markdown with frontmatter)
- Video transcripts (YouTube, Loom)
- Podcast episodes (audio transcripts)
- Course materials (lessons, exercises)
- Code examples (GitHub repositories)
- Social media posts (Twitter, LinkedIn)
- Email newsletters (past communications)

**Ingestion Pipeline:**

```typescript
// Step 1: Schedule ingestion for all Anthony's content
export const scheduleCloneTraining = mutation({
  args: { cloneId: v.id("things") },
  handler: async (ctx, { cloneId }) => {
    // Get all content authored by Anthony
    const authoredContent = await ctx.db
      .query("connections")
      .withIndex("from_type", q =>
        q.eq("fromThingId", ANTHONY_CREATOR_ID)
         .eq("relationshipType", "authored")
      )
      .collect();

    // Schedule embedding for each piece of content
    for (const conn of authoredContent) {
      await ctx.scheduler.runAfter(0, internal.rag.ingestThing, {
        thingId: conn.toThingId,
        cloneId,
        fields: ["title", "content", "transcript", "description"]
      });
    }
  }
});

// Step 2: Ingest content into knowledge base
export const ingestThing = internalAction({
  args: {
    thingId: v.id("things"),
    cloneId: v.id("things"),
    fields: v.array(v.string())
  },
  handler: async (ctx, { thingId, cloneId, fields }) => {
    const thing = await ctx.runQuery(internal.entities.get, { id: thingId });
    const texts = extractTexts(thing, fields);

    let totalChunks = 0;
    let index = 0;

    for (const t of chunk(texts, { size: 800, overlap: 200 })) {
      // Embed text
      const { embedding, model, dim } = await ctx.runAction(
        internal.rag.embedText,
        { text: t.text, model: "text-embedding-3-large" }
      );

      // Create knowledge item
      const knowledgeId = await ctx.runMutation(internal.rag.upsertKnowledge, {
        item: {
          knowledgeType: "chunk",
          text: t.text,
          embedding,
          embeddingModel: model,
          embeddingDim: dim,
          sourceThingId: thingId,
          sourceField: t.field,
          chunk: {
            index,
            tokenCount: t.tokens,
            overlap: 200
          },
          labels: [thing.type, ...t.labels],
          metadata: {
            hash: hashText(t.text), // For deduplication
            quality: calculateQualityScore(t.text),
            createdAt: Date.now()
          }
        }
      });

      // Link knowledge to clone
      await ctx.runMutation(internal.rag.linkThingKnowledge, {
        thingId: cloneId,
        knowledgeId,
        role: "uses_knowledge"
      });

      // Create trained_on connection
      await ctx.runMutation(internal.connections.create, {
        fromThingId: cloneId,
        toThingId: thingId,
        relationshipType: "trained_on",
        metadata: {
          contentType: thing.type,
          chunksGenerated: 1,
          embeddingCompleted: true,
          lastIngested: Date.now()
        }
      });

      index++;
      totalChunks++;
    }

    // Log ingestion event
    await ctx.runMutation(internal.events.create, {
      thingId: cloneId,
      eventType: "knowledge_ingested",
      timestamp: Date.now(),
      actorType: "system",
      metadata: {
        sourceThingId: thingId,
        contentType: thing.type,
        chunksCreated: totalChunks,
        tokensProcessed: totalChunks * 800,
        embeddingModel: "text-embedding-3-large"
      }
    });

    // Update clone knowledge stats
    await ctx.runMutation(internal.entities.patch, {
      id: cloneId,
      updates: {
        properties: {
          totalChunks: (thing.properties.totalChunks || 0) + totalChunks,
          knowledgeBaseSize: (thing.properties.knowledgeBaseSize || 0) + (totalChunks * 800),
          lastTrainingDate: Date.now()
        }
      }
    });
  }
});
```

### RAG Retrieval During Conversations

```typescript
// Query: Retrieve relevant knowledge for user message
export const retrieveKnowledge = query({
  args: {
    cloneId: v.id("things"),
    query: v.string(),
    topK: v.optional(v.number()),
    filters: v.optional(v.object({
      contentTypes: v.optional(v.array(v.string())),
      minRelevance: v.optional(v.number())
    }))
  },
  handler: async (ctx, { cloneId, query, topK = 5, filters }) => {
    // 1. Embed user query
    const queryEmbedding = await ctx.runAction(internal.rag.embedText, {
      text: query,
      model: "text-embedding-3-large"
    });

    // 2. Get all knowledge linked to clone
    const knowledgeLinks = await ctx.db
      .query("connections")
      .withIndex("from_type", q =>
        q.eq("fromThingId", cloneId)
         .eq("relationshipType", "uses_knowledge")
      )
      .collect();

    // 3. Vector search over knowledge
    const results = await vectorSearch({
      knowledgeIds: knowledgeLinks.map(k => k.toThingId),
      queryEmbedding: queryEmbedding.embedding,
      topK,
      filters
    });

    // 4. Hybrid scoring: semantic + lexical
    const scoredResults = results.map(r => ({
      ...r,
      hybridScore: (r.semanticScore * 0.7) + (r.lexicalScore * 0.3)
    })).sort((a, b) => b.hybridScore - a.hybridScore);

    return scoredResults.slice(0, topK);
  }
});

// Mutation: Generate response with RAG context
export const generateResponse = mutation({
  args: {
    cloneId: v.id("things"),
    conversationId: v.string(),
    userMessage: v.string(),
    useVoice: v.optional(v.boolean())
  },
  handler: async (ctx, { cloneId, conversationId, userMessage, useVoice }) => {
    // 1. Retrieve relevant knowledge
    const knowledge = await ctx.db.query(api.rag.retrieveKnowledge, {
      cloneId,
      query: userMessage,
      topK: 5
    });

    // 2. Build context from knowledge chunks
    const context = knowledge.map(k => k.text).join("\n\n");

    // 3. Get clone configuration
    const clone = await ctx.db.get(cloneId);

    // 4. Generate response
    const completion = await ctx.scheduler.runAfter(0, internal.ai.generateCompletion, {
      systemPrompt: clone.properties.systemPrompt,
      context,
      userMessage,
      temperature: clone.properties.temperature,
      maxTokens: clone.properties.maxTokens
    });

    // 5. Log interaction event
    await ctx.db.insert("events", {
      thingId: cloneId,
      eventType: "message_sent",
      timestamp: Date.now(),
      actorType: "ai_agent",
      actorId: cloneId,
      metadata: {
        conversationId,
        messageId: generateId(),
        promptTokens: completion.promptTokens,
        completionTokens: completion.completionTokens,
        responseTimeMs: completion.responseTimeMs,
        knowledgeChunksUsed: knowledge.length,
        confidenceScore: completion.confidenceScore
      }
    });

    // 6. Optionally generate voice
    if (useVoice) {
      const audio = await ctx.scheduler.runAfter(0, internal.voice.synthesize, {
        text: completion.text,
        voiceId: clone.properties.voiceId,
        voiceSettings: clone.properties.voiceSettings
      });

      return {
        text: completion.text,
        audio: audio.url,
        metadata: completion.metadata
      };
    }

    return {
      text: completion.text,
      metadata: completion.metadata
    };
  }
});
```

---

## Training Requirements

### Voice Cloning (ElevenLabs)

**Requirements:**
- 25-30 high-quality audio samples
- Each sample: 10-30 seconds
- Total duration: 5-10 minutes
- Clear speech, minimal background noise
- Varied emotions and tones

**Sources:**
- Podcast episodes from Bull FM
- Video recordings from one.ie
- Loom videos with clear audio
- Zoom recordings from courses

**Process:**
```typescript
export const cloneVoice = internalAction({
  args: {
    cloneId: v.id("things"),
    audioSamples: v.array(v.string()) // URLs to audio files
  },
  handler: async (ctx, { cloneId, audioSamples }) => {
    const elevenlabs = yield* ElevenLabsProvider;

    // 1. Create voice clone
    const voice = await elevenlabs.createVoice({
      name: "Anthony O'Connell Clone",
      samples: audioSamples,
      description: "AI clone of Anthony O'Connell for ONE Platform"
    });

    // 2. Update clone entity
    await ctx.runMutation(internal.entities.patch, {
      id: cloneId,
      updates: {
        properties: {
          voiceId: voice.voiceId,
          voiceProvider: "elevenlabs",
          voiceModel: voice.model
        }
      }
    });

    // 3. Log event
    await ctx.runMutation(internal.events.create, {
      thingId: cloneId,
      eventType: "voice_cloned",
      timestamp: Date.now(),
      actorType: "system",
      metadata: {
        provider: "elevenlabs",
        voiceId: voice.voiceId,
        sampleCount: audioSamples.length,
        trainingDuration: voice.trainingTime,
        quality: voice.quality
      }
    });

    return voice;
  }
});
```

### Appearance Cloning (D-ID or HeyGen)

**Requirements:**
- 10-15 high-quality photos
- Varied angles and expressions
- Good lighting
- High resolution (1080p+)

**OR:**
- 2-3 minute video clip
- Clear face visibility
- Natural expressions
- Multiple angles

**Process:**
```typescript
export const cloneAppearance = internalAction({
  args: {
    cloneId: v.id("things"),
    sourceImages: v.array(v.string()), // URLs to images
    sourceVideo: v.optional(v.string()) // URL to video
  },
  handler: async (ctx, { cloneId, sourceImages, sourceVideo }) => {
    const didProvider = yield* DIDProvider;

    // 1. Create avatar
    const avatar = await didProvider.createAvatar({
      name: "Anthony O'Connell Clone",
      images: sourceImages,
      video: sourceVideo
    });

    // 2. Update clone entity
    await ctx.runMutation(internal.entities.patch, {
      id: cloneId,
      updates: {
        properties: {
          appearanceId: avatar.id,
          appearanceProvider: "d-id"
        }
      }
    });

    // 3. Log event
    await ctx.runMutation(internal.events.create, {
      thingId: cloneId,
      eventType: "appearance_cloned",
      timestamp: Date.now(),
      actorType: "system",
      metadata: {
        provider: "d-id",
        avatarId: avatar.id,
        sourceImages: sourceImages.length,
        resolution: avatar.resolution,
        animationQuality: avatar.quality
      }
    });

    return avatar;
  }
});
```

### Knowledge Base Training

**Requirements:**
- ALL content from one.ie
- Transcripts from Bull FM podcasts
- Course materials and lessons
- Code examples from GitHub
- Social media posts (Twitter, LinkedIn)
- Email newsletters

**Estimated Stats:**
- ~500 blog posts
- ~100 videos with transcripts
- ~50 podcast episodes
- ~20 courses with ~200 lessons
- ~1000 social posts
- Total: ~50,000 chunks (40M tokens)

**Process:**
See RAG Ingestion Strategy above

---

## Usage Examples

### 1. Natural Conversation
```typescript
// User asks: "How do I create an AI agent on ONE?"
const response = await convex.mutation(api.clone.chat, {
  cloneId: anthonyCloneId,
  message: "How do I create an AI agent on ONE?",
  conversationId: "conv_123"
});

// Response (retrieves from RAG knowledge):
"Great question! Creating an AI agent on ONE is designed to be really straightforward...

[Context retrieved from documentation and blog posts about agent creation]

The key steps are:
1. Define your agent's purpose and capabilities
2. Set up the ontology mapping (things, connections, events)
3. Implement the Effect.ts service with business logic
4. Create Convex wrappers for mutations and queries
5. Build the UI components

Want me to walk you through a specific type of agent?"
```

### 2. Voice Interaction
```typescript
// User sends voice message
const response = await convex.mutation(api.clone.chat, {
  cloneId: anthonyCloneId,
  message: "Tell me about the token economy feature",
  conversationId: "conv_123",
  useVoice: true // Request voice response
});

// Returns:
{
  text: "The token economy is one of my favorite features...",
  audio: "https://storage.com/audio_response.mp3", // ElevenLabs generated
  metadata: { ... }
}
```

### 3. Video Avatar
```typescript
// Generate video response with D-ID avatar
const video = await convex.action(api.clone.generateVideo, {
  cloneId: anthonyCloneId,
  script: "Welcome to ONE Platform! I'm Anthony's AI clone...",
  avatarId: clone.properties.appearanceId
});

// Returns video URL with Anthony's avatar speaking the script
```

### 4. Content Generation
```typescript
// Generate blog post in Anthony's style
const blogPost = await convex.mutation(api.clone.generateContent, {
  cloneId: anthonyCloneId,
  contentType: "blog_post",
  topic: "Building AI-First Creator Tools",
  tone: "technical but accessible",
  length: 1500
});

// Uses RAG to retrieve Anthony's writing style and past posts
// Generates new content that matches his voice
```

---

## Summary

**Anthony's AI Clone Capabilities:**

1. **Voice Cloning** - Natural speech with Anthony's voice via ElevenLabs
2. **Appearance Cloning** - Video avatar with Anthony's likeness via D-ID
3. **Knowledge Base** - RAG-powered retrieval from ALL one.ie content (~40M tokens)
4. **Natural Conversations** - Context-aware responses using GPT-4/Claude
5. **Content Generation** - Blog posts, course lessons, emails in Anthony's style
6. **Community Engagement** - Answer questions, provide guidance, build relationships
7. **Teaching** - Deliver courses and lessons as Anthony would

**Training Requirements:**

- **Voice:** 25-30 audio samples (5-10 min total) from podcasts and videos
- **Appearance:** 10-15 photos OR 2-3 min video clip
- **Knowledge:** Ingest ~500 blog posts, ~100 videos, ~50 podcasts, ~20 courses
- **Time:** ~1-2 weeks for initial training, ongoing updates

**Ontology Integration:**
- Thing type: `ai_clone`
- Connections: `owns`, `clone_of`, `powers`, `trained_on`, `uses_knowledge`
- Events: `clone_created`, `voice_cloned`, `appearance_cloned`, `knowledge_ingested`, `clone_interaction`, `message_sent`, `feedback_received`

**Result:** A highly authentic AI representation of Anthony that can handle conversations, generate content, teach courses, and engage with the ONE community while maintaining his voice, personality, and expertise.
