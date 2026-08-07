<div class="dpr-home-notice-card">
  <h3 class="dpr-home-notice-title">🚀 Start Here</h3>
  <ul class="dpr-home-notice-list">
    <li><a href="#/tutorial/README">使用教程</a></li>
  </ul>
</div>

## 每次日报
- 最新运行日期：2026-08-07
- 运行时间：2026-08-07 20:11:17 UTC
- 运行状态：成功
- 本次总论文数：17
- 精读区：6
- 速读区：11

### 今日简报（AI）
今日共扫描17篇论文，精读6篇、速读11篇，重点聚焦视觉分割与机器人操控的零样本泛化能力。最值得关注的是MAVISEG提出的流形传播与视觉原型方法，显著提升扩散Transformer中零样本开放词汇分割效果；OC-VLA++则通过几何引导的跨视角一致性，增强机器人操控的视角鲁棒性。若追求实用进展，建议优先精读这两篇高分工作，理解其如何用结构先验突破数据稀缺瓶颈。
- 详情：[/202608/07/README](/202608/07/README)

### 精读区论文标签
1. [MAVISEG: Manifold Propagation and Visual Prototypes for Zero-Shot Open-Vocabulary Segmentation in Diffusion Transformers](/202608/07/2608.05878v1-maviseg-manifold-propagation-and-visual-prototypes-for-zero-shot-open-vocabulary-segmentation-in-diffusion-transformers)  
   标签：评分：9.0/10、query:open-vocab-d
   evidence：零样本开放词汇分割，利用流形传播与视觉原型，进行掩码级视觉-语言对齐
2. [OC-VLA++: Monocular Geometry-Guided Cross-View Consistency for Viewpoint-Robust Robotic Manipulation](/202608/07/2608.01066v1-oc-vla-monocular-geometry-guided-cross-view-consistency-for-viewpoint-robust-robotic-manipulation)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：机器人操作中的跨视角一致性与几何引导监督
3. [Multi-View Unified Camera Fields: Geometry-Shaped Action-Facing Representations for RGB-Only Multi-Camera VLA Policies](/202608/07/2608.01826v1-multi-view-unified-camera-fields-geometry-shaped-action-facing-representations-for-rgb-only-multi-camera-vla-policies)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：多相机VLA框架，跨视角一致性对齐且具备遮挡感知能力
4. [Talk2Sensors: 3D Visual Grounding in Autonomous Driving via Sensor-Adaptive Physical Cue Matching](/202608/07/2608.04568v1-talk2sensors-3d-visual-grounding-in-autonomous-driving-via-sensor-adaptive-physical-cue-matching)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：融合相机、LiDAR与雷达的开放词汇三维视觉定位
5. [CDSeg: A Renderable Gaussian Carrier for Image-to-3D Label Transfer](/202608/07/2608.05482v1-cdseg-a-renderable-gaussian-carrier-for-image-to-3d-label-transfer)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：基于渲染可见性将2D掩码赋给3D高斯原语
6. [SCI-CLIP: Segment-Centric Inference with Reference Memory for Training-Free Open-Vocabulary Segmentation](/202608/07/2608.05627v1-sci-clip-segment-centric-inference-with-reference-memory-for-training-free-open-vocabulary-segmentation)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：免训练的开放词汇分割，以片段为中心的推理与区域一致性交互图

### 速读区论文标签
1. [Prior-SG: Task and Prior Driven Region Segmentation for Scene Graphs in Arbitrarily-Structured Environments](/202608/07/2608.06170v1-prior-sg-task-and-prior-driven-region-segmentation-for-scene-graphs-in-arbitrarily-structured-environments)  
   标签：评分：8.0/10、query:open-vocab-d
   evidence：分层三维场景图结合开放词汇特征融合，支持任意结构环境中的区域分割
2. [Distill What RGB Can Recover: Privileged 3D Evidence for RGB-Only Vision-Language Models](/202608/07/2608.00110v1-distill-what-rgb-can-recover-privileged-3d-evidence-for-rgb-only-vision-language-models)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：将特权3D证据蒸馏到纯RGB视觉语言模型，用于三维场景理解
3. [Hi-Token: Hierarchical Coordinate Tokenization for Generative Visual Grounding](/202608/07/2608.03471v1-hi-token-hierarchical-coordinate-tokenization-for-generative-visual-grounding)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：通过结构化坐标标记化实现区域级视觉-语言对齐
4. [EgoAfford: Task-Oriented Affordance Grounding via Egocentric Referring Segmentation](/202608/07/2608.04533v1-egoafford-task-oriented-affordance-grounding-via-egocentric-referring-segmentation)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：第一视角参照分割实现任务导向功能接地，属掩码级对齐
5. [DistMedVL: Distributional Vision-Language Alignment for Uncertainty-Aware Medical Image Segmentation](/202608/07/2608.05683v1-distmedvl-distributional-vision-language-alignment-for-uncertainty-aware-medical-image-segmentation)  
   标签：评分：7.0/10、query:open-vocab-d
   evidence：掩码级概率视觉-语言对齐，用于细粒度分割并建模不确定性
6. [Articulated Object Reconstruction from Rest-State Observation](/202608/07/2607.27749v1-articulated-object-reconstruction-from-rest-state-observation)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：融合视觉-语言与分割模型输出获得三维部分结构
7. [ObjectStream: Latent Objects as Memory Anchors for Streaming Video Understanding](/202608/07/2607.28312v2-objectstream-latent-objects-as-memory-anchors-for-streaming-video-understanding)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：将潜在对象作为跨帧记忆锚点，维持视频内容的一致性，符合多视角/视频中一致性特征聚合需求
8. [SSR: Similarity-Shift Refinement for Training-Free Object-Centric Masks](/202608/07/2608.01103v1-ssr-similarity-shift-refinement-for-training-free-object-centric-masks)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：利用亲和图进行免训练掩码细化，提升目标中心掩码质量，可支持2D掩码到3D分配
9. [SpatioLM: Towards General Physical Spatial Intelligence in Vision-Language Models](/202608/07/2608.01899v1-spatiolm-towards-general-physical-spatial-intelligence-in-vision-language-models)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：利用伪深度与相机信息增强视觉语言模型的空间智能
10. [VR3D: View-Robust 3D Representation Learning for Aerial-Ground Person Re-Identification](/202608/07/2608.02598v1-vr3d-view-robust-3d-representation-learning-for-aerial-ground-person-re-identification)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：将2D外观特征提升至规范3D空间以获得视角鲁棒表示
11. [Global Graph-Validated Optimization for VLM-based 3D Indoor Scene Generation](/202608/07/2608.03064v1-global-graph-validated-optimization-for-vlm-based-3d-indoor-scene-generation)  
   标签：评分：6.0/10、query:open-vocab-d
   evidence：基于VLM和全局图优化的开放词汇三维室内场景生成


<div class="dpr-home-promo-card">
  <h3 class="dpr-home-promo-title">💬 社区与支持</h3>
  <ul class="dpr-home-promo-list">
    <li>欢迎 Star / Fork / Issue / PR</li>
    <li>QQ群：583867967（欢迎交流，已有：1151人）</li>
  </ul>
</div>
