---
title: "Dr. Splat: Directly Referring 3D Gaussian Splatting via Direct Language Embedding Registration"
title_zh: Dr. Splat：通过直接语言嵌入注册实现直接引用的3D高斯泼溅
authors: "Jun-Seong, Kim, Kim, GeonU, Yu-Ji, Kim, Wang, Yu-Chiang Frank, Choe, Jaesung, Oh, Tae-Hyun"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Jun-Seong_Dr._Splat_Directly_Referring_3D_Gaussian_Splatting_via_Direct_Language_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 将CLIP嵌入直接注册到3D高斯以实现开放词汇场景理解
tldr: 现有语言嵌入3DGS方法依赖渲染过程，效率较低。本文提出Dr. Splat，通过像素射线与高斯相交直接分配CLIP嵌入，并使用产品量化压缩嵌入。无需每场景优化，在3D开放词汇感知任务上显著超越现有方法。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有语言嵌入3DGS方法依赖渲染过程分配特征，效率低且性能受限。
method: 通过像素射线与高斯相交直接注册CLIP嵌入到主高斯，并采用产品量化压缩表示。
result: 在3D感知和检索任务上显著优于现有方法。
conclusion: 直接注册语言嵌入是一种高效且可扩展的3D开放词汇场景理解方式。
---

## Abstract
We introduce Dr. Splat, a novel approach for open-vocabulary 3D scene understanding leveraging 3D Gaussian Splatting. Unlike existing language-embedded 3DGS methods, which rely on a rendering process, our method directly associates language-aligned CLIP embeddings with 3D Gaussians for holistic 3D scene understanding. The key of our method is a language feature registration technique where CLIP embeddings are assigned to the dominant Gaussians intersected by each pixel-ray. Moreover, we integrate Product Quantization (PQ) trained on general large scale image data to compactly represent embeddings without per-scene optimization. Experiments demonstrate that our approach significantly outperforms existing approaches in 3D perception benchmarks, such as open-vocabulary 3D semantic segmentation, 3D object localization, and 3D object selection tasks.

---

## 论文详细总结（自动生成）

# Dr. Splat: 中文详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **问题**：现有语言嵌入的3D高斯泼溅（3DGS）方法（如LangSplat、LEGaussians）依赖2D渲染过程将语言特征蒸馏到3D高斯中。这种“通过渲染蒸馏”的方式存在两个关键缺陷：
  - 渲染过程会扭曲CLIP嵌入的原始语义，导致3D特征与2D语言对齐特征之间存在偏差。
  - 推理时必须预先渲染多视角2D特征图，再进行2D搜索，再提升到3D，计算成本高、覆盖不完整，且需要解决NP难的相机覆盖问题。
- **动机**：希望绕过中间渲染步骤，直接在3D高斯空间注册和引用语言对齐的CLIP嵌入，实现高效、准确、全空间覆盖的开放词汇3D场景理解。
- **整体意义**：提出一种无需每场景优化的直接注册方法，显著加速训练和推理，同时保持特征保真度。

## 2. 论文提出的方法论

### 2.1 核心思想
- 将2D CLIP嵌入直接分配给每个像素射线所穿过的“主高斯”（dominant Gaussians），通过多视图特征聚合得到每个高斯的最终特征，从而在3D空间直接进行搜索和引用。

### 2.2 关键技术细节

#### 特征注册过程（Feature Registration）
1. **2D CLIP嵌入提取**：使用SAM分割训练图像，为每个掩码区域提取对应的CLIP嵌入 `f_map`，得到每个像素的CLIP嵌入图。
2. **权重计算**：对于每个像素射线r，利用已优化的3DGS参数（固定），计算每个高斯θ_i对该像素的贡献权重 `wi(I,r) = Ti(I,r) * α̃i(I,r)`，其中Ti是透射率，α̃i是有效不透明度。
3. **Top-k选择**：沿每条射线只保留权重最高的k个高斯（Top-k Gaussians），减少计算和存储。
4. **多视图聚合**：通过加权求和公式 `˙fi = (∑_j wij * f_map_j) / ‖·‖₂`，其中 `wij = ∑_I ∑_r Mj(I,r) * wi(I,r)`，将来自不同视图的CLIP嵌入聚合到每个高斯。
5. **特征注册**：聚合后的特征直接赋值给对应高斯，无需梯度优化。

#### 产品量化（Product Quantization, PQ）特征压缩
- **动机**：存储高维32位浮点CLIP特征（512-D）对大量高斯来说内存过大。
- **方法**：在通用大规模图像数据集（LVIS，120万实例）上离线训练PQ码本，每个特征向量分为L个子向量，每个子向量使用8-bit索引量化。不需要针对每个场景训练码本。
- **优势**：压缩率达1/16（128维子向量），搜索复杂度从O(D)降至O(1)，且能泛化任意场景。
- **注册后**：每个高斯只存储PQ索引 `j_i`，推理时重建近似特征 `¯f_i`。

#### 推理：文本查询驱动的3D定位
1. 使用CLIP文本编码器提取文本查询特征 `q`。
2. 从PQ索引重建每个高斯的量化特征。
3. 计算余弦相似度，并采用LeRF中的相关性评分（relevancy score）进行重排序，增强判别力。
4. 通过阈值选择与查询最相关的3D高斯集合用于下游任务（选择、定位、分割）。

