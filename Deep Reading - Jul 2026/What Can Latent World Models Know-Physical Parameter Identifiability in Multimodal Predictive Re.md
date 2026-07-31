---
user_id: "cheng tan"
paper_id: 6036
arxiv_id: "2607.27017v1"
title: "What Can Latent World Models Know? Physical Parameter Identifiability in Multimodal Predictive Representations"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27017v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27017v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:24:10"
---
# What Can Latent World Models Know? Physical Parameter Identifiability in Multimodal Predictive Representations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：latent world model · physical parameter identifiability · predictive representation · multimodal

## 一句话总结

本文通过证书门控协议和POKEWORLD实验揭示，潜世界模型中的物理参数可识别性由预测目标结构决定——只有目标要求的内容被保留，输入限制决定可获取的边界，额外数据无法拓展这一边界。

## 摘要

> A central premise of latent world models is that predicting the future forces a representation to internalize the physics of its environment. Which physical quantities does a trained latent actually contain, and what decides this? We answer with controlled interventions in POKEWORLD, an interactive environment whose visually identical objects hide mass, drag, and contact stiffness. A certificate-gated protocol first certifies each parameter as recoverable from raw observations, then measures whether it enters the latent, so a null result can be attributed to the objective rather than to the environment. The resulting identifiability map has two organizing mechanisms and one frontier. Inputs limit what can be known, while prediction targets decide what is retained. Stiffness enters the latent only when touch is forecast ($R^2=0.50$, compared with $-0.02$ when the same signal is merely fused into the input), and under single-step prediction a vision-only latent discards even perfectly visible object state. Drag marks the frontier. It carries a recoverability certificate of 0.89 yet plateaus near 0.13 under every deterministic prediction objective we test, while a supervised head on the same trunk reaches 0.45. Parameters whose readout is slow and ratio-type under the sensed coordinates fall outside what these objectives acquire. On RH20T, an input-target factorial across scaling curves reproduces both mechanisms across two robots and 4,258 episodes. Every arm missing information or prediction pressure stays flat over a fivefold data range, and only the full multimodal objective forecasts force beyond a persistence baseline, with held-out gains that grow with scale. Objective structure determines which physical parameters a latent acquires, and additional data improves only the parameters it already acquires.

Q1: 这篇论文试图解决什么问题？

潜世界模型（如JEPA、Dreamer）的核心预设是通过预测未来使表示内化环境物理规律，但这一预设缺乏严格验证：经过预测训练的潜表示究竟包含了哪些物理参数？这些参数的出现由什么决定？是输入数据、预测目标还是模型容量？现有工作通常假设预测自动导致有用表示，或通过下游任务间接评估，未能分离输入与目标的不同作用。本文旨在通过可控实验直接回答：设计环境使物理参数独立且视觉混淆，再用证书门控协议区分环境可恢复性与目标获取能力，从而建立预测目标与潜表示内容的因果图。

Q2: 有哪些相关研究？

本文涉及两条研究脉络：(1) 潜世界模型与联合嵌入预测架构（JEPA），如I-JEPA、V-JEPA（Assran et al., 2023/2025），通过预测未来潜表示而非像素学习通用表示，但未量化表示中的具体物理量。(2) 从视频和交互中估计物理参数（Wu et al., 2017; Gelada et al., 2019; Guo et al., 2020），通常使用监督学习，未研究预测目标如何影响参数内化。Cui et al. (2026) 指出预测目标获取的内容局限于其要求，但未提供系统性可识别性分析。本文的独特贡献是引入可恢复性证书，在输入-目标分离的框架下绘制可识别性地图，并批驳“更多数据可弥补目标结构缺陷”的隐含假设。

Q3: 论文如何解决这个问题？

论文通过三步解决：(1) 构建POKEWORLD环境：力控代理戳动视觉相同但隐藏物理参数（质量m、阻力γ、接触刚度k）的物体，生成解耦参数轨迹。(2) 建立证书门控协议：先训练监督模型从观测预测各参数，得到可恢复性证书R²；再训练潜世界模型（V-JEPA风格）预测未来（多种目标变体），冻结编码器后用线性探针测量潜含量R²；比较证书与潜含量：证书高而潜含量低归因于预测目标，证书低则环境限制。(3) 设计输入-目标因子实验：在POKEWORLD中变化输入（纯视觉V vs 视觉+力输入VF）和预测目标（视觉V、力F、多模态V+F），并在RH20T真实机器人数据（4258 episodes）上重复因子设计，同时变化数据规模（1×-5×），检验结论的泛化性和规模效应。

