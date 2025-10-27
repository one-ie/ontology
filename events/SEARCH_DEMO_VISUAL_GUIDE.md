# Search/Knowledge Demo - Visual Structure Guide

## Page Layout

```
┌─────────────────────────────────────────────────────────────────────┐
│                          DEMO CONTAINER                             │
│  Header: "Knowledge: Semantic Search & RAG"                         │
│  Description: "Dimension 6 of 6 - Vector embeddings..."             │
│  Status: Connected (42ms latency) | Refresh Button                  │
│  Backend URL: https://veracious-marlin-319.convex.cloud            │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                        HERO SECTION                                  │
│  Icon: 🔍 Search                                                    │
│  Title: "Knowledge: The 6th Dimension"                              │
│  Description: "Vector embeddings, semantic search..."               │
│  Badges: [Semantic Search] [RAG Ready] [Vector Embeddings]         │
│  CTA: "Scroll to Playground"                                        │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                  WHAT IS KNOWLEDGE SECTION                          │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  🔍 Full-Text Search                                           │ │
│  │  Search across all text content in the system with keyword    │ │
│  │  matching and filtering                                       │ │
│  └────────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  📊 Vector Embeddings                                          │ │
│  │  Semantic similarity search using high-dimensional embeddings  │ │
│  │  for meaning-based results                                    │ │
│  └────────────────────────────────────────────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │  ⚡ RAG Pipeline                                               │ │
│  │  Retrieve augmented generation combining semantic search      │ │
│  │  with LLM generation for AI-powered responses                 │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│            INTERACTIVE KNOWLEDGE PLAYGROUND                         │
│                                                                      │
│  LEFT COLUMN (1/3)                 RIGHT COLUMN (2/3)              │
│  ┌──────────────────────────┐    ┌──────────────────────────────┐  │
│  │    ACTIONS (collapsible) │    │ DATA VIEW (list/grid toggle) │  │
│  ├──────────────────────────┤    ├──────────────────────────────┤  │
│  │                          │    │                              │  │
│  │ Knowledge Content        │    │ Recent Knowledge Items (5)   │  │
│  │ [textarea 24h height]    │    │ ┌──────────────────────────┐ │  │
│  │                          │    │ │ Knowledge Item 1         │ │  │
│  │ Labels (Categories)      │    │ │ Labels: [ai][ml][tut]    │ │  │
│  │ [text input]  [Add]      │    │ │ Date: 10/25/2025         │ │  │
│  │ [Tag] [Tag] [Tag] [x]    │    │ └──────────────────────────┘ │  │
│  │                          │    │ ┌──────────────────────────┐ │  │
│  │ [Create Knowledge Item]  │    │ │ Knowledge Item 2         │ │  │
│  │                          │    │ └──────────────────────────┘ │  │
│  │ ℹ️ Knowledge items are   │    │ ... (more items)            │  │
│  │ indexed immediately      │    │                              │  │
│  └──────────────────────────┘    └──────────────────────────────┘  │
│                                                                      │
│  Footer: "Use this form..." | "Real-time updates enabled"          │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│              TRY SEMANTIC SEARCH SECTION                            │
│                                                                      │
│  💫 Try Semantic Search                                             │
│  Type a query below to see how semantic search finds relevant       │
│  knowledge across the system. Results ranked by relevance using    │
│  vector similarity.                                                │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ 🔍 Try: 'machine learning', 'embeddings', 'neural networks' │   │
│  │ [Search Input with autocomplete]                             │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  Results:                                                           │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Result 1: "Machine Learning Fundamentals"                   │   │
│  │ Type: [course]  From: ML 101 Course                         │   │
│  │ Score: [████████████████████░] 95%                          │   │
│  │ Content preview...                                           │   │
│  │ #1 of 5 results [View Details →]                            │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Result 2: "Deep Learning Guide"                             │   │
│  │ ... (more results)                                          │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  Found 5 relevant results for "machine learning"                   │
│  Results ranked by semantic similarity. Higher % = better matches. │
│                                                                      │
│  Example searches: [machine learning] [embeddings] [neural...]     │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│           CODE EXAMPLES & API REFERENCE                             │
│                                                                      │
│  💻 Code Examples & API                                             │
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Semantic Search Query         │ Copy                   ts    │   │
│  │                                                              │   │
│  │ // Search knowledge with semantic matching                  │   │
│  │ import { useKnowledgeSearch } from '@/hooks/...'            │   │
│  │                                                              │   │
│  │ ... (code example continues)                                │   │
│  │                                                              │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Create Knowledge Item         │ Copy                   ts    │   │
│  │ ... (code example)                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Retrieve for RAG              │ Copy                   ts    │   │
│  │ ... (code example)                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Filter with Labels            │ Copy                   ts    │   │
│  │ ... (code example)                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ REST API: Search              │ Copy                  bash   │   │
│  │ ... (curl example)                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ REST API: Create              │ Copy                  bash   │   │
│  │ ... (curl example)                                           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  Key API Endpoints:                                                │
│  [POST /api/knowledge] [POST /api/knowledge/search]                │
│  [GET /api/knowledge/:id] [POST /api/knowledge/retrieve]           │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│           KNOWLEDGE STATISTICS & INSIGHTS                           │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │ 📊 Total Items │ ⚡ Avg Latency │ 📈 Embeddings │ 🎯 Accuracy   │   │
│  │      42        │     42ms       │    1.5K dims   │    94%        │   │
│  │ knowledge      │ semantic       │ dimensions     │ relevance     │   │
│  │ items indexed  │ search         │ per vector     │ score         │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                                                                      │
│  How Semantic Search Works:                                         │
│  1. Query converted to 1,536-dim vector                             │
│  2. Database searched for similar vectors                           │
│  3. Results ranked by cosine similarity                             │
│  4. Top matches returned with scores                                │
│                                                                      │
│  Vector Similarity Scoring:                                         │
│  100% = Exact semantic match                                        │
│  80-99% = Very similar meaning                                      │
│  60-79% = Related concepts                                          │
│  <60% = Marginally relevant                                         │
│                                                                      │
│  RAG Pipeline:                                                      │
│  Retrieve: Find relevant knowledge                                  │
│  Augment: Inject into LLM context                                   │
│  Generate: LLM creates response                                     │
│  Cite: Original sources referenced                                  │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                     USE CASES & FEATURES                            │
│                                                                      │
│  💡 Use Cases                   ⚡ Technical Features               │
│  ✓ AI document search           ✓ Vector embeddings (1,536 dims)   │
│  ✓ Semantic similarity          ✓ Full-text search (BM25)          │
│  ✓ Intelligent autocomplete     ✓ Faceted search                   │
│  ✓ RAG for LLMs                 ✓ Cosine similarity ranking        │
│  ✓ Content discovery            ✓ Real-time indexing              │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│         EXPLORED ALL 6 DIMENSIONS!                                  │
│                                                                      │
│  [Groups] [People] [Things] [Connections] [Events] [Knowledge]     │
│                                                                      │
│  The complete 6-dimension ontology powers every feature in the     │
│  ONE Platform. Ready to build?                                      │
│                                                                      │
│  [Back to Demo Home]                                                │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                      NAVIGATION LINKS                               │
│                                                                      │
│  ← Back to Demo Home  •  Ontology Specification  •  Full API Docs  │
└─────────────────────────────────────────────────────────────────────┘
```

