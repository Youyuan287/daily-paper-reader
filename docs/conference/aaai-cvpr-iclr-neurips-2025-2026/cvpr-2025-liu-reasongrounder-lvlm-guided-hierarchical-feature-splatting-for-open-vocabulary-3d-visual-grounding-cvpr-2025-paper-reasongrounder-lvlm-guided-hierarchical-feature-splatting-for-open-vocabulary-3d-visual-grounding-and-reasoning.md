---
title: "ReasonGrounder: LVLM-Guided Hierarchical Feature Splatting for Open-Vocabulary 3D Visual Grounding and Reasoning"
title_zh: ReasonGrounder：LVLM引导的分层特征泼溅用于开放词汇3D视觉定位与推理
authors: "Liu, Zhenyang, Wang, Yikai, Zheng, Sixiao, Pan, Tongying, Liang, Longfei, Fu, Yanwei, Xue, Xiangyang"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liu_ReasonGrounder_LVLM-Guided_Hierarchical_Feature_Splatting_for_Open-Vocabulary_3D_Visual_Grounding_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 8.0
evidence: 分层3D高斯场用于开放词汇3D定位
tldr: 现有开放词汇3D定位方法严重依赖3D标注和掩码提议，难以处理多样语义。本文提出ReasonGrounder，采用LVLM引导的分层3D特征高斯场，根据物理尺度自适应分组，实现开放词汇3D定位与推理。实验证明该方法在遮挡场景下也能准确推理。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有方法依赖3D标注和掩码提议，无法处理多样语义和常识推理。
method: 使用LVLM引导的分层3D特征高斯场，根据物理尺度自适应分组，结合语言描述进行定位。
result: 在多个基准上取得了最先进的开放词汇3D视觉定位性能。
conclusion: 分层高斯场与LVLM结合有效提升了开放词汇3D场景理解的鲁棒性。
---

## Abstract
Open-vocabulary 3D visual grounding and reasoning aim to localize objects in a scene based on implicit language descriptions, even when they are occluded. This ability is crucial for tasks such as vision-language navigation and autonomous robotics. However, current methods struggle because they rely heavily on fine-tuning with 3D annotations and mask proposals, which limits their ability to handle diverse semantics and common knowledge required for effective reasoning. To address this, we propose ReasonGrounder, an LVLM-guided framework that uses hierarchical 3D feature Gaussian fields for adaptive grouping based on physical scale, enabling open-vocabulary 3D grounding and reasoning. ReasonGrounder interprets implicit instructions using large vision-language models (LVLM) and localizes occluded objects through 3D Gaussian splatting. By incorporating 2D segmentation masks from the Segment Anything Model (SAM) and multi-view CLIP embeddings, ReasonGrounder selects Gaussian groups based on object scale, enabling accurate localization through both explicit and implicit language understanding, even in novel, occluded views. We also contribute ReasoningGD, a new dataset containing over 10K scenes and 2 million annotations for evaluating open-vocabulary 3D grounding and amodal perception under occlusion. Experiments show that ReasonGrounder significantly improves 3D grounding accuracy in real-world scenarios.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：开放词汇3D视觉定位与推理旨在根据隐式语言描述（如“桌上被玩具羊部分遮挡的红色圆形甜水果”）在场景中定位物体，即使物体被遮挡。该能力对视觉语言导航、自主机器人等应用至关重要。
- **现有局限**：当前方法严重依赖3D标注和掩码提议（如ScanRefer、ReferIt3D），导致泛化能力差，无法处理多样语义和常识推理；即使近期的开放词汇方法（如LERF、LangSplat）能处理显式查询，但对隐式指令理解不足，且在遮挡场景下难以定位完整物体。
- **整体意义**：提出ReasonGrounder，结合LVLM（大型视觉语言模型）与层次化3D高斯特征场，首次实现开放词汇下的隐式指令理解和遮挡物体的完整定位，并贡献了大规模评价数据集ReasoningGD。

## 2. 论文提出的方法论

