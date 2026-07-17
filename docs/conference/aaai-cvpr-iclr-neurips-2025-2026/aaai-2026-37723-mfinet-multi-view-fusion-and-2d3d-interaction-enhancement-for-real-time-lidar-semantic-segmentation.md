---
title: "MFINet: Multi-view Fusion and 2D–3D Interaction Enhancement for Real-Time LiDAR Semantic Segmentation"
title_zh: "MFINet: 实时LiDAR语义分割的多视图融合与2D-3D交互增强"
authors: "Nan Ma, Zhijie Liu, Yiheng Han"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37723/41685"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 多视图融合与2D-3D交互用于LiDAR语义分割
tldr: 投影法LiDAR分割存在3D信息丢失和后处理耗时问题。本文提出MFINet，采用三分支结构融合3D点视图、2D俯视图和2D距离图，并设计3D点特征投影器将3D特征注入2D视图，保留空间细节。在多个数据集上实现实时性与高精度的最佳平衡。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 投影法LiDAR分割丢失3D信息且后处理耗时。
method: 提出三分支多视图融合架构和3D点特征投影器，增强2D-3D交互。
result: 在LiDAR语义分割基准上达到实时且高精度。
conclusion: 多视图融合与3D特征保持有效提升LiDAR分割性能。
---

## Abstract
LiDAR semantic segmentation is a key task in advanced autonomous driving systems. Projection-based methods exhibit real-time potential due to their efficiency, but suffer from inevitable 3D information loss and rely on time-consuming post-processing, limiting overall performance. To address this, we propose MFINet, a real-time semantic segmentation network based on multi-view fusion and 2D-3D interaction enhancement. It adopts a three-branch architecture that integrates 3D Point View (3D-PV), 2D Bird’s Eye View (2D-BEV) and 2D Range View (2D-RV) to make full use of 2D and 3D representation. From 3D to 2D, we design a 3D Point Feature Projector (3DPFP), which injects 3D features into the 2D BEV and RV pseudo-images to retain effective 3D information. From 2D to 3D, a Feature Enhancement (FE) module is designed to leverage the advantages of 2D information in extracting geometric and semantic features. We also introduce a 2D-3D Fusion Head (FH) to aggregate point features from multiple views. Besides, we incorporate a Multi-Scale Dilated Attention (MSDA) module with a sliding window strategy to enhance feature discrimination. Extensive experiments on the SemanticKITTI and NuScenes benchmarks demonstrate that MFINet outperforms existing methods on the SemanticKITTI, NuScenes val set and achieves competitive results on the NuScenes test set.

---

## 论文详细总结（自动生成）

# MFINet: 实时LiDAR语义分割的多视图融合与2D-3D交互增强 - 详细总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：LiDAR点云语义分割是自动驾驶环境感知的关键任务。投影类方法（将3D点云投影到2D伪图像）因其高效推理潜力受到关注，但投影过程不可避免地造成3D信息丢失（如几何结构、垂直方向信息），且往往依赖耗时的后处理来恢复点级预测，限制了整体性能和实时性。
- **背景**：现有方法可分为点基、体素基和投影基。点基方法计算成本高；体素基需权衡精度与内存；投影基方法速度快但信息损失严重。尤其2D Range View (RV) 损失空间结构，2D Bird's Eye View (BEV) 丢失垂直特征。此外，LiDAR数据稀疏导致伪图像中存在大量空像素，引入噪声。
- **整体含义**：论文旨在通过多视图融合与2D-3D交互增强，在保持实时性的同时最大程度保留3D信息，达到精度与速度的更好平衡。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 采用**三分支架构**：3D Point View (3D-PV)、2D Bird's Eye View (2D-BEV) 和 2D Range View (2D-RV)，充分利用不同视图的互补特性。
- **双向信息流动**：
    - **3D → 2D**：设计3D Point Feature Projector (3DPFP)，在投影前提取3D特征，通过池化和投影注入2D伪图像，增强几何保真度。
    - **2D → 3D**：设计Feature Enhancement (FE) 模块，在解码阶段利用2D信息提取几何和语义特征，并通过多尺度边界监督强化细节。
- **端到端实时预测**：通过2D-3D Fusion Head (FH) 聚合多视图、多层次点特征，直接输出点级语义，避免后处理。

### 关键技术细节
1. **2D投影**：
   - RV：球面投影，根据方位角θ和仰角φ映射到 (u,v)。
   - BEV：平面投影，将(x,y)映射到 (u,v)。
2. **3DPFP**：
   - 对每个3D点用MLP提取特征，对投影到同一像素位置的点特征进行max pooling，再投影到RV和BEV平面，生成增强的伪图像。
3. **Multi-Scale Dilated Attention (MSDA)**：
   - 在2D分支的浅层编码器中引入，使用不同扩张率（r=1,2,3）的滑动窗口注意力，在稀疏特征图上高效建模多尺度上下文，平衡感受野与计算量。
4. **Feature Enhancement (FE)**（训练阶段激活）：
   - 对多尺度解码特征图上采样后，分别计算2D分割损失、边界细节损失（Laplacian提取+二值化）和点级预测损失，联合优化（权重λ1=0.4, λ2=0.2, λ3=0.4）。
