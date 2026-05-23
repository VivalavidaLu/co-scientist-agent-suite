# Co-Scientist Discovery Loop

This workflow operationalizes the paper architecture as a portable, iterative agent loop.

## 1. Supervisor Frame

Inputs:

- Research objective.
- Domain and disease context.
- User-provided evidence.
- Data files and available tools.
- Constraints and stop rules.

Output:

- Research plan configuration.
- Missing critical inputs.
- Stage plan.

Scientist-in-the-loop checkpoints:

- Accept initial goal definition.
- Ask for clarification when constraints are ambiguous.
- Register user-provided hypotheses as tournament candidates.
- Register user-provided critiques as manual reviews.

## 2. Generation

Generate diverse hypotheses satisfying:

- Alignment with the research goal.
- Plausibility.
- Novelty potential.
- Testability.
- Safety.

Use the paper-derived generation strategy library:

- Literature exploration and evidence synthesis.
- Simulated scientific debate to generate competing mechanisms.
- Assumption identification and conditional reasoning.
- Research expansion guided by previous meta-review feedback.

Label each hypothesis as:

- Verified fact.
- Data-supported inference.
- Literature-grounded hypothesis.
- Exploratory idea.

## 3. Reflection

Critique each hypothesis for:

- Correctness.
- Novelty.
- Assumptions.
- Confounders.
- Feasibility.
- Reproducibility.
- Safety and ethics.

Use layered review:

- Initial review: fast screen without external tools.
- Full review: literature/tool-grounded review when tools are available.
- Deep verification review: decompose assumptions into sub-assumptions.
- Observation review: test whether the hypothesis explains long-tail observations.
- Simulation review: step through the mechanism or experiment to identify failure modes.
- Recurrent/tournament review: adapt review focus based on ranking and debate results.

## 4. Proximity

Cluster and deduplicate hypotheses:

- Duplicate.
- Near duplicate.
- Same mechanism, different test.
- Same phenotype, different mechanism.
- Distinct.

## 5. Ranking

Prioritize candidates through transparent scoring or pairwise tournament comparison.

Use an Elo-style protocol when the platform can maintain state:

- Initialize new hypotheses at Elo 1200.
- Compare top-ranked hypotheses through multi-turn scientific debate.
- Compare lower-ranked hypotheses through shorter pairwise review.
- Prefer matches between similar hypotheses identified by Proximity.
- Prioritize new and high-ranking hypotheses for additional matches.

Default criteria:

- Novelty.
- Scientific validity.
- Logical coherence.
- Testability.
- Feasibility.
- Reproducibility.
- Translational value.
- Safety.

## 6. Evolution

Improve promising hypotheses without overwriting parent hypotheses.

Strategies:

- Grounding enhancement.
- Feasibility repair.
- Assumption reduction.
- Combination.
- Simplification.
- Divergent alternative.

## 7. Meta-review

Synthesize:

- Recurring weaknesses.
- Evidence gaps.
- Overrepresented mechanisms.
- Missing controls.
- Next-round instructions.
- Stop/go recommendation.

Produce:

- Research overview.
- Next-round prompt additions for Generation, Reflection, Ranking, and Evolution.
- Optional expert-contact profile: what type of expert should review the hypothesis. Name specific experts only when supported by verified public sources.

## 8. Validation

Convert top hypotheses into staged validation:

- Stage 0: computational or public-data sanity check.
- Stage 1: minimal experiment.
- Stage 2: mechanistic confirmation.
- Stage 3: translational feasibility check, if applicable.

Define supportive, falsifying, and ambiguous outcomes.

## Iteration Policy

After Meta-review, return to Generation or Evolution if:

- The top hypothesis still has repairable weaknesses.
- Proximity analysis shows narrow search-space coverage.
- Reflection identifies missing but answerable evidence.
- User injects a new hypothesis or review.
- The user refines the research goal or changes feasibility constraints.

Stop and ask the user when:

- Evidence gaps cannot be resolved with available tools.
- The next step requires choosing a disease model, dataset, assay, or budget tradeoff.
- Safety, ethics, or clinical overclaiming concerns appear.

## Expert-In-The-Loop Policy

The user can intervene between any two stages:

- refine the goal
- provide a manual review
- inject a new hypothesis
- change ranking criteria
- select final candidates for validation

These interventions must be recorded in the Context Ledger and propagated into Meta-review feedback for the next round.
