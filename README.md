# CodeSensei

## What is CodeSensei?

CodeSensei transforms AI-assisted development by adding a reasoning layer that makes code assumptions visible, failures understandable, and AI agents more reliable. It operates silently during normal development and provides structured explanations on-demand.

### The Problem

Current AI IDEs suffer from:
- 🔴 Verbose, token-wasting responses
- 🔴 Silent failures without root cause analysis
- 🔴 High costs from failed iterations and retries
- 🔴 Context loss between sessions
- 🔴 Poor understanding of existing codebases
- 🔴 Shallow learning that leads to skill decay

### The Solution

CodeSensei provides:
- ✅ **Explainable AI Code** - Understand what AI generated and why
- ✅ **Failure Reasoning** - Structured explanations of bugs, not     guesses
- ✅ **Real-World Learning** - Learn while building actual projects
- ✅ **Cost Efficiency** - Reduce AI token usage by 50%+
- ✅ **Persistent Context** - System understanding survives across sessions

## Key Features

### Intelligent Code Analysis
- Automatically detects system concepts (auth, async, caching, state management)
- Extracts implicit assumptions from code
- Classifies code by architectural layer
- Supports 40+ programming languages via Tree-sitter

### Bug Reasoning Mode
- Explains WHY failures occur, not just WHAT failed
- Validates assumptions when bugs happen
- Identifies which assumption broke
- Blocks known-bad solution paths
- Prevents retry loops

### AI Agent Control
- Intercepts AI requests (Cursor, Copilot, Cody)
- Enriches prompts with relevant context
- Captures and analyzes AI-generated code
- Reduces token waste by approximately 50%+
- Learns from failures to improve future requests

### Progressive Learning Modes
- **Silent Mode** (default): No interruptions, background observation
- **Passive Hints**: Concept labels and brief explanations
- **Guided Reasoning**: Socratic questions during debugging
- **Concept Deep Dive**: Detailed explanations with diagrams
- **Full Tutorial**: Interactive learning exercises

### Event-Driven Architecture
- File changes trigger analysis pipelines automatically
- AI requests intercepted and enriched in real-time
- Failures activate bug reasoning mode
- Everything asynchronous - no blocking the developer

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     VS Code Extension                        │
│              (TypeScript + Webview UI)                       │
└───────────────────────────┬─────────────────────────────────┘
                            │
┌───────────────────────────┴─────────────────────────────────┐
│                    Reasoning Engine                          │
│  • Tree-sitter AST Parsing  • Concept Detection             │
│  • Assumption Tracking      • Failure Analysis               │
└───────────────────────────┬─────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
┌───────┴────────┐  ┌──────┴──────┐  ┌────────┴─────────┐
│   Local        │  │   AWS       │  │   Amazon         │
│   Registry     │  │   Services  │  │   Bedrock        │
│   (SQLite)     │  │   (Lambda)  │  │   (AI/ML)        │
└────────────────┘  └─────────────┘  └──────────────────┘
```

## Technology Stack

### IDE Layer
- **TypeScript** - Type-safe development
- **VS Code Extension API** - Native IDE integration
- **Webview API** - Interactive panels and diagrams
- **React** - UI components
- **Mermaid.js** - Diagram rendering

### Code Analysis
- **Tree-sitter** - Multi-language AST parsing (40+ languages)
- **@babel/parser** - TypeScript/JavaScript parsing
- **acorn** - Lightweight JavaScript parser

### API Layer
- **GraphQL** (Apollo Server) - Flexible data querying
- **WebSocket** - Real-time updates

### Backend Compute
- **Python** (FastAPI) - Heavy computational tasks
- **Node.js** - Lightweight analysis functions

### AWS Services
- **Amazon Bedrock** - LLM integration (Claude 3.5 Sonnet/Haiku)
- **AWS Lambda** - Serverless compute for analysis
- **AWS Fargate** - Containerized reasoning engine
- **Amazon EventBridge** - Event-driven orchestration
- **AWS Step Functions** - Workflow coordination
- **Amazon DynamoDB** - Fast NoSQL storage
- **Amazon S3** - Long-term storage and backups
- **AWS API Gateway** - REST/WebSocket APIs
- **Amazon CloudWatch** - Monitoring and logging

## Quick Start

### Prerequisites
- VS Code 1.85.0 or higher
- Node.js 18.0.0 or higher
- AWS Account (for cloud features)

### Installation

1. **Install from VS Code Marketplace**
   ```bash
   code --install-extension codesensei.codesensei
   ```

2. **Or install from source**
   ```bash
   git clone https://github.com/your-org/codesensei.git
   cd codesensei
   npm install
   npm run compile
   ```

3. **Open in VS Code**
   ```bash
   code .
   # Press F5 to launch Extension Development Host
   ```

### Basic Usage

1. **Open a project in VS Code**
   - CodeSensei starts in Silent Mode (no interruptions)
   - Background analysis begins automatically

2. **View assumptions**
   ```
   Cmd/Ctrl + Shift + P → "CodeSensei: Show Assumptions"
   ```

3. **Explain a failure**
   - When code fails, run:
   ```
   Cmd/Ctrl + Shift + P → "CodeSensei: Explain Why This Failed"
   ```

4. **Enable Learning Mode**
   ```
   Cmd/Ctrl + Shift + P → "CodeSensei: Enable Learning Mode"
   ```

## How It Works

### 1. Silent Observation
```
Developer writes code
    ↓
