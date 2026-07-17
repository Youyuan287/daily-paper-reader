---
title: "DPSeg: Dual-Prompt Cost Volume Learning for Open-Vocabulary Semantic Segmentation"
title_zh: DPSeg：开放词汇语义分割的双提示成本体积学习
authors: "Zhao, Ziyu, Li, Xiaoguang, Shi, Lingjia, Imanpour, Nasrin, Wang, Song"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhao_DPSeg_Dual-Prompt_Cost_Volume_Learning_for_Open-Vocabulary_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 双提示成本体积学习的开放词汇语义分割
tldr: 开放词汇语义分割中CLIP嵌入存在域间隙且缺乏浅层特征引导。本文提出DPSeg，通过双提示成本体积生成和成本体积引导解码器，融合深层和浅层特征，提升了小物体和细节分割精度。实验在多个2D分割数据集上验证了有效性，但方法局限于2D图像。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 855, \"height\": 392, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 469, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 867, \"height\": 345, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1800, \"height\": 747, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 860, \"height\": 286, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 866, \"height\": 512, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1809, \"height\": 1056, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 860, \"height\": 248, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1812, \"height\": 990, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 830, \"height\": 398, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 876, \"height\": 362, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 835, \"height\": 293, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhao-dpseg-dual-prompt-cost-volume-learning-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 843, \"height\": 353, \"label\": \"Table\"}]"
motivation: CLIP的域间隙和浅层特征缺失限制了开放词汇分割性能。
method: 设计双提示成本体积生成和引导解码器，融合多层特征。
result: 在Pascal VOC等数据集上分割精度提升，小物体检测改善。
conclusion: DPSeg有效缓解了CLIP域间隙，但仅适用于2D图像。
---

## Abstract
Open-vocabulary semantic segmentation aims to segment images into distinct semantic regions for both seen and unseen categories at the pixel level. Current methods utilize text embeddings from pre-trained vision-language models like CLIP but struggle with the inherent domain gap between image and text embeddings, even after extensive alignment during training. Additionally, relying solely on deep text-aligned features limits shallow-level feature guidance, which is crucial for detecting small objects and fine details, ultimately reducing segmentation accuracy. To address these limitations, we propose a dual prompting framework, DPSeg, for this task. Our approach combines dual-prompt cost volume generation, a cost volume-guided decoder, and a semantic-guided prompt refinement strategy that leverages our dual prompting scheme to mitigate alignment issues in visual prompt generation. By incorporating visual embeddings from a visual prompt encoder, our approach reduces the domain gap between text and image embeddings while providing multi-level guidance through shallow features. Extensive experiments demonstrate that our method significantly outperforms existing state-of-the-art approaches on multiple public datasets.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：开放词汇语义分割（OVSS）旨在像素级分割图像中的可见和未见类别。现有方法利用CLIP等预训练视觉语言模型的文本嵌入，但面临两个关键挑战：
  1. **模态差距**：图像嵌入与文本嵌入之间存在固有的域间隙，即使经过大量对齐训练也难以完全弥合。
  2. **浅层特征缺失**：仅依赖深层文本对齐特征，缺乏浅层特征的引导，难以检测小物体和精细细节，导致分割精度下降。
- **整体含义**：本文通过引入**双提示（dual-prompt）**框架，融合视觉提示和文本提示，缓解模态差距，并利用多尺度特征提升分割精度，尤其是小物体和细节区域。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用视觉提示（由Stable Diffusion生成的与类别对应的合成图像）和文本提示共同构建成本体积（cost volume），并通过多尺度成本体积引导解码器逐步融合浅层特征，最后通过语义引导的提示细化策略进一步提升对齐质量。
- **关键技术细节**：
  - **双提示成本体积生成（Dual-Prompt Cost Volume Generation）**：
    - 图像编码器提取多尺度图像特征 $E_j$（j=2,3,4,5）。
    - 文本编码器生成文本提示嵌入 $T$（多种模板）。
    - Stable Diffusion生成视觉提示，经视觉提示编码器生成视觉嵌入 $V$ 及多尺度特征 $V_j$。
    - 将 $V$ 和 $T$ 平均得到联合表示 $R = \text{Avg}(V + T)$。
    - 计算余弦相似度生成成本体积 $\mathcal{F}_c(x,y,k,m) = \frac{E(x,y) \cdot R(k,m)}{\|E(x,y)\|\|R(k,m)\|}$。
  - **成本体积引导解码器（Cost Volume-Guided Decoder）**：
    - 采用HD Conv（混合空洞卷积）、MLP、自注意力层处理特征。
    - 在解码器每一阶段，将上采样后的成本体积与中间层图像特征 $E_j$ 及对应多尺度视觉特征 $V_j$ 计算视觉成本体积 $\mathcal{F}_v^j$，从而提供多尺度引导。
  - **语义引导的提示细化（Semantic-Guided Prompt Refinement）**：两阶段推理：
    - **Inference I**：使用文本提示和初始视觉提示得到初步分割结果。
    - **Inference II**：将检测到的类别对应的分割掩码裁剪出的对象用作新的视觉提示，替换该类别的视觉提示，再次输入模型得到更精细的分割。

