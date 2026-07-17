---
title: "VoxelSplat: Dynamic Gaussian Splatting as an Effective Loss for Occupancy and Flow Prediction"
title_zh: VoxelSplat：动态高斯泼溅作为占用和流预测的有效损失
authors: "Zhu, Ziyue, Wang, Shenlong, Xie, Jin, Liu, Jiang-jiang, Wang, Jingdong, Yang, Jian"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Zhu_VoxelSplat_Dynamic_Gaussian_Splatting_as_an_Effective_Loss_for_Occupancy_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 语义3D高斯投影到2D用于占用和流监督
tldr: 针对基于相机的占用预测中遮挡和动态环境挑战，本文提出VoxelSplat，利用3D高斯泼溅将稀疏语义高斯投影到2D视图提供额外监督，同时利用动态高斯建模场景流，显著提升语义占用和流预测精度。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 784, \"height\": 656, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1743, \"height\": 741, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1828, \"height\": 537, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1814, \"height\": 676, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 836, \"height\": 678, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 837, \"height\": 801, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1503, \"height\": 436, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1804, \"height\": 614, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 836, \"height\": 334, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-zhu-voxelsplat-dynamic-gaussian-splatting-as-an-effective-loss-for-occupancy-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1613, \"height\": 395, \"label\": \"Table\"}]"
motivation: 相机占用预测受遮挡和动态场景影响，需要更好的正则化。
method: 将语义3D高斯投影到2D视图作为额外监督，并利用动态高斯建模场景流。
result: 在占用和流预测任务上达到SOTA，验证了高斯投影的有效性。
conclusion: 3D高斯泼溅可作为有效的正则化手段提升3D感知任务性能。
---

## Abstract
Recent advancements in camera-based occupancy prediction have focused on the simultaneous prediction of 3D semantics and scene flow, a task that presents significant challenges due to specific difficulties, e.g., occlusions and unbalanced dynamic environments. In this paper, we analyze these challenges and their underlying causes. To address them, we propose a novel regularization framework called VoxelSplat. This framework leverages recent developments in 3D Gaussian Splatting to enhance model performance in two key ways: (i)Enhanced Semantics Supervision through 2D Projection: During training, our method decodes sparse semantic 3D Gaussians from 3D representations and projects them onto the 2D camera view. This provides additional supervision signals in the camera-visible space, allowing 2D labels to improve the learning of 3D semantics. (ii) Scene Flow Learning: Our framework uses the predicted scene flow to model the motion of Gaussians, and is thus able to learn the scene flow of moving objects in a self-supervised manner using the labels of adjacent frames. Our method can be seamlessly integrated into various existing occupancy models, enhancingperformance without increasing inference time. Extensive experiments on benchmark datasets demonstrate the effectiveness of VoxelSplat in improving the accuracy of both semantic occupancy and scene flow estimation. The project page and codes are available at https://zzy816.github.io/VoxelSplat-Demo/.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究领域**：基于相机的3D占用预测（occupancy prediction）及其与场景流（scene flow）的联合预测，是自动驾驶感知中的关键任务。
- **面临挑战**：
  - **遮挡问题**：标注区域中有很大部分被遮挡，相机不可见，直接利用这些区域的3D标签进行监督可能传递误导信号，反而损害模型性能。
  - **动态建模困难**：体素化表示天然适合欧拉运动（implicit Eulerian），但安全规划更需要显式的拉格朗日运动（explicit Lagrangian）建模。
  - **类别与速度不平衡**：数据集（如nuScenes）中高速运动物体样本稀少，导致模型对罕见动态事件鲁棒性不足。
- **研究动机**：为解决上述困难，本文探索利用最近发展的3D高斯泼溅（3D Gaussian Splatting）技术作为训练阶段的额外正则化损失，以提升语义占用和场景流预测的准确性，且不增加推理成本。

## 2. 方法论：核心思想、关键技术细节
### 核心思想
- VoxelSplat是一个即插即用的训练框架，在现有占用预测网络基础上额外添加一个**动态高斯泼溅分支**，通过将3D表示解码为稀疏语义高斯，并投影到2D相机视图获得渲染损失，从而提供来自2D标签的额外监督，同时借助高斯中心的位置更新实现场景流的自监督学习。
- **关键创新**：
  1. **增强语义监督**：从体素特征中解码3D语义高斯，渲染到2D视图，与2D真值计算交叉熵和L1损失，使3D模型从可见空间得到额外信号。
  2. **场景流学习**：利用预测的场景流更新高斯中心位置，得到下一帧的高斯，再渲染到相邻帧视图，通过2D标签自监督学习运动。
  3. **加权点采样**：基于类别和速度将体素划分为多个组，采用概率函数 \( P(x) = 1/(x^t + 1) \) 进行采样，以缓解类别和速度不平衡。

### 技术细节（文字说明）
1. **基础体素网络**：使用现有占用网络（如FB-Occ、BEVDet-Occ等）输入多帧多视图图像，输出体素特征 \( V \)、3D语义 \( S \) 和场景流 \( F \)。
2. **加权点采样**：
   - 通过射线投射工具获得相机可见区域掩码。
   - 从可见区域内的占据体素中，按语义类别和速度共 \( P \times Q \) 类，计算每类采样概率 \( p_{p,q} \propto 1/(N_{p,q}^t + 1) \)，平衡样本数量差异。
   - 采样得到 \( N \) 个3D坐标 \( \{\mu_n\} \)，并从体素特征中查询对应嵌入 \( v_n \)、语义 \( s_n \)、场景流 \( \Delta x_n \)。
