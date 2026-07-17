---
title: "PanoGS: Gaussian-based Panoptic Segmentation for 3D Open Vocabulary Scene Understanding"
title_zh: PanoGS：基于高斯的全景分割用于3D开放词汇场景理解
authors: "Zhai, Hongjia, Li, Hai, Li, Zhenzhe, Pan, Xiaokun, He, Yijia, Zhang, Guofeng"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhai_PanoGS_Gaussian-based_Panoptic_Segmentation_for_3D_Open_Vocabulary_Scene_Understanding_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 使用3DGS的开放词汇全景场景理解
tldr: 本文针对现有3D开放词汇方法无法区分实例级信息的问题，提出PanoGS方法。采用金字塔三平面建模连续特征空间，通过3D特征解码器回归多视图融合的2D特征云，并利用语言引导的图割进行实例分割。实验表明该方法在大规模室内场景中实现了高效精确的开放词汇全景分割。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1782, \"height\": 515, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1798, \"height\": 603, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 489, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 375, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1817, \"height\": 953, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 869, \"height\": 410, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 838, \"height\": 314, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 872, \"height\": 255, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 902, \"height\": 468, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 867, \"height\": 354, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 812, \"height\": 225, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhai-panogs-gaussian-based-panoptic-segmentation-for-3d-open-vocabulary-scene-understanding-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 823, \"height\": 302, \"label\": \"Table\"}]"
motivation: 现有3D开放词汇方法无法区分实例级信息，通常只预测场景特征与文本查询的热图。
method: 采用金字塔三平面建模连续参数特征空间，用3D特征解码器回归多视图融合2D特征云，并结合语言引导图割进行全景分割。
result: 在多个室内数据集上实现了优于现有方法的开放词汇全景分割性能。
conclusion: PanoGS有效融合了多视图特征和语言信息，实现了高效可扩展的3D开放词汇全景理解。
---

## Abstract
Recently, 3D Gaussian Splatting (3DGS) has shown encouraging performance for open vocabulary scene understanding tasks. However, previous methods can not distinguish 3D instance-level information, which usually predicts a heatmap between the scene feature and text query. In this paper, we propose PanoGS, a novel and efficient 3D panoptic open vocabulary scene understanding approach. Technical-wise, to learn accurate 3D language features that can scale to large indoor scenarios, we adopt the pyramid tri-planes to model the latent continuous parametric feature space and use a 3D feature decoder to regress the multi-view fused 2D feature cloud. Besides, we propose language-guided graph cuts that synergistically leverage reconstructed geometry and learned language cues to group 3D Gaussian primitives into a set of super-primitives. To obtain 3D consistent instance, we perform graph clustering based segmentation with SAM-guided edge affinity computation between different super-primitives. Extensive experiments on widely used datasets show better or more competitive performance on 3D panoptic open vocabulary scene understanding.

---

## 论文详细总结（自动生成）

# PanoGS：基于高斯的全景分割用于3D开放词汇场景理解

## 1. 核心问题与整体含义（研究动机和背景）

现有基于3D高斯溅射（3DGS）的开放词汇场景理解方法存在两个关键局限：
- **不准确的3D语言特征学习**：为每个高斯基元附着的离散特征破坏了语言特征的平滑性，且alpha混合导致的2D-3D特征空间域差距会损害区分能力。
- **无法识别3D实例级信息**：之前方法通常只预测场景特征与文本查询之间的热图，导致实例分割结果不一致，无法区分同一语义下的不同对象。

为此，本文提出**PanoGS**，首个将3DGS用于全景开放词汇场景理解的方法，旨在同时实现准确的3D语义特征和一致的3D实例分割。

## 2. 方法论

### 核心思想
- **连续参数化语言特征场**：使用**金字塔三平面**建模隐式连续参数特征空间，避免离散表示带来的噪声和存储问题。
- **3D空间蒸馏**：通过多视图融合的2D特征云和置信度，直接对3D语言特征进行优化，而非在2D渲染空间蒸馏，从而避免alpha混合造成的域差距。
- **图聚类分割**：将3D实例分割转化为图聚类问题，通过**语言引导的图割**将高斯基元分组为超基元，再基于SAM掩码的多视图一致性计算边亲和度，渐进式聚类得到全局一致的实例。

### 关键技术细节
1. **场景表示**：每个3D高斯基元附加一个低维潜码g，通过3D语言特征解码器`D_lang`将潜码映射为高维语言特征`f_3D`。
2. **金字塔三平面**：给定3D位置μ，通过三线性插值在不同分辨率的三个正交平面（XY、YZ、XZ）上查询潜码，并求和得到g(μ)。公式：
   ```
   g(μ) = Σ_i {T(μ, X^i_xy), T(μ, X^i_yz), T(μ, X^i_xz)}
   ```
