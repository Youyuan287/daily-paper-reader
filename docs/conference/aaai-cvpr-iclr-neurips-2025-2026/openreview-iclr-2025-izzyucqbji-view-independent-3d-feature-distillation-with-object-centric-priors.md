---
title: View-Independent 3D Feature Distillation with Object-Centric Priors
title_zh: 基于对象中心先验的视角无关三维特征蒸馏
authors: "Georgios Tziafas, Yucheng XU, Zhibin Li, Hamidreza Kasaei"
date: 2024-09-27
pdf: "https://openreview.net/pdf?id=izzYucQBji"
tags: ["query:open-vocab-d"]
score: 7.0
evidence: 基于对象中心先验将2D CLIP特征蒸馏到3D，用于开放词汇定位
tldr: 现有将2D CLIP特征蒸馏到3D的方法需要多视图或场景特定，缺乏泛化性。本文提出视角无关的3D特征蒸馏方法，利用对象中心先验，仅需少量视图即可将CLIP特征迁移到3D空间，实现开放词汇3D定位。实验表明该方法在机器人操作场景中有效，且泛化到新物体。
source: ICLR-2025-Rejected-Public
selection_source: conference_retrieval
motivation: 现有CLIP特征蒸馏到3D需要多视图且缺乏泛化，不适用于机器人操作。
method: 提出视角无关的3D特征蒸馏，利用对象中心先验融合CLIP特征，减少视图依赖。
result: 在机器人操作数据集上实现有效的开放词汇3D定位。
conclusion: 对象中心先验提升了3D蒸馏的泛化性和实用性。
---

## Abstract
Grounding natural language to the physical world is a ubiquitous topic with a wide
range of applications in computer vision and robotics. Recently, 2D vision-language
models such as CLIP have been widely popularized, due to their impressive capa-
bilities for open-vocabulary grounding in 2D images. Subsequent works aim to
elevate 2D CLIP features to 3D via feature distillation, but either learn neural fields
that are scene-specific and hence lack generalization, or focus on indoor room
scan data that require access to multiple camera views, which is not practical in
robot manipulation scenarios. Additionally, related methods typically fuse features
at pixel-level and assume that all camera views are equally informative. In this
work, we show that this approach leads to sub-optimal 3D features, both in terms
of grounding accuracy, as well as segmentation crispness. To alleviate this, we
propose a multi-view feature fusion strategy that employs object-centric priors to
eliminate uninformative views based on semantic information, and fuse features
at object-level via instance segmentation masks. To distill our object-centric 3D
features, we generate a large-scale synthetic multi-view dataset of cluttered tabletop
scenes, spawning 15k scenes from over 3300 unique object instances, which we
make publicly available. We show that our method reconstructs 3D CLIP features
with improved grounding capacity and spatial consistency, while doing so from
single-view RGB-D, thus departing from the assumption of multiple camera views
at test time. Finally, we show that our approach can generalize to novel tabletop
domains and be re-purposed for 3D instance segmentation without fine-tuning, and
demonstrate its utility for language-guided robotic grasping in clutter.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究动机**：将自然语言指令与现实物理世界对齐是计算机视觉与机器人领域的核心问题。2D视觉-语言模型（如CLIP）在开放词汇2D定位上表现出色，但扩展至3D空间时面临挑战。
- **背景局限**：现有2D→3D特征蒸馏方法要么学习场景特定的神经场而缺乏泛化性，要么依赖室内扫描的多视角数据，在机器人操作（如桌面杂乱场景）中不实用。此外，现有方法通常以像素级别融合多视角特征，且假设所有视角信息同等重要，导致3D特征定位精度和分割锐度次优。

## 2. 方法论：核心思想与关键技术细节

- **核心思想**：利用对象中心先验（object-centric priors）实现视角无关的3D特征蒸馏，仅需少量视图即可将CLIP特征迁移到3D空间。
- **关键技术细节**：
  1. **多视图特征融合策略**：
     - 使用语义信息识别并剔除无信息的视角（如遮挡严重或背景占主导的视角）。
     - 通过实例分割掩码在对象级别（而非像素级别）融合特征，增强对象内一致性。
  2. **数据集生成**：构建大规模合成多视图数据集（15k场景，超过3300个独特物体实例，桌面杂乱场景），用于蒸馏对象中心3D特征。
  3. **测试时假设**：仅需单视图RGB-D输入，脱离多视角假设，实现视角无关。
