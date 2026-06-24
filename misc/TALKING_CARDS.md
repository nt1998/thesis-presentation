# Talking cards — ICARUS defense

One card per slide. ▶ opener · ● what to say (enough to talk ~1 min) · ⛔ not here (deferred) · ⏱ time.
Sourced version with citations: `TALKING_CARDS_SOURCED.md`.

---

### 1 · Title  ⏱ 0:00
**▶ SAY:** "Good morning. My thesis is a design study on visual analytics for EEG artefact removal — a tool called ICARUS, built with EEG experts here at TU Graz."
- Reset timer, breathe, look up.

---

### 2 · Design Study Methodology  ⏱ ~1:00
**▶ "First, how I approached this."**
- A **design study** is *problem-driven* visualisation research: you work with real domain experts on a real problem and build a tool for it — the opposite of *technique-driven* work that invents an algorithm in isolation.
- It runs over a **nine-stage framework** in three phases: *Precondition* (learn the field, pick the collaboration), *Core* (discover the problem, design, implement, deploy), *Analysis* (reflect, write).
- Validation by the experts is built into every stage — real users, real data.
- The point that matters for this talk: the contribution is allowed to be a **well-justified combination of existing techniques** — a design study does *not* require a new algorithm.
- I ran this with the Institute of Neural Engineering over several months: regular meetings, prototype reviews, expert interviews.

**⛔ not here:** don't enumerate the nine stages.

---

### 3 · Electroencephalography (EEG)  ⏱ ~1:45
**▶ "The data we work with is EEG."**
- It measures the tiny voltages from the **post-synaptic potentials of neurons**, picked up by scalp electrodes, at **millisecond** resolution — so it's excellent for *timing*, unlike fMRI.
- The scalp variant is **non-invasive, portable, and low-cost** — which is why it's a workhorse in both research and the clinic.
- It's used for **epilepsy diagnosis, sleep staging, brain–computer interfaces, and cognitive studies**.
- Electrodes follow the standardised **10–10 system**; our recordings use a 32-channel subset (left: the layout; right: a real cap).
- But raw EEG is essentially **unusable as-is** — it needs preprocessing (filtering, bad-channel handling, ICA, resampling), because the quality of every downstream conclusion depends on the quality of the signal.

**⛔ not here:** the artefacts themselves — next slide.

---

### 4 · EEG Artefacts  ⏱ ~2:30
**▶ "And every recording is full of non-brain signal."**
- An **artefact** is any recorded potential that doesn't come from the brain — and they can be **larger in amplitude than the brain activity itself**, so they dominate the trace (figure: three eye-blinks burying the frontal channels).
- They come from the body (eyes, muscle, heart) and from outside (power line, movement).
- The hard part is that they **overlap the brain signal in time and frequency** — ocular activity mimics slow brain waves, muscle spreads across every band — so you *can't* just band-pass or threshold them away.
- And if they slip through, they corrupt the analysis — wrong artefacts in, wrong conclusions out.

**⛔ not here:** the specific types — next slide.

---

### 5 · Artefact Types  ⏱ ~3:15
**▶ "They split into two families — and each behaves differently."**
**Physiological (from the body):**
- **Ocular** — eye movements and blinks; because the eyes sit right next to the brain, these are the **most common and impactful**; large-amplitude, slow, low-frequency, frontal; measured by EOG.
- **Cardiac** — the heartbeat; **rhythmic, sharp spikes** mirroring the ECG, strongest over the left temporal area.
- **Muscle (EMG)** — contractions in the head and neck; they occur at **random, non-repetitive** times, span **below 1 Hz to over 200 Hz** so they hit *every* brain band, and show up on *all* sensors — which makes muscle the **hardest** artefact to remove cleanly.

**Technical (external):**
- **Line noise** — 50 Hz mains, a constant oscillation on all channels; removed with a notch filter.
- **Electrode/channel** — poor contact or dry gel; shows as jumps, flat lines, or a single noisy electrode.
- **Movement** — cable or electrode shifts; sudden big deflections, common in mobile setups.
- **Environmental** — nearby devices (lights, monitors, phones).

- Punchline: **line noise goes in preprocessing**; the **physiological** ones overlap the brain and must be *separated* — that's ICA. And remember muscle is the hard one — it comes back later.

**⛔ not here:** how we separate them — ICA, next.

---