3. **多视图特征融合**：将每个高斯基元投影到多个视图，对2D语言特征（由LSeg提取）进行加权平均得到融合特征`f_2D`，同时根据观测次数和特征方差计算置信度`γ_2D`。
4. **损失函数**：
   ```
   L_feat = Σ_i γ_2D_i · |1 - cos(D_lang(g_i), f_2D_i)|
   ```
   其中cos为余弦相似度，置信度用于降低不可靠特征的权重。
5. **图顶点构建**：通过语言引导的图割，同时考虑几何法线相似度和语义特征相似度，将基元合并为超基元。合并条件：
   ```
   Δ = 1(n_i·n_j > λ_n) · 1(f_3D_i·f_3D_j > λ_f)
   ```
6. **边亲和度计算**：将超基元投影到各视图的SAM掩码上，获得掩码标签分布，使用Jensen-Shannon散度度量分布差异，并聚合多视图亲和度。
7. **渐进式图聚类**：从局部到全局，逐步降低亲和度阈值（从0.9到0.6，共4次迭代），合并超基元。

## 3. 实验设计

- **数据集**：Replica（8个场景：room0-2, office0-4）和 ScanNetV2（10个序列），后者含有挑战性室内场景。
- **评估指标**：
  - 语义：点云mIoU、mAcc
  - 全景：3D全景重建质量PRQ（分为thing级PRQ(T)和stuff级PRQ(S)）
- **对比方法**：
  - 3DGS方法：LangSplat（作者重新实现，记为*）、OpenGaussian（记为†，不进行高斯滤波）
  - 点云方法：OpenScene（两种变体：Dis.和Ens.）
  - 对全景分割，基线使用全监督3D实例分割方法SoftGroup（在ScanNetV2上训练）生成实例提案。

## 4. 资源与算力

文中**未明确说明**具体的GPU型号、数量或训练时长。仅提及实现细节使用LSeg作为2D特征提取器、OpenCLIP ViT-B/16作为文本编码器、ViT-H SAM生成2D掩码。因此无法量化算力消耗。

## 5. 实验数量与充分性

- **主要实验**：在ScanNetV2和Replica两个数据集上进行了语义分割和全景分割评测。
- **消融实验**：针对3D语言特征学习模块（Tab.3）和图聚类分割模块（Tab.4）分别进行消融，验证了3D解码器、金字塔三平面、语言引导图割、JSD多视图亲和度、预测投票等组件的作用。
- **定性结果**：展示了语义分割对比（Fig.4）、全景分割对比（Fig.5）、开放词汇查询可视化（Fig.6）以及图割质量对比（Fig.8）。
- **充分性判断**：实验覆盖了主流量化指标和定性可视化，消融设计系统合理，对比基线全面（包括最新的3DGS方法和点云方法）。但未报告跨数据集泛化、不同文本编码器或不同2D特征提取器的影响，也未提供运行效率对比（如推理时间）。整体充分但可进一步扩展。

## 6. 主要结论与发现

- PanoGS在语义分割（mIoU、mAcc）上显著优于LangSplat和OpenGaussian，并超过OpenScene，尤其在长尾类别上效果更好。
- 在全景分割方面，PanoGS在Replica上获得最高PRQ(T)和PRQ(S)；在ScanNetV2上，当使用SoftGroup提供的实例掩码后，PanoGS+Sup.mask也达到最优，说明其语义特征质量高。
- 消融实验证明：金字塔三平面和3D蒸馏有效缩小特征学习差距（蒸馏相似度从0.9提升至接近1）；置信度蒸馏可减少不可靠区域影响；语言引导图割避免将不同语义对象（如门和墙）错误合并；多视图JSD亲和度显著提升全景分割质量。

## 7. 优点

1. **首次实现3DGS的全景开放词汇理解**：克服了现有方法只能输出热图、无法区分实例的缺陷。
2. **连续特征空间**：金字塔三平面替代离散特征，节省内存、提升平滑性，且通过3D解码器避免alpha混合造成的域差距。
3. **有效融合多模态线索**：语言引导图割同时利用几何和语义信息，SAM引导的图聚类利用2D分割先验，实现一致的3D实例。
4. **实验全面**：在多个数据集上对比多种基线，消融实验验证了各组件贡献。

## 8. 不足与局限

1. **计算资源未报告**：缺乏GPU型号、训练时间和内存消耗的量化，难以评估实际可复制性和成本。
2. **依赖预训练模型**：2D特征提取（LSeg）、文本编码（OpenCLIP）和SAM掩码均使用外部模型，其质量直接影响最终性能；未测试不同选择的影响。
3. **场景规模受限**：方法在室内场景（Replica、ScanNetV2）上评估，未在室外或更大规模场景（如Matterport3D）上验证泛化性。
4. **全景指标依赖实例评判**：ScanNetV2上PRQ(T)不如OpenScene+SoftGroup，说明实例聚类在某些场景下仍有提升空间；且受到SoftGroup监督信息的影响，公平性需斟酌。
5. **未提供推理速度**：图聚类过程涉及多视图投影和迭代聚类，实际部署效率未知。
6. **未讨论失败案例**：文中未展示明显错误分割或遮挡严重场景下的表现，局限性分析不足。

（完）
