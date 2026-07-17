---
title: Parameter-efficient Fine-tuning in Hyperspherical Space for Open-vocabulary Semantic Segmentation
title_zh: 超球面空间中的参数高效微调用于开放词汇语义分割
authors: "Peng, Zelin, Xu, Zhengqin, Zeng, Zhilin, Huang, Yu, Wang, Yaoming, Shen, Wei"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Peng_Parameter-efficient_Fine-tuning_in_Hyperspherical_Space_for_Open-vocabulary_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 开放词汇语义分割，参数高效微调CLIP
tldr: 微调CLIP进行开放词汇分割存在计算成本高、模态对齐差及泛化下降的问题。本文提出在超球面对称参数高效微调策略，同时调整CLIP两个模态，保持低计算开销的同时改善模态对齐。实验表明该方法在多种分割基准上性能优异，且对未见类别泛化良好。不过，该方法主要面向二维图像分割。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1804, \"height\": 694, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1810, \"height\": 1006, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 862, \"height\": 300, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 1112, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 551, \"height\": 468, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1533, \"height\": 605, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-peng-parameter-efficient-fine-tuning-in-hyperspherical-space-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1478, \"height\": 416, \"label\": \"Table\"}]"
motivation: 微调CLIP进行像素级预测存在高计算成本、模态不对齐和泛化下降的问题。
method: 在超球面对CLIP两个模态进行对称参数高效微调，使用一系列高效模块。
result: 在多个语义分割数据集上取得最佳性能，且计算开销低，泛化能力强。
conclusion: 所提策略解决了微调CLIP的三大问题，为开放词汇分割提供了轻量化方案。
---

## Abstract
Open-vocabulary semantic segmentation seeks to label each pixel in an image with arbitrary text descriptions. Vision-language foundation models, especially CLIP, have recently emerged as powerful tools for acquiring open-vocabulary capabilities. However, fine-tuning CLIP to equip it with pixel-level prediction ability often suffers three issues: 1) high computational cost, 2) misalignment between the two inherent modalities of CLIP, and 3) degraded generalization ability on unseen categories. To address these issues, we propose \alg, a symmetrical parameter-efficient fine-tuning (PEFT) strategy conducted in hyperspherical space for both of the two CLIP modalities. Specifically, the PEFT strategy is achieved by a series of efficient block-diagonal learnable transformation matrices and a dual cross-relation communication module among all learnable matrices. Since the PEFT strategy is conducted symmetrically to the two CLIP modalities, the misalignment between them is mitigated. Furthermore, we apply an additional constraint to PEFT on the CLIP text encoder according to the hyperspherical energy principle, i.e., minimizing hyperspherical energy during fine-tuning preserves the intrinsic structure of the original parameter space, to prevent the destruction of the generalization ability offered by the CLIP text encoder. Extensive evaluations across various benchmarks show that H-CLIP achieves new SOTA open-vocabulary semantic segmentation results while only requiring updating approximately 4% of the total parameters of CLIP.

---

## 论文详细总结（自动生成）

# 论文详细中文总结：《超球面空间中的参数高效微调用于开放词汇语义分割》（H-CLIP）

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：开放词汇语义分割的目标是根据任意文本描述对图像中的每个像素进行标注。视觉-语言基础模型（尤其是CLIP）已成为赋予模型开放词汇能力的有力工具。然而，微调CLIP以使其具备像素级预测能力面临三个主要问题：  
  - **高计算成本**：全参数微调计算开销大。  
  - **模态不对齐**：现有方法通常不对称地冻结文本编码器而微调图像编码器，导致图像编码器从“图像-文本”对齐转向“像素-文本”对齐，而文本编码器仍保持“图像-文本”对齐，两种对齐粒度不一致，优化受阻。  
  - **泛化能力下降**：在有限类别上微调会破坏CLIP在未见类别上的泛化能力。

- **整体含义**：如何在保持CLIP泛化能力的同时高效地赋予其像素级分割能力，是当前开放词汇语义分割的关键挑战。H-CLIP通过对称的参数高效微调（PEFT）策略和超球面空间约束，试图同时解决上述三个问题。

