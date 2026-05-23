# Platform Packaging Guide

This guide explains how to package or install the Co-Scientist Agent Suite across common AI coding platforms. The word "plugin" means different things on different platforms, so this guide uses each platform's native extension mechanism instead of pretending there is one universal format.

Checked: 2026-05-24.

## Quick Mapping

| Platform | Native mechanism | Recommended package for this suite |
| --- | --- | --- |
| Codex | Codex plugin with `.codex-plugin/plugin.json` | Use `plugin-creator`; see `docs/build-with-plugin-creator.md` |
| Claude Code | Claude Code plugin with `.claude-plugin/plugin.json` | Convert `subagents/` to plugin `agents/`; optionally add a marketplace |
| Google Antigravity | Plugin directory with `plugin.json`, plus optional `skills/`, `agents/`, `rules/`, MCP, hooks | Package as `.agents/plugins/co-scientist-agent-suite/` or global Antigravity plugin |
| Cursor | Project Rules `.cursor/rules/*.mdc`, AGENTS.md, Custom Modes | Keep `.cursor/rules/co-scientist.mdc`; optionally define a Custom Mode |
| TRAE | Custom agents, project rules, MCP configuration | Import roles as custom agents; use rules/MCP where supported |
| GitHub Copilot | Custom agent profiles in `.github/agents/`, repo instructions, MCP | Add `.github/agents/*.md` profiles for each role; keep `.github/copilot-instructions.md` |

## Claude Code

Claude Code has a true plugin format. Official docs describe plugins as directories with `.claude-plugin/plugin.json` and root-level components such as `skills/`, `agents/`, `hooks/`, `.mcp.json`, `.lsp.json`, and `monitors/`.

Recommended structure:

```text
co-scientist-agent-suite-claude-plugin/
  .claude-plugin/
    plugin.json
  agents/
    co-scientist-supervisor.md
    co-scientist-generation.md
    co-scientist-reflection.md
    co-scientist-proximity.md
    co-scientist-ranking.md
    co-scientist-evolution.md
    co-scientist-meta-review.md
    co-scientist-validation.md
  skills/
    co-scientist-agent-suite/
      SKILL.md
  README.md
  docs/
```

Minimal manifest:

```json
{
  "name": "co-scientist-agent-suite",
  "description": "Portable Co-Scientist-style scientific discovery agents inspired by the Nature paper.",
  "version": "0.1.0"
}
```

Build steps:

1. Create `.claude-plugin/plugin.json`.
2. Copy this repository's `subagents/*.md` into plugin root `agents/`.
3. Keep `AGENTS.md`, `workflows/`, and `docs/` as supporting documentation.
4. If you want a slash-command-style entry point, add `skills/co-scientist-agent-suite/SKILL.md` that points Claude to `AGENTS.md` and the role files.
5. Validate locally:

```bash
claude plugin validate <plugin-path>
```

6. For team distribution, create a Claude Code marketplace repository with `.claude-plugin/marketplace.json`, then ask users to add it with `/plugin marketplace add`.

Important: Do not put `agents/`, `skills/`, or `hooks/` inside `.claude-plugin/`; only `plugin.json` belongs there.

Sources:

- <https://code.claude.com/docs/en/plugins>
- <https://code.claude.com/docs/en/plugins-reference>
- <https://code.claude.com/docs/en/plugin-marketplaces>

## Google Antigravity

Antigravity has a native plugin concept. A plugin is a directory containing `plugin.json` and optional subdirectories such as `skills/`, `agents/`, `rules/`, plus optional `mcp_config.json` and `hooks.json`.

Workspace-level package:

```text
.agents/
  plugins/
    co-scientist-agent-suite/
      plugin.json
      agents/
        co-scientist-generation.md
        co-scientist-reflection.md
        ...
      rules/
        co-scientist.md
      skills/
        co-scientist-agent-suite/
          SKILL.md
      mcp_config.json        # optional
      hooks.json             # optional
```

Minimal `plugin.json`:

```json
{
  "name": "co-scientist-agent-suite"
}
```

Recommended mapping:

- `subagents/*.md` -> `agents/*.md`
- `AGENTS.md` and `workflows/co-scientist-discovery-loop.md` -> `rules/co-scientist.md` or `skills/co-scientist-agent-suite/SKILL.md`
- `docs/tool-contract.md` -> keep as supporting reference
- Scientific database tools -> optional `mcp_config.json`; do not bundle private credentials

Install locations:

- Workspace: `.agents/plugins/co-scientist-agent-suite/`
- Global desktop Antigravity: `~/.gemini/config/plugins/co-scientist-agent-suite/`
- Antigravity CLI staged plugins: `~/.gemini/antigravity-cli/plugins/<plugin_name>/`

Sources:

- <https://antigravity.google/docs/plugins?app=antigravity>
- <https://antigravity.google/docs/cli-features>
- <https://antigravity.google/docs/rules-workflows>
- <https://www.antigravity.google/docs/hooks>

## Cursor

Cursor does not need a packaged plugin for this suite. The native, version-controlled mechanism is Project Rules in `.cursor/rules`, written as MDC files with frontmatter. Cursor also supports `AGENTS.md` as a simpler agent-instruction file, and Custom Modes for specialized tool/instruction combinations.

