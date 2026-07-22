# 🎬 Awesome Reward Models for Video Generation

<p align="center">
  <a href="https://awesome.re"><img src="https://img.shields.io/badge/Awesome-%F0%9F%8E%AC_Reward_Models_for_Video_Generation-000000?style=for-the-badge&labelColor=000000" alt="Awesome Reward Models for Video Generation"></a>
</p>

<p align="center">
  <!-- entry-count-start --><a href="#contents"><img src="https://img.shields.io/badge/Entries-217-000000?style=for-the-badge&labelColor=000000" alt="Entries"></a><!-- entry-count-end -->
  <a href="https://github.com/chrisliu298/awesome-rm-for-video-generation/stargazers"><img src="https://img.shields.io/github/stars/chrisliu298/awesome-rm-for-video-generation?style=for-the-badge&logo=github&logoColor=white&label=Stars&labelColor=000000&color=000000" alt="GitHub Stars"></a>
  <a href="https://github.com/chrisliu298/awesome-rm-for-video-generation/network/members"><img src="https://img.shields.io/github/forks/chrisliu298/awesome-rm-for-video-generation?style=for-the-badge&logo=github&logoColor=white&label=Forks&labelColor=000000&color=000000" alt="GitHub Forks"></a>
  <a href="https://github.com/chrisliu298/awesome-rm-for-video-generation/commits"><img src="https://img.shields.io/github/last-commit/chrisliu298/awesome-rm-for-video-generation?style=for-the-badge&logo=github&logoColor=white&label=Last%20Commit&labelColor=000000&color=000000" alt="Last Commit"></a>
</p>

A curated collection of papers, datasets, benchmarks, and code for **video reward models**: reward models, preference models, scoring models, verifier-style critics, and VideoLLM judges used to evaluate, rank, rerank, filter, or align video generation models.

> **Video reward model** = a model, metric, or verifier that takes a prompt and a generated video (or a pair of generated videos) and outputs a scalar reward, a preference label, or a quality score used to train, rank, filter, or improve a video generator.

This repository covers text-to-video, image-to-video, and text-driven video editing; RLHF/DPO/GRPO for video generation; VideoLLM/MLLM judges; reward-guided inference; and human-preference benchmarks. It excludes generic video QA not tied to generation and image-only reward models unless they are direct foundations for video reward modeling.

The field now spans three partially overlapping camps:

1. **Learned reward and judge models** trained from human scores, pairwise preferences, or synthetic supervision.
2. **Reward-guided training methods** that use those models for RLHF, DPO, GRPO, reranking, or reward backpropagation.
3. **Benchmark and evaluator papers** whose metrics are routinely reused as practical rewards or selection criteria for video generation.