## 2. 方法论

### 2.1 核心思想

- **对称参数高效微调**：同时对CLIP的图像编码器和文本编码器进行微调，但引入的参数量很少（约CLIP总参数的4%），从而缓解模态不对齐。
- **超球面空间**：在超球面空间中进行微调，利用超球面能量原理保持参数空间的固有结构，防止泛化能力受损。
- **正交约束**：对文本编码器的可学习矩阵施加正交约束（通过Cayley参数化），使其仅做旋转/反射变换，从而保持超球面能量不变。

### 2.2 关键技术细节

1. **部分正交参数化（POP, Partial Orthogonal Parameterization）**  
   - 为CLIP中每个预训练权重矩阵引入一个可学习的块对角变换矩阵 \( R \)。  
   - 对文本编码器中的 \( R \) 施加正交约束：\( R^\top R = I \)，通过Cayley变换 \( R = (I+Q)(I-Q)^{-1} \) 实现，其中 \( Q \) 是斜对称矩阵。  
   - 采用块对角结构（分 \( b \) 块）以提高效率，将每个变换矩阵的维度降为 \( d/b \)。图像编码器的 \( R \) 不要求正交，从而保留学习新能力的灵活性。

2. **双跨关系通信模块（DCRC, Dual Cross-Relation Communication）**  
   - 利用张量积操作，将所有层的块对角矩阵组织成一个4阶张量 \( T \in \mathbb{R}^{q \times q \times (b+b) \times L} \)。  
   - 通过两个可学习的变换矩阵（或深层神经网络）分别沿“模态-层”（第3维）和“层间”（第4维）进行通信，促进跨模态和跨层的参数交互。  
   - 具体实现：\( T_w = T \times_3 S_3 \times_4 S_4 \)，再用浅层MLP替代线性变换以引入非线性。最终更新 \( T = T + \alpha T_w \)，\( \alpha \) 可学习。

3. **损失函数**：采用交叉熵损失 \( L_{ce} \) 进行微调。

### 2.3 算法流程（文字描述）

1. 加载预训练CLIP（图像编码器和文本编码器），冻结所有原始参数。  
2. 为每个Transformer层（共L层）的可学习权重矩阵构造块对角变换矩阵 \( R \)（图像和文本各有b块）。  
3. 对所有文本编码器的 \( R \) 施加正交约束（POP）；图像编码器的 \( R \) 不约束。  
4. 将各层的所有 \( R \) 组织成4阶张量 \( T \)，通过DCRC进行跨模态和跨层通信，得到更新后的 \( T \)。  
5. 使用更新后的变换矩阵调整每层的特征映射：\( \tilde{M}_\ell = F_\ell(M_\ell; T_\ell W_\ell) \)。  
6. 在分割数据集上（COCO-Stuff）微调所有可学习参数，包括 \( R \) 矩阵和DCRC中的 \( S_3, S_4 \) 等，损失为交叉熵。

## 3. 实验设计

### 3.1 数据集与基准

- **训练集**：COCO-Stuff（约118K张图像，171个语义类别）。  
- **测试集**（三个基准共6个测试集）：  
  - ADE20K (A-847, A-150)  
  - PASCAL VOC (PAS-20, PAS-20b)  
  - PASCAL-Context (PC-459, PC-59)  
- **评估指标**：mIoU（平均交并比）。

### 3.2 比较方法

- **传统微调方法**：ZS3Net, LSeg, ZegFormer, ZSseg, OpenSeg, OVSeg, ZegCLIP, SED, CAT-Seg, FC-CLIP, ODISE等。  
- **参数高效微调方法**：SAN（Side Adapter），以及通用的PEFT方法（LoRA, OFT, VPT, Adapter, LST, SSF）用于消融比较。  
- **CLIP变体**：ViT-B/16和ViT-L/14两种主干。

## 4. 资源与算力

- **文中说明**：  
  - 使用4张NVIDIA RTX 3090 GPU。  
  - 训练迭代80,000次。  
  - 批大小：每GPU 1张图像（即总批大小4？文中写“one image per mini-batch”，但多卡并行可能每卡1张，合计4张）。  
  - 优化器：Adam，初始学习率 \(5\times10^{-6}\)，权重衰减 \(10^{-4}\)。  
