---
user_id: "cheng tan"
paper_id: 2537
arxiv_id: "2607.02517"
title: "WorldDirector: Building Controllable World Simulators with Persistent Dynamic Memory"
publish_date: "2026-07-03"
pdf_url: "https://arxiv.org/pdf/2607.02517"
abs_url: "https://arxiv.org/abs/2607.02517"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T13:00:42"
---
# WorldDirector: Building Controllable World Simulators with Persistent Dynamic Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video generation · world model · object permanence · controllable generation

## 一句话总结

WorldDirector 提出一种解耦语义运动编排与视觉生成的可控视频世界模型框架，利用 LLM 规划 3D 轨迹实现持久动态对象记忆与自由视点探索。

## 摘要

> We present WorldDirector, a highly controllable video world model framework designed for persistent dynamic object memory and unrestricted viewpoint exploration. Unlike existing world models that entangle physical dynamics with pixel rendering and rely on continuous visual observation to sustain motion, our framework explicitly decouples semantic motion orchestration from visual generation. By leveraging an LLM to coordinate 3D trajectories with camera movements and subsequently employing these orchestrated trajectories as control signals for video generation, our approach ensures strict physical logic and appearance stability, successfully preserving the exact visual identities of dynamic entities even when they re-enter the scene after prolonged periods out of view. Experimental results demonstrate that our method supports the synthesis of complex and extended events with unprecedented controllability and persistent dynamic object memory. Project Page: https://worlddirector.github.io/

Q1: 这篇论文试图解决什么问题？

现有视频世界模型将物理动力学模拟与像素渲染耦合，依赖连续的视觉观察来维持物体的运动状态。当对象长时间离开视野或被遮挡时，模型难以保持一致的外观和身份，即缺乏对象永久性（object permanence）。此外，现有模型通常缺乏独立的运动控制，无法让用户独立指定多个对象的轨迹，限制了可编辑性和视点自由度。核心挑战包括：（1）如何提供持久的动态对象记忆，使得对象在离开视野后仍能保持身份和状态；（2）如何实现不受限制的视点探索，允许相机自由移动；（3）如何保持高度可控性，允许用户通过高级语义指令（如文本描述）约束对象行为和场景事件。

Q2: 有哪些相关研究？

相关工作包括：（1）视频生成模型：如扩散模型和自回归模型，通常缺乏对长期事件和对象一致性的控制，长期视频生成采用滑动窗口但面临遮挡问题。（2）世界模型：旨在学习环境物理动力学，但将物理与渲染耦合，依赖连续观测，如 Genie、Dreamer 等。（3）对象永久性研究：在物理推理和视频生成中是核心挑战，已有工作通过内部先验（如记忆编码）或外部监控（如 FOV 检索）维持一致性，但内部先验在长时间遮挡时可能出现轨迹漂移，外部监控计算成本高。（4）可控视频生成：通过条件（分割图、深度图、文本）控制内容，但通常不支持多对象独立运动和消失/重现时的身份保持。（5）大语言模型在规划中的应用：LLMs 可用于高层语义规划，例如生成 3D 轨迹，然后通过视觉模型渲染，但缺乏与视频生成模型的紧密集成。

Q3: 论文如何解决这个问题？

WorldDirector 框架核心是解耦语义运动编排与视觉生成，包含两个主要模块：（1）语义规划模块：使用大语言模型（LLM）根据文本提示生成场景中每个动态对象的 3D 轨迹及相机运动轨迹，轨迹以帧级别时间序列表示，作为持久记忆载体。（2）条件视频生成模块：采用因果自回归架构，将语义规划模块输出的轨迹条件与文本 prompt 映射到帧中的对应空间区域，生成视频帧。轨迹条件作为控制信号，确保生成严格遵循物理逻辑，即使对象暂时离开视野，其运动信息和外观特征也能通过轨迹恢复。框架还支持用户通过文本定义对象身份、出场时间和运动，实现灵活视点探索和事件设计。

