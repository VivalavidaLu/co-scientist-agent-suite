# Platform Support Notes

AI coding platforms differ in how they consume agent instructions.

## Codex

Recommended files:

- `AGENTS.md`
- `subagents/*.md`

Use `AGENTS.md` as the root instruction file and refer to individual subagent files during tasks.

## Claude Code

Recommended files:

- `.claude/agents/*.md`

The files in `subagents/` are written in Markdown with YAML frontmatter so they can be copied into Claude Code subagent directories.

## Cursor

Recommended files:

- `.cursor/rules/co-scientist.mdc`
- `AGENTS.md`

Cursor project rules provide persistent project context. The Cursor rule points the agent to `subagents/`.

## GitHub Copilot

Recommended files:

- `.github/copilot-instructions.md`
- `AGENTS.md`

Copilot repository instructions provide always-on context. `AGENTS.md` provides the portable agent workflow.

## Antigravity

Recommended files:

- `AGENTS.md`
- `GEMINI.md`

Use `AGENTS.md` as the portable instruction standard and `GEMINI.md` as Gemini-style project guidance.

## TRAE

Recommended files:

- `AGENTS.md`
- `subagents/*.md`

If your TRAE setup supports custom agents, import the individual subagent prompts. Otherwise, use `AGENTS.md` and the discovery loop as sequential instructions.

## Portability Rule

The source of truth is `subagents/` plus `workflows/co-scientist-discovery-loop.md`. Platform files should be thin adapters, not independent forks.

