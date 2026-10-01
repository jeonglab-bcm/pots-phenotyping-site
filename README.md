# POTS-phenotyping

A PubMed harvester and a curated phenotype vocabulary for deep phenotyping of
postural orthostatic tachycardia syndrome (POTS), built as the literature layer
under an eventual OMOP computable phenotype.

## The premise

POTS "subtypes" are not mutually exclusive categories. Angeli et al. classified
352 patients as hyperadrenergic, hypovolemic and/or neuropathic and found that
42% met two definitions and 11% met all three, with symptoms alone failing to
separate them. So this repository models overlapping **evidence flags** rather
than a partition, and derives combinations afterwards.

Seven mechanism flags form the intended phenotype output:

```
hyperadrenergic_evidence   neuropathic_evidence     hypovolemic_evidence
deconditioning_evidence    autoimmune_evidence      postinfectious_evidence
cerebral_hypoperfusion_evidence
```

Around them sit 65 more facets covering the objective tests that make those
flags observable, the comorbidities, triggers and differential diagnoses that
change their interpretation, and the treatments whose prescription implies a
clinician's mechanistic hypothesis. 72 facets in total.

**The ontology is built evidence first.** The questions it must answer are in
[`docs/COMPETENCY_QUESTIONS.md`](docs/COMPETENCY_QUESTIONS.md). Every link
that rests on the literature starts as a quoted claim from one of seven seed
papers in `config/relation_excerpts.yaml`, and `scripts/build_relations.py`
generates those links into `config/relations.yaml`. POTS is the root: links
point from tests and treatments to causes, and from causes and conditions to
POTS.

## What is here

| Path | Contents |
| --- | --- |
| `config/query.yaml` | The broad-recall PubMed query, as named clauses with a rationale each. One profile, `core`; narrower corpora are derived downstream from `article_query_block`. |
| `docs/COMPETENCY_QUESTIONS.md` | The ten questions the ontology answers, one per predicate. |
| `config/facets.yaml` | The 72-facet vocabulary: definitions, PubMed queries, MeSH descriptors, OMOP concept search terms, seed citations. |
| `config/relation_excerpts.yaml` | The source of truth for literature links: 132 claimed links backed by 401 verbatim passages from the seed papers (supports / partial / contradicts), plus claims not turned into links. |
| `config/relations.yaml` | The ontology: 10 predicates, 38 abstract classes, 102 hand-kept `is_a` edges and 132 generated literature links. |
| `scripts/build_relations.py`, `scripts/verify_excerpts.py` | Generate the literature links from the excerpts; check every quote against local copies of the seed texts. |
| `src/pots_phenotyping/` | The harvester, parser, SQLite store, concept resolvers, report generator and CLI. |
| `data/articles.jsonl` | The harvested corpus itself, one JSON object per article. Version-controlled: everything else under `data/` is derived from it. |
| `data/derived/*.tsv` | Version-controlled outputs of the last run: facet counts, the relation graph and its closure, candidate concept mappings, corpus composition. |
| `docs/HARVEST_REPORT.md` | Generated validation report for the last run. |
| `docs/DATA_DICTIONARY.md` | Every table and column. |
| `docs/ONTOLOGY.md` | The predicates, why the graph is shaped this way, and how to query it. |

## Design decisions

**One broad query defines the corpus; facets are tags, not filters.** A POTS
paper is never dropped for failing to mention a mechanism. Facet membership is a
column, so any inclusion rule can change without re-crawling.

**No filters at harvest time.** No publication type, language, species or date
restriction. Language, publication type, retraction status and MeSH species
check-tags are captured per record, so filtering is a `WHERE` clause.

**Two independent facet tag paths.** Each facet is applied twice: once from its
title/abstract PubMed query, once from NLM's own MeSH indexing of the record.
The two disagree often, and the disagreement is kept rather than reconciled.
`v_facet_evidence_disagreement` is the closest thing this corpus has to a
measure of how much a phenotype definition depends on free text versus curated
indexing.

**No concept code is hand-written.** `config/facets.yaml` declares *search
terms*, never codes or `concept_id` values. Resolvers look them up and record
provenance, and every mapping lands as `mapping_status='unreviewed'`.

**The curated graph and the resolved graph never mix.** Hand-authored edges live
in `ontology_edge` with `source='curated'`. Terminology-derived edges live in
`omop_concept_relationship` with the resolver named. Nothing silently promotes
one to the other.

## Results of the committed run

Profile `core`, harvested 2026-10-01. Full numbers in
[`docs/HARVEST_REPORT.md`](docs/HARVEST_REPORT.md).

| | |
| --- | --- |
| Articles | 2,668 (all of them; esearch reported 2,668) |
| With an abstract | 1,950 |
| MeSH indexed | 2,015 |
| Facet tag rows | 6,572 from queries, 4,099 from MeSH indexing |
| Ontology | 110 nodes, 234 edges (132 literature links), 271 closure rows |
| Candidate concept mappings | 308 across SNOMED, LOINC and RxNorm, all unreviewed |
| Validation checks | 23 of 23 pass |

Two findings worth knowing before you use the corpus:

- The historical-names clause (`neurocirculatory asthenia`, `soldier's heart`,
  `irritable heart`, `Da Costa syndrome`) contributes 556 records, **555 of
  which no other clause retrieves**. It is effectively a separate,
  mostly mid-twentieth-century corpus. Exclude it downstream by dropping
  records that only `tiab_historical` retrieved (`article_query_block`).
- `tiab_pots_full` contributes **zero** records that other clauses miss. It is
  fully subsumed and kept only for readability.

