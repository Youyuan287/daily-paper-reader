---
title: Evolving Semantic Propagation for Aerial Semantic 3D Gaussian Splatting
title_zh: 面向航空语义3D高斯泼溅的演化语义传播
authors: "Zihan Gao, Lingling Li, Xu Liu, Fang Liu, Licheng Jiao, Puhua Chen, Wenping Ma, Shuyuan Yang"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/42418/46379"
tags: ["query:open-vocab-d"]
score: 7.0
evidence: 通过传播实现弱监督语义3D高斯泼溅
tldr: 针对大范围航空场景精细标注成本高的问题，本文提出EvoPropGS，利用SAM与DINOv2构建提示库，通过余弦相似度匹配将稀疏标注语义传播至未标注区域，仅需少量标注即可实现3D高斯泼溅模型的语义分割，在多个航拍数据集上验证了高效性。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 大范围航空场景的密集3D语义标注成本极高，亟需弱监督方法。
method: 构建SAM+DINOv2提示库，通过区域匹配为未标注区域生成伪标签，实现语义传播。
result: 在少量标注下达到接近全监督的语义分割精度，显著降低标注成本。
conclusion: 利用结构重复性进行语义传播是弱监督3D语义分割的有效途径。
---

## Abstract
Semantic understanding of large-scale aerial scenes represents a critical challenge in 3D computer vision, hindered by the prohibitive cost of dense annotation. This paper introduces EvoPropGS, a novel approach for the semantic segmentation of 3D Gaussian Splatting models that requires only minimal supervision. Our core insight is to leverage the inherent structural repetitions within aerial environments to propagate semantic information from a sparse set of annotations across the entire 3D scene. Our approach constructs a prompt library by pairing SAM-generated mask candidates with DINOv2 feature embeddings from annotated views. For unannotated regions, we generate pseudo-labels by matching region proposals with these featured prompts via cosine similarity. We then formulate optimal prompt selection as a discrete optimization problem solved via evolutionary search, guided by our novel fitness function that evaluates both 3D consistency and 2D semantic coherence. Extensive experiments demonstrate that EvoPropGS achieves accurate segmentation with only 2 percent annotated pixels.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

大范围航空场景的语义理解是3D计算机视觉的关键挑战，但密集标注成本极高，成为主要瓶颈。现有方法分为两类：

- **特征蒸馏方法（如LERF、LangSplat）**：利用CLIP等视觉-语言模型将2D特征提升到3D，但存在显著的域偏移（训练数据多为地面视角）且特征粗糙，难以处理空中场景中的小物体。
- **标签学习方法（如Semantic-NeRF、Gaussian Grouping）**：直接从2D标注学习，但标注需覆盖大量视角，人工成本过高。

本文观察到航空场景具有**结构性重复**（建筑物、道路、植被等在不同区域保持相似视觉特征），可利用这一特性从极稀疏标注（仅2%像素）高效传播语义，从而降低标注成本。

## 2. 方法论：核心思想、关键技术细节与算法流程

**核心思想**：利用SAM生成类不可知实例掩码，结合DINOv2特征构建提示库；通过余弦相似度匹配为未标注视图生成伪标签，并将最优提示选择建模为离散优化问题，使用进化算法搜索。

### 关键技术细节（三阶段）

1. **候选提示生成（Candidate Prompt Generation）**  
   - 对少量标注视图，使用SAM生成大量实例掩码。  
   - 通过IoU和**包含比** \( \mathcal{C}(s_j, M_{k,c}) = |s_j \cap M_{k,c}| / |s_j| \) 筛选高质量掩码（保留 \( \mathcal{C} \geq \tau_{\text{contain}} \) 的掩码）。  
   - 对每个选定掩码，使用DINOv2特征图进行掩码池化，得到特征向量 \( f_{j,c,k} \)，构成候选提示库 \( \mathcal{P} \)。

2. **伪标签生成与进化提示选择（Evolutionary Prompt Selection）**  
   - **伪标签生成**：对未标注目标视图，同样用SAM生成区域提议，提取DINOv2特征；通过余弦相似度与参考特征集匹配，分配语义标签。  
   - **进化搜索**：将选择最优提示组合编码为整数向量（每个类-视角对分配固定数量的槽位），使用差分进化算法搜索。  
   - **适应度函数**（公式8）：  
     \[
     \mathcal{F}(v) = \lambda_{3D} \mathcal{F}_{3D}(v) + \lambda_{2D} \mathcal{F}_{2D}(M_{\text{pseudo}}^t)
     \]
     - 3D一致性 \( \mathcal{F}_{3D} \)：将伪标签反投影到3D高斯中，与基于稀疏标注计算的**粗略语义先验**（公式5）比较，计算无冲突高斯比例。  
     - 2D连贯性 \( \mathcal{F}_{2D} \)：基于DINOv2特征，计算伪标签的类内相似度 \( S_{\text{intra}} \) 与类间相似度 \( S_{\text{inter}} \) 之差（公式7）。  

