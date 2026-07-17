---
title: Chain of Semantics Programming in 3D Gaussian Splatting Representation for 3D Vision Grounding
title_zh: 基于3D高斯泼溅表示的语义链编程用于三维视觉定位
authors: "Shi, Jiaxin, Xiang, Mingyue, Sun, Hao, Huang, Yixuan, Weng, Zhi"
date: 2025-06-01
pdf: "https://openaccess.thecvf.com/content/CVPR2025/papers/Shi_Chain_of_Semantics_Programming_in_3D_Gaussian_Splatting_Representation_for_CVPR_2025_paper.pdf"
tags: ["query:open-vocab-d"]
score: 6.0
evidence: 利用3DGS和LLM的零样本三维视觉定位
tldr: 三维视觉定位任务需要理解细粒度语义和空间关系。本文提出零样本神经符号框架，利用大语言模型作为神经符号函数，在3D高斯泼溅表示中定位物体。通过动态渲染多视角图像丰富语义信息，并构建关系图和语义链。实验表明该方法在未见场景中有效泛化，推动了3D视觉与语言理解的结合。
source: CVPR-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/fig-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 852, \"height\": 400, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/fig-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 1801, \"height\": 967, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/fig-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 834, \"height\": 549, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/fig-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1767, \"height\": 446, \"label\": \"Figure\"}, {\"url\": \"assets/figures/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/fig-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 852, \"height\": 721, \"label\": \"Figure\"}]"
tables_json: "[{\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-001.webp\", \"caption\": \"\", \"page\": 0, \"index\": 1, \"width\": 707, \"height\": 240, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-002.webp\", \"caption\": \"\", \"page\": 0, \"index\": 2, \"width\": 596, \"height\": 507, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-003.webp\", \"caption\": \"\", \"page\": 0, \"index\": 3, \"width\": 1629, \"height\": 523, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-004.webp\", \"caption\": \"\", \"page\": 0, \"index\": 4, \"width\": 1547, \"height\": 438, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-005.webp\", \"caption\": \"\", \"page\": 0, \"index\": 5, \"width\": 602, \"height\": 506, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-006.webp\", \"caption\": \"\", \"page\": 0, \"index\": 6, \"width\": 1559, \"height\": 366, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-007.webp\", \"caption\": \"\", \"page\": 0, \"index\": 7, \"width\": 1559, \"height\": 366, \"label\": \"Table\"}, {\"url\": \"assets/tables/cvpr-2025-accepted/cvpr-2025-shi-chain-of-semantics-programming-in-3d-gaussian-splatting-representation-for-cvpr-2025-paper/table-008.webp\", \"caption\": \"\", \"page\": 0, \"index\": 8, \"width\": 905, \"height\": 176, \"label\": \"Table\"}]"
motivation: 三维视觉定位面临语义和空间关系理解挑战，现有方法泛化性差。
method: 提出基于3DGS和LLM的零样本神经符号框架，利用关系图和语义链进行定位。
result: 实验在多个3DVG基准上取得领先结果，零样本泛化能力强。
conclusion: 该工作展示了3DGS与LLM结合在三维视觉定位中的潜力。
---

## Abstract
3D Vision Grounding (3DVG) is a fundamental research area that enables agents to perceive and interact with the 3D world. The challenge of the 3DVG task lies in understanding fine-grained semantics and spatial relationships within both the utterance and 3D scene. To address this challenge, we propose a zero-shot neuro-symbolic framework that utilizes a large language model (LLM) as neuro-symbolic functions to ground the object within the 3D Gaussian Splatting (3DGS) representation. By utilizing 3DGS representation, we can dynamically render high-quality 2D images from various viewpoints to enrich the semantic information. Given the complexity of spatial relationships, we construct a relationship graph and chain of semantics that decouple spatial relationships and facilitate step-by-step reasoning within 3DGS representation. Additionally, we employ a grounded-aware self-check mechanism to enable the LLM to reflect on its responses and mitigate the effects of ambiguity in spatial reasoning. We evaluate our method using two publicly available datasets, Nr3D and Sr3D, achieving accuracies of 60.8% and 91.4%, respectively. Notably, our method surpasses current state-of-the-art zero-shot methods on the Nr3D dataset. In addition, it outperforms the recent supervised models on the Sr3D dataset.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：3D视觉定位（3DVG）面临两大挑战：一是从语义稀疏、噪声干扰大的点云中获取细粒度语义信息；二是对文本中复杂的空间关系（如多重关系、条件性关系）进行准确推理。现有零样本方法往往将空间关系简化为二元关系，忽略了关系之间的连接和条件性，导致与监督方法存在显著性能差距。
- **研究动机**：为了提升零样本3DVG的性能，本文提出利用3D高斯泼溅（3DGS）表示动态渲染高质量2D图像，结合大语言模型（LLM）实现零样本神经符号推理，显式建模关系连接和条件性，并引入自检机制增强鲁棒性。
- **整体含义**：该工作展示了3DGS与LLM结合在3D视觉定位中的巨大潜力，为目标检测、机器人交互等下游任务提供了可行的零样本范式。

