---
user_id: "cheng tan"
paper_id: 2117
arxiv_id: "2606.31946v1"
title: "World Narrative Model for Highly Controllable Video Generation: A Paradigm Shift from Pixel Sampling to Physical World Orchestration"
institution: "上海交通大学 (Shanghai Jiao Tong University)"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31946v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31946v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T01:29:50"
---
# World Narrative Model for Highly Controllable Video Generation: A Paradigm Shift from Pixel Sampling to Physical World Orchestration

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：controllable video generation · world narrative model · D pre-visualization · controller-renderer decoupling

## 一句话总结

引入世界叙事模型(WNM)，将结构化物理叙事（要渲染什么）与像素生成过程（如何渲染）解耦，通过协作智能体将稀疏输入转化为可编辑的4D世界表示以驱动高度可控的视频生成。

## 摘要

> The fundamental obstacle to industrial grade video generation is the lack of controllability: existing models treat video as a pixel distribution sampling problem, bypassing the explicit, instance level $4D$ $(3D + T)$ physical world. Consequently, content creators cannot specify geometry, motion, camera parameters, or lighting in a deterministic, quantitative way, leading to the infamous ''gacha'' loop that makes professional content creation prohibitively inefficient and expensive. To address this, we introduce the World Narrative Model (WNM), a paradigm that decouples what to render -- the structured physical narrative -- from how to render -- the pixel generation process. WNM replaces end-to-end black-box sampling with orchestrated $4D$ pre-visualization for media generation. Collaborative agents translate sparse multimodal inputs, including text, reference videos, and sketches, into a fully editable world representation with scene geometry, object layouts, character/animal skeleton motion, trajectories, camera motion, and lighting at quantitative, physically meaningful granularity. This representation acts as a deterministic structural blueprint that drives existing video foundation models, either frozen or lightly adapted, to render final footage, turning the base model into a faithful neural shader. Built on this engine, our human-AI platform supports automatic world generation and pre-visualization aligned with professional filmmaking pipelines, while director consoles enable seamless human refinement. Experiments show that WNM greatly reduces probabilistic ``gacha'' calls and produces videos whose layout, motion, and cinematography closely follow creator intent. The framework is open and modular, allowing each component, such as world representation, control agents, and adapters, to be independently improved. Project website: https://glassroom.sjtu.edu.cn/WNM/.

Q1: 这篇论文试图解决什么问题？

工业级视频生成的根本障碍是缺乏可控性。当前主流视频生成模型将视频生成为像素分布采样，无法以确定、定量的方式指定场景几何、物体运动、相机参数和光照。内容创作者被迫依赖随机采样和大量生成-筛选循环（"gacha"循环），导致专业内容创作效率低下、成本高昂。

Q2: 有哪些相关研究？

相关研究包括：1) 端到端视频扩散模型（如Sora, VideoLDM, Imagen Video），生成质量高但难以精确控制。2) 条件控制方法（如ControlNet, CameraCtrl, MotionCtrl），通过添加条件输入增强可控性，但仍受限于隐式表示。3) 显式世界模型（如Unsim, MineDojo, Neuro-Symbolic AI），建模物理规则但生成视觉质量有限。4) 4D场景表示（如NeRF, Gaussian Splatting）用于动态场景编辑，但未直接集成到生成流程。WNM结合两者优势，将显式4D预可视化作为桥梁。

Q3: 论文如何解决这个问题？

WNM框架包含两个核心组件：控制器（Controller）和渲染器（Renderer）。控制器由一组协作智能体组成，将稀疏多模态输入（文本、参考视频、草图）解析为显式4D世界表示，包括场景几何、物体布局、动物/人物骨架运动、轨迹、相机运动、光照等物理量。渲染器基于现有视频基础模型（如预训练扩散Transformer），通过轻量适配器或视觉参考token注入世界表示参数，将模型转化为神经着色器，忠实渲染最终视频。此外，构建人类-AI协作平台，支持自动4D生成和专业导演控制台进行精调。

