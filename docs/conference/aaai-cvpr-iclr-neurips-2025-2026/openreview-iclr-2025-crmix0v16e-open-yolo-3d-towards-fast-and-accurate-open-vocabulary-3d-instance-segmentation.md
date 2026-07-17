---
title: "Open-YOLO 3D: Towards Fast and Accurate Open-Vocabulary 3D Instance Segmentation"
title_zh: Open-YOLO 3D：迈向快速准确的开放词汇3D实例分割
authors: "Mohamed El Amine Boudjoghra, Angela Dai, Jean Lahoud, Hisham Cholakkal, Rao Muhammad Anwer, Salman Khan, Fahad Shahbaz Khan"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=CRmiX0v16e"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 仅利用多视图2D目标检测实现快速开放词汇3D实例分割
tldr: 现有开放词汇三维实例分割依赖昂贵的2D基础模型（如SAM和CLIP），计算量大、速度慢。本文提出Open-YOLO 3D，仅利用多视图RGB图像的2D目标检测结果，高效实现开放词汇三维实例分割。通过轻量级检测避免了特征提取瓶颈。实验在保持高精度的同时显著提升推理速度，适用于实时应用场景。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法依赖昂贵2D模型导致速度慢，难以实际应用。
method: 仅利用多视图2D目标检测，结合轻量级融合进行3D实例分割。
result: 在速度和精度的平衡上取得最优，比现有方法快数倍。
conclusion: Open-YOLO 3D为实时开放词汇3D分割提供了实用解决方案。
---

## Abstract
Recent works on open-vocabulary 3D instance segmentation show strong promise but at the cost of slow inference speed and high computation requirements. This high computation cost is typically due to their heavy reliance on aggregated clip features from multi-view, which require computationally expensive 2D foundation models like Segment Anything (SAM) and CLIP. Consequently, this hampers their applicability in many real-world applications that require both fast and accurate predictions. To this end, we propose a novel open-vocabulary 3D instance segmentation approach, named Open-YOLO 3D, that efficiently leverages only 2D object detection from multi-view RGB images for open-vocabulary 3D instance segmentation. 
 We demonstrate that our proposed Multi-View Prompt Distribution (MVPDist) method makes use of multi-view information to account for misclassification from the object detector to predict a reliable label for 3D instance masks. Furthermore, since projections of 3D object instances are already contained within the 2D bounding boxes, we show that our proposed low granularity label maps, which require only a 2D object detector to construct, are sufficient and very fast to predict prompt IDs for 3D instance masks when used with our proposed MVPDist.
 We validate our Open-YOLO 3D on two benchmarks, ScanNet200 and Replica, 
 under two scenarios: (i) with ground truth masks, where labels are required for given object proposals, and (ii) with class-agnostic 3D proposals generated from a 3D proposal network.
 Our Open-YOLO 3D achieves state-of-the-art performance on both datasets while obtaining up to $\sim$16$\times$ speedup compared to the best existing method in literature. On ScanNet200 val. set, our Open-YOLO 3D achieves mean average precision (mAP) of 24.7% while operating at 22 seconds per scene. github.com/aminebdj/OpenYOLO3D

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究问题**：开放词汇3D实例分割（Open-Vocabulary 3D Instance Segmentation）现有方法依赖昂贵的2D基础模型（如Segment Anything SAM 和 CLIP）从多视图图像中聚合特征，导致推理速度极慢、计算成本高昂，严重限制了在实时应用场景中的可行性。
- **整体含义**：本文旨在实现**快速且准确**的开放词汇3D实例分割，目标是打破现有方法“精度高但速度慢”的瓶颈，使技术更贴近实际部署需求（如机器人、自动驾驶等）。

## 2. 论文提出的方法论