Q4: 论文做了哪些实验？

论文包含两组主要实验：(1) POKEWORLD可控实验：生成约50k轨迹，训练多个潜世界模型（不同输入-目标组合），测量各参数的证书与潜含量，并设置监督头对比。关键条件包括单步/多步预测、纯视觉/多模态输入、触觉预测仅作为目标或仅作为输入融合。(2) RH20T真实机器人实验：使用Franka Panda等两个机器人共4258 episodes，采用类似因子设计，输入（V vs VF），目标（预测视觉、预测力、联合），并在1×、2×、5×数据子集上训练，重点评估力参数（因证书高但可能被目标限制）。测量指标为线性探针R²（5折交叉验证）。实验还包含持久性基线（使用前一帧真值作为预测）以对比模型能力。

Q5: 发现了什么实验现象？

反直觉发现：(1) 预测目标选择决定参数进入潜表示：刚度k只在触觉预测目标下编码（R²=0.50），而仅作为融合输入时不编码（-0.02）。(2) 单步视觉预测下，潜表示甚至丢弃完全可见的物体状态（如位置）。(3) 阻力γ代表“前沿”：证书0.89，但所有确定性预测目标下潜含量≤0.13，而监督头达0.45，说明预测目标结构根本限制其获取，且由于阻力是“慢变、比率型”物理量，与坐标系统不对齐。(4) 数据规模不能补偿目标结构：在POKEWORLD和RH20T上，增加数据仅改善已获取参数（如多模态目标下的力），未获取参数（如仅视觉目标下的力）保持平坦（RH20T上5倍数据仍持平于持久性基线）。(5) RH20T跨机器人一致性：两个机器人结果一致，表明机制非环境特例。

Q6: 有什么可以进一步探索的点？

(1) 探索随机预测目标（如对比损失、对数似然）能否改变可识别性边界。(2) 多步预测、规划目标是否有助于获取慢变参数（如阻力）？(3) 证书方法的扩展：使用信息论下界替代监督模型，提高严谨性。(4) 在更复杂物理系统（非线性、高维、部分可观测）中验证当前结论。(5) 建立预测目标分类学，预测目标函数与可获取参数之间的映射。(6) 研究基于本文发现的“目标工程”方法，设计能获取所需物理参数的预测目标。(7) 结合强化学习，分析任务相关物理量如何通过目标设计进入表示。

Q7: 总结一下论文的主要内容

本文系统研究了潜世界模型中物理参数的可识别性问题。作者构建了POKEWORLD环境，其中物体视觉相同但隐藏物理参数（质量m、阻力γ、接触刚度k）可独立变化，实现可控实验。核心贡献是证书门控协议：先训练监督模型评估每个参数从观测中的可恢复性（证书），再训练潜世界模型（V-JEPA）在不同预测目标下，用线性探针测量潜表示中的参数含量（潜含量）。通过比较分离环境与目标的作用。POKEWORLD实验涵盖输入（V vs VF）和目标（V、F、V+F）的因子化设计，发现：(1) 刚度k仅在触觉预测目标下编码（R²=0.50 vs -0.02）；(2) 单步视觉预测下潜表示丢弃可见状态；(3) 阻力γ在所有确定性目标下潜含量仅0.13（证书0.89），而监督头0.45；(4) 数据规模仅改善已获取参数。RH20T真实机器人实验（两机器人、4258 episodes）重现上述机制：仅多模态目标使力参数可预测，其他条件持平，且5倍数据下仍无改进。结论：预测目标结构决定参数可识别性，输入限制边界，数据规模不能替代目标设计。本文挑战了“预测自动内化物性”的假设，为世界模型设计提供了严格的实验证据和诊断框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：系统性的实验设计方法论可作为可重复因果推断的范例，适合学习如何分离影响因素。

## 基本信息

- 作者：Kaizhen Tan, Xin Xu, Siru Tao, Hanzhe Hong, Yang Feng, Heqing Du
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.RO
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27017v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成在字段内容上优先参考了PDF检索证据（retrieved_evidence和field_evidence_map），并结合启发式草稿和论文元数据，对于信息缺口已标注合理推断。