3. **动态高斯解码**：
   - 直接使用 \( \mu_n \)、\( s_n \)、\( \Delta x_n \) 作为高斯中心、语义logits和运动向量。
   - 通过一个两层MLP（Gaussian Decoder）解码嵌入 \( v_n \) 与可学习位置编码 \( pe \) 的和，得到高斯形状属性：不透明度 \( \alpha \)、旋转 \( r \)、缩放 \( sc \)。
   - 得到完整的高斯集合 \( \{g_n\} \)。
4. **动静分解与更新**：
   - 根据语义真值将高斯分为静态 \( G_s \) 和动态 \( G_d \)。
   - 对动态高斯，用场景流更新中心：\( \mu' = \mu + \Delta x \)，得到未来帧动态高斯 \( G_{df} \)。
5. **可微分泼溅渲染**：
   - 使用3D高斯泼溅的快速可微分栅格化，分别渲染静态高斯 \( G_s \) 和动态高斯组合 \( G_d \cup G_{df} \)，得到语义图和深度图。
   - 对静态和动态分别计算损失，合并为2D损失 \( L_{2D} \)。
6. **损失函数**：
   - 3D损失 \( L_{3D} \)：原始占用损失（如交叉熵）+ 场景流L1损失（按速度加权）。
   - 2D损失 \( L_{2D} \)：渲染的语义图与2D真值的交叉熵 + 渲染深度与2D真值的L1损失。
   - 总损失 \( L_{total} = L_{3D} + L_{2D} \)。
7. **推理时**：仅保留原体素网络分支，不包含高斯渲染过程，因此无额外计算开销。

## 3. 实验设计
- **数据集与标注**：
  - **nuScenes**：1000个视频序列（700/150/150分割）。使用OpenOcc [24,36] 提供的占用和场景流真值（\( H \times W \times D \) 网格，2D场景流），以及Occ3D [40] 和 SurroundOcc [46] 的语义标注。
- **基准（Benchmark）**：OpenOcc（CVPR 2024 workshop挑战）和 Occ3D。
- **对比方法**：
  - 占用预测：RenderOcc、BEVFormer、BEVDet-Occ、FB-Occ、SparseOcc、OccFormer、SurroundOcc、GaussianFormer、TPVFormer等。
  - 基于三个基础模型集成VoxelSplat：BEVDet-Occ、FB-Occ、SparseOcc。
- **评价指标**：
  - 占用预测：RayIoU（1m/2m/4m）、mIoU。
  - 场景流预测：平均绝对速度误差（mAVE），针对8个动态类别的真阳性（TP，距离阈值2m）。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力细节。仅提及学习率、优化策略、输入图像尺寸等与原始模型保持一致。因此无法统计训练资源。

## 5. 实验数量与充分性
- **实验组数**：
  - **主表（Table 1）**：在nuScenes验证集上与7种方法的定量对比（占用+流）。
  - **Table 2**：在OpenOcc标注下的详细类别mIoU对比（16类）。
  - **Table 3**：在三种不同基础模型（BEVDet-Occ、FB-Occ、SparseOcc）上的改进效果。
  - **Table 4**：消融实验，共6个版本（A~F），分别验证体积渲染 vs. 高斯、多帧、动静分解、加权采样等组件。
  - **图5**：采样超参数 \( t \) 的消融。
  - **图6**：数据分布与采样效果分析。
  - **定性结果**：图3、4可视化占用和场景流预测。
- **充分性与公平性**：
  - 实验覆盖了多个主流占用模型，并展示了即插即用的通用性。
  - 消融实验逐一去掉关键组件，证明每个设计的贡献。
  - 对比方法包括最新SOTA，同时基线模型也加入了场景流解码器以保证公平对比。
  - 评价指标全面（RayIoU、mIoU、mAVE）。但**未在更多数据集（如Waymo、KITTI）上验证**，存在一定单数据集偏差风险。

## 6. 主要结论与发现
- VoxelSplat在nuScenes验证集上显著提升性能：
  - 占用预测：RayIoU提升3.4个点（相比FB-Occ），mIoU提升3.1个点。
  - 场景流预测：mAVE从0.505降至0.303，提升0.202。
  - 在三种不同基线上均获得一致改进（RayIoU提升2.9~3.6）。
- 各组件均有效：
  - 高斯泼溅优于传统体积渲染（RayIoU +1.1 vs +0.4）。
  - 动静分解与未来视图显著改善场景流学习（mAVE从0.487降至0.353）。
  - 加权采样缓解不平衡，进一步提升（RayIoU +3.1，mAVE +0.231）。
- 推理时无需额外计算，是高效的正则化方法。

## 7. 优点
- **即插即用**：可无缝集成到多种占用网络架构，不增加推理开销。
- **创新性损失设计**：利用动态高斯泼溅连接3D表示与2D标签，同时增强语义和场景流学习。
- **解决不平衡问题**：加权点采样策略有效缓解类别和速度分布长尾。
- **实验全面**：多种基线、详细消融、定性可视化，结果支撑充分。

## 8. 不足与局限
- **仍需3D场景流真值**：尽管2D渲染损失辅助自监督，但模型仍依赖3D场景流标注（如OpenOcc）。完全无监督的场景流学习尚未实现。
- **单数据集验证**：仅在nuScenes上评估，未在Waymo、KITTI等数据集上测试，泛化性存疑。
- **仅针对占用与流预测**：作者指出可扩展到占用预测（forecasting）等任务，但当前未展示。
- **计算资源细节缺失**：未报告训练时间或GPU消耗，不利于复现和资源评估。

（完）
