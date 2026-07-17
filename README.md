# 🎬 Awesome Video Reward Models

<p align="center">
  <a href="https://awesome.re"><img src="https://img.shields.io/badge/Awesome-%F0%9F%8E%AC_Video_Reward_Models-000000?style=for-the-badge&labelColor=000000" alt="Awesome Video Reward Models"></a>
</p>

<p align="center">
  <!-- entry-count-start --><a href="#contents"><img src="https://img.shields.io/badge/Entries-127-000000?style=for-the-badge&labelColor=000000" alt="Entries"></a><!-- entry-count-end -->
  <a href="https://github.com/chrisliu298/awesome-video-reward-models/stargazers"><img src="https://img.shields.io/github/stars/chrisliu298/awesome-video-reward-models?style=for-the-badge&logo=github&logoColor=white&label=Stars&labelColor=000000&color=000000" alt="GitHub Stars"></a>
  <a href="https://github.com/chrisliu298/awesome-video-reward-models/network/members"><img src="https://img.shields.io/github/forks/chrisliu298/awesome-video-reward-models?style=for-the-badge&logo=github&logoColor=white&label=Forks&labelColor=000000&color=000000" alt="GitHub Forks"></a>
  <a href="https://github.com/chrisliu298/awesome-video-reward-models/commits"><img src="https://img.shields.io/github/last-commit/chrisliu298/awesome-video-reward-models?style=for-the-badge&logo=github&logoColor=white&label=Last%20Commit&labelColor=000000&color=000000" alt="Last Commit"></a>
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

## Localized, Structured, and Reasoning-Based Rewards

Reward designs that try to move past a single opaque scalar score.

### Structured and decomposed rewards

