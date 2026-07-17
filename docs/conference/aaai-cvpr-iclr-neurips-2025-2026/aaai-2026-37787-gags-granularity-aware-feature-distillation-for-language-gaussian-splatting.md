---
title: "GAGS: Granularity-Aware Feature Distillation for Language Gaussian Splatting"
title_zh: GAGS：粒度感知的特征蒸馏用于语言高斯泼溅
authors: "Yuning Peng, Haiping Wang, Yuan Liu, Chenglu Wen, Zhen Dong, Bisheng Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/37787/41749"
tags: ["query:open-vocab-d"]
score: 10.0
evidence: 将2D CLIP特征蒸馏到3D高斯泼溅实现开放词汇查询
tldr: 针对2D特征蒸馏到3D场时多视图不一致的问题，提出GAGS框架。通过将SAM的提示点密度与相机距离关联，并引入粒度感知策略，有效提升了多视图一致性。该方法将CLIP特征嵌入3D高斯泼溅，支持开放词汇查询任意视角渲染，在开放词汇3D语义分割任务上取得了先进性能。其核心思想直接匹配用户对3D高斯泼溅语义场和开放词汇的需求。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 将2D CLIP特征蒸馏到3D场时面临多视图不一致，导致语义场不稳定。
method: 提出粒度感知策略，关联SAM提示点密度与相机距离，蒸馏CLIP特征到3D高斯泼溅。
result: 在开放词汇3D语义分割基准上，GAGS优于现有蒸馏方法。
conclusion: 多视图一致性改进使高斯泼溅能有效承载开放词汇语义。
---

## Abstract
3D open-vocabulary scene understanding, which accurately perceives complex semantic properties of objects in space, has gained significant attention in recent years. In this paper, we propose GAGS, a framework that distills 2D CLIP features into 3D Gaussian splatting, enabling open-vocabulary queries for renderings on arbitrary viewpoints. The main challenge of distilling 2D features for 3D fields lies in the multiview inconsistency of extracted 2D features, which provides unstable supervision for the 3D feature field. GAGS addresses this challenge with two novel strategies. First, GAGS associates the prompt point density of SAM with the camera distances to scene objects, which significantly improves the multiview consistency of segmentation results. Second, GAGS further decodes a granularity factor to guide the distillation process and this granularity factor can be learned in a unsupervised manner to only select the multiview consistent 2D features in the distillation process. Experimental results on two datasets show that GAGS improves visual grounding accuracy by an average of 10.9% and semantic segmentation accuracy by an average of 7.0%, with an inference speed 2× faster than baseline methods.

---

## 论文详细总结（自动生成）

# GAGS: Granularity-Aware Feature Distillation for Language Gaussian Splatting 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：3D 开放词汇场景理解旨在通过自然语言查询的方式感知空间中物体的复杂语义属性，在机器人、自动驾驶等领域具有重要应用。由于缺乏大规模带语言标注的 3D 数据集，现有方法多将 2D 视觉 - 语言模型（如 CLIP）的知识扩展到 3D 场景。
- **核心问题**：将 2D CLIP 特征蒸馏到 3D 场（如 3D Gaussian Splatting）时，不同视角下提取的 2D 特征存在严重的多视图不一致性，导致 3D 特征场监督不稳定、产生模糊和退化的特征。现有方法（如 LangSplat）虽试图通过多粒度（whole、part、subpart）分离蒸馏缓解该问题，但仍存在训练成本高、查询开销大、结果模糊等问题。
- **整体含义**：GAGS 旨在学习一个轻量、统一且高度一致的 3D 特征场，通过粒度感知策略提升多视图一致性，实现高效、准确的开放词汇场景理解。

## 2. 方法论：核心思想、关键技术细节

### 核心思想
- 利用场景的几何表示（3D Gaussian 深度场）指导 SAM 和 CLIP 产生多视图一致的语义特征，并通过自监督方式自适应选择最合适的粒度进行特征蒸馏。

