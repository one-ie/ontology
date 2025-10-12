# Plan 1: Agent-Based Ontology-Driven Workflow System

**Director:** Engineering Director Agent (agent-director.md)
**Team:**
- Backend Specialist (agent-backend.md) - orchestrator, event system
- Integration Specialist (agent-integration.md) - coordination, knowledge management
- Quality Agent (agent-quality.md) - test definition, validation
- Problem Solver (agent-problem-solver.md) - ultrathink analysis, fixes
- Documenter (agent-documenter.md) - documentation

**Duration:** 4-6 weeks
**Status:** In Progress
**Created from:** `ideas/1-create-workflow.md`

---

## Executive Summary

Build the complete workflow system that enables agents to autonomously:
1. Validate ideas against ontology
2. Create plans and assign features
3. Write specifications and define tests
4. Design wireframes that enable tests to pass
5. Implement code and validate quality
6. Solve problems and accumulate knowledge

**Goal:** 100x simpler, 5x faster, continuous learning, YAML-configurable

---

## Features Overview

### 1-1: Agent Prompts System
**Owner:** Backend Specialist
**Priority:** Critical
**Dependencies:** None

Define prompts for 8 agent roles:
- Engineering Director (agent-director.md) - validates ideas, creates plans, assigns work
- Backend Specialist (agent-backend.md) - services, mutations, queries, schemas
- Frontend Specialist (agent-frontend.md) - pages, components, UI/UX
- Integration Specialist (agent-integration.md) - connections, data flows, workflows
- Quality Agent (agent-quality.md) - defines tests, validates implementations
- Design Agent (agent-designer.md) - creates wireframes from test criteria
- Problem Solver (agent-problem-solver.md) - ultrathink mode for failures
- Documenter (agent-documenter.md) - writes documentation

Each prompt specifies:
- Role and responsibilities
- Input/output expectations
- Context requirements (token budget)
- Decision-making framework
- Communication patterns

**Files:**
- `one/things/agents/agent-director.md` ✅
- `one/things/agents/agent-backend.md` ✅
- `one/things/agents/agent-frontend.md` (enhance existing)
- `one/things/agents/agent-integration.md` ✅
- `one/things/agents/agent-quality.md` ✅
- `one/things/agents/agent-designer.md` (enhance existing)
- `one/things/agents/agent-problem-solver.md` ✅
- `one/things/agents/agent-documenter.md` ✅

---

### 1-2: YAML-Driven Orchestrator
**Owner:** Backend Specialist
**Priority:** Critical
**Dependencies:** 1-1 (needs agent definitions)

Build the orchestrator that:
- Reads workflow configuration from `ontology-minimal.yaml`
- Executes 6-level flow (ideas → plans → features → tests → design → implementation)
- Routes work to appropriate agents
- Manages parallel execution
- Monitors progress via events

**Target:** ~150 lines of TypeScript (vs 15,000+ in old system)

**Key components:**
- `AgentOrchestrator` class
- `loadWorkflowConfig()` - reads YAML
- `executeStage()` - runs each workflow level
- `runAgent()` - executes agent with context
- `runAgentParallel()` - parallel execution

**Files:**
- `one/workflows/orchestrator.ts`
- `one/connections/ontology-minimal.yaml` (workflow section)

---

### 1-3: Event Coordination System
**Owner:** Backend Specialist
**Priority:** High
**Dependencies:** 1-2 (orchestrator needs events)

Implement event-driven coordination:
- Events table as message bus
- 20+ workflow event types
- Agents query events to coordinate
- Complete audit trail
- No external coordination system needed

**Event types:**
- `plan_started`, `feature_assigned`, `feature_started`
- `implementation_complete`, `quality_check_started`, `quality_check_complete`
- `test_started`, `test_passed`, `test_failed`
- `problem_analysis_started`, `solution_proposed`
- `fix_started`, `fix_complete`, `lesson_learned_added`
- `documentation_started`, `documentation_complete`
- `feature_complete`

**Files:**
- `one/events/workflow/` (event log directory)
- `one/events/completed/` (completion events)
- Event type definitions in ontology YAML

---

### 1-4: Knowledge Management System
**Owner:** Integration Specialist
**Priority:** High
**Dependencies:** 1-5 (problem solver adds lessons)

Build knowledge accumulation system:
- `lessons-learned.md` structure
- Pattern library organization
- Lesson capture after fixes
- Knowledge search for agents
- Continuous learning loop

**Structure:**
```markdown
# Lessons Learned

## Backend Patterns
### Pattern Name
- Problem: What went wrong
- Solution: How it was fixed
- Rule: Principle to follow
- Example: Code snippet

## Frontend Patterns
...

## Testing Patterns
...
```

**Files:**
- `one/knowledge/lessons-learned.md`
- `one/knowledge/patterns/backend/`
- `one/knowledge/patterns/frontend/`
- `one/knowledge/patterns/design/`
- `one/knowledge/patterns/test/`

---

### 1-5: Quality Loops and Problem Solving
**Owner:** Backend Specialist + Documentation Specialist
**Priority:** High
**Dependencies:** 1-1, 1-3 (needs quality agent + events)

Implement quality validation and problem solving:
- Quality agent validates against ontology
- Defines user flows and acceptance criteria
- Runs tests after implementation
- Problem solver analyzes failures (ultrathink)
- Proposes solutions, delegates fixes
- Captures lessons learned

**Flow:**
```
Implementation → Quality validates → Tests run
  → PASS: Documenter writes docs
  → FAIL: Problem solver analyzes → Proposes fix → Specialist fixes
    → Add to lessons learned → Re-test
```

