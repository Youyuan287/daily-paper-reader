---
title: Rethinking End-to-End 2D to 3D Scene Segmentation in Gaussian Splatting
title_zh: 重新思考高斯泼溅中的端到端二维到三维场景分割
authors: "Zhu, Runsong, Qiu, Shi, Liu, Zhengzhe, Hui, Ka-Hei, Wu, Qianyi, Heng, Pheng-Ann, Fu, Chi-Wing"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhu_Rethinking_End-to-End_2D_to_3D_Scene_Segmentation_in_Gaussian_Splatting_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 端到端2D掩码到3D高斯分割，带对象感知码本
tldr: 现有端到端2D到3D分割方法依赖直接匹配或两阶段方案，效果不佳或复杂。本文提出Unified-Lift，基于对象感知3D高斯表示，利用对比学习学习高斯级特征，并引入可学习对象级码本，实现高质量3D场景分割。实验证明该方法在多个基准上达到领先性能，为高斯泼溅中的3D理解提供了新范式。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有2D到3D分割方法依赖直接匹配或复杂的两阶段处理，效果不佳。
method: 提出Unified-Lift，基于对象感知3D高斯表示，利用对比学习和高斯级特征，引入可学习对象级码本进行端到端提升。
result: 在多个3D场景分割基准上取得领先性能。
conclusion: 端到端对象感知提升方法有效提高了高斯泼溅中的3D分割质量。
---

## Abstract
Lifting multi-view 2D instance segmentation to a radiance field has proven effective to enhance 3D understanding. Existing works rely on direct matching for end-to-end lifting, yielding inferior results, or employ a two-stage solution constrained by complex pre- or post-processing. In this work, we design Unified-Lift, a new end-to-end object-aware lifting approach that aims for high-quality 3D segmentation based on our object-aware 3D Gaussian representation. To start, we augment each Gaussian point with a Gaussian-level feature learned using a contrastive loss to encode instance information. Importantly, we introduce a learnable object-level codebook to account for individual objects in the scene for an explicit object-level understanding and associate the encoded object-level features with the Gaussian-level point features for segmentation predictions. While promising, achieving effective codebook learning is nontrivial and a naive solution leads to degraded performance. Hence, we formulate the association learning module and the noisy label filtering module for effective and robust codebook learning. We conduct experiments on three benchmarks LERF-Masked, Replica, and Messy Rooms. Both qualitative and quantitative results manifest that our Unified-Lift clearly outperforms existing methods in terms of segmentation quality and time efficiency.

---

## 论文详细总结（自动生成）

# 论文《Rethinking End-to-End 2D to 3D Scene Segmentation in Gaussian Splatting》详细总结

## 1. 核心问题与整体含义（研究动机与背景）

- **问题**：将多视图2D实例分割提升到NeRF或3D高斯泼溅等辐射场中，可增强3D场景理解，但现有方法存在严重局限：
  - **端到端方法**（如Panoptic Lifting）依赖直接匹配，对匹配结果敏感，易产生多视图不一致分割。
  - **两阶段方法**（如Gaussian Grouping、Gaga）需要复杂的预处理（如视频跟踪、启发式匹配）或后处理（如HDBSCAN聚类），引入累积误差，且超参数敏感。
- **背景**：3D高斯泼溅（3D-GS）因高效渲染和高质量重建成为理想基底；2D基础模型（如SAM）可提供丰富的2D分割掩码，但缺乏多视图一致性。
- **目标**：设计一种**无需预/后处理的端到端**2D到3D场景分割方法，同时提升分割质量和效率。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 在3D-GS基础上，同时学习**高斯级特征**（对比学习优化）和**可学习的对象级码本**，显式建模场景中的每个物体，并通过对象-高斯关联实现端到端分割预测。

### 关键技术细节
1. **高斯级特征学习**  
   - 为每个3D高斯点\(g_i\)附加可学习特征\(f_i \in \mathbb{R}^d\)（\(d=16\)）。  
   - 使用可微分光栅化渲染特征图\(F_u\)（\(u\)为像素位置）。  
   - 采用InfoNCE对比损失（公式1）优化特征，使得同一实例的像素特征相似，不同实例的特征相异。

2. **对象级码本表示**  
   - 定义可学习码本矩阵\(F_{obj} \in \mathbb{R}^{L \times d}\)，其中\(L=256\)为最大物体数，每行代表一个潜在物体的特征。  
   - **对象-高斯关联**：对每个像素\(u\)，通过公式2计算属于各码本物体的概率分布\(P_u\)（基于点积相似度）。  
   - **推断**：选择概率最高的码本索引作为分割ID，无需聚类后处理。

3. **码本学习策略**（两个模块）  
   - **关联学习模块**  
     - 提出**面积感知ID映射**（公式5）：放弃归一化项，优先考虑大区域，提升跨视图一致性。  
     - 引入**集中约束**（公式6）：使码本特征与对应高斯级特征的方向最小化。  
     - 联合使用稀疏交叉熵损失（公式4）与集中约束，构成总关联损失。  
   - **噪声标签过滤模块**  
     - 利用高斯级特征计算每个像素的不确定性（公式7）：若当前像素特征与其对应实例中心特征的相似度低，则视为高不确定。  
     - 在总损失（公式8）中，仅使用不确定性低于阈值\(\tau=0.8\)的像素训练，过滤噪声2D掩码。

