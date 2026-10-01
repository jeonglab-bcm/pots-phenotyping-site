# The relation graph

Two graphs are kept apart and joined only through facets.

**The curated graph** lives in `config/relations.yaml` and lands in
`ontology_node` / `ontology_edge` with `source='curated'`. Its nodes are the 41
facets plus 29 abstract classes. Every edge carries a provenance string naming
the seed reference it rests on, or `taxonomy` for pure structure.

**Every literature edge is backed by a quote.** `config/relation_excerpts.yaml`
holds, for each edge that cites a seed paper, the verbatim passage in that
paper and a verdict (`supports` or `partial`). On 2026-10-01 the graph was
refined against those excerpts: 9 edges with no supporting passage, or
contradicted by their source, were removed, and edges whose only support was in
a different seed paper were re-cited to it. The removed edges are listed under
`removed` in the same file with the reason. `tests/test_relation_excerpts.py`
keeps the two files consistent.

**The resolved graph** is built by `pots-phenotyping ontology` and lands in
`omop_concept` / `omop_concept_relationship` with the resolver named as the
source. It is never merged into the curated edges.

## Predicates

Only the first two are subsumption-like. The rest are domain relations and must
not be traversed as if they were `is_a`.

| Predicate | Transitive | Subsumption | Inverse | Meaning |
| --- | --- | --- | --- | --- |
| `is_a` | yes | yes | `subsumes` | Subsumption. |
| `part_of` | yes | no | `has_part` | Component of a composite test or battery. |
| `evidence_for` | no | no | `assessed_by` | A measurement supplies objective evidence for a mechanism flag. |
| `indicates` | no | no | `indicated_by` | Prescribing a treatment is weak evidence a clinician hypothesised a mechanism. |
| `treats` | no | no | `treated_by` | Therapeutic target. |
| `measures` | no | no | `measured_by` | The physiological quantity a test quantifies. |
| `associated_with` | no | no | itself (symmetric) | Non-causal co-occurrence. |
| `proposed_mechanism_of` | no | no | `has_proposed_mechanism` | A mechanism proposed to produce POTS. |
| `trigger_of` | no | no | `triggered_by` | Exposure temporally preceding onset. |
| `differential_diagnosis_of` | no | no | `has_differential_diagnosis` | Must be excluded before the diagnosis stands. |

## Why mechanisms are not subtypes

There is deliberately **no** `is_a` edge from any mechanism flag to `pots`. The
six flags relate to POTS by `proposed_mechanism_of` instead. An `is_a` edge
would make them subclasses of the disorder, and subclasses invite a partition,
and a partition is the thing Angeli et al. showed is false: 42% of 352 patients
met two of the three classical definitions and 11% met all three.

The mechanism flags do have `is_a` edges, but to `mechanistic_phenotype`, an
abstract class that groups axes of evidence. That is subsumption over *kinds of
evidence*, which is sound, rather than over *kinds of patient*, which is not.
`tests/test_config.py::test_mechanisms_are_not_modelled_as_subtypes_of_pots`
enforces this.

## Why `part_of` is separate from `is_a`

The Mayo autonomic reflex screen is one study with several components. QSART,
the Valsalva manoeuvre, heart rate variability and head-up tilt are `part_of`
it, and each also `is_a` some kind of autonomic test. Those are different
claims, and OMOP cares: in electronic health record data the battery may appear
as a single procedure code or as several, and a phenotype that traverses
`part_of` as if it were `is_a` will conclude a patient had a sudomotor test
because they had a tilt table.

The closure table keeps them apart, one row per predicate:

```sql
-- every ancestor of QSART by subsumption
SELECT ancestor_id, min_levels FROM ontology_closure
WHERE predicate = 'is_a' AND descendant_id = 'qsart_sudomotor_testing';

-- every component of the reflex screen, at any depth
SELECT descendant_id FROM ontology_closure
WHERE predicate = 'part_of' AND ancestor_id = 'autonomic_reflex_screen';
```

## Poly-hierarchy is intentional

`beta_blocker_therapy` is both a `negative_chronotrope` and a
`sympatholytic_agent`, because both are true and clinicians choose it for either
reason. The vocabulary validator checks for cycles among transitive predicates,
not for a single parent.

## Deliberate omissions

`ivabradine_therapy` has a `treats` edge to `pots` but **no** `indicates` edge
to any mechanism. It lowers heart rate without committing to a mechanism, so
inferring a phenotype from an ivabradine prescription would be wrong. The
absence is the assertion.

## Useful queries

Everything a measurement is evidence for, following subsumption on the
measurement side:

```sql
SELECT DISTINCT e.object_id AS mechanism
FROM ontology_edge e
WHERE e.predicate = 'evidence_for'
  AND e.subject_id IN (
    SELECT 'skin_biopsy_ienfd'
    UNION SELECT descendant_id FROM ontology_closure
    WHERE predicate = 'is_a' AND ancestor_id = 'structural_nerve_assessment'
  );
```

Which treatments imply each mechanism, with the reasoning attached:

```sql
SELECT object_id AS mechanism, subject_id AS treatment, provenance
FROM ontology_edge WHERE predicate = 'indicates' ORDER BY 1, 2;
```

A facet's full neighbourhood, from the command line:

```bash
python -m pots_phenotyping.cli graph --node neuropathic_evidence
```

## Exports

| File | Contents |
| --- | --- |
| `data/derived/ontology_nodes.tsv` | Every node with its kind, group and definition. |
| `data/derived/ontology_edges.tsv` | Every curated edge with predicate, provenance and source. |
| `data/derived/ontology_closure.tsv` | Transitive closure of `is_a` and `part_of`, with path lengths. |
| `data/derived/omop_concept_relationships.tsv` | Terminology-derived edges, resolver named. |

The closure is materialised rather than computed with a recursive CTE, so
"every descendant of X" is one join. It is rebuilt by `Store.rebuild_closure()`
whenever the vocabulary snapshot is written.
