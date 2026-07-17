---
title: "LangSplatV2: High-dimensional 3D Language Gaussian Splatting with 450+ FPS"
title_zh: LangSplatV2：高维3D语言高斯泼溅，450+ FPS
authors: "Wanhua Li, Yujie Zhao, Minghan Qin, Yang Liu, Yuanhao Cai, Chuang Gan, Hanspeter Pfister"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=XR5y4nvTfz"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 高维3D语言高斯泼溅用于实时开放词汇查询
tldr: 针对现有3D语言场方法LangSplat速度慢（8.2 FPS）的问题，本文提出LangSplatV2，通过高效的高维特征泼溅和查询优化，实现476 FPS的泼溅和384 FPS的开放词汇文本查询，显著提升了精度和实时性，为复杂场景中的语言交互应用铺平道路。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
motivation: LangSplat实时性差（8.2 FPS），限制了3D语言场在实际中的应用。
method: 优化高维特征泼溅和文本查询管道，实现460+ FPS性能。
result: 在速度和精度上均大幅超越LangSplat，查询速度提升47倍。
conclusion: 高效的高维高斯泼溅使实时3D开放词汇查询成为可能。
---

## Abstract
In this paper, we introduce LangSplatV2, which achieves high-dimensional feature
splatting at 476.2 FPS and 3D open-vocabulary text querying at 384.6 FPS for
high-resolution images, providing a 42 × speedup and a 47 × boost over LangSplat
respectively, along with improved query accuracy. LangSplat employs Gaussian
Splatting to embed 2D CLIP language features into 3D, significantly enhancing
speed and learning a precise 3D language field with SAM semantics. Such advancements in 3D language fields are crucial for applications that require language
interaction within complex scenes. However, LangSplat does not yet achieve real-
time performance (8.2 FPS), even with advanced A100 GPUs, severely limiting
its broader application. In this paper, we first conduct a detailed time analysis of
LangSplat, identifying the heavyweight decoder as the primary speed bottleneck.
Our solution, LangSplatV2 assumes that each Gaussian acts as a sparse code within
a global dictionary, leading to the learning of a 3D sparse coefficient field that
entirely eliminates the need for a heavyweight decoder. By leveraging this sparsity,
we further propose an efficient sparse coefficient splatting method with CUDA optimization, rendering high-dimensional feature maps at high quality while incurring
only the time cost of splatting an ultra-low-dimensional feature. Our experimental
results demonstrate that LangSplatV2 not only achieves better or competitive query
accuracy but is also significantly faster. Codes and demos are available at our project page: https://langsplat-v2.github.io.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）

- **现有瓶颈**：3D 语言场方法 LangSplat 虽然通过高斯泼溅嵌入 2D CLIP 特征并利用 SAM 语义实现精确的语言场学习，但在高端 A100 GPU 上仅达到 8.2 FPS，远未达到实时性能，严重限制了在复杂场景中语言交互的应用。
- **本文目标**：提出 LangSplatV2，通过高效的**高维特征泼溅**和**查询优化**，实现 476.2 FPS 的泼溅速度和 384.6 FPS 的开放词汇文本查询（速度分别提升 42 倍和 47 倍），同时保持或提升查询精度，为实时 3D 语言交互铺平道路。

### 2. 论文提出的方法论

- **核心思想**：将每个 Gaussian 视为全局字典中的**稀疏编码**，学习一个 3D 稀疏系数场，从而完全消除对 LangSplat 中重解码器（heavyweight decoder）的依赖，解决该解码器导致的性能瓶颈。
- **关键技术细节**：
  - 构建一个全局字典，每个高斯只需学习一个低维的稀疏系数，而非直接编码高维特征。
  - 利用稀疏性设计高效的高维特征泼溅方法：通过 **CUDA 优化**实现稀疏系数泼溅，使渲染高维特征图的时间代价仅相当于泼溅超低维特征。
  - 整体流程：输入图像 → 3D 高斯泼溅 → 稀疏系数场 → 高效泼溅 → 高维特征图 → 文本查询。
- **公式与算法**：文中未给出具体数学公式，主要依赖于**稀疏编码**和**CUDA 加速**的工程优化。

### 3. 实验设计

- **数据集与场景**：文中未明确提及具体数据集（如 ScanNet、Replica、L3D 等），仅提到在“高分辨率图像”上进行测试。
- **Benchmark**：主要以 LangSplat 为基准对比，未说明是否还有其他 SOTA 方法（如 LERF、3D-OVS 等）。
- **对比方法**：仅明确与 LangSplat 对比，欠缺与更多开放词汇方法的比较。
- **评价指标**：未提及定量指标（如 mIoU、Recall 等），仅宣称“better or competitive query accuracy”。

### 4. 资源与算力

- **GPU 型号与数量**：摘要中提及“even with advanced A100 GPUs”，推测至少使用 A100，但未说明具体数量或配置。
- **训练时长**：文中未提供训练时间或收敛轮数等信息。
- **备注**：实验资源配置细节缺失，无法判断复现成本。

### 5. 实验数量与充分性

- **实验数量**：由于仅提供摘要，无法得知具体实验组数（如不同场景、消融实验等）。未提及对稀疏编码维度、字典大小、CUDA 优化策略等的消融研究。
- **充分性与公平性**：缺乏公开的定量结果（表格/曲线）和多方法全面对比，因此现有信息下无法判断实验的客观性和公平性。需待完整论文补充。

### 6. 论文的主要结论与发现

- LangSplatV2 在**速度**上大幅超越 LangSplat（泼溅速度 42 倍提升，查询速度 47 倍提升），且**精度**相当或更优。
- 证明了**稀疏系数场**和**高效 CUDA 泼溅**可以有效消除重解码器瓶颈，实现实时 3D 开放词汇查询。

### 7. 优点

- **工程创新**：将高斯泼溅与稀疏编码结合，巧妙利用全局字典降低特征维度，同时引入 CUDA 级优化，使高维特征泼溅几乎不增加时间开销。
- **性能卓越**：在速度上实现了跨数量级的提升，有力推动了 3D 语言场走向实际应用。
- **思路清晰**：通过深入的时间分析定位重解码器为瓶颈，并针对性地提出解决方案，逻辑严密。

### 8. 不足与局限

- **实验覆盖不全**：未提供具体数据集、与更多 SOTA 方法的对比、定量指标（如准确率、召回率、mIoU），以及消融实验，导致结论可信度有限。
- **资源信息缺失**：GPU 型号、数量、训练时长等关键复现信息未给出，影响可复现性。
- **应用限制**：未讨论在遮挡、大尺度场景、多语言或细粒度语义下的鲁棒性；依赖 CLIP 和 SAM 的性能，可能受预训练模型偏差影响。
- **潜在偏差**：仅与 LangSplat 对比，可能刻意回避与更近期方法的比较（如 3D-OVS、OpenScene、StructionGS 等）。

（完）
