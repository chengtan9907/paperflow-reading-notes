---
user_id: "cheng tan"
paper_id: 5479
arxiv_id: "2607.21580"
title: "GraphVid: Interactive Graph-Controllable Video Generation"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21580.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21580"
abs_url: "https://arxiv.org/abs/2607.21580"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-24T13:16:07"
---
# GraphVid: Interactive Graph-Controllable Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：graph-conditioned video generation · interactive control · structured interaction · diffusion transformer

## 一句话总结

GraphVid提出了一种基于图条件的图像到视频生成模型，通过结构化交互图实现多物体动态的精确控制，显著优于现有轨迹控制方法。

## 摘要

> Controllable video generation remains challenging due to the difficulty of specifying precise multi-object interactions using text prompts or motion-control inputs that primarily constrain pixel movement. In practice, trajectory-based control often requires users to draw accurate tracks for multiple objects, which scales poorly with scene complexity and becomes ambiguous under occlusion or overlap. To enable flexible yet precise multi-subject control, we introduce $\textbf{GraphVid}$, a graph-conditioned image-to-video generation model that enables interactive control through structured interaction graphs. We further curate $\textbf{GraphVid-Bench}$, a large-scale interaction-centric video dataset with structured relational annotations to enable training of interaction-aware video generation models. Despite using substantially less training data and fewer trainable parameters than prior motion-control methods, GraphVid delivers strong controllability and video quality. Compared with Motion-I2V, GraphVid reduces FID by up to 39.9% and FVD by 37.6%, while improving PSNR (9.87=>15.98) and SSIM (0.38=>0.61). Our results highlight the potential of structured semantic interfaces as a powerful paradigm for controllable video generation.

Q1: 这篇论文试图解决什么问题？

现有可控视频生成方法主要依赖文本提示、轨迹控制或运动场条件。文本提示难以精确描述多物体交互；轨迹控制需要用户绘制精确路径，在复杂场景下难以扩展，且对遮挡和重叠情况模糊。此外，这些方法通常需要大量训练数据（如Motion-I2V使用10M样本），限制了实际应用。GraphVid旨在通过图结构化的语义界面，实现对多物体交互的灵活、精确控制，同时降低数据需求。

Q2: 有哪些相关研究？

相关研究可分为几类：1）布局条件方法，如基于空间布局的生成；2）运动场条件，如Motion-I2V等；3）轨迹条件，如用户定义轨迹。轨迹方法依赖密集点跟踪监督，数据需求大。近期也有基于场景图的视频合成工作，但主要面向从文本生成视频，而非交互控制。GraphVid首次提出使用有向交互图作为用户控制界面，结合预训练扩散模型，实现交互级控制。

Q3: 论文如何解决这个问题？

GraphVid的核心是图条件图像到视频生成框架。具体而言：1）使用预训练的LTX-Video Diffusion Transformer（DiT）作为骨干网络，冻结其VAE编码器和DiT权重；2）输入有向交互图，通过Edge-Aware Graph Reasoning模块进行消息传递，该模块利用边属性编码物体间关系，输出物体级特征表示；3）将物体特征作为条件注入到DiT的时序模块（如通过跨注意力机制），引导视频生成；4）仅训练图推理模块和少量注入层，保持骨干不变。这种方法在显著减少可训练参数的同时实现了有效的交互控制。

Q4: 论文做了哪些实验？

论文创建了GraphVid-Bench数据集，包含多个交互场景的视频和结构化关系标注。实验将GraphVid与Motion-I2V基线进行对比，在相同输入图像和条件设置下评估。定量指标包括FID（Frechet Inception Distance）、FVD（Frechet Video Distance）、PSNR和SSIM。此外还进行了人类偏好研究，在10个多样化场景中比较语义意图和视觉质量。

Q5: 发现了什么实验现象？

实验观察到：1）定量上，GraphVid相比Motion-I2V降低了FID 39.9%和FVD 37.6%，PSNR从9.87提高到15.98，SSIM从0.38提高到0.61，表明生成视频在保真度和结构相似性上均有大幅度提升；2）人类偏好测试中，GraphVid在6/10场景中获得多数偏好，3个场景平局，仅1个场景落后，说明其在语义一致性和视觉质量上更符合用户意图；3）值得注意的是，GraphVid使用远少于Motion-I2V的训练数据（GraphVid-Bench规模未明确，但远小于10M），且可训练参数更少，显示了结构化条件的效率优势。

Q6: 有什么可以进一步探索的点？

基于当前工作，未来方向包括：1）扩展到更长的视频生成和更复杂的多物体交互；2）自动图构建或结合文本提示生成图，降低用户输入成本；3）动态图支持，允许交互关系随时间变化；4）与其他条件（如轨迹、文本）融合，实现混合控制；5）在更多下游任务（如机器人仿真、内容创作）中验证模型；6）探索结构化控制与无控制生成的结合。

Q7: 总结一下论文的主要内容

本文聚焦于可控视频生成中多物体交互控制的挑战。作者提出GraphVid，一种基于图条件的图像到视频生成模型。与需要精准轨迹或大量数据的现有方法不同，GraphVid允许用户通过有向交互图直观指定物体间的交互关系，从而实现生成控制的灵活性和可解释性。模型基于预训练的LTX-Video Diffusion Transformer，并引入Edge-Aware Graph Reasoning模块来编码图结构信息，通过跨注意力机制注入条件。在训练过程中，骨干网络被冻结，仅训练少量可学习参数，从而在数据效率上具有优势。为支持此类任务，作者还构建了GraphVid-Bench数据集，包含带关系标注的交互视频。在定量评估中，GraphVid在FID、FVD、PSNR和SSIM上均显著优于Motion-I2V，例如FID降低39.9%，PSNR由9.87提升至15.98。人类偏好研究进一步证实了其生成结果在语义和视觉上的一致性和质量。论文展示了结构化语义界面作为可控视频生成新范式的潜力，并为进一步研究提供了基准和方法。主要贡献包括：首次提出图交互控制框架、Edge-Aware图推理模块、以及GraphVid-Bench数据集。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文直接贡献于生成方向（用户权重0.1），提出了一种新的视频生成控制范式。

## 基本信息

- 作者：Vedant Shah, Onkar Susladkar, Tushar Prakash, Kiet Nguyen, Tianjio Yu, Adheesh Juvekar, Muntasir Waheed, Ismini Lourentzou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21580`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括introduction、related work、human preference等片段；部分推断基于摘要和片段内容。
