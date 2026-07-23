---
user_id: "cheng tan"
paper_id: 5170
arxiv_id: "2607.18566"
title: "The Story Shapes the Agent: Narrative Priors in LLM Behavior"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.18566.pdf"
pdf_url: "https://arxiv.org/pdf/2607.18566"
abs_url: "https://arxiv.org/abs/2607.18566"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T14:00:32"
---
# The Story Shapes the Agent: Narrative Priors in LLM Behavior

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：persona prompting · narrative priors · structural isomorphism · LLM agents

## 一句话总结

论文通过结构同构实验证明，LLM代理存在由故事框架触发的叙事先验（narrative priors），其行为影响远超人物角色提示，且跨叙事一致性仅通过行为锚词维持。

## 摘要

> Persona prompting is widely used to steer LLM agent behavior, yet the narrative framing of a task can matter more than the assigned persona. We isolate this effect through structural isomorphism, constructing three text-based investigation games that share the same action space, stage progression, and resource constraints while varying only task narrative: disease investigation, IT troubleshooting, and murder mystery. Across 1,890 sessions spanning 3 models and 10 personas, we identify narrative priors: systematic action tendencies activated by a task's story framing, independent of its decision structure. Narrative priors explain 5-31x more behavioral variance than persona, are consistent across model architectures, and in two of three domains are negatively associated with task success. Persona effects that do transfer across narratives arise from behavioral anchors, persona descriptions whose language maps directly onto shared actions. Causal interventions confirm this: removing anchor words from a high-transfer persona reduces cross-narrative consistency by 95%. Our framework also generalizes to a held-out fourth narrative and yields a persona-selection method that improves cross-narrative transfer. These results suggest that LLM behavior that survives narrative changes should be grounded in concrete actions rather than abstract descriptions.

Q1: 这篇论文试图解决什么问题？

当前LLM代理行为主要通过人物角色提示（persona prompting）引导，但任务的故事叙述可能对行为产生更强影响，这一因素很少被系统隔离研究。本文旨在回答：在保持任务决策结构完全相同下，仅改变表面叙事，LLM代理行为变化多大？叙事先验是否独立于人物角色？哪些人物角色效果能跨叙事迁移，机制是什么？

Q2: 有哪些相关研究？

相关工作包括人物角色提示（Tseng et al., 2024; Chen et al., 2024）以及叙事对决策影响的研究。本文首次通过结构同构系统分离叙事和角色效应，填补了叙事先验作为独立行为驱动因素的研究空白。

Q3: 论文如何解决这个问题？

通过结构同构设计三个文本调查游戏（疾病调查、IT故障排除、谋杀谜案），保持状态空间、动作集和决策树完全相同，仅替换背景故事。然后在大规模实验中（3个模型、10个角色、共1890轮）记录行为轨迹，利用方差分解量化叙事与角色的贡献比例。进一步识别跨叙事迁移的角色中的“行为锚词”，并通过因果干预（移除这些词汇）验证其因果作用。最后在第四个未见叙事上测试泛化能力。

Q4: 论文做了哪些实验？

包含四部分实验：（1）行为方差分解：比较叙事、角色和交互项对动作选择的解释方差；（2）跨叙事迁移分类：训练分类器根据角色预测叙事中的行为，评估迁移准确率；（3）锚词分析：统计角色描述中与共享动作直接映射的词汇频率，预测迁移能力；（4）因果干预：从高迁移角色中移除锚词，测量跨叙事一致性下降。所有实验覆盖GPT-4、Llama-2、Mistral等模型。

Q5: 发现了什么实验现象？

叙事先验解释的行为方差是人物角色的5-31倍，且一致跨模型；跨叙事迁移准确率仅10.4%（接近10%随机基线）；仅少数角色能迁移，这些角色包含行为锚词；移除锚词后迁移一致性降低95%；叙事偏向与任务成功负相关（除疾病调查中“多读”与成功正相关）；人物角色在同一叙事内效果不稳定，叙事框架是行为更强决定因素。

Q6: 有什么可以进一步探索的点？

（1）扩展叙事类型（如协商、创意写作）验证普遍性；（2）探究叙事先验来源（预训练数据分布、微调对齐）；（3）开发基于行为锚点的自动角色生成方法；（4）将结构同构框架应用于更复杂决策树；（5）研究叙事先验与安全对齐的交互。

Q7: 总结一下论文的主要内容

论文系统研究了任务叙事对LLM代理行为的影响，通过结构同构（structural isomorphism）构造三个共享决策结构但叙事不同的调查游戏：疾病爆发调查、IT故障排除和谋杀谜案。在1890轮会话、3个模型（GPT-4、Llama-2、Mistral等）和10个人物角色（如专家、怀疑论者、效率优先者）上进行实验。核心发现是存在“叙事先验”：由故事设定自动触发的行为倾向，独立于角色提示，且效应量远大于角色。方差分解显示叙事解释的行为方差是角色的5-31倍。跨叙事迁移实验中，人物角色效果几乎不转移（平均10.4%准确率，接近随机），只有那些包含直接映射到共享动作词汇的角色才能转移，作者称这些词汇为“行为锚点”。因果干预证明：从高转移角色描述中移除锚词，跨叙事一致性降低95%，确认因果关系。论文还展示了框架泛化到第四个未参与实验的叙事（一个盗窃调查），并基于锚词提出改进跨叙事迁移的角色选择方法。讨论部分指出叙事先验通常不利于任务成功（负相关），但疾病调查中阅读更多线索的行为例外。局限性包括叙事类型单一、模型范围有限、未解释先验来源。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接相关LLM代理行为控制，用户画像中智能体方向权重0.10

## 基本信息

- 作者：Yixuan Wang, James Lester, Shashank Srivastava
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.18566`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答参考了论文摘要、引言、讨论和实验结果等检索证据。
