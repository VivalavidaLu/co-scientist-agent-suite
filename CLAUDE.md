# Claude Code Usage

This project contains Co-Scientist-style subagents in `subagents/`.

For Claude Code, copy or symlink these files into `.claude/agents/` if you want native subagent routing.

Recommended mapping:

- `subagents/co-scientist-supervisor.md`
- `subagents/co-scientist-generation.md`
- `subagents/co-scientist-reflection.md`
- `subagents/co-scientist-ranking.md`
- `subagents/co-scientist-evolution.md`
- `subagents/co-scientist-proximity.md`
- `subagents/co-scientist-meta-review.md`
- `subagents/co-scientist-validation.md`

When native delegation is unavailable, run the roles sequentially according to `workflows/co-scientist-discovery-loop.md`.

For the iterative workflow, context ledger format, tool contract, and expert-in-the-loop checkpoints, see:

- `workflows/co-scientist-discovery-loop.md`
- `docs/context-ledger-template.md`
- `docs/tool-contract.md`
- `AGENTS.md`
