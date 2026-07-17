---
title: "SPC-GS: Gaussian Splatting with Semantic-Prompt Consistency for Indoor Open-World Free-view Synthesis from Sparse Inputs"
title_zh: SPC-GS：基于语义提示一致性的高斯泼溅用于室内开放世界稀疏输入自由视角合成
authors: "Liao, Guibiao, Li, Qing, Bao, Zhenyu, Qiu, Guoping, Liu, Kanglin"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Liao_SPC-GS_Gaussian_Splatting_with_Semantic-Prompt_Consistency_for_Indoor_Open-World_Free-view_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 8.0
evidence: 语义提示一致性正则化用于开放世界高斯泼溅
tldr: 针对高斯泼溅在稀疏输入下表现差的问题，提出SPC-GS。通过视频生成模型生成视角变换图像初始化密集高斯分布，并引入语义提示一致性正则化。尽管主要任务是新视角合成，但其语义提示一致性方法可提升3D高斯字段的语义质量，间接支持开放词汇特征场。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1772, \"height\": 607, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1795, \"height\": 1079, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 863, \"height\": 291, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1739, \"height\": 454, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 1744, \"height\": 550, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 852, \"height\": 341, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/fig-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 848, \"height\": 593, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 1766, \"height\": 425, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 872, \"height\": 276, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 868, \"height\": 203, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-liao-spc-gs-gaussian-splatting-with-semantic-prompt-consistency-for-indoor-open-world-free-view-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 870, \"height\": 171, \"label\": \"Table\"}]"
motivation: 现有高斯泼溅方法在稀疏输入下因高斯点分布稀疏和视角监督不足而性能下降。
method: 利用视频生成模型初始化场景布局高斯分布，并加入语义提示一致性正则化。
result: 在稀疏输入设置下，SPC-GS实现了更好的开放世界自由视角合成质量。
conclusion: 语义提示一致性能有效改善高斯泼溅的泛化能力，适用于开放场景。
---

## Abstract
3D Gaussian Splatting-based indoor open-world free-view synthesis approaches have shown significant performance with dense input images. However, they exhibit poor performance when confronted with sparse inputs, primarily due to the sparse distribution of Gaussian points and insufficient view supervision. To relieve these challenges, we propose SPC-GS, leveraging Scene-layout-based Gaussian Initialization (SGI) and Semantic-Prompt Consistency (SPC) Regularization for open-world free view synthesis with sparse inputs. Specifically, SGI provides a dense, scene-layout-based Gaussian distribution by utilizing view-changed images generated from the video generation model and view-constraint Gaussian points densification. Additionally, SPC mitigates limited view supervision by employing semantic-prompt-based consistency constraints developed by SAM2. This approach leverages available semantics from training views, serving as instructive prompts, to optimize visually overlapping regions in novel views with 2D and 3D consistency constraints. Extensive experiments demonstrate the superior performance of SPC-GS across Replica and ScanNet benchmarks. Notably, our SPC-GS achieves a 3.06 dB gain in PSNR for reconstruction quality and a 7.3% improvement in mIoU for open-world semantic segmentation.

---

## 论文详细总结（自动生成）

# SPC-GS：基于语义提示一致性的高斯泼溅用于室内开放世界稀疏输入自由视角合成——详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有的 3D Gaussian Splatting（3DGS）方法在密集输入图像下可实现高质量的开放世界自由视角合成，但在稀疏输入（如仅 12 张图像）时性能急剧下降。
- **根本原因**：
  - 高斯点分布稀疏：使用 SfM 从稀疏视图初始化的点数量极少且分布不均匀，难以覆盖复杂室内场景。
  - 视角监督不足：仅有少量训练视图的约束导致模型过拟合训练视图，对未观察区域的渲染产生语义模糊。
- **研究意义**：解决室内“由内向外”视角配置下、稀疏输入的开放世界自由视角合成问题，对增强/虚拟现实、机器人导航等应用至关重要。

## 2. 论文提出的方法论

### 核心思想
提出 SPC-GS 框架，包含两个关键模块：
- **Scene-layout-based Gaussian Initialization (SGI)**：生成稠密且具有场景布局指导性的高斯点初始化。
- **Semantic-Prompt Consistency (SPC) Regularization**：利用训练视图的语义作为提示，对伪视图施加 2D 和 3D 语义一致性约束。

### 关键技术细节

#### SGI（场景布局高斯初始化）
1. **VGM-based Point Creation**：使用视频生成模型 MotionCtrl 为每张训练图像生成 8 个相邻视角图像（如平移、旋转、缩放等）。将这些生成图像与原始训练图像一起输入 SfM（COLMAP），得到更稠密的 SfM 点云。
2. **Scene-layout Point Generation**：
   - 以稠密 SfM 点初始化 3DGS，通过视图约束的高斯密化生成更密集的场景布局高斯点。
   - **Outlier Gaussian Primitive Removal (OGR)**：基于密度去除离散的离群点（邻域内少于 P=5 个点且半径 r=1 的高斯被删除），避免干扰优化。

#### SPC（语义提示一致性正则化）
1. **Region Correspondence Establishment**：
   - 在训练过程中渲染伪视角图像（位置由邻近训练视角加随机噪声生成）。
   - 使用 **Iterative Stochastic Prompting (ISP)**：在训练视图中随机采样一个点作为 SAM2 的点提示，跨训练视图和伪视图生成对应的区域掩码（P=2 个伪视图）。
