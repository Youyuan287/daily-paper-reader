---
title: "3D-AVS: LiDAR-based 3D Auto-Vocabulary Segmentation"
title_zh: "3D-AVS: 基于激光雷达的三维自动词汇分割"
authors: "Wei, Weijie, Ülger, Osman, Nejadasl, Fatemeh Karimi, Gevers, Theo, Oswald, Martin R."
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wei_3D-AVS_LiDAR-based_3D_Auto-Vocabulary_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 自动词汇三维点云分割
tldr: 现有开放词汇分割方法依赖人工提供类别文本，限制了可扩展性。3D-AVS提出自动词汇分割方法，在运行时为每个输入自动生成词汇，然后对点云进行分割。该方法融合图像和点云识别，增强了鲁棒性，无需人类干预即可提供更丰富的语义标注。这项工作显著提升了三维场景理解的自动化水平。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 809, \"height\": 1110, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1765, \"height\": 467, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1692, \"height\": 444, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 846, \"height\": 297, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 859, \"height\": 765, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1761, \"height\": 901, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wei-3d-avs-lidar-based-3d-auto-vocabulary-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 860, \"height\": 451, \"label\": \"Table\"}]"
motivation: 现有开放词汇分割需要人工提供类别，限制了可扩展性；本文旨在消除人工干预，实现自动词汇生成。
method: 从图像或点云中自动识别语义实体，生成词汇，然后对所有点进行分割；融合图像和点云识别。
result: 在多个数据集上验证了自动词汇分割的有效性，实现了更丰富的语义标注。
conclusion: 3D-AVS通过自动生成词汇实现了免人工的三维开放词汇分割，提升了系统的可扩展性。
---

## Abstract
Open-vocabulary segmentation methods offer promising capabilities in detecting unseen object categories, but the category must be aware and needs to be provided by a human, either via a text prompt or pre-labeled datasets, thus limiting their scalability. We propose 3D-AVS, a method for Auto-Vocabulary Segmentation of 3D point clouds for which the vocabulary is unknown and auto-generated for each input at runtime, thus eliminating the human in the loop and typically providing a substantially larger vocabulary for richer annotations. 3D-AVS first recognizes semantic entities from image or point cloud data and then segments all points with the automatically generated vocabulary. Our method incorporates both image-based and point-based recognition, enhancing robustness under challenging lighting conditions where geometric information from LiDAR is especially valuable. Our point-based recognition features a Sparse Masked Attention Pooling (SMAP) module to enrich the diversity of recognized objects. To address the challenges of evaluating unknown vocabularies and avoid annotation biases from label synonyms, hierarchies, or semantic overlaps, we introduce the annotation-free Text-Point Semantic Similarity (TPSS) metric for assessing generated vocabulary quality. Our evaluations on nuScenes and ScanNet200 demonstrate 3D-AVS's ability to generate semantic classes with accurate point-wise segmentations.

---

## 论文详细总结（自动生成）

# 论文核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的开放词汇（Open-Vocabulary）三维点云分割方法虽然能够识别未见过的类别，但严重依赖人工提供的类别文本（text prompt）或预定义标签集，限制了其可扩展性和实际应用能力。人工预定义所有可能类别在动态世界中既不可扩展也不切实际，导致模型在遇到未预定义的稀有物体或新类别时失效。
- **研究背景**：现有感知方法通常假设训练数据包含所有感兴趣的类别，但公开数据集标注类别有限，且常遗漏稀有物体。开放词汇学习利用预训练的视觉-语言模型（如CLIP）建立视觉与语义之间的对应，但仍需人工指定查询。自动词汇分割（Auto-Vocabulary Segmentation）则旨在从感知数据中直接推断类别，无需人工参与，但在三维领域此前尚未被探索。
- **整体目标**：提出**3D-AVS**，第一个用于激光雷达点云的自动词汇分割框架，能够在运行时为每个输入场景自动生成词汇，并基于该词汇对所有点进行语义分割，消除人工在环，提供更丰富、更精确的语义标注。

# 方法论：核心思想、关键技术细节

- **核心思想**：采用“词汇生成 → 分割”的两阶段流水线。首先从图像或点云数据中自动识别语义实体，生成一组场景特定的名词标签（词汇）；然后利用开放词汇分割模型（如OpenScene）对点云进行逐点分类，将标签分配给每个点。
- **关键技术细节**：
  1. **场景标注（Scene Captioning）**：
     - **图像标注器**：采用xGen-MM（BLIP-3）为每张多视角图像生成详细描述，通过束搜索（beam search）增加多样性。
     - **点云标注器（Point Captioner）**：本文提出，基于LiDAR几何特征，无需颜色信息。采用**Sparse Masked Attention Pooling (SMAP)** 模块，将点云划分为多个区域（室外用圆柱扇区，室内用方柱），为每个区域生成独立描述，从而覆盖更多语义实体。训练时通过2D→3D蒸馏，利用CLIP图像特征监督SMAP池化后的点特征。
  2. **文本解析（Caption2Tag）**：使用spaCy提取名词并进行词形还原，再通过WordNet验证，得到标签集合。
  3. **分割（Segmentation）**：使用预对齐的CLIP文本编码器、高分辨率图像编码器和点编码器，计算每个点与所有标签的相似度（点特征来自点云或投影的像素特征），取最大值对应的标签作为预测。
