# Context Ledger Template

Use this file format when the platform can persist project state. If not, include the same fields in the response.

```yaml
iteration: 0
research_goal: ""
constraints:
  time: ""
  budget: ""
  available_data: []
  available_models: []
  unavailable_tools: []
verified_facts: []
user_provided_evidence: []
assumptions: []
open_questions: []
hypotheses:
  - id: H001
    statement: ""
    evidence_status: ""
    parent_ids: []
    status: active
reviews:
  - hypothesis_id: H001
    review_type: initial
    verdict: ""
    missing_evidence: []
proximity_clusters: []
elo_or_ranking_state:
  initial_elo: 1200
  matches: []
  current_ranking: []
meta_review_feedback:
  recurring_weaknesses: []
  prompt_additions: []
tool_queries:
  - tool: ""
    query: ""
    date: ""
    identifiers: []
    evidence_tier: ""
unmet_evidence_requirements: []
stop_rules: []
user_decisions_needed: []
```