### 核心思想
- **仅利用多视图RGB图像的2D目标检测结果**来驱动开放词汇3D实例分割，完全避免使用SAM和CLIP等高开销模型。
- 提出**Multi-View Prompt Distribution（MVPDist）** 方法，利用多视图信息修正2D检测器的误分类，从而为3D实例掩码预测可靠的标签。
- 设计**低粒度标签图（Low Granularity Label Maps）**，仅需一个2D目标检测器即可快速构建，结合MVPDist高效地为3D实例掩码分配prompt ID。

### 关键技术细节
- 多视图2D检测 → 投影到3D空间 → 使用MVPDist融合多视图标签分布 → 得到每个3D实例的最终标签。
- MVPDist通过统计多个视角下检测标签的分布，克服单个视角误检问题（如遮挡、视角偏差）。
- 低粒度标签图无需像素级分割或特征嵌入，仅用检测框内的类别投票，极大降低计算量。

### 算法流程（文字说明）
1. 输入多视图RGB图像，使用轻量级2D目标检测器提取每张图中的2D边界框及类别置信度。
2. 对每个候选3D实例（可通过真实掩码或3D提案网络生成），将其投影到各视角图像上，统计落在其投影区域内的2D检测框标签。
3. 通过MVPDist整合所有视角下的标签分布，输出一个加权投票后的类别作为该3D实例的预测标签。
4. 若使用3D提案网络（class-agnostic），先利用简单3D网络生成实例掩码，再执行上述标签分配。

## 3. 实验设计
- **数据集**：
  - **ScanNet200**（200类室内场景）官方验证集
  - **Replica**（细粒度场景）
- **Benchmark设置**：
  - 场景一：使用**真实3D实例掩码**（仅评估标签分配能力）
  - 场景二：使用**类无关3D提案网络**生成的掩码（评估端到端性能）
- **对比方法**：与现有开放词汇3D实例分割方法（如OpenMask3D、OVIS等）比较。

## 4. 资源与算力
- **文中未明确说明**所使用的GPU型号、数量或训练时长。
- 仅提及推理速度为“每场景22秒”（ScanNet200），比现有最好方法快约16倍，但训练开销未报告。

## 5. 实验数量与充分性
- **实验数量**：在两个数据集（ScanNet200、Replica）上各进行两种场景实验，共至少4组主实验。此外还包含消融研究（如MVPDist的有效性、低粒度标签图对比高粒度特征等）。
- **充分性评价**：实验设计覆盖了标签分配和完整分割两条评估线，消融验证了各组件贡献。对比方法数量适中，但未对其他开源方法进行详尽复现比较（如仅引用原文数据）。总体较为充分，但缺少在更多样化场景（如室外）的验证。

## 6. 论文的主要结论与发现
- **主要结论**：Open-YOLO 3D 在**速度和精度的平衡上达到最优**，比现有方法快约16倍的同时，在ScanNet200上取得24.7% mAP（使用真实掩码场景），性能接近甚至超越依赖昂贵基础模型的方法。
- **发现**：低粒度标签图（仅需2D检测）足以支持开放词汇3D分割，MVPDist可有效修正检测噪声；多视图检测无需像素级特征即可实现可靠标签传播。

## 7. 优点（方法或实验设计亮点）
- **极快推理速度**：完全避开SAM/CLIP，实现实时应用潜力。
- **简洁有效**：仅需2D目标检测器，工程实现简单，易于部署。
- **MVPDist创新**：利用多视图信息提升鲁棒性，解决了单视图检测的不确定性。
- **实验场景覆盖两种setting**：既能评估标签分配能力，又能评估端到端分割质量，分析清楚。

## 8. 不足与局限
- **依赖2D检测质量**：若2D检测器在罕见类别上表现差，性能会明显下降。
- **未提供训练算力细节**：无法判断训练成本是否同样低（如模型是否轻量）。
- **仅在室内小数据集验证**：未在室外或大规模自动驾驶场景测试，泛化性存疑。
- **实验对比不够全面**：与SOTA方法的数值对比主要依赖原文引用，未自己复现其他方法。
- **未分析失败案例**：如MVPDist在极端遮挡下的局限性未讨论。

（完）
