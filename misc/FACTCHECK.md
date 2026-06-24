# Fact-check: `presentation.html` vs the thesis

Every slide's text and image checked against the thesis source (`~/Downloads/thesis_extract/*.tex` + figures).

**Legend:** ✅ faithful to thesis · ⚠️ paraphrase/inference — defensible but not verbatim · ❌ not in the thesis · 🔧 was wrong, now fixed.

**Headline finding:** one outright fabrication existed (slide 5 gap table — invented an ICARUS column, invented rows, invented values). **🔧 now replaced with the thesis's actual Table 2.1, values verified cell-by-cell.** Everything else traces to the thesis; remaining issues are minor paraphrase/inference (⚠️) or external additions in two backups (B4), all listed at the bottom.

---

## Main slides

### S1 — Title
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Title | "A Design Study on Visual Analytics for EEG Artefact Removal" | `thesis.tex`: `\thesistitle{...for EEG Artefact Removal}` | ✅ |
| Author | "Niklas Tscheppe, BSc" | `\thesisauthor[Niklas Tscheppe]{Niklas Tscheppe, BSc}` | ✅ |
| Supervisors | "J. Rakuschek, T. Schreck" | Rakuschek, Julian + Tobias Schreck | ✅ |
| Date | "[DATE]" | — | ⚠️ placeholder, fill in |

### S2 — Motivation (inseparability)
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "EEG is contaminated by artefacts that overlap brain activity in time and frequency." | RelWork: muscle artefacts "can range from below 1 Hz to over 200 Hz, they can interfere with all of the brain's frequency bands"; ocular "produce slow, low-frequency signals that can look like genuine slow brain activity" | ✅ (overlap supported, esp. muscle/ocular) |
| Caption | "Overlapping spectra, spread across channels — removing them is not a simple filtering step." | Intro: "Removing these artefacts is not a simple filtering step." Muscle "often recorded by all of the EEG sensors." | ✅ 🔧 (reworded to thesis phrasing) |
| Image | `raw_eeg_artefact.png` | Intro Fig "Eye-blink artefacts in a raw EEG trace" | ✅ same figure, same context |

### S2b — Artefact types (added)
| Element | Slide | Thesis (`related_work.tex` §Common artefacts) | Verdict |
|---|---|---|---|
| Headline | "Artefacts are physiological (from the body) or technical (from the environment)." | "classified into two main categories, physiological and external" | ✅ |
| Physiological list | ocular (slow/low-freq/frontal), cardiac (rhythmic QRS), muscle (~1–200 Hz, all sensors, hardest) | ocular "slow, low-frequency… frontal"; cardiac "rhythmic… sharp spikes… left temporal"; muscle "below 1 Hz to over 200 Hz… recorded by all of the EEG sensors… hardest to classify" | ✅ |
| Technical list | line noise 50 Hz all channels (notch); electrode/movement noise | "power line interference at 50 Hz… removed with a notch filter"; electrode/movement artefacts | ✅ |
| Footer | "Technical artefacts filtered in preprocessing; physiological ones are what ICA + the expert separate" | "line noise are removed during preprocessing before ICA" | ✅ |
| Image | `jiang_artefact_overview.jpg` + credit "Jiang et al. 2019, CC BY 4.0" | RelWork Fig, credited "© 2019 Jiang et al… CC-BY 4.0" | ✅ (credit retained) |

### S3 — ICA
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "ICA unmixes the recording into independent source components." | ICA §: "decomposes the mixed EEG signal into statistically independent components" | ✅ |
| Math | "X = A·S → Ŝ = W·X" | ICA §: "X = A · S"; toy caption: "recovers three independent components $\hat{S} = W X$" | ✅ (incl. the hat notation) |
| Body | "Each component carries a topography, a spectrum, and a time course." | ICA §: "each with three properties... a time series..., a topographic map..., and a power spectrum" | ✅ |
| Image | `ica_toy_example.png` | RelWork Fig "Cocktail-party demonstration of ICA" | ✅ same figure |

