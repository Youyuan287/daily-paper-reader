---
title: Vision-Only Gaussian Splatting for Collaborative Semantic Occupancy Prediction
title_zh: 基于纯视觉高斯泼溅的协作语义占用预测
authors: "Cheng Chen, Hao Huang, Saurabh Bagchi"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37269/41231"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 3D语义高斯泼溅用于协作占用预测
tldr: 针对现有协作感知方法依赖密集体素或2D平面特征导致高通信开销或深度估计需求的问题，提出首个基于稀疏3D语义高斯泼溅的协作语义占用预测方法。通过共享和融合中间高斯基元，降低了通信成本并提高了预测精度。尽管任务为占用预测，其高斯泼溅语义场的思路可借鉴于开放词汇场景。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有方法在协作场景中面临高通信成本或深度估计限制，需要更高效的3D语义表示。
method: 首次利用稀疏3D语义高斯泼溅，通过共享和融合中间高斯基元实现协作语义占用预测。
result: 在协作感知数据集上，该方法在降低通信量的同时保持了预测精度。
conclusion: 稀疏高斯泼溅为协作3D语义理解提供了高效表示框架。
---

## Abstract
Collaborative perception enables connected vehicles to share information, overcoming occlusions and extending the limited sensing range inherent in single-agent (non-collaborative) systems. Existing vision-only methods for 3D semantic occupancy prediction commonly rely on dense 3D voxels, which incur high communication costs, or 2D planar features, which require accurate depth estimation or additional supervision, limiting their applicability to collaborative scenarios. To address these challenges, we propose the first approach leveraging sparse 3D semantic Gaussian splatting for collaborative 3D semantic occupancy prediction. By sharing and fusing intermediate Gaussian primitives, our method provides three benefits: a neighborhood-based cross-agent fusion that removes duplicates and suppresses noisy or inconsistent Gaussians; a joint encoding of geometry and semantics in each primitive, which reduces reliance on depth supervision and allows simple rigid alignment; and sparse, object-centric messages that preserve structural information while reducing communication volume. Extensive experiments demonstrate that our approach outperforms single-agent perception and baseline collaborative methods by +8.42 and +3.28 points in mIoU, and +5.11 and +22.41 points in IoU, respectively. When further reducing the number of transmitted Gaussians, our method still achieves a +1.9 improvement in mIoU, using only 34.6% communication volume, highlighting robust performance under limited communication budgets.

---

## 论文详细总结（自动生成）

# 论文总结：Vision-Only Gaussian Splatting for Collaborative Semantic Occupancy Prediction

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：协作感知中，现有基于纯视觉的3D语义占用预测方法要么依赖密集3D体素（高通信开销），要么使用2D平面特征（如BEV、三平面），这通常需要精确的深度估计或额外监督，限制了其在协作场景中的应用。
- **背景**：单智能体感知受视野遮挡限制；多智能体协作可通过V2X通信整合多视角信息，但需兼顾精度与带宽。首个协作SOP方法CoHFF采用三平面特征，但依赖深度估计并采用多阶段训练，复杂且不利于跨智能体对齐。
- **意义**：提出首个基于稀疏3D语义高斯泼溅的协作感知框架，以紧凑、可解释的高斯基元作为通信媒介，实现高效、精准的3D语义占用预测。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：用一组3D高斯原语（每个包含位置、尺度、旋转、不透明度、语义）表示场景，通过智能体间共享和融合这些高斯原语替代传统稠密特征。
- **关键技术细节**：
  - **场景表示为3D高斯**：每个高斯定义形状和语义，通过高斯到体素的泼溅生成占用预测。
  - **高斯包装（Gaussian Packaging）**：利用已知外参，将对齐的高斯刚体变换到自车坐标系，并通过自车感兴趣区域（ROI）过滤，仅传输落在ROI内的高斯，减少通信量。
  - **跨智能体高斯融合**：采用轻量级邻域融合模块：对自车每个高斯，在半径ρ内搜索邻居高斯，构造成对特征（残差位置、尺度、旋转、不透明度、语义），通过MLP生成精细提案，再经均值池化或注意力池化聚合。更新规则采用混合方式，语义部分根据置信度加权融合。
- **形式化流程**：
  - 单智能体：图像编码 → 图像到高斯模块 → 高斯表示。
  - 协作：接收邻居高斯 → 刚性变换 + ROI滤波 → 堆叠 → 邻域融合 → 更新自车高斯 → 高斯-体素泼溅输出占用。

