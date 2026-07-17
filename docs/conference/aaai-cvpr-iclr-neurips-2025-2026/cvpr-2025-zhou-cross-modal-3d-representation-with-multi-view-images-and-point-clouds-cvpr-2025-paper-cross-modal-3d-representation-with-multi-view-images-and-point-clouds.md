---
title: Cross-Modal 3D Representation with Multi-View Images and Point Clouds
title_zh: 基于多视图图像与点云的跨模态三维表示
authors: "Zhou, Ziyang, Wang, Pinghui, Liang, Zi, Bai, Haitao, Zhang, Ruofei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhou_Cross-Modal_3D_Representation_with_Multi-View_Images_and_Point_Clouds_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 7.0
evidence: 多视图与点云融合的三维表示
tldr: 针对现有3D语义表示仅依赖点云、忽视多视图图像丰富细节的问题，本文提出OpenView方法，通过跨模态融合编码器将点云与多视图图像统一为三维表示，采用序列无关建模与渐进式困难学习策略，实验证明该方法在多个3D理解任务上取得更优性能，为融合多模态的三维表示提供了新范式。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1798, \"height\": 348, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1782, \"height\": 609, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1720, \"height\": 1173, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1442, \"height\": 433, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1453, \"height\": 478, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhou-cross-modal-3d-representation-with-multi-view-images-and-point-clouds-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1646, \"height\": 542, \"label\": \"Table\"}]"
motivation: 现有3D语义表示仅关注点云，忽略了多视图图像的丰富视觉细节，限制了3D表示能力。
method: 提出OpenView，包含跨模态融合编码器、序列无关建模和渐进式困难学习策略，融合点云与多视角图像。
result: 在多个3D理解基准上，OpenView显著优于仅依赖点云的方法，证明多视图信息的有效性。
conclusion: 融合多视图图像与点云能够提升三维语义表示的丰富性和准确性。
---

## Abstract
The advancement of 3D understanding and representation is a crucial step for the next phase of autonomous driving, robotics, augmented and virtual reality, 3D gaming and 3D e-commerce products. However, existing 3D semantic representation research has primarily focused on point clouds to perceive 3D objects and scenes, overlooking the rich visual details offered by multi-view images, thereby limiting the potential of 3D semantic representation. This paper introduces OpenView, a novel representation method that integrates both point clouds and multi-view images to form a unified 3D representation. OpenView comprises a unique fusion framework, sequence-independent modeling, a cross-modal fusion encoder, and a progressive hard learning strategy. Our experiments demonstrate that OpenView outperforms the state-of-the-art by 11.5% and 5.5% on the R@1 metric for cross-modal retrieval and the Top-1 metric for zero-shot classification tasks, respectively. Furthermore, we showcase some applications of OpenView: 3D retrieval, 3D captioning and hierarchical data clustering, highlighting its generality in the field of 3D representation learning.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有3D语义表示研究主要依赖点云作为唯一数据模态，忽略了多视图图像中丰富的纹理、颜色等视觉细节，限制了3D理解的深度和跨模态应用能力。
- **动机**：多视图图像在3D生成、重建中已显示出潜力，但语义表示领域未充分利用。结合点云的精确几何与多视图图像的丰富视觉信息，有望实现更全面的3D表示。
- **背景**：以往方法要么仅关注点云（如Point-BERT、OpenShape），要么仅关注图像（如LRM），缺乏有效的跨模态融合。此外，多视图图像的无序性和易混淆视角也带来挑战。
- **整体含义**：提出OpenView框架，通过融合点云与多视图图像，统一3D语义表示，在分类、检索、描述等任务上显著超越单一模态方法，为多模态3D理解树立新范式。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
将3D对象表示为点云与多视图图像的联合输入，通过跨模态融合编码器生成对齐到CLIP文本-图像空间的3D语义嵌入。

### 关键技术细节

- **框架结构**（参照图2）：
  - **图像编码器**：使用预训练ViT（OpenCLIP的ViT-bigG-14），每张视图提取一个[CLS]嵌入（仅输出一个语义向量，节省显存）。
  - **点云编码器**：基于Point-BERT结构，将点云划分为k个子点云，经mini-PointNet生成patch嵌入，加[CLS]嵌入通过transformer。
  - **跨模态融合编码器**：设计为transformer变体，包含**模态内自注意力**（分别处理图像和点云特征）和**模态间交叉注意力**（并行使用cross-attention和self-attention进行信息交互）。输出为两个特殊token（图像平均特征和点云[CLS]）的归一化加和。

- **序列无关建模**：由于多视图图像无序，融合编码器中对图像特征不添加位置编码，仅添加可学习的类型嵌入（t_I, t_P），使输出不受输入顺序影响。

- **参数初始化**：
  - 图像编码器：OpenCLIP预训练（冻结）。
  - 点云编码器：OpenShape预训练（微调）。
  - 融合编码器：均匀分布随机初始化。

- **训练目标**（对比学习）：
  - 不使用图像作为对齐目标（避免单视角误导），仅对齐3D表示与文本表示。
  - 损失函数为对称的InfoNCE（式8），温度τ可学习。
  - 文本编码器固定为OpenCLIP，冻结。

- **渐进式困难学习**：
  - 按每张视图图像与文本描述的余弦相似度排序，逐步移除相似度高的“容易”图像。每epoch减少10%的图像，直至保留50%。
  - 强制模型关注全局与困难视角，提升鲁棒性。

