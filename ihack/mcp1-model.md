# MCP 1.0: Current Model

## Architecture

### Participants

Host ↔ Client ↔ Server (JSON-RPC 2.0)

- **Host:** LLM application (ChatGPT, Claude, etc.)
- **Client:** Connector/adapter that bridges Host and Server
- **Server:** Provides capabilities (tools, resources, prompts)

## Capability Declaration (Static)

Server announces what it can do at connection time:

*(Example schema for illustration; actual protocol definitions contain more detail.)*

```json
{
  "capabilities": {
    "tools": {
      "listChanged": true
    },
    "resources": {
      "subscribe": true,
      "listChanged": true
    },
    "prompts": {
      "listChanged": true
    }
  }
}
```

## Core Interaction Patterns

### 1. Tool Execution

```json
// Client requests tool list
{"method": "tools/list"}

// Server responds with available tools
{
  "tools": [
    {
      "name": "calculator", 
      "description": "Perform arithmetic calculations",
      "inputSchema": {"type": "object", "properties": {"expression": {"type": "string"}}}
    }
  ]
}

// Client calls tool
{"method": "tools/call", "params": {"name": "calculator", "arguments": {"expression": "2+2"}}}

// Server executes and responds
{"result": {"content": [{"type": "text", "text": "4"}]}}
```

### 2. Resource Access

```json
// Client lists resources
{"method": "resources/list"}

// Server provides resource list
{
  "resources": [
    {"uri": "file://data.csv", "name": "Sales Data", "mimeType": "text/csv"}
  ]
}

// Client reads resource
{"method": "resources/read", "params": {"uri": "file://data.csv"}}

// Server returns content
{"contents": [{"uri": "file://data.csv", "mimeType": "text/csv", "text": "name,sales\nAlice,100\n..."}]}
```

### 3. Prompt Templates

```json
// Client lists prompts
{"method": "prompts/list"}

// Server provides prompt templates
{
  "prompts": [
    {
      "name": "analyze_data",
      "description": "Analyze dataset with specific focus",
      "arguments": [{"name": "focus", "description": "Analysis focus", "required": true}]
    }
  ]
}

// Client gets prompt
{"method": "prompts/get", "params": {"name": "analyze_data", "arguments": {"focus": "trends"}}}

// Server returns filled template
{"messages": [{"role": "user", "content": {"type": "text", "text": "Analyze this dataset focusing on trends..."}}]}
```

## Key Characteristics

### Stateful Connections

- Persistent connection between client and server
- Capability negotiation happens once at start
- Tools/resources can change, triggering notifications

### Request/Response Model

- Client initiates all interactions
- Server responds to requests
- No server-initiated conversations

### Passive Tools

- Tools wait to be called with specific parameters
- No clarification or context requests
- Execute function, return result

### Orchestration Outside Protocol

- MCP handles tool connectivity
- LangChain/etc. handles when/why to call tools
- Memory management separate from protocol
- Routing logic external to MCP

## Limitations

1. **No conversational capability** - Tools can't ask questions
2. **Context isolation** - Each tool call is independent
3. **Static capabilities** - No dynamic discovery or negotiation
4. **External orchestration required** - MCP is just the "pipe"
5. **No policy awareness** - Governance handled separately

## What Works Well

- **Simple and focused** - Clear separation of concerns
- **Incrementally adoptable** - Designed for gradual rollout across platforms  
- **Vendor neutral** - Works across different AI platforms
- **Secure** - Explicit user control and consent model
- **Extensible** - JSON-RPC allows for future enhancements
