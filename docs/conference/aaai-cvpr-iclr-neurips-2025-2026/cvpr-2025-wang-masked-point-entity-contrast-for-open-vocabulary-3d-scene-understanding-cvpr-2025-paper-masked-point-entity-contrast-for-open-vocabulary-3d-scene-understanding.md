---
title: Masked Point-Entity Contrast for Open-Vocabulary 3D Scene Understanding
title_zh: 掩码点-实体对比用于开放词汇三维场景理解
authors: "Wang, Yan, Jia, Baoxiong, Zhu, Ziyu, Huang, Siyuan"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wang_Masked_Point-Entity_Contrast_for_Open-Vocabulary_3D_Scene_Understanding_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 通过掩码点-实体对比学习实现开放词汇三维语义分割
tldr: 开放词汇三维场景理解对具身智能至关重要。本文提出掩码点-实体对比学习（MPEC），利用三维实体-语言对齐和跨视图点-实体一致性，增强实体级特征表示。在ScanNet上取得开放词汇语义分割最优性能，零样本能力出色。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1809, \"height\": 772, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1795, \"height\": 752, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 523, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 857, \"height\": 650, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 615, \"height\": 254, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 777, \"height\": 268, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 683, \"height\": 340, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1805, \"height\": 209, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 864, \"height\": 274, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 835, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 529, \"height\": 235, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-masked-point-entity-contrast-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-009.webp\", \"caption\": \"\", \"page\": 0, \"index\": 9, \"width\": 865, \"height\": 319, \"label\": \"Table\"}]"
motivation: 现有方法在开放词汇三维分割中语义判别力不足，实例区分困难。
method: 提出掩码点-实体对比学习，结合三维实体-语言对齐和跨视图一致性。
result: 在ScanNet上取得开放词汇语义分割最优结果，零样本性能领先。
conclusion: 提供有效的对比学习框架，显著提升开放词汇三维场景理解能力。
---

## Abstract
Open-vocabulary 3D scene understanding is pivotal for enhancing physical intelligence, as it enables embodied agents to interpret and interact dynamically within real-world environments. This paper introduces MPEC, a novel Masked Point-Entity Contrastive learning method for open-vocabulary 3D semantic segmentation that leverages both 3D entity-language alignment and point-entity consistency across different point cloud views to foster entity-specific feature representations. Our method improves semantic discrimination and enhances the differentiation of unique instances, achieving state-of-the-art results on ScanNet for open-vocabulary 3D semantic segmentation and demonstrating superior zero-shot scene understanding capabilities. Extensive fine-tuning experiments on 8 datasets, spanning from low-level perception to high-level reasoning tasks, showcase the potential of learned 3D features, driving consistent performance gains across varied 3D scene understanding tasks.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：开放词汇三维场景理解要求模型不仅能识别训练中见过的预定义类别，还能根据自然语言指令识别任意灵活概念，同时保留足够的空间和几何信息以支持重建、操作等任务。现有方法大多依赖2D基础模型生成或蒸馏3D特征，但面临2D视图视野有限、多视图一致性难以保持、缺乏3D空间信息等问题，导致语义判别力不足、实例区分困难。
- **整体含义**：该研究旨在学习同时具备灵活语义理解能力和实体特定几何/空间信息的三维场景表示，以推动具身智能中三维视觉-语言（3D-VL）任务的突破。

## 2. 方法论
- **核心思想**：提出**掩码点-实体对比学习框架（MPEC）**，通过两个对齐目标联合训练3D编码器：
  1. **点-实体对齐（Point-to-Entity Alignment）**：对同一场景生成两个不同增强视图（随机掩码网格），利用现成3D分割模型获取实体掩码，然后跨视图进行点-实体对比学习：增强属于同一实体的点对之间的特征相似性，同时拉远不同实体（包括背景点）之间的距离。
  2. **实体-语言对齐（Entity-to-Language Alignment）**：将3D实体点特征与通过预训练CLIP文本编码器提取的文本描述（包括物体描述和空间关系描述）进行对比对齐，实现开放词汇理解。
- **关键技术细节**：
  - 使用稀疏卷积UNet（SPUNet）作为3D编码器，CLIP文本编码器冻结，仅训练3D编码器和视觉-语言适配器（两层MLP）。
  - 点-实体对比损失（Lp2e）采用对称交叉熵，实体-语言对比损失（Le2l）采用文本到实体的交叉熵损失和实体到文本的二元交叉熵损失（因一个实体可能对应多个描述）。
  - 训练目标为两者之和：L_overall = Lp2e + Le2l。
- **数据构建**：使用现成实例分割模型生成实体掩码，并利用场景图和2D基础模型生成物体描述与空间参照文本；训练数据来自SceneVerse数据集（包含ScanNet、MultiScan、3RScan、HM3D等真实场景以及部分合成数据）。