**New to the area?** Read [Start Here](#start-here). **Picking a model or dataset?** Jump to [Quick Start by Goal](#quick-start-by-goal).

## Contents

- [Quick Start by Goal](#quick-start-by-goal)
- [Start Here](#start-here)
- [Taxonomy](#taxonomy)
- [Core Video Reward Model Papers](#core-video-reward-model-papers)
- [Foundations](#foundations)
  - [Image reward models and preference data](#image-reward-models-and-preference-data)
  - [Reward modeling, RLHF, and DPO foundations for diffusion](#reward-modeling-rlhf-and-dpo-foundations-for-diffusion)
  - [Adjacent video quality assessment foundations](#adjacent-video-quality-assessment-foundations)
- [General-Purpose Video Reward Models and Judges](#general-purpose-video-reward-models-and-judges)
  - [Early generation-specific evaluators](#early-generation-specific-evaluators)
  - [Large-scale and fine-grained judges](#large-scale-and-fine-grained-judges)
- [Preference Optimization and Post-Training](#preference-optimization-and-post-training)
  - [Reward-weighted or gradient-based finetuning](#reward-weighted-or-gradient-based-finetuning)
  - [DPO / IPO / GRPO and related preference optimization](#dpo--ipo--grpo-and-related-preference-optimization)
- [Localized, Structured, and Reasoning-Based Rewards](#localized-structured-and-reasoning-based-rewards)
  - [Structured and decomposed rewards](#structured-and-decomposed-rewards)
  - [Localized and detail-aware optimization](#localized-and-detail-aware-optimization)
  - [Reasoning-based judges](#reasoning-based-judges)
  - [Training-free and decomposed critics](#training-free-and-decomposed-critics)
- [Physics, World, and Geometry Rewards](#physics-world-and-geometry-rewards)
  - [Benchmarks and problem framing](#benchmarks-and-problem-framing)
  - [Physics-aware optimization](#physics-aware-optimization)
  - [World and geometry critics](#world-and-geometry-critics)
- [Editing, Identity, Camera, and Domain-Specific Rewards](#editing-identity-camera-and-domain-specific-rewards)
  - [Video editing](#video-editing)
  - [Identity preservation](#identity-preservation)
  - [Camera control](#camera-control)
  - [Domain-specific alignment](#domain-specific-alignment)
- [Inference-Time Reward, Search, and Process Rewards](#inference-time-reward-search-and-process-rewards)
  - [Reward-guided inference](#reward-guided-inference)
  - [Latent search and process rewards](#latent-search-and-process-rewards)
- [Datasets and Benchmarks](#datasets-and-benchmarks)
- [Evaluation Suites and Stress Tests](#evaluation-suites-and-stress-tests)
- [Project Pages, Repos, and Useful Links](#project-pages-repos-and-useful-links)
- [Contributing](#contributing)
- [Citation](#citation)

## Quick Start by Goal

| Goal | Start with | Then read |
|---|---|---|
| New to the area | [VideoScore](https://arxiv.org/abs/2406.15252), [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) | [MJ-VIDEO](https://arxiv.org/abs/2502.01719), [VisionReward](https://arxiv.org/abs/2412.21059), [VideoScore2](https://arxiv.org/abs/2509.22799) |
| Building a reusable judge | [AIGV-Assessor](https://arxiv.org/abs/2411.17221), [VisionReward](https://arxiv.org/abs/2412.21059), [VideoReward](https://arxiv.org/abs/2501.13918) | [Q-Eval-100K](https://arxiv.org/abs/2503.02357), [LOVE](https://arxiv.org/abs/2505.12098), [VR-Thinker](https://arxiv.org/abs/2510.10518) |
| Aligning a T2V model with preferences | [InstructVideo](https://arxiv.org/abs/2312.12490), [VideoDPO](https://arxiv.org/abs/2412.14167), [DanceGRPO](https://arxiv.org/abs/2505.07818) | [OnlineVPO](https://arxiv.org/abs/2412.15159), [Dual-IPO](https://arxiv.org/abs/2502.02088), [DenseDPO](https://arxiv.org/abs/2506.03517) |
| Fixing local artifacts and temporal defects | [Patch-level Reward Models](https://arxiv.org/abs/2502.06812), [DenseDPO](https://arxiv.org/abs/2506.03517), [REACT](https://arxiv.org/abs/2601.04033) | [Mind the Generative Details](https://arxiv.org/abs/2601.04068), [VideoScore2](https://arxiv.org/abs/2509.22799) |
| Working on physics or world consistency | [PISA](https://arxiv.org/abs/2503.09595), [VideoPhy-2](https://arxiv.org/abs/2503.06800), [WMReward](https://arxiv.org/abs/2601.10553) | [VIGOR](https://arxiv.org/abs/2603.16271), [PhysHPO](https://arxiv.org/abs/2508.10858), [VGGRPO](https://arxiv.org/abs/2603.26599) |
| Working on editing, identity, or control | [TDVE-Assessor](https://arxiv.org/abs/2505.19535), [Identity-GRPO](https://arxiv.org/abs/2510.14256), [CamPilot](https://arxiv.org/abs/2601.16214) | [Aligning Anime Video Generation with Human Feedback](https://arxiv.org/abs/2504.10044), [Prompt-A-Video](https://arxiv.org/abs/2412.15156) |

## Start Here

The fastest reading path through the area:

0. **Image-side foundations** — [ImageReward](https://arxiv.org/abs/2304.05977), [Pick-a-Pic / PickScore](https://arxiv.org/abs/2305.01569), and [HPS v2](https://arxiv.org/abs/2306.09341). These established learned human-preference scoring for generative models.
1. **First video-side transfer of RLHF ideas** — [InstructVideo](https://arxiv.org/abs/2312.12490). A pragmatic starting point: adapt image rewards to videos and see what breaks.
2. **First widely reused open video evaluator** — [VideoScore](https://arxiv.org/abs/2406.15252). The standard starting baseline for human-aligned video scoring.
3. **Modern open reward model + alignment stack** — [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918). Introduces VideoReward, VideoGen-RewardBench, and Flow-DPO / Flow-RWR / Flow-NRG.
4. **Fine-grained multi-criterion reward modeling** — [MJ-VIDEO](https://arxiv.org/abs/2502.01719) and [VisionReward](https://arxiv.org/abs/2412.21059). These are the clearest examples of decomposed reward design.
5. **Preference optimization for video generators** — [VideoDPO](https://arxiv.org/abs/2412.14167), [OnlineVPO](https://arxiv.org/abs/2412.15159), and [DenseDPO](https://arxiv.org/abs/2506.03517).
6. **Reasoning-based judges** — [VideoScore2](https://arxiv.org/abs/2509.22799), [VR-Thinker](https://arxiv.org/abs/2510.10518), and [REACT](https://arxiv.org/abs/2601.04033). The reward model itself becomes a reasoning system.
7. **Physics and world consistency** — [PISA](https://arxiv.org/abs/2503.09595), [VideoPhy-2](https://arxiv.org/abs/2503.06800), [WMReward](https://arxiv.org/abs/2601.10553), [VIGOR](https://arxiv.org/abs/2603.16271), and [PhysHPO](https://arxiv.org/abs/2508.10858).
8. **Specialized settings** — [TDVE-Assessor](https://arxiv.org/abs/2505.19535) for editing, [Identity-GRPO](https://arxiv.org/abs/2510.14256) for multi-human identity, and [CamPilot](https://arxiv.org/abs/2601.16214) for camera control.

## Taxonomy

Many papers fit multiple categories. The tables below are for orientation, not strict partitioning.

### By training signal

| Signal type | Typical papers |
|---|---|
| Human preferences / MOS / expert ratings | [T2VQA](https://arxiv.org/abs/2403.11956), [VideoScore](https://arxiv.org/abs/2406.15252), [LiFT](https://arxiv.org/abs/2412.04814), [AIGV-Assessor](https://arxiv.org/abs/2411.17221), [VisionReward](https://arxiv.org/abs/2412.21059), [VideoReward](https://arxiv.org/abs/2501.13918), [MJ-VIDEO](https://arxiv.org/abs/2502.01719), [Q-Eval-100K](https://arxiv.org/abs/2503.02357), [LOVE](https://arxiv.org/abs/2505.12098), [VideoScore2](https://arxiv.org/abs/2509.22799), [TDVE-Assessor](https://arxiv.org/abs/2505.19535) |
| Synthetic comparisons or corruption-based supervision | [Discriminator-Free DPO](https://arxiv.org/abs/2504.08542), [DenseDPO](https://arxiv.org/abs/2506.03517), [Mind the Generative Details](https://arxiv.org/abs/2601.04068), [Reg-DPO](https://arxiv.org/abs/2511.01450), [REACT](https://arxiv.org/abs/2601.04033), [PhyGDPO](https://arxiv.org/abs/2512.24551) |
| Training-free or external critics | [InstructVideo](https://arxiv.org/abs/2312.12490), [Diffusion-DRF](https://arxiv.org/abs/2601.04153), [HuDA](https://arxiv.org/abs/2601.14037), [WMReward](https://arxiv.org/abs/2601.10553), [VIGOR](https://arxiv.org/abs/2603.16271), [PISCES](https://arxiv.org/abs/2602.01624) |
| Hybrid pipelines | [T2V-Turbo](https://arxiv.org/abs/2405.18750), [T2V-Turbo-v2](https://arxiv.org/abs/2410.05677), [Dual-IPO](https://arxiv.org/abs/2502.02088), [UnifiedReward](https://arxiv.org/abs/2503.05236), [DanceGRPO](https://arxiv.org/abs/2505.07818), [PhysHPO](https://arxiv.org/abs/2508.10858) |

### By architecture

| Architecture family | Typical papers |
|---|---|
| CLIP / VLM scorer + regression or ranking head | [T2VQA](https://arxiv.org/abs/2403.11956), early [T2V-Turbo](https://arxiv.org/abs/2405.18750) reward mixtures, image-reward carryovers used in [InstructVideo](https://arxiv.org/abs/2312.12490) |
| Specialized video encoder + regression / fusion head | [VideoScore](https://arxiv.org/abs/2406.15252), [UGVQ](https://arxiv.org/abs/2407.21408), [T2VEval](https://arxiv.org/abs/2501.08545), [CRAVE](https://arxiv.org/abs/2502.04076) |
| MLLM / VideoLLM judge | [AIGV-Assessor](https://arxiv.org/abs/2411.17221), [Q-Eval-100K](https://arxiv.org/abs/2503.02357), [LOVE](https://arxiv.org/abs/2505.12098), [VideoReward](https://arxiv.org/abs/2501.13918), [VideoScore2](https://arxiv.org/abs/2509.22799), [VR-Thinker](https://arxiv.org/abs/2510.10518), [TDVE-Assessor](https://arxiv.org/abs/2505.19535), [UnifiedReward](https://arxiv.org/abs/2503.05236) |
| Structured / MoE / decomposed reward | [MJ-VIDEO](https://arxiv.org/abs/2502.01719), [VisionReward](https://arxiv.org/abs/2412.21059), [Patch-level Reward Models](https://arxiv.org/abs/2502.06812), [DenseDPO](https://arxiv.org/abs/2506.03517), [REACT](https://arxiv.org/abs/2601.04033) |

### By usage mode

| Usage mode | Typical papers |
|---|---|
| Best-of-N reranking / evaluator-only use | [VideoScore](https://arxiv.org/abs/2406.15252), [VisionReward](https://arxiv.org/abs/2412.21059), [MJ-VIDEO](https://arxiv.org/abs/2502.01719), [AIGV-Assessor](https://arxiv.org/abs/2411.17221), [Q-Eval-100K](https://arxiv.org/abs/2503.02357), [LOVE](https://arxiv.org/abs/2505.12098), [VideoScore2](https://arxiv.org/abs/2509.22799) |
| Reward-weighted or gradient-based finetuning | [InstructVideo](https://arxiv.org/abs/2312.12490), [T2V-Turbo](https://arxiv.org/abs/2405.18750), [Video Diffusion Alignment via Reward Gradients](https://arxiv.org/abs/2407.08737), [DanceGRPO](https://arxiv.org/abs/2505.07818), [PhysRVG](https://arxiv.org/abs/2601.11087) |
| DPO / IPO / preference optimization | [VideoDPO](https://arxiv.org/abs/2412.14167), [OnlineVPO](https://arxiv.org/abs/2412.15159), [HuViDPO](https://arxiv.org/abs/2502.01690), [Dual-IPO](https://arxiv.org/abs/2502.02088), [DenseDPO](https://arxiv.org/abs/2506.03517), [Reg-DPO](https://arxiv.org/abs/2511.01450), [PhysCorr](https://arxiv.org/abs/2511.03997), [PhyGDPO](https://arxiv.org/abs/2512.24551) |
| Inference-time reward guidance / search | [Flow-NRG](https://arxiv.org/abs/2501.13918), [DOLLAR](https://arxiv.org/abs/2412.15689), [WMReward](https://arxiv.org/abs/2601.10553), [VIGOR](https://arxiv.org/abs/2603.16271), [LatSearch](https://arxiv.org/abs/2603.14526), [PISCES](https://arxiv.org/abs/2602.01624), [Euphonium](https://arxiv.org/abs/2602.04928) |
| Data filtering / pair construction / prompt evolution | [Prompt-A-Video](https://arxiv.org/abs/2412.15156), [V.I.P.](https://arxiv.org/abs/2508.03254), [Discriminator-Free DPO](https://arxiv.org/abs/2504.08542), [PhysHPO](https://arxiv.org/abs/2508.10858), [UnifiedReward](https://arxiv.org/abs/2503.05236) |

### By target property

| Target property | Typical papers |
|---|---|
| General quality + prompt alignment | [VideoScore](https://arxiv.org/abs/2406.15252), [AIGV-Assessor](https://arxiv.org/abs/2411.17221), [VideoReward](https://arxiv.org/abs/2501.13918), [Q-Eval-100K](https://arxiv.org/abs/2503.02357), [LOVE](https://arxiv.org/abs/2505.12098), [UnifiedReward](https://arxiv.org/abs/2503.05236) |
| Motion / temporal consistency / local defects | [CRAVE](https://arxiv.org/abs/2502.04076), [Patch-level Reward Models](https://arxiv.org/abs/2502.06812), [DenseDPO](https://arxiv.org/abs/2506.03517), [REACT](https://arxiv.org/abs/2601.04033), [Mind the Generative Details](https://arxiv.org/abs/2601.04068) |
| Physics / world / geometry consistency | [PISA](https://arxiv.org/abs/2503.09595), [VideoPhy](https://arxiv.org/abs/2406.03520), [VideoPhy-2](https://arxiv.org/abs/2503.06800), [PhysHPO](https://arxiv.org/abs/2508.10858), [WMReward](https://arxiv.org/abs/2601.10553), [VIGOR](https://arxiv.org/abs/2603.16271), [VGGRPO](https://arxiv.org/abs/2603.26599), [PhysRVG](https://arxiv.org/abs/2601.11087) |
| Editing / identity / camera / domain control | [TDVE-Assessor](https://arxiv.org/abs/2505.19535), [Identity-GRPO](https://arxiv.org/abs/2510.14256), [Identity-Preserving I2V](https://arxiv.org/abs/2510.14255), [CamPilot](https://arxiv.org/abs/2601.16214), [Aligning Anime Video Generation with Human Feedback](https://arxiv.org/abs/2504.10044) |

## Core Video Reward Model Papers

The papers below are the fastest way to get a working mental model of the field.

- [InstructVideo: Instructing Video Diffusion Models with Human Feedback](https://arxiv.org/abs/2312.12490) *(2023)* — First explicit RLHF-style alignment paper for text-to-video; adapts image reward models to video segments and highlights the gap between frame quality and temporal quality.
- [Subjective-Aligned Dataset and Metric for Text-to-Video Quality Assessment](https://arxiv.org/abs/2403.11956) *(2024)* — One of the first dedicated human-rated T2V quality datasets and metrics.
- [VideoScore: Building Automatic Metrics to Simulate Fine-Grained Human Feedback for Video Generation](https://arxiv.org/abs/2406.15252) *(2024)* — The most important early reusable open evaluator for generated videos.
- [AIGV-Assessor: Benchmarking and Evaluating the Perceptual Quality of Text-to-Video Generation with LMM](https://arxiv.org/abs/2411.17221) *(2024)* — Large-scale LMM-based judge for pointwise scoring and pairwise comparison.
- [VisionReward: Fine-Grained Multi-Dimensional Human Preference Learning for Image and Video Generation](https://arxiv.org/abs/2412.21059) *(2024)* — Fine-grained decomposed reward over multiple dimensions and judgment questions.
- [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) *(2025)* — Introduces VideoReward, VideoGen-RewardBench, and a strong open alignment stack for flow-based video models.
- [MJ-VIDEO: Fine-Grained Benchmarking and Rewarding Video Preferences in Video Generation](https://arxiv.org/abs/2502.01719) *(2025)* — MoE-based fine-grained reward model with criterion-level preference supervision.
- [VideoDPO: Omni-Preference Alignment for Video Diffusion Generation](https://arxiv.org/abs/2412.14167) *(2024)* — Reference DPO adaptation for video diffusion.
- [Q-Eval-100K: Evaluating Visual Quality and Alignment Level for Text-to-Vision Content](https://arxiv.org/abs/2503.02357) *(2025)* — Large MOS dataset and judge model spanning image and video.
- [VideoScore2: Think before You Score in Generative Video Evaluation](https://arxiv.org/abs/2509.22799) *(2025)* — Adds structured reasoning traces and interpretable scoring.
- [VR-Thinker: Boosting Video Reward Models through Thinking-with-Image Reasoning](https://arxiv.org/abs/2510.10518) *(2025)* — Active perception and memory-driven reward reasoning.
- [PISA Experiments: Exploring Physics Post-Training for Video Diffusion Models by Watching Stuff Drop](https://arxiv.org/abs/2503.09595) *(2025)* — Physics as a first-class reward-design problem.
- [Inference-time Physics Alignment of Video Generative Models with Latent World Models](https://arxiv.org/abs/2601.10553) *(2026)* — WMReward shows latent world models can act as strong inference-time critics.
- [VIGOR: VIdeo Geometry-Oriented Reward for Temporal Generative Alignment](https://arxiv.org/abs/2603.16271) *(2026)* — Geometry-based reward for temporal alignment and world consistency.
- [TDVE-Assessor: Benchmarking and Evaluating the Quality of Text-Driven Video Editing with LMMs](https://arxiv.org/abs/2505.19535) *(2025)* — The clearest dedicated reward/evaluator paper for text-driven video editing.

## Foundations

These are not all video papers, but they are essential background for understanding why video reward models look the way they do.

### Image reward models and preference data

- [ImageReward: Learning and Evaluating Human Preferences for Text-to-Image Generation](https://arxiv.org/abs/2304.05977) *(2023)* — Canonical learned reward model for text-to-image generation.
- [Human Preference Score v2: A Solid Benchmark for Evaluating Human Preferences of Text-to-Image Synthesis](https://arxiv.org/abs/2306.09341) *(2023)* — Strong image preference benchmark and scorer; often used as a baseline or inherited signal.
- [Pick-a-Pic: An Open Dataset of User Preferences for Text-to-Image Generation](https://arxiv.org/abs/2305.01569) *(2023)* — Open image preference dataset and the basis for PickScore.

### Reward modeling, RLHF, and DPO foundations for diffusion

- [Aligning Text-to-Image Models using Human Feedback](https://arxiv.org/abs/2302.12192) *(2023)* — Early demonstration that human feedback can align diffusion models.
- [Aligning Text-to-Image Diffusion Models with Reward Backpropagation](https://arxiv.org/abs/2310.03739) *(2023)* — AlignProp; direct backpropagation through diffusion against differentiable rewards.
- [Directly Fine-Tuning Diffusion Models on Differentiable Rewards](https://arxiv.org/abs/2309.17400) *(2023)* — DRaFT; a general recipe for direct reward optimization.
- [Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/abs/2305.18290) *(2023)* — Core preference-optimization formulation later reused by video work.
- [Diffusion Model Alignment Using Direct Preference Optimization](https://arxiv.org/abs/2311.12908) *(2024)* — The most direct image-to-video bridge for diffusion DPO.
- [Using Human Feedback to Fine-tune Diffusion Models without Any Reward Model](https://arxiv.org/abs/2311.13231) *(2023)* — D3PO-style alternative to explicit reward fitting.

### Adjacent video quality assessment foundations

- [FAST-VQA: Efficient End-to-end Video Quality Assessment with Fragment Sampling](https://arxiv.org/abs/2207.02595) *(2022)* — Important no-reference VQA backbone and efficiency design.
- [Exploring Video Quality Assessment on User Generated Contents from Aesthetic and Technical Perspectives](https://arxiv.org/abs/2211.04894) *(2022)* — DOVER; influential decomposition of video quality into aesthetic and technical views.

## General-Purpose Video Reward Models and Judges

Evaluator-first papers that either are used directly as reward functions or define the main benchmark landscape.

### Early generation-specific evaluators

- [Subjective-Aligned Dataset and Metric for Text-to-Video Quality Assessment](https://arxiv.org/abs/2403.11956) *(2024)* — T2VQA-DB plus a subjective-aligned T2V evaluator.
- [VideoScore: Building Automatic Metrics to Simulate Fine-Grained Human Feedback for Video Generation](https://arxiv.org/abs/2406.15252) *(2024)* — Open multi-aspect evaluator trained on VideoFeedback.
- [Benchmarking Multi-dimensional AIGC Video Quality Assessment: A Dataset and Unified Model](https://arxiv.org/abs/2407.21408) *(2024)* — LGVQ and UGVQ; decomposes spatial quality, temporal quality, and text alignment.
- [Towards A Better Metric for Text-to-Video Generation](https://arxiv.org/abs/2401.07781) *(2024)* — T2VScore; decomposes evaluation into text-video alignment and mixture-of-experts video quality, calibrated on human judgments over generated videos.
- [Boosting Text-to-Video Generative Model with MLLMs Feedback](https://proceedings.neurips.cc/paper_files/paper/2024/hash/fbe2b2f74a2ece8070d8fb073717bda6-Abstract-Conference.html) *(2024)* — VideoRM, an early general-purpose text-to-video preference reward model trained on VideoPrefer, a 135K MLLM-annotated video-preference dataset.

### Large-scale and fine-grained judges

- [AIGV-Assessor: Benchmarking and Evaluating the Perceptual Quality of Text-to-Video Generation with LMM](https://arxiv.org/abs/2411.17221) *(2024)* — Large LMM-based judge trained on AIGVQA-DB.
- [VisionReward: Fine-Grained Multi-Dimensional Human Preference Learning for Image and Video Generation](https://arxiv.org/abs/2412.21059) *(2024)* — Interpretable reward built from many binary judgment questions.
- [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) *(2025)* — VideoReward, a strong open VLM-based video reward model with a dedicated leaderboard.
- [MJ-VIDEO: Fine-Grained Benchmarking and Rewarding Video Preferences in Video Generation](https://arxiv.org/abs/2502.01719) *(2025)* — Aspect-routed MoE judge for criterion-level preference prediction.
- [Content-Rich AIGC Video Quality Assessment via Intricate Text Alignment and Motion-Aware Consistency](https://arxiv.org/abs/2502.04076) *(2025)* — CRAVE; tailored to long prompts and next-generation content-rich videos.
- [T2VEval: T2V-generated Videos Benchmark Dataset and Objective Evaluation Method](https://arxiv.org/abs/2501.08545) *(2025)* — Multi-branch evaluator for consistency, realness, and technical quality.
- [Q-Eval-100K: Evaluating Visual Quality and Alignment Level for Text-to-Vision Content](https://arxiv.org/abs/2503.02357) *(2025)* — Large MOS dataset and judge model spanning both image and video.
- [LOVE: Benchmarking and Evaluating Text-to-Video Generation and Video-to-Text Interpretation](https://arxiv.org/abs/2505.12098) *(2025)* — LMM-based evaluator with AIGVE-60K and bidirectional T2V / V2T benchmarking.
- [Unified Reward Model for Multimodal Understanding and Generation](https://arxiv.org/abs/2503.05236) *(2025)* — Joint reward modeling across understanding and generation tasks, including video.
- [VideoScore2: Think before You Score in Generative Video Evaluation](https://arxiv.org/abs/2509.22799) *(2025)* — Reasoning-based, interpretable multi-dimensional judge.
- [VR-Thinker: Boosting Video Reward Models through Thinking-with-Image Reasoning](https://arxiv.org/abs/2510.10518) *(2025)* — Memory-based visual reasoning for stronger reward prediction.
- [SoliReward: Mitigating Susceptibility to Reward Hacking and Annotation Noise in Video Generation Reward Models](https://arxiv.org/abs/2512.22170) *(2025)* — VLM-based video reward model hardened against reward hacking and annotation noise via cross-prompt binary annotations and a win-tie Bradley-Terry objective.
- [AesRM: Improving Video Aesthetics with Expert-Level Feedback](https://arxiv.org/abs/2604.28078) *(2026)* — Video aesthetic reward models (base + chain-of-thought variants) trained on expert-annotated preferences with a dedicated benchmark; applied to SFT, GRPO, and process rewards.
- [GT-SVJ: Generative-Transformer-Based Self-Supervised Video Judge For Efficient Video Reward Modeling](https://arxiv.org/abs/2602.05202) *(2026)* — Repurposes a video generator as an energy-based judge (low energy for high-quality videos), trained self-supervised on synthetic degraded videos instead of a VLM.
- [VQ-Insight: Teaching VLMs for AI-Generated Video Quality Understanding via Progressive Visual Reinforcement Learning](https://arxiv.org/abs/2506.18564) *(2025)* — Trains a reasoning VLM quality judge through progressive reinforcement learning with multi-dimension scoring, preference, and temporal rewards.
- [Unified Personalized Reward Model for Vision Generation](https://arxiv.org/abs/2602.02380) *(2026)* — UnifiedReward-Flex; grounds scores in visual evidence and dynamically instantiates fine-grained hierarchical criteria for image and video generation.
- [Omni-Judge: Can Omni-LLMs Serve as Human-Aligned Judges for Text-Conditioned Audio-Video Generation?](https://arxiv.org/abs/2602.01623) *(2026)* — Probes omni-modal LLMs as chain-of-thought judges scoring text-conditioned audio-video generation across perceptual and cross-modal alignment dimensions.
- [Comparison Drives Preference: Reference-Aware Modeling for AI-Generated Video Quality Assessment](https://arxiv.org/abs/2604.17074) *(2026)* — RefVQA; reformulates AI-generated video quality assessment as reference-graph comparison, aggregating graph-guided differences from related videos to the query.
- [EvalVerse: Pipeline-Aware and Expert-Calibrated Benchmarking for Professional Cinematic Video Generation](https://arxiv.org/abs/2605.23271) *(2026)* — Fine-tunes a cinematic video judge from expert-calibrated pairwise preferences over a filmmaking-workflow taxonomy (multi-shot composition, cinematography, motion, audio-visual production).

## Preference Optimization and Post-Training

Papers whose main contribution is using reward or preference signals to align a video generator.

### Reward-weighted or gradient-based finetuning

- [InstructVideo: Instructing Video Diffusion Models with Human Feedback](https://arxiv.org/abs/2312.12490) *(2023)* — Uses transferred image-side rewards for RLHF-style video alignment.
- [T2V-Turbo: Breaking the Quality Bottleneck of Video Consistency Model with Mixed Reward Feedback](https://arxiv.org/abs/2405.18750) *(2024)* — Mixed reward feedback for fast few-step video generation.
- [T2V-Turbo-v2: Enhancing Video Generation Model Post-Training through Data, Reward, and Conditional Guidance Design](https://arxiv.org/abs/2410.05677) *(2025)* — Broader post-training stack combining reward, data, and guidance design.
- [Video Diffusion Alignment via Reward Gradients](https://arxiv.org/abs/2407.08737) *(2024)* — Direct reward backpropagation for video diffusion.
- [Improving Dynamic Object Interactions in Text-to-Video Generation with AI Feedback](https://arxiv.org/abs/2412.02617) *(2024)* — Uses AI feedback to improve difficult object interactions and physical motion.
- [LiFT: Leveraging Human Feedback for Text-to-Video Model Alignment](https://arxiv.org/abs/2412.04814) *(2024)* — Human ratings + rationales for reward-based alignment.
- [Prompt-A-Video: Prompt Your Video Diffusion Model via Preference-Aligned LLM](https://arxiv.org/abs/2412.15156) *(2024)* — Reward-guided prompt evolution rather than direct generator alignment.
- [Reward Forcing: Efficient Streaming Video Generation with Rewarded Distribution Matching Distillation](https://arxiv.org/abs/2512.04678) *(2025)* — Reward-guided distribution-matching distillation for streaming video generation, using a VLM reward to bias the few-step student toward high-motion samples.
- [Reward-Forcing: Autoregressive Video Generation with Reward Feedback](https://arxiv.org/abs/2601.16933) *(2026)* — Brings reward-guided optimization to autoregressive video generation.
- [GigaVideo-1: Advancing Video Generation via Automatic Feedback with 4 GPU-Hours Fine-Tuning](https://arxiv.org/abs/2506.10639) *(2025)* — Reward-guided fine-tuning that adaptively weights training samples using frozen VLM feedback under a realism constraint, without human annotation.
- [Seedance 1.0: Exploring the Boundaries of Video Generation Models](https://arxiv.org/abs/2506.09113) *(2025)* — Trains three dedicated reward models (VLM-based foundational, latent-space motion, keyframe aesthetic) and aligns the generator by backpropagating their composite reward through the predicted clean video, outperforming DPO/PPO/GRPO in their ablations.
- [Seedance 1.5 pro: A Native Audio-Visual Joint Generation Foundation Model](https://arxiv.org/abs/2512.13507) *(2025)* — Extends Seedance's reward-feedback alignment to joint audio-video generation, training three RLHF reward models (audio-video alignment, motion dynamics, aesthetics) and maximizing their aggregate score across T2V/T2VA/I2VA tasks.
- [Video Consistency Distance: Enhancing Temporal Consistency for Image-to-Video Generation via Reward-Based Fine-Tuning](https://arxiv.org/abs/2510.19193) *(2025)* — Frequency-domain temporal-consistency reward backpropagated to fine-tune image-to-video diffusion models.
- [SHIFT: Motion Alignment in Video Diffusion Models with Adversarial Hybrid Fine-Tuning](https://arxiv.org/abs/2603.17426) *(2026)* — Pixel-flux motion rewards drive an advantage-weighted fine-tuning that unifies supervised and reward objectives, using adversarial advantages to curb reward hacking.
- [Reward Lightning: Fast Video Generation via Homologous Preference Distillation](https://arxiv.org/abs/2607.03960) *(2026)* — Latent-space reward model shares its backbone with adversarial distillation to align and accelerate a few-step video generator jointly.
- [HunyuanVideo 1.5 Technical Report](https://arxiv.org/abs/2511.18870) *(2025)* — Post-training trains its own VLM-based video reward model scoring text alignment, image alignment, visual quality, and motion, then applies online RL (plus DPO for text-to-video).
- [Waver: Wave Your Way to Lifelike Video Generation](https://arxiv.org/abs/2508.15761) *(2025)* — SFTs a VideoLLaMA3 quality model predicting an overall label plus 13 defect dimensions, used to filter the generator's training corpus.
- [Seaweed-7B: Cost-Effective Training of Video Generation Foundation Model](https://arxiv.org/abs/2504.08685) *(2025)* — Trains a visual-quality and artifact-classifier model for data filtering, plus Video-DPO on annotator best/worst picks from four generations per prompt.
- [LTX-Video: Realtime Video Latent Diffusion](https://arxiv.org/abs/2501.00103) *(2025)* — Trains a Siamese aesthetic ranker on human-tagged pairs and uses its scores to filter low-aesthetic clips from the video and image training corpus.
- [MagicPrompt: Ultra-Lightweight Prompt Tuning for Video Generation](https://arxiv.org/abs/2607.14595) *(2026)* — Reward-guided soft-prompt tuning driven by a dual-space reward: a pixel reward mixing HPS with an optical-flow motion-consistency score, plus a CFG-vs-unconditional latent regularizer.

### DPO / IPO / GRPO and related preference optimization

- [VideoDPO: Omni-Preference Alignment for Video Diffusion Generation](https://arxiv.org/abs/2412.14167) *(2024)* — Baseline DPO adaptation for video diffusion.
- [OnlineVPO: Align Video Diffusion Model with Online Video-Centric Preference Optimization](https://arxiv.org/abs/2412.15159) *(2024)* — Online preference optimization with refreshed comparisons.
- [HuViDPO: Enhancing Video Generation through Direct Preference Optimization for Human-Centric Alignment](https://arxiv.org/abs/2502.01690) *(2025)* — Human-centric specialization of DPO.
- [Dual-IPO: Dual-Iterative Preference Optimization for Text-to-Video Generation](https://arxiv.org/abs/2502.02088) *(2025)* — Co-evolves generator and reward model iteratively.
- [Discriminator-Free Direct Preference Optimization for Video Diffusion](https://arxiv.org/abs/2504.08542) *(2025)* — Uses real-vs-distorted or real-vs-generated comparisons without a separate discriminator. *(arXiv admin note: text overlap with VideoDPO, [arXiv:2412.14167](https://arxiv.org/abs/2412.14167), by other authors.)*
- [DenseDPO: Fine-Grained Temporal Preference Optimization for Video Diffusion Models](https://arxiv.org/abs/2506.03517) *(2025)* — Dense temporal labeling instead of one judgment per full video.
- [Reg-DPO: SFT-Regularized Direct Preference Optimization with GT-Pair for Improving Video Generation](https://arxiv.org/abs/2511.01450) *(2025)* — **⚠️ Withdrawn on arXiv (authors: results require further verification); retained for completeness, not recommended.** Proposed ground-truth-positive pairs and SFT regularization for stability.
- [DanceGRPO: Unleashing GRPO on Visual Generation](https://arxiv.org/abs/2505.07818) *(2025)* — General online RL / GRPO framework applied to T2V and I2V.
- [Rethinking Reward Signals in Video GRPO: When Scores Become Targets](https://arxiv.org/abs/2511.19356) *(2025)* — TaRoS; diagnoses reward saturation, component shortcuts, and within-group ranking collapse (Goodhart) in video GRPO, and adaptively downweights saturated reward components.
- [V.I.P.: Iterative Online Preference Distillation for Efficient Video Diffusion Models](https://arxiv.org/abs/2508.03254) *(2025)* — Online preference distillation for efficient student models.
- [SIPO: Stabilized and Improved Preference Optimization for Aligning Diffusion Models](https://arxiv.org/abs/2505.21893) *(2025)* — Clips and masks uninformative timesteps with importance reweighting to stabilize diffusion-DPO on video generators.
- [VPO: Aligning Text-to-Video Generation Models with Prompt Optimization](https://arxiv.org/abs/2503.20491) *(2025)* — DPO-trained prompt rewriter whose preference pairs come from a video reward model scoring generated videos, adding safety and quality feedback.
- [McSc: Motion-Corrective Preference Alignment for Video Generation with Self-Critic Hierarchical Reasoning](https://arxiv.org/abs/2511.22974) *(2025)* — Self-critic hierarchical-reasoning reward model feeds a motion-corrective DPO that counters the bias toward low-motion clips.
- [Beyond Reward Margin: Rethinking and Resolving Likelihood Displacement in Diffusion Models via Video Generation](https://arxiv.org/abs/2511.19049) *(2025)* — Policy-guided DPO with adaptive rejection scaling and implicit preference regularization to stop chosen-sample likelihoods from dropping.
- [TAGRPO: Boosting GRPO on Image-to-Video Generation with Direct Trajectory Alignment](https://arxiv.org/abs/2601.05729) *(2026)* — GRPO loss on intermediate latents pulls rollouts toward high-reward trajectories that share the same initial noise.
- [TeleBoost: A Systematic Alignment Framework for High-Fidelity, Controllable, and Robust Video Generation](https://arxiv.org/abs/2602.07595) *(2026)* — Staged post-training framework that fuses heterogeneous, weakly-discriminative reward signals into a stability-constrained GRPO-and-DPO optimization stack.
- [SPATIALALIGN: Aligning Dynamic Spatial Relationships in Video Generation](https://arxiv.org/abs/2602.22745) *(2026)* — Zeroth-order regularized DPO driven by a geometry-based DSR-SCORE that quantifies dynamic spatial-relationship alignment instead of relying on a VLM judge.
- [A Systematic Post-Train Framework for Video Generation](https://arxiv.org/abs/2604.25427) *(2026)* — Four-stage post-training whose RLHF stage adapts Flow-GRPO to video diffusion with dedicated reward models scoring perceptual quality and temporal coherence.
- [Flash-GRPO: Efficient Alignment for Video Diffusion via One-Step Policy Optimization](https://arxiv.org/abs/2605.15980) *(2026)* — Single-step GRPO with iso-temporal grouping and temporal gradient rectification to slash alignment-training cost.
- [Step-Video-T2V Technical Report: The Practice, Challenges, and Future of Video Foundation Model](https://arxiv.org/abs/2502.10248) *(2025)* — Video-DPO starts from annotator-ranked preference pairs, then trains an in-house human-feedback reward model that scores on-policy samples online and is periodically refreshed.
- [LongCat-Video Technical Report](https://arxiv.org/abs/2510.22200) *(2025)* — Multi-reward GRPO combining two in-house VideoAlign-based reward models (a grayscale motion-quality RM to suppress appearance shortcuts and a text-alignment RM) with external HPSv3.
- [Kling-Omni Technical Report](https://arxiv.org/abs/2512.16776) *(2025)* — Multi-round DPO on human preference pairs over sampled video variants, targeting motion dynamics and visual integrity without trajectory sampling.
- [Alice v1: Distillation-Enhanced Video Generation Surpassing Closed-Source Models](https://arxiv.org/abs/2605.08115) *(2026)* — Trains generation-specific verifiers (ViT physics-violation classifier, hand-quality classifier, identity and optical-flow checks) for hard-example mining, then DPO on ~10K human A/B video-preference pairs.
- [Diffusion-APO: Trajectory-Aware Direct Preference Alignment for Video Diffusion Transformers](https://arxiv.org/abs/2605.07503) *(2026)* — Trajectory-aware DPO that builds preference pairs online from a pairwise video-ranking oracle during alignment.
- [RealDPO: Real or Not Real, that is the Preference](https://arxiv.org/abs/2510.14955) *(2025)* — Reward-model-free DPO using real-action clips as chosen and model generations as rejected, with a tailored loss and the RealAction-5K dataset.

## Localized, Structured, and Reasoning-Based Rewards

Reward designs that try to move past a single opaque scalar score.

### Structured and decomposed rewards

- [VisionReward: Fine-Grained Multi-Dimensional Human Preference Learning for Image and Video Generation](https://arxiv.org/abs/2412.21059) *(2024)* — Decomposes reward into multiple dimensions and binary judgment questions.
- [MJ-VIDEO: Fine-Grained Benchmarking and Rewarding Video Preferences in Video Generation](https://arxiv.org/abs/2502.01719) *(2025)* — MoE-based aspect routing over five aspects and 28 criteria.
- [Harness Local Rewards for Global Benefits: Effective Text-to-Video Generation Alignment with Patch-level Reward Models](https://arxiv.org/abs/2502.06812) *(2025)* — Local patch-level rewards for fine defects that global reward misses.
- [DenseDPO: Fine-Grained Temporal Preference Optimization for Video Diffusion Models](https://arxiv.org/abs/2506.03517) *(2025)* — Segment-level preference supervision over time.
- [RewardDance: Reward Scaling in Visual Generation](https://arxiv.org/abs/2509.08826) *(2025)* — Studies reward scaling and judge robustness with larger generative VLMs.
- [Wan-R1: Verifiable-Reinforcement Learning for Video Reasoning](https://arxiv.org/abs/2603.27866) *(2026)* — Task-verifiable decomposed rewards (agent-trajectory scoring for game videos; embedding-level frame-similarity, ordering, and endpoint verifiers for robot navigation) driving Flow-GRPO, with an analysis of VLM reward hacking.
- [Video Models Can Reason with Verifiable Rewards](https://arxiv.org/abs/2605.15458) *(2026)* — Recasts video reasoning as verifiable visual trajectories with dense, decomposed rule-based verifiers (Maze, FlowFree, Sokoban) optimized via SDE-GRPO.

### Localized and detail-aware optimization

- [Mind the Generative Details: Direct Localized Detail Preference Optimization for Video Diffusion Models](https://arxiv.org/abs/2601.04068) *(2026)* — Localized detail-aware DPO with region-sensitive comparisons.
- [CaC: Advancing Video Reward Models via Hierarchical Spatiotemporal Concentrating](https://arxiv.org/abs/2605.11723) *(2026)* — Coarse-to-fine spatiotemporal concentrating (temporal scan then spatial grounding) for localized, anomaly-aware video reward modeling.
- [Stream-R1: Reliability-Perplexity Aware Reward Distillation for Streaming Video Generation](https://arxiv.org/abs/2605.03849) *(2026)* — Reward distillation that reweights student rollouts by a video reward score and backpropagates it for per-pixel spatio-temporal gradient saliency.

### Reasoning-based judges

- [VideoScore2: Think before You Score in Generative Video Evaluation](https://arxiv.org/abs/2509.22799) *(2025)* — Chain-of-thought reward modeling with explicit sub-scores.
- [VR-Thinker: Boosting Video Reward Models through Thinking-with-Image Reasoning](https://arxiv.org/abs/2510.10518) *(2025)* — Frame selection, visual memory, and reasoning supervision.
- [Thinking with Frames: Generative Video Distortion Evaluation via Frame Reward Model](https://arxiv.org/abs/2601.04033) *(2026)* — REACT; frame-level reasoning model for structural distortions.
- [FingER: Content Aware Fine-grained Evaluation with Reasoning for AI-Generated Videos](https://arxiv.org/abs/2504.10358) *(2025)* — Entity-level reasoning judge that auto-generates fine-grained QA and answers it with a GRPO-trained MLLM for interpretable scoring.
- [Think, then Score: Decoupled Reasoning and Scoring for Video Reward Modeling](https://arxiv.org/abs/2605.05922) *(2026)* — DeScore; decouples chain-of-thought reasoning from a discriminative scoring head to keep reasoning generalization while avoiding coupled reason-and-score instability.
- [Plan-and-Verify Video Reward Reasoning with Spatio-Temporal Scene Graph Grounding](https://arxiv.org/abs/2606.11838) *(2026)* — SG-PVR; decomposes the prompt into atomic claims verified against a persistent spatio-temporal scene graph for grounded reranking.
- [Reward as An Agent for Embodied World Models](https://arxiv.org/abs/2606.19990) *(2026)* — Replaces a fixed scalar judge with a structured reward agent that plans criteria and evaluates quality, instruction following, physics, and task completion (with voting and reflection) to feed DynDiff-GRPO.

### Training-free and decomposed critics

- [Diffusion-DRF: Free, Rich, and Differentiable Reward for Video Diffusion Fine-Tuning](https://arxiv.org/abs/2601.04153) *(2026)* — Reward-model-training-free (a frozen VLM supplies the dense signal) but still fine-tunes the generator; decomposes each prompt into freeform dense VQA questions.
- [Human detectors are surprisingly powerful reward models](https://arxiv.org/abs/2601.14037) *(2026)* — HuDA; specialized human detectors as simple but strong reward models.
- [Your Data Manifold is Secretly a Reward Model: Shell-LCC for Text-to-Video Generation](https://arxiv.org/abs/2606.30248) *(2026)* — Reward-model-training-free (the data manifold itself supplies a dense differentiable reward), modeling the manifold surface as an isotropic shell to preserve detail.

## Physics, World, and Geometry Rewards

Reward papers where “better” means more physically or geometrically consistent.

### Benchmarks and problem framing

- [Evaluating Physical Commonsense for Video Generation](https://arxiv.org/abs/2406.03520) *(2024)* — VideoPhy; benchmark and evaluator for physical commonsense in generated videos.
- [VideoPhy-2: A Challenging Action-Centric Physical Commonsense Evaluation in Video Generation](https://arxiv.org/abs/2503.06800) *(2025)* — Stronger action-centric physics benchmark with human and automatic evaluation.
- [PISA Experiments: Exploring Physics Post-Training for Video Diffusion Models by Watching Stuff Drop](https://arxiv.org/abs/2503.09595) *(2025)* — Free-fall as a controlled testbed for physics-aware reward design.
- [WorldModelBench: Judging Video Generation Models As World Models](https://arxiv.org/abs/2502.20694) *(2025)* — Physics-adherence and instruction-following benchmark that also fine-tunes a reusable learned judge whose rewards improve world-modeling generators.
- [Do Joint Audio-Video Generation Models Understand Physics?](https://arxiv.org/abs/2605.07061) *(2026)* — AV-Phys Bench; probes visual, audio, and cross-modal physical commonsense of joint audio-video generators across steady-state, transition, and adversarial prompts.

### Physics-aware optimization

- [Think Before You Diffuse: LLMs-Guided Physics-Aware Video Generation](https://arxiv.org/abs/2505.21653) *(2025)* — Uses LLM/VLM reasoning to supervise physical plausibility.
- [Hierarchical Fine-grained Preference Optimization for Physically Plausible Video Generation](https://arxiv.org/abs/2508.10858) *(2025)* — PhysHPO; hierarchical preference signals over state, motion, and semantics.
- [PhysCorr: Dual-Reward DPO for Physics-Constrained Text-to-Video Generation with Automated Preference Selection](https://arxiv.org/abs/2511.03997) *(2025)* — Dual physics rewards for object stability and interaction.
- [PhyGDPO: Physics-Aware Groupwise Direct Preference Optimization for Physically Consistent Text-to-Video Generation](https://arxiv.org/abs/2512.24551) *(2025)* — Groupwise preference optimization with physics-guided rewards.
- [PhysRVG: Physics-Aware Unified Reinforcement Learning for Video Generative Models](https://arxiv.org/abs/2601.11087) *(2026)* — Unified RL treatment of physics constraints for video generation.
- [PhyPrompt: RL-based Prompt Refinement for Physically Plausible Text-to-Video Generation](https://arxiv.org/abs/2603.03505) *(2026)* — GRPO-refined prompt LM trained with a dynamic reward curriculum that shifts from semantic fidelity toward physical commonsense scored on generated videos.
- [What about gravity in video generation? Post-Training Newton's Laws with Verifiable Rewards](https://arxiv.org/abs/2512.00425) *(2025)* — NewtonRewards; verifiable kinematic and mass-conservation rewards using optical flow as a velocity proxy and appearance features as a mass proxy.
- [RDPO: Real Data Preference Optimization for Physics Consistency Video Generation](https://arxiv.org/abs/2506.18655) *(2025)* — Annotation-free Flow-DPO that reverse-samples real videos into physics-preferred positives paired against generated negatives.

### World and geometry critics

- [Inference-time Physics Alignment of Video Generative Models with Latent World Models](https://arxiv.org/abs/2601.10553) *(2026)* — WMReward; latent world model as inference-time critic.
- [VIGOR: VIdeo Geometry-Oriented Reward for Temporal Generative Alignment](https://arxiv.org/abs/2603.16271) *(2026)* — Geometry-based reward using reprojection consistency.
- [VGGRPO: Towards World-Consistent Video Generation with 4D Latent Reward](https://arxiv.org/abs/2603.26599) *(2026)* — 4D latent geometry reward plus GRPO.
- [CamPilot: Improving Camera Control in Video Diffusion Model with Efficient Camera Reward Feedback](https://arxiv.org/abs/2601.16214) *(2026)* — Camera-consistency reward for controllable motion and view planning.
- [GeoFlow: Enforcing Implicit Geometric Consistency in Video Generation](https://arxiv.org/abs/2605.18365) *(2026)* — Geometry-consistency reward separating rigid camera-induced flow from dynamic object motion, optimized via RL fine-tuning.
- [ReWorld: Multi-Dimensional Reward Modeling for Embodied World Models](https://arxiv.org/abs/2601.12428) *(2026)* — Trains a hierarchical multi-head reward model on ~235K embodied-video preference pairs (physical realism, task completion, embodiment, visual quality) to align world-model generators via RL.
- [VideoGPA: Distilling Geometry Priors for 3D-Consistent Video Generation](https://arxiv.org/abs/2601.23286) *(2026)* — Turns discrepancy against a geometry foundation model into dense self-supervised geometric-consistency preferences that drive DPO, with no human annotation.
- [ABot-3DWorld 0: A Universal World Model to Explore Any 3D Space](https://arxiv.org/abs/2607.11673) *(2026)* — Post-trains a panoramic video generator with a forward-backward optical-flow cycle-consistency reward plus an LPIPS anti-collapse term.

## Editing, Identity, Camera, and Domain-Specific Rewards

Reward models are still sparse outside generic T2V. These papers are especially useful if you work on specialized generation settings.

### Video editing

- [TDVE-Assessor: Benchmarking and Evaluating the Quality of Text-Driven Video Editing with LMMs](https://arxiv.org/abs/2505.19535) *(2025)* — Benchmark and judge model for text-driven video editing.
- [Prompt-A-Video: Prompt Your Video Diffusion Model via Preference-Aligned LLM](https://arxiv.org/abs/2412.15156) *(2024)* — Upstream prompt rewriting using preference signals.
- [VIVA: VLM-Guided Instruction-Based Video Editing with Reward Optimization](https://arxiv.org/abs/2512.16906) *(2025)* — Edit-GRPO post-training with VLM-guided relative rewards for instruction-faithful, content-preserving video editing.

### Identity preservation

- [Identity-Preserving Image-to-Video Generation via Reward-Guided Optimization](https://arxiv.org/abs/2510.14255) *(2025)* — Facial identity reward for I2V generation.
- [Identity-GRPO: Optimizing Multi-Human Identity-preserving Video Generation via Reinforcement Learning](https://arxiv.org/abs/2510.14256) *(2025)* — Identity-aware reward model and GRPO for multi-person videos.
- [HuViDPO: Enhancing Video Generation through Direct Preference Optimization for Human-Centric Alignment](https://arxiv.org/abs/2502.01690) *(2025)* — Human-centric alignment setting.

### Camera control

- [CamPilot: Improving Camera Control in Video Diffusion Model with Efficient Camera Reward Feedback](https://arxiv.org/abs/2601.16214) *(2026)* — Camera-control-specific reward modeling.
- [Geo-Align: Video Generation Alignment via Metric Geometry Reward](https://arxiv.org/abs/2605.23903) *(2026)* — Metric-3D geometry reward extracting camera trajectories from generated videos and penalizing rotation and translation deviations for RL-based camera control.
- [VERTIGO: Visual Preference Optimization for Cinematic Camera Trajectory Generation](https://arxiv.org/abs/2604.02467) *(2026)* — DPO on a camera-trajectory generator with VLM preferences scored on rendered previews to improve camera-controlled video pipelines.
- [Taming Camera-Controlled Video Generation with Verifiable Geometry Reward](https://arxiv.org/abs/2512.02870) *(2025)* — CamVerse; estimates generated-vs-reference 3D camera trajectories and compares segment-wise relative poses for a dense verifiable camera-motion reward optimized with GRPO.

### Domain-specific alignment

- [Aligning Anime Video Generation with Human Feedback](https://arxiv.org/abs/2504.10044) *(2025)* — Domain-specific preference learning for anime video generation.
- [EVA: Aligning Video World Models with Executable Robot Actions via Inverse Dynamics Rewards](https://arxiv.org/abs/2603.17808) *(2026)* — Uses an inverse-dynamics model as an executability reward to align generated robot videos with physically feasible actions (RL post-training).
- [AlignHuman: Improving Motion and Fidelity via Timestep-Segment Preference Optimization for Audio-Driven Human Animation](https://arxiv.org/abs/2506.11144) *(2025)* — Timestep-segment preference optimization that trains separate motion- and fidelity-expert LoRAs active in their own denoising intervals.
- [Hallo4: High-Fidelity Dynamic Portrait Animation via Direct Preference Optimization](https://arxiv.org/abs/2505.23525) *(2025)* — Direct preference optimization on a curated human-preference dataset to align lip-sync and expression naturalness in portrait animation.
- [FantasyTalking2: Timestep-Layer Adaptive Preference Optimization for Audio-Driven Portrait Animation](https://arxiv.org/abs/2508.11255) *(2025)* — Talking-Critic reward model plus timestep-layer adaptive preference optimization that fuses decoupled preference experts across denoising steps and network layers.
- [Hallo-Live: Real-Time Streaming Joint Audio-Video Avatar Generation with Asynchronous Dual-Stream and Human-Centric Preference Distillation](https://arxiv.org/abs/2604.23632) *(2026)* — Reward-reweighted distribution-matching distillation (HP-DMD) that tilts a streaming audio-video avatar generator toward human-preferred fidelity and synchronization.
- [AniMatrix: An Anime Video Generation Model that Thinks in Art, Not Physics](https://arxiv.org/abs/2605.03652) *(2026)* — Deformation-aware preference optimization with an anime-specific reward model that separates intentional artistry from pathological collapse.
- [LongCat-Video-Avatar 1.5 Technical Report](https://arxiv.org/abs/2605.26486) *(2026)* — Extends LongCat's multi-reward GRPO with per-frame and temporal-partitioned rewards that localize motion inconsistency, hand deformation, and structural collapse in avatar generation.

## Inference-Time Reward, Search, and Process Rewards

Not all reward models are only for post-training; some are most useful at test time or over latent trajectories.

### Reward-guided inference

- [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) *(2025)* — Includes Flow-NRG, an inference-time reward-guided generation strategy.
- [DOLLAR: Few-Step Video Generation via Distillation and Latent Reward Optimization](https://arxiv.org/abs/2412.15689) *(2024)* — Latent reward optimization for efficient few-step video generation.
- [Inference-time Physics Alignment of Video Generative Models with Latent World Models](https://arxiv.org/abs/2601.10553) *(2026)* — WMReward as a pure inference-time verifier.
- [VIGOR: VIdeo Geometry-Oriented Reward for Temporal Generative Alignment](https://arxiv.org/abs/2603.16271) *(2026)* — Can be used for both post-training and inference-time selection.
- [PISCES: Annotation-free Text-to-Video Post-Training via Optimal Transport-Aligned Rewards](https://arxiv.org/abs/2602.01624) *(2026)* — Annotation-free **post-training** (direct reward backprop and RL), not only inference-time; optimal-transport-aligned semantic and quality rewards.
- [Diffusion-DRF: Free, Rich, and Differentiable Reward for Video Diffusion Fine-Tuning](https://arxiv.org/abs/2601.04153) *(2026)* — Dense VLM supervision without collecting new preference labels.
- [Free²Guide: Training-Free Text-to-Video Alignment using Image LVLM](https://arxiv.org/abs/2411.17041) *(2024)* — Path-integral guidance that steers video diffusion sampling with non-differentiable image-LVLM alignment rewards, requiring no training or gradients.
- [InfLVG: Reinforce Inference-Time Consistent Long Video Generation with GRPO](https://arxiv.org/abs/2505.17574) *(2025)* — Composite verifier (ArcFace identity, CLIP frame-prompt alignment, Qwen2.5-VL mosaic/artifact detection) serves as the GRPO reward that trains a context-selection policy for consistent long-video inference.

### Latent search and process rewards

- [Video Generation Models Are Good Latent Reward Models](https://arxiv.org/abs/2511.21541) *(2025)* — Process Reward Feedback Learning (PRFL) in the generator’s latent space.
- [Euphonium: Steering Video Flow Matching via Process Reward Gradient Guided Stochastic Dynamics](https://arxiv.org/abs/2602.04928) *(2026)* — Couples process rewards with stochastic flow guidance.
- [LatSearch: Latent Reward-Guided Search for Faster Inference-Time Scaling in Video Diffusion](https://arxiv.org/abs/2603.14526) *(2026)* — Search and pruning in latent space using reward estimates.
- [Inference-Time Text-to-Video Alignment with Diffusion Latent Beam Search](https://arxiv.org/abs/2501.19252) *(2025)* — Beam search over diffusion latents with a lookahead estimator that selects trajectories maximizing a calibrated alignment reward.
- [ScalingNoise: Scaling Inference-Time Search for Generating Infinite Videos](https://arxiv.org/abs/2503.16400) *(2025)* — Reward model anchored on prior frames scores one-step-denoised candidates to select initial noises for consistent long-video generation.
- [Video-T1: Test-Time Scaling for Video Generation](https://arxiv.org/abs/2503.18942) *(2025)* — Recasts generation as trajectory search where test-time verifiers guide a Tree-of-Frames that expands and prunes autoregressive video branches.
- [Proprio: Latent Self-Scoring and Inference-Time Refinement for Physically Plausible Video Generation](https://arxiv.org/abs/2605.28230) *(2026)* — Uses flow residual under latent perturbations as a self-score for best-of-N selection and gradient-based refinement on a frozen generator.
- [Through the PRISM: Preference Representation in Intermediate States of Video Diffusion Models](https://arxiv.org/abs/2606.20310) *(2026)* — Trains a latent video reward model — a query-based head over a frozen diffusion backbone's noisy intermediate states — enabling noise-robust pre-decode Best-of-N selection.
- [TempAct: Advancing Temporal Plausibility in Autoregressive Video Generation via Planner-Executor RL](https://arxiv.org/abs/2606.28016) *(2026)* — Hierarchical planner-executor reward stack (LLM plan judge, full-video VLM evaluator, local transition rewards, PickScore) with group-based credit assignment across plan and segment levels.
- [Learning to Credit the Right Steps: Objective-aware Process Optimization for Visual Generation](https://arxiv.org/abs/2604.19234) *(2026)* — OTCA; decomposes coarse terminal video rewards into timestep-aware, per-objective credit along the denoising trajectory.

## Datasets and Benchmarks

### Human preference and MOS datasets

| Dataset / benchmark | What it contains | Primary paper |
|---|---|---|
| **T2VQA-DB** | Subjective ratings for T2V-generated videos from multiple models | [T2VQA](https://arxiv.org/abs/2403.11956) |
| **VideoFeedback** | Multi-aspect human scores over 37.6K synthesized videos | [VideoScore](https://arxiv.org/abs/2406.15252) |
| **LGVQ** | Spatial, temporal, and text-alignment labels for generated videos | [UGVQ](https://arxiv.org/abs/2407.21408) |
| **AIGVQA-DB** | Expert ratings across perceptual dimensions for T2V videos | [AIGV-Assessor](https://arxiv.org/abs/2411.17221) |
| **LiFT-HRA** | Human ratings and rationales for alignment | [LiFT](https://arxiv.org/abs/2412.04814) |
| **VideoGen-RewardBench** | Reward-model leaderboard and triplet preference benchmark | [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) |
| **MJ-BENCH-VIDEO** | Fine-grained pairwise preferences over 5 aspects and 28 criteria | [MJ-VIDEO](https://arxiv.org/abs/2502.01719) |
| **CRAVE-DB** | Content-rich videos with longer prompts and modern-model artifacts | [CRAVE](https://arxiv.org/abs/2502.04076) |
| **T2VEval-Bench** | Subjective ratings for T2V-generated videos | [T2VEval](https://arxiv.org/abs/2501.08545) |
| **Q-EVAL-100K** | Large MOS dataset over text-to-image and text-to-video content | [Q-Eval-100K](https://arxiv.org/abs/2503.02357) |
| **AIGVE-60K** | MOS + QA pairs over 58.5K generated videos | [LOVE](https://arxiv.org/abs/2505.12098) |
| **TDVE-DB** | Editing benchmark with human ratings for text-driven video editing | [TDVE-Assessor](https://arxiv.org/abs/2505.19535) |
| **VideoFeedback2** | Scores + reasoning traces for reward-model training | [VideoScore2](https://arxiv.org/abs/2509.22799) |
| **PhyVidGen-135K** | Physics-oriented synthetic preference data | [PhyGDPO](https://arxiv.org/abs/2512.24551) |
| **SafeSora** | 14.7K prompts, ~57K videos, ~51.7K human preference pairs over helpfulness + harmlessness (safety) | [SafeSora](https://arxiv.org/abs/2406.14477) |
| **T2V Ranking Human Preferences** | 2,009 prompts, 5 videos each from 18 T2V models, ~91K human rankings over visual quality, text-video alignment, and physical consistency | [Dataset](https://huggingface.co/datasets/datapointai/text-2-video-ranking-human-preferences) |
| **T2V Motion Human Preferences (v2 Large)** | 417 motion prompts across 11 categories, ~115.7K pairwise human labels over 4 T2V models on coherence, aesthetics, and prompt adherence | [Dataset](https://huggingface.co/datasets/datapointai/text-2-video-human-preferences-motion-v2-large) |
| **VideoPrefer** | 135K MLLM-annotated text-to-video preference pairs | [VideoRM](https://proceedings.neurips.cc/paper_files/paper/2024/hash/fbe2b2f74a2ece8070d8fb073717bda6-Abstract-Conference.html) |
| **ReWorld preferences** | ~235K embodied-video preference pairs over physical realism, task completion, embodiment, and visual quality | [ReWorld](https://arxiv.org/abs/2601.12428) |
| **HF-Embodied** | Fine-grained human-feedback scores and rationales for embodied world-model video | [WorldSimBench](https://arxiv.org/abs/2410.18072) |

### Reward-ready evaluation datasets and benchmarks

| Benchmark | Focus | Paper |
|---|---|---|
| **EvalCrafter** | Broad large-video-generation evaluation | [EvalCrafter](https://arxiv.org/abs/2310.11440) |
| **FETV** | Fine-grained open-domain text-to-video evaluation | [FETV](https://arxiv.org/abs/2311.01813) |
| **DEVIL** | Dynamics-specific evaluation | [DEVIL](https://arxiv.org/abs/2407.01094) |
| **VideoPhy** | Physical commonsense | [VideoPhy](https://arxiv.org/abs/2406.03520) |
| **VideoPhy-2** | Action-centric physical commonsense | [VideoPhy-2](https://arxiv.org/abs/2503.06800) |
| **WorldScore** | World consistency and next-scene generation | [WorldScore](https://arxiv.org/abs/2504.00983) |
| **Video-Bench** | Human-aligned MLLM-based evaluation of video generation | [Video-Bench](https://arxiv.org/abs/2504.04907) |
| **T2V-CompBench** | Compositional T2V across attribute binding, spatial/motion relations, actions, interactions, and numeracy, with MLLM/detection/tracking metrics | [T2V-CompBench](https://arxiv.org/abs/2407.14505) |
| **LoCoT2V-Bench** | Long-form, multi-scene T2V: perceptual, alignment, temporal, and dynamic quality plus human-expectation realization | [LoCoT2V-Bench](https://arxiv.org/abs/2510.26412) |
| **WorldRewardBench** | ~6K expert pairwise comparisons over ~1.4K videos for world-model reward-model evaluation | [WorldReasonBench](https://arxiv.org/abs/2605.10434) |
| **Step-Video-TI2V-Eval** | Text-and-image-to-video cases with pairwise Win/Tie/Loss human preference judgments | [Step-Video-TI2V](https://arxiv.org/abs/2503.11251) |
| **VGIF-Bench** | Spatio-temporal instruction following via ST-DAG QA and AutoRubric across 14 generators | [VGIF-Score](https://arxiv.org/abs/2607.13527) |

## Evaluation Suites and Stress Tests

These are especially useful for checking whether a reward model is merely in-domain calibrated or genuinely robust.

- [EvalCrafter: Benchmarking and Evaluating Large Video Generation Models](https://arxiv.org/abs/2310.11440) *(2023)* — General multi-aspect benchmark for large video generators.
- [FETV: A Benchmark for Fine-Grained Evaluation of Open-Domain Text-to-Video Generation](https://arxiv.org/abs/2311.01813) *(2023)* — Fine-grained prompt and evaluation design for open-domain T2V.
- [Evaluation of Text-to-Video Generation Models: A Dynamics Perspective](https://arxiv.org/abs/2407.01094) *(2024)* — DEVIL; dedicated stress test for dynamics and motion vividness.
- [Evaluating Physical Commonsense for Video Generation](https://arxiv.org/abs/2406.03520) *(2024)* — VideoPhy; one of the first strong physics-centric evaluation protocols.
- [VideoPhy-2: A Challenging Action-Centric Physical Commonsense Evaluation in Video Generation](https://arxiv.org/abs/2503.06800) *(2025)* — Stronger, action-centric follow-up benchmark.
- [WorldScore: A Unified Evaluation Benchmark for World Generation](https://arxiv.org/abs/2504.00983) *(2025)* — Multi-scene controllability, quality, and dynamics for world-consistent generation.
- [Video-Bench: Human-Aligned Video Generation Benchmark](https://arxiv.org/abs/2504.04907) *(2025)* — Systematic MLLM-based benchmark with strong human alignment.
- [LOVE: Benchmarking and Evaluating Text-to-Video Generation and Video-to-Text Interpretation](https://arxiv.org/abs/2505.12098) *(2025)* — Bidirectional benchmark for generation and interpretation.
- [HuM-Eval: A Coarse-to-Fine Framework for Human-Centric Video Evaluation](https://arxiv.org/abs/2604.25361) *(2026)* — Coarse-to-fine human-centric video evaluation that layers VLM quality scoring over pose-based anatomical and 3D-motion stability checks.
- [SLVMEval: Synthetic Meta Evaluation Benchmark for Text-to-Long Video Generation](https://arxiv.org/abs/2603.29186) *(2026)* — Meta-evaluates T2V judge and reward systems on long videos via synthetic degradation pairs across ten quality aspects.
- [SafeGen-Bench: Benchmarking Safety in Image-Conditioned Text-to-Video Generation](https://arxiv.org/abs/2606.01481) *(2026)* — Safety stress test for image-to-video generation that exposes guardrail brittleness and jailbreak bypass with a reusable VLM unsafety score.
- [VBench-2.0: Advancing Video Generation Benchmark Suite for Intrinsic Faithfulness](https://arxiv.org/abs/2503.21755) *(2025)* — Intrinsic-faithfulness suite (human fidelity, controllability, creativity, physics, commonsense) combining generalist VLMs with trained specialist anomaly detectors.
- [VideoWeaver: Evaluating and Evolving Skills for Agentic Long Video Generation](https://arxiv.org/abs/2606.08091) *(2026)* — Evidence-grounded agent-as-judge that inspects execution traces, intermediate artifacts, and final videos across 16 task categories, reporting process and output metrics.

## Project Pages, Repos, and Useful Links

### Reward models and judge repos

- **VideoScore** — [paper](https://arxiv.org/abs/2406.15252) · [repo](https://github.com/TIGER-AI-Lab/VideoScore) · [project page](https://tiger-ai-lab.github.io/VideoScore/)
- **VideoScore2** — [paper](https://arxiv.org/abs/2509.22799) · [repo](https://github.com/TIGER-AI-Lab/VideoScore2) · [project page](https://tiger-ai-lab.github.io/VideoScore2/)
- **VisionReward** — [paper](https://arxiv.org/abs/2412.21059) · [repo](https://github.com/zai-org/VisionReward) · [model](https://huggingface.co/zai-org/VisionReward-Video)
- **MJ-VIDEO / MJ-BENCH-VIDEO** — [paper](https://arxiv.org/abs/2502.01719) · [repo](https://github.com/aiming-lab/MJ-Video) · [dataset](https://huggingface.co/datasets/MJ-Bench/MJ-BENCH-VIDEO) · [project page](https://aiming-lab.github.io/MJ-VIDEO.github.io/)
- **VideoReward / VideoAlign** — [paper](https://arxiv.org/abs/2501.13918) · [repo](https://github.com/KlingAIResearch/VideoAlign) · [project page](https://gongyeliu.github.io/videoalign/) · [model](https://huggingface.co/KlingTeam/VideoReward)
- **AIGV-Assessor** — [paper](https://arxiv.org/abs/2411.17221) · [repo](https://github.com/IntMeGroup/AIGV-Assessor)
- **Q-Eval-100K** — [paper](https://arxiv.org/abs/2503.02357) · [repo](https://github.com/zzc-1998/q-eval)
- **Q-Eval-plus** — [repo](https://github.com/Q-Future/Q-Eval-plus)
- **LOVE** — [paper](https://arxiv.org/abs/2505.12098) · [repo](https://github.com/IntMeGroup/LOVE)
- **TDVE-Assessor** — [paper](https://arxiv.org/abs/2505.19535) · [repo](https://github.com/JuntongWang/TDVE-Assessor)
- **CRAVE** — [paper](https://arxiv.org/abs/2502.04076) · [repo](https://github.com/littlespray/crave)
- **UnifiedReward** — [paper](https://arxiv.org/abs/2503.05236) · [repo](https://github.com/CodeGoat24/UnifiedReward) · [project page](https://codegoat24.github.io/UnifiedReward/)

### Benchmarks, leaderboards, and evaluation toolkits

- **VideoGen-RewardBench** — [leaderboard](https://huggingface.co/spaces/KlingTeam/VideoGen-RewardBench)
- **VideoGen-Eval** — [repo](https://github.com/AILab-CVC/VideoGen-Eval)
- **VideoPhy / VideoPhy-2** — [paper](https://arxiv.org/abs/2406.03520) · [repo](https://github.com/Hritikbansal/videophy) · [project page](https://videophy2.github.io/)
- **WorldScore** — [paper](https://arxiv.org/abs/2504.00983) · [repo](https://github.com/haoyi-duan/WorldScore) · [project page](https://haoyi-duan.github.io/WorldScore/)
- **DEVIL** — [paper](https://arxiv.org/abs/2407.01094) · [repo](https://github.com/MingXiangL/DEVIL) · [project page](https://t2veval.github.io/DEVIL/)
- **VQAScore / t2v_metrics** — [repo](https://github.com/linzhiqiu/t2v_metrics) · [project page](https://linzhiqiu.github.io/papers/vqascore/)

### Adjacent collections

- [Awesome RL for Video Generation](https://github.com/wendell0218/Awesome-RL-for-Video-Generation)
- [Awesome Evaluation of Visual Generation](https://github.com/ziqihuangg/Awesome-Evaluation-of-Visual-Generation)
- [T2V-Review](https://github.com/synlp/T2V-Review)

## Contributing

Contributions welcome! Please open a PR if you know of papers, datasets, benchmarks, or tools related to video reward models. See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed criteria, section placement, and formatting guidelines.

- **Inclusion criteria:** The work should define, train, or apply a reward model, preference model, judge model, or verifier specifically for video generation, or directly enable video reward model deployment.
- **Entry format:** `[Title](url) *(Year)* — One-line description.` See [CONTRIBUTING.md](CONTRIBUTING.md) for full examples.

## Citation

```bibtex
@software{awesome-rm-for-video-generation,
  title = {{Awesome Reward Models for Video Generation}},
  author = {Liu, Chris Yuhao and others},
  year = {2026},
  doi = {10.5281/zenodo.21416545},
  url = {https://github.com/chrisliu298/awesome-rm-for-video-generation},
  version = {v1.0.0}
}
```

---

*Repository last updated: 2026-07-21. Literature systematically searched through April 2026; later papers added opportunistically. Coverage: core video reward model papers, foundations, preference optimization, physics and world rewards, datasets, benchmarks, and tooling.*
