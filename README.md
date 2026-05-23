# Co-Scientist Agent Suite

Model-agnostic, platform-portable subagent suite inspired by the Nature paper:

Gottweis et al., "Accelerating scientific discovery with Co-Scientist", Nature, 2026.

Original paper: <https://www.nature.com/articles/s41586-026-10644-y>

This repository is not an official Google, DeepMind, Gemini, or Nature implementation. It translates the public paper architecture into reusable agent instructions for coding-agent platforms such as Codex, Claude Code, Cursor, Antigravity, TRAE, and GitHub Copilot.

## Plain-Language Introduction

Co-Scientist Agent Suite is a portable research-agent template for scientific ideation. It breaks a research task into coordinated subagents that generate hypotheses, criticize them, cluster overlapping ideas, rank candidates, evolve stronger versions, summarize remaining weaknesses, and design validation plans. The goal is not to replace scientists, but to make hypothesis generation more structured, auditable, and falsifiable across different AI coding platforms.

## Design Goal

The paper describes Co-Scientist as a structured scientific thinking engine with:

- A natural language scientist-in-the-loop interface.
- A supervisor and asynchronous task framework.
- Specialized agents: Generation, Reflection, Ranking, Evolution, Proximity, and Meta-review.
- Persistent context memory for long-horizon reasoning.
- Tool use for literature grounding and specialized scientific models.
- Default criteria: alignment, plausibility, novelty, testability, and safety.
- Real-world biomedical validation after hypothesis generation.

This suite preserves that architecture at the prompt/instruction level while remaining model-agnostic. It does not require Gemini 2.0 and can be used with different AI coding platforms.

## Limitations vs. Original

This repository is a portable instruction-level reconstruction, not a reproduction of the original system.

What is simplified:

- **No proprietary Gemini infrastructure**: the original paper used Gemini-based agents and internal infrastructure; this suite is model-agnostic.
- **No guaranteed asynchronous runtime**: platforms with native subagents may run roles in parallel or asynchronously; platforms without that support should use the sequential fallback workflow.
- **No real persistent context memory by default**: this suite defines a portable `Context Ledger` format, but actual persistence depends on the host platform or repository files.
- **No automatic Elo engine by default**: the Ranking agent defines an Elo-style tournament protocol, but score storage and updates require platform support or explicit files.
- **No built-in database access**: literature, preprint, trial, GEO, drug, or protein-structure tools are optional integrations.
- **No automatic wet-lab validation**: the Validation agent produces validation plans only.

The `co-scientist-validation` subagent is an engineering extension. The Nature paper reports real-world validation workflows, but does not list a standalone Validation agent as one of the core specialized agents.

## Repository Layout

```text
co-scientist-agent-suite/
  README.md
  AGENTS.md
  CLAUDE.md
  GEMINI.md
  subagents/
    co-scientist-supervisor.md
    co-scientist-generation.md
    co-scientist-reflection.md
    co-scientist-ranking.md
    co-scientist-evolution.md
    co-scientist-proximity.md
    co-scientist-meta-review.md
    co-scientist-validation.md
  workflows/
    co-scientist-discovery-loop.md
  docs/
    nature-architecture-map.md
    platform-support.md
    tool-contract.md
    context-ledger-template.md
    local-codex-skill-bindings.example.md
  .cursor/rules/
    co-scientist.mdc
  .github/
    copilot-instructions.md
```

## Subagents

| Paper role | File | Purpose |
| --- | --- | --- |
| Supervisor | `subagents/co-scientist-supervisor.md` | Parse research goals, allocate stages, maintain stop rules |
| Generation | `subagents/co-scientist-generation.md` | Generate diverse hypotheses and research proposals |
| Reflection | `subagents/co-scientist-reflection.md` | Critique correctness, novelty, assumptions, safety |
| Ranking | `subagents/co-scientist-ranking.md` | Run pairwise/tournament-style prioritization |
| Evolution | `subagents/co-scientist-evolution.md` | Improve top hypotheses without overwriting parents |
| Proximity | `subagents/co-scientist-proximity.md` | Cluster, deduplicate, and map related hypotheses |
| Meta-review | `subagents/co-scientist-meta-review.md` | Summarize review patterns and guide next iteration |
| Validation | `subagents/co-scientist-validation.md` | Engineering extension: convert top hypotheses into staged validation plans |

## Quick Start

### Universal

Keep `AGENTS.md` at the repository root. Most modern coding agents can read or be directed to read repository-level agent instructions.

### Claude Code

Copy files from `subagents/` into:

```text
.claude/agents/
```

Claude Code supports Markdown subagent files with YAML frontmatter.

### Codex

Keep `AGENTS.md` and `subagents/` in the repository. When asking Codex to use the suite, reference the root `AGENTS.md` and the desired subagent file.

### Cursor

Use:

```text
.cursor/rules/co-scientist.mdc
```

Cursor project rules can reference `subagents/` as reusable role definitions.

### GitHub Copilot

Use:

```text
.github/copilot-instructions.md
AGENTS.md
```

Copilot can consume repository instructions and, where supported, agent instructions from `AGENTS.md`.

### Antigravity

Use:

```text
AGENTS.md
GEMINI.md
```

The suite includes both generic agent instructions and Gemini-style project guidance.

### TRAE

Use `AGENTS.md` as the portable project-level instruction source, and import or paste individual files from `subagents/` as custom agents where your TRAE setup supports role prompts.

## Scientific Integrity

The suite enforces these rules:

- Never present a hypothesis as a validated conclusion.
- Never claim novelty without literature checking.
- Separate verified facts, data-supported inferences, literature-grounded hypotheses, and exploratory ideas.
- Pause when critical inputs are missing.
- Prefer falsifiable validation plans over broad speculative programs.
- Do not infer clinical efficacy from in vitro or in silico results.

## Optional Tool Integrations

For biomedical work, the suite works best when the host platform can provide search/database tools. Recommended optional integrations:

- PubMed or MEDLINE search for peer-reviewed biomedical literature.
- bioRxiv or medRxiv search for recent preprints.
- ClinicalTrials.gov search for translational and clinical trial context.
- GEO or other omics repositories for public expression datasets.
- Drug, target, pathway, and protein-structure databases when relevant.

Do not hard-code local skill paths in a public repository. Instead, expose these as optional tools and record unavailable tools as unmet evidence requirements. See `docs/tool-contract.md`.

### Recommended Skill Source

For users who want ready-made scientific database and analysis skills, refer to:

- <https://github.com/K-Dense-AI/scientific-agent-skills>

That repository describes itself as a broad collection of scientific and research Agent Skills compatible with agent hosts such as Cursor, Claude Code, Codex, and others. Use it as an optional upstream source for capabilities such as PubMed, bioRxiv, ClinicalTrials.gov, GEO, drug databases, pathway databases, omics analysis, and scientific writing.

Security note: install only the skills you need, review each `SKILL.md`, and avoid treating third-party skills as automatically trusted dependencies.

## Scope Limits

This suite does not provide:

- The original Co-Scientist source code.
- Gemini-specific internal infrastructure.
- Automated literature access or wet-lab execution by itself.
- Validation of scientific claims without user-provided data or verified sources.