### 6 · Independent Component Analysis  ⏱ ~4:15
**▶ "The standard way to separate them is ICA."**
- ICA takes the mixed signal recorded at the electrodes and unmixes it into **statistically independent components**, one per underlying source.
- The intuition is the **cocktail-party problem**: several microphones each pick up a different mixture of the same voices, and ICA recovers the individual voices — the EEG electrodes are the microphones.
- Formally: the data is **X = A·S** (mixing matrix × sources); ICA estimates an unmixing matrix **W ≈ A⁻¹** so that **Ŝ = W·X** recovers the sources. It works by making the components as **non-Gaussian** (independent) as possible — that's FastICA.
- Each recovered component then carries the **three properties** an expert reads: a **topography** (how it projects onto the scalp), a **power spectrum**, and a **time course** — these become the three views in the tool.
- The number of components is capped at the number of electrodes; we default to **16 per recording**.

**⛔ not here:** classification — next slide.

---

### 7 · Manual Component Classification  ⏱ ~5:00
**▶ "But ICA doesn't tell you which component is brain and which is artefact."**
- So an **expert looks at every component** and decides **keep** (brain) or **reject** (artefact), based on those three properties.
- Rejected components are removed by **back-projection** — the clean signal is rebuilt from the kept components only (figure: the two blinks in red are gone in the black, cleaned trace).
- That step is **irreversible**, and the cost is **asymmetric**: a wrongly *rejected* brain component **destroys real signal**; a wrongly *kept* artefact **leaves noise behind**. That's the core reason you keep a human in control instead of fully automating.
- And it's the bottleneck: **hundreds of participants × ~16 components** each — the review is slow and cognitively heavy, which pushes people toward rushed, inconsistent decisions, and the *reasoning* behind each call is never recorded.

**⛔ not here:** the tools — next slide. (Figure is MNE sample data, illustrative.)

---

### 8 · Existing Tools  ⏱ ~6:00
**▶ "Don't existing tools already do this?"**
- **MNE-Python** — the richest primitives; its `plot_properties` shows topomap, spectrum and epochs together, and it has per-artefact detectors — but **no unified classifier and no calibrated confidence**.
- **FieldTrip** — MATLAB; ICA classification is a **script-driven manual task**, no automated step, no cross-recording view.
- **EEGLAB** — a GUI with a topographic grid and per-component popups; cross-subject clustering exists but needs **manual setup**, and the classifier ships as the ICLabel plugin.
- **ICLabel / MARA / ADJUST** — automated classifiers that output a label or probability per component, but with **no interactive interface** and fixed outputs.
- The table is the argument: **no single tool combines** linked view updates, a cross-participant overview, batch-applying recommendations, and a calibrated confidence. ICARUS builds those missing pieces **on the same primitives**.

**⛔ not here:** don't narrate every cell; don't say "novel" — concede the parts, own the **combination**.

---

### 9 · Research Questions  ⏱ ~6:45
**▶ "This gives me two research questions."**
- **RQ1:** which visualisation and interaction techniques support expert decision-making in ICA artefact classification?
- **RQ2:** how can ML recommendations and interactive visualisation be integrated to **accelerate** classification while **preserving expert control**?
- These two are the contract for everything that follows.

**Cue:** read both slowly · PAUSE.

---

### 10 · Requirements  ⏱ ~7:30
**▶ "Six requirements came straight out of the design study."**
- They were **derived from the discovery interviews** with the two experts, so each one is a real need.
- **Overview & guidance** — *R1*, visual keep/reject recommendations; *R2*, an overview of all components for exploration and context.
- **Component inspection** — *R3*, when and how artefacts appear over time; *R4*, telling brain from artefact; *R5*, the scalp distribution.
- **Workflow** — *R6*, logging and exporting decisions for reproducibility and team consistency.
- Every feature in the tool maps back to one of these — I'll point them out as we go.

**⛔ not here:** don't read all six word-for-word — group them.

---

### 11 · System Architecture  ⏱ ~8:15
**▶ "Briefly, how it's built."**
- **Two Docker containers**: a Python **Flask** backend (preprocessing, ML inference, file handling) and a **React** frontend served by nginx, talking over a REST API.
- The backend uses the standard stack — **MNE-Python** for signal processing, **scikit-learn** for the classifier, **aeon/MiniRocket** for features, **umap-learn** for the projection — and loads recordings from XDF.
- The frontend draws every visualisation on **HTML canvas** for speed.
- The preprocessing **defaults** (50/100 Hz notch, 1–75 Hz band-pass, 250 Hz resample, FastICA into 16 components) are tuned to the lab but **configurable per study**.

