---
title: Towards 3D Object-Centric Feature Learning for Semantic Scene Completion
title_zh: 面向语义场景补全的三维目标中心特征学习
authors: "Weihua Wang, Yubo Cui, Xiangru Lin, Zhiheng Li, Zheng Fang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37981/41943"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 从2D实例掩码到3D目标中心特征用于语义场景补全
tldr: 语义场景补全通常忽视物体级细节，导致复杂环境下语义和几何模糊。本文提出Ocean框架，首先用MobileSAM从图像提取实例掩码，然后引入3D语义分组注意力模块，将物体级特征融入补全过程。实验在自动驾驶场景中提升了语义占用预测的准确性，尤其在遮挡和重叠区域表现优异。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语义场景补全方法缺乏物体级细节，在复杂环境中产生歧义。
method: 利用MobileSAM提取2D实例掩码，通过3D语义分组注意力聚合物体特征。
result: 在语义场景补全基准上，物体级精度显著提高。
conclusion: 目标中心特征学习有效增强了场景补全的精细度。
---

## Abstract
Vision-based 3D Semantic Scene Completion (SSC) has received growing attention due to its potential in autonomous driving. While most existing approaches follow an ego-centric paradigm by aggregating and diffusing features over the entire scene, they often overlook fine-grained object-level details, leading to semantic and geometric ambiguities, especially in complex environments. To address this limitation, we propose Ocean, an object-centric prediction framework that decomposes the scene into individual object instances to enable more accurate semantic occupancy prediction. Specifically, we first employ a lightweight segmentation model, MobileSAM, to extract instance masks from the input image. Then, we introduce a 3D Semantic Group Attention module that leverages linear attention to aggregate object-centric features in 3D space. To handle segmentation errors and missing instances, we further design a Global Similarity-Guided Attention module that leverages segmentation features for global interaction. Finally, we propose an Instance-aware Local Diffusion module that improves instance features through a generative process and subsequently refines the scene representation in the BEV space. Extensive experiments on the SemanticKITTI and SSCBench-KITTI360 benchmarks demonstrate that Ocean achieves state-of-the-art performance, with mIoU scores of 17.40 and 20.28, respectively.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有基于视觉的3D语义场景补全（SSC）方法大多采用以自我为中心（ego-centric）的范式，对整个场景进行特征聚合和扩散，忽略了物体级别的细粒度细节，导致在复杂环境中（如多个邻近车辆、遮挡区域）产生语义和几何模糊性（例如将车辆间的空隙错误预测为车辆）。
- **研究动机**：为了解决上述歧义，引入物体中心（object-centric）的学习范式，利用显式的物体级先验信息（如实例掩码）来指导特征交互，提升预测的准确性和精细度。
- **整体含义**：提出Ocean框架，通过将场景分解为单个物体实例，实现更精确的语义占用预测，在自动驾驶场景中具有重要应用价值。

### 2. 论文提出的方法论
- **核心思想**：利用轻量级分割模型MobileSAM从输入图像中提取2D实例掩码，然后将这些掩码先验提升到3D空间，以实现物体级别的特征聚合和扩散，最终生成精细的3D语义占用。
- **关键技术细节**：
  - **整体架构**：输入单目图像 → 图像编码器提取多尺度特征 → 通过深度估计和上下文特征外积提升到3D体素表示 → 基于深度预测筛选查询提案（query proposals）。
  - **SemGroup Dual Attention (SGDA) 块**：
    - **3D Semantic Group Attention (SGA3D)**：将3D查询提案投影到图像平面，根据MobileSAM掩码进行实例分组，利用线性注意力机制在组内聚合多尺度图像特征。进一步引入深度相似性矩阵（通过深度箱桥接）扩展为3D聚合，提升几何感知。
    - **Global Similarity-Guided Attention (GSGA)**：使用可变形注意力，利用MobileSAM特征计算查询与图像特征之间的相似度，动态过滤并增强属于同一实例的特征，以弥补掩码错误和遗漏。
  - **Instance-aware Local Diffusion (ILD) 模块**：
    - **Dynamic Instance Decoder (DID)**：将分组后的实例特征通过转置卷积生成BEV表示，使用Gumbel Softmax动态选择实例，并通过重建损失约束。
    - **Local Attention Refinement**：在BEV空间使用窗口注意力，以聚合特征为查询、实例感知特征为键值进行局部精炼。
