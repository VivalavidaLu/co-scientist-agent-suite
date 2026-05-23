# Example: AML Drug Repurposing Discovery Loop

This example mirrors the kind of biomedical task described in the Nature Co-Scientist paper, but it is only a prompt template. It does not validate any drug claim by itself.

## User Prompt

```text
Use the Co-Scientist agent suite to generate and prioritize drug repurposing hypotheses for acute myeloid leukemia.

Constraints:
- Prefer already approved drugs or compounds with known safety information.
- First validation should be low-cost.
- Do not claim clinical efficacy.
- Separate literature-grounded hypotheses from exploratory ideas.
- Output ranked hypotheses plus minimal validation plans.
```

## Expected Iterative Role Sequence

1. Supervisor frames the task and asks for disease subtype, available cell lines, and data sources if missing.
2. Generation proposes candidate hypotheses using literature exploration, simulated scientific debate, assumption identification, and research expansion.
3. Reflection performs layered review:
   - initial review
   - full literature/tool-grounded review when tools are available
   - deep verification review
   - observation review
   - simulation review
   - recurrent/tournament review
4. Proximity merges overlapping drug-mechanism hypotheses.
5. Ranking prioritizes candidates with an Elo-style tournament when state can be maintained:
   - initialize candidates at Elo 1200
   - compare similar hypotheses using Proximity-guided matching
   - use multi-turn debate for top or close candidates
   - use shorter pairwise comparison for lower-ranked candidates
6. Evolution repairs top candidates into more testable versions.
7. Meta-review summarizes recurring weaknesses, writes next-round prompt additions, and decides whether another Generation or Evolution round is justified.
8. Validation proposes staged tests.

Repeat steps 2-7 if the top hypotheses remain unstable, the search space is too narrow, or the user injects a new hypothesis or manual review.

## Expected Scientific Guardrails

- In vitro cytotoxicity is not clinical efficacy.
- Approved status for another indication does not guarantee safety in AML.
- A ranked hypothesis is not a validated therapeutic recommendation.
- Novelty requires literature verification.
- Combination therapy hypotheses require explicit synergy models and controls.

## Example Context Ledger Entries

```yaml
iteration: 1
research_goal: "Drug repurposing hypotheses for AML"
constraints:
  first_validation: "low-cost"
  clinical_overclaiming: "prohibited"
hypotheses:
  - id: H001
    statement: "Candidate drug mechanism hypothesis"
    evidence_status: "literature-grounded hypothesis"
elo_or_ranking_state:
  initial_elo: 1200
  current_ranking: []
tool_queries:
  - tool: "peer-reviewed literature search"
    query: "AML drug repurposing candidate mechanism"
unmet_evidence_requirements:
  - "No clinical trial context checked yet"
user_decisions_needed:
  - "Select AML subtype or available cell lines"
```
