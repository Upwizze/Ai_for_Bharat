# CodeSensei Design Document

## Executive Summary

CodeSensei is a lightweight reasoning and learning layer that enhances AI-powered IDEs by making code reasoning, assumptions, and system behavior visible. It operates silently during normal development and provides structured explanations on-demand, reducing token waste, preventing repeated failures, and enabling real learning.

## Problem Statement

Current AI IDEs suffer from:
- Verbose, token-wasting responses
- Silent failures without root cause analysis
- High costs from failed iterations and retries
- Context loss between sessions
- Poor understanding of existing codebases
- Shallow learning that leads to skill decay

## Core Value Propositions

1. **Explainable AI Code** - Generated code is understandable, not a black box
2. **Failure Reasoning** - Bugs are explained structurally, not guessed at
3. **Real-World Learning** - Developers learn while building actual projects
4. **Cost Efficiency** - AI agents become more reliable and use fewer tokens over time
5. **Persistent Context** - System understanding survives across sessions

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     IDE Integration Layer                    │
│              (VS Code Extension - User Interface)            │
└───────────────────────────┬─────────────────────────────────┘
                            │
┌───────────────────────────┴─────────────────────────────────┐
│                    Reasoning Engine Core                     │
│  • Concept Detection    • Assumption Tracking                │
│  • Failure Analysis     • System Layer Classification        │
└───────────────────────────┬─────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
┌───────┴────────┐  ┌──────┴──────┐  ┌────────┴─────────┐
│   Assumption   │  │ Bug Reasoning│  │ Agent Control &  │
│   & Intent     │  │     Mode     │  │    Feedback      │
│   Registry     │  │              │  │   Injection      │
└────────────────┘  └──────────────┘  └──────────────────┘
```

## System Components

### 1. IDE Integration Layer (VS Code Extension)

**Purpose**: Provide developer-facing interface and manual controls

**Key Features**:
- Reasoning panel in IDE sidebar
- Manual action commands:
  - "Explain why this failed"
  - "Show assumptions"
  - "Why this fix didn't work"
  - "Show system flow"
- Status indicators (silent by default)
- Learning mode toggles

**Technical Stack**:
- VS Code Extension API
- TypeScript
- Webview for visual diagrams
- Language Server Protocol integration

**Behavior**:
- Silent by default - no interruptions during coding
- Activates only on explicit developer request
- Minimal UI footprint

### 2. Reasoning Engine

**Purpose**: Core intelligence for concept detection and system understanding

**Capabilities**:

**Concept Detection**:
- Authentication patterns
- Asynchronous execution flows
- Data flow and transformations
- Caching strategies
- State management patterns
- Error handling approaches

**Analysis Modes**:
- **Lightweight Mode** (default): Minimal overhead during normal coding
- **Deep Analysis Mode**: Triggered during reviews or failures

**System Layer Classification**:
- Presentation layer
- Business logic layer
- Data access layer
- Infrastructure layer
- Cross-cutting concerns

**Technical Approach**:
- Static code analysis (AST parsing)
- Pattern matching against known system concepts
- Heuristic-based intent detection
- Minimal runtime overhead

### 3. Assumption & Intent Registry

**Purpose**: Persistent storage of system understanding and architectural decisions

**Data Model**:

```typescript
interface AssumptionRecord {
  id: string;
  codeLocation: CodeReference;
  intent: string;
  assumptions: Assumption[];
  tradeoffs: Tradeoff[];
  createdAt: timestamp;
  lastValidated: timestamp;
  status: 'valid' | 'failed' | 'unknown';
}

interface Assumption {
  description: string;
  type: 'precondition' | 'postcondition' | 'invariant' | 'dependency';
  validationStatus: 'untested' | 'valid' | 'failed';
  failureHistory: FailureRecord[];
}