## 3. 实验设计

- **训练数据集**：COCO-Stuff（171类，118k训练图像）。
- **测试数据集**：Pascal VOC（20类）、Pascal Context（PC-59和PC-459）、ADE20K-150（A-150）和ADE20K-847（A-847）。
- **基准**：与多种现有方法对比，包括ZS3Net、LSeg、OpenSeg、ZegFormer、OVSeg、CAT-Seg、SAN、EBSeg、SCAN、SED等。
- **对比方法**：涵盖二阶段方法和端到端方法，使用不同骨干（ResNet-101、ViT-B/16、ViT-L/14、ConvNeXt-B/L等）。
- **评估指标**：mIoU（平均交并比）。

## 4. 资源与算力

- **硬件**：两块NVIDIA V100 GPU。
- **训练时长**：80k迭代（iteration）。
- **批大小**：4。
- **实现框架**：PyTorch、Detectron2。
- **优化器**：AdamW，初始学习率2×10⁻⁴，权重衰减1×10⁻⁴。
- **贡献说明**：文中明确给出了训练算力信息。

## 5. 实验数量与充分性

- **主要实验结果**：在ConvNeXt-B和ConvNeXt-L两种配置下，均在5个测试集上报告mIoU（共10组主实验结果）。
- **消融实验**：
  - 提示策略对比：文本/视觉/双提示（1组）。
  - 成本体积生成方式对比（2组）。
  - 成本体积引导策略对比（4组）。
  - 模板数量影响（5组）。
  - 视觉提示质量影响（6组）。
- **充分性**：实验覆盖多种数据集、方法对比充分；消融实验覆盖关键组件（提示类型、融合方式、多尺度特征、模板数、视觉提示质量），有效验证了各设计的作用。实验公平（代码已开源，使用公开基准和标准协议）。

## 6. 论文的主要结论与发现

- 视觉提示比文本提示具有更强的特征对齐能力，能生成更精确的空间-语义成本体积。
- 双提示（文本+视觉）平均嵌入后再计算相似度，优于单独使用或分别计算再融合的方式。
- 多尺度视觉成本体积的渐进式集成有效保留了细节，提升了小物体分割。
- 语义引导的提示细化（Inference II）相比Inference I进一步提升约0.9% mIoU。
- 在五个数据集上，DPSeg均优于当前SOTA方法（如SED、CAT-Seg等），尤其在A-847和PC-459等难例数据集上提升显著。

## 7. 优点

- **创新性**：首次将视觉提示（Stable Diffusion生成）融入开放词汇分割，并通过双提示策略有效缓解模态差距。
- **技术完整性**：从理论分析（t-SNE、成本体积可视化）到系统设计（成本体积生成、多尺度引导、提示细化）形成闭环。
- **鲁棒性**：视觉提示质量变化（不同噪声水平、不同生成模型）对性能影响小，说明方法稳健。
- **可复现性**：代码和模型已开源，训练细节清晰。

## 8. 不足与局限

- **应用限制**：当前方法仅针对2D图像分割，未扩展到视频、3D点云或跨模态任务。
- **计算成本**：Stable Diffusion生成视觉提示增加了预处理开销，且两阶段推理增加推理时间。
- **视觉提示生成依赖**：依赖Stable Diffusion的生成质量，尽管实验显示鲁棒，但生成失败的情况未详细讨论。
- **实验覆盖**：未在更多小样本或极低资源场景下验证；也未与其他基于扩散模型的方法（如ODISE）在同等骨干下严格比较（ODISE使用不同训练集）。
- **消融设计**：缺少对语义引导细化（Inference II）单独失效情况的分析（例如当初始分割错误时，错误掩码会如何影响第二次推理）。

（完）
