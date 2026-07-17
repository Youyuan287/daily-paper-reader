---
title: "OpenScan: A Benchmark for Generalized Open-Vocabulary 3D Scene Understanding"
title_zh: "OpenScan: 广义开放词汇三维场景理解基准"
authors: "Youjun Zhao, Jiaying Lin, Shuquan Ye, Qianshi Pang, Rynson W. H. Lau"
date: 2026-03-17
pdf: "https://ojs.aaai.org/index.php/AAAI/article/download/38331/42293"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 广义开放词汇三维场景理解基准
tldr: 现有开放词汇三维场景理解局限于物体类别的开放词汇问题，未能全面评估模型对三维场景的理解程度。本文提出广义开放词汇三维场景理解任务，包含物体细粒度属性等多样化语言查询，并构建了OpenScan基准。该基准推动了开放词汇三维理解向更深层次发展，为未来研究提供标准化评测平台。
source: AAAI-2026-Accepted
selection_source: conference_retrieval
motivation: 现有基准仅关注物体类别，未涵盖细粒度属性等广义知识，无法全面评估模型。
method: 定义广义开放词汇三维场景理解任务，包含多样化语言查询，构建OpenScan基准。
result: OpenScan提供包括属性级查询在内的评估，揭示了现有模型的局限性。
conclusion: OpenScan推进了开放词汇三维场景理解的全面评估，激励更深入的模型设计。
---

## Abstract
Open-vocabulary 3D scene understanding (OV-3D) aims to localize and classify novel objects beyond the closed set of object classes. However, existing approaches and benchmarks primarily focus on the open vocabulary problem within the context of object classes, which is insufficient in providing a holistic evaluation to what extent a model understands the 3D scene. In this paper, we introduce a more challenging task called Generalized Open-Vocabulary 3D Scene Understanding (GOV-3D) to explore the open vocabulary problem beyond object classes. It encompasses an open and diverse set of generalized knowledge, expressed as linguistic queries of fine-grained and object-specific attributes. To this end, we contribute a new benchmark named OpenScan, which consists of 3D object attributes across eight representative linguistic aspects, including affordance, property, and material. We further evaluate state-of-the-art OV-3D methods on our OpenScan benchmark and discover that these methods struggle to comprehend the abstract vocabularies of the GOV-3D task, a challenge that cannot be addressed simply by scaling up object classes during training. We highlight the limitations of existing methodologies and explore promising directions to overcome the identified shortcomings.

---

## 论文详细总结（自动生成）

# 论文总结：OpenScan: 广义开放词汇三维场景理解基准

## 1. 论文的核心问题与整体含义（研究动机和背景）
现有开放词汇三维场景理解（Open-Vocabulary 3D Scene Understanding, OV-3D）主要聚焦于识别训练集中未出现的**物体类别**（object classes），缺乏对物体细粒度属性（如功能、材质、属性等）的泛化能力。然而，真正的场景理解需要模型能够通过自然语言查询理解物体的抽象属性（例如“用于坐的”、“柔软的”、“木制的”）。目前缺乏能够评估这一能力的标准化基准。为此，作者提出**广义开放词汇三维场景理解（GOV-3D）**任务，并构建了第一个覆盖八类语言属性的3D属性基准**OpenScan**，系统评估现有OV-3D模型在抽象属性上的表现。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **任务定义**：在经典OV-3D基础上，将查询从物体类别扩展到物体属性（attribute）。输入为3D点云场景 \( P \) 和文本查询 \( q \)，输出为对应对象的3D掩码。属性集 \( A = \{a_k\} \) 包含多种抽象描述。
- **OpenScan构建流程**：
  1. **知识图谱关联**：利用ConceptNet知识图谱，为每个物体类别关联关系知识（如“床” → “用于睡眠”）。
  2. **属性选择**：对同一关系保留最高权重的关系，形成候选属性集。
  3. **人工标注**：对视觉属性（如材质）进行人工标注，补充知识图谱无法覆盖的视觉信息。
  4. **属性分类与验证**：将属性归入八类语言方面（Affordance, Property, Type, Manner, Synonym, Requirement, Element, Material），并手动筛选去重，最终保留341个属性。
  5. **查询生成**：隐藏物体类别名，使用“this term”代替，拼接关系词和属性构成查询（如“this term is used for sleep”）。
