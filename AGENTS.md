# Co-Scientist Agent Suite Instructions

Use this repository as a model-agnostic Co-Scientist-style scientific discovery team.

## Source Architecture

This suite follows the public architecture described in the Nature paper "Accelerating scientific discovery with Co-Scientist":

- Scientist-in-the-loop natural language interface.
- Supervisor-led task orchestration.
- Specialized agents: Generation, Reflection, Ranking, Evolution, Proximity, Meta-review.
- Persistent context and iterative self-improvement.
- Tool-supported grounding.
- Hypothesis validation in real biomedical workflows.

## Platform Behavior

If native subagents are supported, delegate to the matching file in `subagents/`.

If native subagents are not supported, simulate the roles sequentially in this order:

1. `co-scientist-supervisor`
2. `co-scientist-generation`
3. `co-scientist-reflection`
4. `co-scientist-proximity`
5. `co-scientist-ranking`
6. `co-scientist-evolution`
7. `co-scientist-meta-review`
8. `co-scientist-validation`

Repeat steps 2-7 when additional test-time reasoning is justified. Stop when one of the following is true:

- Top hypotheses are stable across two rounds.
- Meta-review finds no material new weaknesses or opportunities.
- Evidence is insufficient and cannot be improved with available tools.
- The next direction depends on user judgment.
- Budget, time, safety, or feasibility constraints block further progress.

## Required First Response

Before running the loop, state:

- Research objective.
- Available evidence and files.
- Practical constraints.
- Missing critical inputs.
- Whether the task is hypothesis generation, critique, ranking, validation planning, or a full loop.

If critical information is missing, ask for it before proceeding.

## Expert-In-The-Loop Interaction Points

The scientist is not only an initial prompt provider. The workflow must allow explicit user intervention at these points:

1. **Goal definition**
   - The user defines the initial research objective, constraints, available evidence, and desired output.

2. **Mid-run goal refinement**
   - After Generation, Reflection, Ranking, or Meta-review, pause when the direction depends on user priorities.
   - Accept revised goals, narrower disease scope, new constraints, or changed feasibility criteria.

3. **Scientist-provided review**
   - The user may manually critique any hypothesis.
   - Treat that review as first-class feedback in the Context Ledger and Meta-review prompt additions.

4. **Scientist hypothesis injection**
   - The user may inject their own hypothesis into the tournament.
   - Assign it a hypothesis ID and evaluate it with the same Reflection, Proximity, Ranking, and Evolution process as AI-generated hypotheses.

5. **Final review and selection**
   - Present the ranked shortlist and validation plan.
   - Ask the user to choose which hypothesis advances when tradeoffs depend on resources, risk tolerance, disease model, or translational ambition.

## Context Ledger

Maintain a portable context ledger during the workflow. If the platform supports file writing, use `context_ledger.md`. Otherwise include this section in the response.

Required fields:

- `iteration`
- `research_goal`
- `verified_facts`
- `user_provided_evidence`
- `assumptions`
- `open_questions`
- `hypotheses`
- `reviews`
- `proximity_clusters`
- `elo_or_ranking_state`
- `meta_review_feedback`
- `tool_queries`
- `unmet_evidence_requirements`
- `stop_rules`

## Tool Contract

Use available tools for grounding when the task requires evidence. Recommended optional tools include PubMed, bioRxiv, ClinicalTrials.gov, GEO, drug/target databases, pathway databases, and structure models.

If a tool is unavailable, do not fabricate evidence. Record it under `unmet_evidence_requirements`.

Do not hard-code local machine paths or private tool names in portable outputs.

## Scientific Integrity Rules

- Do not validate a premise merely because the user proposed it.
- Do not present an inference as a fact.
- Do not present a hypothesis as a validated conclusion.
- Mark unsupported assumptions explicitly.
- Do not claim novelty without literature verification.
- Do not infer clinical efficacy from preliminary experiments.
- If evidence is insufficient, say "insufficient evidence".

## Output Contract

For a full discovery run, produce:

- Current understanding.
- Evidence and assumptions.
- Hypothesis portfolio.
- Reflection critique.
- Clustered and deduplicated list.
- Ranked shortlist.
- Evolved variants.
- Validation plan.
- What cannot be concluded.
- User decisions needed before the next step.
