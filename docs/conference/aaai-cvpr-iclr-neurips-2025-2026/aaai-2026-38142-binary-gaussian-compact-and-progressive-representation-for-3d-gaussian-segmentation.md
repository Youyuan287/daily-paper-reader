---
title: "Binary-Gaussian: Compact and Progressive Representation for 3D Gaussian Segmentation"
title_zh: Binary-Gaussian：面向3D高斯分割的紧凑渐进表示
authors: "An Yang, Chenyu Liu, Jun Du, Jianqing Gao, Jia Pan, Jinshui Hu, Baocai Yin, Bing Yin, Cong Liu"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38142/42104"
tags: ["query:open-vocab-d"]
score: 6.0
evidence: 每高斯类别二值编码实现从粗到细分割
tldr: 针对3D高斯分割中高维类别特征导致内存开销大、细粒度分割困难的问题，本文提出Binary-Gaussian，采用从粗到细的二值编码将每高斯类别压缩为单个整数，配合渐进训练策略，在显著降低内存的同时实现多粒度分割控制，性能优于现有方法。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有3D高斯分割依赖高维特征，存储贵且细粒度控制不足。
method: 提出二值编码方案，将每高斯类别映射为整数，设计渐进训练策略实现多粒度分割。
result: 大幅降低内存占用，并在多个分割基准上取得优异性能。
conclusion: 二值编码是一种高效的高斯分割表示方法，支持多粒度语义控制。
---

## Abstract
3D Gaussian Splatting (3D-GS) has emerged as an efficient 3D representation and a promising foundation for semantic tasks like segmentation. However, existing 3D-GS-based segmentation methods typically rely on high-dimensional category features, which introduce substantial memory overhead. Moreover, fine-grained segmentation remains challenging due to label space congestion and the lack of stable multi-granularity control mechanisms. To address these limitations, we propose a coarse-to-fine binary encoding scheme for per-Gaussian category representation, which compresses each feature into a single integer via the binary-to-decimal mapping, drastically reducing memory usage. We further design a progressive training strategy that decomposes panoptic segmentation into a series of independent sub-tasks, reducing inter-class conflicts and thereby enhancing fine-grained segmentation capability. Additionally, we fine-tune opacity during segmentation training to address the incompatibility between photometric rendering and semantic segmentation, which often leads to foreground-background confusion. Extensive experiments on multiple benchmarks demonstrate that our method achieves state-of-the-art segmentation performance while significantly reducing memory consumption and accelerating inference.

---

## 论文详细总结（自动生成）

# 论文总结：Binary-Gaussian: Compact and Progressive Representation for 3D Gaussian Segmentation

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **背景**：3D Gaussian Splatting (3D-GS) 已成为高效的3D场景表示方法，并被广泛用于语义分割等任务。现有基于3D-GS的分割方法通常为每个高斯分配高维连续类别特征（如32维向量），导致超过50%的额外参数开销，内存负担沉重。同时，细粒度分割面临标签空间拥挤、缺乏稳定多粒度控制机制等挑战。
- **核心问题**：如何在不牺牲分割精度的前提下大幅减少3D高斯分割的存储开销，并实现稳定、精确的多粒度（从粗到细）分割。
- **整体含义**：本文提出一种粗到细的二值编码方案，将每个高斯的类别特征压缩为一个整数，结合渐进式对比学习策略，使模型在显著降低内存占用（仅增加1.6%参数）和提升推理速度（超700 FPS）的同时，在细粒度分割和半透明物体分割上达到最优性能。

## 2. 论文提出的方法论

### 核心思想
- **二值编码代替连续向量**：用二进制码表示每个高斯的类别信息，通过“二进制-十进制”映射将每个高斯的特征存储为单个整数，大大压缩存储。
- **粗到细的多粒度表示**：将总特征分为L个层级，每个层级对应一个粒度级别。最终用于分割的特征为当前层及所有更粗层级特征的拼接，形成层次化索引。
- **渐进对比学习**：将多粒度分割任务分解为一系列独立子任务，避免类别冲突。通过数学归纳法推导约束：同一粒度内相同掩码的像素特征拉近，不同掩码且上一级相同的像素特征推远；上一级已不同的像素则不添加额外约束。
- **虚拟负样本指导**：当某粒度下无法进一步细分（只有正对）时，使用全1向量作为虚拟负样本对特征进行L2正则化，防止特征漂移。
- **不透明度微调**：语义渲染与光度渲染存在不兼容（如半透明物体），固定不透明度会导致前景-背景混淆。本文在分割训练中微调不透明度，解决离散表示下低不透明度高斯被抑制的问题。
- **掩码均衡采样**：为防止大掩码主导训练，采用部分随机采样+对每个选取的掩码等量采样的策略。