Current repository support:

```text
.cursor/
  rules/
    co-scientist.mdc
AGENTS.md
subagents/
workflows/
docs/
```

Recommended build steps:

1. Keep `.cursor/rules/co-scientist.mdc`.
2. Make the rule `Agent Requested` or manual if you only want it for research tasks; use `alwaysApply: false`.
3. In Cursor, create or enable a Custom Mode named `Co-Scientist` if you want a dedicated research workflow.
4. Point the mode/rule to `AGENTS.md`, `subagents/`, and `workflows/co-scientist-discovery-loop.md`.
5. Treat this as a rule pack, not a compiled plugin. Shared team distribution currently means keeping the rule files in the repository or copying/symlinking them across projects.

Sources:

- <https://docs.cursor.com/context/rules>
- <https://docs.cursor.com/chat/custom-modes>

## TRAE

I did not find a stable official public manifest format equivalent to Claude Code or Antigravity plugins. Public TRAE documentation describes built-in agents and custom agents configured through prompts and toolsets, with MCP/rules support appearing in current TRAE IDE documentation and release material.

Recommended packaging approach:

```text
trae/
  agents/
    co-scientist-supervisor.md
    co-scientist-generation.md
    co-scientist-reflection.md
    ...
  rules/
    co-scientist-project-rules.md
  README-trae.md
```

Recommended build steps:

1. Create one TRAE custom agent for each core role: Supervisor, Generation, Reflection, Proximity, Ranking, Evolution, Meta-review, and Validation.
2. Paste the corresponding `subagents/co-scientist-*.md` content into each custom agent prompt.
3. Add a project rule that points TRAE to `AGENTS.md` and `workflows/co-scientist-discovery-loop.md`.
4. Configure tools/MCP only if the user's TRAE installation supports them. Treat PubMed, bioRxiv, GEO, ClinicalTrials.gov, and other databases as optional tool contracts.
5. If TRAE exposes one-click custom-agent import/export in the user's installed version, export these agents as a TRAE-specific bundle. Otherwise, keep the Markdown files as the portable source of truth.

Scope note: because the public TRAE plugin/import surface is less documented than Claude Code or Antigravity, this repository should not claim an official TRAE plugin package until a concrete manifest/import format is verified.

Sources:

- <https://www.traehistory.com/docs/ai-code-generation>
- <https://traeide.com/docs>

## GitHub Copilot

GitHub Copilot now supports custom agents as Markdown agent profiles. Repository-level custom agents live in `.github/agents/CUSTOM-AGENT-NAME.md`. Agent profiles use YAML frontmatter plus a Markdown prompt, and can configure tools and MCP servers.

Recommended structure:

```text
.github/
  copilot-instructions.md
  agents/
    co-scientist-supervisor.md
    co-scientist-generation.md
    co-scientist-reflection.md
    co-scientist-proximity.md
    co-scientist-ranking.md
    co-scientist-evolution.md
    co-scientist-meta-review.md
    co-scientist-validation.md
```

Example agent profile:

```markdown
---
name: co-scientist-generation
description: Generate diverse, testable scientific hypotheses while preserving evidence boundaries.
tools: ["read", "search", "edit"]
---

You are the Generation agent in a Co-Scientist-style research team.

Read AGENTS.md, then follow subagents/co-scientist-generation.md.
Do not claim novelty without verified literature or database evidence.
```

Recommended build steps:

1. Keep `.github/copilot-instructions.md` as repository-level guidance.
2. Add `.github/agents/*.md` profiles for the eight Co-Scientist roles.
3. Use `tools` to limit high-risk agents. For example, Reflection and Ranking can often be read/search only; Validation may need edit/search depending on whether it writes plans.
4. Add MCP tools only through explicit repository or agent-profile configuration. Do not hard-code private credentials.
5. Invoke custom agents through Copilot CLI or supported Copilot surfaces by selecting the agent, using `/agent`, or referring to the agent in natural language.

Avoid new GitHub App-based Copilot Extensions for this suite. GitHub's docs state that Copilot Extensions built as GitHub Apps were scheduled to close on November 10, 2025, and recommend MCP servers where applicable. VS Code Copilot Extensions are a different client-side extension path and are not the right default for this prompt-level suite.

Sources:

- <https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents>
- <https://docs.github.com/en/copilot/reference/custom-agents-configuration>
- <https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli-agents/invoke-custom-agents>
- <https://docs.github.com/copilot/how-tos/provide-context/install-copilot-extensions/use-copilot-extensions>

## Repository Recommendation

For this suite, keep the repository as the portable source of truth and provide platform adapters:

- `AGENTS.md`: universal instructions.
- `subagents/`: canonical role definitions.
- `workflows/`: iterative discovery loop.
- `.cursor/rules/`: Cursor adapter.
- `.github/copilot-instructions.md`: Copilot repository instructions.
- `codex-agents/`: Codex local agent adapter.
- Future optional adapters:
  - `.github/agents/` for Copilot custom agents.
  - `.agents/plugins/` or an Antigravity export folder.
  - `.claude-plugin/` plus `agents/` for a Claude Code plugin package.

This separation keeps scientific behavior consistent while respecting each platform's actual packaging model.
