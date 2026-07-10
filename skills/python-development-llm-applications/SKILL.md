---
name: python-llm-applications
description: Contains opinionated general instructions and guidelines on how to develop LLM applications using Python.
---

# Python development for LLM applications skill

Always read `python-general` skill before developing Python code.

## Instructions

- When developing LLM applications, use `langchain>=2.0` for application development. Structure chains with clear prompt templates, output parsers, and retrievers. Keep prompts readable and organised in a dedicated location.
- Prioritise `ollama` for local models unless otherwise noted. If `ollama` is not running, wait for user confirmation stating clearly that another provider should be used.
- Never hardcode API keys or credentials. Load them via environment variables (e.g., `OPENAI_API_KEY`, `ANTHROPIC_API_KEY`) and validate them at startup.
- Prefer local models for sensitive data; use external APIs only after confirming compliance/approval.
- Trace and log LLM calls. Use LangSmith or an equivalent tracing tool to capture inputs, outputs, token usage, and latency.
- Evaluate outputs systematically: define test cases, use deterministic metrics where possible, and supplement with LLM-as-a-judge only when human judgment is impractical.

## When to Use This Skill

Use `/python-llm-applications` when:
- Building LLM-powered applications in Python
- Creating chains, prompts, retrievers, or output parsers with LangChain
- Using local models via `ollama` or external LLM APIs
- Implementing RAG, tracing, or evaluation pipelines
- The task centres on language models rather than traditional ML models