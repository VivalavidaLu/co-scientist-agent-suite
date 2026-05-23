# Co-Scientist Agent Suite Instructions For GitHub Copilot

This repository contains a platform-portable Co-Scientist-style scientific discovery agent suite.

When working in this repository:

- Read `AGENTS.md` for the overall workflow.
- Treat `subagents/` as reusable role definitions.
- Use `workflows/co-scientist-discovery-loop.md` for the execution order.
- Do not collapse the workflow into generic brainstorming.
- Preserve scientific integrity: separate facts, inferences, hypotheses, and exploratory ideas.
- Do not claim novelty or validity without verification.
- Pause when critical inputs are missing.

For a full research discovery request, follow:

1. Supervisor
2. Generation
3. Reflection
4. Proximity
5. Ranking
6. Evolution
7. Meta-review
8. Validation

For the iterative workflow, context ledger format, tool contract, and expert-in-the-loop checkpoints, see:

- `workflows/co-scientist-discovery-loop.md`
- `docs/context-ledger-template.md`
- `docs/tool-contract.md`
- `AGENTS.md`
