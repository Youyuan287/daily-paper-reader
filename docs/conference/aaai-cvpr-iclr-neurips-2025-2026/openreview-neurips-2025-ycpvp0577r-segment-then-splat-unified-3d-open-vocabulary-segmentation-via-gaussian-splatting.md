---
title: "Segment then Splat: Unified 3D Open-Vocabulary Segmentation via Gaussian Splatting"
title_zh: "先分割再溅射: 基于高斯溅射的统一三维开放词汇分割"
authors: "Yiren Lu, Yunlai Zhou, Yiran Qiao, Chaoda Song, Tuo Liang, Jing Ma, Huan Wang, Yu Yin"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=ycPVp0577R"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 基于高斯溅射的三维开放词汇分割，支持静态和动态场景
tldr: 现有方法依赖二维像素级解析导致多视图不一致，且难以处理动态场景。Segment then Splat基于高斯溅射，先划分高斯点成物体集再进行重建，实现三维感知的开放词汇分割。该方法同时适用于静态和动态场景，克服了多视图不一致问题，在物体检索等任务上表现优异。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法多视图不一致且无法处理动态场景，需要三维感知的开放词汇分割。
method: 反转传统先重建后分割流程，在重建前将高斯点划分为物体集合。
result: 实现了静态和动态场景下的高质量开放词汇分割，多视图一致性显著提升。
conclusion: Segment then Splat为开放词汇三维分割提供了统一框架，适用于机器人等应用。
---

## Abstract
Open-vocabulary querying in 3D space is crucial for enabling more intelligent perception in applications such as robotics, autonomous systems, and augmented reality. However, most existing methods rely on 2D pixel-level parsing, leading to multi-view inconsistencies and poor 3D object retrieval. Moreover, they are limited to static scenes and struggle with dynamic scenes due to the complexities of motion modeling.
In this paper, we propose Segment then Splat, a 3D-aware open vocabulary segmentation approach for both static and dynamic scenes based on Gaussian Splatting. 
Segment then Splat reverses the long established approach of "segmentation after reconstruction'' by dividing Gaussians into distinct object sets before reconstruction. Once reconstruction is complete, the scene is naturally segmented into individual objects, achieving true 3D segmentation. This design eliminates both geometric and semantic ambiguities, as well as Gaussian–object misalignment issues in dynamic scenes. It also accelerates the optimization process, as it eliminates the need for learning a separate language field.
After optimization, a CLIP embedding is assigned to each object to enable open-vocabulary querying. Extensive experiments on various datasets demonstrate the effectiveness of our proposed method in both static and dynamic scenarios.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文信息生成的中文总结。

---

## 论文总结：《先分割再溅射：基于高斯溅射的统一三维开放词汇分割》

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：三维空间中的开放词汇查询对于机器人、自主系统和增强现实等应用至关重要。然而，现有方法大多依赖二维像素级解析，导致多视图不一致和三维物体检索效果差；同时，这些方法局限于静态场景，难以处理动态场景中复杂的运动建模。
- **整体含义**：本文提出了一种基于高斯溅射（3D Gaussian Splatting）的3D感知开放词汇分割方法，旨在同时解决静态和动态场景下的分割问题，实现真正的三维感知和统一框架。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：**反转传统的“先重建后分割”流程**，改为“先分割再重建”。即在三维高斯重建之前，先将高斯点划分为不同的物体集合；重建完成后，场景自然地被分割为独立物体，从而实现真正的三维分割。
- **关键技术细节**：
    - **分割前置**：在优化高斯溅射参数前，利用某种方式（论文未详述，但可能基于几何或语义线索）将高斯点划分到不同物体集。
    - **重建与分割同步**：高斯溅射的重建过程同时优化每个物体的几何和外观，消除了传统方法中几何与语义的歧义性，以及动态场景中高斯-物体对齐问题。
    - **开放词汇查询**：优化结束后，为每个物体分配一个CLIP嵌入（embedding），从而支持开放词汇查询（无需学习单独的语义语言场）。
    - **加速优化**：由于不需要额外学习语言场，优化过程更快。
- **算法流程**（文字说明）：
    1. 输入多视图图像（或视频序列）。
    2. 在初始化高斯点云时，预先将点划分到若干物体集合（例如通过可学习的分组或运动线索）。
    3. 对每个物体集合独立优化高斯溅射参数（位置、协方差、颜色、不透明度）。
    4. 重建完成后，每个物体集合对应一个3D物体。
    5. 对每个物体提取CLIP特征（通过渲染的2D视图或直接投影），并建立索引。
    6. 开放词汇查询时，将文本提示与物体CLIP特征匹配，返回对应物体。

### 3. 实验设计

- **数据集与场景**：论文在**多个数据集**上进行了广泛实验，涵盖静态场景和动态场景。具体提及的数据集名称未在摘要中给出，但通常三维场景分割或动态新视图合成数据集会被使用。
- **基准（Benchmark）**：未明确说明使用的基准，但可能包括常见的室内/室外场景分割数据集或动态场景重建数据集。
- **对比方法**：未具体列出，但推测对比了基于二维分割的后处理方法和静态场景下的三维分割方法。

### 4. 资源与算力

- **未明确说明**：论文摘要和元数据中未提及使用的GPU型号、数量或训练时长等算力信息。推测在完整论文中可能有相关描述，但此处无法获取。

### 5. 实验数量与充分性

- **实验数量**：论文声称进行了“extensive experiments on various datasets”，但未给出具体数量或消融实验细节。
- **充分性评估**：基于现有信息，实验覆盖了静态和动态两类场景，但缺少详细的消融实验（如分割前置策略的影响、CLIP嵌入效果等）和对不同复杂场景的定量分析。由于信息有限，难以判断实验是否完全充分和公平。但接受为NeurIPS 2025论文，通常有较严格的实验验证。

### 6. 论文的主要结论与发现

- **主要结论**：所提出的“Segment then Splat”方法能够：
    - 实现静态和动态场景下的高质量开放词汇分割。
    - 显著提升多视图一致性。
    - 消除几何和语义歧义，解决动态场景中的高斯-物体对齐问题。
    - 加速优化过程，无需学习独立语言场。
- **发现**：反转传统流程是有效的，先分割后重建能够获得更鲁棒的3D感知语义分割。

### 7. 优点

- **方法创新性**：颠覆了“先重建后分割”的范式，提出了“先分割后重建”的新思路，从源头解决多视图不一致和对齐问题。
- **统一性**：首次将静态场景和动态场景纳入同一框架，适用于机器人等需要处理动态环境的应用。
- **效率提升**：避免了额外学习语言场，减少了优化负担。
- **真正三维感知**：分割结果直接基于3D高斯物体集，而非后处理，避免了2D错误累积。

### 8. 不足与局限

- **实验覆盖不透明**：摘要中未详细报告数据集、具体评估指标、与基线方法的定量对比结果，读者无法直接判断性能优势。
- **算力资源缺失**：未提到训练所需的计算资源，可能影响可复现性。
- **应用限制**：
    - 可能依赖高质量的多视图输入或动态场景的时序信息。
    - 对物体边界的精确划分可能仍受限于高斯溅射的表示能力（如透明物体、复杂拓扑）。
    - 开放词汇能力依赖于CLIP，对于细粒度类别或晦涩概念可能受限。
- **潜在偏差风险**：未讨论场景类别、遮挡、尺度变化等方面的鲁棒性；未提及消融实验，难以确定每个组件的贡献。

（完）