### 关键技术细节
1. **特征构建**：每个高斯共有 \( \bar{\mathbf{f}}_i \in \{0,1\}^D \)，分为L段 \( \bar{\mathbf{f}}_i^l \in \{0,1\}^{D_l} \)。渲染时通过光栅化得到像素级二值特征 \( \bar{\mathbf{F}}_p \)，并使用直通估计器（STE）保持二值化。
2. **损失函数**：
   - 渐进对比损失 \( \mathcal{L}_{\text{level-l}} \)：根据掩码关系对每个层级独立计算拉近/推远。
   - 虚拟负样本损失 \( \mathcal{L}_{\text{VN-l}} = \|\bar{\mathbf{F}}_p^l\|_2 \)。
   - 正则化损失 \( \mathcal{L}_{\text{reg}} = \|\mathbb{I}(\mathbf{F}_p > 0.5) - \mathbf{F}_p\|_2^2 \) 鼓励特征接近二值。
   - 总损失：\( \mathcal{L}_{\text{total}} = \lambda_{\text{reg}}\mathcal{L}_{\text{reg}} + \lambda_{\text{guiding}}\sum_l \mathcal{L}_{\text{VN-l}} + \lambda_{\text{con}}\sum_l \mathcal{L}_{\text{level-l}} \)，其中 \( \lambda_{\text{reg}}=10, \lambda_{\text{guiding}}=1, \lambda_{\text{con}}=1 \)。
3. **推理**：对每个高斯的二值特征 \( \bar{\mathbf{f}}_i^{1:l} \) 直接映射为十进制整数，用于检索同一类别的所有高斯，无需聚类或计算距离。

## 3. 实验设计

- **数据集与场景**：
  - **LERF-Mask**：包含三个场景，提供人工标注的粗粒度与细粒度真实掩码，用于多目标、多粒度分割评估。
  - **SPIn-NeRF**：包含十个真实场景的多视角单个物体掩码，用于单目标分割评估。
- **基准方法**：对比了两类方法：
  - NeRF-based：OmniSeg3D, GARField。
  - 3D-GS-based：GaussianGrouping, Feature3DGS, Click-Gaussian, SAGA。还对比了SAGA的压缩版本SAGA*（将特征压缩至32字节）。
- **评价指标**：mIoU（平均交并比），参数额外开销、推理速度（FPS）、多粒度支持能力。

## 4. 资源与算力

论文并未明确说明使用的GPU型号、数量或训练时长。仅在实现细节中提及训练配置（学习率、采样像素数等），未提及具体硬件资源。因此，无法从文中推断算力需求。

## 5. 实验数量与充分性

- **主要实验组数**：
  - 在LERF-Mask上进行了多粒度分割的定量对比（表1）及定性对比（图5）。
  - 在SPIn-NeRF上进行了单目标分割的定量对比（表2）。
  - 消融实验（表3）：对虚拟负样本、不透明度微调、掩码均衡采样三个组件分别进行消融，报告了粗/细粒度的mIoU变化。
  - 定性消融：额外提供了虚拟负样本缺失时的分割可视化（图4）和不透明度微调的可视化（图6）。
- **公平性与充分性**：
  - 实验覆盖了两个主流公开数据集，涵盖多目标和单目标、粗粒度和细粒度设置。
  - 对比方法包括各类代表性方法（NeRF-based和GS-based），并且包含了SAGA的压缩版本，保证了比较的公平性。
  - 消融实验完整，验证了每个提出组件的效果。
  - 然而，论文未在更大规模或更复杂场景（如大规模户外场景）上进行测试，可能存在泛化性局限。另外，未提供关于训练时间/收敛速度的比较。

## 6. 论文的主要结论与发现

- 二值编码方案能以极小的额外参数（1.6%增长）实现与高维连续特征相当甚至更优的分割性能。
- 渐进对比学习能有效分解多粒度分割任务，显著提升细粒度分割精度（在LERF-Mask上细粒度mIoU达86.2%，超过所有对比方法）。
- 不透明度微调有效解决了半透明物体前景-背景混淆问题。
- 推理速度大幅提升（769 FPS），比现有方法快10倍以上。
- 虚拟负样本策略在仅有正对的梯度隔离组中防止特征漂移，而非特征坍塌。
- 综合指标显示Binary-Gaussian在参数效率、速度、精度上均达到最优。

## 7. 优点

- **存储高效**：每高斯仅需32位整数，参数开销极低，易于部署。
- **推理快速**：直接十进制映射类别，无需聚类或距离计算，实现700+ FPS。
- **多粒度控制稳定**：粗到细层次化设计+渐进训练，避免了现有方法（如SAGA）中简单门控带来的不稳定性和精度损失。
- **解决半透明目标**：微调不透明度弥补了离散表示与语义渲染的不兼容性。
- **消融实验完整**：每个设计组件都通过定量和定性实验验证，结论可靠。
- **方法通用**：可集成到任何预训练的3D-GS模型上，仅需添加二值特征和训练。

## 8. 不足与局限

- **训练资源未明确**：未报告训练所需的GPU型号、数量和时间，难以评估实际算力成本。
- **场景覆盖有限**：仅在室内小规模场景（LERF-Mask、SPIn-NeRF）上验证，未涉及大规模无界场景或动态场景，泛化性有待验证。
- **对监督质量敏感**：依赖SAM生成的2D掩码作为训练信号（文中提及使用SAM自动掩码生成模块），掩码质量直接影响性能，但未讨论掩码噪声的影响。
- **二值表示的理论容量上限**：对于非常多的类别（如超过2^D个），需要增大D，可能增加参数量，论文未讨论极端情况下的扩展性。
- **与现有压缩方法的整合**：未探讨与SH压缩、坐标量化等压缩技术的联合优化，实际部署中可进一步降低整体存储。
- **实验数量**：定性结果仅展示部分场景，更多可视化见附录，但核心论文中展示的样本有限。

（完）
