---
title: Cross-Modal and Uncertainty-Aware Agglomeration for Open-Vocabulary 3D Scene Understanding
title_zh: 跨模态与不确定性感知的聚集用于开放词汇3D场景理解
authors: "Li, Jinlong, Saltori, Cristiano, Poiesi, Fabio, Sebe, Nicu"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Li_Cross-Modal_and_Uncertainty-Aware_Agglomeration_for_Open-Vocabulary_3D_Scene_Understanding_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 集成多基础模型的开放词汇3D场景理解
tldr: 针对现有方法仅依赖单一视觉语言模型导致语义空间局限的问题，提出CUA-O3D，首次集成CLIP、DINOv2和Stable Diffusion等多个基础模型进行开放词汇3D场景理解。引入确定性不确定性估计自适应分配不同模型权重，缓解特征冲突。在开放词汇3D语义分割和检索任务上取得显著提升，直接匹配用户对开放词汇3D语义分割的核心需求。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 860, \"height\": 611, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 839, \"height\": 350, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1605, \"height\": 649, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1696, \"height\": 998, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 863, \"height\": 508, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1450, \"height\": 207, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 867, \"height\": 823, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 659, \"height\": 362, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1295, \"height\": 217, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 864, \"height\": 359, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-li-cross-modal-and-uncertainty-aware-agglomeration-for-open-vocabulary-3d-scene-understanding-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 865, \"height\": 307, \"label\": \"Table\"}]"
motivation: 现有开放词汇3D方法依赖单一VLM，限制了模型利用多种基础模型能力。
method: 集成多个基础模型（CLIP、DINOv2、Stable Diffusion），提出确定性不确定性估计自适应融合。
result: 在开放词汇3D语义分割和检索基准上，CUA-O3D达到了新的最优结果。
conclusion: 多模型集成与不确定性学习有效提升了开放词汇3D理解的鲁棒性。
---

## Abstract
The lack of a large-scale 3D-text corpus has led recent works to distill open-vocabulary knowledge from vision-language models (VLMs). However, these methods typically rely on a single VLM to align the feature spaces of 3D models within a common language space, which limits the potential of 3D models to leverage the diverse spatial and semantic capabilities encapsulated in various foundation models. In this paper, we propose Cross-modal and Uncertainty-aware Agglomeration for Open-vocabulary 3D Scene Understanding dubbed CUA-O3D, the first model to integrate multiple foundation models--such as CLIP, DINOv2, and Stable Diffusion--into 3D scene understanding. We further introduce a deterministic uncertainty estimation to adaptively distill and harmonize the heterogeneous 2D feature embeddings from these models. Our method addresses two key challenges: (1) incorporating semantic priors from VLMs alongside the geometric knowledge of spatially-aware vision foundation models, and (2) using a novel deterministic uncertainty estimation to capture model-specific uncertainties across diverse semantic and geometric sensitivities, helping to reconcile heterogeneous representations during training. Extensive experiments on ScanNetV2 and Matterport3D demonstrate that our method not only advances open-vocabulary segmentation but also achieves robust cross-domain alignment and competitive spatial perception capabilities.

---

## 论文详细总结（自动生成）

## 论文总结：CUA-O3D

### 1. 核心问题与整体含义（研究动机与背景）
- **问题**：开放词汇（Open-Vocabulary）3D场景理解因缺乏大规模3D-文本配对数据，常通过蒸馏视觉语言模型（VLM）的2D知识到3D模型。然而，现有方法多依赖单一VLM（如CLIP），限制了模型利用不同基础模型（如DINOv2、Stable Diffusion）所蕴含的多样化空间与语义能力。
- **动机**：不同2D基础模型具有异质且互补的特性（例如Lseg擅长语义对齐，DINOv2擅长几何感知，Stable Diffusion提供丰富视觉表征），但将它们统一集成到3D模型中面临特征分布不一致、多视图预测噪声等问题。
- **整体含义**：首次提出跨模态与不确定性感知的聚集方法（CUA-O3D），整合多个2D基础模型知识，同时通过确定性不确定性估计自适应处理异质性噪声，提升了3D开放词汇语义分割的鲁棒性与泛化能力。

