# Gemini / Antigravity Usage

This repository defines a Co-Scientist-style scientific discovery team.

Use `AGENTS.md` as the primary cross-platform instruction file and `subagents/` as role definitions.

If the environment does not support native subagent files, execute the roles sequentially:

1. Supervisor
2. Generation
3. Reflection
4. Proximity
5. Ranking
6. Evolution
7. Meta-review
8. Validation

Always preserve the scientific integrity rules from `AGENTS.md`.

For the iterative workflow, context ledger format, tool contract, and expert-in-the-loop checkpoints, see:

- `workflows/co-scientist-discovery-loop.md`
- `docs/context-ledger-template.md`
- `docs/tool-contract.md`
- `AGENTS.md`
