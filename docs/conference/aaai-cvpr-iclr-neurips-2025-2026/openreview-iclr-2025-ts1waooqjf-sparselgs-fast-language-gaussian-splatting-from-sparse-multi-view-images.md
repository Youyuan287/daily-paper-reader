---
title: "SparseLGS: Fast Language Gaussian Splatting from Sparse Multi-View Images"
title_zh: "SparseLGS: 从稀疏多视图图像快速语言高斯泼溅"
authors: "Kangjie Chen, BingQuan Dai, Minghan Qin, DongbinZhang, Peihao Li, Haoqian Wang"
date: 2024-09-19
pdf: "https://openreview.net/pdf?id=Ts1waOOQjF"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 从稀疏视图快速构建开放词汇3D语义场的高斯泼溅
tldr: 现有3D语义场学习方法依赖多视图密集优化，在稀疏视图下效率低。本文提出SparseLGS，一种前馈式方法，采用3D高斯泼溅、视频跟踪一致的SAM分割和低维CLIP特征索引，从稀疏视图快速构建3D语言场。实验表明在稀疏视图下仍能准确分割，推理速度快，适用于自动驾驶和机器人等实时场景。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有3D语义场学习方法在稀疏视图下效率低，不实用。
method: 提出SparseLGS，利用视频跟踪一致的SAM分割和低维CLIP索引，前馈式构建高斯泼溅语义场。
result: 在稀疏视图下实现快速准确的3D语义分割，推理速度快。
conclusion: 前馈式高斯泼溅语义场构建方法适用于实时场景。
---

## Abstract
3D semantic field learning is crucial for applications like autonomous navigation, AR/VR, and robotics, where accurate comprehension of 3D scenes from limited viewpoints is essential. Existing methods struggle under sparse view conditions, relying on inefficient per-scene multi-view optimizations, which are impractical for many real-world tasks. To address this, we propose SparseLGS, a feed-forward method for constructing 3D semantic fields from sparse viewpoints, allowing direct inference of 3DGS-based scenes. By ensuring consistent SAM segmentations through video tracking and using low-dimensional indexing for high-dimensional CLIP features, SparseLGS efficiently embeds language information in 3D space, offering a robust solution for accurate 3D scene understanding under sparse view conditions. In experiments on two-view sparse 3D object querying and segmentation in the LERF and 3D-OVS datasets, SparseLGS outperforms existing methods in chosen IoU, Localization Accuracy, and mIoU. Moreover, our model achieves scene inference in under 30 seconds and open-vocabulary querying in just 0.011 seconds per query.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：3D语义场学习对于自动驾驶、AR/VR和机器人等应用至关重要，需要在有限视角下准确理解3D场景。现有方法（如基于NeRF或3DGS的语义场）依赖逐场景的多视图密集优化，在稀疏视图条件下效率极低，难以满足实时任务需求。
- **核心问题**：如何从稀疏多视图（如仅两视图）快速构建开放词汇的3D语义场，实现实时推理和查询。
- **整体含义**：提出一种前馈式（feed-forward）方法SparseLGS，避免逐场景优化，直接基于3D高斯泼溅（3DGS）推理构建语义场，兼顾速度与精度。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将语言信息高效嵌入3D空间，通过前馈网络直接从稀疏视图生成3DGS表示，并利用低维索引压缩高维CLIP特征。
- **关键技术细节**：
  - **3D高斯泼溅**：作为场景表示，支持快速渲染和推理。
  - **视频跟踪一致的SAM分割**：利用视频跟踪技术在不同视图间保持SAM（Segment Anything Model）分割的一致性，确保多视图语义对应。
  - **低维索引高维CLIP特征**：将高维CLIP语言-视觉特征通过低维索引进行压缩和存储，减少存储和计算开销，同时保留开放词汇查询能力。
- **算法流程**（文字说明）：
  1. 输入稀疏多视图图像（如两视图）。
  2. 使用预训练模型提取图像特征，并通过视频跟踪获得一致的SAM分割掩码。
  3. 将分割结果与图像特征结合，通过前馈网络预测3D高斯参数（位置、协方差、颜色等）及其对应的低维CLIP特征索引。
  4. 构建3DGS场景，支持任意开放词汇查询：将查询文本通过CLIP编码后，与场景中存储的低维索引进行匹配，实现语义分割或目标定位。

### 3. 实验设计：数据集、基准与对比方法

- **数据集**：LERF（大规模室外场景）和3D-OVS（开放词汇3D语义分割）数据集。
- **基准任务**：
  - 两视图稀疏3D目标查询（object querying）
  - 两视图稀疏3D语义分割（segmentation）
- **评估指标**：选择的IoU（Chosen IoU）、定位精度（Localization Accuracy）、平均交并比（mIoU）。
- **对比方法**：未在摘要中列出具体名称，但提到“现有方法”均基于多视图密集优化，SparseLGS在各项指标上均超越它们。

### 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。仅在结论中提到“场景推理时间小于30秒，每查询仅需0.011秒”，暗示推理阶段对算力需求较低，但训练阶段资源未知。

### 5. 实验数量与充分性

- **实验数量**：摘要提及两个数据集（LERF和3D-OVS），每个数据集包含稀疏两视图的实验。未明确列出消融实验或更多变体。
- **充分性**：从摘要看，实验覆盖了主要基准任务，但缺少对更多视图（如3-5视图）或更大规模数据集的评估。也未报告与更多基线方法的详细对比表格。消融研究（如是否使用视频跟踪一致性、低维索引的影响）未提及，可能存在于完整论文中。整体实验**较为初步**，公开信息不足以全面判断充分性。

### 6. 论文的主要结论与发现

- SparseLFS在稀疏视图（两视图）下，能够快速、准确地构建3D语义场，在开放词汇查询和分割任务上优于现有依赖密集优化的方法。
- 推理速度显著优于传统方法：场景构建<30秒，每查询0.011秒，适用于实时场景（如自动驾驶、机器人）。
- 验证了前馈式高斯泼溅+视频跟踪一致分割+低维索引的有效性。

### 7. 优点：方法或实验设计上的亮点

- **前馈式设计**：避免逐场景优化，直接推理，实现实时性。
- **稀疏视图友好**：仅需两视图即可工作，大幅降低数据采集成本。
- **开放词汇能力**：通过CLIP特征索引支持任意文本查询，灵活性高。
- **高效率**：推理时间极短，适合实际部署。

### 8. 不足与局限

- **实验覆盖不足**：仅评估两视图场景，未验证更多稀疏视图（如3-5视图）或极端稀疏（单视图）下的性能。
- **缺失消融研究**：未公开视频跟踪一致性、低维索引等关键组件的独立贡献分析。
- **资源与可重复性**：未报告训练细节，难以复现和对比。
- **应用局限**：可能对场景复杂度有要求（如LERF室外场景），在更复杂室内场景或动态场景中表现未知。
- **潜在偏差**：仅使用两个特定数据集，泛化性需进一步验证。

（完）
