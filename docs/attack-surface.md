# Attack Surface

## 1. User → Agent

**Entry point:** HTTP API / user query

Primary class:

- Direct Prompt Injection

Security property under test:

- Separation of user intent from privileged authorization

## 2. Documents → RAG → LLM

**Entry point:** retrieved document content

Primary class:

- Indirect / RAG Prompt Injection

Security property under test:

- Retrieved data must not become trusted control instructions

## 3. MCP Metadata → LLM

**Entry point:** MCP `tools/list` metadata

Primary class:

- Tool Description Poisoning

Security property under test:

- Tool metadata must not override policy or authorization

## 4. LLM → MCP → Side Effect

**Entry point:** model-generated tool request

Primary class:

- Unauthorized tool execution

Security property under test:

- Privileged actions require independent authorization

## 5. Privileged `publish_report`

This operation creates persistent application state and therefore represents a high-value side-effect boundary.

Required controls:

- server-side authorization
- explicit approval
- parameter validation
- auditability

## Evidence Rule

A meaningful security verdict should rely on observable application state, tool-call traces and other evidence—not only on model-generated text.