## 3. 实验设计

### 数据集
- **训练集（4个）**：
  - Objaverse（798.8k模型，去除LVIS子集）
  - ShapeNetCore（52.5k）
  - ABO（8.0k）
  - 3D-Future（16.6k）
- **测试基准（3个）**：
  - Objaverse-LVIS（46.8k模型，1156类别）
  - ModelNet40（2.4k模型，40类别，合成CAD无纹理）
  - Omniobject3D（6.0k模型，190类别，真实扫描，无RGB点云）

### 任务与指标
- **零样本分类**：Top-1/3/5准确率（基于模板提示）。
- **跨模态检索**：Text→3D和3D→Text的Recall@1/5/10。

### 对比方法
- 点云基线：PointCLIP v2、ReCon、CLIP2Point、ULIP、OpenShape、Uni3D。
- 多视图基线：PointCLIP v2（用多视图替换点云投影）。
- 融合基线：用基础transformer替换跨模态融合编码器。

### 消融实验
- 输入模态：去除多视图/去除点云。
- 组件：无融合编码器、无序列无关建模、无模态间/模态内注意力。
- 图像数量：3、6、10。
- 训练策略：是否使用渐进式困难学习。

### 应用展示
- 图像/文本查询的3D检索、3D描述（结合ClipCap）、层次聚类（t-SNE）。

## 4. 资源与算力

- **GPU**：8块Tesla V100（32GB显存）。
- **训练时长**：10个epoch，24小时。
- **批大小**：64。
- **优化器**：AdamW，学习率1e-7，余弦调度。
- **注**：未提及推理资源需求，但推断单卡V100可处理。

## 5. 实验数量与充分性评估

- **实验数量**：主实验2类（分类3个数据集×3指标 + 检索3个数据集×6指标）共18个表项；消融实验9个变体；额外应用示例。整体实验组数约10+。
- **充分性**：
  - **正面**：对比了7种以上SOTA，覆盖不同输入类型；消融全面，验证每个组件贡献；在3个不同特性基准上评估（大词表、合成、真实扫描），结果一致。
  - **不足**：未在自动驾驶数据集（如KITTI、Waymo）或更多真实场景测试；只用了10张视图，未探索更多视图的效果；未与最新的多模态大模型（如LLM-based方法）对比；未报告方法在纯净点云（无颜色）上的性能退化细节。
- **公平性**：沿用OpenShape/ULIP相同的模板提示和评测协议，保证对比公平；但Omniobject3D数据集本身不含点云颜色，导致纯点云方法（如OpenShape）性能骤降，而OpenView利用多视图颜色获得优势，可能存在数据预处理不公平（点云颜色缺失）。

## 6. 主要结论与发现

- **性能提升**：在Objaverse-LVIS上，OpenView比SOTA（Uni3D）在零样本分类Top-1提高5.5%（47.2%→52.7%），在Text→3D检索R@1提高11.5%（56.1%→67.6%）。
- **多视图价值验证**：去除多视图导致性能下降超10%（分类Top-1从51.8%降至39.0%），证明视觉细节的关键作用。
- **融合编码器有效性**：替换为基础transformer后性能下降13.6%以上，说明专门设计的交叉注意力机制必要。
- **渐进式学习收益**：使性能额外提升约1.2%。
- **泛化能力**：在无纹理的ModelNet40上提升不大（点云信息已足够），但在真实扫描且有纹理的Omniobject3D上提升显著。

## 7. 优点（方法或实验设计亮点）

- **创新融合范式**：首次系统性地将多视图图像与点云结合用于3D语义表示，并设计针对性的跨模态融合编码器，解决模态间差距（余弦相似度0.25→通过交叉注意力提升）。
- **序列无关设计**：解决多视图顺序敏感性，使框架可适应任意数量和顺序的输入。
- **渐进式困难学习**：新颖的训练策略，自动筛选和丢弃“容易”视角，迫使模型学习更难、更全局的特征，类似课程学习逆序。
- **丰富的应用展示**：不仅报告标准任务，还演示了3D描述、层次聚类等实际场景，体现通用性。
- **高效训练**：冻结大模型（OpenCLIP），仅训练融合编码器+微调点云编码器，24小时完成，资源可控。
- **开源友好**：代码和模型可用于复现。

## 8. 不足与局限

- **实验覆盖局限**：
  - 评估数据集主要来自合成和电商，未在自动驾驶（如nuScenes、Waymo）或移动机器人场景测试。
  - 仅使用10张视图，未分析更多视图（如20、36）是否带来进一步提升。
- **依赖预训练模型**：图像编码器和文本编码器冻结自OpenCLIP，可能继承其偏见（如文化、语义倾向），且限制了跨领域迁移能力。
- **对无纹理数据效果有限**：ModelNet40上优势不明显，说明多视图颜色信息对纯形状识别帮助不大，甚至引入干扰。
- **未考虑对抗鲁棒性**：论文提及但未实验，多视图图像可能易受攻击，影响模型安全。
- **计算资源要求**：8×V100 24小时仍不低，对资金有限的研究团队有门槛。
- **数据预处理成本**：需要采集或渲染多视图图像，对于只有点云存储的数据库增加额外工作量。
- **消融实验部分细节缺失**：如渐进式学习的比例选择未做参数敏感性分析；不同数量图像实验仅3、6、10三档，未测试10以上。

（完）