- [VisionReward: Fine-Grained Multi-Dimensional Human Preference Learning for Image and Video Generation](https://arxiv.org/abs/2412.21059) *(2024)* — Decomposes reward into multiple dimensions and binary judgment questions.
- [MJ-VIDEO: Fine-Grained Benchmarking and Rewarding Video Preferences in Video Generation](https://arxiv.org/abs/2502.01719) *(2025)* — MoE-based aspect routing over five aspects and 28 criteria.
- [Harness Local Rewards for Global Benefits: Effective Text-to-Video Generation Alignment with Patch-level Reward Models](https://arxiv.org/abs/2502.06812) *(2025)* — Local patch-level rewards for fine defects that global reward misses.
- [DenseDPO: Fine-Grained Temporal Preference Optimization for Video Diffusion Models](https://arxiv.org/abs/2506.03517) *(2025)* — Segment-level preference supervision over time.
- [RewardDance: Reward Scaling in Visual Generation](https://arxiv.org/abs/2509.08826) *(2025)* — Studies reward scaling and judge robustness with larger generative VLMs.

### Localized and detail-aware optimization

- [Mind the Generative Details: Direct Localized Detail Preference Optimization for Video Diffusion Models](https://arxiv.org/abs/2601.04068) *(2026)* — Localized detail-aware DPO with region-sensitive comparisons.
- [CaC: Advancing Video Reward Models via Hierarchical Spatiotemporal Concentrating](https://arxiv.org/abs/2605.11723) *(2026)* — Coarse-to-fine spatiotemporal concentrating (temporal scan then spatial grounding) for localized, anomaly-aware video reward modeling.

### Reasoning-based judges

- [VideoScore2: Think before You Score in Generative Video Evaluation](https://arxiv.org/abs/2509.22799) *(2025)* — Chain-of-thought reward modeling with explicit sub-scores.
- [VR-Thinker: Boosting Video Reward Models through Thinking-with-Image Reasoning](https://arxiv.org/abs/2510.10518) *(2025)* — Frame selection, visual memory, and reasoning supervision.
- [Thinking with Frames: Generative Video Distortion Evaluation via Frame Reward Model](https://arxiv.org/abs/2601.04033) *(2026)* — REACT; frame-level reasoning model for structural distortions.

### Training-free and decomposed critics

- [Diffusion-DRF: Free, Rich, and Differentiable Reward for Video Diffusion Fine-Tuning](https://arxiv.org/abs/2601.04153) *(2026)* — Reward-model-training-free (a frozen VLM supplies the dense signal) but still fine-tunes the generator; decomposes each prompt into freeform dense VQA questions.
- [Human detectors are surprisingly powerful reward models](https://arxiv.org/abs/2601.14037) *(2026)* — HuDA; specialized human detectors as simple but strong reward models.

## Physics, World, and Geometry Rewards

Reward papers where “better” means more physically or geometrically consistent.

### Benchmarks and problem framing

- [Evaluating Physical Commonsense for Video Generation](https://arxiv.org/abs/2406.03520) *(2024)* — VideoPhy; benchmark and evaluator for physical commonsense in generated videos.
- [VideoPhy-2: A Challenging Action-Centric Physical Commonsense Evaluation in Video Generation](https://arxiv.org/abs/2503.06800) *(2025)* — Stronger action-centric physics benchmark with human and automatic evaluation.
- [PISA Experiments: Exploring Physics Post-Training for Video Diffusion Models by Watching Stuff Drop](https://arxiv.org/abs/2503.09595) *(2025)* — Free-fall as a controlled testbed for physics-aware reward design.

### Physics-aware optimization

- [Think Before You Diffuse: LLMs-Guided Physics-Aware Video Generation](https://arxiv.org/abs/2505.21653) *(2025)* — Uses LLM/VLM reasoning to supervise physical plausibility.
- [Hierarchical Fine-grained Preference Optimization for Physically Plausible Video Generation](https://arxiv.org/abs/2508.10858) *(2025)* — PhysHPO; hierarchical preference signals over state, motion, and semantics.
- [PhysCorr: Dual-Reward DPO for Physics-Constrained Text-to-Video Generation with Automated Preference Selection](https://arxiv.org/abs/2511.03997) *(2025)* — Dual physics rewards for object stability and interaction.
- [PhyGDPO: Physics-Aware Groupwise Direct Preference Optimization for Physically Consistent Text-to-Video Generation](https://arxiv.org/abs/2512.24551) *(2025)* — Groupwise preference optimization with physics-guided rewards.
- [PhysRVG: Physics-Aware Unified Reinforcement Learning for Video Generative Models](https://arxiv.org/abs/2601.11087) *(2026)* — Unified RL treatment of physics constraints for video generation.

### World and geometry critics

- [Inference-time Physics Alignment of Video Generative Models with Latent World Models](https://arxiv.org/abs/2601.10553) *(2026)* — WMReward; latent world model as inference-time critic.
- [VIGOR: VIdeo Geometry-Oriented Reward for Temporal Generative Alignment](https://arxiv.org/abs/2603.16271) *(2026)* — Geometry-based reward using reprojection consistency.
- [VGGRPO: Towards World-Consistent Video Generation with 4D Latent Reward](https://arxiv.org/abs/2603.26599) *(2026)* — 4D latent geometry reward plus GRPO.
- [CamPilot: Improving Camera Control in Video Diffusion Model with Efficient Camera Reward Feedback](https://arxiv.org/abs/2601.16214) *(2026)* — Camera-consistency reward for controllable motion and view planning.

## Editing, Identity, Camera, and Domain-Specific Rewards

Reward models are still sparse outside generic T2V. These papers are especially useful if you work on specialized generation settings.

### Video editing

- [TDVE-Assessor: Benchmarking and Evaluating the Quality of Text-Driven Video Editing with LMMs](https://arxiv.org/abs/2505.19535) *(2025)* — Benchmark and judge model for text-driven video editing.
- [Prompt-A-Video: Prompt Your Video Diffusion Model via Preference-Aligned LLM](https://arxiv.org/abs/2412.15156) *(2024)* — Upstream prompt rewriting using preference signals.

### Identity preservation

- [Identity-Preserving Image-to-Video Generation via Reward-Guided Optimization](https://arxiv.org/abs/2510.14255) *(2025)* — Facial identity reward for I2V generation.
- [Identity-GRPO: Optimizing Multi-Human Identity-preserving Video Generation via Reinforcement Learning](https://arxiv.org/abs/2510.14256) *(2025)* — Identity-aware reward model and GRPO for multi-person videos.
- [HuViDPO: Enhancing Video Generation through Direct Preference Optimization for Human-Centric Alignment](https://arxiv.org/abs/2502.01690) *(2025)* — Human-centric alignment setting.

### Camera control

- [CamPilot: Improving Camera Control in Video Diffusion Model with Efficient Camera Reward Feedback](https://arxiv.org/abs/2601.16214) *(2026)* — Camera-control-specific reward modeling.

### Domain-specific alignment

- [Aligning Anime Video Generation with Human Feedback](https://arxiv.org/abs/2504.10044) *(2025)* — Domain-specific preference learning for anime video generation.
- [EVA: Aligning Video World Models with Executable Robot Actions via Inverse Dynamics Rewards](https://arxiv.org/abs/2603.17808) *(2026)* — Uses an inverse-dynamics model as an executability reward to align generated robot videos with physically feasible actions (RL post-training).

## Inference-Time Reward, Search, and Process Rewards

Not all reward models are only for post-training; some are most useful at test time or over latent trajectories.

### Reward-guided inference

- [Improving Video Generation with Human Feedback](https://arxiv.org/abs/2501.13918) *(2025)* — Includes Flow-NRG, an inference-time reward-guided generation strategy.
- [DOLLAR: Few-Step Video Generation via Distillation and Latent Reward Optimization](https://arxiv.org/abs/2412.15689) *(2024)* — Latent reward optimization for efficient few-step video generation.
- [Inference-time Physics Alignment of Video Generative Models with Latent World Models](https://arxiv.org/abs/2601.10553) *(2026)* — WMReward as a pure inference-time verifier.
- [VIGOR: VIdeo Geometry-Oriented Reward for Temporal Generative Alignment](https://arxiv.org/abs/2603.16271) *(2026)* — Can be used for both post-training and inference-time selection.
- [PISCES: Annotation-free Text-to-Video Post-Training via Optimal Transport-Aligned Rewards](https://arxiv.org/abs/2602.01624) *(2026)* — Annotation-free **post-training** (direct reward backprop and RL), not only inference-time; optimal-transport-aligned semantic and quality rewards.
- [Diffusion-DRF: Free, Rich, and Differentiable Reward for Video Diffusion Fine-Tuning](https://arxiv.org/abs/2601.04153) *(2026)* — Dense VLM supervision without collecting new preference labels.

### Latent search and process rewards

- [Video Generation Models Are Good Latent Reward Models](https://arxiv.org/abs/2511.21541) *(2025)* — Process Reward Feedback Learning (PRFL) in the generator’s latent space.
- [Euphonium: Steering Video Flow Matching via Process Reward Gradient Guided Stochastic Dynamics](https://arxiv.org/abs/2602.04928) *(2026)* — Couples process rewards with stochastic flow guidance.
- [LatSearch: Latent Reward-Guided Search for Faster Inference-Time Scaling in Video Diffusion](https://arxiv.org/abs/2603.14526) *(2026)* — Search and pruning in latent space using reward estimates.

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
@software{awesome-video-reward-models,
  title = {{Awesome Video Reward Models}},
  author = {Liu, Chris Yuhao and others},
  year = {2026},
  doi = {10.5281/zenodo.21416545},
  url = {https://github.com/chrisliu298/awesome-video-reward-models},
  version = {v1.0.0}
}
```

---

*Repository last updated: 2026-07-17. Literature systematically searched through April 2026; later papers added opportunistically. Coverage: core video reward model papers, foundations, preference optimization, physics and world rewards, datasets, benchmarks, and tooling.*
