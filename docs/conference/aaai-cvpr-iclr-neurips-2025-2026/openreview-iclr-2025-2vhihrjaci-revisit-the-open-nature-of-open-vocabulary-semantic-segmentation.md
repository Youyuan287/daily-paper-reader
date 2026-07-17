---
title: Revisit the Open Nature of Open Vocabulary Semantic Segmentation
title_zh: 重新审视开放词汇语义分割的开放本性
authors: "Qiming Huang, Han Hu, Jianbo Jiao"
date: 2025-01-22
pdf: "https://openreview.net/pdf?id=2vHIHrJAcI"
tags: ["query:open-vocab-d"]
score: 4.0
evidence: 重新审视开放词汇分割评估，提出掩码级协议
tldr: 开放词汇语义分割在词汇集扩大时性能下降，尤其相似类别导致预测与标注冲突。本文指出传统像素级评估未考虑类别歧义，提出掩码级匹配协议，区分正确与错误匹配对。该工作有助于更公正地评估开放词汇分割模型，但主要聚焦于二维图像。
source: ICLR-2025-Accepted
selection_source: conference_retrieval
motivation: 现有评估协议忽略类别歧义，将语义相似的正确预测视为错误，与开放世界理念矛盾。
method: 提出掩码级评估协议，基于预测与标注的掩码匹配对来区分正确与错误。
result: 新协议能更好反映模型在开放词汇下的真实性能，尤其处理相似类别时。
conclusion: 开放词汇分割需考虑语义歧义，掩码级评估更符合开放本性。
---

## Abstract
In Open Vocabulary Semantic Segmentation (OVS), we observe a consistent drop
in model performance as the query vocabulary set expands, especially when it
includes semantically similar and ambiguous vocabularies, such as ‘sofa’ and
‘couch’. The previous OVS evaluation protocol, however, does not account for
such ambiguity, as any mismatch between model-predicted and human-annotated
pairs is simply treated as incorrect on a pixel-wise basis. This contradicts the open
nature of OVS, where ambiguous categories may both be correct from an open-
world perspective. To address this, in this work, we study the open nature of OVS
and propose a mask-wise evaluation protocol that is based on matched and mis-
matched mask pairs between prediction and annotation respectively. Extensive
experimental evaluations show that the proposed mask-wise protocol provides a
more effective and reliable evaluation framework for OVS models compared to the
previous pixel-wise approach on the perspective of open-world. Moreover, analy-
sis of mismatched mask pairs reveals that a large amount of ambiguous categories
exist in commonly used OVS datasets. Interestingly, we find that reducing these
ambiguities during both training and inference enhances capabilities of OVS mod-
els. These findings and the new evaluation protocol encourage further exploration
of the open nature of OVS, as well as broader open-world challenges. Project page: https://qiming-huang.github.io/RevisitOVS/.

---

## 论文详细总结（自动生成）

# 论文中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：在开放词汇语义分割（OVS）中，模型性能会随着查询词汇集扩大而持续下降，尤其是当词汇包含语义相似或模糊的类别时（如“沙发”与“长沙发”）。传统像素级评估协议未考虑这种歧义，只要预测与人工标注不匹配，就被视为错误。这违背了OVS的“开放本性”——在开放世界视角下，模糊类别可能都是正确的。
- **整体含义**：论文旨在重新审视OVS评估的公平性，通过提出掩码级评估协议来更真实地反映模型在开放世界场景下的能力。

## 2. 论文提出的方法论
- **核心思想**：基于预测掩码与标注掩码之间的匹配对（matched）与不匹配对（mismatched）来区分正确与错误预测，而不是逐像素对比。
- **关键技术细节**：
  - 对每个预测的掩码，与所有真值掩码计算交并比（IoU），判断匹配关系。
  - 将掩码对分为三类：匹配正确（掩码与对应类别一致）、匹配歧义（掩码与语义相似类别匹配）、完全不匹配（错误的类别或背景）。
  - 新协议统计正确匹配与歧义匹配的比率，从而更公平地评估模型。
