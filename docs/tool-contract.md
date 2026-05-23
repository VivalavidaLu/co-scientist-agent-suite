# Optional Tool Contract

This suite should remain platform-portable. Do not hard-code local skill paths or private tool names in public agent files.

Instead, each host environment can bind available tools to these abstract capabilities.

## Recommended Biomedical Tool Capabilities

| Capability | Example tools or skills | Used by |
| --- | --- | --- |
| Peer-reviewed literature search | PubMed, MEDLINE, Europe PMC, OpenAlex | Generation, Reflection |
| Preprint search | bioRxiv, medRxiv | Generation, Reflection |
| Clinical trial search | ClinicalTrials.gov | Reflection, Validation |
| Public expression datasets | GEO, ArrayExpress, SRA metadata | Generation, Validation |
| Drug and target lookup | DrugBank, ChEMBL, PubChem, Open Targets | Generation, Reflection, Validation |
| Pathway and network lookup | Reactome, KEGG, STRING, GO | Generation, Proximity, Reflection |
| Variant and disease evidence | ClinVar, OMIM, GWAS Catalog, Ensembl | Reflection, Validation |
| Protein structure or design support | AlphaFold, ESM, RCSB PDB | Generation, Validation |

## Optional Upstream Skill Repository

For concrete Agent Skill implementations, users can refer to:

- <https://github.com/K-Dense-AI/scientific-agent-skills>

This suite should treat that repository as an optional upstream source, not a required dependency. Public agent prompts should refer to abstract capabilities such as "peer-reviewed literature search" or "public expression dataset search" rather than assuming that a specific local skill is installed.

Recommended pattern:

```text
If available, use scientific database skills from K-Dense-AI/scientific-agent-skills or equivalent tools.
If unavailable, record the missing database lookup under unmet_evidence_requirements.
```

## Local Codex Skill Mapping Example

In a local/private Codex setup, these abstract capabilities can be mapped to skills such as:

- `pubmed-database`
- `biorxiv-database`
- `clinicaltrials-database`
- `geo-database`
- `openalex-database`
- `drugbank-database`
- `opentargets-database`
- `kegg-database`
- `reactome`
- `string`
- `clinvar-database`
- `ensembl-database`
- `gwas-database`
- `pubchem`
- `rcsb-pdb`

The public repository should describe these as optional integrations, not required dependencies.

## Tool Use Rules

1. Use tools when claims depend on current literature, trials, datasets, drugs, or external biology resources.
2. Store each query in the Context Ledger:
   - tool or database
   - query
   - date
   - filters
   - returned identifiers
   - evidence tier
3. If a tool is unavailable, record an unmet evidence requirement.
4. Never fabricate citations, trial IDs, accessions, drug approvals, or database results.
5. Do not claim novelty until an appropriate literature/preprint search has been performed.
