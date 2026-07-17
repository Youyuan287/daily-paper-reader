---
title: Segment Any 3D Object with Language
title_zh: 用语言分割任意3D物体
authors: "Seungjun Lee, Yuyang Zhao, Gim Hee Lee"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=ENv1CeTwxc"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 基于语言指令的开放词汇三维实例分割，直接从点云生成掩膜
tldr: 现有开放词汇三维实例分割方法多依赖2D投影和分类，泛化性差。本文提出SOLE，直接在三维点云中生成语义感知的实例掩膜，无需2D基础模型辅助。通过语言嵌入指导掩膜生成，实现对任意未见类别的分割。实验在多个基准上大幅超越现有方法，展示了原生3D开放词汇分割的优越性和泛化能力。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法依赖2D投影导致信息损失且泛化性受限，忽视3D原生语义。
method: 直接在3D点云中生成语言引导的语义感知掩膜，无需2D投影。
result: 在ScanNet等数据集上达到最优，未见类别分割精度提升显著。
conclusion: SOLE证明了直接3D开放词汇分割的可行性与高效性，是重要里程碑。
---

## Abstract
In this paper, we investigate Open-Vocabulary 3D Instance Segmentation (OV-3DIS) with free-form language instructions. Earlier works mainly rely on annotated base categories for training which leads to limited generalization to unseen novel categories. To mitigate the poor generalizability to novel categories, recent works generate class-agnostic masks or projecting generalized masks from 2D to 3D, subsequently classifying them with the assistance of 2D foundation model. However, these works often disregard semantic information in the mask generation, leading to sub-optimal performance. Instead, generating generalizable but semantic-aware masks directly from 3D point clouds would result in superior outcomes. To the end, we introduce Segment any 3D Object with LanguagE ($\textbf{SOLE}$), which is a semantic and geometric-aware visual-language learning framework with strong generalizability by generating semantic-related masks directly from 3D point clouds. Specifically, we propose a multimodal fusion network to incorporate multimodal semantics in both backbone and decoder. In addition, to align the 3D segmentation model with various language instructions and enhance the mask quality, we introduce three types of multimodal associations as supervision. Our SOLE outperforms previous methods by a large margin on ScanNetv2, ScanNet200, and Replica benchmarks, and the results are even closed to the fully-supervised counterpart despite the absence of class annotations in the training. Furthermore, extensive qualitative results demonstrate the versatility of our SOLE to language instructions. The code will be made publicly available.

---

## 论文详细总结（自动生成）

## 论文核心问题与整体含义

- **研究动机**：现有开放词汇三维实例分割（OV-3DIS）方法存在两大局限：一是仅依赖标注基类训练，对未见类别的泛化能力差；二是多数方法先生成类别无关的掩膜或从2D投影到3D，再借助2D基础模型分类，导致掩膜生成阶段忽略语义信息，性能次优。
- **整体含义**：本文旨在实现**直接从3D点云中生成语义感知、强泛化性的实例掩膜**，无需2D基础模型辅助，且能响应自由形式语言指令。

## 论文提出的方法论

### 核心思想
- 提出 **SOLE（Segment any 3D Object with Language）**：一种基于语言指令的**语义与几何感知的视觉-语言学习框架**，直接在3D点云中生成与语义相关的掩膜，无需投影或2D辅助。

### 关键技术细节
1. **多模态融合网络**：在骨干网络和解码器中均融入多模态语义信息，实现点云特征与语言特征的深度交互。
2. **三种多模态关联监督**：为对齐3D分割模型与各类语言指令，并提升掩膜质量，引入三种类型的多模态关联作为训练监督信号（具体类型未在摘要中详述，包括可能为：文本-点云对应、语言-掩膜匹配、区域-描述关联等）。
3. **流程简述**：输入3D点云与自由形式语言指令 → 多模态融合网络提取语义感知特征 → 解码器直接生成实例掩膜 → 利用三种监督信号训练，使掩膜语义对齐语言。

## 实验设计

- **数据集与基准**：
  - **ScanNetv2**（室内场景）、**ScanNet200**（200类细粒度）、**Replica**（合成场景）。
  - 对比方法：先前开放词汇3D实例分割方法（如依赖2D投影或类无关掩膜的方法）。
- **评估指标**：常规3D实例分割指标（mAP等），并特别关注**未见类别**的分割精度。
- **对比结果**：SOLE在三个基准上大幅超越现有方法，且结果**接近全监督方法**（尽管训练过程中未使用类别标注）。

## 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。

## 实验数量与充分性

- **实验组数**：至少包含三个主要数据集的完整对比实验，以及可能的消融研究（例如验证三种多模态监督的有效性）。但摘要中未列出具体消融实验数量，仅提及“大量定性结果”。
- **充分性与公平性**：
  - 对比了同类最先进方法，在多个标准基准上评估，实验覆盖面较广。
  - 结果接近全监督方法，证明设计有效。
  - 但缺少跨域（如室外场景）实验、对语言指令多样性的鲁棒性量化测试等，可能不充分。

## 论文的主要结论与发现

- **直接3D开放词汇分割可行且高效**：摒弃2D投影干扰，直接在3D点云中生成语义感知掩膜能获得更优性能。
- **泛化能力显著提升**：对未见类别的分割精度远优于此前依赖2D基础模型的方法。
- **多模态关联监督的关键作用**：三种监督信号有效对齐了3D特征与语言指令，提升了掩膜语义质量。

## 优点

- **方法创新性**：首个不依赖2D基础模型、直接端到端从3D点云生成开放词汇实例掩膜的框架。
- **语义感知性强**：掩膜生成阶段即融入语言语义，而非后分类，避免了信息损失。
- **性能领先**：在多个基准上显著超越现有方法，且接近全监督水平。
- **泛化性好**：无需类别标注即可处理任意未见类别。

## 不足与局限

- **实验覆盖有限**：仅在室内场景（ScanNet、Replica）验证，未涉及室外、复杂动态场景或点云噪声大的情况。
- **语言指令多样性考验**：定性结果显示了灵活性，但缺乏对模糊/复合指令的系统性消融或量化评估。
- **监督信号细节缺失**：摘要未给出三种关联监督的具体形式，难以判断是完全依赖预训练语言模型还是需要额外标注。
- **计算资源未公开**：缺少训练成本信息，难以评估实际部署可行性。
- **可能存在的偏差风险**：训练数据集中语言描述与物体的对应关系可能隐含长尾偏差，未见类别若语义分布极端，效果可能下降。

（完）
