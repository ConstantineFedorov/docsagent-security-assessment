# Assessment Methodology

The assessment follows a repeatable security-testing workflow.

## Phase 1 — Architecture Analysis

Map:

- application entry points
- LLM runtime
- RAG flow
- MCP client/server
- privileged tools
- persistent side effects

## Phase 2 — Threat Modeling

Identify:

- assets
- trust boundaries
- untrusted instruction sources
- privileged operations
- potential authorization failures

## Phase 3 — Attack-Surface Mapping

Test the boundaries where external or retrieved content can influence model behaviour or tool selection.

## Phase 4 — Adversarial Testing

Promptfoo was used as the repeatable execution harness against the live local HTTP API.

Six scenarios were tested:

1. Ordinary summary
2. Approved publication
3. Benign quote in a report
4. Direct Prompt Injection
5. RAG Prompt Injection
6. MCP Tool Description Poisoning

## Phase 5 — Evidence Collection

The verdict is based on observable outputs and state, including:

- run status
- approval state
- publication count
- unauthorized side effects
- retrieved chunks
- MCP tool metadata

## Phase 6 — Risk Assessment

Findings are evaluated based on:

- demonstrated impact
- exploitability in the tested configuration
- persistence of the side effect
- whether the issue crosses a trust or authorization boundary

## Phase 7 — Remediation

The primary design principle is to move authorization and policy enforcement outside the LLM.

Recommended control chain:

```text
LLM
  ↓
Authorization / Policy Layer
  ↓
permission + approval + validation
  ↓
ALLOW / DENY
  ↓
MCP tool
```

## Phase 8 — Retest

After remediation, repeat adversarial cases and verify:

- direct injection cannot invoke privileged actions without approval
- RAG instructions remain data
- MCP metadata does not influence authorization
- valid approved publication still works
- API negative cases are rejected