## Component Sizes

### Responsive Breakpoints

**Mobile (< 640px)**
```
┌─────────────────┐
│   Hero Section  │ Full width
├─────────────────┤
│   Info Cards    │ Stacked 1 column
├─────────────────┤
│   Playground    │ Stacked form over data
├─────────────────┤
│   Search        │ Full width
├─────────────────┤
│   Code Examples │ 1 column
├─────────────────┤
│   Statistics    │ Stacked
└─────────────────┘
```

**Tablet (640px - 1024px)**
```
┌──────────────────────────────────┐
│        Hero Section              │ Full width
├──────────────────────────────────┤
│  Info Cards (2 columns)          │
├──────────────────────────────────┤
│  Playground (2 column layout)    │
│  Left 50% | Right 50%            │
├──────────────────────────────────┤
│  Search (full width)             │
├──────────────────────────────────┤
│  Code Examples (2 columns)       │
├──────────────────────────────────┤
│  Statistics (3 columns)          │
└──────────────────────────────────┘
```

**Desktop (> 1024px)**
```
┌──────────────────────────────────────────────┐
│            Hero Section                      │
├──────────────────────────────────────────────┤
│  Info Card 1  │  Info Card 2  │  Info Card 3│
├──────────────────────────────────────────────┤
│  Playground Layout                           │
│  Left (33%) Form │ Right (67%) Data         │
├──────────────────────────────────────────────┤
│  Search (full width)                         │
├──────────────────────────────────────────────┤
│  Code Examples (2x3 grid)                    │
│  Example 1 │ Example 2                       │
│  Example 3 │ Example 4                       │
│  Example 5 │ Example 6                       │
├──────────────────────────────────────────────┤
│  Stats: 5 equal width columns                │
│  Items │ Latency │ Embedding │ Accuracy │ St│
└──────────────────────────────────────────────┘
```