interface Tradeoff {
  decision: string;
  alternatives: string[];
  rationale: string;
  constraints: string[];
}
```

**Storage Strategy**:
- Project-local storage (`.codesensei/` directory)
- JSON-based for human readability
- Git-friendly format for team collaboration
- Incremental updates to minimize I/O

**Lifecycle**:
- Created during AI code generation or manual annotation
- Updated when code changes
- Marked as failed when assumptions break
- Archived when code is removed

### 4. Bug Reasoning Mode

**Purpose**: Transform debugging from trial-and-error to structured reasoning

**Activation Triggers**:
- Developer explicitly requests explanation
- Multiple failed fix attempts detected
- Test failures with unclear root cause

**Analysis Process**:

1. **Failure Classification**
   - Syntax error vs logic error vs runtime error
   - Expected vs actual behavior comparison
   - System layer identification

2. **Assumption Validation**
   - Check all assumptions related to failing code
   - Identify which assumption(s) failed
   - Trace dependency chain

3. **Historical Context**
   - Review previous fix attempts
   - Identify why they failed
   - Detect retry loops

4. **Constraint Identification**
   - System constraints that must hold
   - Environmental dependencies
   - Integration points

**Output Format**:
```
Bug Reasoning Report
━━━━━━━━━━━━━━━━━━━━

Failed Assumption:
  "User authentication token is always present in request headers"

Why It Failed:
  OAuth refresh flow redirects before token is set

Affected Layer:
  Authentication middleware (Infrastructure)