Q4: 论文做了哪些实验？

论文进行了定量和定性实验评估（具体数据集和基线需参考正文）。合理推断实验包括：（1）定量评估：在标准视频生成指标（FVD、IS）上比较基线，并设计专门指标衡量对象永久性（如重新识别准确率、外观保持度）和轨迹一致性。（2）定性展示：提供视频样本，展示对象在离开视野后重新出现时外观保持不变，以及多个对象独立运动的效果。（3）消融研究：验证解耦架构、轨迹条件、LLM 规划等各组件的贡献，可能包括移除轨迹条件或使用固定轨迹的对比。（4）用户研究：评估生成视频的可控性和视觉质量。（5）可能涉及长时间视频生成和复杂事件合成场景。具体数值和设置需参阅原文。

Q5: 发现了什么实验现象？

从有限的片段和摘要推断，主要发现包括：（1）解耦语义编排与视觉生成显著提升了对象在长期遮挡后的身份保持能力，优于依赖连续观测的基线。（2）LLM 规划的轨迹可产生符合物理逻辑的运动，避免了轨迹漂移问题。（3）因果自回归架构提高了扩展视频生成的一致性。（4）在可控性方面，用户可通过文本灵活定义对象行为，实现了前所未有的自由度。（5）可能存在反直觉现象：当 LLM 规划出现语义错误（如轨迹穿模），或轨迹条件空间分辨率不足时，可能导致生成结果违反物理规律；这些需原文确认。

Q6: 有什么可以进一步探索的点？

可能的方向包括：（1）扩展到更复杂的对象间物理交互（如碰撞、力传递）。（2）结合更丰富的 3D 场景表示（如 NeRF、3D Gaussian Splatting）以实现更精确的几何和物理模拟。（3）提升 LLM 规划的多样性和鲁棒性，处理更多对象和更长时间跨度的复杂事件。（4）探索无文本输入的任意视点自由探索，实现完全沉浸式体验。（5）将对象记忆扩展到其他模态（如声音、触觉），构建多模态世界模型。（6）降低计算成本，使其适用于交互式应用或实时模拟。（7）将持久记忆机制与强化学习结合，用于具身智能体的环境模拟和策略训练。

Q7: 总结一下论文的主要内容

本文提出 WorldDirector，一种面向可控视频世界模型的新型框架，核心创新在于持久动态对象记忆和不受限制的视点探索。现有世界模型将物理动力学与像素渲染耦合，依赖连续视觉观测，导致对象在长时间遮挡后丢失身份。WorldDirector 通过两大支柱解决该问题：第一，将语义运动编排与视觉生成解耦，利用 LLM 规划 3D 轨迹和相机运动，作为高层控制信号；第二，在因果自回归视频生成架构中将轨迹条件化，确保生成视频严格遵循规划的物理逻辑。该方法能够在视频中保持动态对象的外观一致性，即使对象离开视野很长时间后重新出现也能保持精确的视觉身份。实验表明，该方法在复杂、长时间的事件合成上实现了前所未有的可控性和持久动态记忆。主要贡献包括：（1）提出解耦语义运动编排与视觉生成的范式；（2）实现持久动态对象记忆，超越被动视频生成；（3）支持用户通过文本自由定义对象和行为，实现高度可控性。局限可能包括：依赖 LLM 规划的质量，以及长时间生成的计算开销。这篇论文代表了从视频生成向持久世界模拟器的重要一步。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向（权重0.10）直接重合，特别是可控视频生成和世界模型。

## 基本信息

- 作者：Hanlin Wang, Hao Ouyang, Qiuyu Wang, Wen Wang, Qingyan Bai, Ka Leong Cheng, Yue Yu, Yixuan Li, Yihao Meng, Zichen Liu, Yanhong Zeng, Yujun Shen, Qifeng Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02517`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段，但未获取全文；部分内容基于摘要和片段进行合理推断，具体细节请以原文为准。
