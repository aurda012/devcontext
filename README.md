[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/aurda012-devcontext-badge.png)](https://mseep.ai/app/aurda012-devcontext)

# DevContext: Autonomous Context Awareness Model-Context-Protocol (MCP) Server

<p align="center">
  <img src="https://i.postimg.cc/sghKLKf6/Dev-Context-banner.png" alt="DevContext Banner" width="100%" />
</p>

> **Empower your development workflow with intelligent context awareness** - DevContext understands your codebase, conversations, and development patterns to provide relevant context exactly when you need it.

## Introduction

DevContext is a cutting-edge Model Context Protocol (MCP) server designed to provide developers with continuous, project-centric context awareness. Unlike traditional context systems, DevContext continuously learns from and adapts to your development patterns. DevContext leverages sophisticated retrieval methods, focusing on keyword analysis, relationship graphs, and structured metadata to deliver highly relevant context during development, understanding both your conversations and your codebase at a deeper level.

The server operates with a database instance dedicated to a single project, eliminating cross-project complexity and ensuring performance with minimal resource requirements. DevContext builds a comprehensive understanding of your codebase - from repository structure down to individual functions - while continuously learning from and adapting to your development patterns.

**The BEST way** to use this MCP server is to follow the guide below on implementing the provided Cursor Rules system which in turn gives you:

- **Completely autonomous context management**
- Autonomous **external documentation context and use**
- Complete **task management** workflow integration

### Core Technologies

- **Node.js**: Runtime environment (Node.js 18+)
- **TursoDB**: SQL database optimized for edge deployment (compatible with SQLite)
- **Model Context Protocol SDK**: For standardized communication with IDE clients
- **Cursor Rules**: Autonomous development environment and workflow management
- **JavaScript/TypeScript**: Pure JavaScript implementation with no external ML dependencies

## Installation Guide

### Prerequisites

- Node.js 18.0.0 or higher
- Cursor IDE with MCP support
- TursoDB account (for database)

### Step 1: Set up TursoDB Database

1. **Sign up for TursoDB**:

   - Visit [Turso](https://turso.tech/) and create an account
   - The free tier is sufficient for most projects

2. **Install Turso CLI** (optional but recommended):

   ```bash
   curl -sSfL https://get.turso.tech/install.sh | bash
   ```

3. **Authenticate with Turso**:

   ```bash
   turso auth login
   ```

4. **Create a project database**:

   ```bash
   turso db create devcontext
   ```

5. **Get database credentials**:

   ```bash
   # Get database URL
   turso db show devcontext --url

   # Create auth token
   turso db tokens create devcontext
   ```

   Save both the URL and token for the next step.

### Step 2: Configure MCP in Cursor (can be applied to other IDE's as well)

Create or edit `.cursor/mcp.json` in your project directory:

```json
{
  "mcpServers": {
    "devcontext": {
      "command": "npx",
      "args": ["-y", "devcontext@latest"],
      "enabled": true,
      "env": {
        "TURSO_DATABASE_URL": "your-turso-database-url",
        "TURSO_AUTH_TOKEN": "your-turso-auth-token"
      }
    }
  }
}
```

Replace `your-turso-database-url` and `your-turso-auth-token` with the values obtained in Step 1.

## Cursor Rules Implementation

DevContext implements a sophisticated set of Cursor Rules that create an autonomous development environment. These rules guide Cursor's AI assistants in maintaining project scope alignment, incorporating up-to-date documentation, and implementing advanced task workflows.

**Be on the lookout** for the DevContext Project Generator which is coming very soon and will create a COMPLETE set up for your project to literally 10x your development workflow.

### Key Rule Components

#### 1. DevContext MCP Tools Usage Guide

The core rule defines a precise sequence for tool execution:

```
1. FIRST: Call initialize_conversation_context EXACTLY ONCE at START
2. AS NEEDED: Call update_conversation_context for code changes/new messages
3. AS NEEDED: Call retrieve_relevant_context when specific context is required
4. OCCASIONALLY: Call record_milestone_context for significant achievements
5. LAST: Call finalize_conversation_context EXACTLY ONCE at END
```

This workflow ensures comprehensive context management throughout the entire development session.

#### 2. External Library Documentation Requirements

All external library usage must be preceded by proper documentation retrieval:

- **Two-Step Documentation Retrieval** using Context7
- **Web Search Fallback** for documentation not available through Context7
- **Multi-Source Documentation Synthesis** for comprehensive understanding

This prevents common issues with incorrect API usage, incompatible versions, or missing dependencies.

#### 3. Task Workflow System

The task workflow system enables:

- Structured task management in `tasks.md`
- Task ID-based implementation order
- Status tracking with completion metadata
- Project blueprint integration for architectural context

### Setting Up Cursor Rules

1. **Create Rules Directory**:

   ```bash
   mkdir -p .cursor/rules
   ```

2. **Download/Copy and Paste Rules**:

Download or copy and paste the .cursor/rules directory into your project. Next, copy and paste the contents of the `.cursorrules` file in your project root and paste it into your cursor settings rules (Cursor Settings -> Rules -> User Rules). You should also copy and paste the `.cursorrules` file into your main directory as well.

3. **Customize Task Workflow** (Optional):
   Once the cursor rules are implemented, restart Cursor and proceed to ask it to create tasks for you based on whatever project idea you may have.

## Configuration Example

Below is a complete example of an `mcp.json` file that configures both DevContext and Context7 MCP servers:

```json
{
  "mcpServers": {
    "devcontext": {
      "command": "npx",
      "args": ["-y", "devcontext@latest"],
      "enabled": true,
      "env": {
        "TURSO_DATABASE_URL": "libsql://your-project-db-name.turso.io",
        "TURSO_AUTH_TOKEN": "your_turso_auth_token_here"
      }
    },
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

### Important Parameters

| Parameter            | Description                      | Default Value   |
| -------------------- | -------------------------------- | --------------- |
| `TURSO_DATABASE_URL` | URL of your TursoDB instance     | None (Required) |
| `TURSO_AUTH_TOKEN`   | Authentication token for TursoDB | None (Required) |

## Table of Contents

- [System Overview](#system-overview)
- [Core Components](#core-components)
  - [Text Processing](#text-processing)
  - [Context Management](#context-management)
  - [Pattern Recognition](#pattern-recognition)
  - [Intent & Relevance Analysis](#intent--relevance-analysis)
- [MCP Tools](#mcp-tools)
  - [initialize_conversation_context](#initialize_conversation_context)
  - [update_conversation_context](#update_conversation_context)
  - [retrieve_relevant_context](#retrieve_relevant_context)
  - [record_milestone_context](#record_milestone_context)
  - [finalize_conversation_context](#finalize_conversation_context)
- [Data Architecture](#data-architecture)
- [Technical Specifications](#technical-specifications)
- [Performance Considerations](#performance-considerations)
- [Security](#security)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## System Overview

DevContext is a state-of-the-art context management system for software development, implementing the Model Context Protocol (MCP) with advanced context retrieval techniques.

The system operates as a Node.js application with a modular JavaScript codebase, bundled into a single `.js` file using `esbuild`. It leverages TursoDB (or similar SQL database) as the persistent store for all context, metadata, and optional logs for a specific project.

**Key Differentiators:**

- **Non-Vector Retrieval**: Context retrieval uses sophisticated keyword analysis, relationship graphs, and structured metadata instead of vector embeddings
- **Project-Centric Design**: Each server instance is dedicated to a single project, simplifying data management
- **Minimal Dependencies**: Restricted to core essentials - MCP SDK, TursoDB client, and lightweight AST parsing
- **Hierarchical Understanding**: Context is understood from repository structure down to function/variable levels
- **Intelligent Context Prioritization**: Multi-factor relevance scoring based on recency, importance, relationships, and developer focus

## Core Components

### Text Processing

- **Language-aware tokenization** with specialized handling for JavaScript/TypeScript, Python, Java, C#, Ruby, and Go
- **Keyword extraction** with language-specific weighting
- **Semantic boundary respecting n-grams**
- **Language-specific idiom detection**

### Context Management

- **Code entity indexing** and relationship tracking
- **Conversation topic segmentation** and purpose detection
- **Timeline event recording** and milestone snapshots
- **Focus area prediction** based on developer activity

### Pattern Recognition

- **Code pattern identification** and storage
- **Automatic pattern learning** from examples
- **Cross-session pattern promotion**
- **Design pattern detection**

### Intent & Relevance Analysis

- **Query intent prediction**
- **Multi-factor context prioritization**
- **Token budget management**
- **Context integration across topic shifts**

## MCP Tools

DevContext implements the following MCP tools that can be invoked by Cursor IDE:

### initialize_conversation_context

Initializes a new conversation session with comprehensive project context.

**When to use**: At the beginning of every conversation, exactly once.

**Key parameters**:

- `initialQuery`: The user's first message or question
- `contextDepth`: Minimal, standard, or comprehensive context depth
- `includeArchitecture`: Whether to include architectural context
- `focusHint`: Optional focus on specific code entity

**Returns**: Conversation ID and initial context summary

### update_conversation_context

Updates the active context with new messages and code changes.

**When to use**: After code changes or new messages are exchanged.

**Key parameters**:

- `conversationId`: ID from initialize_conversation_context
- `newMessages`: New messages exchanged since last update
- `codeChanges`: Code files created or modified
- `preserveContextOnTopicShift`: Whether to maintain context during topic changes

**Returns**: Updated focus and context continuity information

### retrieve_relevant_context

Retrieves context snippets relevant to a specific query.

**When to use**: When specific project context is needed.

**Key parameters**:

- `conversationId`: ID from initialize_conversation_context
- `query`: Specific question about the project
- `constraints`: Optional filters for entity types, file paths, etc.
- `weightingStrategy`: How to prioritize results

**Returns**: Relevant context snippets with explanations

### record_milestone_context

Records significant development milestones for future reference.

**When to use**: After completing important features, fixing critical bugs, or making architectural decisions.

**Key parameters**:

- `conversationId`: ID from initialize_conversation_context
- `name`: Short, descriptive milestone name
- `description`: Detailed explanation
- `milestoneCategory`: Category (feature, bug fix, refactoring, etc.)
- `assessImpact`: Whether to analyze impact

**Returns**: Milestone ID and impact assessment

### finalize_conversation_context

Concludes a conversation, extracting learnings and suggesting next steps.

**When to use**: At the end of every conversation, exactly once.

**Key parameters**:

- `conversationId`: ID from initialize_conversation_context
- `extractLearnings`: Whether to identify and extract learnings
- `promotePatterns`: Whether to promote patterns to global repository
- `generateNextSteps`: Whether to suggest follow-up actions

**Returns**: Conversation summary, extracted learnings, and next steps

## Data Architecture

DevContext uses a SQL database (TursoDB) with the following core tables:

- **code_entities**: Stores indexed code from files, functions, classes, etc.
- **entity_keywords**: Maps keywords to code entities for search
- **code_relationships**: Tracks relationships between code entities
- **conversation_history**: Stores conversation messages
- **conversation_topics**: Segments conversations into coherent topics
- **timeline_events**: Records significant development events
- **project_patterns**: Stores identified code patterns
- **focus_areas**: Tracks developer attention and intention

The database schema is automatically created and maintained by the server.

## Technical Specifications

- **Node.js**: Version 18.0.0 or higher required
- **Database**: TursoDB (or compatible SQLite)
- **Bundling**: ESBuild for single-file deployment
- **Protocol**: Model Context Protocol via @modelcontextprotocol/sdk
- **Parsing**: Lightweight JavaScript AST parsing (acorn)
- **Operating Systems**: Cross-platform (Windows, macOS, Linux)

## Performance Considerations

DevContext is optimized for performance with:

- **Efficient SQL queries** with proper indexing
- **In-memory caching** for frequently accessed data
- **Incremental updates** to minimize processing
- **Asynchronous operations** for non-blocking execution
- **Adaptive context retrieval** based on token budget
- **Scheduled background tasks** during idle periods

For large codebases (>100,000 LOC), initial indexing may take several minutes, but subsequent operations remain fast and responsive.

## Security

- **Isolated Database**: Each project uses a dedicated database instance
- **Secure Credentials**: TursoDB credentials managed via environment variables
- **Input Validation**: All inputs validated with Zod schemas
- **Parameterized Queries**: SQL injection protection
- **No External APIs**: All processing happens locally

## Troubleshooting

Common issues and solutions:

- **Connection Errors**: Verify TursoDB credentials and database URL
- **Slow Initial Startup**: Normal for large codebases; subsequent startups are faster
- **Missing Context**: Check token budget; increase if necessary
- **Tool Errors**: Ensure proper conversation ID is being passed between tools
- **Performance Issues**: Consider reducing scope of indexed files or increasing cache size

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

_DevContext: Continuous Context for Continuous Progress_
