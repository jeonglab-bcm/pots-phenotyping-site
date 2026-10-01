# POTS PubMed harvest report
| field | value |
| --- | --- |
| run_id | run_20260903T072328Z |
| started_at | 2026-09-03T07:23:28+00:00 |
| finished_at | 2026-09-03T07:24:15+00:00 |
| query_profile | core |
| esearch_count | 2639 |
| pmids_retrieved | 2639 |
| articles_stored | 2639 |
| facet_queries_run | 41 |
| eutils_requests | 68 |
| tool_version | 0.1.0 |

## Checks

19 of 19 passed.

| result | check | detail |
| --- | --- | --- |
| PASS | corpus is non-empty | 2639 articles stored |
| PASS | every PMID esearch reported was retrieved | esearch reported 2639, retrieved 2639 |
| PASS | seed angeli2024 is in the corpus | expected PMID 38168762 found |
| PASS | seed angeli2024 lookup query still resolves correctly | lookup returned 1 hits, resolved to 38168762 by title_exact; expected 38168762 |
| PASS | seed gibbons2013 is in the corpus | expected PMID 24386408 found |
| PASS | seed gibbons2013 lookup query still resolves correctly | lookup returned 1 hits, resolved to 24386408 by title_exact; expected 24386408 |
| PASS | seed zhang2022 is in the corpus | expected PMID 36349067 found |
| PASS | seed zhang2022 lookup query still resolves correctly | lookup returned 1 hits, resolved to 36349067 by title_exact; expected 36349067 |
| PASS | seed larsen2025 (preprint, expected absent) | not indexed in PubMed; absence is expected, not a defect |
| PASS | seed low2009 is in the corpus | expected PMID 19207771 found |
| PASS | seed low2009 lookup query still resolves correctly | lookup returned 4 hits, resolved to 19207771 by title_exact; expected 19207771 |
| PASS | every facet tagged at least one article | all facets non-empty |
| PASS | every facet has at least one candidate concept | 41 of 41 facets mapped, 195 candidate rows |
| PASS | no undeclared concept-resolution misses | 9 declared fallback gaps, no undeclared misses |
| PASS | declared fallback gaps are still gaps (informational) | 9 of 9 declared gaps unresolved by the public fallback, as expected; supply --athena-dir to close them |
| PASS | ontology graph has no dangling edges | 0 dangling |
| PASS | no node is its own ancestor | 0 self-loops |
| PASS | curated vocabulary is internally consistent | clean |
| PASS | every declared MeSH descriptor is a real NLM descriptor | 149 descriptors checked against PubMed's MeSH index; 11 are valid but unused in this corpus |

## Corpus query clause contribution

`esearch_count` is the clause's hit count across all of PubMed; `n_in_corpus` is how many of those ended up in this corpus.