## Color Scheme

### Light Mode
```
Background:     #ffffff (white)
Text Primary:   #1e293b (slate-900)
Text Secondary: #64748b (slate-600)
Accent Blue:    #3b82f6
Accent Purple:  #a855f7
Success Green:  #16a34a
Warning Yellow: #ca8a04
Error Red:      #dc2626
Neutral Bg:     #f1f5f9 (slate-100)
Border:         #e2e8f0 (slate-200)
```

### Dark Mode
```
Background:     #1e293b (slate-800)
Text Primary:   #f1f5f9 (slate-100)
Text Secondary: #cbd5e1 (slate-400)
Accent Blue:    #60a5fa (blue-400)
Accent Purple:  #c084fc (purple-400)
Success Green:  #4ade80 (green-400)
Warning Yellow: #facc15 (yellow-400)
Error Red:      #f87171 (red-400)
Neutral Bg:     #0f172a (slate-900/5)
Border:         #334155 (slate-700)
```

## Interactive States

### Forms
```
Normal:    White bg, slate border, slate text
Hover:    Light gray bg, slate-300 border
Focus:    Blue ring, blue border, blue accent
Disabled: Gray bg, gray text, cursor not-allowed
Error:    Red border, red text, red icon
Success:  Green border, green bg
Loading:  Spinner animation, disabled state
```

### Buttons
```
Primary:  Blue gradient bg, white text, blue hover
Secondary: Slate bg, slate text, slate-200 hover
Ghost:    Transparent, colored text, colored bg on hover
Disabled: Gray, cursor not-allowed, reduced opacity
Loading:  Spinner icon, disabled state
```

### Cards
```
Resting:   White bg, slate border, slate shadow
Hover:     Slight shadow increase, border color change
Focus:     Blue ring, blue border
Selected:  Blue bg, white text
Disabled:  Gray bg, gray text
Error:     Red border, red bg tint
```

## Typography

```
Headings:
  H1 (Hero):        5xl, bold, slate-900
  H2 (Sections):    2xl, bold, slate-900
  H3 (Subsections): lg, semibold, slate-900
  H4 (Cards):       base, semibold, slate-900

Body:
  Large:            lg, slate-600
  Regular:          base, slate-600
  Small:            sm, slate-500
  Code:             mono, slate-600, bg-slate-100

Links:
  Default:          blue-600, underline on hover
  Dark mode:        blue-400, underline on hover
```

## Spacing System

```
Padding:
  Cards:     p-6 (24px)
  Sections:  p-8 (32px)
  Forms:     p-4 (16px)
  Buttons:   px-4 py-2 (16px x 8px)

Margins:
  Sections:  mb-12 (48px between)
  Cards:     mb-6 (24px between)
  Items:     mb-3 (12px between)
  Text:      mb-2 (8px between)

Gaps:
  Grid:      gap-4 (16px)
  Cards:     gap-6 (24px)
  Form:      gap-3 (12px)
```

## Visual Hierarchy

```
1. Hero Section (Large, colorful, prominent)
2. Section Headers (Large, bold)
3. Card Headers (Bold, colored)
4. Card Content (Regular, colored text)
5. Metadata (Small, muted)
6. Hints/Help (Smallest, muted)
```

## Animation & Transitions

```
Hover:      150ms ease-in-out
Loading:    Infinite spin animation
Success:    Fade in/out 3 seconds
Error:      Shake on focus
Collapse:   Smooth height transition
```

This visual guide shows the complete layout and styling of the Search/Knowledge demo page with all responsive breakpoints, color schemes, typography, and interactive states.
