# MCP 1.0: Asset Types Analysis

## Protocol Methods by Asset Type

### Tools - Executable Functions

**Methods:**

- `tools/list` - List available tools (paginated)
- `tools/call` - Execute tool with arguments

**Characteristics:**

- **Addressing:** By name (string identifier)
- **Parameters:** Arbitrary object passed as arguments
- **Side effects:** Can modify state, execute arbitrary code
- **Security:** Highest risk - requires explicit user consent for execution
- **Response:** Content, structured content, error handling

**Use case:** Calculator, file operations, API calls, database queries

### Resources - Data Sources

**Methods:**

- `resources/list` - List available resources (paginated)
- `resources/templates/list` - List resource templates with parameters
- `resources/read` - Read resource content

**Characteristics:**

- **Addressing:** By URI (structured resource identifier)
- **Parameters:** URI for access, template parameters for dynamic resources
- **Side effects:** Read-only, no state modification
- **Security:** Data privacy focused - consent for data exposure
- **Response:** Resource content (text, binary, structured data)

**Use case:** Files, databases, APIs, documentation, knowledge bases

### Prompts - Message Templates

**Methods:**

- `prompts/list` - List available prompts (paginated)  
- `prompts/get` - Get prompt with parameter substitution

**Characteristics:**

- **Addressing:** By name (string identifier)
- **Parameters:** Template variables for instantiation
- **Side effects:** Template instantiation only
- **Security:** Lowest risk - guides interaction patterns
- **Response:** Templated messages ready for LLM consumption

**Use case:** Conversation starters, workflows, structured queries

## Key Protocol Differences

### Addressing Schemes

- **Tools & Prompts:** Named entities (`name: string`)
- **Resources:** URI-based (`uri: string`) with optional template parameters

### Parameter Handling

- **Tools:** Arbitrary arguments object for function execution
- **Resources:** URI for direct access, parameters for template instantiation
- **Prompts:** Template variables for message generation

### Security Models

- **Tools:** Execution permission (highest risk)
- **Resources:** Data access permission (privacy risk)
- **Prompts:** Interaction guidance (lowest risk)

### Side Effects

- **Tools:** Stateful operations, can modify external systems
- **Resources:** Stateless data retrieval
- **Prompts:** Stateless template instantiation

## Why These Distinctions Matter

**Different interaction patterns:**

- Tools require function call semantics with arbitrary parameters
- Resources need URI-based addressing with content retrieval
- Prompts use template instantiation with variable substitution

**Different security implications:**

- Tools can execute arbitrary code and modify systems
- Resources expose potentially sensitive data
- Prompts primarily guide conversation flow

**Different caching and optimization strategies:**

- Tools results may not be cacheable (side effects)
- Resources can be cached and subscribed to for changes
- Prompts can be pre-compiled and cached

These aren't arbitrary categories - they reflect fundamentally different protocol behaviors, security models, and interaction patterns that applications need to handle differently.
