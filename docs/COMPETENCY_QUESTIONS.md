# Competency questions

The ontology exists to answer these questions. Every predicate in
`config/relations.yaml` is here because one of them needs it, and every
literature link is a claim, quoted in `config/relation_excerpts.yaml`, that
answers one of them. Links point toward POTS, which is the root.

| # | Question | Predicate | Typical path |
|---|---|---|---|
| CQ1 | Which possible causes (mechanisms) are proposed for POTS? | `proposed_mechanism_of` | cause → POTS |
| CQ2 | Which objective tests provide evidence for a cause, or for POTS? | `evidence_for` | test → cause |
| CQ3 | Which treatments, when prescribed, imply that a clinician suspected a cause? | `indicates` | treatment → cause |
| CQ4 | Which treatments are used for POTS itself? | `treats` | treatment → POTS |
| CQ5 | Which conditions co-occur with POTS, or with a cause? | `associated_with` | condition → POTS or cause |
| CQ6 | Which exposures precede and trigger POTS or a cause? | `trigger_of` | exposure → POTS or cause |
| CQ7 | What must be ruled out before POTS can be diagnosed? | `differential_diagnosis_of` | condition → POTS |
| CQ8 | What does each test quantify, including the diagnostic heart-rate rise? | `measures` | test → quantity |
| CQ9 | Which tests are components of a test battery? | `part_of` | test → battery |
| CQ10 | What kind of thing is each topic, for mapping to OMOP domains? | `is_a` | topic → class |

CQ1–CQ9 are answered only by quoted claims from the seed papers. CQ10 is
structure (`taxonomy` provenance) and is not a literature claim.