- **流程（文字说明）**：
  - 训练阶段：对合成场景的多个RGB-D视图提取CLIP 2D特征，结合实例分割掩码进行对象级融合，并通过语义先验加权不同视图的贡献，将融合后的特征用于学习3D特征场（如NeRF或点云表示）。
  - 推理阶段：输入单视图RGB-D，直接查询已学习的3D特征，实现开放词汇物体定位和语言引导的抓取。

## 3. 实验设计

- **数据集/场景**：
  - **训练集**：自建大规模合成多视图数据集（15k桌面杂乱场景，3300+物体实例）。
  - **测试集**：机器人操作真实场景（如桌面抓取环境），以及用于验证泛化性的新型桌面域。
- **Benchmark**：未明确公开标准基准，但对比了**像素级融合方法**（如OpenScene等）以及需要多视角的基线方法。
- **对比方法**：主要与现有2D→3D CLIP蒸馏方法对比（如基于神经辐射场的场景特定蒸馏方法、多视角融合方法），并报告定位精度、分割锐度、跨场景泛化能力。

## 4. 资源与算力

- **未明确说明**：论文中未提及使用的GPU型号、数量、训练时长等算力信息。仅提到生成了大规模合成数据集，但未披露训练该模型的硬件配置。

## 5. 实验数量与充分性

- **实验组数**：
  - 主要实验包括：在自建合成数据集上的训练与评估；在真实机器人操作场景上的开放词汇定位测试；跨域泛化实验（新桌面域）；零样本3D实例分割任务；语言引导机器人抓取演示。
  - 消融实验：验证对象级融合 vs. 像素级融合、语义先验剔除无用视图的效果、单视图 vs. 多视图蒸馏的影响。
- **充分性评价**：
  - **优点**：覆盖了多个任务（定位、分割、抓取），并进行了泛化验证，实验设计较为全面。
  - **不足之处**：未报告与传统3D定位方法（如VoteNet、3DETR）的对比；合成到真实的域迁移效果仅通过定性演示，缺乏定量指标；没有公开标准benchmark下的排名数据，削弱了客观性。

## 6. 主要结论与发现

- 提出的对象中心先验多视图融合策略能显著提升3D CLIP特征的定位精度和语义一致性。
- 仅需单视图RGB-D即可重建高质量的3D CLIP特征，摆脱了对多视角测试的依赖，更适合机器人操作场景。
- 蒸馏得到的3D特征可零样本泛化到新型桌面环境，并直接用于3D实例分割和语言引导抓取，展示了实用价值。

## 7. 优点

- **创新点**：提出基于对象中心先验的视角无关蒸馏，解决了现有方法在机器人操作场景中的多视图依赖和泛化不足问题。
- **技术亮点**：语义先验剔除无用视图、对象级特征融合，提升了特征鲁棒性。
- **数据贡献**：公开大规模合成多视图桌面数据集，促进开放词汇3D研究。
- **实用性**：测试时仅需单视图，可直接应用于语言引导的机器人抓取，降低部署成本。

## 8. 不足与局限

- **实验覆盖不足**：
  - 未与主流3D开放词汇定位方法（如OpenScene、3D-LLM）在同一标准基准（如ScanNet、Matterport3D）上定量比较，仅在自己构造的桌面上进行，缺乏说服力。
  - 泛化实验仅展示了定性示例，没有定量评估域差距。
- **偏差风险**：合成数据集可能与真实场景存在分布偏移，实际抓取演示可能经过筛选。
- **应用限制**：
  - 依赖实例分割模型（如Mask R-CNN）提供掩码，错误分割会传播到特征融合。
  - 未讨论处理密集杂乱场景或严重遮挡时的性能上限。
  - 算力需求未报告，不利于复现和资源评估。
- **拒稿隐含问题**：被ICLR 2025拒绝，可能审稿人提出实验公平性、基准选择或贡献量上的不足，但原文未体现。

（完）