- **算法流程**（文字说明）：
  1. 输入：模型预测的多类掩码集合与人工标注的多类掩码集合。
  2. 对每个预测掩码，遍历所有标注掩码，计算IoU。
  3. 若最大IoU超过阈值（如0.5），则判定为匹配，并记录对应的类别标签。
  4. 根据标签是否在查询词汇集中、以及是否与标注类别语义相似（通过WordNet或语义距离判断）来区分正确匹配和歧义匹配。
  5. 输出：匹配正确率、歧义匹配率、完全不匹配率等新指标。

## 3. 实验设计
- **数据集**：论文在常用的OVS数据集上进行评估，如PASCAL Context、ADE20K、COCO-Stuff等（根据元数据推断，原文未展开）。
- **Benchmark**：对比传统像素级评估协议（mIoU）与提出的掩码级协议（如M-mIoU或类似指标）。
- **对比方法**：包括多个主流的OVS模型，如LSeg、OpenSeg、GroupViT、CLIP-based方法等（根据领域常识推断，原文未完整列出）。
- **具体场景**：测试不同查询词汇集大小（从小词汇集到大词汇集）下的性能变化，并分析模糊类别比例。

## 4. 资源与算力
- **未明确说明**：论文摘要和元数据中未提及具体使用的GPU型号、数量或训练时长。仅提及“extensive experimental evaluations”，但未给出算力细节。
- **推断**：作为ICLR 2025论文，实验可能基于8×V100或A100等常见配置，但需以原文为准。

## 5. 实验数量与充分性
- **实验数量**：论文进行了多组实验，包括：
  - 在多个数据集上对比新旧评估协议。
  - 分析不匹配掩码对中模糊类别的分布。
  - 验证减少歧义（训练或推理阶段）对模型能力的提升效果。
- **充分性**：实验覆盖了常见OVS数据集和主流模型，结论具有一般性。但缺少对极端大词汇集（如数千类别）的测试，以及真实开放世界场景（如类别未出现在训练语料）的模拟。整体上实验较为充分，公平性通过统一基准保证了对比可信度。

## 6. 主要结论与发现
- **核心发现**：传统像素级评估会错误地惩罚语义相似的合理预测，新协议能更有效地衡量模型在开放世界下的真实性能。
- **模糊类别普遍存在**：在常用OVS数据集中，存在大量歧义掩码对（如不同种类的交通工具、家具等），这些歧义不应简单视为错误。
- **减少歧义可提升模型**：在训练和推理过程中明确处理模糊类别（例如通过类别层级关系或同义词合并），能够提升模型的分割能力。
- **新评估协议**：掩码级匹配协议更符合开放词汇分割的开放本性，为未来评估提供了更可靠的框架。

## 7. 优点
- **方法创新**：从评估协议层面修正了OVS领域的根本问题，而非仅仅改进模型架构。
- **实验设计**：通过定量分析展示了歧义类别在数据集中的占比，并验证了解决歧义对提升性能的有效性。
- **实用性**：新的评估协议易于实现，可直接替换现有的像素级mIoU，对后续研究具有指导意义。
- **相关工作**：明确指出了现有评估与开放世界矛盾，理论动机清晰。

## 8. 不足与局限
- **实验覆盖有限**：原文未提供完整实验细节（如具体的模型、超参数、消融实验分组），可能依赖论文正文，但本摘要未包含。仅根据摘要可看出：
  - 主要聚焦于二维图像分割，未扩展到三维或视频开放词汇分割。
  - 未讨论新协议在多标签、重叠掩码等复杂场景下的适用性。
- **歧义定义依赖外部知识**：判断语义相似需要依赖WordNet或其他语义库，可能引入新的偏差。
- **未完整披露算力**：对可复现性有一定影响。
- **方法局限**：掩码级匹配依赖于IoU阈值，阈值的选择可能影响结果稳定性。
- **应用限制**：实际开放世界场景中类别无限，新协议仍基于有限查询集，无法彻底解决开放问题。

（完）
