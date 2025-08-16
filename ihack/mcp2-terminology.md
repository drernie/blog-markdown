# MCP Terminology Analysis

## Understanding MCP 1.0 Current Reality

Before proposing changes, we need to precisely understand what each term means in MCP 1.0 today.

### Complete MCP 1.0 Vocabulary

**Core Entities:**

- **Host** - LLM application (ChatGPT, Claude, etc.)
- **Client** - JSON-RPC connector between host and server
- **Server** - Process that provides capabilities

**Capability Types:**

- **Tool** - Executable function with schema
- **Resource** - Data source (file, URI, database)
- **Prompt** - Message template with parameters

**Protocol Artifacts:**

- **Capabilities** - Feature set negotiated at connection
- **Message** - JSON-RPC request/response structure
- **Method** - Specific operation (tools/call, resources/read, etc.)
- **Parameters** - Arguments passed with method calls
- **Result** - Response data from method execution

**Communication Flows:**

- **Sampling** - Client capability for LLM generation requests
- **Roots** - Client capability for filesystem boundary queries
- **Elicitation** - Client capability for user input requests

**Session Management:**

- **Connection** - Persistent stateful link between client and server
- **Subscription** - Notification mechanism for capability changes
- **Initialization** - Capability negotiation handshake

### MCP 1.0 Communication Patterns

**Current flow patterns:**

- Host → Client → Server (for tools/resources/prompts)
- Server → Client (for sampling/roots/elicitation)
- Client → Host (for user interaction)

**What's NOT possible in MCP 1.0:**

- Server → Server direct communication
- Tool → Tool coordination  
- Server-initiated sampling to other servers
- Cross-server context sharing
- Dynamic server discovery

### Terminology Constraints

The current terms reflect MCP 1.0's actual capabilities and limitations:

**"Server"** accurately describes the current reality:

- Provides services when requested
- Waits for client requests
- Cannot initiate peer connections

**"Tool"** accurately describes current behavior:

- Executes specific functions
- Has no context awareness
- Cannot ask clarifying questions

**"Client"** accurately describes the mediation role:

- All communication flows through it
- Acts as coordinator between host and servers
- Single point of control

## Terminology Evolution Strategy

Looking at the gap analysis, the core issue is that MCP 1.0's Client/Server asymmetry prevents the peer-to-peer conversations needed for Models, Agents, and autonomous capabilities.

### The Asymmetry Problem

**Current MCP 1.0 architecture:**

```tree
Host (LLM app)
├── MCP Client (protocol wrapper)
└── app logic

MCP Server (protocol wrapper)  
├── Tools
├── Resources  
└── Prompts
```

**Problem:** Client and Server are asymmetric - only Clients can initiate sampling, only Servers provide capabilities. This breaks down when we need:

- Models (other LLMs) that both provide and consume capabilities
- Agents that autonomously initiate conversations
- Tools that delegate to other tools

### Proposed Solution: Symmetric Shims

**MCP 2.0 architecture with peer Shims:**

```tree
Host (LLM app)
├── MCP Shim (symmetric protocol wrapper)
└── app logic

MCP Shim (symmetric protocol wrapper)
├── Model capabilities
├── Agent capabilities  
└── Tool capabilities

MCP Shim (symmetric protocol wrapper)
├── Different capabilities
└── Different behaviors
```

### Shim Capabilities Model

Each **MCP Shim** can both:

- **Provide capabilities** (tools, resources, prompts, models, agents)
- **Consume capabilities** (sampling, elicitation, discovery, delegation)

**Symmetric communication patterns:**

```tree
Shim ↔ Shim (peer conversation)
Shim ↔ Shim (capability delegation)  
Shim ↔ Shim (collaborative problem-solving)
```

### Terminology Evolution

| MCP 1.0 Term | MCP 2.0 Evolution | Rationale |
|--------------|------------------|-----------|
| Client | Shim | Symmetric protocol wrapper, can initiate and respond |
| Server | Shim | Symmetric protocol wrapper, can initiate and respond |
| Host | Host *(unchanged)* | Still the LLM application, but now uses symmetric Shim |
| Tool/Resource/Prompt | Capability | Unified term for anything a Shim can provide |
| Sampling/Elicitation | Conversation | Generalized peer-to-peer interaction |

### Key Benefits

**Eliminates artificial constraints:**

- Any Shim can initiate sampling (Gap 1: Server-Initiated Sampling)
- Any Shim can discover other Shims (Gap 2: Autonomous Agent Discovery)  
- Shims can maintain shared context (Gap 3: Shared Workflow Context)
- Shims can negotiate capabilities dynamically (Gap 4: Runtime Capability Negotiation)
- Policy metadata flows between peer Shims (Gap 5: Policy-Aware Context Propagation)

**Enables natural scaling:**

- Add Models as peer Shims that can both consume and provide LLM capabilities
- Add Agents as peer Shims that autonomously initiate conversations
- Add Memory systems as peer Shims that maintain persistent context
- All communicate through the same symmetric protocol