2. **2D 约束（L2d_SPC）**：
   - **View-to-View Regularization (L_inter)**：强制训练视图与伪视图在对应区域的特征和相关性图上一致。
   - **Contrastive Regularization (L_intra)**：在同一训练视图内，利用 SAM2 自动掩码和语义标签，对属于同一类别的特征进行对比学习，拉近正样本、推远负样本。
3. **3D 约束（L3d_SPC）**：
   - 针对对应区域中权重最大的高斯基元，鼓励其语义特征在训练视图和伪视图间相似。
   - **Region Boundary Erosion**：通过形态学腐蚀去除边界模糊像素对应的基元，只保留区域内部高置信度基元。
4. **损失函数总览**：
   ```
   L = L_C + L_S + L_GS + L_3dS + λ(L2d_SPC + L3d_SPC)
   ```
   - L_C：颜色重建损失（与 3DGS 一致）
   - L_S：语义损失（含余弦相似度和交叉熵）
   - L_GS：对生成图像的语义损失（仅用于语义，不用颜色）
   - L_3dS：3D 局部自适应正则化（鼓励邻域高斯基元语义相似）
   - λ 初始为 0，经过 4k 次迭代后线性增至 1。

## 3. 实验设计

- **数据集**：
  - **Replica**（合成室内场景）
  - **ScanNet**（真实室内场景）
- **稀疏输入设置**：从每个场景中选取 12 张均匀分布的训练图像，其余每 10 帧取一张作为测试视图（遵循 FSGS、DNGaussian 等标准协议）。
- **评估指标**：
  - 重建质量：PSNR、SSIM、LPIPS
  - 开放世界语义分割：mIoU、mAcc
- **对比方法**：
  - 开放词汇语义方法：3DOVS、Feature 3DGS、LangSplat、Gau-Grouping
  - 稀疏输入重建方法：DNGaussian、FSGS、CoR-GS
- **公平性**：所有对比方法均采用同一训练/测试划分，并使用 CLIP 特征进行语义优化，确保比较公平。对于稀疏输入重建方法，额外添加语义属性使其具备开放世界分割能力。

## 4. 资源与算力

- 论文提及使用 **NVIDIA A100 GPU**、PyTorch 框架。
- **未明确说明**使用的 GPU 数量（单卡或多卡）以及完整训练时长。从 typical 3DGS 训练效率推断，稀疏输入场景训练大概在数十分钟到数小时级别，但原文未提供具体数字。

## 5. 实验数量与充分性

- **实验总量**较多，包括：
  1. **主要定量对比**（Table 1）：在 Replica 和 ScanNet 两个数据集上，与 7 种方法对比 5 个指标。
  2. **消融实验**：
     - 关键组件消融（Table 2）：逐步添加 VGM-Point Creation、Scene-layout Point Generation、SPC（2D+3D）。
     - SPC 内部组件消融（Table 3）：去除 view-to-view、contrastive、boundary erosion。
     - 鲁棒性实验（Table 4）：使用纯 SfM 点初始化 + SGI + SPC，验证策略有效性。
  3. **可视化对比**（Figure 4-7）：展示重建和分割结果及初始化效果。
  4. **补充实验**：文中提到使用不同 CLIP 模型（如 LSeg）验证通用性（见补充材料）。
- **充分性评价**：实验设计较为全面，覆盖了不同数据集、不同方法类型、消融所有关键设计。消融实验逐步验证各模块贡献，对比方法也涵盖了现有主流方法。结论可信度高。

## 6. 论文的主要结论与发现

- SPC-GS 在稀疏输入条件下显著优于所有现有方法：
  - 重建 PSNR 提升 **3.06 dB**（平均），SSIM 和 LPIPS 均有改善。
  - 开放世界语义分割 mIoU 提升 **7.3%**（相对绝对值）。
- SGI 有效解决了高斯点稀疏问题，为后续优化提供良好起点。
- SPC 通过语义提示一致性约束大幅增强了跨视图语义一致性，缓解了监督不足。
- 方法对不同的 CLIP 模型（ViT-B/16、LSeg）均表现稳健。

## 7. 优点

- **创新性强**：首次尝试联合重建和语义理解解决室内稀疏输入开放世界问题，提出 SGI 和 SPC 两个新颖模块。
- **方法有效**：VGM 生成视角增强点云，场景布局初始化比纯 SfM 更优；语义提示一致性利用了 SAM2 强时序分割能力，实现跨视图约束。
- **设计细致**：包括区域边界侵蚀、迭代随机提示、对比学习等，增强正则化的准确性和效率。
- **实验充分**：在两个主流数据集上进行了广泛定性和定量评估，消融实验完整，验证了每个设计点的必要性。
- **代码开源**：项目网站提供代码，可复现。

## 8. 不足与局限

- **计算开销**：需要运行视频生成模型（MotionCtrl）和 SAM2，增加了前期处理和训练时间，对实时应用不友好。
- **依赖 VGM 质量**：生成图像可能存在伪影或不一致，影响 SfM 点质量，最终效果受 VGM 性能限制。
- **场景类型局限**：实验只针对室内场景（Replica、ScanNet），未在室外或复杂动态场景中验证，泛化性未知。
- **稀疏度固定**：仅测试了 12 张输入的情况，未探索更极端（如 3 张）或不同稀疏度的性能。
- **语义监督**：依赖 CLIP 特征，其开放词汇能力不足以处理超细粒度或罕见类别的分割。
- **未讨论局限**：论文未明确分析方法失败案例或潜在偏差风险（如对纹理/光照变化的敏感性）。
- **与并发工作比较**：未与最新 3DGS 方法（如 ViewCrafter、GS-LRM 等）对比，可能存在更优方案。

（完）