### 关键技术细节

#### （1）粒度感知分割（Granularity-aware Segmentation, GaS）
- **观察**：SAM 的分割粒度由提示点密度控制；同一物体在不同视角下的表观大小随相机距离变化（远处小、近处大）。因此最优提示点密度应与相机距离相关。
- **方法**：
  - 为每张输入图像渲染深度图 D，并计算每个像素对应的最小可见深度值 M_D（跨所有视角）。
  - 将图像划分为网格块，对每个块 P，根据公式计算提示点数量：
    \[
    n_P = \frac{1}{|P|} \sum_{p \in P} \frac{D^2(p)}{M_D^2(p)} \cdot n
    \]
    其中 n 为预设的提示点基数。
  - 该公式使得近距离视角使用较稀疏的提示点（接近基准 n），远距离视角使用更密集的提示点，从而在不同视角下对同一物体实现一致的 SAM 分割结果。

#### （2）粒度感知蒸馏（Granularity-aware Distillation, GaD）
- **目标**：自动为每个物体选择最合适的分割粒度（subpart、part、whole），即在该粒度下多视图 CLIP 特征一致性最好。
- **步骤**：
  - 对每个训练视角渲染特征图 \( f_{\text{render}} \)，通过 MLP 解码器 \( D \) 得到预测的 CLIP 特征 \( f_{\text{clip}} \) 和粒度因子 \( \eta \in \mathbb{R}^3 \)。
  - 对 \( \eta \) 进行 softmax 得到三个粒度的权重 \( \alpha = \text{softmax}(\eta) \)。
  - 加权蒸馏损失：
    \[
    \ell_{\text{distill}} = \sum_{n \in \{s,p,w\}} \alpha_n \| f_{\text{clip}} - f_n \|_2^2
    \]
    其中 \( f_s, f_p, f_w \) 分别为对应粒度的 CLIP 特征图。
  - 熵正则化项 \( \ell_{\text{entropy}} = -\sum \alpha_n \log \alpha_n \) 促使权重收敛到单一粒度，避免平均化。
- **区域感知加权蒸馏（Region-aware Weighted Distillation）**：为解决大物体主导损失的问题，用融合后的掩码 \( m_f \)（按最高权重选择粒度）计算每个区域的面积，对损失进行归一化：
    \[
    \ell_{\text{r-distill}} = \beta_r \ell_{\text{distill}}, \quad \beta_r = \frac{\sum_i S(R_i)}{n_r \cdot S(R)}
    \]
- **特征一致性损失**：鼓励同一分割区域内的特征相互接近，增强 3D 高斯体特征一致性：
    \[
    \ell_{\text{cons}} = \sum_{i=1}^{n_r} \frac{\sum_{p \in R_i} (f_{\text{clip}}^p - f_{R_i})^2}{S(R_i)}
    \]
- **总损失**：\( L = \ell_{\text{r-distill}} + \lambda_{\text{entropy}} \ell_{\text{entropy}} + \lambda_{\text{cons}} \ell_{\text{cons}} \)

## 3. 实验设计

- **数据集**：
  - **LERF 数据集**：增强版，包含 4 个场景（ramen、waldo kitchen、teatime、figurines），提供文本描述及多视角分割掩码，按 SAM 粒度分为 subpart、part、whole 三级。
  - **Mip-NeRF360 数据集**：自标注版本，选用其中最复杂的 4 个场景（room、counters、garden、bonsai），采用与 LERF 相同格式标注。
- **基准方法**：GS-Grouping、LEGaussian、GOI、LangSplat。所有方法统一使用 GSplat 构建初始高斯场。
- **评估指标**：
  - 定位：平均准确率（mAcc，预测位置落在 GT 框内）。
  - 语义分割：平均 IoU（mIoU）。
