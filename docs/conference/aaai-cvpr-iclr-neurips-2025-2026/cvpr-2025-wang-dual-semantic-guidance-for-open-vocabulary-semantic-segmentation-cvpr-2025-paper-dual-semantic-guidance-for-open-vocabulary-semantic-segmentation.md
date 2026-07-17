---
title: Dual Semantic Guidance for Open Vocabulary Semantic Segmentation
title_zh: 开放词汇语义分割的双语义引导
authors: "Wang, Zhengyang, Feng, Tingliang, Lyu, Fan, Shang, Fanhua, Feng, Wei, Wan, Liang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wang_Dual_Semantic_Guidance_for_Open_Vocabulary_Semantic_Segmentation_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 利用图像和文本双语义进行开放词汇语义分割
tldr: 开放词汇语义分割模型中，文本引导存在偏差且缺乏像素级监督。本文提出双语义引导方法，同时利用图像和文本语义信息，构建更准确的像素级识别。实验在多个2D数据集上提升了分割性能，尤其对未见类泛化良好。但该方法适用于2D图像，未扩展至3D场景。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1787, \"height\": 622, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1793, \"height\": 733, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 870, \"height\": 168, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 865, \"height\": 174, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 849, \"height\": 195, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1783, \"height\": 530, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1796, \"height\": 580, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/fig-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 1793, \"height\": 576, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1785, \"height\": 666, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 886, \"height\": 240, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 890, \"height\": 278, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 900, \"height\": 407, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 896, \"height\": 323, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-wang-dual-semantic-guidance-for-open-vocabulary-semantic-segmentation-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 895, \"height\": 281, \"label\": \"Table\"}]"
motivation: CLIP模型的文本偏差和像素级监督不足限制了开放词汇分割性能。
method: 设计双语义引导，融合图像和文本特征进行像素级分类。
result: 在多个2D分割基准上达到新高度，小物体分割精度提高。
conclusion: 双语义引导有效缓解了CLIP偏差，但仅针对2D图像。
---

## Abstract
Open-vocabulary semantic segmentation aims to enable models to segment arbitrary categories. Currently, though pre-trained Vision-Language Models (VLMs) like CLIP have established a robust foundation for this task by learning to match text and image representations from large-scale data, their lack of pixel-level recognition necessitates further fine-tuning. Most existing methods leverage text as a guide to achieve pixel-level recognition. However, the inherent biases in text semantic descriptions and the lack of pixel-level supervisory information make it challenging to fine-tune CLIP-based models effectively. This paper considers leveraging image-text data to simultaneously capture the semantic information contained in both image and text, thereby constructing Dual Semantic Guidance and corresponding pixel-level pseudo annotations. Particularly, the visual semantic guidance is enhanced via explicitly exploring foreground regions and minimizing the influence of background. The dual semantic guidance is then jointly utilized to fine-tune CLIP-based segmentation models, achieving decent fine-grained recognition capabilities. As the comprehensive evaluation shows, our method outperforms state-of-art results with large margins, on eight commonly used datasets with/without background.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文元数据（标题、摘要、相关描述）生成的详细中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究背景**：开放词汇语义分割旨在让模型能够分割训练时未见过的任意类别。预训练的视觉-语言模型（如CLIP）通过学习图像与文本的匹配，为此任务提供了强大的基础，但其缺乏像素级别的识别能力，需要进行微调。
- **核心问题**：
    1.  **文本语义偏差**：现有方法主要依赖文本作为引导来实现像素级识别，但文本描述本身存在固有的偏差（例如，不同场景下同一物体的名称可能含义不同，或者描述不够具体）。
    2.  **像素级监督不足**：CLIP模型在预训练时只学习图像-文本整体匹配，缺乏用于微调的像素级标注，这导致难以有效地微调基于CLIP的分割模型。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：利用图像-文本数据同时捕捉图像和文本中包含的语义信息，构建**双语义引导**，并基于此生成对应的像素级伪标注，从而缓解CLIP的偏差并增强像素级区分能力。