5. **2D-3D Fusion Head (FH)**：
   - 将2D分支多层级解码特征反投影到3D空间并拼接，再与3DPFP提取的3D特征融合，用线性头输出最终语义分数。
6. **总体损失**：主损失（交叉熵）+ 辅助损失（FE损失）。

## 3. 实验设计

- **数据集与基准**：
  - **SemanticKITTI**：包含43k+扫描，360°覆盖，19类语义。序列00-10训练（08验证），11-21测试。
  - **NuScenes**：1000场景，28,130训练样本，6,019验证样本，6,008测试样本。16类语义。
- **对比方法**：涵盖了主流投影基、体素基、点基方法，如RangeNet++、SalsaNext、Cylinder3D、SphereFormer、RangeFormer、PC-BEV、WaffleIron、GFNet、AMVNet、PVKD等。同时也与多模态方法（LCPs、EPMF、2DPASS）进行了比较（仅对比mIoU和计算量）。
- **评估指标**：mIoU（平均交并比）、FPS（帧率）、参数量、FLOPs。

## 4. 资源与算力

- 训练使用**单块NVIDIA RTX 4090 GPU**；为公平比较，测试推理速度时使用**单块NVIDIA RTX 2080Ti GPU**。
- 论文未明确说明训练时长或训练迭代轮数，也未提及多卡并行细节。
- 在Jetson TX2上测试了FPS：LSK3DNet 1.2 FPS，LSK3DNet-S 3.8 FPS，MFINet 5.1 FPS。

## 5. 实验数量与充分性

- **实验充分性**：
  - 在 **三个不同的测试集** (SemanticKITTI val, NuScenes val, NuScenes test) 上进行了全量对比，覆盖室内外场景。
  - 与 **15+种** 现有一流方法在表格中逐一对比（表1-3），结果清晰。
  - 进行了**详细的消融实验**：
    - 三分支组合消融（表5）：3D-PV+2D-RV vs. 3D-PV+2D-BEV vs. 全三分支。
    - 各核心组件消融（表6）：分别去除3DPFP、MSDA、FE、FH，以及去除单一组件保留其他的组合。
  - 同时报告了参数量、FLOPs、FPS，与多种方法对比（表4）。
- **客观性与公平性**：实验设置（分辨率、优化器、学习率策略）描述清晰；与同类方法在相同硬件上测试FPS（2080Ti）；代码已开源。

## 6. 主要结论与发现

- MFINet在**SemanticKITTI val**上达到68.1% mIoU、28.1 FPS，显著优于所有对比方法（如PC-BEV 66.4% mIoU）。
- 在**NuScenes val**上以80.3% mIoU超越所有LiDAR-only方法（如SphereFormer 79.5%、SFPNet 80.1%）。
- 在**NuScenes test**上达到79.7% mIoU，仅次于RangeFormer（80.1%），但在参数量（10.1M）和FLOPs（151.9G）上远小于后者。
- 相比多模态方法，MFINet仅用LiDAR即可获得可比的mIoU，且参数量、FLOPs更低。
- 消融实验证实每个组件均有效提升精度：3DPFP (+0.6%), MSDA (+0.9%), FE (+1.5%), FH (+1.6%)。

## 7. 优点

- **创新性**：首次将3D点特征投影与2D多视图交互增强系统结合，在投影类方法中有效保留了3D几何信息。
- **实时性与轻量化**：采用轻量3D分支和高效2D架构，在RTX 2080Ti上达28.1 FPS，参数仅10.1M，FLOPs 151.9G，适合嵌入式部署（Jetson TX2上5.1 FPS）。
- **端到端训练**：无需后处理，Fusion Head直接输出点级预测，简化流程。
- **模块化设计**：各组件可独立验证，且相互增益（如去除3DPFP后MSDA效果下降，表明它们协同工作）。
- **实验全面性**：覆盖两大主流数据集，与多种投影/体素/点基方法对比，并包含计算开销比较，结果公开透明。

## 8. 不足与局限

- **投影方法固有局限**：尽管通过3DPFP缓解了信息丢失，但投影操作本身仍存在多对一映射和分辨率限制，对于细小物体（如行人、自行车）的边界可能仍有误差（从类别IoU看，bicycle/motorcycle等类别精度相对较低）。
- **实时性定义**：28.1 FPS在RTX 2080Ti上达到，但在更低算力平台（如Jetson TX2）仅5.1 FPS，可能不足以满足严格实时要求（30 FPS）。论文未测试在更主流车载GPU上的性能。
- **数据集多样性**：仅在SemanticKITTI和NuScenes上验证，缺乏在Waymo或户外大规模点云数据集上的评估；且均为城市驾驶场景，未涉及非结构化环境。
- **与多模态方法比较略简**：仅对比了三种多模态方法（LCPs、EPMF、2DPASS），且未在同一实验设置下进行公平比较（例如是否使用相同的预处理、是否在相同划分下训练），表格中标注了模态差异，但未深入分析多模态融合的优缺点。
- **训练细节不详**：未报告具体训练时长、学习率衰减曲线、数据增强策略等，可复现性略受影响（尽管提供代码）。
- **泛化性风险**：模型在NuScenes test上性能略低于RangeFormer（0.4%），说明在特定场景下仍有提升空间。且对于语义种别的长尾分布（如motorcyclist在KITTI中为0% IoU）未做专门处理。

（完）
