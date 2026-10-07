# POTS PubMed harvest report
| field | value |
| --- | --- |
| run_id | run_20261001T190057Z |
| started_at | 2026-10-01T19:00:57+00:00 |
| finished_at | 2026-10-01T19:02:03+00:00 |
| query_profile | core |
| esearch_count | 2668 |
| pmids_retrieved | 2668 |
| articles_stored | 2668 |
| facet_queries_run | 72 |
| eutils_requests | 101 |
| tool_version | 0.1.0 |

## Checks

23 of 23 passed.

| result | check | detail |
| --- | --- | --- |
| PASS | corpus is non-empty | 2668 articles stored |
| PASS | every PMID esearch reported was retrieved | esearch reported 2668, retrieved 2668 |
| PASS | seed angeli2024 is in the corpus | expected PMID 38168762 found |
| PASS | seed angeli2024 lookup query still resolves correctly | lookup returned 1 hits, resolved to 38168762 by title_exact; expected 38168762 |
| PASS | seed gibbons2013 is in the corpus | expected PMID 24386408 found |
| PASS | seed gibbons2013 lookup query still resolves correctly | lookup returned 1 hits, resolved to 24386408 by title_exact; expected 24386408 |
| PASS | seed zhang2022 is in the corpus | expected PMID 36349067 found |
| PASS | seed zhang2022 lookup query still resolves correctly | lookup returned 1 hits, resolved to 36349067 by title_exact; expected 36349067 |
| PASS | seed larsen2025 (preprint, expected absent) | not indexed in PubMed; absence is expected, not a defect |
| PASS | seed lau2026 is in the corpus | expected PMID 41519610 found |
| PASS | seed lau2026 lookup query still resolves correctly | lookup returned 1 hits, resolved to 41519610 by title_exact; expected 41519610 |
| PASS | seed chung2026 is in the corpus | expected PMID 42635998 found |
| PASS | seed chung2026 lookup query still resolves correctly | lookup returned 1 hits, resolved to 42635998 by title_exact; expected 42635998 |
| PASS | seed low2009 is in the corpus | expected PMID 19207771 found |
| PASS | seed low2009 lookup query still resolves correctly | lookup returned 4 hits, resolved to 19207771 by title_exact; expected 19207771 |
| PASS | every facet tagged at least one article | all facets non-empty |
| PASS | every facet has at least one candidate concept | 72 of 72 facets mapped, 308 candidate rows |
| PASS | no undeclared concept-resolution misses | 9 declared fallback gaps, no undeclared misses |
| PASS | declared fallback gaps are still gaps (informational) | 9 of 9 declared gaps unresolved by the public fallback, as expected; supply --athena-dir to close them |
| PASS | ontology graph has no dangling edges | 0 dangling |
| PASS | no node is its own ancestor | 0 self-loops |
| PASS | curated vocabulary is internally consistent | clean |
| PASS | every declared MeSH descriptor is a real NLM descriptor | 224 descriptors checked against PubMed's MeSH index; 29 are valid but unused in this corpus |

## Corpus query clause contribution

`esearch_count` is the clause's hit count across all of PubMed; `n_in_corpus` is how many of those ended up in this corpus.

