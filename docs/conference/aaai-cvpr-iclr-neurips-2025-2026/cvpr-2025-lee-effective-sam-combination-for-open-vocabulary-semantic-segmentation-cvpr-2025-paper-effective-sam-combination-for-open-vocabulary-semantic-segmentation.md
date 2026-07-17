---
title: Effective SAM Combination for Open-Vocabulary Semantic Segmentation
title_zh: 有效的SAM组合用于开放词汇语义分割
authors: "Lee, Minhyeok, Cho, Suhwan, Lee, Jungho, Yang, Sunghun, Choi, Heeseung, Kim, Ig-Jae, Lee, Sangyoun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Lee_Effective_SAM_Combination_for_Open-Vocabulary_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 通过伪提示将SAM解码器与CLIP结合的单阶段开放词汇分割
tldr: 两阶段开放词汇分割方法（SAM+CLIP）计算成本高。本文提出ESC-Net，将图像-文本相关性生成的伪提示嵌入SAM解码器，实现单阶段分割。通过利用SAM的提示分割框架和CLIP的文本对齐，在降低计算开销的同时保持了分割精度。实验在多个2D数据集上验证了效率与性能的双重优势。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1815, \"height\": 400, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 844, \"height\": 556, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1807, \"height\": 472, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 856, \"height\": 602, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 859, \"height\": 315, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1792, \"height\": 928, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1799, \"height\": 1040, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 871, \"height\": 159, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 874, \"height\": 267, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-effective-sam-combination-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 875, \"height\": 265, \"label\": \"Table\"}]"
motivation: 两阶段分割方法计算与存储开销大，难以实用。
method: 将图像-文本伪提示嵌入SAM解码器，构建单阶段分割网络。
result: 在保持高分割精度的同时，推理速度提升数倍。
conclusion: ESC-Net展示了高效融合SAM和CLIP的可行性，推动了开放词汇分割实用化。
---

## Abstract
Open-vocabulary semantic segmentation aims to assign pixel-level labels to images across an unlimited range of classes. Traditional methods address this by sequentially connecting a powerful mask proposal generator, such as the Segment Anything Model (SAM), with a pre-trained vision-language model like CLIP. But these two-stage approaches often suffer from high computational costs, memory inefficiencies. In this paper, we propose ESC-Net, a novel one-stage open-vocabulary segmentation model that leverages the SAM decoder blocks for class-agnostic segmentation within an efficient inference framework. By embedding pseudo prompts generated from image-text correlations into SAM's promptable segmentation framework, ESC-Net achieves refined spatial aggregation for accurate mask predictions. Additionally, a Vision-Language Fusion (VLF) module enhances the final mask prediction through image and text guidance. ESC-Net achieves superior performance on standard benchmarks, including ADE20K, PASCAL-VOC, and PASCAL-Context, outperforming prior methods in both efficiency and accuracy. Comprehensive ablation studies further demonstrate its robustness across challenging conditions.

---

## 论文详细总结（自动生成）

### 1. 检索相关性
通过伪提示将SAM解码器与CLIP结合的单阶段开放词汇分割。

### 2. 核心内容
两阶段开放词汇分割方法（SAM+CLIP）计算成本高。本文提出ESC-Net，将图像-文本相关性生成的伪提示嵌入SAM解码器，实现单阶段分割。通过利用SAM的提示分割框架和CLIP的文本对齐，在降低计算开销的同时保持了分割精度。实验在多个2D数据集上验证了效率与性能的双重优势。

### 3. 对应检索需求
open vocabulary 3d semantic segmentation。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Lee_Effective_SAM_Combination_for_Open-Vocabulary_Semantic_Segmentation_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Lee_Effective_SAM_Combination_for_Open-Vocabulary_Semantic_Segmentation_CVPR_2025_paper.html)
