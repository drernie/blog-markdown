# MCP Terminology Evolution

## 1. MCP 1.0 Terminology

### Core Entities

- **Host** - LLM application (ChatGPT, Claude, etc.)
- **Client** - JSON-RPC connector between host and server
- **Server** - Process that provides capabilities

### Capability Types

- **Tool** - Executable function with schema
- **Resource** - Data source (file, URI, database)
- **Prompt** - Message template with parameters

### Communication Patterns

- **Sampling** - Client capability for LLM generation requests
- **Roots** - Client capability for filesystem boundary queries
- **Elicitation** - Client capability for user input requests

### Current Architecture

```tree
Host (LLM app)
├── MCP Client (protocol wrapper)
└── app logic

MCP Server (protocol wrapper)  
├── Tools
├── Resources  
└── Prompts
```

## 2. Limitations of MCP 1.0 Terminology

### Asymmetric Communication

**Problem:** Client and Server are asymmetric roles:

- Only Clients can initiate tool requests
- Only Servers can provide tools/resources/prompts
- No Server-to-Server communication possible

**Impact:** This prevents peer collaboration between:

- Models (other LLMs) that need to both provide and consume capabilities
- Agents that autonomously initiate conversations
- Tools that delegate to other tools

### Artificial Capability Boundaries

**Problem:** Static distinctions between Tool/Resource/Prompt create artificial categories:

- Forces premature abstraction of what capabilities can do
- Prevents natural evolution of capabilities
- Makes it harder to add new capability types (Models, Agents, Memory)

### Static Protocol Operations

**Problem:** Fixed operations (tools/call, resources/read, sampling/createMessage):

- Each interaction is isolated and purpose-specific
- No way to ask clarifying questions or negotiate requirements
- No shared context across related operations

## 3. MCP 2.0 Proposal

### Core Concepts

#### Actors

**External systems** (human, AI, tool, database, etc.) that:

- Have some capability they can provide to others
- May need capabilities from other actors
- Don't know anything about MCP protocol details
- Examples: LLM applications, autonomous agents, calculation tools, databases, humans

#### Capabilities

**What an Actor can provide** to other Actors:

- Unified term replacing Tool/Resource/Prompt/Model distinctions
- Discovered dynamically through conversation, not declared statically
- Can evolve and adapt based on context

#### Adapters

**Thin protocol layers** that:

- Wrap Actors to make them MCP-compatible
- Handle MCP protocol mechanics (message formatting, connection management)
- Translate between Actor's native interface and MCP conversations
- Enable symmetric communication - any Actor can talk to any other Actor

### Proposed Architecture

```tree
Actor (Human) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (LLM)
Actor (Tool) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (Database)  
Actor (Agent) ←→ Adapter ←→ MCP Protocol ←→ Adapter ←→ Actor (Model)
```

### Communication Model

**Symmetric Conversations:** All interactions become peer-to-peer conversations:

- Any Actor can initiate requests to any other Actor
- Any Actor can ask clarifying questions
- Any Actor can suggest alternative approaches
- Context flows naturally through conversation threads

**Dynamic Capability Discovery:** Instead of static schemas:

```text
Actor A: "I need help calculating mortgage payments"
Actor B: "I can help with calculations. What data do you have?"
Actor A: "I have income but need current rates"  
Actor B: "Let me connect you with a financial data source"
```

### Terminology Mapping

| MCP 1.0 Term | MCP 2.0 Evolution | Role |
|--------------|------------------|------|
| Client | Adapter | Symmetric protocol adapter |
| Server | Adapter | Symmetric protocol adapter |
| Host | Actor | External system wrapped by Adapter |
| Tool/Resource/Prompt | Capability | What an Actor can provide |
| Model/Agent | Actor | New external systems, same treatment |
| Sampling/Elicitation | Conversation | Peer-to-peer interaction pattern |

## 4. Open Questions

### Does This Terminology Address All User Stories?

1. **Adapter-Enabled Sampling**: Can the Actor/Adapter model enable any Actor to request help from LLM Actors without asymmetric Client/Server constraints?

2. **Autonomous Agent Discovery**: Can Actors discover and negotiate with other Actors through their Adapters without human mediation?

3. **Shared Workflow Context**: Can conversations between Actors maintain persistent context across multi-step collaborative workflows?

4. **Runtime Capability Negotiation**: Can Actors dynamically discover what other Actors can provide through natural conversation rather than static declarations?

5. **Policy-Aware Context Propagation**: Can governance metadata and audit requirements flow naturally through Actor conversations via their Adapters?