- **训练时间**：在ViT-B/16上，H-CLIP训练耗时约12.2小时（表2对比LoRA 11.8h, OFT 16.1h, VPT 14.1h）。  
- **可训练参数量**：约5.6M（仅CLIP部分，不包括分割解码器），约为CLIP总参数的4%。

## 5. 实验数量与充分性

- **实验组数**：  
  - **主表对比**（表1）：在两个主干（ViT-B/16, ViT-L/14）上，在6个测试集上与~15种方法对比，共12组性能数字。  
  - **效率对比**（表2）：与OVSeg, CAT-Seg, SAN对比可训练参数量；与LoRA, VPT, OFT对比训练时间。  
  - **消融实验**（表3）：对POP和DCRC两个主要组件进行消融（共8种设置），同时与6种通用PEFT方法对比。  
  - **POP设计分析**（表4）：不同块维度（q=64/128/256）和不同正交约束策略（无约束、全部约束、仅文本约束）的影响。  
  - **跨任务泛化**（表5）：在开放词汇目标检测任务（COCO）上验证（AP50）。  
  - **定性结果**（图2）：与CAT-Seg的视觉对比。  
- **充分性评价**：实验覆盖了主要基准、主流对比方法、关键消融、效率分析以及跨任务泛化。消融实验对比了多种PEFT基线，并单独分析POP和DCRC的贡献，充分验证了设计选择。公平性方面，对比方法均采用相同或相似设置（如均基于COCO-Stuff训练）。结论支持充分。

## 6. 主要结论与发现

1. **H-CLIP在6个测试集上均取得最佳或次佳性能**，在ViT-L/14上整体超越CAT-Seg（例如A-150: 38.4 vs 37.9, PAS-20: 97.7 vs 97.0）。  
2. **仅更新约4%的参数**即可达到SOTA，远少于传统全微调或部分微调方法（如OVSeg需147M参数）。  
3. **对称微调有效缓解模态不对齐**：同时微调图像和文本编码器（但文本编码器被正交约束）比仅微调图像编码器（如SAN）性能更优。  
4. **超球面能量原则指导的正交约束是关键**：对文本编码器施加正交约束可保护泛化，而对图像编码器不施加约束则保证了学习新能力的灵活性。  
5. **DCRC通过跨模态、跨层通信进一步提升了对齐和性能**，仅增加极少量参数（0.01M）。  
6. **方法可泛化到其他密集预测任务**（如开放词汇目标检测），验证了通用性。

## 7. 优点

- **创新性**：首次将超球面能量原理引入CLIP微调用于语义分割，并提出对称PEFT框架，从理论上解释了正交约束保护泛化的机制。  
- **效率高**：仅4%参数更新，训练时间和内存消耗低，易于部署。  
- **性能强**：在多个基准上一致领先，且对未见类别泛化能力突出（如A-847数据集提升显著）。  
- **设计简洁**：POP和DCRC模块轻量且可并行化，DCRC通过张量运算实现，计算开销小。  
- **实验全面**：涵盖多数据集、多主干、消融、效率、跨任务验证，结果可靠。

## 8. 不足与局限

- **实验覆盖**：主要面向二维图像分割，未涉及视频或3D点云分割任务。  
- **应用限制**：方法依赖预训练的CLIP，若CLIP针对特定领域（如医学图像）未预训练，可能效果有限。  
- **可解释性**：DCRC中张量变换和MLP引入的非线性缺乏直观解释，其具体作用机制分析不够深入。  
- **对比公平性**：部分对比方法（如CAT-Seg）使用不同的解码器，尽管实验尽量对齐，但解码器差异可能引入一定不公平。  
- **偏差风险**：训练集仅使用COCO-Stuff，虽然该数据集类别多样，但仍可能存在场景偏差（如室内外分布不均）。  
- **超参数依赖**：块维度q和正交约束策略需要手工调整，文中仅报告了最佳设置（q=128），缺乏自动搜索。

（完）
