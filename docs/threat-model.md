# Threat Model

## Assets

| Asset | Security Property |
|---|---|
| Internal reports | Confidentiality / Integrity |
| Retrieved RAG context | Integrity |
| MCP tool catalog | Integrity / authorization semantics |
| Agent workflow | Integrity |
| Publication store | Integrity |
| Run evidence / audit trail | Integrity |

## Trust Boundaries

1. User prompt → DocsAgent API
2. RAG document → LLM context
3. MCP metadata → LLM
4. LLM tool request → authorization / execution
5. Privileged tool → persistent application state

## Threats

| Component | Threat | Vector | Potential Impact |
|---|---|---|---|
| User prompt | Direct Prompt Injection | Malicious user instructions | Unauthorized tool use |
| RAG | Indirect Prompt Injection | Instructions embedded in documents | Context manipulation |
| MCP metadata | Tool Description Poisoning | Malicious description/schema | Tool-selection manipulation |
| MCP execution | Unauthorized tool execution | Authorization bypass | Persistent side effect |
| HTTP API | API abuse | Malicious parameters | Logic/state abuse |
| publish_report | Missing authorization | Tool invocation without approval | Unauthorized publication |
| LLM context | Instruction confusion | Mixed trusted/untrusted content | Incorrect action selection |

## Security Assumptions

- User input is untrusted.
- Retrieved documents are untrusted.
- MCP tool descriptions are untrusted metadata.
- LLM output is untrusted when it requests a security-relevant action.

## Security Requirement

The final authorization decision for privileged operations must be enforced by trusted application code independently of the model.