## 3. 实验设计

- **数据集**：
  - **LeRF-OVS**（由LangSplat标注）：5个场景（waldo, kitchen, ramen, figurines, teatime），用于3D对象选择任务。
  - **ScanNet**：随机选取8个室内场景（共4个场景？原文说randomly select eight scenes，但后面表格显示多个scenes），用于3D对象定位和3D语义分割任务。
- **基准（Benchmark）**：
  - 3D对象选择：2D渲染后的mIoU和mAcc。
  - 3D对象定位：使用提出的体积感知IoU（significant score加权），阈值设为0.15/0.3/0.45下的IoU。
  - 3D语义分割：多类别（10/15/19类）下的mIoU和mAcc。
- **对比方法**：
  - **LangSplat-m** 和 **LEGaussians-m**：将原始2D渲染方法修改为直接3D搜索的baseline（避免不公平比较）。
  - **OpenGaussian**：并发工作，同样支持直接3D搜索但需要每场景优化码本。
- **评价指标**：mIoU, mAcc, 体积感知IoU；所有方法使用相同的RGB预训练3DGS和相同的CLIP/SAM提取设置。

## 4. 资源与算力

- **未明确说明**：论文没有给出具体的GPU型号、数量、总训练时长等细节。图1的表格中仅提及我们的训练时间约10分钟（"～10 m"），但未说明硬件配置。其他方法的训练时间分别标为约24h（LERF）、4h（LangSplat/LEGaussians）、1h（OpenGaussian）。请注意这些数据可能在不同硬件下测得，论文未披露详细信息。

## 5. 实验数量与充分性

- **实验数量**：
  - 3个主要任务（对象选择、定位、语义分割）各包括多个场景/类别。
  - 定量结果表：表1（对象选择，5个场景）、表2a（定位，4个IoU阈值）、表2b（分割，3种类别粒度）。
  - 定性可视化：图5、7、8。
  - 消融实验：图9a（PQ子向量数量64/128/256）、图9b（Top-k值10/20/40/60/80），分析了搜索速度、压缩率与精度的权衡。
- **充分性**：实验覆盖了主要任务场景，对比了多个相关方法，并进行了消融研究。但局限性在于：
  - 仅使用了两个公开数据集，且ScanNet只选了8个场景，规模有限。
  - 缺少对更多样场景（如室外、动态场景）的验证。
  - 其他方法（LangSplat-m、LEGaussians-m）是自行修改的baseline，其公平性需要谨慎评估（尽管论文声称使用相同初始高斯和特征提取）。
- **客观性**：论文提出了新的体积感知IoU评估协议（图6论证了必要性），并尽量统一了所有方法的设置，整体较为客观。

## 6. 论文的主要结论与发现

1. **直接特征注册显著优于渲染蒸馏方法**：在对象选择、定位、分割任务上，Dr. Splat 在大多数指标上达到最佳或次佳，尤其在高IoU阈值下表现更突出（例如定位任务IoU>0.45时，Ours Top-40达25.6%，远超OpenGaussian的18.3%）。
2. **PQ压缩高效且通用**：128维子向量配置实现了存储效率与性能的最佳平衡，在1/16压缩率下保持竞争力。无需每场景优化即可泛化。
3. **Top-k参数控制性能-内存权衡**：增大k值提高定位和分割精度，但增加内存占用和未剪枝高斯数量（图9b）。
4. **渲染方法不适合直接3D任务**：LangSplat-m、LEGaussians-m在3D定位中表现很差（<10% IoU），证实了渲染特征在3D空间中的退化问题。

## 7. 优点

- **方法创新**：
  - 提出“逆体积渲染”的无梯度特征注册，直接分配CLIP嵌入，无需优化。
  - 首次将预训练PQ引入语言嵌入式3DGS，实现通用、可迁移的压缩。
- **效率优势**：无需每场景训练、无需渲染，10分钟完成特征注册；推理时直接3D搜索，速度远超基于渲染的方法（图1对比）。
- **公平性考量**：为渲染方法设计了修改版baseline（LangSplat-m等），并统一预训练3DGS和特征提取，减少比较偏差。
- **评估创新**：提出volume-aware IoU指标，考虑高斯椭球体积和不透明度的不等贡献，弥补了点云IoU的不足。

## 8. 不足与局限

- **实验覆盖有限**：
  - 仅在室内数据集（ScanNet）和少量物体场景（LeRF-OVS）上验证，未涉及室外、大尺度或动态场景。
  - 对比方法中，OpenGaussian是基于点级查询的，但不同方法对Top-k参数的敏感度不同，文中未深入讨论。
- **PQ压缩信息损失**：虽然提出了权衡，但未分析不同子向量数量对细粒度区分类别的影响（例如对相似物体的区分能力）。
- **Top-k选择依赖**：k值需根据场景和任务预设，没有自适应调整策略；过小的k导致信息丢失，过大则增加存储。
- **特征注册准确性**：基于像素射线与高斯的相交权重分配，可能对高斯分布初始化敏感（文中提到使用相同RGB预训练3DGS，但不同方法可能收敛到不同高斯分布）。
- **未提及算力细节**：无法独立复现或比较计算开销，削弱了可重复性。
- **应用限制**：当前仅演示了分割、定位、选择任务，未测试更复杂的引用（如属性描述、关系查询）或交互式应用（如抓取）。

（完）
