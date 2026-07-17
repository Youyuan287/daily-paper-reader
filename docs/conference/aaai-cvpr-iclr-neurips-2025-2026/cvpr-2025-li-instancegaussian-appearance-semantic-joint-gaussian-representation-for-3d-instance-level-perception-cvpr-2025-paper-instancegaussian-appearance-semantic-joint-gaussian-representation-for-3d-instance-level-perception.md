---
title: "InstanceGaussian: Appearance-Semantic Joint Gaussian Representation for 3D Instance-Level Perception"
title_zh: InstanceGaussian：面向三维实例级感知的外观-语义联合高斯表示
authors: "Li, Haijie, Wu, Yanmin, Meng, Jiarui, Gao, Qiankun, Zhang, Zhiyao, Wang, Ronggang, Zhang, Jian"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Li_InstanceGaussian_Appearance-Semantic_Joint_Gaussian_Representation_for_3D_Instance-Level_Perception_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 8.0
evidence: 外观语义联合高斯表示用于三维实例感知
tldr: 针对3D高斯泼溅在场景理解中外观与语义不平衡、边界不一致和实例分割困难的问题，本文提出InstanceGaussian，通过语义支架高斯表示联合学习外观与语义特征，并采用渐进训练策略增强稳定性，在多个3D实例感知任务上取得显著改进。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 3D高斯泼溅在实例级感知中存在外观与语义不平衡、边界不一致和分割困难。
method: 提出Semantic-Scaffold-GS表示，结合渐进训练策略联合优化外观与语义特征。
result: 在多个3D实例分割和感知基准上达到SOTA，验证了方法的有效性。
conclusion: 联合学习外观和语义的高斯表示能有效提升3D实例级感知性能。
---

## Abstract
3D scene understanding is vital for applications in autonomous driving, robotics, and augmented reality. However, scene understanding based on 3D Gaussian Splatting faces three key challenges: (i) an imbalance between appearance and semantics, (ii) inconsistencies in object boundaries, and (iii) difficulties with top-down instance segmentation. To address these challenges, we propose InstanceGaussian, a method that jointly learns appearance and semantic features while adaptively aggregating instances. Our contributions are as follows: (i) a new Semantic-Scaffold-GS representation to improve feature representation and boundary delineation, (ii) a progressive training strategy for enhanced stability and segmentation, and (iii) a category-agnostic, bottom-up instance aggregation approach for better segmentation. Experimental results demonstrate that our approach achieves state-of-the-art performance in category-agnostic, open-vocabulary 3D point-level segmentation, validating the effectiveness of our proposed method.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
3D场景理解是自动驾驶、机器人、增强现实等应用的关键。基于3D高斯泼溅（3D Gaussian Splatting, 3DGS）的方法在实例级感知中面临三大挑战：
- **外观与语义不平衡**：一个物体/区域需要多个外观各异的高斯，但语义只需一个共享属性，导致高斯数量对语义表达冗余。
- **边界不一致**：在纯外观重建中，单个高斯可能同时表示前景与背景，导致外观与语义边界错位。
- **自上而下分割困难**：现有方法（如GaussianGrouping、OpenGaussian）依赖预设类别数或固定代码本，在复杂场景中易出现过分割或欠分割。

为此，论文提出**InstanceGaussian**，通过联合学习外观与语义特征，并自下而上自适应聚合实例，实现类别无关、开放词汇的3D点级分割。

## 2. 方法论：核心思想与关键技术细节
InstanceGaussian的核心由三部分组成：

### 2.1 Semantic-Scaffold-GS表示
以Scaffold-GS为基础，将每个锚点（anchor）关联一个**外观嵌入**（appearance embedding）和一个**实例特征**（instance feature）。每个锚点解码出5个子高斯，这些子高斯共享父锚点的实例特征，但外观属性（颜色、位置等）通过MLP从外观嵌入独立生成。这种设计平衡了外观丰富度与语义效率，增强了稀疏场景的外观学习，同时促进了相邻点的语义一致。

### 2.2 渐进式外观-语义联合训练策略
训练分三个阶段（总步数30k）：
- **0-10k步**：仅训练外观属性（颜色等）。
- **10-20k步**：独立训练外观和语义（实例特征）。
- **20-30k步**：联合优化外观与语义。

为避免对比损失过大导致训练不稳定，对OpenGaussian的跨掩码对比损失进行**截断**：仅当两个掩码的平均特征距离小于阈值τ（设为0.4）时才施加损失，即：
\[
L_c = \frac{1}{m(m-1)} \sum_{i=1}^m \sum_{j\neq i} \mathbf{1}_{\|\bar{M}_i-\bar{M}_j\|_2<\tau} \|\bar{M}_i-\bar{M}_j\|_2^2
\]
同时保留平滑损失 \(L_s\) 使掩码内特征一致。

