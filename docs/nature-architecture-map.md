# Nature Paper Architecture Map

This file maps the public Co-Scientist paper concepts to this repository.

| Paper concept | Repository implementation |
| --- | --- |
| Natural language scientist interface | `AGENTS.md`, `co-scientist-supervisor` |
| Supervisor agent | `subagents/co-scientist-supervisor.md` |
| Asynchronous task execution framework | Platform-dependent delegation; sequential fallback in `workflows/co-scientist-discovery-loop.md` |
| Generation agent | `subagents/co-scientist-generation.md` |
| Reflection agent | `subagents/co-scientist-reflection.md` |
| Ranking agent and tournament | `subagents/co-scientist-ranking.md` |
| Evolution agent | `subagents/co-scientist-evolution.md` |
| Proximity agent | `subagents/co-scientist-proximity.md` |
| Meta-review agent | `subagents/co-scientist-meta-review.md` |
| Persistent context memory | Implemented as explicit context ledger sections in outputs; platform memory is optional |
| Tool use | Delegated to platform tools, literature search, databases, scripts, or user-provided files |
| Default criteria | Alignment, plausibility, novelty, testability, safety |
| Real-world validation | `subagents/co-scientist-validation.md` as an engineering extension |

## Important Difference From The Paper

The paper implementation used Gemini models and proprietary infrastructure. This repository does not reproduce that infrastructure. It preserves the role architecture and reasoning loop as platform-portable instructions.

## Validation Agent Note

The paper describes real-world validation of generated hypotheses, but does not define a standalone Validation agent alongside Generation, Reflection, Ranking, Evolution, Proximity, and Meta-review. This suite adds `co-scientist-validation` as an engineering extension to make the final validation-planning step explicit and reusable.

## Runtime Approximation

The original system used an asynchronous task execution framework, persistent context memory, and tournament dynamics. This repository approximates those mechanisms through:

- Platform-native subagents where available.
- Sequential fallback where subagents are unavailable.
- A portable Context Ledger format.
- An Elo-style tournament protocol in the Ranking agent.
- Explicit iteration policy in the workflow.

## Biomedical Validation Scope

The Nature paper validated the system in biomedical use cases such as drug repurposing, target discovery, and antimicrobial resistance mechanism explanation. This suite supports the same categories at the planning and reasoning level, but all claims require independent verification.