## 2. 方法论

### 核心思想
提出零样本神经符号框架，以LLM作为神经符号函数，在3DGS重建的场景中完成多步空间推理。框架分为三部分：语义解析、动态3DGS交互、语义链编程+自检。

### 关键技术细节

- **语义解析（Utterance Semantics Parsing）**：
  - 使用LLM将自然语言 utterance 解析为两个表示：
    - **关系图** \( \mathcal{G(U)} \)：顶点为物体，边为关系（如“attached”、“above”），显式建模多重关系的连接。
    - **语义链** \( \mathcal{C(U)} \)：将 utterance 拆分为多个依次依赖的推理步骤（如先找到大柜子以上的两个柜子，再选较小的，最后找带绿色便条的），显式处理条件性。
- **3DGS动态交互**：
  - **场景重建**：利用3DGS从3D点云、2D图像帧及其位姿重建场景，得到稠密、可自由渲染的表示 \( \mathcal{O}^{\text{GS}} \)。
  - **视点推断**：先用LLM过滤与 utterance 相关的3D物体信息 \( \mathcal{O}^{\text{3D}}_f \)，提取所有相关物体中心点集，PCA计算主方向，结合物体范围确定最佳相机位置 \( T'_c \)，再通过 Look-at 变换得到旋转矩阵 \( R_c \)，最终得到视点参数 \( P_c = (R_c | T'_c) \)。
  - **信息检索**：根据视点 \( P_c \) 渲染2D图像 \( \mathcal{O}^{\text{render}} \)，再用VLM（GPT-4o Vision）获取文本描述 \( \mathcal{O}^{\text{2D}} \)。
- **语义链编程**：
  - 以语义链 \( \mathcal{C(U)} \) 引导LLM生成Python代码，代码中融合 utterance、3D信息、2D信息以及关系图 \( \mathcal{G(U)} \)。执行代码后得到目标物体 ID。
- **自检机制**：
  - 用LLM评估执行结果的有效性（如返回对象数量错误、语法错误、无结果等），若无效，将错误信息 \( \mathcal{I}^{\text{err}} \) 重新注入LLM，要求重新生成代码，从而避免重复犯错。

### 算法流程（文字说明）

1. 输入：3D点云、2D图像帧及其位姿、自然语言 utterance。
2. 3DGS场景重建 → 得到 \( \mathcal{O}^{\text{GS}} \)。
3. LLM解析 utterance → 关系图 \( \mathcal{G(U)} \) 和语义链 \( \mathcal{C(U)} \)。
4. 基于 \( \mathcal{O}^{\text{3D}} \) 和 utterance，LLM过滤场景 → 视点推断 → 渲染2D图像 → VLM提取语义 → 得到 \( \mathcal{O}^{\text{2D}} \)。
5. 语义链编程：LLM生成Python代码，执行得到目标。
6. 自检：若结果无效，重复步骤5并融入错误信息。
7. 输出：目标物体ID。

## 3. 实验设计

- **数据集**：
  - **Sr3D**：83.5K 基于模板的 utterance，语言规范。
  - **Nr3D**：41.5K 自然、自由形式的 utterance，语义更复杂。
  - 两者均基于 ScanNetV2 场景。
- **Benchmark**：遵循 ReferIt3D 标准，将样本分为四类：Easy、Hard、View-Dependent、View-Independent。
- **对比方法**：
  - **监督方法**：SAT、LAR、MVT、ViL3DRel、3D-VisTA、CoT3DRef。
  - **零样本方法**：ZS3DVG、VLM-Grounder。
  - **数据高效方法**：NS3D、LARC（在不同比例训练数据下对比）。
- **实现细节**：使用 ground-truth 物体掩码，3DGS原始实现，GPT-4o作为LLM核心（同时进行VLM和编程模块）。