### S4 — Expert labels + irreversible
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "ICA does not label the components — an expert classifies each, and the removal is irreversible." | "Each component is then classified by an expert as either brain activity (keep) or an artefact (reject)"; back-projection "is irreversible" | ✅ |
| Bullet: keep/reject | "Each component judged keep (brain) or reject (artefact)" | verbatim concept | ✅ |
| Bullet: back-projection | "Rejected components are back-projected out" | "Rejected components are removed via back-projection" | ✅ |
| Bullet: stakes | "A wrong reject deletes real brain signal; a wrong keep leaves noise" | "A brain component mistakenly rejected removes real neural signal; an artefact component mistakenly kept leaves noise in the data." | ✅ near-verbatim |
| Bullet: scale | "Manual, per-component, hundreds per study" | "studies involving hundreds of participants, the total number of components to review grows quickly" | ✅ |
| Image | none — conceptual flow (unlabeled components → keep/reject → back-project → cleaned signal) + stakes | concepts from §ICA / §Back-Projection | ✅ 🔧 (removed the MNE screenshot — moved to backup B8 in its proper "current workflow" context) |
| Flow + stakes text | "Irreversible: a wrong reject deletes real brain signal; a wrong keep leaves noise" / "Manual, per-component, hundreds of components per study" | §Back-Projection (verbatim concept) + Intro (scale) | ✅ |

### S5 — Related-work gap 🔧
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "Existing tools cover parts of the review, but none combine coordinated views, a cross-participant overview, and ML assistance." | Abstract: "Existing EEG tools cover parts of this review... but none combine coordinated detail views, a cross-participant overview, and machine learning assistance in a single workflow." | ✅ near-verbatim |
| Table | 6 tools (MNE-Py, FieldTrip, EEGLAB, ICLabel, MARA, ADJUST) × 11 rows, 2 sections, footnotes a/b | RelWork Table "ICA classification capabilities" | ✅ **all 66 cells verified against the thesis table** (was fabricated; 🔧 replaced) |
| Legend | "● supported · ◐ partial · ○ absent in core · — out of scope" | thesis: "\CIRCLE supported; \Circle absent in core; \LEFTcircle partial; --- out of scope by design" | ✅ |

**Verified cells (slide ↔ thesis):** ICA decomp ●●●——— ✅ · Power spectrum ●●●——— ✅ · Algorithmic decision ◐ᵃ○○ᵇ●●● ✅ · Confidence score ◐ᵃ○○ᵇ●●○ ✅ · Cross-subject matching ●○◐——— ✅ · Single-component view ●◐◐——◐ ✅ · Linked view updates ○○○——— ✅ · Cross-participant browser ○○◐——— ✅ · Component clustering view ○○◐——— ✅ · Batch apply ○○○ᵇ——— ✅ · Label export ●◐◐●●● ✅

### S6 — Research questions
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| RQ1 | "Which visualisation & interaction techniques support expert decision-making in ICA artefact classification?" | Intro: identical | ✅ verbatim |
| RQ2 | "How can machine learning recommendations and interactive visualisation techniques be integrated to accelerate classification while preserving expert control?" | Intro: identical | ✅ 🔧 verbatim |

### S7 — Requirements
| Element | Slide | Thesis (`system_design.tex` §Requirements) | Verdict |
|---|---|---|---|
| Grouping | Overview & guidance / Component inspection / Workflow | "grouped into three categories: overview and guidance, component inspection, and workflow support" | ✅ |
| R1 | "visual keep/reject recommendations" | "receiving visual recommendations on which components to keep or reject" | ✅ condensed |
| R2 | "overview across all components" | "gaining an overview of all components and supporting exploration, pattern recognition…" | ✅ condensed |
| R3 | "when artefacts appear over time" | "inspecting when and how artefacts appear in the recording over time" | ✅ |
| R4 | "distinguish brain from artefact" | "visually distinguishing brain activity from artefacts" | ✅ |
| R5 | "distribution across the scalp" | "seeing how a component's activity is distributed across the scalp sensors" | ✅ |
| R6 | "log & export decisions" | "logging and exporting classification decisions for reproducibility and team consistency" | ✅ condensed |
| Footer | "Months of collaboration… Institute of Neural Engineering" | "collaboration with Michael Leitner and Selina Wriessnegger from the Institute of Neural Engineering… over several months in 2025" | ✅ |

### S8 — Workflow
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "ICARUS supports the full keep/reject workflow in one tool." | Fig caption: "Classification workflow supported by ICARUS, with four main parts: preprocessing (A), applying recommendations (B), inspecting components (C), and controlling clusters (D)" | ✅ |
| Image | `workflow.png` | same figure | ✅ |

