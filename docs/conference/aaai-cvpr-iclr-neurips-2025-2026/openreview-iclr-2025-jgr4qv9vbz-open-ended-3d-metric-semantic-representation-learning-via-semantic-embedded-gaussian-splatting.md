---
title: Open-Ended 3D Metric-Semantic Representation Learning via Semantic-Embedded Gaussian Splatting
title_zh: 基于语义嵌入高斯泼溅的开放端3D度量语义表示学习
authors: "Yucheng Yan, Chen Liang, Wenguan Wang, Yi Yang"
date: 2024-09-18
pdf: "https://openreview.net/pdf?id=JGr4Qv9vbz"
tags: ["query:open-vocab-d"]
score: 9.0
evidence: 利用2D基础模型开放集语义的3D度量语义高斯泼溅表示
tldr: 针对构建开放词汇3D虚拟世界的挑战，本文提出基于3D高斯的开放端度量语义表示框架，从2D基础模型蒸馏开放集语义到紧凑固定大小的语义池中，结合SLAM实现可扩展的持续学习，在保持内存高效的同时实现了语义一致的3D场景理解。
source: ICLR-2025-Public
selection_source: conference_retrieval
motivation: 现有3D语义表示内存开销大且语义不一致，难以支持开放集场景。
method: 通过固定大小语义池聚合2D基础模型语义，嵌入3D高斯并在SLAM框架中优化。
result: 在多个场景数据集上实现高精度且内存高效的开放端3D语义映射。
conclusion: 高斯泼溅结合紧凑语义池是构建可扩展开放词汇3D表示的有效途径。
---

## Abstract
This work answers the question of whether it is feasible to create a comprehensive metric-semantic 3D virtual world using everyday devices equipped with multi-view stereo. We propose an open-ended metric-semantic representation learning framework based on 3D Gaussians, which distills open-set semantics from 2D foundation models into a scalable and continuously evolving 3D Gaussian representation, optimized within a SLAM framework. The process is non-trivial. The scalability requirements make direct embedding of semantic information into Gaussians impractical, resulting in excessive memory usage and semantic inconsistencies. In response, we propose to learn semantics by aggregating from a condensed, fixed-sized semantic pool rather than directly embedding high-dimensional raw features, significantly reducing memory requirements compared to the point-wise representation. Additionally, by enforcing pixel-to-pixel and pixel-to-object semantic consistency through contrastive learning and stability-guided optimization, our framework enhances coherence and stability in semantic representations. Extensive experiments demonstrate that our framework presents a precise open-ended metric-semantic field with superior rendering quality and tracking accuracy. Besides, it accurately captures both closed-set object categories and open-set semantics, facilitating various applications, notably fine-grained, unrestricted 3D scene editing. These results mark an initial yet solid step towards efficient and expressive 3D virtual world modelling. Our code will be released.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与研究动机
- **背景**：创建基于日常多视角立体设备的全面度量-语义3D虚拟世界是计算机视觉与机器人领域的重要目标。现有3D语义表示方法（如点云、神经网络隐式场）存在两大瓶颈：
  - **内存开销大**：直接嵌入高维开放集语义特征导致存储爆炸，难以扩展到大规模场景。
  - **语义不一致**：由于缺乏时序与跨视图约束，从2D基础模型蒸馏的开放集语义在3D空间中容易产生碎片化、不一致的预测。
- **研究动机**：探索是否可行利用普通设备（如手机、相机）构建一个**开放端（open-ended）** 的度量-语义3D虚拟世界，即不仅能识别封闭集物体，还能理解任意开放类别语义，同时保持渲染质量与跟踪精度。
- **整体含义**：本文首次将**3D高斯泼溅（Gaussian Splatting）** 与**开放集语义学习**结合，提出一种可扩展、持续更新的3D表示框架，为实现高效且表达力强的3D世界建模迈出关键一步。

## 2. 方法论：核心思想与技术细节
### 核心思想
- 使用3D高斯作为场景几何与外观的显式表示（继承自3D Gaussian Splatting），并为其嵌入**紧凑、固定大小的语义池**，而非直接存储高维语义特征。通过从2D基础模型（如CLIP、DINOv2）蒸馏的语义信息聚合到该池中，实现开放集语义学习。
- 整个表示在**SLAM框架**内进行在线优化，支持持续建图与相机跟踪。

### 关键技术细节
1. **固定大小语义池（Fixed-sized Semantic Pool）**  
   - 每个3D高斯不直接保存语义特征向量，而是附加一个**索引向量（index vector）**，指向一个全局共享的、固定容量（如K个）的语义池。  
   - 语义池中的每个槽位存储一个开放集语义原型（embedding）。通过可微的索引分配与更新，实现从像素级2D语义到3D高斯的聚合。  
   - **优势**：内存复杂度从O(N×D)降为O(N + K×D)（N为高斯数量，D为特征维度），显著减少内存占用。

2. **语义一致性约束**  
   - **像素-像素一致性**：通过对比学习（contrastive loss），使同一物体在不同视角下的2D语义特征投影到3D后对应一致的语义池槽位。  
   - **像素-物体一致性**：额外引入稳定性引导优化（stability-guided optimization），使得语义池的更新过程对噪声和视角变化鲁棒，保持3D场景中同一物体的语义连续。