- **公式与算法流程说明**：
  - 投影公式：\( d [u, v, 1]^T = K [R|t] [x, y, z, 1]^T \)
  - SGA3D聚合公式：\(\tilde{Q} = \text{Concat}\left[ \frac{\phi(Q_j) A_j \sum_{i=1}^{m_j} \phi(K_{ji})^T V_{ji}}{\phi(Q_j) \sum_{i=1}^{m_j} \phi(K_{ji})^T} \right]_{j=1}^M\)
  - GSGA聚合公式：\(\hat{Q} = \sum_k (G_k W F(p_q + \Delta p_k)) \odot A_k\)
  - DID融合公式：\( \hat{P} = \sum_{l=1}^L P_l \odot \hat{w}_l \)，其中\(\hat{w}_l\)由Gumbel Softmax决策生成。

### 3. 实验设计
- **数据集与基准**：
  - **SemanticKITTI**：常用于语义场景补全的自动驾驶数据集。
  - **SSCBench-KITTI360**：另一个街景语义场景补全基准。
- **评价指标**：IoU（几何占用率）和 mIoU（语义平均交并比）。
- **对比方法**：与 MonoScene、TPVFormer、VoxFormer、OccFormer、MonoOcc、Symphonies、LOMA、CGFormer、HTCL、SGFormer 等方法进行了比较。这些方法代表了近期SSC领域的主流和先进水平。

### 4. 资源与算力
- 论文明确说明：使用 **4张 GeForce 3090 GPU** 进行训练。
- 优化器：AdamW，初始学习率 3e-4，权重衰减 0.01。
- 未明确说明训练总时长。

### 5. 实验数量与充分性
- **实验数量**：
  - 在主数据集（SemanticKITTI test 和 SSCBench-KITTI360 test）上进行了与多种方法的全面对比。
  - 在 SemanticKITTI validation 上进行了多组消融实验：
    - 主模块消融（SGA3D、GSGA、LA、DID）。
    - SGA3D组件消融（多尺度特征、3D扩展、替换为DFA2D/DFA3D）。
    - GSGA消融（去除、去除相似度引导）。
    - 实例解码器融合策略消融（直接求和、softmax加权、sigmoid加权、动态选择）。
  - 提供了定性可视化结果（图6）。
- **充分性与客观性**：实验设计较为全面，覆盖了核心模块、组件替换、融合策略等关键因素，对比方法具有一定代表性。但未在更多数据集（如 nuScenes）上验证，也未进行跨模态或跨任务泛化实验。总体公平，消融实验控制变量合理。

### 6. 论文的主要结论与发现
- Ocean 在 SemanticKITTI 上达到 mIoU 17.40，在 SSCBench-KITTI360 上达到 mIoU 20.28，均取得当前最优（SOTA），验证了物体中心特征学习的有效性。
- 显式利用MobileSAM实例掩码进行3D物体级特征聚合，能够有效缓解传统全局特征融合导致的语义和几何歧义。
- SGA3D（结合深度相似性）和GSGA（结合相似度引导的全局交互）共同作用，既利用了物体先验又弥补了掩码缺陷。
- 动态实例解码器（DID）和局部注意力精炼进一步提升了场景表示的语义一致性。

### 7. 优点
- **方法创新**：将2D视觉基础模型（MobileSAM）显式引入3D语义场景补全，实现了从“场景级”到“物体级”的范式转变。
- **模块设计合理**：SGA3D在3D空间进行物体级局部聚合，GSGA处理错误/遗漏，ILD模块实现实例感知扩散，模块间互补性强。
- **性能提升显著**：在两大主流基准上超越先前SOTA（如CGFormer、HTCL），尤其在细粒度物体（如自行车、杆子）上表现突出。
- **计算效率**：采用线性注意力、可变形注意力等轻量机制，参数量和FLOPs较低（SGA3D仅0.280M参数，1.333G FLOPs）。

### 8. 不足与局限
- **实验覆盖有限**：仅测试了SemanticKITTI和SSCBench-KITTI360两个数据集，未在nuScenes、Waymo等更大规模或不同场景数据集上验证泛化能力。
- **依赖2D先验质量**：MobileSAM的掩码质量直接影响性能，虽然GSGA部分弥补，但在极端遮挡、小物体或模糊场景中仍可能存在性能下降风险。
- **未讨论失败案例**：定性可视化仅展示成功改进，未分析预测明显错误的案例（如复杂交叉口、动态物体）。
- **应用限制**：方法基于单目图像，对深度估计精度敏感；假设场景主要由静态或慢速物体组成，对高速运动物体的处理未明确评估。
- **生成模块的稳定性**：Dynamic Instance Decoder中使用Gumbel Softmax和重建损失，训练稳定性或收敛行为未详细分析。

（完）