## Quick start

```bash
pip install -r requirements.txt

make check                      # validate the curated vocabulary, no network
make counts                     # how big is everything, esearch only
make smoke                      # 200-record harvest, end to end
make harvest                    # full corpus harvest with facet tagging
make ontology                   # resolve facets to OMOP concepts
make report                     # regenerate the report and derived tables
make test                       # offline test suite
```

Set `NCBI_API_KEY` to lift the E-utilities rate limit from 3 to 10 requests per
second, and `NCBI_TOOL_EMAIL` so NCBI can contact you rather than block you.

Explore the graph:

```bash
python -m pots_phenotyping.cli graph --node neuropathic_evidence
python -m pots_phenotyping.cli graph --node autonomic_reflex_screen
```

## OMOP concepts

The authoritative path is a vocabulary bundle downloaded from
[ATHENA](https://athena.ohdsi.org), which requires an interactive login and
therefore cannot be fetched by this code:

```bash
make ontology ATHENA_DIR=/path/to/athena-bundle
```

That gives real `concept_id` values, real `CONCEPT_RELATIONSHIP` rows including
`Is a` and `Part of`, and `CONCEPT_ANCESTOR` levels.

Without a bundle, three public services stand in, one per vocabulary:

| Vocabulary | Service | Gives | Does not give |
| --- | --- | --- | --- |
| SNOMED | EBI Ontology Lookup Service (OLS4) | codes, `is_a` parents and ancestors | `concept_id`, and its SNOMED slice is incomplete |
| LOINC | NLM Clinical Table Search Service | codes and long common names | `concept_id`, hierarchy |
| RxNorm | NLM RxNav | RXCUIs and ingredient relations | `concept_id` |

The ATHENA public web API is implemented as a resolver but currently answers
HTTP 403 to programmatic clients; the attempt is logged so the report says which
sources actually ran.

Nine search terms are marked `fallback_gap: true`. They are correct against full
SNOMED but unreachable through OLS4's subset, and they resolve once a bundle is
supplied. The report lists them separately from real errors.

## The record layer

`data/articles.jsonl` is the corpus, tracked in git: 2,668 lines, 18 MB, one
JSON object per article carrying the full parsed record including MeSH headings,
structured abstract sections, authors and reference PMIDs.

It is written by appending during a harvest and **compacted at the end of the
run**: deduplicated by PMID with the newest record winning, then sorted
numerically by PMID. So a re-harvest produces a readable diff of what actually
changed rather than a second copy appended to the first, and the file is
byte-identical for identical input. `pots-phenotyping compact` runs the pass on
its own.

Everything else under `data/` is derived and untracked, because both rebuild
from the JSONL with no network access:

```bash
pots-phenotyping rebuild     # data/db/ from data/articles.jsonl
pots-phenotyping report      # data/derived/ and docs/HARVEST_REPORT.md
```

`data/raw/` holds every efetch response verbatim and gzipped, so a change to the
parser can be replayed against the original XML without re-crawling.

## Limitations

- **PubMed only.** No full text, so the methods and results sections where
  phenotype thresholds actually live are out of reach. medRxiv is not indexed,
  so the Larsen long-COVID deep phenotyping preprint is absent by construction;
  the validation report treats its absence as expected.
- **Candidate mappings are candidates.** A string match against a concept name
  is not a phenotype definition. `data/derived/mapping_review_priority.tsv`
  lists the rows most likely to be wrong. A `contains` match can still be the
  wrong concept: "heart rate variability" resolves to "Fetal heart rate
  variability" through OLS4.
- **The claims were extracted by an AI assistant (Claude)** from seven seed
  papers and have not been reviewed by a clinician. Every quote is verbatim
  (`scripts/verify_excerpts.py`), but which passages were chosen, and whether a
  passage fully or only partly supports a link, are judgements to review.
  174 of the 401 passages are marked partial and 5 argue against their link.
  `reports/ontology_graph.html` shows each link with its quotes.
- **Facet queries are keyword queries.** They find papers that discuss a facet,
  not papers that measured it. Negation and hedging are not handled.

## Seed references

- Angeli et al. Symptom presentation by phenotype of postural orthostatic
  tachycardia syndrome. *Scientific Reports*, 2024. PMID 38168762.
- Gibbons et al. Structural and functional small fiber abnormalities in the
  neuropathic postural tachycardia syndrome. *PLoS ONE*, 2013. PMID 24386408.
- Zhang et al. Skin biopsy and quantitative sudomotor axon reflex testing in
  patients with postural orthostatic tachycardia syndrome. *Cureus*, 2022.
  PMID 36349067.
- Larsen et al. Long-COVID POTS: a deep phenotyping study. *medRxiv* preprint,
  2025. Not indexed in PubMed; only the abstract was used.
- Lau DH et al. Postural orthostatic tachycardia syndrome: a state-of-the-art
  review. *Heart, Lung and Circulation*, 2026. PMID 41519610.
- Chung TH, Raj SR. Postural orthostatic tachycardia syndrome (POTS): a review.
  *JAMA*, 2026. PMID 42635998. The publisher reserves text and data mining
  rights, so its quotes are kept short and withheld from the public site.
- Low et al. Postural tachycardia syndrome (POTS). *Journal of Cardiovascular
  Electrophysiology*, 2009. PMID 19207771.

Data comes from PubMed via NCBI E-utilities. Follow NCBI's
[usage policies](https://www.ncbi.nlm.nih.gov/home/about/policies/), and note
that SNOMED CT, LOINC and RxNorm each carry their own licence terms.