3. **语义场优化（Semantic Field Optimization）**  
   - 使用交叉熵损失（公式9）同时优化原始标注视图和进化生成的伪标签视图，训练每个3D高斯的语义特征向量 \( f_i \)。

## 3. 实验设计

- **数据集**：3D-AS数据集（来源为Google Earth，分辨率1600×900，像素对应0.3-0.5米），包含9个真实场景（City、Country、Port各3个场景）。  
- **稀疏监督设置**：每场景仅标注3个视图中的少量物体，约2%像素。  
- **对比方法**：  
  - 2D分割模型：LSeg、MaskCLIP（直接逐视图预测）  
  - 特征蒸馏方法：LERF、LangSplat、Feature 3DGS  
  - 标签学习方法：Semantic-Gaussian（3DGS版Semantic-NeRF）、Gaussian Grouping（修改为使用稀疏标注）  
- **评估指标**：mIoU（平均交并比）和mAcc（平均准确率）。

## 4. 资源与算力

文中**未明确说明**使用的GPU型号、数量及训练时长。仅提及实验在常规深度学习硬件上完成，未提供具体算力信息。

## 5. 实验数量与充分性

实验较为充分，包含：

- **主对比实验**：在9个场景上与7种方法对比，呈现完整定量和定性结果（表1、图3）。  
- **消融实验**（表2）：  
  - 稀疏监督无传播基线  
  - 替换搜索机制（随机选择、特征平均）  
  - 适应度函数分量（仅2D、仅3D）  
  - 编码方式（刚性编码 vs. 灵活编码）  
- **基础模型选择分析**（表3）：对比CLIP、MAE、DINOv2作为特征提取器。  
- **与密集监督对比**（表4）：与使用相同3视图完整标注的密集监督方法比较。  

**充分性评价**：实验覆盖多类场景、多种基线、完整消融，结果客观公平。但缺少更大规模场景或更多标注比例的扩展实验，也未做统计显著性检验。

## 6. 主要结论与发现

- EvoPropGS在仅2%标注像素下，mIoU达46.3%–72.4%，显著优于所有对比方法。  
- 特征蒸馏方法因域偏移和粗糙特征导致性能差（mIoU常低于15%），标签学习方法在未标注区传播不足（mIoU约25%–48%）。  
- 消融研究表明各组件（进化搜索、混合适应度、灵活编码）均贡献明显。  
- DINOv2优于CLIP和MAE作为特征提取器。  
- EvoPropGS甚至超越了使用相同视图密集标注的Gaussian Grouping，接近密集监督的Semantic-Gaussian性能。

## 7. 优点

- **创新性**：首次将语义传播与进化优化用于航空3DGS弱监督分割，利用结构重复性这一先验。  
- **实用性**：仅需2%像素标注，大幅降低人工成本。  
- **技术巧妙性**：  
  - 结合SAM的几何提议与DINOv2的强表征。  
  - 混合适应度函数同时考虑3D全局一致性与2D局部语义清晰性。  
  - 进化搜索避免穷举，高效选择最优提示。  
- **实验完整**：多场景、多基线、多消融，验证充分。

## 8. 不足与局限

- **算力信息缺失**：未提及训练时间与GPU资源，无法评估计算开销。  
- **数据集单一**：仅在3D-AS数据集上评估，缺少对更多样化航空数据集（如Urban3D、AIRS）的验证。  
- **依赖SAM质量**：航空小物体可能无法被SAM完美分割。  
- **阈值敏感性**：包含比阈值 \( \tau_{\text{contain}} \) 等超参数对结果可能有影响，未做敏感性分析。  
- **规模化问题**：最大场景规模有限（1600×900），超大规模场景（如城市级）的传播效果未知。  
- **标签类别假设**：假设场景中仅包含预先定义的固定类别，未讨论开放词汇场景。  
- **潜在偏差**：仅依靠3个标注视图的先验进行3D一致性评估，若标注视图分布不佳可能引入偏差。

（完）