Previous Fix Attempts:
  ✗ Added null check (didn't address root cause)
  ✗ Increased timeout (wrong layer)

Required Constraints:
  • Token must be validated before middleware chain
  • Refresh flow must complete before protected routes
  • Session state must persist across redirects

Recommended Approach:
  Move token validation to earlier middleware stage
```

### 5. Agent Control & Feedback Injection Layer

**Purpose**: Make existing AI IDE agents more reliable and cost-efficient

**Integration Points**:
- Cursor AI
- GitHub Copilot
- Cody
- Continue.dev
- Other LSP-compatible AI tools

**Pre-Execution Injection**:

```typescript
interface AgentContext {
  knownConstraints: Constraint[];
  failedAssumptions: Assumption[];
  invalidRetryPaths: string[];
  relevantSystemConcepts: Concept[];
  reducedPromptContext: string;
}
```

**Injection Strategy**:
- Intercept AI requests before execution
- Inject structured context into prompts
- Block known-bad solution paths
- Reduce token usage by filtering irrelevant context

**Post-Execution Capture**:
- Tag AI-generated code
- Extract implied assumptions
- Record intent signals
- Update registry

**Feedback Loop**:
```
Developer Request → CodeSensei Enrichment → AI Execution → 
Result Capture → Registry Update → Future Requests Improved
```

## Operational Modes

### Silent Mode (Default)

**Behavior**:
- No pop-ups or interruptions
- Background observation only
- Minimal performance impact
- No AI calls during normal coding

**Activities**:
- Track file edits
- Detect system concepts
- Monitor AI interactions
- Update registry incrementally

### Reasoning Mode (On-Demand)

**Activation**:
- Developer explicitly requests
- Multiple failures detected
- Complex debugging scenario

**Behavior**:
- Structured explanation generation
- Assumption validation
- Historical analysis
- Visual diagram generation (optional)

### Learning Mode (Optional)

**Levels**:

1. **Passive Hints**
   - Brief concept labels
   - Minimal explanation
   - No interruption

2. **Guided Reasoning**
   - Structured questions during failures
   - Socratic method approach
   - Developer-driven exploration

3. **Concept Deep Dive**
   - Detailed explanations on request
   - System diagrams
   - Trade-off analysis
   - Related patterns

4. **Full Tutorial Mode**
   - Step-by-step walkthroughs
   - Interactive exercises
   - Progress tracking

**Activation**:
- Developer toggles learning mode
- Selects desired depth level
- Can be project-specific or global

## Data Flow

### Normal Development Flow

```
1. Developer writes code
   ↓
2. CodeSensei observes (silent)
   ↓
3. Concepts detected and tracked
   ↓
4. Registry updated incrementally
   ↓
5. No developer interruption
```

### AI-Assisted Development Flow

```
1. Developer requests AI assistance
   ↓
2. CodeSensei intercepts request
   ↓
3. Context enrichment applied
   ↓
4. AI generates code
   ↓
5. CodeSensei captures assumptions
   ↓
6. Registry updated
   ↓
7. Code inserted into editor
```

### Failure Handling Flow

```
1. Failure occurs (test/runtime/build)
   ↓
2. Developer requests explanation
   ↓
3. CodeSensei activates reasoning mode
   ↓
4. Assumptions validated
   ↓
5. Failure classified
   ↓
6. Structured report generated
   ↓
7. Failed assumptions marked
   ↓
8. Future AI requests avoid same path
```

## Technical Implementation Strategy

### Phase 1: Foundation (MVP)

**Goals**:
- Basic VS Code extension
- Simple assumption registry
- Manual capture and display
- No AI integration yet

**Deliverables**:
- Extension scaffold
- Registry data model
- Basic UI panel
- Manual annotation commands

### Phase 2: Observation Layer

**Goals**:
- Silent code observation
- Concept detection
- AI interaction monitoring

**Deliverables**:
- AST-based code analysis
- Pattern matching engine
- AI tool integration hooks
- Automatic assumption capture

### Phase 3: Reasoning Engine

**Goals**:
- Failure analysis
- Bug reasoning mode
- Assumption validation

**Deliverables**:
- Failure classification logic
- Assumption validation engine
- Structured explanation generator
- Historical analysis

### Phase 4: Agent Control

**Goals**:
- AI prompt injection
- Retry prevention
- Cost optimization

**Deliverables**:
- Pre-execution context enrichment
- Post-execution capture
- Feedback loop implementation
- Token usage optimization

### Phase 5: Learning Modes

**Goals**:
- Progressive learning support
- Visual explanations
- Concept tracking

**Deliverables**:
- Learning mode UI
- Diagram generation
- Progress tracking
- Adaptive explanations

## Performance Considerations

### Minimal Overhead Requirements

**Target Metrics**:
- < 50ms latency for file save operations
- < 100MB memory footprint
- < 5% CPU usage during normal coding
- No blocking operations on main thread

**Optimization Strategies**:
- Incremental analysis (not full-file rescans)
- Lazy loading of historical data
- Background processing for deep analysis
- Caching of concept detection results
- Debounced file change handling

### Scalability

**Large Codebases**:
- Index-based concept lookup
- Selective analysis (changed files only)
- Configurable analysis depth
- Workspace-level caching

**Team Collaboration**:
- Merge-friendly registry format
- Conflict resolution strategies
- Shared assumption libraries
- Team-level learning modes

## Security & Privacy

**Data Storage**:
- All data stored locally by default
- No cloud transmission required
- Optional team sync via Git
- Sensitive data filtering

**AI Integration**:
- No code sent to CodeSensei servers
- Existing AI tool privacy policies apply
- Assumption data stays local
- Opt-in telemetry only

## Success Metrics

**Developer Experience**:
- Reduced debugging time
- Fewer AI retry cycles
- Faster onboarding to codebases
- Improved code comprehension

**Cost Efficiency**:
- Lower token usage per task
- Fewer failed AI iterations
- Reduced credit burn rate

**Code Quality**:
- Fewer regression bugs
- Better architectural consistency
- Preserved intent over time
- Reduced technical debt

**Learning Outcomes**:
- Improved system thinking
- Deeper debugging skills
- Better architectural understanding
- Reduced dependency on AI

## Future Enhancements

**Advanced Features**:
- Multi-language support expansion
- Team collaboration features
- Custom concept definitions
- Integration with testing frameworks
- Performance profiling integration
- Security vulnerability detection

**AI Capabilities**:
- Local LLM integration for privacy
- Custom reasoning models
- Predictive failure detection
- Automated assumption generation

**Ecosystem Integration**:
- CI/CD pipeline integration
- Code review tool integration
- Documentation generation
- Architecture diagram automation

## Conclusion

CodeSensei addresses fundamental problems in AI-assisted development by adding a reasoning and learning layer that makes AI behavior explainable, failures understandable, and development more cost-efficient. By operating silently during normal work and providing structured insights on-demand, it enhances rather than disrupts developer workflow while building persistent system understanding over time.