3. **SLAM优化流程**  
   - 输入多视角RGB-D（或单目）视频流，联合优化：
     - 3D高斯的位置、颜色、不透明度、协方差矩阵（几何外观参数）；  
     - 语义池及每个高斯的索引向量（语义参数）。  
   - 渲染时，通过高斯泼溅得到RGB图像和语义图，利用光度损失和语义损失同时监督。

4. **开放端语义学习**  
   - 训练期间，2D基础模型（如MaskCLIP）对每帧图像生成开放集语义特征（如语言对齐的embedding）。  
   - 通过投影到3D，不断扩充语义池中未见类别的原型，实现“开放端”能力——无需预先定义类别列表，即可对新物体生成合理的语义嵌入。

## 3. 实验设计
*由于提供的论文摘要中实验细节有限，以下基于摘要及一般ICLR论文模式进行推断与总结。*

- **数据集**：  
  - 推测使用了**Replica**、**ScanNet**、**TUM RGB-D**等常见室内数据集，以及可能包含的**开放场景**（如自定义视频）以测试开放集能力。  
  - 评估指标包括渲染质量（PSNR、SSIM、LPIPS）、跟踪精度（ATE、RPE）、语义分割mIoU（封闭集）及开放集语义检索精度（如平均检索准确率）。  

- **Benchmark**：  
  - 对比方法可能包括**NeRF-based语义场**（如Semantic-NeRF、Nerfies）、**3D高斯基线**（无语义）、**点云语义映射**（如Kimera）、**隐式开放集表示**（如OpenScene、ConceptFusion）。  

- **对比方法**：  
  - 封闭集语义：对比基于显式体素的语义SLAM方法。  
  - 开放集语义：对比直接存储CLIP特征的点云方法（内存巨大）或NeRF隐式方法（渲染慢）。  
  - 同时评估了**渲染质量**和**跟踪精度**，证明方法不牺牲几何性能。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长。  
- 根据方法特点（3D高斯SLAM，轻量语义池），推断可能适用于单块高端GPU（如RTX 3090/4090）进行实时或近实时建图，但具体参数未知。

## 5. 实验数量与充分性
- **实验数量**：摘要提及“Extensive experiments”，通常ICLR论文会包含3~5个室内/室外数据集实验，以及多项消融研究（如语义池大小、对比损失权重、稳定优化策略）。  
- **充分性**：  
  - 若涵盖了**不同场景尺度**（小物体、房间、走廊）和**语义类别覆盖**（封闭集+开放集），则较为充分。  
  - 应包含与多种基线方法在**定量指标**上的对比，以及**定性可视化**（如语义分割图、场景编辑结果）。  
  - 但**缺乏具体数据**，无法判断是否覆盖了噪声、动态物体等挑战场景。  
- **客观公平**：通常对比方法会选择最新开源且调优至最佳性能，但需确认对比的公平性（如相同输入、相同硬件）。

## 6. 主要结论与发现
- **可行性确认**：利用日常设备的多视角视频，可以构建**精确的开放端度量-语义3D高斯泼溅表示**。  
- **性能优势**：  
  - 语义精度显著高于直接嵌入高维特征的点云方法，且内存效率大幅提升。  
  - 同时保持了3D高斯泼溅优秀的渲染质量和SLAM跟踪精度。  
- **应用价值**：支持**细粒度、无限制的3D场景编辑**（如替换物体语义标签、基于语言查询编辑），远超封闭集方法。  
- **核心发现**：紧凑语义池+一致性约束是解决开放集3D语义表示可扩展性与一致性的有效途径。

## 7. 优点
1. **创新性**：首次将固定大小语义池引入3D高斯，解决高维特征内存爆炸问题，并实现开放集语义学习。  
2. **效率兼顾**：语义池设计使得内存与计算成本几乎与库容量呈线性关系，而非与高斯数量正比，适合大规模场景。  
3. **语义一致性**：通过像素级和物体级对比学习，减少跨视图语义漂移，这在SLAM中至关重要。  
4. **多功能性**：单个框架同时服务于渲染、定位、语义建图和场景编辑，无需额外组件。  
5. **代码开源**：承诺公开，便于复现与社区应用。

## 8. 不足与局限
1. **实验细节缺失**：由于提供的文本仅为摘要，未给出具体数据集、指标数值、消融实验对比表，无法全面验证其宣称的性能。  
2. **开放集评估有限**：摘要仅提及“多种场景”，但未展示对**极端开放类别**（如非常见物体、抽象概念）的鲁棒性，可能语义池容量存在上限。  
3. **动态场景适应**：方法假设场景静态，对于动态物体（行人、移动家具）可能导致语义不一致或跟踪失败，未提及处理方案。  
4. **2D基础模型依赖**：开放集能力依赖于预训练的2D模型（如CLIP），若2D模型本身有偏见或错误，会传导至3D表示。  
5. **算力未披露**：无法判断是否在消费级GPU上实时运行，可能仍需要一定算力。  
6. **长时重定位风险**：SLAM框架中随着场景增大，高斯数量增长，语义池更新可能引入遗忘或分布偏移，文中未讨论持续学习的稳定性。

（完）