## 3. 实验设计
- **数据集**：Semantic-OPV2V（基于CARLA模拟器，OpenCDA框架），由CoHFF增强提供语义占用标签。
- **场景**：40m × 40m × 3.2m检测区域，体素分辨率0.4m³，共12个语义类+空类。
- **评价指标**：mIoU、IoU（3D语义占用），以及2D BEV分割的IoU（后续应用）。
- **对比方法**：
  - 单智能体基线（CoHFF单智能体、GaussianFormer单智能体）
  - 协作方法：CoHFF（三平面+深度估计），以及本文的三个变体：Zero-shot（直接堆叠，无训练）、Naive Fusion（端到端训练整个多智能体系统）、Learned Fusion（使用提出的邻域融合模块）。
- **其他实验**：通信量对比（高斯数量 vs 特征大小）、BEV分割对比（与CoBEVT、CoHFF）、消融实验（高斯数量、邻域半径ρ、池化方式）。

## 4. 资源与算力
- 训练在一台NVIDIA A100 (40GB)上进行，batch size=8，训练30个epoch，优化器AdamW，学习率策略为warmup+余弦退火。
- 文中未提及具体GPU数量、训练总时长或分布式设置；单卡训练足以完成所报告实验。

## 5. 实验数量与充分性
- **实验组数**：主表（Table 1）对比了6种方法的IoU和mIoU及12个类别细分；Table 2给出2种场景（2车和≤7车）下BEV分割结果；Table 3为通信量对比；Table 4为3因素消融（6种配置）；另有定性可视化（Figure 3）。
- **充分性评价**：
  - **优点**：基准覆盖全面（单智能体、现有协作方法SOTA CoHFF、本文多个变体）；消融设计合理（分析关键超参数和组件）；通信量分析实用。
  - **不足**：仅使用单一模拟数据集（Semantic-OPV2V），缺乏真实世界数据（如OPV2V本身无占用标签，需转换）；未在更大规模或更高难度数据集（如nuScenes Occ3D）上验证跨场景泛化性；对比方法仅包括CoHFF，未与其他基于体素的协作方法（如直接降采样体素后传输）比较；缺少对遮挡程度不同场景的细分分析；未提供统计显著性检验或重复实验的方差。

## 6. 主要结论与发现
- 本文提出的稀疏高斯泼溅表示作为协作媒介，在3D语义占用预测中显著优于现有基于平面特征的CoHFF方法（mIoU +3.28，IoU +22.41）。
- 即使是Zero-shot直接堆叠（无协同训练）也能提升单智能体性能，表明高斯表示的可迁移性。
- 学习式融合模块通过邻域聚合有效抑制冗余与噪声，尤其在稀疏类别（如桥梁）上带来提升。
- 在通信量方面，使用6400高斯即可达到优于CoHFF的性能，但传输量仅为CoHFF的34.6%，展示出高带宽效率。
- BEV分割下游任务表明，本文预测的3D占用高度更准确，投影后2D IoU优于对比方法。

## 7. 优点
- **表示创新**：首次将3D高斯泼溅用于协作感知，以稀疏、显式、刚性可对齐的原语替代传统稠密特征，兼具几何与语义信息。
- **低深度依赖**：高斯本身编码几何形状，无需额外深度监督。
- **通信高效**：通过ROI裁剪和稀疏表示，显著降低带宽占用。
- **融合设计轻量**：邻域融合模块参数少，可端到端训练，避免多阶段流水线。
- **灵活性**：支持Zero-shot部署，无需重新训练即可通过堆叠邻居高斯获得提升；实际部署可平衡精度与带宽。
- **实验覆盖合理**：主流指标、消融、定性分析齐全，验证了方法的有效性。

## 8. 不足与局限
- **数据集单一**：仅基于CARLA模拟的Semantic-OPV2V，缺乏真实世界（如感知噪声、通信延迟、定位误差）验证，模拟环境与真实环境差异可能影响结论泛化性。
- **高斯数量固定**：实验中默认使用25600个高斯，但场景复杂度不同时固定数量可能不是最优；无自适应调整机制。
- **邻域半径ρ为超参数**：对性能有一定影响，但消融显示较鲁棒；然而ρ固定为0.4m可能不适应尺度变化。
- **计算资源有限**：仅单卡A100，未提供推理速度或多卡训练扩展性分析；高斯泼溅模块可能引入额外计算开销。
- **缺乏与更多SOTA协作方法的对比**：如基于点云的协作方法（虽然本文是纯视觉），但文献中可能存在其他以平面或体素压缩的协作方案，未进行对比。
- **未评估鲁棒性**：未考虑通信丢包、异步、延迟等实际部署问题；未测试在不同车通信延迟或部分车辆故障时的性能下降。
- **局限性讨论不足**：论文对抗遮挡的解释主要依靠可视化，缺少定量遮挡指标（如遮挡比例与性能关系）；自由空间建模依赖单一固定高斯，可能限制细粒度空区域表示。

（完）
