# Talking cards — SOURCED background

Same content as `TALKING_CARDS.md`, with **every claim tagged to its source**.

**Source key.** `[T·<Ch> §<Section>]` = this thesis (Ch = Intro · BG · RW · SD · Eval · Disc). External papers **as actually cited in the thesis** (verified by grep against the `.tex`): `[Sedlmair+ 2012]`, `[Hyvärinen & Oja 2000]`, `[Hendrickx+ 2024]`, `[Dempster+ 2021]` (MiniRocket), `[McInnes+ 2020]` (UMAP), `[Larson+ 2025]` (MNE-Python — the thesis's MNE cite), `[Pion-Tonachini+ 2019]` (ICLabel), `[Winkler+ 2011]` (MARA), `[Mognon+ 2011]` (ADJUST), `[Tandle; Fickling]` (artefact taxonomy), `[Welch 1967]` (PSD).
Note: `[Gramfort+ 2013]` is the canonical MNE paper, used **only** for the external before/after overlay figure I added (not in the thesis).

---

### 2 · Design Study Methodology
- Problem-driven research: work with experts on a real problem and build a tool, contrary to technique-driven work. `[T·BG §Design Study Methodology]` `[Sedlmair+ 2012]`
- Nine stages, three phases (Precondition: learn/winnow/cast; Core: discover/design/implement/deploy; Analysis: reflect/write). `[T·BG §Design Study Methodology]` `[Sedlmair+ 2012]`
- Real users + real data mandatory; validation woven through. `[Sedlmair+ 2012]`
- Contribution may be a well-justified combination of existing techniques, not a new algorithm. `[Sedlmair+ 2012]`; thesis frames itself this way `[T·Conclusion]`
- Months of collaboration with Leitner & Wriessnegger (Institute of Neural Engineering): meetings, prototype reviews, interviews. `[T·SD §System Design intro]`

---

### 3 · Electroencephalography (EEG)
- Measures post-synaptic neuronal potentials at the scalp, millisecond resolution. `[T·BG §Electroencephalography]`
- Non-invasive, portable, low-cost → core in neuroscience & clinical neurophysiology. `[T·BG §Electroencephalography]`
- Used for epilepsy, sleep, BCI, cognitive studies. `[T·BG §Electroencephalography]`
- Standardised 10–10 electrode system; 32-channel subset here. `[T·BG §Electroencephalography]`
- Raw EEG unusable as-is; needs preprocessing (filter, bad-channel, ICA, regression, segmentation, resampling); conclusion quality ∝ data quality. `[T·BG §Electroencephalography]` `[T·Intro §Motivation]`
- Figures: 10–10 schematic `[T·BG fig. sensor placement]`; cap photo `[Chris Hope, CC BY 2.0, Wikimedia]` (external).

---

### 4 · EEG Artefacts
- Artefact = any recorded potential not from the brain; amplitudes can exceed the brain activity itself. `[T·BG §Common artefacts in EEG]`
- From body (eyes/muscle/heart) + external. `[T·BG §Common artefacts in EEG]`
- Overlap brain in time & frequency → ocular mimics slow brain waves, muscle spans all bands → not a simple filtering step. `[T·Intro §Problem Statement]` `[T·BG §Ocular]` `[T·BG §Muscle]`
- Unhandled artefacts → incorrect conclusions. `[T·BG §Common artefacts in EEG]`
- Figure: three frontal eye-blinks dominating the trace. `[T·Intro fig. raw EEG artefact]`

---

### 5 · Artefact Types
**Physiological** `[T·BG §Physiological Artefacts]`
- **Ocular** — eye movements/blinks; eyes' proximity to brain → most common & impactful; large-amplitude, slow, low-frequency, frontal; measured by EOG. `[T·BG §Ocular]`
- **Cardiac** — heartbeat; rhythmic sharp spikes mirroring the ECG; stronger over the left temporal region. `[T·BG §Cardiac]`
- **Muscle (EMG)** — head/neck contractions; random, non-repetitive; below 1 Hz to over 200 Hz → interfere with all bands; recorded by all sensors; hardest to remove. `[T·BG §Muscle]`

**Technical** `[T·BG §Technical Artefacts]`
- **Line noise** — 50 Hz mains, constant on all channels; notch filter. `[T·BG §Technical]`
- **Electrode/channel** — poor contact/dry gel → jumps, flat lines, single-electrode noise. `[T·BG §Technical]`
- **Movement** — cable/electrode shifts → sudden deflections, common in mobile EEG. `[T·BG §Technical]`
- **Environmental** — nearby devices (lights, monitors, phones). `[T·BG §Technical]`
- Line noise removed in preprocessing; physiological ones separated by ICA; muscle flagged later by a badge. `[T·BG §Technical]` `[T·SD §Recommender Bar]`

---

### 6 · Independent Component Analysis
- Unmixes the recorded mixture into statistically independent components, one per source. `[T·BG §ICA]` `[Hyvärinen & Oja 2000]`
- Cocktail-party analogy; electrodes = microphones recording mixtures. `[T·BG §ICA]`
- X = A·S; estimate W ≈ A⁻¹ so Ŝ = W·X; works by maximising non-Gaussianity (FastICA). `[T·BG §ICA]` `[Hyvärinen & Oja 2000]`
- Each component = a topography (mixing-matrix column), a time series, and a power spectrum. `[T·BG §ICA]`
- #components ≤ #electrodes; default 16. `[T·BG §ICA]` `[T·SD §Data Processing Pipeline]`

---

### 7 · Manual Component Classification
- Components are unlabeled; expert judges each keep (brain) / reject (artefact) from the three properties. `[T·BG §ICA]` `[T·Intro §Problem Statement]`
- Back-projection rebuilds the clean signal from kept components only. `[T·BG §Back-Projection]`
- Irreversible, asymmetric cost: wrong reject deletes real signal; wrong keep leaves noise → keep the human in control. `[T·BG §Back-Projection]`
- Bottleneck: hundreds of participants × ~16 components → rushed/inconsistent decisions; reasoning never recorded. `[T·Intro §Motivation]` `[T·Intro §Problem Statement]`
- Figure: MNE `ica.plot_overlay` before(red)/after(black). **External figure**, cited on slide as `[Gramfort+ 2013]`, BSD-3.

---

### 8 · Existing Tools
- MNE: richest primitives, `plot_properties` (topo+spectrum+epochs), per-artefact detectors, but no unified classifier / no calibrated confidence. `[T·RW §MNE-Python]` `[Larson+ 2025]`
- FieldTrip: MATLAB; ICA classification is script-driven/manual; no automated step, no cross-recording view. `[T·RW §FieldTrip]`
- EEGLAB: GUI, topographic grid + per-component popups; cross-subject clustering needs manual setup; classifier = ICLabel plugin. `[T·RW §EEGLAB]`
- ICLabel (CNN, 7-class), MARA (linear, posterior prob.), ADJUST (threshold rules) — one-shot, no interactive UI, fixed outputs. `[T·RW §Automated ICA Component Classifiers]` `[Pion-Tonachini+ 2019]` `[Winkler+ 2011]` `[Mognon+ 2011]`
- Capability table: no tool combines linked views + cross-participant browser + batch-apply + calibrated confidence → build the missing pieces on the same primitives. `[T·RW §ICA Classification Workflows, Table]`

---

### 9 · Research Questions
- RQ1 (techniques) and RQ2 (integrate ML, preserve control) — verbatim. `[T·Intro §Research Goal and Contributions]`

---

### 10 · Requirements
- Six requirements from the discovery interviews, three groups. `[T·SD §Requirements]`
- R1–R6 wording (overview & guidance / inspection / workflow). `[T·SD §Requirements]`

---

### 11 · System Architecture
- Two Docker containers (Compose): Flask backend + React/nginx frontend, REST API. `[T·SD §Technology Stack]`
- Backend: MNE-Python, scikit-learn, aeon/MiniRocket, umap-learn; XDF via pyxdf; NumPy. `[T·SD §Technology Stack]`
- Frontend renders on HTML canvas. `[T·SD §Technology Stack]`
- Defaults (50/100 Hz notch, 1–75 Hz IIR, 250 Hz, FastICA 16) configurable per study. `[T·SD §Data Processing Pipeline]`

---

### 12 · Classification Workflow
- (A) create study + preprocess; (B) apply Tier 1 in bulk; (C) inspect uncertain; (D) control clusters; export; optional retrain. `[T·SD §Overall Classification Workflow]`

---

### 13 · Confidence Recommender
- Keep-probability per component (1 = brain, 0 = artefact). `[T·SD §Recommender Bar]` `[T·SD §Machine Learning Pipeline]`
- Three tiers / five colour bands, symmetric around 0.5: Tier 1 ≥0.75 or ≤0.25 (batch); Tier 2 0.60–0.75 / 0.25–0.40 (quick review); Tier 3 0.40–0.60 (manual). `[T·SD §Recommender Bar]` `[T·SD §Machine Learning Pipeline]`
- "Classification with a reject option" — defer uncertain cases to the human. `[T·SD §Recommender Bar]` `[Hendrickx+ 2024]`
- Separate muscle badge from MNE's detector: faded orange 50–70%, solid >70%. `[T·SD §Recommender Bar]` `[Larson+ 2025]`
- Indicator only, never pre-set (changed after the evaluation). `[T·Eval §Design Revision]`
- ~40% of components land in Tier 1. `[T·Disc §RQ2]`

---

### 14 · Participants and Components Overview
- Left: participants with kept/rejected/undecided totals; right: components with confidence bar, tier button, muscle badge. `[T·SD §Visual Analytics Design (component-selection)]`
- Apply action: rejects undecided muscle-badge components first, then applies Tier 1; never overwrites manual labels; reset available. `[T·SD §Recommender Bar]`
- Click a row → opens the inspector. `[T·SD §Visual Analytics Design]`

---

### 15 · Component Inspector
- Central interface: all coordinated views for one component on one screen. `[T·SD §Visual Analytics Design]`
- Switching component (row / cluster point / arrow keys) keeps the inspector open and updates the linked views together. `[T·SD §Visual Analytics Design]` `[T·SD §Component Info and Settings]`
- Script workflow lacks a simultaneously-updating coordinated view. `[T·Eval §Comparison with MNE-Python Workflow]`
- Quote "the entire visualisation on one screen, more or less." `[T·Eval §Comparison with MNE-Python Workflow]`

---

### 16 · Component Cluster and Mapper Graph
- Both project every component from every participant into a shared 2D space (UMAP on MiniRocket descriptors, cosine distance). `[T·SD §Component Cluster and Mapper Graph]` `[T·SD §Embedding Pipeline]` `[McInnes+ 2020]` `[Dempster+ 2021]`
- Cluster: one dot per component, coloured by label or by confidence; similar components close. `[T·SD §Component Cluster]`
- Mapper Graph: DBSCAN groups nearby components into pie-chart nodes (sized by count) + proximity edges. `[T·SD §Mapper Graph]` `[T·SD §Cluster Mapping Pipeline]`
- Best use = outlier detection (off-colour dot = likely misclassification); overview-first → drill down. `[T·SD §Component Cluster]` `[T·SD §Interaction Between Views]`

---

### 17 · Topomap
- Each sensor's contribution + polarity, from the ICA mixing matrix, in µV; interpolated; blue→white→red. `[T·SD §Topomap]`
- Dipolar = brain · frontal = ocular · peripheral patch = muscle · single electrode = bad channel. `[T·SD §Topomap]` `[Winkler+ 2011]`
- Hover = electrode name + amplitude; updates to a selected time range. `[T·SD §Topomap]`
- First primary criterion. `[T·Eval §Expert Classification Process]`

---

### 18 · Power Spectral Density
- Power across frequency, five EEG bands shaded; computed with Welch's method. `[T·SD §Power Spectral Density]` `[Welch 1967]`
- 1/f falling (often alpha peak) = brain · flat/rising = muscle · delta/theta = ocular. `[T·SD §Power Spectral Density]` `[Winkler+ 2011]`
- Updates to a selected timeline segment. `[T·SD §Power Spectral Density]`
- Second primary criterion. `[T·Eval §Expert Classification Process]`

---

### 19 · Timeline
- Component signal over the whole recording; scroll = time zoom, drag = pan, y-axis = amplitude zoom. `[T·SD §Timeline]`
- Right-click-drag selects a range → updates topomap + PSD. `[T·SD §Timeline]`
- Overlays: EOG markers (blink events via MNE on Fp1), trial markers (XDF marker stream, blue bands), PSD clusters. `[T·SD §Component Info and Settings: EOG/Trial/PSD]` `[Larson+ 2025]`
- Confirmatory; eye artefacts = large slow deflections. `[T·Eval §Findings: Timeline]`

---

### 20 · PSD Clustering
- K-means on the PSD of overlapping windows across the participant's components; colour-codes timeline segments. `[T·SD §PSD Clusters]`
- Configurable cluster count, window size, step; Euclidean distance on per-window PSD vectors. `[T·SD §PSD Clusters]`
- Shows where a component's frequency profile changes; hover = cluster mean PSD; figure = All Clusters modal. `[T·SD §PSD Clusters]`

---

### 21 · Evaluation
- Qualitative deploy-stage; two experts (E1 Leitner interviewed, E2 Wriessnegger indirect); real BCI recordings; think-aloud + semi-structured interview. `[T·Eval §Expert Evaluation]` `[T·Eval §Participants]`
- Five tasks (recommendations, artefact identification, band-power analysis, mapper exploration, view comparison). `[T·Eval §Procedure: Task]`
- Consistent decision sequence (index → topomap → PSD → timeline) matches the layout. `[T·Eval §Expert Classification Process]`
- Recommender trusted on ocular; muscle checked manually. `[T·Eval §Findings: Recommendations]`
- Cross-participant overview caught a mislabeled component → imperfect classifier + expert inspection corrects errors the model alone would miss. `[T·Abstract]` `[T·Eval §Findings: Component Overview]`
- Close — "value is the integration". `[T·Conclusion]`

---

### B · Backups
- **Recommender Pipeline:** MiniRocket (9,996) + 16 unmixing-matrix weights → Random Forest → keep-probability; ~75% test, 480 components / 30 participants. `[T·SD §Machine Learning Pipeline]` `[T·Eval §Classification Pipeline]` `[Dempster+ 2021]`
  - Deployed = MiniRocket 75%; Matrix Profile 79% but needs a self-join. `[T·Eval §Classification Pipeline / Matrix Profile]`
  - On ICLabel: it does output probabilities and is a strong baseline → defend the custom classifier with **retrainability on the team's labels**, not "no tool has probabilities." `[T·RW §Automated ICA Component Classifiers]` `[Pion-Tonachini+ 2019]`
- **Classification Experiments:** nine-experiment table; MiniRocket chosen for speed. `[T·Eval §Classification Pipeline]`
- **Data Flow / Export:** YAML → `ica.exclude` → `ica.apply()`; metadata preserved; labels can retrain. `[T·SD §Export and Integration]`

---

### Limitations (Q&A only — sourced)
- 480 components, one study, one expert's labels; ~75% → ~1 in 4 wrong (fine with review). `[T·Disc §Limitations]`
- Single paradigm (BCI), generalisation untested. `[T·Disc §Limitations]`
- No quantitative task-time comparison. `[T·Eval §Limitations of the Evaluation Method]`
- No per-decision explanation; red/green not colour-blind safe. `[T·Disc §Limitations]`
- Frame "combination beats sum" as a *proposed* result from a small qualitative study. `[T·Eval §Limitations]` `[Sedlmair+ 2012 — "liking ≠ validation"]`