### 核心思想
- 利用3D Gaussian Splatting（3DGS）表示场景，为每个高斯附加一个潜在特征向量，通过两个浅层MLP分别映射为层次化语言特征和实例特征。
- 引入物理尺度（从深度图计算）作为额外条件，使特征场具有尺度感知能力，支持自适应分组。
- 利用LVLM（LLaVA 1.5）从顶视图和隐式查询中推理出目标物体，并选择与查询最相关的参考视图。
- 基于参考视图中的CLIP相似度选择最佳尺度，使用HDBSCAN对层次化实例特征进行聚类，从而定位对应物体，并通过渲染实现任意新视角下的完整感知（包括遮挡部分）。

### 关键技术细节
1. **尺度层次化特征高斯场**
   - 训练标准3DGS后，使用SAM从训练视图中生成2D掩码，并计算每个掩码的3D物理尺度（基于深度标准差）。
   - 为每个高斯附加潜在特征 \( f_{g_i} \)，通过语言映射器 \( F_l(s_i, f_{g_i}) \) 和实例映射器 \( F_g(s_i, f_{g_i}) \) 分别得到层次化语言特征 \( \phi_{g_i}^s \) 和实例特征 \( \psi_{g_i}^s \)。
   - 语言特征通过可微分栅格化渲染后，使用Huber损失监督CLIP嵌入（经PCA压缩），确保多视角一致性。
   - 实例特征通过对比损失（类似GARField）进行监督：同一掩码内的像素特征接近，不同掩码的特征远离，损失函数如公式(7)所示。

2. **LVLM引导的层次化分组**
   - **参考视图选择**：将顶视图 \( I_{td} \) 和隐式查询 \( Q_{im} \) 输入LVLM，得到目标物体 \( O_t \) 和解释 \( E \)；然后利用CLIP图像-文本编码器从训练视图中选出与 \( O_t \) 最相似的视图作为参考视图 \( \hat{V} \)。
   - **层次化高斯分组**：在参考视图上计算每个像素的语言特征与目标物体CLIP嵌入的相似度，选出最相关的像素及其对应的尺度 \( s_{i^*} \)；用该尺度对实例特征进行层次化渲染，再通过HDBSCAN聚类得到高斯组 \( \{G_i\} \)；最后选择与参考像素实例特征最匹配的聚类中心对应的组 \( G_{i^*} \) 作为目标物体。
   - 对于任意新视角，只需渲染该高斯组即可实现完整物体的定位（包括被遮挡部分）。

### 公式说明（文字描述）
- 高斯函数定义：\( G(x) = \exp(-\frac12 (x-\mu)^\top \Sigma^{-1} (x-\mu)) \)
- 颜色/特征渲染：使用alpha混合公式 \( C = \sum_{i\in\mathcal{N}} c_i \alpha_i \prod_{j=1}^{i-1} (1-\alpha_j) \)
- 损失函数：语言损失 \( L_{lang} = L_\delta(\phi_i^s, \hat{\phi_i}) \)；实例对比损失如公式(7)所示。
- 参考视图选择：\( \hat{V} = \arg\max_{V_i} \frac{E_{im}(V_i)\cdot E_{te}(O_t)}{\|E_{im}(V_i)\|\|E_{te}(O_t)\|} \)
- 尺度选择：\( s_{i^*} = \arg\max_{s_i} \text{sim}(\phi_{o_t}, \phi_i) \)
- 最终高斯组选择：\( G_{i^*} = \{ G_i \mid T_i = \arg\max_{T_j} \frac{\hat{T_i}\cdot T_j}{\|\hat{T_i}\|\|T_j\|} \} \)

## 3. 实验设计

### 数据集 / 场景
- **LERF数据集**：13个真实场景（ramen, figurines, teatime, kitchen等），用于开放词汇3D视觉定位。
- **3D-OVS数据集**：包含长尾物体，用于开放集合3D语义分割。
- **ReasoningGD数据集（本文贡献）**：超10K场景，263种物体类型，约200万标注，每个场景含100张RGB-D图像、点云、相机位姿、2D可见/完整掩码，专门用于评价遮挡下的隐式指令定位和模态外感知。

### Benchmark / 对比方法
- 对比方法包括：
  - 2D方法：LSeg、ODISE、OV-Seg
  - 3D方法：LERF、LangSplat、3D-OVS、FFD（Feature Field Distillation）等
- 评价指标：**定位准确率（Localization Accuracy）**和**平均IoU**。

