---
user_id: "cheng tan"
paper_id: 3175
arxiv_id: "2607.08233"
title: "Playing ZendoWorld: Challenging AI Agents on Active Visual Concept Induction"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.08233.pdf"
pdf_url: "https://arxiv.org/pdf/2607.08233"
abs_url: "https://arxiv.org/abs/2607.08233"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:23:16"
---
# Playing ZendoWorld: Challenging AI Agents on Active Visual Concept Induction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：active visual concept induction · zendo world · perception-induction-experiment loop · vision-language model

## 一句话总结

提出ZendoWorld环境，用于评估智能体在主动视觉概念归纳中的闭环感知、归纳和实验设计能力。

## 摘要

> A central challenge in building intelligent systems is enabling agents to jointly perceive complex inputs, form hypotheses about hidden patterns, and design informative experiments to test them. To study this problem, we propose ZendoWorld, a controlled interactive environment in which agents must infer a logical rule about visual game observations, acquire information by proposing new scenes, and refine their hypotheses based on feedback from the game environment. We evaluate several agents spanning pure VLM reasoning, Bayesian particle filtering, dynamic concept discovery, and neuro-symbolic methods. Our main findings are: (1) high accuracy in predicting labels for observed examples does not imply recovery of the underlying rule; (2) perception and induction are distinct bottlenecks for different agent classes; and (3) VLM-based agents propose near-uninformative experiments, failing to actively reduce hypothesis uncertainty. To compare these results, we collect human data on the task, which reveals a gap in inductive reasoning, particularly for more complex rules. Overall, ZENDOWORLD takes an important step toward evaluating intelligent agents and identifies concrete avenues for improvement, particularly in domains like scientific discovery.
> Code: https://github.com/ml-research/ZendoWorld
> Data: https://huggingface.co/datasets/ss567uhg/zendo-synthetic-data

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：如何使智能体能够同时完成感知复杂视觉输入、归纳隐藏概念模式以及主动设计信息性实验这三项能力，并将其整合在闭环中？当前缺乏一个可控的基准来系统评估这些能力之间的交互与瓶颈。具体而言，现有研究要么只关注感知（如视觉理解），要么只关注归纳（如符号概念学习），很少能在同一任务中同时要求智能体进行实验设计。ZendoWorld通过模拟类似Zendo桌游的环境，要求智能体根据少量带标签的观察例提出新场景、获取标签并逐步逼近隐藏规则，从而暴露感知、归纳和实验设计之间的耦合与短板。

Q2: 有哪些相关研究？

相关研究包括：(1)主动学习与实验设计：传统主动学习侧重于不确定性采样，但通常不涉及概念归纳和感知。(2)概念归纳与规则学习：如Burgess & Grim（2025）的图形概念归纳工作，但缺乏视觉感知环节。(3)视觉推理基准：如CLEVR、GQA等，但通常不要求智能体主动设计实验。(4)神经符号系统：整合神经网络感知和符号推理，但多数工作未考虑实验设计。(5)视觉-语言模型（VLM）推理：如GPT-4V在多步推理中表现，但缺乏主动信息获取。(6)贝叶斯概念学习：使用粒子滤波进行假设空间搜索，但处理原始视觉输入时依赖额外感知模块。(7)科学发现与假设检验：如AI科学家系统，但通常不在受控视觉环境中。ZendoWorld填补了这些方向之间的空白。

Q3: 论文如何解决这个问题？

论文通过构建ZendoWorld环境来解决问题。环境核心是一个逻辑规则归纳游戏：智能体看到一组物体场景，每个场景有一个隐藏规则决定的二元标签（符合／不符合规则）。智能体可以观察若干带标签的例场景，然后主动提出新场景并获得其标签，逐步更新对规则的假设。游戏按回合进行，智能体在每一轮提交一个场景并得到标签，最后给出对规则的完整描述。论文实现了五种智能体：(1)VLM Agent：完全依赖GPT-5-mini进行感知、生成和推理；(2)贝叶斯粒子滤波Agent：使用符号感知模块提取属性，用粒子滤波维护假设分布；(3)动态概念发现Agent：基于神经符号方法，学习潜在的符号概念；(4)神经符号Agent：结合视觉编码器和符号推理；(5)随机猜测基线。智能体在22个游戏（涵盖5种规则复杂度+OOD规则）上评估，并与人类表现对比。