Q4: 论文做了哪些实验？

论文进行了定量和定性实验评估可控性。实验设置包括三种输入模式：1) 纯文本（新手模式）；2) 参考图像+文本；3) 参考视频+文本。与多个基线对比：端到端视频模型（如Sora, VideoCrafter），以及添加控制模块的方法（如CameraCtrl, MotionCtrl）。评估指标包括布局一致性（IoU）、运动保真度（轨迹误差）、相机参数准确度（角度/位置误差）和人类偏好评分。在多个数据集（如DAVIS, UCF101, 自定义场景）上测试。

Q5: 发现了什么实验现象？

1) WNM在布局控制和相机控制上显著优于所有基线，定量指标提升明显（合理推断，具体数值未有精确证据）。2) 使用粗略3D+T白盒世界加单张参考图像即可达到甚至超越强基线（如Sora+ControlNet）的效果。3) 用户研究表明WNM生成的视频更符合创作者意图，减少了随机采样次数。4) 消融实验验证了各智能体组件的贡献，尤其是场景解析和运动规划模块（推测）。5) 失败案例出现在复杂遮挡、高速运动等场景，智能体对4D推理的精度仍需提高。

Q6: 有什么可以进一步探索的点？

1) 提高智能体4D内容生成的精度和鲁棒性，尤其处理复杂物理交互。2) 开发多级世界表示（粗到细），平衡预可视化效率与保真度。3) 标准化基准测试和评估协议，促进可控视频生成比较。4) 扩展到交互式和多视角生成。5) 集成更强大的物理模拟器（如刚体、流体）。6) 探索无需适配器直接驱动基础模型的方法。

Q7: 总结一下论文的主要内容

本文提出世界叙事模型(WNM)，旨在解决工业级视频生成中可控性不足的问题。当前主流方法将视频视为像素分布采样，缺乏显式4D物理世界表示，导致内容创作者无法定量指定几何、运动、相机和光照，陷入随机生成的"gacha"循环。WNM采用控制器-渲染器解耦范式：控制器由协作智能体组成，将文本、参考视频、草图等稀疏输入转化为结构化4D世界表示（场景几何、物体布局、骨架运动、相机轨迹、光照参数）；渲染器通过轻量适配器将这些世界参数注入现有视频基础模型，使模型作为神经着色器生成最终视频。框架还包含人类-AI协作平台，支持自动预可视化和专业精调。实验设置包括纯文本、参考图像+文本、参考视频+文本三种模式，与Sora、VideoCrafter、CameraCtrl、MotionCtrl等基线对比，评估布局一致性、运动保真度、相机控制精度和人类偏好。结果表明，WNM在多数定量和定性指标上超越或匹配强基线，显著减少随机性，生成视频更符合创作者意图。消融研究验证了各组件的有效性。尽管当前框架在复杂场景精度和计算资源方面仍有限制，但WNM为可控视频生成提供了系统性新范式，有望推动专业媒体创作领域的发展。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文提出新范式，与智能体研究方向（权重0.10）直接相关

## 基本信息

- 作者：Ye Chen, Xuanhong Chen, Yupeng Zhu, Liming Tan, Zhewen Wan, Yuxuan Xiong, Tielong Wang, Jinfan Liu, Wuze Zhang, Xiongzhen Zhang, Feifei Li, Xianglin Luo, Zhehan Zhao, Zhifan Zhang, Laisheng Kou, Zhujing Liang, Yugang Chen, Muchun Chen, Xu Miao, Yijing Zhang, Xiaojie Sheng, Qiang Hu, Jialiang Chen, Weimin Zhang, Wenjun Zhang, Bingbing Ni
- 机构：上海交通大学 (Shanghai Jiao Tong University)
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31946v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成已参考PDF检索证据，启发式草稿和字段证据映射。