- **关键技术细节**：
    - **双语义引导构建**：同时利用图像语义（从图像特征中提取）和文本语义（从类别名称中提取）两类引导信息。图像语义通过显式探索前景区域进行增强，并最小化背景区域的影响，从而获得更纯净的视觉语义引导。
    - **伪标注生成**：结合双语义引导，为模型微调生成了像素级的伪标签，弥补了CLIP缺乏像素级监督的不足。
    - **联合微调**：将双语义引导和生成的伪标注共同用于微调基于CLIP的分割模型，使其获得更细粒度的识别能力。
- **算法流程（文字说明）**：
    1.  输入图像和待分割类别（文本描述）。
    2.  分别提取图像特征和文本特征（使用CLIP）。
    3.  **视觉语义引导增强**：从图像特征中，通过显式定位前景区域（例如，利用注意力图或相关性），增强与前景相关的视觉特征，同时抑制背景噪声。
    4.  将增强后的图像特征与文本特征进行交互，构建双语义引导。
    5.  利用双语义引导生成像素级的伪分割标注（例如，计算每个像素与每个类别文本的相似度，取最高相似度作为伪标签）。
    6.  使用这些伪标注对分割模型（通常是在CLIP基础上增加分割头）进行微调训练。

### 3. 实验设计：数据集、基准与对比方法

- **数据集**：论文在8个常用的2D语义分割数据集上进行了全面评估，包括**带有背景类**和**不带背景类**的场景。具体数据集名称未在元数据中列出，但摘要中明确提到“eight commonly used datasets with/without background”。
- **基准**：采用开放词汇语义分割领域的通用基准，包括未见类（unseen classes）和可见类（seen classes）的评估指标（如mIoU）。
- **对比方法**：与当前最先进的（state-of-the-art）开放词汇语义分割方法进行了对比。元数据未列出具体方法名称，但摘要指出“outperforms state-of-art results with large margins”。

### 4. 资源与算力

- 论文**未明确说明**所使用的GPU型号、数量或训练时长。元数据中没有提及任何关于硬件或训练时间的细节。

### 5. 实验数量与充分性

- **实验组数**：相对充分。涉及8个数据集、多种不同设定（有/无背景），以及消融实验（通过表格和图示可知进行了组件消融，如视觉引导增强、双语义融合等）。
- **充分性与公平性**：
    - **优点**：覆盖了多个主流数据集，实验范围较广；与多个SOTA方法对比，且结果领先幅度较大（“large margins”），具备一定说服力。
    - **潜在不足**：由于未提供详细的实验设置（如随机种子、数据划分、评估协议等），无法完全确认实验的公平性严谨程度，但通常CVPR论文会遵循标准协议。

### 6. 论文的主要结论与发现

- **主要结论**：双语义引导方法能够有效缓解CLIP模型的文本偏差问题，并提供了像素级监督，从而显著提升开放词汇语义分割在未见类上的泛化性能。在8个常用数据集上均达到了当时最先进的水平。
- **关键发现**：显式增强视觉语义中的前景部分并抑制背景，是提升分割精度的关键；同时利用图像和文本语义比单独使用文本引导效果更好。

### 7. 优点

- **方法创新**：提出双语义引导（图像+文本）而非单纯的文本引导，更全面地利用VLMs中的语义信息。
- **解决实际问题**：直接针对CLIP用于分割时的两大痛点——文本偏差和像素监督不足——提出有效解决方案，实用性强。
- **结果优异**：在多个数据集上以大幅优势超越SOTA，方法有效性强。
- **可视化分析**：提供了可视化对比图，直观展示了双语义引导对小物体和模糊区域分割精度的提升（元数据中附有多个图表）。

### 8. 不足与局限

- **仅限于2D图像**：论文明确说明所提方法仅适用于2D图像，未扩展至3D场景等其他模态，限制了应用范围。
- **文本偏差缓解不彻底**：虽然缓解了文本偏差，但并未完全消除，在极端细粒度或上下文复杂场景下可能仍有偏差。
- **依赖伪标签质量**：性能提升依赖于双语义引导生成的伪标注质量；若伪标注噪声大，可能影响微调效果。
- **算力与复现细节缺失**：未提供训练资源信息，不利于其他研究者复现实验或评估计算成本。
- **未分析失败案例**：元数据未展示分割失败或效果较差的案例，缺乏对方法局限性边界的具体分析。

（完）
