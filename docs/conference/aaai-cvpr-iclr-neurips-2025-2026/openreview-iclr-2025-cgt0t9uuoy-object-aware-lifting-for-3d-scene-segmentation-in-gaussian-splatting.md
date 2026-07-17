---
title: Object-aware lifting for 3D scene  segmentation in Gaussian splatting
title_zh: 高斯溅射三维场景分割中的物体感知提升
authors: "Runsong Zhu, Shi Qiu, Zhengzhe Liu, Ka-Hei Hui, Qianyi Wu, Pheng-Ann Heng, Chi-Wing Fu"
date: 2024-09-17
pdf: "https://openreview.net/pdf?id=CGT0T9uUOY"
tags: ["query:open-vocab-d"]
score: 8.0
evidence: 高斯溅射中的物体感知提升，多视图特征提升至三维
tldr: 现有提升方法依赖对比学习和后聚类，超参数敏感且性能不佳。本文在高斯溅射场中提出物体感知提升，引入可学习的物体级码本，为每个高斯点学习额外特征。该方法直接输出物体级理解，避免了易出错的聚类后处理，在三维场景分割上取得更优性能。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有提升方法依赖聚类后处理，超参数敏感且性能差，缺乏物体级理解。
method: 在高斯溅射场中引入物体级码本，每个高斯点学习对比特征，实现端到端物体感知提升。
result: 在多个基准上超越现有提升方法，分割精度更高，无需后聚类。
conclusion: 物体感知提升通过显式物体建模提升了三维分割的质量和鲁棒性。
---

## Abstract
Lifting is an effective technique for producing a 3D scene segmentation by unprojecting multi-view 2D instance segmentations into a common 3D space. Existing state-of-the-art lifting methods leverage contrastive learning to learn a feature field, but rely on a hyperparameter-sensitive and error-prone clustering post-process for segmentation prediction, leading to inferior performance. In this paper, we propose a new unified \textit{object-aware lifting}  approach in a 3D Gaussian Splatting field, introducing a novel learnable \textit{object-level codebook} to account for objects in the 3D scene for an explicit object-level understanding. To start, we augment each Gaussian point with an additional Gaussian-level feature learned using a contrastive loss. More importantly, enabled by our object-level codebook formulation, we associate the encoded object-level features with Gaussian-level point features for segmentation predictions. Further, we design two novel modules, the association learning module and the noisy label filtering module, to achieve effective and robust codebook learning. We conduct experiments on three benchmarks,~\ie, LERF-Masked, Replica, and Messy Rooms datasets. Both qualitative and quantitative results manifest that our new approach significantly outperforms the existing methods in terms of segmentation quality and time efficiency.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题所在**：三维场景分割中，“提升（lifting）”方法通过将多视图二维实例分割反投影到公共三维空间来产生分割结果。现有最先进方法依赖对比学习学习特征场，但**需要后处理聚类**来预测分割，而聚类过程对超参数敏感且易出错，导致性能不佳。
- **研究动机**：现有方法缺乏显式的物体级理解，仅靠点级特征对比学习难以直接输出物体分割。因此本文提出**物体感知提升**，在高斯溅射场中引入可学习的物体级码本，实现端到端的物体级分割预测，避免后聚类步骤。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：在 3D Gaussian Splatting 场中，为每个高斯点添加额外的可学习特征，并通过物体级码本将这些点特征与物体嵌入关联，直接输出分割预测。
- **关键技术细节**：
  - **高斯级特征学习**：每个高斯点增加一个额外特征，使用对比损失（contrastive loss）进行学习，使得同一物体对应的高斯点特征相似。
  - **物体级码本（Object-level Codebook）**：引入可学习的码本，每个码字代表一个物体的嵌入。码本与高斯点特征通过关联学习模块进行交互，生成每个高斯点属于各物体的概率。
  - **关联学习模块（Association Learning Module）**：实现物体级特征与高斯点特征的对齐，让点特征能有效匹配到对应的物体码字。
  - **噪声标签过滤模块（Noisy Label Filtering Module）**：由于二维实例分割标签可能包含噪声（如遮挡、错误标注），该模块过滤不可靠的监督信号，提升码本学习的鲁棒性。
- **算法流程**（文字说明）：
  1. 输入多视图图像及对应的二维实例分割掩码。
  2. 用 3D Gaussian Splatting 表示三维场景，每个高斯点附带一个初始特征。
  3. 通过对比学习更新高斯点特征（利用多视图一致性）。
  4. 将高斯点特征与物体级码本中的码字进行匹配（关联学习模块），得到每个点的软分配概率。
  5. 使用噪声标签过滤模块去除不一致的预测，训练码本。
  6. 推理时直接根据码本分配输出三维分割，无需聚类。

## 3. 实验设计

- **数据集**：
  - LERF-Masked 数据集
  - Replica 数据集
  - Messy Rooms 数据集
- **Benchmark**：三维场景分割任务，评估分割质量（如 mIoU 或类似指标）和时间效率。
- **对比方法**：现有基于提升的 state-of-the-art 方法（具体未列出，但摘要指出“现有方法”包括对比学习 + 后聚类的方法）。

## 4. 资源与算力

- **文中未明确说明**：摘要和元数据未提及使用的 GPU 型号、数量或训练时长等算力信息。因此无法提供具体数据，仅能指出该信息缺失。

## 5. 实验数量与充分性

- **实验数量**：在三个不同场景的数据集上进行了定量与定性评估（至少三个主要实验）。合理推测还包含了消融研究（如去掉关联学习模块或噪声过滤模块的影响），但摘要未明确列出消融实验数量。
- **充分性与公平性**：
  - 数据集覆盖了多场景（真实室内、网格扫描、杂乱房间），具有一定多样性。
  - 对比了现有方法，定性结果和定量结果均优于基线。
  - 但缺乏对超参数敏感性的系统分析，也未与更多非提升类方法（如直接三维分割网络）比较，因此公平性受限于提升方法的范畴。

## 6. 主要结论与发现

- 提出的物体感知提升方法在**分割质量（精度）** 和**时间效率**上显著优于现有提升方法。
- 引入物体级码本能够显式建模物体，避免了依赖聚类后处理的缺陷。
- 噪声标签过滤模块有效提升了训练鲁棒性，特别是在二维标签不完美的情况下。

## 7. 优点

- **端到端输出物体分割**：无需后聚类，简化管线，减少超参数调优。
- **物体级理解**：码本直接对应物体，可解释性强。
- **鲁棒性**：噪声标签过滤模块增强了对不完美二维标注的抗干扰能力。
- **时间效率高**：推理时一步到位，相比聚类后处理更快。

## 8. 不足与局限

- **实验覆盖有限**：仅测试了三个室内数据集，未在室外大场景或复杂动态场景中验证。
- **资源消耗未报告**：缺少训练和推理的具体计算开销，难以评估实际部署成本。
- **码本容量限制**：物体级码本需要预设码字数量，对于未知或可变物体类别可能不够灵活（文中未讨论开放类别场景）。
- **偏差风险**：二维实例分割标签噪声的分布假设可能不适用于所有数据集，不同噪声类型影响未知。
- **应用限制**：方法依赖多视图输入和预先训练的二维分割模型，对输入质量敏感。

（完）