- **实验结果**：GAGS 在 LERF 上提升定位 8.7% mAcc、分割 6.1% mIoU；在 Mip-NeRF360 上提升 13.2% mAcc、7.9% mIoU，尤其于 fine-grained 部件级提升显著。

## 4. 资源与算力

- **硬件**：所有实验在单张 RTX 4090 GPU 上完成（文中明确说明）。
- **训练时间**（以场景“ramen”为例）：预处理 25 min + RGB 高斯训练 10 min + 语言场训练 50 min，总计约 85 min。
- **推理时间**：渲染特征 13 s + 预测掩码 11 s，总计 24 s，相比 LangSplat（49 s）快约 2 倍。

## 5. 实验数量与充分性

- **主要对比实验**：在两个数据集（共 8 个场景）上，与 4 种基线方法比较定位和分割性能，按 subpart/part/whole 三级报告，共计 8×2×2=32 组数值结果。
- **消融实验**：在 Mip-NeRF360 数据集上进行了 7 组设置（表 4），包括平均蒸馏、单独粒度蒸馏、加入 GaD、加入 GaS 等，验证了各模块的有效性。
- **充分性评价**：
  - **覆盖全面**：涉及多个场景、多种粒度、多种指标。
  - **对比公平**：所有基线基于相同 GSplat 基础，采用统一评价流程。
  - **消融彻底**：分别验证了 GaS 和 GaD 的贡献，并对比了单粒度蒸馏和简单平均。
  - **客观性**：实验设计规范，结果详实，无明显偏向。

## 6. 主要结论与发现

1. **多视图一致性是关键瓶颈**：显式提升 SAM 分割和 CLIP 特征的多视图一致性（通过 GaS）是改善 3D 特征场质量的有效手段。
2. **自适应的粒度选择优于固定多粒度**：GaD 通过自监督方式为每个物体选择最合适的粒度，避免了不一致特征的干扰，且仅需一个统一特征场，提高了查询效率。
3. **性能显著提升**：GAGS 在开放词汇定位和分割任务上大幅超越现有方法，尤其在小物体和复杂场景下；推理速度是 LangSplat 的两倍。
4. **泛化性良好**：在两个不同数据集上均取得领先结果，验证了方法的鲁棒性。

## 7. 优点

- **方法创新性**：
  - 首次将 SAM 的提示点密度显式地与相机距离关联，提出几何引导的粒度一致分割（GaS），简单有效。
  - 提出自监督的粒度因子学习机制（GaD），无需额外标注即可自动选择最佳蒸馏粒度，避免多粒度分离存储和查询。
- **效率优异**：仅维护一个特征场，推理时只需一次渲染和比较，速度比 LangSplat 快 2 倍；存储开销更小。
- **实验设计严谨**：多数据集、多层次评价、充分消融，并与多个强基线公平对比。
- **实际应用价值**：在开放词汇 3D 场景理解任务上实现了精度与速度的双重提升，适用于机器人导航、增强现实等实时交互场景。

## 8. 不足与局限

- **信息损失风险**：自监督的粒度优化虽然减少了异常值，但可能丢弃一些多视角不一致但仍有用的信息，导致复杂场景中的组件级识别受限（如图 7 所示，某些小部件或精细特征可能未被正确激活）。
- **依赖初始高斯场质量**：GaS 中最小深度图计算依赖预训练 RGB 高斯场的几何准确性，若初始重建不佳（如稀疏视角或弱纹理区域），会影响提示点密度计算。
- **场景局限性**：仅评估了室内和庭院等有限场景，未在更大规模或户外驾驶等场景中测试，泛化性有待进一步验证。
- **标注依赖**：LERF 数据集和 Mip-NeRF360 标注均基于 SAM 预定义三级粒度，可能无法覆盖所有真实物体尺度；区域感知损失的掩码融合策略也可能引入误差。
- **消融仅针对 Mip-NeRF360 数据集**：缺少在 LERF 数据集上的独立消融，略有不足。

（完）