### S9 — Inspector
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "Selecting a component updates all linked detail views at once." | "only updates the linked detail views (Topomap, PSD, Timeline) to show the newly selected component" | ✅ |
| Image | `inspector_overview.png` | Fig "component inspector with coordinated views" | ✅ (placeholder for demo video) |

### S10 — Tiers
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "sorts components into three confidence tiers" | "maps each probability to one of three confidence tiers" | ✅ |
| Tier labels | "Tier 1 high-confidence · Tier 2 quick review · Tier 3 manual inspection" | "Tier 1 (automatic decision)… Tier 2 (quick review)… Tier 3 (manual inspection)" | ✅ |
| Body | "Shown as an indicator — the expert confirms every decision" | design revision: "Recommendations are now shown as visual indicators only… no longer pre-set"; eval: "wanted to confirm each decision explicitly" | ✅ |
| Image | `tier_tooltips.png` | Fig "Recommender Bar tooltip per tier" | ✅ |

### S11 — Cluster
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "arranges every component from every participant by similarity" | "each dot represents one ICA component from one participant… similar… end up close together" | ✅ |
| Body | "coloured by label" / "cross-participant view the per-recording workflow lacks" / "off-colour point invites a second look" | "labelled based on their decision status"; mne_comparison cross-participant gap; "a point whose colour differs from its surrounding cluster flags a likely misclassification" | ✅ |
| Image | `crop_umap.png` | Fig "Component Cluster view" | ✅ |

### S12 — Export
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "Decisions export as labels that re-enter the MNE-Python pipeline." | "stored as a YAML file per experiment… used directly in an MNE-Python script by assigning the indices to `ica.exclude` and calling `ica.apply()`" | ✅ |
| Body: metadata | "Preprocessing metadata preserved (reproducible)" | "preprocessing metadata… is preserved alongside the labels so that the full pipeline is reproducible (R6)" | ✅ |
| Body: retrain | "Accumulated labels can retrain the recommender" | "Accumulated labels can also be used to retrain the recommender" | ✅ |
| Image | `data_flow.png` | Fig "Overall data flow" | ✅ |

### S13 — ML pipeline
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "combines time-series and topography features in a Random Forest" | "Both feature sets are concatenated and used to train a Random Forest classifier with 100 trees" | ✅ |
| Body: features | "MiniRocket features + 16 topography weights → keep-probability → tiers" | "MiniRocket… 9,996-element feature vector… 16 spatial-filter weights" | ✅ |
| Body: data | "480 components / 30 participants" | "480 ICA components (30 participants × 16 components)" | ✅ |
| Body: accuracy | "~75% test accuracy" | Discussion: "the deployed pipeline uses MiniRocket features at 75%" | ✅ (correct number — the 79% is Exp 05, not deployed) |
| Body: ocular/muscle | "Strong on ocular; muscle is hard for any method" | eval: ocular "works really damn well"; "muscle… E1 did not trust"; RelWork "muscle… the hardest to classify reliably" | ✅ |
| Image | `ml_flowchart.png` | Fig "Recommender inference pipeline" | ✅ |

### S14 — Evaluation divider
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "Evaluated with two domain experts on their own EEG data" | "Two domain experts from the Institute of Neural Engineering… participated" | ✅ |
| Sub | "Think-aloud + semi-structured interview · real BCI recordings" | "semi-structured expert interview combined with a think-aloud protocol"; "real recordings from brain-computer interface experiments" | ✅ |

### S15 — Decision sequence
| Element | Slide | Thesis (`evaluation.tex` §Classification Process) | Verdict |
|---|---|---|---|
| Headline | "The expert's decision sequence matched the tool's layout — topography and spectrum first." | "This aligns with the system's layout, which gives the topomap and PSD the most space." | ✅ (singular "expert" correct — sequence is E1's; E2 didn't think-aloud) |
| Sequence | index → topography → spectrum → timeline → decide (~5 s) | "1. Component index… 2. Topographic map… 3. Power spectral density… 4. Timeline… 5. Decision… about five seconds per component" | ✅ |
| Quote | "The entire visualisation on one screen, more or less." | mne_comparison: E1 "the entire visualisation on one screen, more or less" | ✅ verbatim |

