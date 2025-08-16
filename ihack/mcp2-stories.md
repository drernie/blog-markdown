# MCP 2.0: The Universal Router Vision

## Master Story

**When building a modern AI system,**  
**I want to use MCP as the universal router across tools, models, and agents,**  
**So that my LLM doesn't need to worry about which is which.**

## The Current Problem

Today, LLMs must understand the difference between:

- **Tools** (functions to execute)
- **Models** (other LLMs to consult)
- **Agents** (autonomous systems to coordinate with)
- **Memory** (where to store/retrieve context)
- **Policies** (what governance rules apply)

This creates **cognitive overhead** and **brittle orchestration**.

## Technical Gap Analysis: Why MCP 1.0 Cannot Solve These Problems

### Gap 1: Server-Initiated Sampling

**Job Story:**  
When I am an MCP server processing a request that exceeds my capabilities,  
I want to initiate a sampling request to a more capable model,  
So that I can provide better results without requiring client orchestration.

**Current MCP 1.0 limitation:**

```json
// Only clients can initiate sampling - servers cannot
{"method": "sampling/createMessage", "params": {...}}
// ↑ This can only be sent Client → Server, never Server → Server
```

**Technical gap:** While MCP supports sampling, it is strictly Client → Server. Servers cannot directly request completions ("sampling") from other servers. All such calls must be routed back through the client, preventing direct Server → Server or cascading model invocations.

### Gap 2: Autonomous Agent Discovery

**Job Story:**  
When I am an MCP server that needs specialized capabilities I don't have,  
I want to discover and negotiate with other MCP servers directly,  
So that I can coordinate multi-agent workflows without human mediation.

**Current MCP 1.0 limitation:**

```json
// Elicitation only goes to humans, not other agents
{"method": "elicitation/request", "params": {"message": "Need user input"}}
// ↑ No mechanism for server-to-server discovery or negotiation
```

**Technical gap:** No protocol for server discovery, capability negotiation, or autonomous handoffs between servers.

### Gap 3: Shared Workflow Context

**Job Story:**  
When I am an MCP tool executing as part of a multi-step workflow,  
I want to access context from previous tools and share my results with subsequent tools,  
So that I can make informed decisions and contribute to coordinated problem-solving.

**Current MCP 1.0 limitation:**

```json
// Each tool call is completely isolated
{"method": "tools/call", "params": {"name": "calc", "arguments": {"expr": "2+2"}}}
// ↑ No access to workflow context, previous results, or shared state
```

**Technical gap:** No mechanism for persistent workflow context or inter-tool coordination within a session.

### Gap 4: Runtime Capability Negotiation

**Job Story:**  
When I am an MCP participant encountering a task that requires capabilities I didn't declare at connection time,  
I want to dynamically negotiate new capabilities or discover better-suited participants,  
So that I can adapt to actual requirements rather than being limited by static declarations.

**Current MCP 1.0 limitation:**

```json
// Capabilities fixed at connection establishment
{"capabilities": {"tools": {...}, "resources": {...}}}
// ↑ No runtime negotiation, adaptation, or dynamic discovery
```

**Technical gap:** No protocol for dynamic capability evolution, runtime negotiation, or task-based participant selection.

### Gap 5: Policy-Aware Context Propagation

**Job Story:**  
When I am an MCP participant processing a request that contains sensitive data or requires governance compliance,  
I want to receive and enforce policy metadata that travels with the request,  
So that I can automatically comply with data protection, auditing, and enterprise governance requirements.

**Current MCP 1.0 limitation:**

```json
// No fields for policies, audit trails, or governance metadata
{"method": "tools/call", "params": {"name": "search", "arguments": {"query": "data"}}}
// ↑ No way to attach compliance rules, data classifications, or audit requirements
```

**Technical gap:** No protocol support for policy metadata, governance enforcement, or audit trail propagation.