### 2.3 自下而上、类别无关的实例聚合
- **过分割**：对所有高斯点使用最远点采样（FPS）选取s个种子（默认s=1000），结合位置编码与实例特征进行k-means聚类，得到s个子对象。
- **图连通性聚合**：将子对象视为节点，构建邻接矩阵，综合考虑**特征L2距离**和**空间邻接性**（通过体素化判断是否相邻）。对连通值在(0, γ]内的节点使用图连通算法（如并查集）合并，自适应得到最终实例。

算法流程见原文Algorithm 1。

## 3. 实验设计
### 3.1 数据集与基准
- **ScanNet**：用于类别无关3D实例分割和开放词汇语义分割。取OpenGaussian选定的10个场景，计算mIoU和mAcc（@0.25）。
- **LeRF数据集**：用于开放词汇3D对象选择与渲染，渲染后2D图像计算mIoU和mAcc。

### 3.2 对比方法
- **类别无关实例分割**：对比OpenGaussian、GaussianGrouping，以及引入深度的MaskClustering。
- **开放词汇语义分割**：对比LangSplat、LEGaussians、OpenGaussian、MaskClustering。
- **开放词汇3D对象选择与渲染**：对比LangSplat、LEGaussians、OpenGaussian。

所有方法均使用RGB图像和SAM掩码作为输入。

### 3.3 实验结果概要
- **类别无关实例分割**（表1）：mIoU 50.27，mAcc 80.22，显著优于GaussianGrouping（22.55/30.54）和OpenGaussian（27.32/52.44），接近使用深度的MaskClustering（54.17/80.95）。
- **开放词汇语义分割**（表2）：在19/15/10类上mIoU 40.66/42.51/47.94，mAcc 54.01/59.15/64.01，全面超越所有对比方法，甚至超过依赖深度的MaskClustering。
- **开放词汇3D对象选择与渲染**（表3）：mIoU 45.30，mAcc 58.44，优于OpenGaussian（38.36/51.34）等。

## 4. 资源与算力
论文中**未明确说明**使用的GPU型号、数量和具体训练时长。仅在局限性中提及“训练时间较长，渲染慢”，因为外观属性需通过MLP计算。训练步数为30k（渐进三阶段），但未提供单场景训练时间或硬件配置。

## 5. 实验数量与充分性
论文共设计了**三个主要任务**（各一个表格），两个消融实验（表4、表5），以及丰富的可视化结果（图3-6）。消融实验覆盖：
- 表示和训练策略（联合表示 vs 独立训练，联合训练 vs 不联合）。
- 聚合条件（仅颜色+位置、仅特征、特征+体素空间）。

实验设计较为充分：对比了多种主流高斯方法，场景来自标准数据集（ScanNet、LeRF），评估指标常用。但注意：
- 类别无关实例分割仅用了10个场景，规模较小；开放词汇任务使用了全部场景但未说明场景数。
- 所有比较均在同一输入（RGB+SAM掩码）下进行，公平性较好。但MaskClustering额外使用了深度，对比时已注明“depth-aided”，体现了公平性。
- 消融实验仅在一个任务（ScanNet语义+实例分割）上开展，未在其他数据集验证，覆盖稍显不足。

总体实验客观、有效，但可进一步在更多场景或数据集上验证泛化性。

## 6. 主要结论与发现
- InstanceGaussian通过**外观-语义联合表示**和**渐进训练**有效解决了外观语义不平衡与边界不一致问题，显著提升了3D实例分割精度。
- **自下而上自适应聚合**避免了预设类别数带来的过分割/欠分割，使方法更具通用性。
- 在**类别无关实例分割**和**开放词汇点级分割**上均达到SOTA，甚至在开放词汇语义分割上超越了依赖深度的MaskClustering。
- 联合表示还能提升对象渲染的质量（边界更清晰）。

## 7. 优点
- **创新性**：首次提出Semantic-Scaffold-GS表示，实现外观与语义的协同优化。
- **实用性**：自下而上聚合完全无需预设实例数，适合开放世界场景。
- **稳定性**：截断对比损失有效避免了训练动荡，联合训练策略简单有效。
- **全面性**：在多个3D感知任务（实例分割、语义分割、对象渲染）上均验证了有效性。
- **可解释性**：通过3D查看器可视化高斯椭球，直观展示了外观-语义一致性（图6）。

## 8. 不足与局限
- **训练效率**：外观属性需通过MLP解码，导致训练时间较长、渲染速度慢，不适合实时应用。
- **依赖SAM质量**：当场景中多数图像被SAM错误分割时，分割精度会下降（文中提及）。
- **实验规模**：类别无关实例分割仅使用10个ScanNet场景，泛化性需在更多场景（如Replica、KITTI）上验证。
- **缺少算力报告**：未说明硬件环境和训练耗时，不利于复现和比较。
- **消融不足**：未消融s（子对象数量）和阈值τ、γ的影响，这些超参数对聚合结果可能有较大影响。

（完）