### S16 — Caught mislabeled component
| Element | Slide | Thesis | Verdict |
|---|---|---|---|
| Headline | "In the session, the cross-participant overview surfaced a mislabeled component." | Abstract: "the cross-participant overview let the expert catch a misclassified component on review"; eval: "such an outlier turned out to be wrongly classified" | ✅ |
| Body | "off-colour among its neighbours; on review, it had been misclassified" + "why the expert stays in the loop on a task hard to automate reliably" | "outlier detection… flags a likely misclassification"; abstract "pairing an imperfect classifier with expert inspection can correct errors the model alone would miss… reliable ICA classification remains a hard problem" | ✅ (note: n=1; thesis hedges — slide states the fact, no accuracy claim) |
| Image | `crop_umap.png` (**reused from S11**) | thesis has **no figure of the caught outlier** | ⚠️ reuses S11's figure; **not annotated** to show the specific outlier — TODO add circle/arrow, and it's illustrative, not the actual logged case |

### S17 — Design refinements
| Element | Slide | Thesis (§Design Revision / Findings) | Verdict |
|---|---|---|---|
| Headline | "Expert feedback in the session led to design refinements." | "After the evaluation session, the system was revised to address the feedback." | ✅ |
| Body | "Experts classify by spectral slope, not band values" / "changed to the continuous curve" | "experts wanted the raw PSD curve because they classify by spectral slope, not by discrete band values" | ✅ |
| Image | `psd_before_bandpower.png` → `psd_after_curve.png` | Fig "PSD view evolution" (before/after subfigures) | ✅ same figures |

### S18 — Contribution
| Element | Slide | Thesis (§Requirements Revisited / Conclusion) | Verdict |
|---|---|---|---|
| R3–R5 | "Coordinated inspection of topography, spectrum, time (R3–R5)" | "Coordinated views support detailed per-component inspection across R3, R4 and R5" | ✅ |
| R1 | "Confidence-tier recommender (R1)" | "Confidence-tier recommendations… cover R1" | ✅ |
| R2 | "Cross-participant overview (R2)" | "…and a cross-participant overview cover R1 and R2" | ✅ |
| R6 | "Reproducible label export (R6)" | "exported in a structured, reproducible format… satisfying R6" | ✅ |
| Message | "The value is the integration: ML handles the clear cases while the expert keeps control of the rest." | Conclusion: "the system works because its parts work together"; "accepting the high-confidence ones in one go… without giving up control over the uncertain ones" | ✅ |

---

## Backup slides

### B1 — Experiments table
All 9 rows × (CV / Test / Reject-recall) **verified against the thesis evaluation table:** 01 (—/77/58) · 02 (70/70/58) · 03 (56/48/52) · 04 (63/75/70) · 05 (67/**79**/76, best) · 06 (65/75/70, deployed) · 07 (67/72/58) · 08 (65–73/61–69/—) · 09a (—/70/—). ✅ all match. Caption "MiniRocket chosen… faster… no self-join" = thesis verbatim reasoning. ✅

### B2 — Why custom classifier
| Claim | Thesis | Verdict |
|---|---|---|
| "Retrainable on accumulated labels — Python-native" | "the ability to retrain on the team's accumulated labels"; "ICLabel, MARA, and ADJUST are fixed pretrained pipelines" | ✅ |
| "ICLabel is a strong baseline; integrating/fine-tuning it is sound future work" | thesis says ICLabel "outperforms or performs comparably"; does **not** call it future work | ⚠️ **my framing**, not the thesis's. Note says "do NOT claim no tool has probabilities" — this **softens the thesis's own justification** (the thesis leans on "calibrated probability… no existing tool provides," which is contestable since MARA/ICLabel output probabilities). Defensible coaching, but know it departs from the thesis wording |

### B3 — Evaluation scope
"Small panels typical (Sedlmair)" ✅ ("Small expert panels are typical for design studies"); "longer deployment, more raters, task-time vs MNE" ✅ (Future Work). ✅

