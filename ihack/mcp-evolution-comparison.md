# MCP Evolution: 1.0 → 2.0 Comparison

## MCP 1.0: Current State

**Architecture:** Host ↔ Client ↔ Server (JSON-RPC 2.0)

**Core Capabilities:**
- **Resources:** Static contextual data
- **Tools:** Functions AI can execute  
- **Prompts:** Templated messages/workflows
- **Sampling:** LLM interactions (client feature)

**Protocol Pattern:**
```
Client: "List available tools"
Server: "I have: calculator, search, weather"
Client: "Execute calculator with params: {a: 2, b: 2}"
Server: "Result: 4"
```

**Key Characteristics:**
- Stateful connections
- Capability negotiation at connection time
- Request/response (no ongoing dialogue)
- Server provides capabilities, client orchestrates usage

## MCP 2.0 Proposals

### Envelope Approach (Original)
**Innovation:** Portable context containers with embedded policies
```
MetaContext {
  content: "Calculate budget",
  history: [...],
  policies: ["no-external-data"],
  trace: [...]
}
```

**vs MCP 1.0 Tension:** Requires protocol redesign - context becomes the primary abstraction rather than tools/resources

### Fractal Dialogue Approach  
**Innovation:** Context negotiation like ALPN
```
Level 0: Tool: "Calculate 2+2" → Server: "4"
Level 1: Tool: "Calculate 2+2 [context: budget]" → Server: "4 (currency?)"
Level 2: Tool: "Help with calculation [supports: dialogue]" 
         Server: "What are you calculating? [requesting: context]"
```

**vs MCP 1.0 Alignment:** Natural extension - tools become conversational participants

## Key Innovation: Context Negotiation

**Like ALPN for protocols, but for context sharing:**

1. **Backward compatibility:** All MCP 1.0 tools work (Level 0)
2. **Graceful upgrade:** Tools can advertise context capabilities
3. **Adaptive complexity:** Only share context that's useful
4. **Emergent coordination:** No separate orchestration framework needed

## Technical Implementation

**MCP 1.0 capability negotiation:**
```json
{
  "capabilities": {
    "tools": {...},
    "resources": {...},
    "prompts": {...}
  }
}
```

**MCP 2.0 context negotiation:**
```json
{
  "capabilities": {
    "tools": {...},
    "context": {
      "supports": ["dialogue", "clarification", "context-sharing"],
      "prefers": "incremental"
    }
  }
}
```

**Extended message patterns:**
```json
// Level 0 (backward compatible)
{"method": "tools/call", "params": {"name": "calc", "arguments": {"a": 2, "b": 2}}}

// Level 1 (context-aware)
{"method": "tools/call", "params": {"name": "calc", "arguments": {"a": 2, "b": 2}, "context": {"purpose": "budget"}}}

// Level 2 (dialogue)
{"method": "tools/converse", "params": {"intent": "calculation", "context": {"purpose": "budget"}, "supports": ["clarification"]}}
```

## Benefits

1. **Evolution not revolution:** MCP 1.0 investment preserved
2. **Natural progression:** Tools can gradually become more conversational
3. **Solves orchestration:** No need for separate LangChain/etc.
4. **Policy-aware:** Context negotiation can include governance rules
5. **Observable:** Dialogue creates natural audit trails

## Open Questions

1. How do we handle context negotiation failures gracefully?
2. What are the performance implications of adaptive dialogue?
3. How do policies propagate through multi-hop conversations?
4. Can we maintain MCP 1.0's security model with richer context sharing?