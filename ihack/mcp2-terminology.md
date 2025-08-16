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
