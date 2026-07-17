---
title: Distilling Spectral Graph for Object-Context Aware Open-Vocabulary Semantic Segmentation
title_zh: 利用谱图蒸馏实现物体上下文感知的开放词汇语义分割
authors: "Kim, Chanyoung, Ju, Dayun, Han, Woojung, Yang, Ming-Hsuan, Hwang, Seong Jae"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Kim_Distilling_Spectral_Graph_for_Object-Context_Aware_Open-Vocabulary_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 利用谱图蒸馏实现物体上下文感知的开放词汇语义分割
tldr: 针对开放词汇语义分割缺乏物体级上下文考虑的问题，本文提出基于谱图蒸馏的无需训练方法，通过图结构捕获物体内语义一致元素，提升任意查询词下的分割精度，无需任何训练即可适应新类别。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 867, \"height\": 938, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1791, \"height\": 634, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 856, \"height\": 696, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 858, \"height\": 474, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 861, \"height\": 500, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1807, \"height\": 675, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 863, \"height\": 720, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 865, \"height\": 640, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1800, \"height\": 826, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-kim-distilling-spectral-graph-for-object-context-aware-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 861, \"height\": 266, \"label\": \"Table\"}]"
motivation: 开放词汇语义分割需考虑物体级上下文以实现更精确的语义分组。
method: 通过谱图蒸馏将物体上下文融入视觉语言模型，无需训练即可适应新类别。
result: 在多个基准上达到无需训练方法的最优，证明了上下文的重要性。
conclusion: 物体上下文信息能显著提升开放词汇语义分割的准确性。
---

## Abstract
Open-Vocabulary Semantic Segmentation (OVSS) has advanced with recent vision-language models (VLMs), enabling segmentation beyond predefined categories through various learning schemes. Notably, training-free methods offer scalable, easily deployable solutions for handling unseen data, a key goal of OVSS. Yet, a critical issue persists: lack of object-level context consideration when segmenting complex objects in the challenging environment of OVSS based on arbitrary query prompts. This oversight limits models' ability to group semantically consistent elements within object and map them precisely to user-defined arbitrary classes. In this work, we introduce a novel approach that overcomes this limitation by incorporating object-level contextual knowledge within images. Specifically, our model enhances intra-object consistency by distilling spectral-driven features from vision foundation models into the attention mechanism of the visual encoder, enabling semantically coherent components to form a single object mask. Additionally, we refine the text embeddings with zero-shot object presence likelihood to ensure accurate alignment with the specific objects represented in the images. By leveraging object-level contextual knowledge, our proposed approach achieves state-of-the-art performance with strong generalizability across diverse datasets.

---

## 论文详细总结（自动生成）

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：开放词汇语义分割（OVSS）旨在根据用户任意查询提示分割图像中的物体，但现有训练‑free 方法（如基于 CLIP 的方法）缺乏**物体级上下文**考虑，导致同一物体的不同部件（如卡车的车厢、车轮）无法被统一分组为一个语义实体。
- **背景**：训练‑free 方法具有可扩展性和易部署性，这类方法的核心是利用预训练的视觉‑语言模型（VLM，如 CLIP）直接推理，但 CLIP 的全局图像对齐特性使其难以捕捉密集的物体级语义关系。
- **动机**：通过引入物体级上下文（object‑level context），增强物体内的语义一致性和与用户提示的精确对齐，从而提升 OVSS 在任意类别上的分割质量。

## 2. 论文提出的方法论

- **核心思想**：  
  将视觉基础模型（VFM，如 DINO）中的物体级结构信息蒸馏到 CLIP 的注意力机制中，同时利用 CLIP 的零样本物体分类能力（物体存在先验）调整文本嵌入和相似度计算，实现无需训练的物体上下文感知分割。

- **关键技术细节**：
  - **谱图蒸馏（Spectral Object‑Level Context Distillation）**  
    1. 将 VFM 和 CLIP 的多头注意力图视为图，通过特征分解得到各头的特征值。  
    2. 使用 Wasserstein 距离计算图间结构差异构造代价矩阵，并用匈牙利算法匹配**互补**的头对（即结构差异最大的头）。  
    3. 对匹配后的 VFM 图进行低秩近似（能量保留策略）和动态特征缩放（放大主要特征值、抑制噪声），得到去噪增强的 VFM 图 $\ddot{A}_{\text{VFM}}$。  
    4. 将 $\ddot{A}_{\text{VFM}}$ 与 CLIP 的原始图 $A_{\text{CLIP}}$ 按 Wasserstein 距离加权融合，得到增强后的注意力图 $A_{\psi}$，替换原注意力以生成更一致的物体特征。

  - **物体存在先验驱动的文本调整（Object Presence‑Driven Object‑Level Context）**  
    1. **物体存在先验计算**：对全图提取 CLIP 的 [CLS] 特征，与所有文本类别嵌入计算相似度，得到每个类别的出现概率 $P(i)$。  
    2. **文本嵌入调整**：将文本嵌入按 CLIP 语义距离分层聚类，在每类簇内基于 $P(i)$ 选择最可能出现的类别，并利用图像中与该类别最相似的 $n$ 个 patch 特征均值 $\mu_n$ 对该文本嵌入进行方向引导（加权插值）。  
    3. **相似度细化**：将全图物体先验 $P(i)$ 与 patch‑text 相似度 $\hat{S}$ 加权融合，得到最终分割熵。