**⛔ not here:** don't dwell — it's plumbing; the ML pipeline is a backup slide.

---

### 12 · Classification Workflow  ⏱ ~9:00
**▶ "The whole workflow lives in one tool — here it is end to end."**
- **(A) Preprocess** — you create a *study* (recordings from several participants sharing a config), upload, and run preprocessing.
- **(B) Apply recommendations** — the model scores every component, and you apply the **high-confidence ones in bulk**, covering most without opening them.
- **(C) Inspect** — for the uncertain ones, you open the coordinated views and decide.
- **(D) Control clusters** — at any point you check the cross-participant overview to catch outliers.
- Then you **export** the labels for downstream analysis, and you can feed them back to **retrain** the recommender as more data accumulates.

**⛔ not here:** don't explain each view — they each get a slide next.

---

### 13 · Confidence Recommender  ⏱ ~10:00
**▶ "First building block — the recommender."**
- The model gives each component a **keep-probability** (1 = brain, 0 = artefact).
- That maps to **three tiers**, shown as five colour bands symmetric around 50%: **Tier 1** (≥75% or ≤25%) is confident enough to **batch-apply**; **Tier 2** (the 60–75% / 25–40% zones) is a **quick review**; **Tier 3** (40–60%) is the uncertain middle that needs **full manual inspection**.
- That's the principle of **classification with a reject option** — instead of forcing a guess on the uncertain ones, you hand them to the expert.
- Muscle is hard, so it gets a **separate badge** from MNE's muscle detector — faded orange for medium scores, solid for high — an independent second signal.
- Key design choice: it's an **indicator only**. It never pre-sets the decision — the expert confirms every component. That came directly from the evaluation.

**⛔ not here:** the ML internals / accuracy — backup. (~40% land in Tier 1 — only if asked.)

---

### 14 · Participants and Components Overview  ⏱ ~10:45
**▶ "You start from this overview."**
- The **left panel** lists every participant with their running tally — kept / rejected / undecided out of the total components.
- The **right panel** shows the selected participant's components, each with its **confidence bar, a one-click tier button, and a muscle badge** where relevant.
- One **Apply** per participant does the bulk work: it first auto-rejects any undecided component with a strong muscle badge, then applies the Tier 1 recommendation to the rest — and it **never overwrites** anything you labelled by hand. There's a reset too.
- Click any component row and it opens in the inspector.

**⛔ not here:** the inspector detail — next slide.

---

### 15 · Component Inspector  ⏱ ~11:30
**▶ "Click a component and the inspector opens — this is the heart of the tool."**
- It puts all the views for **one component on a single screen**: the recommender bar, the cluster, the topomap, the spectrum, and the timeline.
- When you move to another component — a row, a point in the cluster, or the arrow keys — the inspector **stays open and every linked view updates together**.
- That's the thing the **script workflow can't do**: there you'd regenerate static plots for each component and lay them out yourself. Here it's one coordinated screen.
- As the expert put it: *"the entire visualisation on one screen, more or less."*

**[DEMO LOOP — narrate it; look at the committee, not the screen.]**
**⛔ not here:** don't explain each view in depth — topomap, PSD, timeline get their own slides.

---

### 16 · Component Cluster and Mapper Graph  ⏱ ~12:15
**▶ "And a study-wide overview across all participants."**
- Both views take **every component from every participant** and project them into one shared 2D space, so components that look alike sit close together. (Under the hood: a UMAP projection of MiniRocket descriptors.)
- The **Component Cluster** is a scatter — **one dot per component**, coloured by its keep/reject label, or switched to colour by the recommender's confidence.
- The **Mapper Graph** summarises the same data: nearby components are grouped into **pie-chart nodes** sized by how many they hold and joined by edges — the big-picture structure without the overplotting.
- The most valuable use is **outlier detection**: a dot whose colour differs from the cluster around it flags a likely **misclassification** worth a second look — overview first, then drill down.

**⛔ not here:** the actual caught case is the evaluation — don't over-claim here.

---

