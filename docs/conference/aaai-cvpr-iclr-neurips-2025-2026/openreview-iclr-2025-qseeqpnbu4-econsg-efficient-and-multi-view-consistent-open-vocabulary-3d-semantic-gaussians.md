---
title: "econSG: Efficient and Multi-view Consistent Open-Vocabulary 3D Semantic Gaussians"
title_zh: econSG：高效且多视角一致的开放词汇三维语义高斯场
authors: "Can Zhang, Gim Hee Lee"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=qSEEQPNbu4"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 高效多视图一致的开放词汇三维语义高斯场，含置信区域引导正则化
tldr: 现有开放词汇神经场大多依赖SAM正则化CLIP且降维造成多视图不一致。本文提出econSG，基于3D高斯泼溅，设计置信区域引导正则化（CRR）相互精炼SAM和CLIP特征，实现高效且多视图一致的语义场。在多个基准上达到领先分割精度。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有开放词汇3D语义场存在多视图不一致和效率低的问题。
method: 提出置信区域引导正则化(CRR)，相互精炼SAM和CLIP特征，保持多视图一致。
result: 在多个数据集上取得开放词汇分割最优性能，且效率高。
conclusion: 为开放词汇三维场景理解提供高效且一致的语义高斯场方法。
---

## Abstract
The primary focus of most recent works on open-vocabulary neural fields is extracting precise semantic features
from the VLMs and then consolidating them efficiently into a multi-view consistent 3D neural fields
representation. However, most existing works over-trusted SAM to regularize image-level CLIP without any further refinement. Moreover, several existing works improved efficiency by dimensionality reduction of semantic features from 2D VLMs before fusing with 3DGS semantic fields, which inevitably leads to multi-view inconsistency. In this work, we propose econSG for open-vocabulary semantic segmentation with 3DGS. Our econSG consists of: 1) A Confidence-region Guided Regularization (CRR) that mutually refines SAM and CLIP to get the best of both worlds for precise semantic features with complete and precise boundaries. 2) A low dimensional contextual space to enforce 3D multi-view consistency while improving computational efficiency by fusing backprojected multi-view 2D features and follow by dimensional reduction directly on the fused 3D features instead of operating on each 2D view separately. Our econSG show state-of-the-art performance on four benchmark datasets compared to the existing methods. Furthermore, we are also the most efficient training among all the methods.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：现有开放词汇（open-vocabulary）三维语义场方法存在两个主要缺陷：① 大多数工作过度依赖 SAM 来正则化图像级 CLIP 特征，但未对 SAM 和 CLIP 进行进一步精炼，导致语义特征精度不够；② 部分方法为了提升效率，先对 2D 视觉语言模型（VLM）的语义特征降维，再融合到 3D 高斯泼溅语义场中，这不可避免地引入了多视图不一致性问题。
- **整体含义**：本文旨在构建一个高效且多视图一致的开放词汇三维语义高斯场，用于三维场景的开放词汇语义分割，克服现有方法的效率与一致性问题。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：基于 3D 高斯泼溅（3DGS）框架，提出**置信区域引导正则化（CRR，Confidence-region Guided Regularization）**，相互精炼 SAM 和 CLIP 特征，同时设计低维上下文空间以保持多视图一致性并提升计算效率。
- **关键技术细节**：
  - **CRR 模块**：不单向地使用 SAM 正则化 CLIP，而是让 SAM 和 CLIP 互相精炼，利用高置信度区域的特征互相增强，从而获得边界完整、语义精确的特征。
  - **多视图一致性处理**：不先对每个 2D 视图单独降维，而是先反向投影多视图 2D 特征并融合成 3D 特征，再直接在融合后的 3D 特征上进行降维，从而避免多视图不一致。
  - **效率优化**：通过低维上下文空间（low dimensional contextual space）压缩语义维度，同时保持多视图一致性，实现高效训练与推理。
- **公式/算法流程（文字说明）**：输入多视图图像 → 用预训练的 SAM 和 CLIP 提取 2D 特征 → 通过 CRR 模块相互精炼 → 反向投影并融合多视图特征到 3D 高斯场 → 在 3D 空间直接降维 → 训练语义高斯场 → 推理时直接查询高斯点获得语义标签。

## 3. 实验设计
- **数据集/场景**：在四个基准数据集上进行评估（具体名称未在摘要中列出，推测包括 ScanNet、Replica、Matterport3D 等常见室内/室外语义分割数据集）。
- **Benchmark**：开放词汇三维语义分割的常见基准，包括量化指标如 mIoU（平均交并比）等。
- **对比方法**：与现有开放词汇 3D 语义场方法（如基于 NeRF 的方法、基于 3DGS 的方法等）进行对比，结果显示本文方法达到最先进性能（SOTA）。

## 4. 资源与算力
- 摘要和元数据中**未明确说明**所使用的 GPU 型号、数量、训练时长等信息。仅定性提到“效率最高”（most efficient training among all the methods），但缺乏具体数值。需补充阅读全文才能获取详细算力配置。

## 5. 实验数量与充分性
- **实验数量**：至少进行了四个基准数据集上的完整对比实验，并可能包含消融实验（验证 CRR 和低维上下文空间的有效性）。元数据中的“result”提及在多个数据集取得最优性能，暗示实验较充分。
- **充分性与公平性**：对比了多种现有方法，并宣称效率最高，实验设计和指标选择应是客观的。但由于摘要信息有限，无法判断是否涵盖了所有主流基线、是否进行了统计显著性检验等。总体看，实验设置看起来合理。

## 6. 主要结论与发现
- 提出的 econSG 方法在开放词汇三维语义分割的四个基准上均达到**最先进性能**，同时训练效率最高。
- 置信区域引导正则化（CRR）能有效精炼 SAM 和 CLIP 特征，获得更精确的语义边界和语义特征。
- 在 3D 空间直接降维而非在 2D 视图上分别降维，能有效保持多视图一致性，避免降维引入的不一致。

## 7. 优点
- **方法创新**：CRR 相互精炼机制突破了以往单向约束的局限，是一个新颖的融合策略。
- **效率高**：通过低维上下文空间和 3D 级联降维，在保持高精度的同时显著加速训练，达到所有方法中最高效率。
- **多视图一致性**：直接解决了现有方法因 2D 降维导致的多视图不一致问题，提升了语义场的稳定性。
- **技术简练有效**：基于 3DGS 框架，结合成熟的 2D 视觉模型（SAM、CLIP），设计简洁但有效。

## 8. 不足与局限
- **实验覆盖未知**：摘要仅提及四个数据集，但未列出数据集名称和具体性能数值，无法判断模型在室外大场景、小样本、复杂遮挡等场景下的泛化能力。
- **算力信息缺失**：未提供训练所需 GPU 时间、内存等资源消耗，使得“效率最高”的说法缺乏定量支撑。
- **局限性未讨论**：论文可能未充分探讨 CRR 在低置信度区域或噪声标签下的鲁棒性，以及低维上下文空间对语义细节的保留程度。
- **偏差风险**：方法依赖于 SAM 和 CLIP 的预训练质量，如果遇到域外数据或对抗性样本，性能可能下降。
- **应用限制**：开放词汇分割依赖视觉语言模型的 embedding 空间，对于抽象概念或未见类别可能存在模糊性。

（完）
