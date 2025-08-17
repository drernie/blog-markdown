# MCP Terminology Evolution

## MCP 1.0: Current State

**Core entities:**

- **Host** - LLM application (ChatGPT, Claude, etc.)
- **Client** - JSON-RPC connector between host and server
- **Server** - Process that provides capabilities

**Asset types:**

- **Tool** - Executable function (tools/call, stateful, execution permissions)
- **Resource** - Data source (resources/read, stateless, URI-addressed)
- **Prompt** - Message template (prompts/get, template instantiation)

**Communication patterns:**

- **Request-Response** - tools/call, resources/read, prompts/get, sampling/createMessage
- **Subscribe-Notify** - resources/subscribe, tools/listChanged, prompts/listChanged
- **Bidirectional** - Servers can initiate sampling/roots/elicitation to Clients

**Key limitation:** All communication flows through Client intermediary - no direct Server-to-Server communication possible.

## MCP 2.0: Proposed Model

### Core Definitions

**Actor:**
External system (human, AI, tool, database, etc.) that has capabilities to provide and may need capabilities from others. Actors don't know anything about MCP protocol details.

**Adapter:**
Thin protocol layer that wraps an Actor to make it MCP-compatible. Handles message formatting and connection management but makes no decisions - all intelligence stays with the Actor.

**Asset:**
What an Actor can provide to other Actors. In MCP 1.0, Tools/Resources/Prompts have different protocol methods, addressing schemes, and security models. MCP 2.0 question: should these distinctions be preserved or unified?

*See: [Capability Document](mcp1-assets.md) for detailed analysis of MCP 1.0 asset type differences*

**Conversation:**
Symmetric peer-to-peer interaction between Actors via their Adapters. Any Actor can initiate requests, ask clarifying questions, or suggest alternatives.

### Architecture

```tree
Actor (Human) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (LLM)
Actor (Tool) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (Database)  
Actor (Agent) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (Model)
```

### What Changes

**From MCP 1.0 to MCP 2.0:**

- Client → Adapter (symmetric protocol layer)
- Server → Adapter (symmetric protocol layer)
- Host → Actor (external system)
- Tool/Resource/Prompt → Asset (what Actor provides)
- Model/Agent → Actor (new external systems)
- Sampling/Elicitation → Conversation (peer interaction)

### Key Benefits

**Enables symmetric communication:**

- Any Actor can request help from any other Actor
- Any Actor can discover and negotiate with other Actors
- Conversations maintain context across multi-step workflows
- Assets discovered dynamically through conversation
- Policy metadata flows naturally between Actors

## Validation Questions

Does this model address the core gaps?

1. **Adapter-Enabled Sampling** - Can any Actor request help from LLM Actors?
2. **Autonomous Discovery** - Can Actors find and negotiate with each other?
3. **Shared Context** - Do conversations maintain workflow state?
4. **Dynamic Assets** - Are assets discovered through conversation rather than static schemas?
5. **Policy Propagation** - Does governance metadata flow between Actors?