- **数据规模**：基于ScanNet200的1,513个场景，共153,644个属性标注，每个对象平均有3.15个属性。

## 3. 实验设计：数据集、基准、对比方法
- **基准**：OpenScan（验证集），包含341个属性、八类语言方面。
- **对比方法**：
  - **3D实例分割**：OpenMask3D, SAI3D, MaskClustering, Open3DIS。
  - **3D语义分割**：OpenScene, PLA, RegionPLC。
- **评估指标**：
  - 实例分割：AP（AP25, AP50, 均值AP）。
  - 语义分割：mIoU, mAcc。
- **实验设置**：所有方法在零样本（zero-shot）设置下评估，未在OpenScan上训练。对比了在ScanNet200（物体类别）上的结果。

## 4. 资源与算力
论文明确提到：**所有实验在一块NVIDIA RTX 4090 GPU上完成**。未提及训练时长、多卡并行或具体训练轮数等信息（因为零样本评估，无需训练）。

## 5. 实验数量与充分性
论文进行了以下实验：
- **主实验**：3D实例分割（4个方法×8个属性方面×3个指标）和3D语义分割（3个方法×8个方面×2个指标），共超过400个结果。
- **消融/分析实验**：
  - 预训练词汇量大小对性能的影响（使用RegionPLC在5种词汇量下对8个属性方面进行mIoU和mAcc评估）。
  - 查询形式的影响（对比“this term is made of wood”模板 vs 纯词“wood”）。
- **定性分析**：提供了Open3DIS的失败案例和CLIP相似度分析。
实验设计覆盖了主要方法，消融实验合理，但未进行更细致的组件消融（如不同CLIP版本、不同知识图谱来源等）。整体充分、客观，公平性良好（所有方法零样本、同一硬件）。

## 6. 论文的主要结论与发现
- 现有OV-3D模型在GOV-3D任务上的表现**远低于**在物体类别上的表现：例如Open3DIS在ScanNet200上AP均值为23.7，在OpenScan上仅15.8。
- 不同属性方面表现差异：**Synonym**（同义词）和**Material**（材质）相对较好，**Affordance**（功能）和**Property**（属性）最差。
- **增加预训练词汇量不能有效提升抽象属性理解**，因为现有训练数据缺乏属性标注。
- **使用查询模板**（包含关系词）比纯属性词更有助于模型理解，说明模型依赖常识知识。
- 模型在视觉属性（材质）上表现较好，归因于CLIP对视觉模式的预训练；在需要常识推理的属性上表现差。
- 简单迁移OV-3D技术无法解决GOV-3D的挑战，未来需将属性知识注入VLM。

## 7. 优点：方法或实验设计上的亮点
- **任务创新**：首次提出超越物体类别的广义开放词汇3D理解任务，填补了评估维度空白。
- **基准构建全面**：覆盖8个语言方面、341个属性、15万+标注，并融合知识图谱自动标注与人工验证，确保质量和多样性。
- **评估全面**：同时评估语义分割和实例分割，并分析不同属性方面、查询形式、词汇规模的影响，揭示了模型在抽象属性上的根本局限。
- **公开资源**：代码、数据集、扩展版本均已开源（https://youjunzhao.github.io/OpenScan/）。

## 8. 不足与局限
- **属性来源偏倚**：大部分属性来自ConceptNet知识图谱，受其覆盖范围和关系质量限制；材质等视觉属性依赖人工标注，可能引入主观性。
- **查询设计局限**：查询模板固定为“this term + relation + attribute”，可能遗漏更自然的语言表达；未考虑否定、多属性组合等复杂查询。
- **实验覆盖不足**：
  - 仅评估了零样本场景，未探索微调或few-shot设置。
  - 仅测试了4种实例分割和3种语义分割方法，未涉及最新的2D-3D融合模型或大语言模型驱动的3D理解方法。
  - 未对属性内部类别平衡性进行分析（某些方面如Manner仅21个属性，Material仅10个，可能影响结果可靠性）。
- **算力限制**：仅使用单卡RTX 4090，未评估在更大规模场景或更高分辨率下的性能。
- **应用范围**：仅限室内场景（ScanNet200），未扩展到室外或复杂开放场景。

（完）