### B4 — XAI
| Claim | Thesis | Verdict |
|---|---|---|
| "Topography weights tabular → SHAP/counterfactuals" | Future Work: SHAP + DiCE on the tabular topographic side | ✅ |
| "Time-series side is harder → adapt a tool such as XAIVIER" | Future Work: "adapting tools such as XAIVIER… could surface attributions" | ✅ 🔧 (replaced my external additions with the thesis's own wording) |
| "no per-decision explanation (stated limitation)" | §Limitations: "provides confidence scores but no explanation for individual classification decisions" | ✅ |

### B5 — Architecture
Stack (Flask · MNE · scikit-learn · aeon/MiniRocket · umap-learn · React/canvas · 2 Docker containers) ✅ verbatim from §Tech Stack. Defaults (50/100 Hz notch · 1–75 Hz IIR · 250 Hz · FastICA 16) ✅ verbatim from §Data Pipeline. ✅

### B6 — Limitations
Single study/paradigm ✅ · labels from one expert ✅ ("480 training labels were assigned by one of the two experts") · no quantitative comparison ✅ · muscle weakest + red/green not colour-blind safe ✅ (all in §Limitations). ✅

### B7 — Future work
Muscle / `find_bads_muscle` ✅ · multiple labs/raters ✅ · controlled task-time study vs MNE ✅ · project artefact events onto trial timeline ✅ (all in §Future Work). ✅

### B8 — Current MNE workflow
`mne_ica_properties.png` shown in its correct context = the MNE `plot_properties` per-component view / script baseline. Text from §Comparison with MNE-Python Workflow. ✅ (this is the proper home for the figure that was wrongly on S4)

---

## Action items (everything not a clean ✅)

1. ~~**S1** — fill in `[DATE]`.~~ 🔧 set to "25 June 2026"; consultants Leitner + Wriessnegger added (from thesis title page).
2. ~~**S2 caption** — "amplitude thresholds" inference.~~ 🔧 reworded to "not a simple filtering step."
3. ~~**S4 image** — MNE's view used as generic manual review.~~ 🔧 **resolved** — removed from S4 (now conceptual flow); MNE view moved to backup B8 in proper context.
4. ~~**S6 RQ2** — condensed paraphrase.~~ 🔧 restored verbatim.
5. **S16 image** — reuses S11's `crop_umap.png`, **not annotated**; thesis has no figure of the caught outlier. Annotate (circle/arrow) and treat as illustrative. *(open — manual)*
6. **B2** — framing softens the thesis's "calibrated probability" justification; know the departure. *(intentional coaching — left as-is)*
7. ~~**B4** — external XAI specifics.~~ 🔧 replaced with the thesis's own wording (SHAP/DiCE on topo, XAIVIER for time-series, "no per-decision explanation").

**No remaining fabrications.** Open item: **S16 annotation** (manual). B2 is intentional Q&A coaching.

---

## 25-June refactor — new/changed slides

### S4b — Cleaning payoff (NEW, external figure)
| Element | Slide | Source | Verdict |
|---|---|---|---|
| Headline | "Rejecting the artefact components recovers a clean signal." | concept = §Back-Projection ("cleaner data"); ICA removal is the thesis's method | ✅ illustrative |
| Image | `mne_ica_overlay_blink.png` (before red / after black) | **external** — MNE `ica.plot_overlay`, sample dataset, BSD-3-Clause | ⚠️ **not a thesis figure**; MNE doc illustration, **footnote-cited** (Gramfort et al. 2013). Same BSD-3 provenance as the thesis's `mne_ica_properties`. Say "MNE sample data, illustrative" if asked |

### S2 — Design study / frame (moved to slide 2 per requested order)
| Element | Slide | Thesis (§Design Study Methodology) | Verdict |
|---|---|---|---|
| Headline | "A design study solves a real problem with domain experts — it need not invent a new technique." | "problem-driven research… contrary to technique-driven research" | ✅ |
| Body | problem-driven; nine stages / three phases (Precondition→Core→Analysis); "combination of existing techniques" | "nine-stage framework grouped into three main phases…"; Conclusion "works because its parts work together… not because any single part is novel" | ✅ |
| Image | `sedlmairDesignStudyMethodology2012.png` | thesis figure, "© 2012 IEEE", footnote-cited | ✅ |

### Figure footnote citations
- **S2b Jiang** — Sensors 19(5):987, 2019, CC BY 4.0 ✅ (thesis `jiangRemovalArtifactsEEG2019`)
- **S4b MNE** — Gramfort et al., Front. Neurosci. 7:267, 2013 ✅ (canonical MNE cite)
- **S6b Sedlmair** — IEEE TVCG 18(12):2431–2440, 2012 ✅
