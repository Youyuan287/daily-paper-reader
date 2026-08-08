<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-08-08
- 运行时间：2026-08-08 20:10:26 UTC
- 运行状态：成功
- 本次总论文数：14
- 精读区：5
- 速读区：9

### 今日简报（AI）
今日聚焦3D视觉与多模态，精读5篇、速读9篇，覆盖3D问答、车辆损伤评估与机器人场景几何。  
最值得关注的是两篇8.0分精读：3D问答的Token压缩方法《3DZip》与结合分割的车辆损伤评估《Grounding Agentic VLMs》。  
建议普通读者优先把握3D问答效率优化及细粒度视觉评估两条主线，后续可关注自监督3D编码器方向。
- 详情：[/202608/08/README](/202608/08/README)

### 精读区论文标签
1. [3DZip: Spatial-Aware Feature Diversity-Guided Token Compression for 3D Question Answering](/202608/08/2608.01185v1-3dzip-spatial-aware-feature-diversity-guided-token-compression-for-3d-question-answering)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：将2D视觉特征投影到世界坐标构建几何感知的3D令牌，直接涉及特征提升问题
2. [Grounding Agentic VLMs with Dedicated Segmentation for Fine-Grained Vehicle Damage Assessment](/202608/08/2608.02470v1-grounding-agentic-vlms-with-dedicated-segmentation-for-fine-grained-vehicle-damage-assessment)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：通过专用分割实现掩码级视觉-语言对齐
3. [Better, Stronger, Faster, and Broader: Structured All-Mask Prediction for MLLM-Based Segmentation](/202608/08/2608.02791v1-better-stronger-faster-and-broader-structured-all-mask-prediction-for-mllm-based-segmentation)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：结构化的全掩码预测实现文本与图像token对齐，用于指代/推理分割
4. [CROSS: Cascaded Distillation and Dual-Constraint Grounding for Remote Sensing Referring Segmentation](/202608/08/2608.03147v2-cross-cascaded-distillation-and-dual-constraint-grounding-for-remote-sensing-referring-segmentation)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：遥感指称分割，通过级联蒸馏与双约束定位实现区域级VLM-SAM对齐
5. [CoordRefer: Coordinate-Aware 3D Visual Grounding from Multiview Images](/202608/08/2608.05569v1-coordrefer-coordinate-aware-3d-visual-grounding-from-multiview-images)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：基于多视图图像的坐标感知3D视觉定位解耦坐标框架选择与框回归

### 速读区论文标签
1. [ARGUS: Aligning Robot Scene Geometry Under Shifting Views with Large 3D Vision Models](/202608/08/2608.05579v1-argus-aligning-robot-scene-geometry-under-shifting-views-with-large-3d-vision-models)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：将任意相机视角的观测对齐到规范视角，处理机器人操作中的跨视角一致性问题
2. [Confidence matters: Leveraging Multi-view Geometric Priors for GS-based Reconstruction](/202608/08/2608.06117v1-confidence-matters-leveraging-multi-view-geometric-priors-for-gs-based-reconstruction)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：在3DGS重建中集成多视图法向/深度先验并使用置信度加权
3. [On the Efficacy of Self-Supervised Point Cloud Encoders for Efficient 3D Large Language Models](/202608/08/2607.29136v1-on-the-efficacy-of-self-supervised-point-cloud-encoders-for-efficient-3d-large-language-models)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：评估低成本自监督点云编码器用于三维大语言模型，是三维场景理解的关键组件
4. [Look Up and Look Back: Hidden Attention and Latent Orientation in a Frozen Foundation Model for Panoramic SLAM](/202608/08/2608.00925v1-look-up-and-look-back-hidden-attention-and-latent-orientation-in-a-frozen-foundation-model-for-panoramic-slam)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：全景SLAM利用跨视角注意力与成本感知级联实现一致的回环检测
5. [Credit the Right Box: Marginal Contribution Assignment for Structured Visual Perception](/202608/08/2608.01055v1-credit-the-right-box-marginal-contribution-assignment-for-structured-visual-perception)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：多模态大模型中框级贡献分配用于grounding与分割，区域级监督
6. [FineMoLA: Towards Fine-Grained Motion-Language Alignment from Clip-Level Supervision](/202608/08/2608.01392v1-finemola-towards-fine-grained-motion-language-alignment-from-clip-level-supervision)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：利用最优传输从片段级弱监督实现细粒度帧-短语视觉语言对齐
7. [Hunyuan3D-Buffalo 1.0: A Unified Multimodal Model for Scalable 3D Generation, Understanding, and Editing](/202608/08/2608.02711v1-hunyuan3d-buffalo-10-a-unified-multimodal-model-for-scalable-3d-generation-understanding-and-editing)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：统一多模态3D模型支持3D理解与文本接地部件生成，并构建了8700万规模的3D多模态语料
8. [UniEvo-RS: Omni-Prompt Unified Remote Sensing Segmentation with Representative Exemplar-Driven Prototype Evolution](/202608/08/2608.03911v1-unievo-rs-omni-prompt-unified-remote-sensing-segmentation-with-representative-exemplar-driven-prototype-evolution)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：面向密集分割的区域级视觉-语言对齐，利用范例原型演进
9. [BridgeVLA++: A Data-Efficient, Generalizable, and Memory-Augmented Vision-Language-Action Framework for 3D Manipulation](/202608/08/2608.05042v1-bridgevla-a-data-efficient-generalizable-and-memory-augmented-vision-language-action-framework-for-3d-manipulation)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：将点云投影为多视图图像以复用预训练VLM特征，实现2D视觉语言特征到3D的提升


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