- **公式/算法流程**（文字说明）：
  - 标注生成：\( \mathbf{D} = \text{ImageCaptioner}(\mathbf{I}) \) 或 \( \mathbf{D} = \text{PointCaptioner}(\mathbf{P}) \)
  - 文本解析：\( \mathbf{L} = \text{Caption2Tag}(\mathbf{D}) \)
  - 编码：\( \mathbf{E}_{\text{tx}} = h_{\text{tx}}(\mathbf{L}), \mathbf{F}_{\text{pt}} = h_{\text{pt}}(\mathbf{P}), \mathbf{F}_{\text{im}} = h_{\text{im}}(\mathbf{I}) \)
  - 标签分配：\( \hat{l}_n = \underset{m}{\mathrm{argmax}} \left( \max( \mathrm{SIM}(f_n, e_m) \parallel \mathrm{SIM}(f_n^{\text{im}}, e_m) ) \right) \)

# 实验设计

- **数据集**：
  - **nuScenes**：自动驾驶室外数据集，原始16个类别（LiDAR分割基准）。
  - **ScanNet**：室内数据集，20个类别。
  - **ScanNet200**：ScanNet的更新版，200个细粒度类别。
- **Benchmark**：自动词汇分割无标准基准，本文提出两种评估方式：
  1. **TPSS指标**：基于CLIP空间计算点与文本语义相似度，无需标注，评估生成词汇的质量。
  2. **LAVE映射+传统mIoU**：使用LLM（GPT-4o）将自动生成的词汇映射到固定标注类别，然后计算mIoU，以便与现有方法比较。
- **对比方法**：
  - 固定词汇方法：OpenScene [38]、CLIP2Scene [8]、ConceptFusion [21]、OpenMask3D [45]、HICL [22]、AdaCo [69]、CNS [7]、Diff2Scene [67]等。
  - 对比了使用官方标签集、扩展标签集（OpenScene手动定义的43个类）以及3D-AVS自动生成词汇（仅图像、仅LiDAR、两者结合）的性能。

# 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量及训练时长。仅在致谢中提及使用了荷兰国家电子基础设施（SURF），但无具体算力细节。因此无法量化训练成本。

# 实验数量与充分性

- **实验数量**：
  - 表1：TPSS对比（nuScenes和ScanNet上，比较官方标签、扩展标签、3D-AVS不同变体）。
  - 表2：mIoU对比（nuScenes、ScanNet、ScanNet200，与7种以上基线方法对比）。
  - 图5：不同光照和天气条件下TPSS分析（Daylight/Night, Not-Rainy/Rainy）。
  - 图6：定性结果展示。
  - 消融实验（在补充材料中，文中提及但未详细展开，包括图像标注器、点标注器、LAVE映射等）。
- **充分性**：
  - 实验覆盖室外（nuScenes）和室内（ScanNet/ScanNet200）两种场景，且考虑了不同光照条件。
  - 对比方法涵盖了近年主流开放词汇分割方法（10余种），对比全面。
  - 采用TPSS和mIoU两种互补评价方式，既评估词汇质量又评估分割精度，客观且公平。
  - 消融实验验证了图像+LiDAR融合收益、点标注器在不同条件下的鲁棒性。
  - 总体实验设计较为充分，但缺少更多真实极端场景的测试（如大雪、雾霾），且仅基于三个公开数据集。

# 主要结论与发现

- 3D-AVS生成的词汇比人工定义的词汇（包括扩展的43类）在TPSS指标上更高（表1），证明自动生成的标签语义上与点云更一致。
- 在nuScenes上，3D-AVS的mIoU（36.2）超过所有固定词汇方法，尤其对模糊类别（如“manmade”、“drivable surface”）提升显著。
- 在ScanNet200上，3D-AVS达到SOTA（14.6 mIoU），但在ScanNet（20类粗粒度）上略低于OpenScene，因为粗粒度映射损失了细粒度信息。
- 点标注器在夜间和雨天场景下比图像标注器更鲁棒，融合两者效果最佳（图5）。
- 提出的TPSS指标能有效评估自动词汇质量，避免标注偏见。

# 优点

- **创新性**：首次提出自动词汇三维点云分割任务，并构建完整流水线，消除人工在环。
- **鲁棒性**：融合图像和LiDAR双模态，在光照变化等困难条件下仍保持性能。
- **可扩展性**：词汇生成与分割解耦，可无缝集成任意开放词汇分割模型。
- **评估方法**：提出TPSS无监督指标，避免标注偏差，便于自动评估开放词汇。
- **SMAP模块**：通过几何划分实现可控数量的区域标注，提高词汇覆盖度。

# 不足与局限

- **实验覆盖有限**：仅测试了nuScenes、ScanNet、ScanNet200三个数据集，未在更多自动驾驶数据集（如SemanticKitti、Waymo）或极端天气下评估。
- **计算资源未报告**：无法评估方法的训练成本。
- **分割性能受限于映射**：在粗粒度数据集（ScanNet 20类）上，自动词汇映射到少类时信息丢失，导致mIoU低于固定词汇方法。
- **词汇质量依赖标注模型**：图像标注器（xGen-MM）和点标注器（训练）的质量直接影响最终结果，可能对特定场景产生偏差。
- **未与其他自动词汇方法对比**：由于三维领域无此前工作，仅对比了开放词汇方法，未与2D自动词汇方法进行公平比较。
- **潜在偏差**：LAVE映射使用GPT-4o，可能引入大模型自身偏见；点标注器训练依赖CLIP特征，受限于CLIP的分布。

（完）
