# Talk storyline — ICARUS defense

## The spine (one sentence)
ICA cleans EEG by splitting it into components, but a human still has to judge every component keep-or-reject — a slow, per-component bottleneck — and **ICARUS supports that decision** by combining a confidence recommender, coordinated detail views, and a cross-participant overview, so the ML handles the clear cases while the expert keeps control of the rest.

## The narrative (concise, in order)

**1 · Why this matters.** EEG records brain activity cheaply, non-invasively, at millisecond resolution — but every recording is full of non-brain *artefacts* (eyes, muscle, heart, line noise) that overlap the brain signal in time and frequency, so you can't simply filter or threshold them out.

**2 · The standard fix is ICA.** Independent Component Analysis unmixes the recording into independent source components, each with a topography, a spectrum, and a time course. That is what makes cleaning possible.

**3 · The catch — and the real problem.** ICA does not say which component is brain and which is artefact. An expert must review **every component** and decide keep or reject, and the removal is irreversible. This manual, per-component judgment is the bottleneck: slow, subjective, and it does not scale to studies with many participants.

**4 · The gap.** Existing tools each do parts of this — MNE and EEGLAB give the per-component views, ICLabel and MARA give automated labels — but **no single tool combines** linked detail views, a cross-participant overview, and ML assistance in one workflow.

**5 · The approach.** Through a **design study** with EEG experts at TU Graz, I built **ICARUS** around that keep/reject decision. The point isn't a new algorithm — it's combining the building blocks that work into one tool, validated with the experts.

**6 · The tool.** A confidence recommender triages components into three tiers (auto-accept the clear ones, review the uncertain ones). You start from a list of every participant and component, click one, and a coordinated inspector shows its topomap, spectrum, and timeline together, updating as you switch components. A cross-participant cluster gives a study-wide overview, and the decisions export straight back into the MNE pipeline.

**7 · Evaluation.** Two domain experts used ICARUS on their own EEG data through five hands-on tasks, thinking aloud.

**8 · The takeaway (the one thing to remember).** The value is the **integration**: the recommender removes the easy cases, the expert keeps control of the hard ones, and the cross-participant overview can even surface a component that had been mislabeled — something automation alone would miss.

## What to keep in front of the message
- Lead with the **problem** (the manual per-component bottleneck), not the tool.
- The contribution is the **combination**, not novelty of any single part — say that out loud (design-study framing).
- Keep limitations/issues for the Q&A — the talk stays on what works.