### 对比结果
- **LERF数据集**（显式查询）：ReasonGrounder定位准确率86.7%（vs LangSplat 84.3%）；平均IoU 55.1%（vs LangSplat 51.4%）。
- **3D-OVS数据集**（显式查询）：ReasonGrounder平均IoU 94.7%（vs LangSplat 93.4%）。
- **隐式指令实验**：在ReasoningGD的5个场景上平均IoU 91.4%；在挑战性场景（小物体、多层级、相似物体）上平均IoU 78.5%（远超LangSplat 58.9%）。
- **模态外感知**：在ReasoningGD的5个场景上平均IoU 90.4%（仅对ReasonGrounder评估，因为其他方法不支持）。

## 4. 资源与算力

- **训练硬件**：NVIDIA RTX-3090 GPU（单卡） + 14 vCPU Intel Xeon Gold 6330 @ 2.00GHz。
- **训练时长**：
  - 标准3DGS训练30,000迭代。
  - 层次化特征场训练10,000迭代（仅优化潜在特征和两个MLP）。
- **推理速度**：在消融实验中提到，使用3DGS后每视角渲染约0.025秒（vs NeRF 0.92秒），效率显著提升。
- **未说明**：总训练时间（小时级）未明确给出，但根据迭代数可推算较短（约数小时内可完成一个场景）。

## 5. 实验数量与充分性

- **实验组数**：至少包括三大数据集上的定量对比（表1-4）、挑战场景实验（表5）、模态外感知实验（表6）、消融实验（表7）以及定性可视化（图4-6）。
- **充分性分析**：
  - 对比了主流开放词汇3D定位方法（LSeg、LERF、LangSplat等），覆盖了2D和3D基线。
  - 在显式和隐式查询两种场景下均评估，且专门设计了挑战场景测试鲁棒性。
  - 消融实验验证了各组件（LVLM、3DGS、层次化特征）的贡献。
  - 定性结果展示了遮挡、复杂指令等典型情况。
- **客观性与公平性**：方法间使用相同的显式查询进行公平比较；隐式指令实验因其他方法不支持故未直接对比，但给出了ReasonGrounder自身的性能。总体实验设计合理，结果有说服力。

## 6. 论文的主要结论与发现

- **分层特征+LVLM是关键**：通过将物理尺度融入高斯特征并利用LVLM推理隐式意图，ReasonGrounder显著提升了开放词汇3D定位的准确性和鲁棒性，特别是在遮挡场景下。
- **规模数据集有效**：提出的ReasoningGD数据集包含大量遮挡标注，可有效评估隐式指令理解和模态外感知。
- **效率优势**：相比NeRF-based方法（LERF），3DGS-based的ReasonGrounder推理速度快约36倍（每视角0.025s vs 0.92s）。
- **综合性能领先**：在LERF、3D-OVS、ReasoningGD三个基准上均达到SOTA。

## 7. 优点

- **创新性强**：首次将层次化3D高斯特征场与LVLM结合，实现开放词汇隐式指令定位和遮挡物体完整感知。
- **数据贡献**：提供10K+场景、200万标注的ReasoningGD数据集，填补了该领域缺乏评估遮挡感知和隐式推理的基准空白。
- **效率与精度平衡**：基于3DGS的渲染速度远超NeRF方法，同时精度更高。
- **消融扎实**：逐步验证了每个模块的必要性（LVLM推理、3DGS加速、层次化分组、尺度引导）。
- **通用性**：支持显式和隐式两种查询模式，适用于多种实际应用场景。

## 8. 不足与局限

- **依赖预训练模型**：方法严重依赖SAM生成掩码和CLIP提供语言嵌入，这些模型在特定场景（如极端光照、罕见物体）可能失效，引入误差。
- **实验覆盖有限**：
  - 真实场景仅采用LERF的13个场景（规模较小），且ReasoningGD为合成数据集（Blenderproc生成），真实世界泛化能力有待进一步验证。
  - 隐式指令实验仅在ReasoningGD的5个场景上进行，对比对象不足（无其他开放词汇推理方法）。
- **计算资源**：虽然单卡3090可训练，但每个场景需分别训练，对于大规模实际部署可能效率不足。
- **遮挡处理局限性**：对于完全遮挡且无任何视角可见的物体，LVLM推理可能失败（因为需从顶视图推理），且实验未覆盖此类极端情况。
- **评价指标单一**：主要使用IoU和定位准确率，未评估推理正确性（如解释E的质量）或对话能力。

（完）