4. **总体优化**：所有参数（3D高斯参数、高斯级特征、码本）联合优化，迭代30k次，使用光度量损失（同3D-GS）和上述损失。

## 3. 实验设计

### 数据集与基准
| 数据集 | 场景数 | 特点 | 指标 |
|--------|--------|------|------|
| LERF-Mask | 3个真实场景（figures, ramen, teatime） | 每个场景6-10个物体标注 | mIoU, mBIoU |
| Replica | 8个室内场景 | 室内精细标注 | mIoU, F-score（IoU>0.5） |
| Messy Rooms | 2个环境（旧房间、大走廊），物体数25/50/100/500 | 最多500个物体，评估可扩展性 | PQscene（实例ID一致性） |

### 对比方法
- **预处理型**：Gaussian Grouping (ECCV'24)、Gaga (Arxiv'24)  
- **后处理型**：OmniSeg3D-GS (CVPR'24)  
- **端到端型**：Panoptic-Lifting-GS（自行实现）、Panoptic Lifting (NeRF)、Contrastive Lift (NeRF)  
- **开放词汇型**：LERF (ICCV'23)、LangSplat (CVPR'24)

### 对比公平性
- 所有3D-GS方法使用相同3D-GS骨干。  
- OmniSeg3D-GS使用HDBSCAN后处理并报告最佳超参数下的结果。  
- 方法代码和超参数设置均遵循原论文或官方实现。

## 4. 资源与算力

- **GPU**：单个NVIDIA 3090 RTX（24GB显存）。  
- **训练时长**：约1小时（30,000次迭代），如Table 3脚注所示。  
- **对比**：NeRF方法（如Contrastive Lift）需≥20小时，3D-GS方法均约1小时。  
- **未说明**：未提及训练时的具体批大小、数据加载细节，但基本实现基于3D-GS官方代码库。

## 5. 实验数量与充分性

- **主要对比实验**：在三个数据集上进行，覆盖了6-7种代表性方法，包括不同范式（预/后处理、端到端、开放词汇）。  
- **消融实验**（Table 4）：在Replica数据集上逐步验证各模块贡献：
  - 代码书基线 vs. 之前端到端基线（Panoptic-Lifting-GS）  
  - + 集中约束  
  - + 面积感知ID映射  
  - + 噪声标签过滤（完整方法）  
- **可扩展性实验**：在Messy Rooms上测试不同物体数量（25~500），与NeRF方法（Contrastive Lift）和3D-GS方法对比，显示优势。  
- **定性结果**：提供可视化比较（图3、6）和不确定度图（图5），直观展示提升。  
- **应用展示**：复制粘贴编辑、多尺度物体选择（图6c）。  
- **评估**：实验设计全面，对比方法类型丰富，消融清晰，指标选择合理。但仅覆盖了三个公开数据集，未见跨域泛化测试。

## 6. 主要结论与发现

- Unified-Lift在**所有数据集和指标上均显著优于现有方法**，尤其在小物体分割和减少伪影方面表现突出。  
- 实现了**真正的端到端**：无需预/后处理，推理时直接渲染得到分割图。  
- 在**超多物体场景**（500个）下依然保持竞争力，且训练时间远低于NeRF方法。  
- 噪声标签过滤模块有效抑制了2D掩码不一致带来的错误。

## 7. 优点

1. **方法创新**  
   - 引入可学习对象级码本是核心亮点，将隐式逐高斯特征显式转化为物体概念。  
   - 面积感知ID映射和集中约束增强了跨视图一致性和码本质量。  
   - 自监督不确定度过滤有效提升鲁棒性，无需额外标注。

2. **效率优势**  
   - 基于3D-GS，训练仅需约1小时，远快于NeRF方法（≥20小时）。  
   - 推理时无需后处理，直接快速渲染分割图。

3. **实验充分**  
   - 在多个难度递增的数据集上验证，对比方法覆盖主流方案，消融实验严谨。  
   - 展示了分割质量提升带来的下游应用效果（编辑、多尺度选择）。

## 8. 不足与局限

1. **实验覆盖范围有限**  
   - 仅使用三个室内/桌面场景数据集，未在室外大场景、动态场景或复杂遮挡场景中测试。  
   - 未与最新NeRF端到端方法（如Contrastive Lift）在相同3D-GS骨干下比较（原文对比的NeRF方法基于不同骨干）。

2. **依赖2D掩码质量**  
   - 初始2D掩码由SAM生成，若SAM在特定域（如医学、遥感）表现不佳，可能影响结果。  
   - 噪声过滤模块依赖高斯级特征学习效果，初期可能不稳定。

3. **参数敏感性**  
   - 码本物体数L=256需预先设定，若场景物体数远少于或远超256，可能影响性能（虽然500物体实验显示仍有效，但未测试更大容量）。  
   - 阈值为0.8为固定值，缺乏自适应机制。

4. **方法复杂性**  
   - 同时训练高斯级特征和码本，引入了额外模块（关联学习、噪声过滤），调参需一定经验。

5. **泛化性论证不足**  
   - 未在不依赖SAM的其他分割模型（如Mask2Former）下测试，鲁棒性验证不完备。

（完）