### 2. 方法论：核心思想、关键技术细节
- **核心思想**：将多个2D预训练视觉编码器（Lseg、DINOv2、Stable Diffusion）分别提取的多视图图像特征投影到3D点云空间，作为独立蒸馏监督；同时引入确定性不确定性估计，为每个蒸馏损失学习一个观测噪声标量，动态调整各监督信号的权重，从而实现异构知识的自适应融合。
- **关键技术细节**：
  - **蒸馏聚集**：使用MinkowskiNet作为3D编码器。对每个2D模型独立设计投影MLP层和蒸馏损失：Lseg采用余弦相似度损失（L_cos_lseg），DINOv2采用L1损失（L_l1），Stable Diffusion采用余弦损失（L_cos_sd），并对其特征进行去均值操作以减轻异常值影响。
  - **确定性不确定性估计**：假设每个2D模型的蒸馏目标服从高斯分布，模型输出三个独立的噪声标量σ_i（对应三个教师）。总蒸馏损失定义为加权和：L_total = 1/(2σ₁²) L_cos_lseg + 1/(2σ₂²) L_l1 + 1/(2σ₃²) L_cos_sd + log(σ₁σ₂σ₃)。σ_i可学习，使得高噪声监督自动降权。训练后丢弃不确定性模块用于推理。
- **算法流程**：输入点云和RGB多视图图像→冻结的2D编码器提取多视图特征→投影至3D空间→与3D特征经MLP映射后计算加权蒸馏损失并更新3D模型。

### 3. 实验设计
- **数据集与预定义类别**：ScanNetV2（20/40类，1201训练/312验证/100测试场景）、Matterport3D（21类，90建筑）。
- **Benchmark**：开放词汇3D语义分割（零样本设置），评估指标为mIoU和mAcc。额外评估跨域泛化（ScanNet→Matterport3D及反向）、细粒度词汇（K=40,80,160）以及线性探测下游性能。
- **对比方法**：全监督方法（MinkowskiNet等）和零样本方法（OpenScene、PLA、CLIP2Scene、RegionPLC等）。

### 4. 资源与算力
- **文中明确说明**：单个NVIDIA A100 GPU分别用于ScanNetV2和Matterport3D实验。训练50个epoch，batch size=2，优化器Adam（初始学习率1e-4，指数衰减）。论文未提及总训练时长或GPU数量细节。

### 5. 实验数量与充分性
- **实验组数**：
  1. 开放词汇语义分割（表2）：在ScanNetV2和Matterport3D上与13种方法对比。
  2. 跨域泛化（表3）：两个方向（ScanNet→Matterport，Matterport→ScanNet）。
  3. 细粒度跨域实验（表4）：K=21,40,80,160，对比OpenScene。
  4. 线性探测下游性能（表5）：不同初始化及多head融合策略。
  5. 消融实验（表6）：逐步添加DINOv2、Stable Diffusion、不确定性模块、去均值等组件。
- **充分性**：实验覆盖主要基准、跨域能力、消融分析及下游任务，对比方法全面，消融设计合理（每步增加单一变量）。未进行参数敏感度分析或更广泛的基础模型组合实验（如加入RegNet等），但整体较为充分、公平。

### 6. 主要结论与发现
- **性能提升**：CUA-O3D在ScanNetV2上3D蒸馏设置下mIoU比OpenScene baselines高2.5%，在Matterport3D上高0.8%；跨域设置下提升1.4%~2.1% mIoU。
- **组件有效性**：单独添加DINOv2无明显增益，但融合Stable Diffusion后提升1.3%；引入不确定性估计提升2.1% mIoU；去均值操作进一步改善稳定性（表6）。
- **下游迁移**：线性探测在ScanNetV2上达到62.1% mIoU，比基线初始化提升7.7%，表明蒸馏后的特征更丰富。
- **互补性验证**：不同2D模型特征分布存在差异（图1），K-Means和UMAP可视化显示异质但互补，融合后聚类更一致（图4）。

### 7. 优点
- **创新性**：首次系统性地聚合多个2D基础模型（语义、几何、生成类）用于3D开放词汇学习，而非依赖单一VLM。
- **理论严谨**：采用确定性不确定性建模，将蒸馏损失加权转化为概率似然优化，理论合理且避免手动调参。
- **实验充分**：跨域、细粒度、线性探测等多角度验证了泛化能力，消融实验清晰地展示了每个组件贡献。
- **实用性强**：训练后仅需3D模型推理，不确定性模块可丢弃，不增加部署成本。

### 8. 不足与局限
- **实验覆盖**：仅测试了ScanNetV2和Matterport3D室内场景，未涉及室外（如SemanticKITTI）或自动驾驶场景，泛化性存疑。
- **模型依赖**：所选的三种2D模型（Lseg、DINOv2、Stable Diffusion）性能高度依赖其预训练数据分布，若基模型更新则需重新蒸馏；未探讨更多基础模型（如SAM、CLIP变体）。
- **偏差风险**：不确定性估计假设高斯分布，但实际2D模型特征可能服从更复杂分布；去均值操作虽缓解异常值但可能损失信息。
- **计算资源**：单GPU训练50个epoch，未报告推理速度或实时性；多视图投影步骤可能带来额外开销。
- **可重复性**：论文未提供超参数细节（如σ初始化值），部分代码未开源，可能影响复现。

（完）