### 17 · Topomap  ⏱ ~12:45
**▶ "The first detail view — topography, the expert's first criterion."**
- It shows **how strongly each scalp sensor contributes** to the component and with what **polarity**, read straight from the ICA mixing matrix, in microvolts; values between electrodes are interpolated into a continuous map (blue → white → red = negative → zero → positive).
- The patterns do the classifying: a **smooth dipolar** gradient = **brain**; activity over the **frontal** electrodes = **ocular**; an isolated **peripheral patch** = **muscle**; a single dominant electrode usually means a **bad channel**.
- Hovering shows the electrode name and exact amplitude, and it re-computes for a selected timeline segment.

**⛔ not here:** don't re-explain the inspector.

---

### 18 · Power Spectral Density  ⏱ ~13:15
**▶ "The second detail view — the spectrum, the other primary criterion."**
- It shows how the component's **power is distributed across frequency**, with the five EEG bands shaded as the background.
- The *shape* names the source: a **falling 1/f slope**, often with an alpha peak, = **brain**; a **flat or rising** profile at high frequency = **muscle**; **delta/theta-dominant** = **ocular**.
- Like the topomap, it updates to whatever **time range** you select on the timeline — so you can check whether a component's spectrum changes between clean and noisy stretches.

---

### 19 · Timeline  ⏱ ~13:45
**▶ "The third detail view — the timeline, used to confirm."**
- It shows the component's **signal over the whole recording**; scroll to zoom time, drag to pan, and zoom the amplitude on the y-axis.
- **Right-click-drag selects a time range**, which drives the topomap and PSD to that segment — so you compare a clean period against an artefact-heavy one.
- It carries **three overlays**: **EOG markers** (detected eye-blink events, from MNE's blink detection on Fp1) — if a component's spikes line up with them, it's likely ocular; **trial markers** (the experimental trial periods, so you can tell whether an artefact even falls inside a trial); and **PSD clusters**.
- Its role is **confirmatory** — eye artefacts show up as the classic large, slow deflections.

**⛔ not here:** the PSD-cluster detail — next slide.

---

### 20 · PSD Clustering  ⏱ ~14:15
**▶ "One overlay is worth a closer look — PSD clustering."**
- It runs **k-means on the power spectra of overlapping time windows**, taken across all of the participant's components, and **colour-codes the timeline** by which spectral cluster each segment belongs to.
- You can set the **number of clusters, the window size, and the step**; segments with similar spectral profiles get the same colour.
- The payoff: you can **see at a glance where a component's frequency character changes** over time — and hovering a segment shows that cluster's mean spectrum (the figure shows all clusters side by side).

---

### 21 · Evaluation  ⏱ ~15:15
**▶ "I evaluated ICARUS with two domain experts on their own EEG data."**
- It was a **think-aloud** session plus a semi-structured interview, on **real brain–computer-interface recordings** from their lab — so the data and the task were exactly what they do day to day.
- I gave them **five hands-on tasks** that touched every building block: **recommendations · artefact identification · band-power analysis · mapper exploration · view comparison**.
- Two things stood out. First, the think-aloud revealed a **consistent decision sequence** — component index, then topomap, then spectrum, then timeline — which **matches the layout**, confirming the views were the right ones. The recommender was trusted on **ocular** artefacts; **muscle** they still checked by hand.
- Second — the key moment — the cross-participant overview let the expert **catch a component that had been mislabeled**. That's the headline: pairing an imperfect classifier with expert inspection can correct errors the model alone would miss.

**▶ CLOSE (no slide — say it):** "So the value is the integration — the recommender handles the clear cases, the expert keeps control of the rest. Thank you, I'm happy to take your questions." *(PAUSE before "Thank you".)*
**⛔ not here:** don't volunteer limitations — keep them for Q&A.

---

### B · Backups (Q&A only)
- **Recommender Pipeline** — *how it works:* each component's time series → **MiniRocket** (9,996 features) + the **16 unmixing-matrix weights** → a **Random Forest** → keep-probability → tiers; ~75% test accuracy on 480 components from 30 participants. *(Lead with retrainability; don't volunteer "second-best model"; ICLabel question → answer verbally.)*
- **Classification Experiments** — the nine-experiment table; MiniRocket was chosen for **speed** — Matrix Profile scored a bit higher (79% vs 75%) but needs an expensive self-join.
- **Data Flow** — decisions export as **YAML**, re-importable into MNE (`ica.exclude` → `ica.apply()`), with the preprocessing metadata preserved; accumulated labels can retrain the recommender.

**Q&A routine:** listen → rephrase → pause → answer briefly. Press **O** for the slide overview to jump to a backup.
