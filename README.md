# Awesome VLM Adversarial Robustness [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[![arXiv](https://img.shields.io/badge/arXiv-TODO-b31b1b.svg)](https://arxiv.org/abs/TODO)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
![Papers](https://img.shields.io/badge/papers-130%2B-0a7bbb.svg)
![Maintained](https://img.shields.io/badge/Maintained%3F-yes-brightgreen.svg)
<!-- After publishing, replace OWNER with your GitHub org/user to activate these:
![Stars](https://img.shields.io/github/stars/OWNER/Awesome-VLM-Adversarial-Robustness?style=social)
![Last Commit](https://img.shields.io/github/last-commit/OWNER/Awesome-VLM-Adversarial-Robustness) -->

> A curated, **genealogy-organized** reading list on the **adversarial robustness of Vision-Language Models (VLMs)** — from CLIP-style contrastive models to generative MLLMs (LLaVA, MiniGPT-4). Companion repository to our survey *"A Survey of Adversarial Robustness for Vision-Language Models: From Fine-Tuning to Test-Time Defense."*

> 📌 **Taxonomy figure — coming soon.** A high-resolution taxonomy tree and a method-evolution timeline will be added under [`assets/`](assets/).

Methods are grouped by **sub-family** and ordered by **genealogy**: a method's row sits *after* the work it builds on, and the `→ X` note in each title says which earlier method it improves. ⭐ marks the most-cited **foundational baselines**.

## 🎉 News

<details open>
<summary><b>2026-05 — v1.0 initial release</b></summary>

- First public release: **130+ papers** across 7 categories, organized by method genealogy.
- Sub-family grouping with foundational-baseline highlighting and "improves-upon" notes.
- Benchmark, attack-protocol, and open-challenge sections distilled from the survey.

</details>

## 🗺️ Overview

Vision-language models such as CLIP, BLIP, and their generative successors (LLaVA, MiniGPT-4) excel across multimodal tasks, yet remain **vulnerable to adversarial perturbations**, raising safety concerns for real deployments. This list follows the survey's **unified, defense-centric taxonomy**, organized along an axis of **decreasing defense cost**:

> **Adversarial Fine-Tuning** (modify weights) → **Adversarial Prompt Tuning** (frozen backbone, learn prompts) → **Test-Time Defense** (no training) → **Certified Defense** (provable guarantees)

**What this repo offers**
1. The first unified taxonomy spanning **contrastive VLMs and generative MLLMs**.
2. A **defense-cost axis** (AFT → APT → TTD → Certified) that organizes the whole field.
3. Explicit **method genealogies** validated by cross-referencing baseline comparisons across 130+ papers.
4. The most comprehensive treatment of **adversarial prompt tuning**, the most active direction.
5. **Baseline-frequency analysis** that empirically identifies the foundational method (⭐) in each sub-field.

**Legend** — ⭐ foundational/most-cited baseline · `→ X` improves upon X · 📄 paper · 💻 official code · 🔍 search (no canonical link in source yet) · — code not yet linked (see [CONTRIBUTING](CONTRIBUTING.md)).

## 📑 Table of Contents

- [⭐ Foundational Baselines at a Glance](#-foundational-baselines-at-a-glance)
- [📚 Background & Resources](#-background--resources)
- **Defenses, by decreasing cost** ↓
  - [1 · Attacks & Threat Models](#1-attacks--threat-models)
  - [2 · Adversarial Fine-Tuning (AFT)](#2-adversarial-fine-tuning-aft)
  - [3 · Adversarial Prompt Tuning (APT)](#3-adversarial-prompt-tuning-apt)
  - [4 · Test-Time Defense (TTD)](#4-test-time-defense-ttd)
  - [5 · Certified Defense](#5-certified-defense)
  - [6 · MLLM Robustness](#6-mllm-robustness)
- [7 · Benchmarks & Evaluation](#7-benchmarks--evaluation)
- [🔭 Related Surveys](#-related-surveys)
- [🚧 Open Challenges](#-open-challenges)
- [🤝 Contributing](#-contributing) · [📜 Citation](#-citation) · [🙏 Acknowledgments](#-acknowledgments)

## ⭐ Foundational Baselines at a Glance

The eight methods every robustness paper compares against — identified by **baseline-frequency analysis** (how often each is used as a comparison point across the 130+ surveyed works). Each anchors a sub-family; the parenthetical is its citation frequency in the survey.

| Category | ⭐ Baseline | One-line idea | Venue | Cited as baseline |
|:--|:--|:--|:--:|:--:|
| **AFT** · Supervised | [TeCoA](#supervised-aft) | Contrastive adversarial loss — the universal foundation | ICLR'23 | 14/15 |
| **AFT** · Supervised | [PMG-AFT](#supervised-aft) | Pre-trained-model guidance during AFT | CVPR'24 | 9/15 |
| **AFT** · Unsupervised | [FARE](#unsupervised-aft) | Unsupervised embedding ℓ₂ alignment; MLLM-transferable | ICML'24 | 9/15 |
| **AFT** · Attention | [TGA-ZSR](#attention--feature-alignment) | Text-guided attention alignment | NeurIPS'24 | 5/15 |
| **APT** · Text-prompt | [APT](#apt-family-text-prompt-centric) | One prompt word boosts robustness | CVPR'24 | 6/12 |
| **APT** · Multi-modal | [FAP](#fap-family-multi-modal-prompt-centric) | Few-shot adversarial prompts + TRADES loss | NeurIPS'24 | 5/12 |
| **TTD** · Prompt-tuning | [R-TPT](#test-time-prompt-tuning) | Test-time prompt tuning, entropy + reliability | CVPR'25 | 8/17 |
| **TTD** · Counterattack | [TTC](#counterattack) | First training-free counterattack for CLIP | CVPR'25 | 9/17 |
| **TTD** · Embedding | [AOM](#embedding-space-defense) | Anchor-guided one-step move (training-free SoTA) | CVPR'25 | — |

> 💡 New to the field? Read these eight first, then branch into the sub-family that matches your compute budget (AFT → APT → TTD → Certified, cheapest defense effort last).

## 📚 Background & Resources

Key models and prior art that the robustness literature builds on.

#### VLMs / MLLMs

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2021 | CLIP | Learning Transferable Visual Models From Natural Language Supervision <br/><sub>contrastive image-text pre-training</sub> | ICML'21 | [📄](https://arxiv.org/abs/2103.00020) | [💻](https://github.com/openai/CLIP) |
| 2022 | BLIP | Blip: Bootstrapping language-image pre-training for unified vision-language understanding and generation | ICML'22 | [📄](https://arxiv.org/abs/2201.12086) | [💻](https://github.com/salesforce/BLIP) |
| 2023 | BLIP-2 | Blip-2: Bootstrapping language-image pre-training with frozen image encoders and large language models <br/><sub>frozen image encoders + LLM</sub> | ICML'23 | [📄](https://arxiv.org/abs/2301.12597) | [💻](https://github.com/salesforce/LAVIS) |
| 2023 | LLaVA | Visual instruction tuning | NeurIPS'23 | [📄](https://arxiv.org/abs/2304.08485) | [💻](https://github.com/haotian-liu/LLaVA) |
| 2024 | MiniGPT-4 | Minigpt-4: Enhancing vision-language understanding with advanced large language models | ICLR'24 | [📄](https://arxiv.org/abs/2304.10592) | [💻](https://github.com/Vision-CAIR/MiniGPT-4) |
| 2023 | InstructBLIP | Instructblip: Towards general-purpose vision-language models with instruction tuning | NeurIPS'23 | [📄](https://arxiv.org/abs/2305.06500) | [💻](https://github.com/salesforce/LAVIS) |
| 2023 | OpenFlamingo | Openflamingo: An open-source framework for training large autoregressive vision-language models | arXiv'23 | [📄](https://arxiv.org/abs/2308.01390) | [💻](https://github.com/mlfoundations/open_flamingo) |

#### Adaptation baselines (clean)

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2022 | CoOp | Learning to prompt for vision-language models <br/><sub>prompt learning for VLMs</sub> | IJCV'22 | [📄](https://arxiv.org/abs/2109.01134) | [💻](https://github.com/KaiyangZhou/CoOp) |
| 2022 | CoCoOp | Conditional prompt learning for vision-language models <br/><sub>conditional prompts</sub> | CVPR'22 | [📄](https://arxiv.org/abs/2203.05557) | [💻](https://github.com/KaiyangZhou/CoOp) |
| 2023 | MaPLe | Maple: Multi-modal prompt learning | CVPR'23 | [📄](https://arxiv.org/abs/2210.03117) | [💻](https://github.com/muzairkhattak/multimodal-prompt-learning) |
| 2022 | TPT | Test-time prompt tuning for zero-shot generalization in vision-language models | NeurIPS'22 | [📄](https://arxiv.org/abs/2209.07511) | [💻](https://github.com/azshue/TPT) |
| 2024 | CLIP-LoRA | Low-rank few-shot adaptation of vision-language models | CVPRW'24 | [📄](https://arxiv.org/abs/2405.18541) | [💻](https://github.com/MaxZanella/CLIP-LoRA) |
| 2024 | MTA | On the test-time zero-shot generalization of vision-language models: Do we really need prompt learning? <br/><sub>training-free test-time augmentation</sub> | CVPR'24 | [📄](https://arxiv.org/abs/2405.02266) | [💻](https://github.com/MaxZanella/MTA) |

#### Adversarial-robustness foundations

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2019 | TRADES | Theoretically principled trade-off between robustness and accuracy <br/><sub>robustness-accuracy trade-off</sub> | ICML'19 | [📄](https://arxiv.org/abs/1901.08573) | [💻](https://github.com/yaodongyu/TRADES) |
| 2019 | Robustness@Odds | Robustness may be at odds with accuracy | ICLR'19 | [📄](https://arxiv.org/abs/1805.12152) | — |
| 2020 | Adaptive Attacks | On adaptive attacks to adversarial example defenses <br/><sub>on adaptive attacks to defenses</sub> | NeurIPS'20 | [📄](https://arxiv.org/abs/2002.08347) | [💻](https://github.com/wielandbrendel/adaptive_attacks_paper) |
| 2022 | DiffPure | Diffusion models for adversarial purification <br/><sub>diffusion-based purification</sub> | ICML'22 | [📄](https://arxiv.org/abs/2205.07460) | [💻](https://github.com/NVlabs/DiffPure) |

## 1 Attacks & Threat Models

Attacks define the threat each defense targets — by modality (image / text / multi-modal) and goal (untargeted / targeted / jailbreak / black-box transfer).

#### Image-Modality Attacks

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2015 | FGSM | Explaining and harnessing adversarial examples <br/><sub>fast gradient sign method</sub> | ICLR'15 | [📄](https://arxiv.org/abs/1412.6572) | — |
| 2018 | PGD | Towards deep learning models resistant to adversarial attacks <br/><sub>projected gradient descent</sub> | ICLR'18 | [📄](https://arxiv.org/abs/1706.06083) | [💻](https://github.com/MadryLab/mnist_challenge) |
| 2020 | AutoAttack | Reliable evaluation of adversarial robustness with an ensemble of diverse parameter-free attacks <br/><sub>parameter-free ensemble; eval gold-standard</sub> | ICML'20 | [📄](https://arxiv.org/abs/2003.01690) | [💻](https://github.com/fra31/auto-attack) |
| 2023 | APGD-VLM | On the adversarial robustness of multi-modal foundation models <br/><sub>captioning attack on generative VLMs</sub> | ICCVW'23 | [📄](https://arxiv.org/abs/2308.10741) | — |
| 2024 | VisualAdv | Visual adversarial examples jailbreak aligned large language models <br/><sub>single image jailbreaks aligned MLLMs</sub> | AAAI'24 | [📄](https://arxiv.org/abs/2306.13213) | [💻](https://github.com/Unispac/Visual-Adversarial-Examples-Jailbreak-Large-Language-Models) |
| 2026 | VEAttack | VEAttack: Downstream-agnostic Vision Encoder Attack against Large Vision Language Models <br/><sub>encoder-level, downstream/LLM-agnostic</sub> | ICLR'26 | [📄](https://arxiv.org/abs/2505.17440) | [💻](https://github.com/hefeimei06/VEAttack-LVLM) |

#### Text-Modality Attacks

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2020 | BERT-Attack | Bert-attack: Adversarial attack against bert using bert <br/><sub>MLM-guided word substitution</sub> | EMNLP'20 | [📄](https://arxiv.org/abs/2004.09984) | [💻](https://github.com/LinyangLee/BERT-Attack) |

#### Multi-Modal Attacks

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2022 | Co-Attack | Towards adversarial attack on vision-language pre-training models <br/><sub>first attack on VLP (image+text)</sub> | ACM MM'22 | [📄](https://arxiv.org/abs/2206.09391) | [💻](https://github.com/adversarial-for-goodness/Co-Attack) |
| 2024 | MMCoA | Revisiting the adversarial robustness of vision language models: a multimodal perspective <br/><sub>multimodal contrastive attack & study</sub> | arXiv'24 | [📄](https://arxiv.org/abs/2404.19287) | [💻](https://github.com/ElleZWQ/MMCoA) |
| 2026 | MAT (attack) | Multimodal Adversarial Defense for Vision-Language Models by Leveraging One-To-Many Relationships <br/><sub>joint image+text; single-modality defense insufficient</sub> | WACV'26 | [📄](https://arxiv.org/abs/2405.18770) | [💻](https://github.com/CyberAgentAILab/multimodal-adversarial-training) |
| 2025 | SA-AET | Semantic-Aligned Adversarial Evolution Triangle for High-Transferability Vision-Language Attack <br/><sub>semantic-aligned adversarial evolution triangle; transferable</sub> | TPAMI'25 | [📄](https://doi.org/10.1109/TPAMI.2025.3581476) | [💻](https://github.com/jiaxiaojunQAQ/SA-AET) |

#### Targeted & Black-Box Transfer

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | PTA | Improving generalizability and undetectability for targeted adversarial attacks on multimodal pre-trained models <br/><sub>generalizable + undetectable targeted attack</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2509.19994) | — |
| 2024 | Replace-then-Perturb | Replace-then-perturb: Targeted adversarial attacks with visual reasoning for vision-language models <br/><sub>targeted via segmentation + inpainting</sub> | arXiv'24 | [📄](https://arxiv.org/abs/2411.00898) | — |
| 2025 | TransferVLM | Transferable Adversarial Attacks on Black-Box Vision-Language Models <br/><sub>black-box transfer to GPT-4o / Claude / Gemini</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2505.01050) | — |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 2 Adversarial Fine-Tuning (AFT)

Modify model weights by training on adversarial examples. Three pillars — **TeCoA** (supervised), **FARE** (unsupervised), and the attention-aligned **TGA-ZSR** — anchor the field.

#### Supervised AFT

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2023 | **TeCoA** ⭐ | Understanding Zero-Shot Adversarial Robustness for Large-Scale Models <br/><sub>universal foundation — contrastive adversarial loss (14/15)</sub> | ICLR'23 | [📄](https://arxiv.org/abs/2212.07016) | [💻](https://github.com/cvlab-columbia/ZSRobust4FoundationModel) |
| 2024 | **PMG-AFT** ⭐ | Pre-trained model guided fine-tuning for zero-shot adversarial robustness <br/><sub>→ TeCoA; pre-trained model guidance (9/15)</sub> | CVPR'24 | [📄](https://arxiv.org/abs/2401.04350) | [💻](https://github.com/serendipity1122/Pre-trained-Model-Guided-Fine-Tuning-for-Zero-Shot-Adversarial-Robustness) |
| 2026 | AdvFLYP | Finetune Like You Pretrain: Boosting Zero-shot Adversarial Robustness in Vision-language Models <br/><sub>→ TeCoA; match pre-training contrastive loss</sub> | CVPR'26 | [📄](https://arxiv.org/abs/2604.11576) | [💻](https://github.com/Sxing2/AdvFLYP) |
| 2025 | QT-AFT | Quality Text, Robust Vision: The Role of Language in Enhancing Visual Robustness of Vision-Language Models <br/><sub>→ TeCoA; caption-guided text supervision</sub> | ACM MM'25 | [📄](https://arxiv.org/abs/2507.16257) | — |
| 2026 | SAFT | Semantic-aware Adversarial Fine-tuning for CLIP <br/><sub>→ QT-AFT; semantic-aware AE generation</sub> | TMLR'26 | [📄](https://arxiv.org/abs/2602.12461) | [💻](https://github.com/tmlr-group/SAFT) |
| 2026 | AGFT | AGFT: Alignment-Guided Fine-Tuning for Zero-Shot Adversarial Robustness of Vision-Language Models <br/><sub>→ PMG-AFT; distribution-consistency calibration</sub> | CVPR'26 | [📄](https://arxiv.org/abs/2603.29410) | [💻](https://github.com/YuboCui/AGFT) |
| 2024 | RCSD | On enhancing adversarial robustness of large pre-trained vision-language models <br/><sub>label-free correlation self-distillation</sub> | CSAI'24 | [📄](https://doi.org/10.1145/3709026.3709059) | — |
| 2025 | CAW | Zero-Shot Robustness of Vision Language Models Via Confidence-Aware Weighting <br/><sub>confidence-aware hard-example weighting</sub> | NeurIPSW'25 | [📄](https://arxiv.org/abs/2510.02913) | — |
| 2025 | AdvSimplex | Improving zero-shot adversarial robustness in vision-language models by closed-form alignment of adversarial path simplices <br/><sub>closed-form simplex path alignment</sub> | ICML'25 | [📄](https://proceedings.mlr.press/v267/dong25f.html) | — |

#### Attention & Feature Alignment

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | TGA-ZSR | Text-guided attention is all you need for zero-shot robustness in vision-language models <br/><sub>text-guided attention alignment (5/15 baseline)</sub> | NeurIPS'24 | [📄](https://arxiv.org/abs/2410.21802) | [💻](https://github.com/zhyblue424/TGA-ZSR) |
| 2026 | Comp-TGA | Complementary Text-Guided Attention for Zero-Shot Adversarial Robustness <br/><sub>→ TGA-ZSR; complementary attention</sub> | TPAMI'26 | [📄](https://arxiv.org/abs/2603.18598) | [💻](https://github.com/zhyblue424/TGA-ZSR) |
| 2025 | AoS | Robustifying zero-shot vision language models by subspaces alignment <br/><sub>subspace (2nd-order) alignment</sub> | ICCV'25 | [📄](https://openaccess.thecvf.com/content/ICCV2025/papers/Dong_Robustifying_Zero-Shot_Vision_Language_Models_by_Subspaces_Alignment_ICCV_2025_paper.pdf) | — |
| 2026 | TIMA | TIMA: Text-Image Mutual Awareness for Balancing Zero-Shot Adversarial Robustness and Generalization Ability <br/><sub>inter-class distance enlargement</sub> | AAAI'26 | [📄](https://arxiv.org/abs/2405.17678) | — |
| 2026 | GRACE | The Geometry of Robustness: Optimizing Loss Landscape Curvature and Feature Manifold Alignment for Robust Finetuning of Vision-Language Models <br/><sub>PAC-Bayes curvature + feature manifold</sub> | CVPR'26 | [📄](https://arxiv.org/abs/2603.27139) | — |
| 2025 | GLADIATOR | Stabilizing modality gap & lowering gradient norms improve zero-shot adversarial robustness of vlms <br/><sub>modality-gap stabilization + low gradient norm</sub> | KDD'25 | [📄](https://doi.org/10.1145/3690624.3709296) | — |

#### Multi-Modal Adversarial Training

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2026 | MAT | Multimodal Adversarial Defense for Vision-Language Models by Leveraging One-To-Many Relationships <br/><sub>perturb image + text; one-to-many augmentation</sub> | WACV'26 | [📄](https://arxiv.org/abs/2405.18770) | [💻](https://github.com/CyberAgentAILab/multimodal-adversarial-training) |
| 2025 | AGH-MAT | Attention-Guided Hierarchical Defense for Multimodal Attacks in Vision-Language Models <br/><sub>→ MAT; attention-guided hierarchical fusion</sub> | CVPRW'25 | [📄](https://openaccess.thecvf.com/content/CVPR2025W/TMM-OpenWorld/papers/Chen_Attention-Guided_Hierarchical_Defense_for_Multimodal_Attacks_in_Vision-Language_Models_CVPRW_2025_paper.pdf) | — |
| 2025 | MAVA | Boosting Adversarial Robustness of Vision-Language Pre-training Models against Multimodal Adversarial attacks <br/><sub>multi-granularity cross-modal supervision</sub> | ICLRW'25 | [📄](https://openreview.net/forum?id=9obhyu9csa) | — |
| 2024 | MMCoA | Revisiting the adversarial robustness of vision language models: a multimodal perspective <br/><sub>multimodal contrastive adversarial training</sub> | arXiv'24 | [📄](https://arxiv.org/abs/2404.19287) | [💻](https://github.com/ElleZWQ/MMCoA) |
| 2026 | Hier. Robust | Hierarchically Robust Zero-shot Vision-language Models <br/><sub>hyperbolic hierarchical embeddings</sub> | CVPR'26 | [📄](https://arxiv.org/abs/2604.18867) | — |

#### Unsupervised AFT

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | **FARE** ⭐ | Robust CLIP: Unsupervised Adversarial Fine-Tuning of Vision Embeddings for Robust Large Vision-Language Models <br/><sub>unsupervised pillar — embedding ℓ₂ alignment; MLLM-transferable (9/15)</sub> | ICML'24 | [📄](https://arxiv.org/abs/2402.12336) | [💻](https://github.com/chs20/RobustVLM) |
| 2026 | Sim-CLIP | Sim-clip: Unsupervised siamese adversarial fine-tuning for robust and semantically-rich vision-language models <br/><sub>→ FARE; Siamese cosine similarity</sub> | IJCNN'26 | [📄](https://arxiv.org/abs/2407.14971) | [💻](https://github.com/speedlab-git/SimCLIP) |
| 2025 | SLADE | SLADE: Shielding against Dual Exploits in Large Vision-Language Models <br/><sub>→ FARE; dual-level + jailbreak defense</sub> | CVPR'25 | [📄](https://openaccess.thecvf.com/content/CVPR2025/html/Hossain_SLADE_Shielding_against_Dual_Exploits_in_Large_Vision-Language_Models_CVPR_2025_paper.html) | — |
| 2025 | Adv-W2S | Robust superalignment: Weak-to-strong robustness generalization for vision-language models <br/><sub>→ FARE; weak-to-strong robustness transfer</sub> | NeurIPS'25 | [📄](https://openreview.net/forum?id=rOR5IZcwJx) | — |
| 2025 | LEAF | Robustness in Both Domains: CLIP Needs a Robust Text Encoder <br/><sub>→ FARE; first robust TEXT encoder</sub> | NeurIPS'25 | [📄](https://arxiv.org/abs/2506.03355) | [💻](https://github.com/LIONS-EPFL/LEAF) |

#### Parameter-Efficient AFT

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | AdvCLIP-LoRA | Few-shot adversarial low-rank fine-tuning of vision-language models <br/><sub>minimax over LoRA adapters (few-shot)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2505.15130) | [💻](https://github.com/sajjad-ucsb/AdvCLIP-LoRA) |
| 2025 | AdvLoRA | Enhancing adversarial robustness of vision-language models through low-rank adaptation <br/><sub>clustering-based LoRA reparameterization</sub> | ICMR'25 | [📄](https://arxiv.org/abs/2404.13425) | — |
| 2026 | AdvMask | Identifying Robust Neural Pathways: Few-Shot Adversarial Mask Tuning for Vision-Language Models <br/><sub>binary robust subnetwork + LAFA loss</sub> | ICLR'26 | [📄](https://openreview.net/forum?id=kzGkXpW4FT) | — |

#### Novel Paradigms

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | CLIP-H | Double visual defense: Adversarial pre-training and instruction tuning for improving vision-language model robustness <br/><sub>double visual defense (adv pre-train + instruct)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2501.09446) | [💻](https://github.com/zw615/Double_Visual_Defense) |
| 2026 | AdPO | Adpo: Enhancing the adversarial robustness of large vision-language models with preference optimization <br/><sub>adversarial training as preference optimization (DPO)</sub> | ICLR'26 | [📄](https://arxiv.org/abs/2504.01735) | — |
| 2026 | PISTOLE | Tug-of-war no more: Harmonizing accuracy and robustness in vision-language models via stability-aware task vector merging <br/><sub>stability-aware task-vector merging (no training)</sub> | ICLR'26 | [📄](https://openreview.net/forum?id=KOO1cDm2bt) | — |
| 2026 | HPT-GPD | Proxy Robustness in Vision Language Models is Effortlessly Transferable <br/><sub>proxy robustness transfer across architectures</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2601.12865) | [💻](https://github.com/fxw13/HPT-GPD) |
| 2025 | Alignment Perturbation | Enhancing the robustness of vision-language foundation models by alignment perturbation <br/><sub>robustify the alignment module (Q-Former)</sub> | TIFS'25 | [📄](https://ieeexplore.ieee.org/document/11072221) | — |
| 2026 | REARGUARD | Allies Teach Better Than Enemies: Inverse Adversaries for Robust Knowledge Distillation <br/><sub>inverse-adversary weight-level distillation</sub> | TPAMI'26 | [📄](https://doi.org/10.1109/TPAMI.2026.3660863) | — |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 3 Adversarial Prompt Tuning (APT)

Freeze the backbone, learn robust prompts. Two lineages: the text-prompt **APT** family (← CoOp) and the multi-modal **FAP** family (← MaPLe).

#### APT Family (text-prompt-centric)

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | AdvPT | Adversarial prompt tuning for vision-language models <br/><sub>text prompts aligned to adversarial embeddings</sub> | ECCV'24 | [📄](https://arxiv.org/abs/2311.11261) | [💻](https://github.com/jiamingzhang94/Adversarial-Prompt-Tuning) |
| 2024 | **APT** ⭐ | One Prompt Word is Enough to Boost Adversarial Robustness for Pre-trained Vision-Language Models <br/><sub>one prompt word boosts robustness (6/12)</sub> | CVPR'24 | [📄](https://arxiv.org/abs/2403.01849) | [💻](https://github.com/TreeLLi/APT) |
| 2025 | MoAPT | MoAPT: Mixture of Adversarial Prompt Tuning for Vision-Language Models <br/><sub>→ APT; mixture-of-experts prompts</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2505.17509) | — |
| 2024 | mixprompt | Mixprompt: Enhancing generalizability and adversarial robustness for vision-language models via prompt fusion <br/><sub>instance-specific + hand-crafted fusion</sub> | ICIC'24 | [📄](https://link.springer.com/chapter/10.1007/978-981-97-5606-3_28) | — |
| 2025 | PFPT | Parameter-free and Accessible Prompt Learning to Enhance Adversarial Robustness for Pre-trained Vision-Language Models <br/><sub>parameter-free vocabulary search</sub> | NAACL'25 | [📄](https://aclanthology.org/2025.naacl-long.33/) | — |
|  | Concept-APT | Interpretable Adversarial Prompt Tuning via Semantic Concepts <br/><sub>interpretable semantic concepts</sub> | AdvML-W | [🔍](https://scholar.google.com/scholar?q=Interpretable%20Adversarial%20Prompt%20Tuning%20via%20Semantic%20Concepts) | — |
| 2025 | MDAPT | MDAPT: Multi-Modal Depth Adversarial Prompt Tuning to Enhance the Adversarial Robustness of Visual Language Models <br/><sub>→ APT; multi-modal depth prompting</sub> | Sensors'25 | [📄](https://www.mdpi.com/1424-8220/25/1/258) | — |
| 2025 | ER-APT | Evolution-based region adversarial prompt learning for robustness enhancement in vision-language models <br/><sub>evolutionary adversarial-example diversity</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2503.12874) | — |
| 2024 | Robust-Gen APT | Revisiting the robust generalization of adversarial prompt tuning <br/><sub>revisiting robust generalization of APT</sub> | arXiv'24 | [📄](https://arxiv.org/abs/2405.11154) | — |

#### FAP Family (multi-modal-prompt-centric)

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | **FAP** ⭐ | Few-Shot Adversarial Prompt Learning on Vision-Language Models <br/><sub>→ MaPLe; adversarial text supervision + TRADES loss; AdvMaPLe is its bridge baseline (5/12)</sub> | NeurIPS'24 | [📄](https://arxiv.org/abs/2403.14774) | [💻](https://github.com/lionel-w2/FAP) |
| 2025 | CoAPT | Learning robust vision-language models from natural latent spaces <br/><sub>→ FAP; closed-loop T2V/V2T adapters</sub> | NeurIPS'25 | [📄](https://openreview.net/forum?id=7G9YKty2UZ) | — |
| 2026 | APD | Adversarial prompt distillation for vision-language models <br/><sub>adversarial prompt distillation (teacher→student)</sub> | ICASSP'26 | [📄](https://arxiv.org/abs/2411.15244) | [💻](https://github.com/luolin0715/APD) |
| 2026 | NAP-tuning | NAP-Tuning: Neural Augmented Prompt Tuning for Adversarially Robust Vision-Language Models <br/><sub>neural augmentor + token refiners</sub> | TPAMI'26 | [📄](https://arxiv.org/abs/2506.12706) | [💻](https://github.com/jiamingzhang94/Adversarial-Prompt-Tuning) |
| 2026 | CBA-FAPT | Stabilizing Cross-Modal Bidirectional Attribution: Few-Shot Adversarial Prompt Tuning for Robust Vision-Language Models <br/><sub>bidirectional attribution-map consistency</sub> | AAAI'26 | [📄](https://ojs.aaai.org/index.php/AAAI/article/view/37396) | — |
| 2026 | CLBP | Closed-Loop Bidirectional Prompting for Adversarial Robustness of Vision Language Models <br/><sub>instance-adaptive bidirectional closed-loop (test-time)</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2605.25922) | — |

#### Visual Prompt Tuning

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2023 | C-AVP | Visual prompting for adversarial robustness <br/><sub>class-wise adversarial visual prompts</sub> | ICASSP'23 | [📄](https://arxiv.org/abs/2210.06284) | [💻](https://github.com/Phoveran/vp-for-adversarial-robustness) |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 4 Test-Time Defense (TTD)

No training — defend at inference using only the test input and pre-trained VLM. Two paradigms dominate: counterattack (**TTC**) and test-time prompt tuning (**R-TPT**).

#### Pre-VLM Baselines

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2021 | HD | Attacking adversarial attacks as a defense <br/><sub>hedge defense — attack all classes</sub> | arXiv'21 | [📄](https://arxiv.org/abs/2106.04938) | — |
| 2022 | Anti-Adv | Combating adversaries with anti-adversaries <br/><sub>anti-adversary input layer</sub> | AAAI'22 | [📄](https://arxiv.org/abs/2103.14347) | [💻](https://github.com/MotasemAlfarra/Combating-Adversaries-with-Anti-Adversaries) |
| 2021 | TTE | Enhancing adversarial robustness via test-time transformation ensembling | ICCV'21 | [📄](https://arxiv.org/abs/2107.14110) | [💻](https://github.com/juancprzs/TTE) |

#### Test-Time Prompt Tuning

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | **R-TPT** ⭐ | R-TPT: Improving Adversarial Robustness of Vision-Language Models through Test-Time Prompt Tuning <br/><sub>pointwise entropy + reliability weighting (8/17)</sub> | CVPR'25 | [📄](https://arxiv.org/abs/2504.11195) | [💻](https://github.com/TomSheng21/R-TPT) |
| 2026 | A-TPT | Towards Fine-Grained Robustness: Attention-Guided Test-Time Prompt Tuning for Vision-Language Models <br/><sub>→ R-TPT; attention-guided augmentation</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2605.19956) | [💻](https://github.com/SEU-VIPGroup/A-TPT) |
| 2025 | TAPT | Tapt: Test-time adversarial prompt tuning for robust inference in vision-language models <br/><sub>→ R-TPT; bimodal test-time prompts</sub> | CVPR'25 | [📄](https://arxiv.org/abs/2411.13136) | [💻](https://github.com/xinwong/TAPT) |
| 2026 | TAME | TAME: Test-Time Adversarial Prompt Tuning via Mixture-of-Experts for Vision-Language Models <br/><sub>→ TAPT; mixture-of-prompts routing</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2605.17577) | — |

#### Counterattack

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | **TTC** ⭐ | CLIP is Strong Enough to Fight Back: Test-time Counterattacks towards Zero-shot Adversarial Robustness of CLIP <br/><sub>first training-free counterattack for CLIP (9/17)</sub> | CVPR'25 | [📄](https://arxiv.org/abs/2503.03613) | [💻](https://github.com/Sxing2/CLIP-Test-time-Counterattacks) |
| 2026 | DOC | Diversifying Counterattacks: Orthogonal Exploration for Robust CLlP Inference <br/><sub>→ TTC; orthogonal diversified counterattack</sub> | AAAI'26 | [📄](https://arxiv.org/abs/2511.09064) | [💻](https://github.com/bookman233/DOC) |
| 2025 | FPT-Noise | FPT-Noise: Dynamic Scene-Aware Counterattack for Test-Time Adversarial Defense in Vision-Language Models <br/><sub>→ TTC; dynamic scene-aware modulation</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2510.20856) | — |
| 2025 | SCC | Self-Calibrated Consistency can Fight Back for Adversarial Robustness in Vision-Language Models <br/><sub>→ TTC; self-calibrated consistency (+text)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2510.22785) | — |
| 2025 | TTP | TTP: Test-Time Padding for Adversarial Detection and Robust Adaptation on Vision-Language Models <br/><sub>→ TTC/R-TPT; padding detection + adaptation</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2512.16523) | [💻](https://github.com/lizhiwei23/TTP) |
| 2026 | AGC | AGC: Adaptive Geodesic Correction for Adversarial Robustness on Vision-Language Models <br/><sub>geodesic correction anchors</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2605.15584) | [💻](https://github.com/lizhiwei23/AGC) |

#### Embedding-Space Defense

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | **AOM** ⭐ | On the Zero-Shot Adversarial Robustness of Vision-Language Models: A Truly Zero-Shot and Training-Free Approach <br/><sub>anchor-guided one-step move; SoTA training-free</sub> | CVPR'25 | [📄](https://openaccess.thecvf.com/content/CVPR2025/papers/Tong_On_the_Zero-shot_Adversarial_Robustness_of_Vision-Language_Models_A_Truly_CVPR_2025_paper.pdf) | — |
| 2026 | CSR | Contrastive Spectral Rectification: Test-Time Defense towards Zero-shot Adversarial Robustness of CLIP | arXiv'26 | [📄](https://arxiv.org/abs/2601.19210) | [💻](https://github.com/Summu77/CSR) |
| 2026 | DBD | Adversarial attacks already tell the answer: Directional bias-guided test-time defense for vision-language models <br/><sub>directional bias-guided two-stream</sub> | ICLR'26 | [📄](https://openreview.net/forum?id=UqC2oFRRyc) | — |
| 2025 | COLA | Enhancing clip robustness via cross-modality alignment <br/><sub>cross-modality alignment + optimal transport</sub> | NeurIPS'25 | [📄](https://arxiv.org/abs/2510.24038) | — |
| 2026 | ET3 | A Provable Energy-Guided Test-Time Defense Boosting Adversarial Robustness of Large Vision-Language Models <br/><sub>energy-guided (also certified; +MLLM)</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2603.26984) | [💻](https://github.com/OmnAI-Lab/Energy-Guided-Test-Time-Defense) |
| 2025 | ATAC | ATAC: Augmentation-Based Test-Time Adversarial Correction for CLIP <br/><sub>augmentation-drift correction (lightweight)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2511.17362) | [💻](https://github.com/kylin0421/ATAC) |

#### Diffusion Purification

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | CLIPure | Clipure: Purification in latent space via clip for adversarially robust zero-shot classification <br/><sub>purification in CLIP latent space</sub> | ICLR'25 | [📄](https://arxiv.org/abs/2502.18176) | [💻](https://github.com/ZhangMingKun1/CLIPure) |
| 2025 | DiffCAP | Diffcap: Diffusion-based cumulative adversarial purification for vision language models <br/><sub>→ CLIPure; cumulative purification</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2506.03933) | — |
| 2025 | CoDefend | CoDefend: Cross-Modal Collaborative Defense via Diffusion Purification and Prompt Optimization <br/><sub>fine-tuned diffusion + prompt opt (MLLM)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2510.11096) | — |
| 2024 | MirrorCheck | Mirrorcheck: Efficient adversarial defense for vision-language models <br/><sub>T2I regeneration detection</sub> | NeurIPS'24 | [📄](https://arxiv.org/abs/2406.09250) | — |

#### MLLM-Specific (test-time)

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2026 | PDA | PDA: Text-Augmented Defense Framework for Robust Vision-Language Models against Adversarial Image Attacks <br/><sub>query paraphrase + answer aggregation</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2604.01010) | — |
| 2026 | OCRA | ORCA: An Agentic Reasoning Framework for Hallucination and Adversarial Robustness in Vision-Language Models <br/><sub>agentic Observe-Reason-Critique-Act loop</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2509.15435) | — |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 5 Certified Defense

Provable guarantees. Randomized smoothing is currently the only family that scales to VLMs; ET3 adds a conditional deterministic guarantee.

#### Randomized Smoothing

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2019 | RS | Certified adversarial robustness via randomized smoothing <br/><sub>randomized smoothing (foundation)</sub> | ICML'19 | [📄](https://arxiv.org/abs/1902.02918) | [💻](https://github.com/locuslab/smoothing) |
| 2025 | RS-VLM | Randomized Smoothing Meets Vision-Language Models <br/><sub>RS for generative VLMs via oracle classification</sub> | EMNLP'25 | [📄](https://aclanthology.org/2025.emnlp-main.1396/) | — |
| 2024 | OVC | Fast Certification of Vision-Language Models Using Incremental Randomized Smoothing <br/><sub>incremental RS for open-vocabulary CLIP</sub> | SaTML'24 | [📄](https://arxiv.org/abs/2311.09024) | — |

#### Conditional / Deterministic

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2026 | ET3 | A Provable Energy-Guided Test-Time Defense Boosting Adversarial Robustness of Large Vision-Language Models <br/><sub>energy-based conditional guarantee; single pass</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2603.26984) | [💻](https://github.com/OmnAI-Lab/Energy-Guided-Test-Time-Defense) |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 6 MLLM Robustness

Generative MLLMs add a three-stage attack surface (vision encoder → alignment module → LLM). Defenses are organized by the stage they protect.

#### Vision-Encoder Robustification

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | **FARE** ⭐ | Robust CLIP: Unsupervised Adversarial Fine-Tuning of Vision Embeddings for Robust Large Vision-Language Models <br/><sub>plug-and-play robust encoder → LLaVA/OpenFlamingo</sub> | ICML'24 | [📄](https://arxiv.org/abs/2402.12336) | [💻](https://github.com/chs20/RobustVLM) |
| 2026 | Sim-CLIP | Sim-clip: Unsupervised siamese adversarial fine-tuning for robust and semantically-rich vision-language models <br/><sub>Siamese robust encoder</sub> | IJCNN'26 | [📄](https://arxiv.org/abs/2407.14971) | [💻](https://github.com/speedlab-git/SimCLIP) |
| 2025 | SLADE | SLADE: Shielding against Dual Exploits in Large Vision-Language Models <br/><sub>+ optimization-based jailbreak defense</sub> | CVPR'25 | [📄](https://openaccess.thecvf.com/content/CVPR2025/html/Hossain_SLADE_Shielding_against_Dual_Exploits_in_Large_Vision-Language_Models_CVPR_2025_paper.html) | — |
| 2025 | CLIP-H | Double visual defense: Adversarial pre-training and instruction tuning for improving vision-language model robustness <br/><sub>adv pre-training + adv instruction tuning</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2501.09446) | [💻](https://github.com/zw615/Double_Visual_Defense) |
| 2026 | AdPO | Adpo: Enhancing the adversarial robustness of large vision-language models with preference optimization <br/><sub>DPO encoder; transfers to larger MLLMs</sub> | ICLR'26 | [📄](https://arxiv.org/abs/2504.01735) | — |

#### Alignment-Module Defense

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | Alignment Perturbation | Enhancing the robustness of vision-language foundation models by alignment perturbation <br/><sub>the only alignment-module (Q-Former) method — open gap</sub> | TIFS'25 | [📄](https://ieeexplore.ieee.org/document/11072221) | — |

#### Output-Level Defense

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | RCRT | Robust CLIP-Guided Deep Thinking: A Two-Stage Optimization Strategy for Enhancing Adversarial Robustness and Reliability in LVLMs <br/><sub>robust-CLIP verifier reranks MLLM outputs</sub> | ICASSP'25 | [📄](https://ieeexplore.ieee.org/document/10889297) | — |
| 2025 | Robust-VLGuard | Safeguarding Vision-Language Models: Mitigating Vulnerabilities to Gaussian Noise in Perturbation-based Attacks <br/><sub>safety fine-tuning + DiffPure-VLM</sub> | ICCV'25 | [📄](https://arxiv.org/abs/2504.01308) | [💻](https://github.com/JarvisUSTC/DiffPure-RobustVLM) |

#### Input-Level Defense & Detection

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2025 | CoDefend | CoDefend: Cross-Modal Collaborative Defense via Diffusion Purification and Prompt Optimization <br/><sub>diffusion purification + prompt optimization</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2510.11096) | — |
| 2024 | MirrorCheck | Mirrorcheck: Efficient adversarial defense for vision-language models <br/><sub>T2I-regeneration detection (training-free)</sub> | NeurIPS'24 | [📄](https://arxiv.org/abs/2406.09250) | — |
| 2026 | PDA | PDA: Text-Augmented Defense Framework for Robust Vision-Language Models against Adversarial Image Attacks <br/><sub>query-level paraphrase aggregation</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2604.01010) | — |
| 2026 | OCRA | ORCA: An Agentic Reasoning Framework for Hallucination and Adversarial Robustness in Vision-Language Models <br/><sub>agentic cross-validation</sub> | arXiv'26 | [📄](https://arxiv.org/abs/2509.15435) | — |

#### Analysis & Benchmarks

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2024 | LMM-Robustness | On the robustness of large multimodal models against image adversarial attacks <br/><sub>study: large multimodal models vs image attacks</sub> | CVPR'24 | [📄](https://arxiv.org/abs/2312.03777) | — |
| 2024 | Arch & Prompt | Improving adversarial robustness in vision-language models with architecture and prompt design <br/><sub>architecture & prompt-design effects</sub> | EMNLP-F'24 | [📄](https://aclanthology.org/2024.findings-emnlp.990/) | — |
| 2025 | I-ScienceQA | On the robustness of multimodal language model towards distractions <br/><sub>visual/textual distraction benchmark (8.1k)</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2502.09818) | — |
| 2025 | TTA-VLM | The illusion of progress? a critical look at test-time adaptation for vision-language models | NeurIPS'25 | [📄](https://arxiv.org/abs/2506.24000) | [💻](https://github.com/TomSheng21/tta-vlm) |
| 2025 | Driving-Robustness | Towards Trustworthy Autonomous Vehicles with Vision-Language Models under Targeted and Untargeted Adversarial Attacks <br/><sub>autonomous-driving VQA under attack</sub> | CVPRW'25 | [📄](https://doi.org/10.1109/CVPRW67362.2025.00067) | — |
| 2026 | Modality-Vuln | Understanding Modality-Specific Vulnerabilities in Vision--Language Models Under Adversarial Attacks | AI'26 | [📄](https://doi.org/10.3390/ai7040135) | — |

<div align="right"><sub><a href="#-table-of-contents">↑ back to top</a></sub></div>

## 7 Benchmarks & Evaluation

### Datasets

**Zero-shot classification** (the 15-dataset *TeCoA protocol*, used across AFT/APT/TTD/Certified):
ImageNet · Caltech101 · OxfordPets · StanfordCars · Flowers102 · Food101 · FGVCAircraft · SUN397 · DTD · EuroSAT · UCF101 · Caltech256 · CIFAR-10/100 · STL10 · ImageNet-V2/Sketch/A/R (distribution shift).

**Few-shot** (1/4/16-shot on 11 datasets) — used by APT/FAP; *no standardized splits → ±1–2% variance*.

**MLLM evaluation:** COCO Captions · VQAv2 · TextVQA · ScienceQA · POPE (hallucination) · I-ScienceQA (distraction) · autonomous-driving suite.

### Attack Protocols

| Protocol | ε (ℓ∞) | Steps | Target | Adoption |
|:--|:--:|:--:|:--:|:--|
| PGD-100 | 4/255 | 100 | CLIP | Standard |
| PGD-100 | 1/255 | 100 | CLIP | Conservative |
| PGD-10 | 4/255 | 10 | CLIP | Quick test |
| AutoAttack | 4/255 | adaptive | CLIP | **Gold standard** |
| CW∞ | 4/255 | adaptive | CLIP | Supplementary |
| APGD-NLL | 1–8/255 | adaptive | Gen. MLLM | Captioning |
| Jailbreak | 16–32/255 | adaptive | Gen. MLLM | Safety |

### Metrics & Reliability

- **Classification:** Clean Accuracy (CA) and Robust Accuracy (RA) under PGD / CW / AutoAttack.
- **Generative:** CIDEr/attack-success/hallucination rate for captioning & VQA.
- **Gradient masking** is a real risk — e.g., FAP reports 16.1% PGD but only 4.4% AutoAttack. **Always report AutoAttack**, both ε∈{1/255, 4/255}, adaptive attacks, and compute cost.

## 🔭 Related Surveys

| Year | Method | Title | Venue | Paper | Code |
|:--:|:--|:--|:--:|:--:|:--:|
| 2026 | Attack & Defense Survey | A survey of recent advances in adversarial attack and defense on vision-language models <br/><sub>recent advances in attack & defense on VLMs</sub> | Neural Networks'26 | [📄](https://www.sciencedirect.com/science/article/abs/pii/S0893608026002261) | — |
| 2025 | MLLM Robustness Survey | Survey of adversarial robustness in multimodal large language models <br/><sub>adversarial robustness in multimodal LLMs</sub> | arXiv'25 | [📄](https://arxiv.org/abs/2503.13962) | — |
| 2024 | Attacks Survey | A Survey of Attacks on Large Vision-Language Models: Resources, Advances, and Future Trends <br/><sub>attacks on large VLMs: resources & trends</sub> | arXiv'24 | [📄](https://arxiv.org/abs/2407.07403) | [💻](https://github.com/liudaizong/Awesome-LVLM-Attack) |
| 2025 | Defense Overview | Adversarial Defense in Vision-Language Models: An Overview <br/><sub>adversarial defense in VLMs: an overview</sub> | ICICML'25 | [📄](https://arxiv.org/abs/2601.12443) | — |

## 🚧 Open Challenges

1. **The persistent clean–robust trade-off.** Gap narrowed from ~20% (TeCoA, 2023) to ~5% (GRACE/Hier. Robust). *Can any method exceed 30% AutoAttack robustness at ε=4/255 without dropping clean accuracy below 55%?*
2. **Scalability to larger models.** Most defenses are evaluated on ViT-B/16 or ViT-B/32. *Does the trend hold to ViT-G/14+? Can weak-to-strong transfer (Adv-W2S) scale beyond 10B params?*
3. **The multi-modal attack surface.** MAT/MMCoA show multi-modal attacks are necessary, yet only **LEAF** robustifies the text encoder. *No defense is evaluated against jointly-optimized image+text adaptive attacks.*
4. **Certified robustness for VLMs.** Only a handful of methods (RS-VLM, OVC, ET3). *Can compositional certification exploit CLIP's dual-encoder structure better than randomized smoothing?*
5. **MLLM defense beyond the vision encoder.** 5 encoder methods, **1** alignment-module method, **0** for the LLM decoder. *Can adversarial LoRA on the Q-Former (~100M params) match encoder robustification?*
6. **Evaluation standardization.** *How many published gains survive mandatory AutoAttack + adaptive re-evaluation?*
7. **Real-world deployment.** All evaluation uses digital ℓ∞ perturbations. *Do these defenses transfer to physical patches / printed perturbations?*
8. **Toward unified robustness.** *Can one framework be robust to adversarial perturbations, natural corruptions, distribution shift, and backdoors simultaneously?*

## 🤝 Contributing

Contributions are very welcome! Please read **[CONTRIBUTING.md](CONTRIBUTING.md)** for the entry format and placement rules (group by category/sub-family, keep genealogy order, mark the method you improve upon). Open a PR or an issue to add a paper, fix a link, or add a `Code` link.

## 📜 Citation

If this repository or our survey helps your research, please consider citing:

```bibtex
@article{vlm_adv_robustness_survey,
  title   = {A Survey of Adversarial Robustness for Vision-Language Models:
             From Fine-Tuning to Test-Time Defense},
  author  = {TODO},
  journal = {arXiv preprint arXiv:TODO},
  year    = {2026}
}
```

## 🙏 Acknowledgments

Repository structure is inspired by [Awesome-LLM](https://github.com/Hannibal046/Awesome-LLM), [Awesome-RL-for-LRMs](https://github.com/TsinghuaC3I/Awesome-RL-for-LRMs), and [Awesome-Mental-Health-LLMs](https://github.com/). We thank the authors of all surveyed papers.

<!-- Activate after publishing (replace OWNER):
## ⭐ Star History
[![Star History Chart](https://api.star-history.com/svg?repos=OWNER/Awesome-VLM-Adversarial-Robustness&type=Date)](https://star-history.com/#OWNER/Awesome-VLM-Adversarial-Robustness&Date) -->

## 📄 License

Released under the [MIT License](LICENSE).

---

<sub>Indexing **130+ entries** (some methods cross-listed across defense stages) in 7 categories. Maintained by hand — corrections and additions welcome.</sub>