## 3. 实验设计
- **数据集与场景**：
  - **开放词汇语义分割**：ScanNet (20类)、ScanNet200 (200类)、Matterport3D、SceneVerse-val（零样本跨域泛化）。
  - **零样本/长尾场景**：ScanNet200、Matterport3D（不参与训练）、SceneVerse-val。
  - **下游微调任务**：低层感知（语义分割、实例分割：ScanNet、ScanNet200）、高层推理（视觉定位：ScanRefer、Nr3D、Sr3D、Multi3dRefer；问答：SQA3D；密集描述：Scan2Cap）。
- **Benchmark与对比方法**：
  - 开放词汇语义分割：对比OpenScene (2D/3D)、RegionPLC、PLA、OV3D、MaskCLIP、CLIP2、Uni3D等。
  - 无监督预训练对比：对比SC、PC、CSC、MSC、GroupContrast。
  - 高层推理对比：对比3DJCG、M3DRef-CLIP、3D-VisTA、Vote2CapDetr、PQ3D等。
- **评价指标**：f-mIoU（前景mIoU）、f-mAcc（前景准确率）、Acc@0.5、Acc、CIDEr@0.5、AccQA等。

## 4. 资源与算力
- **文中未明确说明**：论文未提及具体使用的GPU型号、数量、训练时长等算力信息。仅在实现细节中说明采用SPUNet作为3D编码器，CLIP作为文本编码器，训练时仅更新3D编码器和适配器。因此无法给出具体算力消耗。

## 5. 实验数量与充分性
- **实验组数**：大量实验包括：
  - 开放词汇语义分割（ScanNet、Matterport3D、SceneVerse-val、ScanNet200）共4组。
  - 零样本/长尾实验2组。
  - 下游微调：语义/实例分割（2组）、数据效率实验（文中提及，但未列出详细表格）、高层次推理（6组任务）。
  - 消融实验：模型设计（3组）、文本描述类型（3组）、训练数据集组合（8组）。
  - 定性结果展示。
- **充分性与公平性**：实验设计全面，覆盖了从感知到推理的多种任务，对比了当前最先进的方法，并给出了多个消融分析。但数据效率实验未在正文中给出具体表格（仅提到“数据效率实验见补充材料”），削弱了完整性。整体实验是充分且客观的。

## 6. 主要结论与发现
- MPEC在开放词汇3D语义分割上达到当前最优（ScanNet: f-mIoU 66.0%，f-mAcc 81.3%），比第二名OV3D分别高出3.0%和6.5%。
- 零样本跨域泛化能力强：在SceneVerse-val上f-mIoU 45.0%（领先OpenScene 3.7%），在Matterport3D上以零样本达到47.7% f-mIoU（接近领域内方法）。
- 在长尾场景（ScanNet200）上显著领先（f-mIoU 10.8%，f-mAcc 27.4%）。
- 作为3D骨干网络微调后，在感知任务上取得新SOTA（ScanNet200语义分割31.8% mIoU，实例分割31.6% mAP@0.5）；在高层推理任务上带来一致提升（如ScanRefer Acc@0.5从51.2%提升至51.8%）。
- 消融实验表明：跨视图点-实体对比训练至关重要；结合物体描述和空间参照两种文本类型效果最好；扩大训练数据规模（加入更多真实场景）可带来明显性能提升。

## 7. 优点
- **方法创新**：首次在开放词汇3D理解中引入实体级别的跨视图对比学习，有效融合3D几何信息与语言对齐，弥补了纯2D蒸馏方法的不足。
- **实验全面**：在7个任务、8个数据集上进行了评估，覆盖零样本、长尾、低层感知、高层推理等多种场景，验证了泛化能力。
- **实用性强**：可直接作为3D骨干用于多种下游任务，提升效果显著；在数据稀缺场景下（1%训练数据）提升巨大（mIoU从30.7%提升至40.8%）。
- **消融详尽**：从模型设计、文本类型、数据域三个维度进行了系统分析，为后续研究提供了重要参考。

## 8. 不足与局限
- **算力资源未报告**：缺少训练耗时、GPU型号等细节，难以评估方法的实际计算成本。
- **数据效率实验不完整**：文中提及数据效率实验但未在正文展示具体表格，读者需查阅补充材料才能获得完整结果。
- **依赖预训练模型**：实体掩码依赖现成3D实例分割模型，文本描述依赖2D基础模型和大语言模型，存在级联误差风险。
- **文本编码器局限**：固定CLIP文本编码器在处理长且详细的描述时表现不佳（如复杂空间参照），导致错误案例（图3底部），未来需探索更强文本编码器或端到端训练。
- **场景域偏移**：在Matterport3D上零样本性能虽好但仍低于领域内方法，表明对未见过的场景泛化仍有提升空间。
- **评估偏差**：开放词汇语义分割仅使用ScanNet单一数据集作为主基准，尽管有跨域验证，但缺乏更多样化的室内/室外场景评估。

（完）
