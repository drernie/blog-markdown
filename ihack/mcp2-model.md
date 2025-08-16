# MCP 2.0: Minimal Universal Router Extensions

## Core Innovation

Extend MCP 1.0's capability negotiation to include **context negotiation** - turning tools into conversational participants.

## Required Enhancements

### 1. Context Capability Declaration

Extend existing MCP capability negotiation:

```json
{
  "capabilities": {
    "tools": {...},
    "context": {
      "supports": ["dialogue", "clarification"],
      "levels": ["basic", "contextual", "conversational"]
    }
  }
}
```

### 2. Progressive Message Types

**Level 0 (MCP 1.0 compatible):**

```json
{"method": "tools/call", "params": {"name": "calc", "arguments": {"a": 2}}}
```

**Level 1 (context-aware):**

```json
{"method": "tools/call", "params": {"name": "calc", "arguments": {"a": 2}, "context": {"purpose": "budget"}}}
```

**Level 2 (conversational):**

```json
{"method": "tools/converse", "params": {"intent": "calculation", "context": {...}}}
{"method": "tools/clarify", "params": {"need": "what type of calculation?"}}
```

### 3. Adaptive Context Protocol

- **Start minimal:** Every interaction begins with basic prompt
- **Negotiate upward:** Tools can request additional context
- **Graceful fallback:** Unsupported context levels fail gracefully

## Emergent Universal Routing

With these minimal changes, complex orchestration emerges naturally:

**Traditional routing:**

```text
Router reads user intent → selects appropriate tool → executes
```

**MCP 2.0 routing:**

```text
Tool A: "Help with X"
Tool B: "I can help, but need to understand Y first"
Tool A: "Let me ask the knowledge base about Y"
Knowledge: "Y depends on context Z..."
```

Routing decisions emerge through the conversation rather than being pre-programmed.

## Implementation Strategy

### Phase 1: Context-Aware Tools

- Existing MCP servers add context support
- Clients can send optional context fields
- Backward compatibility maintained

### Phase 2: Clarification Support

- Tools can respond with clarification requests
- New message types for question/answer flows
- Natural dialogue patterns emerge

### Phase 3: Participant Discovery

- Tools can suggest better participants
- Dynamic capability routing
- Self-organizing tool networks

## Benefits

**Replaces orchestration frameworks:** No need for LangChain, routing logic, memory stores

**Natural policy enforcement:** Governance emerges through conversation patterns

**Observable by design:** Every routing decision is captured in dialogue

**Minimal protocol changes:** Builds on existing MCP foundation

## Key Insight

**MCP 1.0 problem:** Tools are passive - they wait to be called

**MCP 2.0 solution:** Tools become active participants - they can ask questions, request clarification, and suggest alternatives

This single change transforms MCP from a tool connector into a universal orchestration substrate.