## 4. 资源与算力

- 论文**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。Acknowledgments 仅提及该项目获得国家大学生创新创业训练计划支持。由于是零样本框架（无需训练），主要开销来自LLM推理（调用GPT-4o API）和3DGS重建（可使用单GPU完成，但具体配置未给出）。

## 5. 实验数量与充分性

- **主要实验**：
  - 在Nr3D和Sr3D上与监督/零样本方法对比（主表）。
  - 在数据高效设置下（0.5%、5%、10% Sr3D训练数据；5%、15%、25% Nr3D训练数据）与NS3D、LARC等对比。
- **消融实验**：
  - 在Nr3D（表6）和Sr3D（表7）上分别进行6组消融：对话基线、加语义链、加编程、加3DGS、加自检，按顺序逐步叠加。
  - LLM/VLM消融：在Nr3D上比较Qwen-VL、Gemini 1.5-pro、GPT-4o。
- **定性分析**：展示5个成功/失败案例（图4）和3DGS重建质量对比（图5）。
- **充分性与公平性**：
  - 消融设计完整，覆盖了所有关键模块（CoS、Prog、GS、Check）。
  - 对比方法使用相同基准和GT mask，消融中基线（仅对话）合理。
  - 但未在更多数据集（如ARKitScenes、Multi3DRefer）上验证，泛化性证据有限。另外，所有实验使用固定的GPT-4o版本，未说明是否多次运行取平均或固定种子，可能引入随机性。

## 6. 主要结论与发现

1. **零样本SOTA**：所提方法在Nr3D上达到60.8%（总体），超过所有先前零样本方法；在Sr3D上达到91.4%，超越当前监督方法。
2. **3DGS的关键作用**：引入3DGS后，视点相关样本准确率提升14.7%（Sr3D），说明动态渲染可有效缓解视角歧义，提供细粒度2D语义。
3. **语义链的价值**：在Nr3D（自然语言）上提升明显（+1.9%～2.2%），对复杂语义理解尤其有效；在Sr3D（模板语言）上对编程模式有益（降低了Easy/Hard差距），但直接用于对话模式反而下降（可能增加理解难度）。
4. **编程的收益**：编程显著提升总体准确率（Nr3D +10.1%，Sr3D +20.4%），但在视点相关样本上因模板语言缺乏细节而下降，说明编程对逻辑性要求高但可能忽略视角模糊。
5. **自检机制的增强**：在Nr3D（+4.5%）和Sr3D（+1.8%）均有效，尤其对Hard样本提升更大（超过Easy），说明其能纠正复杂推理中的错误。
6. **跨VLM泛化性**：在不同LLM（Qwen-VL、Gemini 1.5-pro）上仍保持较好性能，尤其是Easy和View-Indep样本。

## 7. 优点

- **创新性**：首次将3DGS引入3DVG，实现了动态视点选择与高质量2D渲染，从而获得细粒度语义。
- **语义建模精细**：提出的关系图和语义链显式处理了多重关系的连接和条件性，克服了以往方法仅建模二元关系的局限。
- **零样本且灵活**：无需任何训练数据，可直接迁移到新场景；框架中LLM/VLM可替换，具备良好通用性。
- **鲁棒性设计**：自检机制使系统能自我纠正推理错误，提升了复杂场景下的成功率。
- **实验全面且开源**：在多个主流数据集上进行了充分的对比和消融，定量定性结合，验证了各模块贡献。

## 8. 不足与局限

- **算力透明度不足**：未报告具体推理成本（如每次查询的token数、3DGS重建时间），难以评估实际计算开销和可扩展性。
- **数据集局限**：仅在ScanNet场景上测试，未验证其他室内（如ARKitScenes）或室外场景，泛化性有待进一步证实。
- **视点推理的假设**：视点推断使用PCA主方向+物体范围，在物体分布不规则（散落各处）时可能产生不良视角，影响渲染质量。
- **LLM推理误差**：框架严重依赖GPT-4o的语义理解和代码生成能力，当utterance高度复杂或存在常识性歧义时，易产生错误推理且自检可能无法完全修正（如定性中的密集场景失败案例）。
- **模板语言退化**：在Sr3D上直接使用语义链反而降低性能，说明该方法对语言类型敏感，需要更鲁棒的自适应机制。
- **未与控制变量**：所有模块组合使用GPT-4o，未探讨使用更小模型（如开源7B LLM）的性能，实际部署成本较高。

（完）
