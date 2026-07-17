---
title: "Mosaic3D: Foundation Dataset and Model for Open-Vocabulary 3D Segmentation"
title_zh: Mosaic3D：开放词汇三维分割的基础数据集与模型
authors: "Lee, Junha, Park, Chunghyun, Choe, Jaesung, Wang, Yu-Chiang Frank, Kautz, Jan, Cho, Minsu, Choy, Chris"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Lee_Mosaic3D_Foundation_Dataset_and_Model_for_Open-Vocabulary_3D_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 开放词汇三维分割的数据集与模型，含高质量3D掩码-文本对
tldr: 现有开放词汇三维分割数据集规模有限且标注质量参差不齐。本文提出自动生成流水线，利用图像分割模型和区域感知视觉语言模型产生高质量3D掩码-文本对，构建了包含5.6M对的大规模数据集Mosaic3D-5.6M。基于该数据集训练的Mosaic3D模型在多个基准上达到领先的开放词汇分割性能，推动了该领域的发展。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1807, \"height\": 475, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1815, \"height\": 553, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1783, \"height\": 392, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 879, \"height\": 453, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1630, \"height\": 575, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 828, \"height\": 424, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1802, \"height\": 375, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1802, \"height\": 418, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 681, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1796, \"height\": 353, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-lee-mosaic3d-foundation-dataset-and-model-for-open-vocabulary-3d-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 629, \"height\": 376, \"label\": \"Table\"}]"
motivation: 现有开放词汇三维分割数据集规模小且标注质量有限，亟需大规模高质量数据集。
method: 利用先进图像分割模型和区域感知视觉语言模型自动生成3D掩码-文本对，构建大规模数据集并训练分割模型。
result: Mosaic3D-5.6M数据集包含超过30K场景的5.6M掩码-文本对，模型在多个基准上表现优异。
conclusion: 提供大规模高质量数据集及模型，显著提升开放词汇三维分割性能。
---

## Abstract
We tackle open-vocabulary 3D scene segmentation tasks by introducing a novel data generation pipeline and training framework. Our work targets three essential aspects required for an effective dataset: precise 3D region segmentation, comprehensive textual descriptions, and sufficient dataset scale. By leveraging state-of-the-art open-vocabulary image segmentation models and region-aware vision-language models (VLM), we develop an automatic pipeline capable of producing high-quality 3D mask-text pairs. Applying this pipeline to multiple 3D scene datasets, we create Mosaic3D-5.6M, a dataset of more than 30K annotated scenes with 5.6M mask-text pairs - significantly larger than existing datasets. Building on these data, we propose Mosaic3D, a 3D visiual foundation model (3D-VFM) combining a 3D encoder trained with contrastive learning and a lightweight mask decoder for open-vocabulary 3D semantic and instance segmentation. Our approach achieves state-of-the-art results on open-vocabulary 3D semantic and instance segmentation benchmarks including ScanNet200, Matterport3D, and ScanNet++, with ablation studies validating the effectiveness of our large-scale training data. https://nvlabs.github.io/Mosaic3D/

---

## 论文详细总结（自动生成）

### 1. 检索相关性
开放词汇三维分割的数据集与模型，含高质量3D掩码-文本对。

### 2. 核心内容
现有开放词汇三维分割数据集规模有限且标注质量参差不齐。本文提出自动生成流水线，利用图像分割模型和区域感知视觉语言模型产生高质量3D掩码-文本对，构建了包含5.6M对的大规模数据集Mosaic3D-5.6M。基于该数据集训练的Mosaic3D模型在多个基准上达到领先的开放词汇分割性能，推动了该领域的发展。

### 3. 对应检索需求
open vocabulary 3d semantic segmentation。

### 4. 来源与原文
- Source：CVPR-2025-Accepted
- OpenReview：[https://openaccess.thecvf.com/content/CVPR2025/html/Lee_Mosaic3D_Foundation_Dataset_and_Model_for_Open-Vocabulary_3D_Segmentation_CVPR_2025_paper.html](https://openaccess.thecvf.com/content/CVPR2025/html/Lee_Mosaic3D_Foundation_Dataset_and_Model_for_Open-Vocabulary_3D_Segmentation_CVPR_2025_paper.html)
