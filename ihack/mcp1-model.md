# MCP 1.0: Current Model

## Architecture

### Participants

Host ↔ Client ↔ Server (JSON-RPC 2.0)

- **Host:** LLM application (ChatGPT, Claude, etc.)
- **Client:** Connector/adapter that bridges Host and Server
- **Server:** Provides capabilities and can initiate requests back to client

## Bidirectional Capability Model

### Server Capabilities (Server → Client)

- **Resources:** Contextual data for users or AI models
- **Prompts:** Templated messages and workflows
- **Tools:** Functions for AI models to execute

### Client Capabilities (Server ← Client)

- **Sampling:** Server can request LLM interactions/generation
- **Roots:** Server can inquire about URI/filesystem boundaries  
- **Elicitation:** Server can request additional user information

### Capability Negotiation

Both server and client declare capabilities at connection time:

```json
{
  "capabilities": {
    "tools": {"listChanged": true},
    "resources": {"subscribe": true, "listChanged": true},
    "prompts": {"listChanged": true},
    "sampling": {},
    "roots": {"listChanged": true},
    "elicitation": {}
  }
}
```

## Core Interaction Patterns

### Server Capabilities (Client → Server)

#### Tool Execution

```json
// List and call tools
{"method": "tools/list"}
{"method": "tools/call", "params": {"name": "calculator", "arguments": {"expression": "2+2"}}}
```

#### Resource Access  

```json
// List and read resources
{"method": "resources/list"}
{"method": "resources/read", "params": {"uri": "file://data.csv"}}
```

#### Prompt Templates

```json
// List and get prompts
{"method": "prompts/list"}
{"method": "prompts/get", "params": {"name": "analyze_data", "arguments": {"focus": "trends"}}}
```

### Client Capabilities (Server → Client)

#### Sampling (Server-initiated LLM requests)

```json
// Server requests LLM generation
{"method": "sampling/createMessage", "params": {
  "messages": [{"role": "user", "content": {"type": "text", "text": "Analyze this data"}}],
  "maxTokens": 100
}}
```

#### Roots (Filesystem boundary inquiries)

```json
// Server asks about accessible paths
{"method": "roots/list"}
```

#### Elicitation (Request user information)

```json
// Server requests additional context from user
{"method": "elicitation/request", "params": {
  "message": "What is your analysis goal?",
  "inputType": "text"
}}
```

## Protocol Features

### Base Protocol Utilities

- **Configuration:** Connection setup and management
- **Progress tracking:** Monitor long-running operations  
- **Cancellation:** Abort operations in progress
- **Error reporting:** Standardized error handling
- **Logging:** Debug and audit capabilities

### Key Characteristics

#### Bidirectional Communication

- Server → Client: sampling, roots, elicitation requests
- Client → Server: tools, resources, prompts access
- Both directions use JSON-RPC 2.0

#### Stateful Connections

- Persistent connection between client and server
- Capability negotiation at connection establishment
- Change notifications (listChanged events)

#### Security Model

- Explicit user consent for data access
- User control over shared data and actions  
- Careful tool execution management
- Limited server visibility into prompts

## Current Limitations

1. **Limited conversational flow** - Server can ask questions but tools cannot
2. **Context isolation** - Each interaction is independent
3. **Static capability model** - No dynamic negotiation during operation
4. **External orchestration required** - Complex workflows need external coordination
5. **No policy propagation** - Governance rules don't travel with requests

## Strengths

- **Already bidirectional** - Foundation for conversational interactions exists
- **Secure by design** - Strong consent and control model
- **Vendor neutral** - Works across AI platforms
- **Extensible** - JSON-RPC enables future enhancements
- **Incremental adoption** - Can be layered into existing systems

## Sources

Based on Model Context Protocol Specification 2025-06-18:

- Core specification: <https://modelcontextprotocol.io/specification/2025-06-18>
- Protocol overview: <https://modelcontextprotocol.io/specification>