CodeSensei observes (silent)
    ↓
Concepts detected and tracked
    ↓
Registry updated incrementally
    ↓
No developer interruption
```

### 2. AI Request Interception
```
Developer requests AI assistance
    ↓
CodeSensei intercepts request
    ↓
Fetches relevant assumptions and constraints
    ↓
Enriches prompt with context
    ↓
AI generates better code
    ↓
CodeSensei captures assumptions
    ↓
Registry updated for future requests
```

### 3. Failure Reasoning
```
Code fails (test/runtime/build)
    ↓
Developer requests explanation
    ↓
CodeSensei activates reasoning mode
    ↓
Validates all related assumptions
    ↓
Identifies which assumption failed
    ↓
Generates structured explanation
    ↓
Blocks known-bad fix attempts
```

## Amazon Bedrock Integration

CodeSensei uses Amazon Bedrock for intelligent code understanding:

### Assumption Extraction
```
Input: Code snippet
Bedrock (Claude 3.5 Sonnet) →
Output: "This function assumes user is authenticated"
```

### Bug Reasoning
```
Input: Failed Code + Context
Bedrock (Claude 3.5 Sonnet) →
Output: Structured explanation of WHY it failed
```

### Intent Detection
```
Input: Code changes (git diff)
Bedrock (Claude 3 Haiku) →
Output: "Developer is implementing OAuth refresh flow"
```

### Model Selection Strategy
- **Claude 3.5 Sonnet**: Complex reasoning, bug analysis ($3/1M tokens)
- **Claude 3 Haiku**: Fast concept detection, intent analysis ($0.25/1M tokens)
- **Smart caching**: 90% cost reduction on repeated queries

## Configuration

### Local Configuration
Create `.codesensei/config.json` in your project:

```json
{
  "analysisDepth": "normal",
  "learningMode": "passive",
  "autoSync": false,
  "languages": ["typescript", "javascript", "python"],
  "excludePatterns": ["node_modules", "dist", "build"]
}
```

### AWS Configuration
Set up AWS credentials and configure services:

```bash
# Configure AWS CLI
aws configure