| block_id | esearch_count | n_in_corpus | rationale |
| --- | --- | --- | --- |
| mesh_pots | 927 | 927 | NLM-indexed POTS descriptor, introduced 2018. |
| tiab_acronym_guarded | 1273 | 1273 | Bare "POTS" is a badly overloaded acronym (cooking vessels, plain old telephone service, potassium abbreviatio |
| tiab_chronic_orthostatic_intolerance | 66 | 66 | Robertson's term for the same patient population; the Vanderbilt literature of the late 1990s uses it in place |
| tiab_historical | 552 | 552 | Historical labels for what is now recognised as overlapping with POTS, retained because the Low 2009 framework |
| tiab_orthostatic_tachycardia | 1410 | 1410 | Descriptive phrasing used before the syndrome was named. |
| tiab_pots_full | 1265 | 1265 | Full modern name, and the plural form used in reviews. |
| tiab_pts | 677 | 677 | "Postural tachycardia syndrome" is the name used by the Mayo group (Low et al.) and much of the pre-2015 liter |

### Clause exclusivity

`n_only_this_block` counts records that NO other clause retrieved. A clause with a large exclusive count is carrying the corpus on its own, which is where recall is won and where off-target noise enters.

| block_id | n_in_corpus | n_only_this_block |
| --- | --- | --- |
| tiab_historical | 552 | 551 |
| tiab_pts | 677 | 116 |
| tiab_orthostatic_tachycardia | 1410 | 86 |
| mesh_pots | 927 | 53 |
| tiab_acronym_guarded | 1273 | 46 |
| tiab_chronic_orthostatic_intolerance | 66 | 19 |
| tiab_pots_full | 1265 | 0 |

## Corpus composition

| metric | n | share |
| --- | --- | --- |
| articles | 2639 | 100.0% |
| with abstract | 1923 | 72.9% |
| MeSH indexed | 2006 | 76.0% |
| human check-tag | 1931 | 73.2% |
| animal check-tag | 34 | 1.3% |
| retracted | 0 | 0.0% |
| has DOI | 2141 | 81.1% |
| has PMC id | 1034 | 39.2% |
| non-English | 425 | 16.1% |

### Publication types, top 15

| type_name | n_articles |
| --- | --- |
| Journal Article | 2468 |
| Review | 401 |
| Research Support, Non-U.S. Gov't | 388 |
| Case Reports | 288 |
| Research Support, N.I.H., Extramural | 193 |
| English Abstract | 110 |
| Comparative Study | 91 |
| Letter | 90 |
| Research Support, U.S. Gov't, P.H.S. | 73 |
| Comment | 65 |
| Editorial | 65 |
| Clinical Trial | 49 |
| Randomized Controlled Trial | 44 |
| Systematic Review | 43 |
| Research Support, U.S. Gov't, Non-P.H.S. | 35 |

## Facet coverage

`query` counts title/abstract query hits inside the corpus; `mesh` counts records NLM indexed with one of the facet's MeSH descriptors. The two paths are independent.

| group | facet_id | query | mesh | any |
| --- | --- | --- | --- | --- |
| comorbidity | differential_syncope_oh | 520 | 294 | 615 |
| comorbidity | me_cfs | 437 | 134 | 450 |
| comorbidity | long_covid | 308 | 151 | 310 |
| comorbidity | chronic_migraine | 200 | 53 | 208 |
| comorbidity | hypermobility_heds | 193 | 90 | 198 |
| comorbidity | gi_dysmotility | 172 | 26 | 174 |
| comorbidity | post_vaccination_onset | 112 | 54 | 113 |
| comorbidity | mast_cell_activation | 97 | 26 | 101 |
| comorbidity | small_fiber_neuropathy_dx | 65 | 19 | 67 |
| comorbidity | systemic_autoimmune_disease | 44 | 28 | 65 |
| measurement | heart_rate_variability | 220 | 560 | 651 |
| measurement | head_up_tilt_test | 460 | 289 | 546 |
| measurement | active_stand_test | 123 | 273 | 381 |
| measurement | valsalva_maneuver | 95 | 316 | 373 |
| measurement | cerebral_blood_flow_measurement | 131 | 50 | 138 |
| measurement | genetic_testing | 111 | 13 | 112 |
| measurement | plasma_catecholamine_assay | 54 | 72 | 98 |
| measurement | plasma_blood_volume_assay | 74 | 33 | 84 |
| measurement | cardiopulmonary_exercise_test | 27 | 49 | 68 |
| measurement | ambulatory_monitoring | 47 | 27 | 57 |
| measurement | skin_biopsy_ienfd | 25 | 28 | 40 |
| measurement | autonomic_autoantibody_assay | 12 | 33 | 38 |
| measurement | qsart_sudomotor_testing | 26 | 8 | 32 |
| measurement | thermoregulatory_sweat_test | 10 | 5 | 12 |
| mechanism | neuropathic_evidence | 231 | 168 | 340 |
| mechanism | postinfectious_evidence | 328 | 155 | 332 |
| mechanism | hyperadrenergic_evidence | 223 | 139 | 274 |
| mechanism | hypovolemic_evidence | 176 | 48 | 187 |
| mechanism | autoimmune_evidence | 182 | 53 | 186 |
| mechanism | deconditioning_evidence | 105 | 42 | 129 |
| treatment | exercise_reconditioning | 140 | 81 | 185 |
| treatment | beta_blocker_therapy | 131 | 67 | 152 |
| treatment | ivabradine_therapy | 102 | 34 | 107 |
| treatment | volume_expansion_therapy | 62 | 50 | 93 |
| treatment | compression_therapy | 66 | 5 | 66 |
| treatment | midodrine_therapy | 54 | 27 | 61 |
| treatment | immunomodulatory_therapy | 41 | 17 | 45 |
| treatment | pyridostigmine_therapy | 22 | 10 | 25 |
| treatment | central_sympatholytic_therapy | 9 | 14 | 18 |
| treatment | serotonergic_therapy | 14 | 6 | 16 |
| treatment | droxidopa_therapy | 5 | 2 | 5 |

## Mechanism flag co-occurrence

Number of the six mechanism flags carried per article. Angeli et al. found overlapping rather than exclusive phenotypes in patients; the literature shows the same shape.

| n_mechanism_flags | n_articles |
| --- | --- |
| 0 | 1616 |
| 1 | 701 |
| 2 | 244 |
| 3 | 60 |
| 4 | 12 |
| 5 | 5 |
| 6 | 1 |

## Declared MeSH descriptors

| status | n_descriptors |
| --- | --- |
| real descriptor, used in this corpus | 138 |
| real descriptor, unused in this corpus | 11 |
| not recognised by PubMed | 0 |

Valid descriptors that no record in this corpus carries. Not errors: they mean no POTS paper is indexed that way, which is itself worth knowing when choosing between a MeSH-based and a text-based phenotype definition.

| facet_id | descriptor_name | n_in_all_of_pubmed |
| --- | --- | --- |
| exercise_reconditioning | Rehabilitation | 400902 |
| gi_dysmotility | Enteric Nervous System | 8096 |
| hypermobility_heds | Connective Tissue Diseases | 376525 |
| hypovolemic_evidence | Plasma Volume | 5239 |
| immunomodulatory_therapy | Immunomodulating Agents | 1273 |
| immunomodulatory_therapy | Rituximab | 21626 |
| plasma_blood_volume_assay | Plasma Volume | 5239 |
| plasma_catecholamine_assay | Metanephrine | 972 |
| serotonergic_therapy | Serotonin and Noradrenaline Reuptake Inhibitors | 706 |
| skin_biopsy_ienfd | Ubiquitin Thiolesterase | 6690 |
| systemic_autoimmune_disease | Thyroiditis, Autoimmune | 12556 |

## Ontology graph

| predicate | n_edges | transitive | subsumption |
| --- | --- | --- | --- |
| is_a | 62 | yes | yes |
| measures | 10 | no | no |
| evidence_for | 9 | no | no |
| indicates | 8 | no | no |
| proposed_mechanism_of | 7 | no | no |
| associated_with | 6 | no | no |
| treats | 5 | no | no |
| part_of | 4 | yes | no |
| trigger_of | 1 | no | no |
| differential_diagnosis_of | 1 | no | no |

Nodes: 70. Edges: 113. Closure rows: 151.

## OMOP concept resolution

| resolver | status | n_attempts |
| --- | --- | --- |
| athena_api | error | 1 |
| nlm_clinical_tables | ok | 18 |
| ols4 | no_match | 9 |
| ols4 | ok | 49 |
| rxnav | ok | 14 |
| vocabulary_id | n_candidates | n_facets | n_with_concept_id |
| --- | --- | --- | --- |
| SNOMED | 120 | 30 | 0 |
| LOINC | 50 | 12 | 0 |
| RxNorm | 25 | 9 | 0 |

### Match quality of candidate mappings

Token-based: `exact` means the same set of meaningful words once case, punctuation, SNOMED's semantic tag, LOINC's unit brackets and British spellings are normalised away. `contains` means every word of the search term appears in the concept name, usually a more specifically named form of the same thing. `loose` means at least one word is missing, so the service matched on something else.

| label_match | n_candidates | n_rank_1 |
| --- | --- | --- |
| contains | 118 | 26 |
| exact | 59 | 52 |
| loose | 18 | 3 |

3 best-candidate rows are loose matches and should be reviewed first. They are exported to `data/derived/mapping_review_priority.tsv`.

| facet_id | vocabulary | search_term | concept_code | concept_name |
| --- | --- | --- | --- | --- |
| gi_dysmotility | SNOMED | intestinal dysmotility | 253768006 | Congenital dysmotility of small intestine |
| immunomodulatory_therapy | RxNorm | immune globulin | 1426680 | immunoglobulin G, human |
| plasma_catecholamine_assay | LOINC | epinephrine plasma | 2056-0 | Catecholamines [Mass/volume] in Plasma |

195 candidate mappings are `unreviewed`. None of them should be treated as a phenotype definition until a human has checked them against ATHENA.

### Declared fallback gaps

These search terms are correct against full SNOMED but unreachable through the public fallback, because the EBI Ontology Lookup Service carries only a subset of SNOMED. They resolve once an ATHENA bundle is supplied with `--athena-dir`.

| facet_id | vocabulary | search_term |
| --- | --- | --- |
| hyperadrenergic_evidence | SNOMED | increased sympathetic activity |
| hyperadrenergic_evidence | SNOMED | orthostatic hypertension |
| autoimmune_evidence | SNOMED | autoimmune autonomic ganglionopathy |
| postinfectious_evidence | SNOMED | post-viral syndrome |
| qsart_sudomotor_testing | SNOMED | quantitative sudomotor axon reflex test |
| thermoregulatory_sweat_test | SNOMED | thermoregulatory sweat test |
| me_cfs | SNOMED | post-exertional malaise |
| long_covid | SNOMED | post COVID-19 condition |
| gi_dysmotility | SNOMED | gastrointestinal dysmotility |
