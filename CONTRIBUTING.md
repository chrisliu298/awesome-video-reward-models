# Contributing

Contributions welcome! Open a PR to add papers, datasets, benchmarks, or tools related to video reward models.

## Inclusion criteria

A resource must pass **one** of these two tests:

1. **The work defines, trains, or substantively applies a video-generation reward signal** — a learned, frozen, programmatic, or self-supervised model, metric, reward/preference model, judge, critic, or verifier whose output (a score, label, constraint, or verification outcome) is used to train, rank, filter, search, steer, or improve a video generator.
2. **The work directly enables video reward model deployment.** Benchmarks, evaluation suites, preference datasets, and tooling that make video reward modeling practical.

The reward signal need not be a separately trained scalar reward model. In scope, all first-class: **pairwise/listwise preference (Bradley-Terry and beyond), pointwise regression / MOS, classification / defect detection (incl. data-filtering quality models), verifiable / rule-based / measurable rewards (RLVR), frozen or training-free critics, and energy-based / generative / self-supervised judges.** An **implicit** preference objective (e.g. DPO/IPO, which trains no separate reward model) and a **model-assisted measurable** reward (optical flow, pose, depth, inverse dynamics) both qualify.

### What does NOT qualify

- **Generic video QA.** Video question-answering or captioning not tied to generation quality assessment.
- **Image-only reward models** unless they are direct conceptual foundations for video reward modeling (e.g., ImageReward, HPS v2).
- **Pure generation methods** without a substantive reward or preference component. A paper that uses a reward model incidentally (e.g., a single Best-of-N line in the appendix) does not qualify.
- **RL with only generic or incidental rewards.** Plain RL whose reward is not a substantive, video-specific design does not qualify — **but a video-specific programmatic, measurable, or verifiable reward (RLVR) is in scope**, even with no learned reward model. "Reward-model-free" is not a rejection reason; "no substantive video-derived reward signal" is.
- **Domain-specific applications** need a transferable methodological contribution beyond the domain. A paper that applies an existing reward model to a new dataset without methodological innovation does not clear the bar.

### When in doubt

Read the actual paper. If the verdict depends on whether a reward, preference, or judge model is a substantive part of the method, check the method section.

## Section placement

### Core Video Reward Model Papers

Only add here if the paper is among the most important and widely referenced works. When unsure, place in a more specific section below.

### Foundations

Essential background from image reward models, RLHF/DPO for diffusion, and video quality assessment. Rarely updated.

### General-Purpose Video Reward Models and Judges

Evaluator-first papers. Subsections:

- **Early generation-specific evaluators** — first-generation T2V evaluators
- **Large-scale and fine-grained judges** — modern LMM-based and decomposed judges

### Preference Optimization and Post-Training

Papers whose main contribution is using reward or preference signals to align a video generator. Subsections:

- **Reward-weighted or gradient-based finetuning** — RLHF-style, reward backpropagation
- **DPO / IPO / GRPO and related preference optimization** — direct preference methods

### Localized, Structured, and Reasoning-Based Rewards

Reward designs that move past a single opaque scalar score.

### Physics, World, and Geometry Rewards

Subsections:

- **Benchmarks and problem framing** — physics evaluation and testbeds
- **Physics-aware optimization** — reward-guided physics alignment
- **World and geometry critics** — world models and geometry-based rewards

### Editing, Identity, Camera, and Domain-Specific Rewards

Specialized generation settings.

### Inference-Time Reward, Search, and Process Rewards

Reward models used at test time or over latent trajectories.

### Datasets and Benchmarks

Table format. Subsections for preference datasets and evaluation benchmarks.

### Evaluation Suites and Stress Tests

Benchmarks for checking reward model robustness.

### Project Pages, Repos, and Useful Links

Links to repos, leaderboards, and adjacent collections.

## Entry format

```
- [Full Paper Title](url) *(Year)* — One-line description.
```

The description should be a single terse sentence capturing what makes this paper distinctive. Look at existing entries for calibration.

## Batch additions

When adding multiple papers at once:

- **Flag section growth.** If a single update would double any subsection's size, consider whether the section needs splitting or whether some entries are marginal.
- **Prioritize gap-filling over completeness.** A paper that opens a new niche has higher priority than a fourth variant in a well-covered area.
- **Cap awareness.** Large batches dilute curation signal. Prefer 5-8 high-confidence additions over 12+ with several borderline entries.

## Taxonomy and reading path

- Update the taxonomy tables when a paper clearly fits an existing row. Skip taxonomy updates for papers that don't fit neatly.
- The Start Here reading path changes rarely. Only add a paper if it is the best introduction to a topic currently underrepresented in the path.

## Duplication

Duplication across sections is fine for exceptionally central papers, but prefer one primary location.
