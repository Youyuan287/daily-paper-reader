---
title: Efficient Decoupled Feature 3D Gaussian Splatting via Hierarchical Compression
title_zh: 高效解耦特征3D高斯泼溅通过分层压缩
authors: "Dai, Zhenqi, Liu, Ting, Zhang, Yanning"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Dai_Efficient_Decoupled_Feature_3D_Gaussian_Splatting_via_Hierarchical_Compression_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 6.0
evidence: 在3D高斯泼溅中解耦语义特征场
tldr: 针对现有3DGS方法将颜色和语义特征耦合导致存储和计算开销大的问题，提出DF-3DGS分离颜色和语义场，减少语义高斯数量。引入分层压缩策略，包括动态码本量化和场景修剪，大幅降低存储需求。该方法的解耦思路可高效构建语义场，间接支持开放词汇场景的特征存储与查询。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 832, \"height\": 670, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1671, \"height\": 775, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1624, \"height\": 861, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 874, \"height\": 378, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 871, \"height\": 379, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 870, \"height\": 248, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 872, \"height\": 543, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1824, \"height\": 429, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-dai-efficient-decoupled-feature-3d-gaussian-splatting-via-hierarchical-compression-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 885, \"height\": 322, \"label\": \"Table\"}]"
motivation: 现有3DGS方法嵌入语义特征导致存储和计算开销大。
method: 解耦颜色和语义场，提出动态码本进化量化与场景修剪的分层压缩策略。
result: 在保证语义精度的前提下，显著减少了高斯数量和存储占用。
conclusion: 解耦表示与压缩技术为高效3D语义场提供了可行方案。
---

## Abstract
Efficient 3D scene representation has become a key challenge with the rise of 3D Gaussian Splatting (3DGS), particularly when incorporating semantic information into the scene representation. Existing 3DGS-based methods embed both color and high-dimensional semantic features into a single field, leading to significant storage and computational overhead. To mitigate this, we propose Decoupled Feature 3D Gaussian Splatting (DF-3DGS), a novel method that decouples the color and semantic fields, thereby reducing the number of 3D Gaussians required for semantic representation. We then introduce a hierarchical compression strategy that first employs our novel quantization approach with dynamic codebook evolution to reduce data size, followed by a scene-specific autoencoder for further compression of the semantic feature dimensions. This multi-stage approach results in a compact representation that enhances both storage efficiency and reconstruction speed. Experimental results demonstrate that DF-3DGS outperforms previous 3DGS-based methods, achieving faster training and rendering times while requiring less storage, without sacrificing performance--in fact, it improves performance in the novel view semantic segmentation task. Specifically, DF-3DGS achieves remarkable improvements over Feature 3DGS, reducing training time by 10xand storage by 20x, while improving the mIoU of novel view semantic segmentation by 4%. The code will be publicly available.

---

## 论文详细总结（自动生成）

# 高效解耦特征3D高斯泼溅通过分层压缩——论文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：3D高斯泼溅（3DGS）在高质量新视角合成方面表现出色，但仅包含外观和几何信息，缺乏语义理解能力。现有方法（如Feature 3DGS、LangSplat、LEGaussians）将颜色和语义特征耦合到同一个3D高斯场中，导致大量高维语义特征嵌入到巨量高斯中，带来巨大存储和计算开销。
- **核心问题**：如何在保持甚至提升语义分割精度的前提下，大幅降低3DGS语义场的存储占用、训练时间和渲染时间。
- **整体含义**：本文提出一种解耦颜色场与语义场的3DGS方法（DF-3DGS），利用语义场本身比颜色场更稀疏的特性（高频信息少），显著减少语义高斯数量；并设计分层压缩策略（自适应量化+场景专用自编码器），进一步压缩语义特征维度，实现高效、紧凑的3D语义表示。

## 2. 论文提出的方法论：核心思想、关键技术细节

### 核心思想
- **解耦颜色和语义场**：将原本耦合在一个3D高斯中的颜色参数（SH系数）和语义特征分开。语义场独立构建，只包含位置、尺度、旋转、不透明度和低维语义特征，不存储颜色信息。这样，语义场所需的3D高斯数量大幅减少（实验显示不到颜色场的5%）。
- **分层压缩策略**：
  1. **自适应数据压缩（ADC）**：使用一种新颖的量化方法，结合动态码本进化（adaptively evolving codebook），将原始高维语义特征（如512维）离散化为码本中的索引，并通过聚类效应增强特征鲁棒性，同时消除同类语义内部的噪声。
  2. **语义压缩（SC）**：训练一个场景专用的自编码器（Encoder+Decoder），将码本中每个码字（codeword）从原始高维空间压缩到低维潜空间（例如9维），然后使用压缩后的潜特征训练解耦的3D高斯语义场。