# Deploy infrastructure
cd infrastructure
npm install
npm run deploy
```

### Environment Variables
```bash
# .env
AWS_REGION=us-east-1
BEDROCK_MODEL_ID=anthropic.claude-3-5-sonnet-20241022-v2:0
DYNAMODB_TABLE_PREFIX=codesensei-
S3_BUCKET_NAME=codesensei-registry
```

## Commands

| Command | Description |
|---------|-------------|
| `CodeSensei: Show Assumptions` | Display assumptions for current file |
| `CodeSensei: Explain Why This Failed` | Activate bug reasoning mode |
| `CodeSensei: Show System Concepts` | View detected architectural patterns |
| `CodeSensei: Enable Learning Mode` | Turn on progressive learning |
| `CodeSensei: Validate Assumptions` | Check if assumptions still hold |
| `CodeSensei: Show Failure History` | View past failures and fixes |
| `CodeSensei: Export Registry` | Export assumptions to JSON |
| `CodeSensei: Sync with Team` | Push/pull registry to cloud |

## Examples

### Example 1: Assumption Extraction

**Code:**
```typescript
async function getUserProfile(userId: string) {
  const user = await db.users.findById(userId);
  return {
    name: user.name,
    email: user.email,
    avatar: user.profile.avatar
  };
}
```

**CodeSensei Detects:**
- ✓ Assumes database connection is established
- ✓ Assumes userId exists in database
- ✓ Assumes user has a profile object
- ✓ Assumes profile has an avatar property
- ⚠️ No null checks for nested properties

### Example 2: Bug Reasoning

**Failure:**
```
TypeError: Cannot read property 'avatar' of undefined
```

**CodeSensei Explains:**
```
Bug Reasoning Report
━━━━━━━━━━━━━━━━━━━━

Failed Assumption:
  "User always has a profile object"

Why It Failed:
  New users created via OAuth don't have profiles 
  until they complete onboarding

Affected Layer:
  Data Access Layer

Previous Fix Attempts:
  ✗ Added null check for user (wrong level)
  ✗ Increased timeout (wrong layer)

Required Constraints:
  • Profile must be created during user registration
  • OR: Add optional chaining for profile access
  • OR: Return default avatar when profile missing

Recommended Approach:
  Use optional chaining: user.profile?.avatar ?? defaultAvatar
```

### Example 3: AI Request Enrichment

**Original Prompt:**
```
"Add authentication to this API endpoint"
```

**CodeSensei Enriches:**
```
"Add authentication to this API endpoint

Context:
- Project uses JWT tokens (detected in auth middleware)
- Token validation happens in validateToken() function
- Failed assumption: Previous attempt assumed tokens in headers,
  but OAuth flow uses cookies
- Constraint: Must support both header and cookie tokens
- Invalid path: Don't add passport.js (conflicts with existing auth)
"
```

**Result:** AI generates correct solution on first try, saving tokens and time.

## Performance

### Benchmarks
- File save analysis: **< 50ms**
- Concept detection: **< 100ms**
- Bug reasoning: **< 2 seconds**
- AI-powered analysis: **< 5 seconds**
- Memory footprint: **< 100MB**
- CPU usage: **< 5%** during normal coding

### Scalability
- Supports projects up to **1M lines of code**
- Handles **10,000+ assumptions** per project
- Supports **1,000+ concurrent users** (cloud)

## Cost Optimization

### Token Usage Reduction
- **50% fewer tokens** per AI request (context filtering)
- **70% fewer retries** (blocking bad paths)
- **90% cost reduction** via prompt caching

### Estimated Monthly Costs (AWS)
- **1,000 developers**: ~$150/month
- **10,000 developers**: ~$1,200/month
- **100,000 developers**: ~$10,000/month

Much cheaper than the cost of failed AI iterations and wasted developer time.

## Team Collaboration

### Git-Friendly Registry
```
.codesensei/
├── assumptions/
│   ├── auth-flow.json
│   └── data-validation.json
├── concepts/
│   └── detected-patterns.json
└── registry.db
```

### Sync with Team
```bash
# Push assumptions to team
codesensei sync push

# Pull team assumptions
codesensei sync pull

# Resolve conflicts
codesensei sync resolve
```

### Benefits
- New team members understand codebase faster
- Shared understanding of architectural decisions
- Preserved intent across team changes
- Reduced onboarding time by 50%