| block_id | esearch_count | n_in_corpus | rationale |
| --- | --- | --- | --- |
| mesh_pots | 932 | 932 | NLM-indexed POTS descriptor, introduced 2018. |
| tiab_acronym_guarded | 1286 | 1286 | Bare "POTS" is a badly overloaded acronym (cooking vessels, plain old telephone service, potassium abbreviatio |
| tiab_chronic_orthostatic_intolerance | 67 | 67 | Robertson's term for the same patient population; the Vanderbilt literature of the late 1990s uses it in place |
| tiab_historical | 556 | 556 | Historical labels for what is now recognised as overlapping with POTS, retained because the Low 2009 framework |
| tiab_orthostatic_tachycardia | 1433 | 1433 | Descriptive phrasing used before the syndrome was named. |
| tiab_pots_full | 1287 | 1287 | Full modern name. The phrase also covers "... tachycardia syndrome" and the plural form used in reviews. |
| tiab_pts | 679 | 679 | "Postural tachycardia syndrome" is the name used by the Mayo group (Low et al.) and much of the pre-2015 liter |

### Clause exclusivity

`n_only_this_block` counts records that NO other clause retrieved. A clause with a large exclusive count is carrying the corpus on its own, which is where recall is won and where off-target noise enters.

| block_id | n_in_corpus | n_only_this_block |
| --- | --- | --- |
| tiab_historical | 556 | 555 |
| tiab_pts | 679 | 116 |
| tiab_orthostatic_tachycardia | 1433 | 87 |
| mesh_pots | 932 | 53 |
| tiab_acronym_guarded | 1286 | 46 |
| tiab_chronic_orthostatic_intolerance | 67 | 19 |
| tiab_pots_full | 1287 | 0 |

## Corpus composition

| metric | n | share |
| --- | --- | --- |
| articles | 2668 | 100.0% |
| with abstract | 1950 | 73.1% |
| MeSH indexed | 2015 | 75.5% |
| human check-tag | 1940 | 72.7% |
| animal check-tag | 34 | 1.3% |
| retracted | 0 | 0.0% |
| has DOI | 2170 | 81.3% |
| has PMC id | 1048 | 39.3% |
| non-English | 425 | 15.9% |

### Publication types, top 15

| type_name | n_articles |
| --- | --- |
| Journal Article | 2491 |
| Review | 403 |
| Research Support, Non-U.S. Gov't | 392 |
| Case Reports | 291 |
| Research Support, N.I.H., Extramural | 200 |
| English Abstract | 110 |
| Letter | 96 |
| Comparative Study | 91 |
| Research Support, U.S. Gov't, P.H.S. | 73 |
| Comment | 65 |
| Editorial | 65 |
| Clinical Trial | 49 |
| Randomized Controlled Trial | 44 |
| Systematic Review | 44 |
| Research Support, U.S. Gov't, Non-P.H.S. | 36 |

## Facet coverage

`query` counts title/abstract query hits inside the corpus; `mesh` counts records NLM indexed with one of the facet's MeSH descriptors. The two paths are independent.

| group | facet_id | query | mesh | any |
| --- | --- | --- | --- | --- |
| comorbidity | differential_syncope_oh | 458 | 261 | 549 |
| comorbidity | me_cfs | 441 | 136 | 454 |
| comorbidity | long_covid | 313 | 153 | 315 |
| comorbidity | anxiety_depression | 230 | 94 | 242 |
| comorbidity | chronic_migraine | 202 | 54 | 210 |
| comorbidity | hypermobility_heds | 198 | 91 | 203 |
| comorbidity | gi_dysmotility | 174 | 26 | 176 |
| comorbidity | arrhythmia_differential | 105 | 71 | 138 |
| comorbidity | post_vaccination_onset | 114 | 54 | 115 |
| comorbidity | mast_cell_activation | 100 | 26 | 104 |
| comorbidity | small_fiber_neuropathy_dx | 66 | 19 | 68 |
| comorbidity | fibromyalgia | 65 | 16 | 66 |
| comorbidity | surgery_onset | 66 | 0 | 66 |
| comorbidity | systemic_autoimmune_disease | 44 | 28 | 65 |
| comorbidity | hormonal_transition_onset | 49 | 31 | 58 |
| comorbidity | sleep_disturbance | 40 | 15 | 45 |
| comorbidity | secondary_sinus_tachycardia | 40 | 14 | 44 |
| comorbidity | structural_cardiopulmonary_disease | 33 | 12 | 42 |
| comorbidity | physical_trauma_onset | 21 | 9 | 22 |
| comorbidity | medication_induced_tachycardia | 17 | 5 | 18 |
| comorbidity | neurodevelopmental_disorder | 17 | 4 | 18 |
| comorbidity | prolonged_bed_rest | 9 | 11 | 15 |
| comorbidity | endometriosis | 7 | 2 | 7 |
| comorbidity | functional_neurological_disorder | 6 | 1 | 7 |
| comorbidity | iron_deficiency | 5 | 2 | 5 |
| comorbidity | adolescent_growth_spurt | 2 | 1 | 3 |
| measurement | heart_rate_variability | 224 | 561 | 655 |
| measurement | head_up_tilt_test | 466 | 292 | 552 |
| measurement | active_stand_test | 127 | 274 | 386 |
| measurement | valsalva_maneuver | 98 | 317 | 377 |
| measurement | cerebral_blood_flow_measurement | 131 | 50 | 138 |
| measurement | genetic_testing | 111 | 13 | 112 |
| measurement | plasma_catecholamine_assay | 55 | 72 | 99 |
| measurement | plasma_blood_volume_assay | 75 | 34 | 85 |
| measurement | muscle_sympathetic_nerve_activity | 21 | 71 | 77 |
| measurement | cardiopulmonary_exercise_test | 27 | 49 | 68 |
| measurement | ambulatory_monitoring | 50 | 27 | 60 |
| measurement | regional_norepinephrine_spillover | 4 | 51 | 52 |
| measurement | end_tidal_co2 | 41 | 10 | 43 |
| measurement | skin_biopsy_ienfd | 26 | 28 | 41 |
| measurement | autonomic_autoantibody_assay | 12 | 33 | 38 |
| measurement | renin_aldosterone_assay | 31 | 15 | 35 |
| measurement | qsart_sudomotor_testing | 26 | 8 | 32 |
| measurement | cardiac_mri | 10 | 6 | 16 |
| measurement | urine_sodium_24h | 13 | 4 | 16 |
| measurement | thermoregulatory_sweat_test | 10 | 5 | 12 |
| measurement | quantitative_sensory_testing | 5 | 0 | 5 |
| mechanism | neuropathic_evidence | 235 | 169 | 344 |
| mechanism | postinfectious_evidence | 334 | 157 | 338 |
| mechanism | hyperadrenergic_evidence | 226 | 139 | 277 |
| mechanism | cerebral_hypoperfusion_evidence | 181 | 73 | 209 |
| mechanism | autoimmune_evidence | 186 | 53 | 190 |
| mechanism | hypovolemic_evidence | 177 | 49 | 188 |
| mechanism | deconditioning_evidence | 105 | 42 | 129 |
| treatment | exercise_reconditioning | 141 | 81 | 186 |
| treatment | beta_blocker_therapy | 131 | 67 | 152 |
| treatment | ivabradine_therapy | 102 | 34 | 107 |
| treatment | volume_expansion_therapy | 62 | 50 | 93 |
| treatment | compression_therapy | 66 | 5 | 66 |
| treatment | midodrine_therapy | 54 | 27 | 61 |
| treatment | lifestyle_modification | 51 | 3 | 52 |
| treatment | immunomodulatory_therapy | 42 | 17 | 46 |
| treatment | physical_countermaneuvers | 8 | 30 | 37 |
| treatment | pyridostigmine_therapy | 22 | 10 | 25 |
| treatment | central_sympatholytic_therapy | 10 | 14 | 19 |
| treatment | auricular_vagus_nerve_stimulation | 16 | 7 | 18 |
| treatment | serotonergic_therapy | 14 | 6 | 16 |
| treatment | antihistamine_therapy | 8 | 4 | 10 |
| treatment | phenobarbital_therapy | 7 | 2 | 7 |
| treatment | droxidopa_therapy | 5 | 2 | 5 |
| treatment | prokinetic_therapy | 3 | 1 | 4 |
| treatment | offending_medication_withdrawal | 1 | 1 | 2 |

## Mechanism flag co-occurrence

Number of the six mechanism flags carried per article. Angeli et al. found overlapping rather than exclusive phenotypes in patients; the literature shows the same shape.

| n_mechanism_flags | n_articles |
| --- | --- |
| 0 | 1525 |
| 1 | 761 |
| 2 | 273 |
| 3 | 80 |
| 4 | 20 |
| 5 | 6 |
| 6 | 3 |

## Declared MeSH descriptors

| status | n_descriptors |
| --- | --- |
| real descriptor, used in this corpus | 195 |
| real descriptor, unused in this corpus | 29 |
| not recognised by PubMed | 0 |

Valid descriptors that no record in this corpus carries. Not errors: they mean no POTS paper is indexed that way, which is itself worth knowing when choosing between a MeSH-based and a text-based phenotype definition.

| facet_id | descriptor_name | n_in_all_of_pubmed |
| --- | --- | --- |
| antihistamine_therapy | Cromolyn Sodium | 4152 |
| cardiac_mri | Magnetic Resonance Imaging, Cine | 11725 |
| exercise_reconditioning | Rehabilitation | 402207 |
| gi_dysmotility | Enteric Nervous System | 8113 |
| hormonal_transition_onset | Menarche | 5880 |
| hormonal_transition_onset | Puberty | 20315 |
| hypermobility_heds | Connective Tissue Diseases | 377315 |
| hypovolemic_evidence | Plasma Volume | 5243 |
| immunomodulatory_therapy | Immunomodulating Agents | 1328 |
| immunomodulatory_therapy | Rituximab | 21720 |
| medication_induced_tachycardia | Adrenergic alpha-1 Receptor Antagonists | 2691 |
| medication_induced_tachycardia | Diuretics | 39518 |
| neurodevelopmental_disorder | Neurodevelopmental Disorders | 236292 |
| offending_medication_withdrawal | Deprescriptions | 1644 |
| physical_trauma_onset | Brain Injuries, Traumatic | 32480 |
| plasma_blood_volume_assay | Plasma Volume | 5243 |
| plasma_catecholamine_assay | Metanephrine | 976 |
| prokinetic_therapy | Domperidone | 1959 |
| prokinetic_therapy | Metoclopramide | 5101 |
| quantitative_sensory_testing | Sensory Thresholds | 61703 |
| quantitative_sensory_testing | Thermosensing | 3436 |
| secondary_sinus_tachycardia | Adrenal Insufficiency | 14426 |
| secondary_sinus_tachycardia | Anorexia Nervosa | 15751 |
| serotonergic_therapy | Serotonin and Noradrenaline Reuptake Inhibitors | 708 |
| skin_biopsy_ienfd | Ubiquitin Thiolesterase | 6742 |
| surgery_onset | Postoperative Period | 64660 |
| surgery_onset | Surgical Procedures, Operative | 3867600 |
| systemic_autoimmune_disease | Thyroiditis, Autoimmune | 12604 |
| urine_sodium_24h | Natriuresis | 9241 |

## Ontology graph

| predicate | n_edges | transitive | subsumption |
| --- | --- | --- | --- |
| associated_with | 108 | no | no |
| is_a | 102 | yes | yes |
| evidence_for | 69 | no | no |
| proposed_mechanism_of | 59 | no | no |
| indicates | 33 | no | no |
| treats | 27 | no | no |
| trigger_of | 19 | no | no |
| differential_diagnosis_of | 8 | no | no |
| part_of | 4 | yes | no |
| measures | 2 | no | no |

Nodes: 110. Edges: 431. Closure rows: 271.

## OMOP concept resolution

| resolver | status | n_attempts |
| --- | --- | --- |
| athena_api | error | 1 |
| nlm_clinical_tables | ok | 24 |
| ols4 | no_match | 9 |
| ols4 | ok | 82 |
| rxnav | ok | 18 |
| vocabulary_id | n_candidates | n_facets | n_with_concept_id |
| --- | --- | --- | --- |
| SNOMED | 208 | 54 | 0 |
| LOINC | 68 | 17 | 0 |
| RxNorm | 32 | 12 | 0 |

### Match quality of candidate mappings

Token-based: `exact` means the same set of meaningful words once case, punctuation, SNOMED's semantic tag, LOINC's unit brackets and British spellings are normalised away. `contains` means every word of the search term appears in the concept name, usually a more specifically named form of the same thing. `loose` means at least one word is missing, so the service matched on something else.

| label_match | n_candidates | n_rank_1 |
| --- | --- | --- |
| contains | 190 | 40 |
| exact | 89 | 78 |
| loose | 29 | 6 |

6 best-candidate rows are loose matches and should be reviewed first. They are exported to `data/derived/mapping_review_priority.tsv`.

| facet_id | vocabulary | search_term | concept_code | concept_name |
| --- | --- | --- | --- | --- |
| functional_neurological_disorder | SNOMED | functional neurological disorder | 735541006 | Dissociative neurological symptom disorder (disorder) |
| gi_dysmotility | SNOMED | intestinal dysmotility | 253768006 | Congenital dysmotility of small intestine |
| immunomodulatory_therapy | RxNorm | immune globulin | 1426680 | immunoglobulin G, human |
| medication_induced_tachycardia | SNOMED | adverse reaction to drug | 62014003 | Adverse reaction caused by drug (disorder) |
| plasma_catecholamine_assay | LOINC | epinephrine plasma | 2056-0 | Catecholamines [Mass/volume] in Plasma |
| quantitative_sensory_testing | SNOMED | quantitative sensory testing | 252775003 | Quantitative sensory test |

308 candidate mappings are `unreviewed`. None of them should be treated as a phenotype definition until a human has checked them against ATHENA.

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