## 3. 实验设计

- **数据集**：  
  - PASCAL VOC 2012（V21/20）、PASCAL Context（PC60/59）、COCO‑Stuff（C‑Obj / C‑Stf）、ADE20K（ADE）、Cityscapes（City），共 8 个变体。  
- **Benchmark**：  
  - 主要指标：mIoU（平均交并比）和 pAcc（像素精度）。  
- **对比方法**：  
  - 需要额外数据/训练的方法：GroupViT、TCL、CoDe、CLIP‑DINOiser 等。  
  - 训练‑free 方法：CLIP、MaskCLIP、GEM、CaR、PnP‑OVSS、CLIPtrase、ClearCLIP、SCLIP、LaVG、ProxyCLIP、NACLIP 等。  
- **公平性**：所有对比方法均在相同设置下（CLIP ViT‑B/16，滑动窗口 224×224）复现或引用原文结果，不使用后处理（如 CRF）。

## 4. 资源与算力

- **文中提及**：所有实验使用 NVIDIA RTX A6000 Ada*（标注为“Advanced Database System Infrastructure”）。  
- **未明确信息**：未说明使用的 GPU 数量、训练时长（因方法为训练‑free，仅需推理过程，故无训练耗时）。推理时采用滑动窗口，窗口大小 224×224，步长 112，较短边设为 336/560 像素。

## 5. 实验数量与充分性

- **实验数量**：  
  - 主表（Table 1）在 8 个数据集上报告 mIoU，对比 14+ 种方法。  
  - 主表（Table 2）在 8 个数据集上报告 pAcc。  
  - 消融实验（Table 3）在 3 个代表性数据集（V21、PC59、C‑Stf）上逐步验证各组件（谱图匹配、低秩近似、动态缩放、文本调整等）。  
  - 额外消融：不同 CLIP 骨干（ViT‑B/32、ViT‑L/14）（Table 4）和不同距离度量（图 8）。  
  - 定性对比（图 3、图 4、图 6、图 7）。  
- **充分性与公平性**：  
  - 消融设计从基线逐步加入组件，清晰展示每个模块贡献。  
  - 对比方法尽可能涵盖近年 SOTA，并标注公平性（“Fair”列），排除需额外训练或支持数据的方法。  
  - 实验覆盖常见 OVSS 数据集，包含背景类与非背景类设置，评估指标全面。  
  - 不足：未提供在更复杂场景（如密集小物体、跨域迁移）上的系统性评估，且依赖固定预训练模型（CLIP + DINO），泛化边界可能受限。

## 6. 论文的主要结论与发现

- 提出 **CASS**（Context‑Aware Semantic Segmentation），通过蒸馏 VFM 的谱图结构到 CLIP，并结合物体存在先验，显著提升了训练‑free OVSS 中物体级上下文的理解。  
- 在 8 个数据集上平均 mIoU 达到 **44.4%**，比第二名（ProxyCLIP 41.4%）高出 **3.0 个百分点**；在 PASCAL VOC 20 类上提升 **5.3 个百分点**。  
- 定性结果（图 6、7）显示 CASS 能有效将同一物体的各个部分（如车轮、躯干、四肢）统一分割为一个语义实体，消除了基线方法中的碎片化误分类。  
- 消融实验证实：谱图匹配、低秩动态缩放、文本引导等每个组件均贡献显著，且 Wasserstein 距离作为图匹配度量优于其他距离。

## 7. 优点

- **方法创新性**：首次将谱图理论引入训练‑free OVSS，利用 VFM 的图结构增强 CLIP 的物体感知，而不仅仅是特征级融合。  
- **无需训练**：完全基于预训练模型的推理后处理，无需任何微调或额外数据，契合 OVSS 的实用需求。  
- **实验全面**：覆盖多个主流数据集和多种基线方法，消融细致，验证了各模块有效性。  
- **代码开源**：提供项目页面 (https://micv-yonsei.github.io/cass/)，有利于复现与应用。

## 8. 不足与局限

- **实验覆盖局限**：未在极端小物体、遮挡严重或跨域场景（如医学、遥感）上评估，泛化能力需进一步验证。  
- **依赖预训练模型**：方法性能受限于 CLIP 和 DINO 的预训练质量，若这些模型在特定领域（如医疗、工业）预训练不足，可能效果下降。  
- **计算开销**：虽然无需训练，但推理时需要同时运行 CLIP 和 DINO 两个模型（ViT‑B/16 + ViT‑B/8），并执行特征分解和匈牙利匹配，可能比单独 CLIP 推理略慢。  
- **超参数选择**：部分超参数（如能量保留比例、$k$ 值、$\alpha$、$\gamma$、$n$）需根据数据集调整，文中未探讨其鲁棒性。  
- **未讨论失败案例**：定性图中未展示错误分割的样本，缺少对极限情况的分析。

（完）
