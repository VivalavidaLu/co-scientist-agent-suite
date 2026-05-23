# Contributing

Contributions are welcome, especially improvements to platform adapters, scientific integrity checks, and optional tool mappings.

## Principles

- Keep the suite model-agnostic and platform-portable.
- Do not hard-code local filesystem paths, private tools, or personal credentials.
- Distinguish paper-derived roles from engineering extensions.
- Preserve strict separation between facts, inferences, hypotheses, and exploratory ideas.
- Do not add unsafe biological, clinical, or dual-use operational guidance.

## Suggested Contributions

- New platform adapters.
- Better examples for specific research domains.
- More precise tool contracts for public scientific databases.
- Improvements to the Context Ledger format.
- Tests or checklists for prompt consistency.

## Before Opening A Pull Request

- Check that no local paths or generated evaluation reports are included.
- Verify that new claims about platform support are backed by documentation.
- Keep source-of-truth content in `subagents/`, `AGENTS.md`, and `workflows/`; platform adapters should remain thin.