Q4: 论文做了哪些实验？

实验在22个ZendoWorld游戏上进行，规则包括基本谓词（如颜色、形状）、计数（如恰好三个红色物体）、奇偶（如偶数个蓝色）、空间关系（如物体在左边）、逻辑连接（与/或/异或）以及一个OOD规则（无法用基础谓词表达）。评估指标：(1)准确标注测试例的准确率；(2)规则恢复准确率（最终假设与真实规则一致）；(3)实验信息性（提出的场景对减少假设不确定性的贡献）。智能体包括：VLM Agent（GPT-5-mini）、贝叶斯粒子滤波（BPF）、动态概念发现（DCD）、神经符号（NS）、随机基线。另外收集了30名人类参与者数据作为对照。实验回答四个研究问题：Q1端到端性能？Q2感知瓶颈？Q3规则归纳能力？Q4实验设计信息性？

Q5: 发现了什么实验现象？

实验揭示了以下主要现象：(1)高标注准确率不保证规则恢复：VLM Agent在多数游戏上标注准确率超过80%，但规则恢复准确率不到20%，表明其仅学到了表面关联。(2)感知和归纳是不同类智能体的不同瓶颈：贝叶斯粒子滤波和神经符号Agent的感知模块限制其性能，而VLM Agent感知强但归纳弱，缺乏结构化推理。(3)VLM Agent的实验设计几乎无信息：VLM提出新场景时往往只有极小的信息增益，甚至不如随机基线；相比之下，粒子滤波Agent在部分游戏中能生成信息性场景。(4)人类在简单规则上表现良好，但在复杂规则（如逻辑连接、计数）上也会失败，且失败模式与VLM Agent相似，表明复杂归纳本身具有挑战性。(5)动态概念发现Agent在规则恢复上表现突出，但在感知上依赖预定义属性，限制了泛化。

Q6: 有什么可以进一步探索的点？

进一步探索点包括：(1)改进实验设计策略：引入强化学习或基于模型的实验规划，使智能体主动减少假设不确定性。(2)增强归纳推理：将符号搜索与VLM结合，实现混合推理以提升规则恢复。(3)扩展环境复杂性：增加连续视觉属性、部分可观测性、多步推理等，更接近真实科学发现。(4)跨领域迁移：将ZendoWorld范式扩展到分子化学、程序合成等科学推理领域。(5)收集更多人类数据：探索人类在复杂规则下的归纳策略，设计认知启发式智能体。(6)研究感知-归纳-实验设计的联合训练，而非独立模块。

Q7: 总结一下论文的主要内容

论文针对智能系统在主动视觉概念归纳中面临的核心挑战，提出了ZendoWorld基准环境。ZendoWorld是一个受控游戏环境，要求智能体从视觉场景中推断隐藏的逻辑规则，并通过设计新场景来主动获取信息，形成感知-归纳-实验设计的闭环。论文实现了多种代表性智能体：基于VLM（GPT-5-mini）的端到端方法、贝叶斯粒子滤波、动态概念发现以及神经符号方法，并在22个不同复杂度的游戏上进行了系统比较。实验围绕四个研究问题展开：端到端解决能力、感知瓶颈、归纳准确性和实验信息性。主要发现包括：(1)标注准确率与规则恢复显著解耦，高标注率并不反映对规则的理解；(2)感知和归纳是不同智能体的不同瓶颈，VLM感知强但归纳弱，而贝叶斯方法归纳好但感知受限；(3)VLM智能体在实验设计上表现极差，提出的场景几乎无信息价值，甚至不如随机基线；(4)人类实验表明，即使是人类在复杂规则下也会出现类似错误，说明归纳本身具有挑战性。ZendoWorld为评估和提升智能体的主动推理能力提供了新的测试平台，并指出了具体改进方向，例如增强实验设计策略、结合符号与神经方法等。论文还公开了代码和数据，以推动该方向的研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体研究方向直接相关，强调主动感知和推理的整合。

## 基本信息

- 作者：Sophia Koehler, Antonia Wüst, Inga Ibs, Wasu Top Piriyakulkij, Wolfgang Stammer, Constantin Rothkopf, Kevin Ellis, Kristian Kersting
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CV
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.08233`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本报告参考了PDF检索证据。