**Files:**
- Quality agent logic in orchestrator
- Problem solver prompt (ultrathink mode)
- Test execution framework
- Fix delegation system

---

### 1-6: Numbering and File Structure
**Owner:** Integration Specialist
**Priority:** Medium
**Dependencies:** 1-2 (orchestrator creates files)

Implement hierarchical numbering system:
- Plans: `1-plan-name`
- Features: `1-1-feature-name`, `1-2-feature-name`
- Task lists: `1-1-feature-name-tasks`
- Individual tasks: `1-1-task-1`, `1-1-task-2`
- Events: `events/1-1-feature-name-complete.md`

**Directory structure:**
```
one/
├── things/
│   ├── agents/          # Agent prompts
│   ├── ideas/           # Generated ideas
│   ├── plans/           # Generated plans
│   └── features/        # Generated features
├── knowledge/
│   ├── patterns/        # Implementation patterns
│   └── lessons-learned.md
├── events/
│   ├── workflow/        # Real-time event log
│   └── completed/       # Completion events
├── connections/
│   └── ontology-minimal.yaml
└── workflows/
    └── orchestrator.ts
```

**Benefits:**
- Clear hierarchy
- Easy tracking
- Git-friendly
- Searchable
- Scalable

---

## Architecture (from Ontology)

### Things
- `agent`, `idea`, `plan`, `feature`, `test`, `design`, `task`, `lesson`

### Connections
- `validates`, `creates`, `assigns_to`, `part_of`, `tests_for`, `designs_for`
- `implements`, `reviews`, `solves`, `documents`

### Events
- 20+ workflow event types for coordination
- Events table IS the message bus
- No external coordination needed

### Knowledge
- Agent prompts (behavior definitions)
- Pattern library (implementation guidance)
- Lessons learned (institutional knowledge)
- Event history (audit trail)

---

## Success Metrics

### Phase 1 (Weeks 1-2): Foundation
- [ ] 6 agent prompts written and tested
- [ ] Basic orchestrator reads YAML and executes
- [ ] Numbering system creates files correctly
- [ ] Events logged to track progress
- [ ] Can complete simple idea → plan → feature flow

### Phase 2 (Weeks 2-3): Quality Loops
- [ ] Quality agent defines tests
- [ ] Design agent creates wireframes
- [ ] Tests run and validate implementations
- [ ] Problem solver handles failures
- [ ] Lessons learned captured

### Phase 3 (Weeks 3-4): Integration
- [ ] Complete workflow idea → implementation works
- [ ] Parallel execution of tasks
- [ ] Event-driven coordination autonomous
- [ ] Knowledge base accumulates patterns
- [ ] 5x faster than manual process

### Phase 4 (Weeks 4-6): Refinement
- [ ] Test on real features
- [ ] Measure context reduction (target: 98%)
- [ ] Measure speed improvement (target: 5x)
- [ ] Gather developer feedback
- [ ] Iterate and improve
- [ ] Documentation complete

---

## Implementation Order

### Week 1: Agent Prompts + Basic Orchestrator
1. **1-1 (Agent Prompts):** Write all 6 agent role prompts
2. **1-6 (File Structure):** Set up directory structure
3. **1-2 (Orchestrator):** Build basic flow (ideas → plans → features)

### Week 2: Events + Quality
4. **1-3 (Events):** Implement event coordination
5. **1-5 (Quality):** Add quality validation and testing

### Week 3: Problem Solving + Knowledge
6. **1-5 (Problem Solver):** Ultrathink mode for failures
7. **1-4 (Knowledge):** Lessons learned system

### Week 4-6: Testing + Refinement
8. Test complete workflow on sample features
9. Measure performance and quality
10. Iterate based on real usage
11. Write documentation

---

## Parallel Execution

**Tasks that can run in parallel:**

Week 1:
- 1-1 (Agent Prompts) + 1-6 (File Structure) can run simultaneously
- 1-2 (Orchestrator) depends on both

Week 2:
- 1-3 (Events) + 1-5 (Quality basics) can run simultaneously

Week 3:
- 1-5 (Problem Solver) + 1-4 (Knowledge) can run simultaneously

---

## Risk Management

### Risk: Complexity Creep
**Mitigation:** Keep orchestrator under 200 lines. If more complex, simplify workflow.

### Risk: Agents Don't Coordinate Well
**Mitigation:** Use simple event patterns. Add more event types if needed.

### Risk: Knowledge Base Not Used
**Mitigation:** Make problem solver always reference lessons learned. Show value immediately.

### Risk: Too Slow
**Mitigation:** Measure at each phase. Optimize context loading. Parallelize aggressively.

### Risk: Developers Reject It
**Mitigation:** Get feedback early. Make it optional. Prove value with metrics.

---

## Next Steps

1. **Feature 1-1:** Create agent prompt specifications
2. **Feature 1-6:** Set up file structure
3. **Feature 1-2:** Build basic orchestrator
4. **Feature 1-3:** Add event coordination
5. **Feature 1-5:** Implement quality loops
6. **Feature 1-4:** Build knowledge management

Each feature will have:
- Feature specification (what we're building)
- Tests (user flows + acceptance criteria)
- Design (structure and components)
- Implementation (working code)

---

## References

- **Idea:** `one/things/ideas/1-create-workflow.md`
- **Workflow spec:** `one/things/plans/workflow.md`
- **Ontology:** `one/connections/ontology-minimal.yaml`

---

**Status:** Ready to proceed to Feature specs (Level 3)

**Director note:** This plan uses the workflow system to build itself. Each feature follows the 6-level flow. We're bootstrapping our way to infinity.
