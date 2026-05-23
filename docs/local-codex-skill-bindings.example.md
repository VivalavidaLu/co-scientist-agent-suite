# Local Codex Skill Bindings Example

This file is an example for private/local Codex setups. Do not hard-code these paths in the public agent prompts.

For concrete skill implementations, see the optional upstream repository:

- <https://github.com/K-Dense-AI/scientific-agent-skills>

Install only the skills needed for your workflow and review each `SKILL.md` before use.

## Recommended Core Skills

| Abstract capability | Suggested local skill | Why |
| --- | --- | --- |
| Peer-reviewed biomedical literature | `pubmed-database` | Novelty checks, evidence grounding, PMID/DOI retrieval |
| Recent preprints | `biorxiv-database` | Emerging mechanisms and unpublished-adjacent evidence |
| Clinical translation context | `clinicaltrials-database` | Trial status, phase, intervention landscape |
| Public expression datasets | `geo-database` | Public validation, cohort discovery, expression sanity checks |

## Additional Useful Skills

| Category | Suggested skills |
| --- | --- |
| Broad literature and citations | `openalex-database`, `citation-verifier`, `literature-review` |
| Targets and disease genetics | `opentargets-database`, `clinvar-database`, `ensembl-database`, `gwas-database` |
| Drugs and chemistry | `drugbank-database`, `pubchem`, `chembl`, `medchem` |
| Pathways and networks | `kegg-database`, `reactome`, `string`, `pathway-network-analyst` |
| Omics analysis | `exploratory-data-analysis`, `pydeseq2`, `anndata`, `cellxgene-census`, `omics-data-specialist` |
| Structure biology | `rcsb-pdb`, `alphafold-skill`, `esm` |
| Clinical evidence | `clinical-decision-support`, `clinicaltrials-database` |

## Binding Pattern

Use these skills through the host agent's normal skill invocation mechanism. The Co-Scientist suite should refer to the abstract capability, not the local filesystem path.

Example:

```text
For novelty review, use available peer-reviewed literature search tools such as pubmed-database.
For preprint coverage, use available preprint tools such as biorxiv-database.
For translational context, use available clinical trial tools such as clinicaltrials-database.
For public expression validation, use available dataset tools such as geo-database.
```

## Why Not Hard-Code These Skills?

- Other users may not have the same skills installed.
- Cursor, GitHub Copilot, Claude Code, Antigravity, and TRAE use different tool systems.
- Public repositories should remain portable.
- Missing tools should be recorded as unmet evidence requirements, not silently ignored.