### 关键技术细节
- **解耦语义场渲染**：语义场独立渲染，公式为 \( F_r = R(p_2, s_2, q_2, o_2, f'; p_{cam}) \)，只输出语义特征图，不渲染颜色。
- **动态码本进化**：
  - 初始化：从第一张图像的特征图中使用最远点采样（FPS）选取ρ个点作为初始码字。
  - 扩展：若查询特征与最近码字的余弦相似度低于阈值α，则将该特征加入码本（限制最大容量Kmax）。
  - 剪枝：若码本中两个码字余弦相似度超过β，则删除冗余码字。
- **量化损失**：对每张图像的特征图，通过最大化余弦相似度映射到码本得到索引图，然后计算每个类别（码字）的平均余弦距离损失，并平衡类别不平衡。
- **自动编码器训练**：使用L2重建损失，将码本中的高维特征映射到低维，再将低维潜特征作为3D高斯的属性进行训练。
- **下游任务应用**：
  - 新视角语义分割：渲染潜特征图 → 解码器恢复原始维度 → 通过码本量化得到索引图 → 根据每个码字与文本特征的余弦相似度分配标签。
  - 语言引导编辑：将语义场中高斯标签通过空间位置近邻映射到颜色场的高斯上，实现对颜色场中特定类别物体的删除、变色等操作。

## 3. 实验设计：数据集、基准、对比方法

- **数据集**：Replica数据集（室内场景），使用5个场景：room0、room1、office0、office2、office3。
- **数据预处理**：每个场景沿随机轨迹捕获900张640×480图像，用COLMAP估计相机位姿并进行去畸变；训练/测试集划分：每10张取第1张为测试，第6张为训练，各约90张。
- **语义特征**：采用预训练LSeg模型提取360×480×512的原始语义特征。
- **对比方法**：
  - Feature 3DGS（CVPR 2024）：耦合场，语义特征维度512或128（加速版）。
  - LangSplat（CVPR 2024）：三维语言高斯泼溅，三个语义级别。
  - LEGaussians（CVPR 2024）：语言嵌入3D高斯，维度8。
- **评估指标**：
  - 新视角语义分割：mIoU（基于手动修正的10类标签）；渲染FPS；语义存储大小（含高斯属性+额外数据如解码器）；训练时间。
- **公平性**：所有对比方法按默认参数运行；测试集一致；存储计算中，本方法计入语义场所有高斯属性，其他方法只计语义特征（不含颜色参数），但即使如此本方法仍显著更小。

## 4. 资源与算力

- **硬件**：Intel Xeon Gold 6326 CPU + NVIDIA GeForce RTX 3090 GPU（单卡）。
- **训练时长**：
  - 完整DF-3DGS（解耦+ADC+SC）：约8分钟/场景。
  - 仅解耦+ADC（无自编码器）：约220分钟/场景。
  - Feature 3DGS（512维）：约351分钟/场景。
- **文中未明确说明GPU数量，推测为单卡。**

## 5. 实验数量与充分性

- **主要实验**：在所有5个Replica场景上进行了定量比较（Tab.1），报告平均指标，质量较高。
- **消融实验**：
  - 解耦 vs 耦合（Feature 3DGS）（Tab.1中"just decouple"行）。
  - 自适应压缩（ADC）效果（Tab.1中"decouple+ADC"）。
  - 语义压缩（SC）效果（Tab.1中完整方法）。
  - 不同潜特征维度（3,6,9,12,15,512）对mIoU、训练时间、高斯数、存储、FPS的影响（Tab.2）。
- **定性实验**：新视角语义分割结果展示（Fig.3）、语言引导编辑（删除椅子和植物变色，Fig.4&5）。
- **充分性评价**：实验较充分，涵盖了关键消融、不同维度、与多种SOTA对比。但仅使用Replica单一数据集，未在更多场景（如真实室外、大规模场景）上验证，可能限制泛化性。另外，对比方法中缺少与基于NeRF的语义方法（如DFF、NeRF）的比较（虽在引言中提及，但实验未涵盖）。

## 6. 论文的主要结论与发现

- **解耦有效**：仅解耦即可将语义高斯数量降至原来的<5%（602.2k vs 28.7k），存储减少95%以上，训练时间缩短约1.5小时，mIoU仅下降0.4%（73.9%→73.5%），说明语义场稀疏性显著。
- **自适应压缩提升精度**：ADC将mIoU提升4.4%（73.5%→77.9%），表明量化后的特征更鲁棒，消除了同类噪声。
- **最终完整方法**：在保持高精度（mIoU 76.6%，超过Feature 3DGS的73.9%）的同时，训练时间从351分钟降至8分钟（约44倍），存储从1175MB降至13MB（约90倍），渲染FPS从5.66提升至36.08（实时）。
- **语言引导编辑可行**：通过空间位置映射，解耦的语义场可间接驱动颜色场的编辑操作。

## 7. 优点：方法或实验设计上的亮点

- **创新性**：首次提出解耦颜色和语义场的3DGS方法，利用语义稀疏性大幅减少高斯数量，思路新颖。
- **高效性**：分层压缩策略（自适应量化+自编码器）在几乎不损失精度的情况下实现巨大压缩比（训练时间10倍，存储20倍以上）。
- **鲁棒性提升**：量化通过聚类效应增强了特征区分度，反而提升了语义分割精度。
- **动态码本设计**：适用于不同场景复杂度，无需手动设定码本大小，具备自适应性。
- **实验充分**：包含详细的消融、维度分析、定性结果、编辑应用，对比了多种最新3DGS语义方法。

## 8. 不足与局限

- **数据集单一**：仅用了Replica（室内合成场景），未在真实复杂场景（如Mip-NeRF 360、大规模户外场景）上验证，泛化性存疑。
- **对比方法局限**：未与基于NeRF的语义方法（如DFF、LERF、NeRF语义蒸馏）比较，虽然其速度慢，但精度表现应纳入考量。
- **存储统计口径不完全公平**：本方法将语义场所有高斯参数计入，而对比方法只计语义特征（不包括颜色参数），虽仍占优，但可能略微低估本方法优势。
- **自动编码器训练需要场景特定**：对新场景需重新训练编码器/解码器，增加预处理时间（但文中已计入训练时间）。
- **码本扩展/剪枝超参数依赖**：α、β、Kmax需手动设定，不同场景最优值可能不同，文中仅设定固定值，未进行敏感性分析。
- **语言引导编辑依赖空间映射阈值λ**：该超参数未在文中进行消融或提供设置建议，可能影响编辑准确性。
- **FPS测量条件**：仅测量特征图渲染FPS，未包含解码、量化等后处理步骤，实际应用时整体延迟可能略高。

（完）
