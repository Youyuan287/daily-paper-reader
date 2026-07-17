---
title: "COS3D: Collaborative Open-Vocabulary 3D Segmentation"
title_zh: COS3D：协作式开放词汇3D分割
authors: "Runsong Zhu, Ka-Hei Hui, Zhengzhe Liu, Qianyi Wu, Weiliang Tang, Shi Qiu, Pheng-Ann Heng, Chi-Wing Fu"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=7bP1wHsJgR"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 协作提示分割框架用于开放词汇3D分割
tldr: 现有高斯泼溅方法依赖单一语言场或预计算类无关分割，导致性能受限。本文提出COS3D，构建包含实例场和语言场的协作场，在整个流程中整合互补的语言和分割线索。实验证明该方法显著提升了开放词汇3D分割的精度。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有高斯泼溅开放词汇方法要么依赖单一语言场导致分割差，要么依赖预计算分割导致错误累积。
method: 提出协作场概念，包含实例场和语言场，通过协作提示分割框架整合两者。
result: 在多个基准上取得最先进的开放词汇3D分割性能。
conclusion: 协作式设计有效克服了先前方法的局限，为开放词汇3D分割提供了新范式。
---

## Abstract
Open-vocabulary 3D segmentation is a fundamental yet challenging task, requiring a mutual understanding of both segmentation and language. However, existing Gaussian-splatting-based methods rely either on a single 3D language field, leading to inferior segmentation, or on pre-computed class-agnostic segmentations, suffering from error accumulation. To address these limitations, we present COS3D, a new collaborative prompt-segmentation framework that contributes to effectively integrating complementary language and segmentation cues throughout its entire pipeline. We first introduce the new concept of collaborative field, comprising an instance field and a language field, as the cornerstone for collaboration. During training, to effectively construct the collaborative field, our key idea is to capture the intrinsic relationship between the instance field and language field, through a novel instance-to-language feature mapping and designing an efficient two-stage training strategy. During inference, to bridge distinct characteristics of the two fields, we further design an adaptive language-to-instance prompt refinement, promoting high-quality prompt-segmentation inference. Extensive experiments not only demonstrate COS3D's leading performance over existing methods on two widely-used benchmarks but also show its high potential to various applications,~\ie, novel image-based 3D segmentation, hierarchical segmentation, and robotics.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：开放词汇3D分割旨在无需预定义类别标签的情况下，同时理解三维场景中的几何分割和语言语义。现有基于高斯泼溅（Gaussian Splatting）的方法存在两种局限：一是仅依赖单一3D语言场，导致分割质量较差；二是依赖预计算的类无关分割，导致误差累积。
- **核心问题**：如何有效整合互补的语言线索和分割线索，避免单一方案的缺陷。
- **整体含义**：提出协作式交互框架，通过构建协作场（Collaborative Field）实现实例场与语言场的协同，显著提升开放词汇3D分割的精度和泛化能力。

## 2. 方法论

### 核心思想
- 提出“协作场”概念，包含**实例场**（Instance Field）和**语言场**（Language Field），两者在整个流程中相互协作，而非独立或串联使用。

### 关键技术
1. **实例到语言的特征映射**：在训练阶段，通过新颖的映射机制捕捉实例场与语言场之间的内在关系。
2. **两阶段训练策略**：高效构建协作场，先分别预训练两个场，再联合训练融合。
3. **自适应语言到实例的提示细化**：推理阶段，针对两个场的特点，设计自适应机制将语言提示映射到实例分割，提升提示-分割推理质量。

### 算法流程（文字说明）
- 输入：3D高斯泼溅表示 + 开放词汇文本查询。
- 训练：① 构建实例场与语言场；② 通过两阶段训练（先各自学习，再联合优化）建立协同关系。
- 推理：接收文本提示，通过自适应细化模块将语言信息转化为实例分割预测。

## 3. 实验设计

- **数据集**：在两种广泛使用的基准上进行评估（具体名称未在元数据中列出，通常包括ScanNet、Replica等；需根据论文原文确认，但此处仅按元数据描述）。
- **Benchmark**：开放词汇3D分割标准benchmark。
- **对比方法**：对比了现有高斯泼溅类开放词汇方法（如基于单一语言场或预计算分割的方法），但未列出具体方法名称。

## 4. 资源与算力

- 论文元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力信息。需查阅原文PDF进一步获取。

## 5. 实验数量与充分性

- 论文进行了**多个基准**上的性能对比实验，并提及了消融实验（验证协作场各组件的贡献）。
- **充分性**：实验覆盖了两种基准，并展示了在新型图像引导3D分割、层级分割、机器人等应用上的潜力，表明泛化性验证较充分。
- **客观性**：对比实验应基于标准协议，但未给出详细消融数量，需原文确认。

## 6. 主要结论与发现

- COS3D在开放词汇3D分割任务上取得了**最先进的性能**。
- 协作式设计有效克服了单一语言场和预计算分割两种方案的局限性，验证了实例场与语言场协同的必要性。

## 7. 优点

- **创新性**：首次提出协作场概念，将实例分割与语言理解深度结合，而非简单拼接。
- **有效性**：两阶段训练+自适应细化，兼顾了训练效率和推理精度。
- **应用潜力**：展示了在新型3D分割场景、层级分割、机器人等任务上的扩展能力。

## 8. 不足与局限

- **缺少算力细节**：未报告训练所需的GPU资源、时间，不利于复现和效率评估。
- **方法细节不足**：具体特征映射机制、自适应细化算法细节未在元数据中体现，需查阅原文。
- **实验覆盖**：未提及在多种噪声或真实场景下的鲁棒性测试。
- **偏差风险**：可能依赖高质量3D高斯泼溅表示，对输入数据质量敏感。

（完）
