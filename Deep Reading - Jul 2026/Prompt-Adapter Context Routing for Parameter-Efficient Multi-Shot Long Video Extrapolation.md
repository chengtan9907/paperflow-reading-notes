---
user_id: "cheng tan"
paper_id: 2942
arxiv_id: "2607.06481"
title: "Prompt-Adapter Context Routing for Parameter-Efficient Multi-Shot Long Video Extrapolation"
publish_date: "2026-07-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.06481.pdf"
pdf_url: "https://arxiv.org/pdf/2607.06481"
abs_url: "https://arxiv.org/abs/2607.06481"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-09T00:31:12"
---
# Prompt-Adapter Context Routing for Parameter-Efficient Multi-Shot Long Video Extrapolation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video generation · diffusion model · multi-shot extrapolation · parameter efficient

## 一句话总结

PACR-Video提出一种参数高效的多镜头长视频外推框架，通过冻结扩散骨干网络并添加低秩时间适配器与递归提示路由，在不微调生成器的情况下保持实体、风格和因果连续性。

## 摘要

> We present PACR-Video, a parameter-efficient framework for multi-shot long video extrapolation that preserves recurring entities, scene structure, visual style, and causal progression without full generator fine-tuning. PACR-Video keeps a text-to-video diffusion transformer frozen and augments it with low-rank temporal adapters conditioned by learned shot-role prompt tokens. To maintain long-horizon coherence, it builds a recursive prompt bank that stores compact entity, location, action, and style prompts from previous shots, then routes them through adapter gates according to predicted narrative dependencies. A Shot-Local/Story-Global tuning objective combines next-shot reconstruction, cross-shot identity contrast, and prompt sparsity regularization, while an adapter composition schedule balances early-shot visual consistency with later-shot event progression and viewpoint change. Across six multi-shot and long-video benchmarks, PACR-Video outperforms text-to-video, tuning-based, memory-augmented, streaming, and recursive-context baselines on distributional quality, semantic alignment, identity consistency, temporal smoothness, motion stability, transition coherence, and human preference. These results show that compact prompt routing and lightweight temporal adaptation provide sufficient controllable capacity for stable long video extrapolation.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决长视频多镜头序列生成中的一致性问题：现有方法在保持重复实体、场景结构、视觉风格和因果事件进展的同时，允许新镜头引入运动、视点和叙事信息，但往往需要微调生成器大部头参数或依赖外部记忆模块，导致长时程下的漂移和退化。核心挑战是在使用冻结骨干的条件下实现可控的多镜头外推。

Q2: 有哪些相关研究？

相关研究分为几类：1）直接文本到视频生成（VideoCrafter2, ModelScopeT2V等）无法跨镜头保持一致性；2）调优基方法（Tune-A-Video, AnimateDiff）需要每段视频微调；3）记忆增强方法（StoryMem）显式存储早期帧，但容量受限；4）流式方法（ShotStream）连续生成但缺乏全局规划；5）递归上下文方法（ReCA）将多镜头扩展视为递归分配问题，但依赖密集帧记忆。PACR-Video在此基础上用紧凑提示替代密集记忆，实现参数高效控制。

Q3: 论文如何解决这个问题？

PACR-Video的解决方案包含五个关键组件：1）冻结预训练的文本到视频扩散Transformer（Gθ），插入低秩时间适配器（LoRA式，每层秩r≪d），通过门控αℓ,t调制时间注意力；2）为每个镜头学习一个角色嵌入标记（如establishing shot, close-up），提供叙事功能信号；3）构建递归提示库，将先前镜头压缩为实体、位置、动作和风格四种紧凑提示，通过门控适配器路由到当前生成；4）训练目标由局部扩散噪声预测、跨镜头身份对比（DINO特征对比）和提示稀疏正则化组成；5）适配器组合调度：早期镜头冻结实体/位置适配器侧重一致性，后期释放动作/风格适配器促进变化。

Q4: 论文做了哪些实验？

在六个多镜头/长视频基准上评估：FlintstonesSV、Pororo-SV（动画）、ActivityNet Captions、YouCook2（真实动作）、Shot2Story、MovieNet（叙事结构）。设置：给定前两个镜头（每个16帧，256×256）及镜头级文本提示，递归生成后续八个镜头。基线包括VideoCrafter2、ModelScopeT2V、AnimateDiff、Tune-A-Video、Text2Video-Zero、SEINE、StoryMem、ShotStream、ReCA。评估指标：FVD（分布质量）、LPIPS时序不一致性、RAFT光流误差（运动稳定性）、CLIPScore（语义对齐）、DINO身份一致性、BLIP-2描述对齐、镜头转换连贯性、人类偏好（600次随机对比）。

Q5: 发现了什么实验现象？

PACR-Video在所有指标上全面超越基线：FVD从268.4降至231.7，LPIPS时序不一致性降低，RAFT误差下降；CLIPScore从31.2提升至32.8，DINO身份一致性从0.724增至0.771，镜头转换连贯性从0.681提升至0.734；人类偏好对比ReCA胜率显著。消融实验表明递归提示库和门控路由贡献最大；适配器调度平衡一致性（早期）和变化（后期）；提示压缩（四种类型）优于单一记忆或简单均值。失败模式包括极端长序列下提示退化、快速运动场景的模糊。

Q6: 有什么可以进一步探索的点？

论文指出若干开放方向：1）更复杂的提示路由策略（如学习动态权重而非固定门控）；2）扩展至交互式/流式生成，允许用户在线调整；3）处理更长的镜头序列（超过10个镜头）和更复杂的叙事结构；4）结合外部知识库或世界模型增强因果一致性；5）将框架迁移至其他条件生成任务（如音频驱动的视频生成）。目前证据不足，建议参考原文第7节。

Q7: 总结一下论文的主要内容

PACR-Video提出一种参数高效的多镜头长视频外推方法。核心思想是将预训练的文本到视频扩散Transformer冻结，仅通过少量可训练参数实现跨镜头一致控制。方法在Transformer的时序注意力层插入低秩适配器，为每个镜头学习角色提示标记，并构建递归提示库存储实体、位置、动作和风格信息。生成下一镜头时，通过门控机制仅路由当前场景所需的提示历史。训练采用局部重建（噪声预测）、全局身份对比（DINO特征）和提示稀疏正则化，并引入适配器组合调度以平衡早期一致性与后期变化。在六个覆盖动画、真实动作和电影叙事的基准上，PACR-Video在FVD、语义对齐、身份保持、运动平滑性和人类偏好等指标上显著优于现有基线（包括记忆增强和递归上下文方法）。实验验证了紧凑提示表示和参数高效适配器可以有效取代密集记忆和全微调，为长视频生成提供可控且稳定的方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于长视频生成，与用户画像中的‘generation’方向直接相关。

## 基本信息

- 作者：Anna Córdoba, Adam Puente Tercero, Nerea Angulo Hijo, Mar Linares Tercero, Julia Barrientos, Ainhoa Miranda, Jesús Olivera
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-08
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.06481`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读报告参考了PDF检索证据片段和完善heuristic_draft，但部分细节（如future_directions）基于论文结构推算，建议返回原文确认。
