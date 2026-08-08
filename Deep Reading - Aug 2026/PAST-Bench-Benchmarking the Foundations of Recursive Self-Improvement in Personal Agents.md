---
user_id: "cheng tan"
paper_id: 6464
arxiv_id: "2608.04003v1"
title: "PAST-Bench: Benchmarking the Foundations of Recursive Self-Improvement in Personal Agents"
publish_date: "2026-08-04"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.04003v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.04003v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T01:45:55"
---
# PAST-Bench: Benchmarking the Foundations of Recursive Self-Improvement in Personal Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：recursive self-improvement · personal agents · persistent memory · benchmark

## 一句话总结

PAST-Bench 是一个专门用于评估个人 AI 智能体是否真正从跨会话保留经验中获益的基准，通过在同任务族内配对开启/关闭持久化存储的实验协议，将性能提升归因于保留经验而非基础模型或运行时噪声，并在此诊断基础上提出 Hermes+ 干预框架。

## 摘要

> Recursive self-improvement requires agents to turn accumulated experience into better future behavior. Personal AI agents offer a concrete setting for studying this capability because they retain preferences, task histories, tool routines, and learned skills across sessions. Yet whether retained experience actually improves them over time has not been systematically tested. We introduce PAST-Bench, a benchmark designed to isolate this question. Each agent runs through ordered sequences of fresh-session tasks under matched conditions that turn retained experience on and off. It spans 26 scenarios and 204 episodes across memory, procedural reuse, information gathering, and update. We report both later-task gains and whether those gains follow the intended save, retrieve, and update pathway. Across seven base models and four agent frameworks, improvement is real but uneven across capabilities. Agents with the same headline gain can differ markedly in whether that gain is supported by evidence of the intended pathway. Guided by these findings, we develop Hermes+, which extends Hermes with five targeted interventions across stages of the agent loop. Hermes+ raises the average gain from retained experience and provides clearer pathway evidence, with its strongest improvement on tasks requiring outdated state to be replaced, although the effect remains capability- and model-dependent. Together, PAST-Bench and Hermes+ provide an evaluation and diagnostic foundation for studying how persistent agents can progress from retaining experience to systematically improving through it. Code: https://github.com/Gen-Verse/PAST-Bench

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：如何可靠地判断个人 AI 智能体是否真正从跨会话保留的经验中受益，从而实现递归自我改进。现有基准无法区分‘后期任务性能提升’的来源：性能提升可能来自保留经验，也可能来自基础模型能力、运行时环境、提示词设计、检索捷径、任务难度变化或评分噪声。论文指出，当前持久化感知（persistence-aware）基准可分为两类：一类把历史内容暴露在长上下文窗口或动态注入模型（如 Letta 2025、Maharana 2024），另一类允许模型把记忆写入外部存储并在后续会话读取。前者无法把上下文长度效应与真正学习分离，后者又缺少对照实验来界定保留经验的因果贡献。PAST-Bench 试图用‘任务族内配对开启/关闭持久化’的评估协议来隔离这个混淆：如果保留经验开启时后期任务表现更好，且该提升能被保存、检索、更新这条预期路径的证据支持，才能把改进归因于保留经验；否则，增益可能只是基础模型或运行时的产物。这个问题是递归自我改进研究的基础，因为不能归因就无法诊断改进机制，更无法指导智能体设计。

Q2: 有哪些相关研究？

相关工作分两条主线。一是持久化感知基准：一类使用长上下文窗口保留历史（Letta 2025、Maharana 2024），另一类允许外部记忆存储与跨会话访问。论文认为这两类都有缺陷，前者混入上下文长度效应，后者缺乏严格的对照归因。二是自进化智能体或记忆增强智能体工作，例如 MemRL（Zhang 等 2026a）通过运行时强化学习在情景记忆上训练智能体，OSWorld（Zhang 等 2024）在多模态真实环境基准中考察开放任务智能体，DSPy（Khattab 等）把语言模型调用编译为可学习管线，也与智能体经验复用相关。此外，引用了递归自我改进相关理论文献（Lee 2026、Qu 2024、Ren 2026、Wang 2026、Yin 等），说明 RSI 概念已有讨论，但缺少可操作的评估协议。PAST-Bench 的独特之处在于它把评估单元从‘单次任务得分’改为‘任务族内的完整轨迹’，并通过 persistence-on/off 配对控制变量，同时报告机制证据（是否走 save-retrieve-update 路径）而非只看最终分数。论文还区分了‘基础模型、运行时、提示、检索捷径、任务难度、评分噪声’等竞争性解释，这些混淆在以往基准中未被系统处理。

Q3: 论文如何解决这个问题？

PAST-Bench 的解决方案由三部分构成：（1）基准构造：设计 26 个场景、204 个 episode，覆盖四类能力——记忆（memory）、程序性复用（procedural reuse）、信息收集（information gathering）和更新（update）。每个能力对应若干任务族（task family），任务族内按照有序的新会话序列组织，早前 episode 给智能体机会保存可复用经验，后续 episode 测试这些经验是否被利用。（2）评估管线：采用‘配对开启/关闭持久化’（persistence-on/off）的匹配条件，在相同任务族和相同基础模型下切换是否允许跨会话保留经验；评分不仅比较后期任务的绝对分数，还计算‘后期任务增益’（later-task gains），并检查增益是否支持预期的 save-retrieve-update 路径。评估单元是智能体穿越任务族的轨迹而非单次分数，从而区分由保留经验带来的提升。（3）诊断驱动设计：基于实验观察，作者开发 Hermes+，在 Hermes 智能体循环的各个阶段加入五项针对性干预，目的是提高保留经验的平均增益并让机制证据更清晰。论文用‘同 headline gain 但路径证据不同’的例子说明诊断指标的必要性：只有路径证据能区分‘真正借由保存-检索-更新获得改进’与‘偶然的分数提升’。整体方法强调性能归因（attribution）而非单纯性能排名。

Q4: 论文做了哪些实验？

论文在 7 个基础模型和 4 个智能体框架上做了系统实验。实验设置包括：在每个任务族内按顺序运行多个 fresh-session episode，每个会话开始时智能体不携带任何记忆（除持久化开启条件下可读取之前保存的内容）。对比条件为持久化开启与关闭，控制所有其他变量（基础模型、运行时、提示、评分器）。主实验结果报告每个能力领域（记忆、程序性复用、信息收集、更新）的后期任务增益。随后进行诊断分析，检查增益是否伴随 save-retrieve-update 路径机制证据。基于诊断结果，作者设计 Hermes+，并对比其与基线 Hermes 在平均增益和路径证据上的差异。实验还特别关注‘需要替换过时状态’（outdated state replacement）的任务类型，因为这类任务对更新能力要求最高。论文没有在摘要中给出具体数值，但明确报告‘improvement is real but uneven across capabilities’，且‘agents with the same headline gain can differ markedly in pathway evidence’。实验覆盖模型和框架的多样性使结论更具泛化性，但受限于摘要范围，详细消融和每个模型的具体分数未在检索片段中呈现。

Q5: 发现了什么实验现象？

检索到的证据显示以下实验现象：（1）跨 7 个基础模型和 4 个框架，保留经验带来的改进是真实的，但在各能力上不均匀——某些能力（如记忆）改进明显，另一些（如更新）可能更弱。（2）一个关键反直觉发现是：两个智能体可能具有相同的总体增益（headline gain），但一个的增益由预期路径（保存-检索-更新）的证据支持，另一个则没有；说明传统聚合分数掩盖了机制层面的差异，单靠总分无法判断智能体是否真正学会了利用经验。（3）Hermes+ 的五项干预平均上提升了收益并提供了更清晰的路径证据，最强效果出现在需要替换过时状态的任务上，这表明过时状态替换是持久化智能体中最困难且干预最有效的环节。（4）效果是能力和模型依赖的：没有单一干预能对所有模型或所有能力一致地带来提升。这些现象共同说明，评估递归自我改进必须同时报告‘是否改进’和‘为什么改进’两层信息。

Q6: 有什么可以进一步探索的点？

论文在 Future Work 部分提出（从检索证据可推断）：首先，未来版本应拓宽生态效度和时间跨度——当前基准虽覆盖 26 场景 204 episode，但仍可能受限于合成任务或有限时间范围，应加入更接近真实个人助理使用的任务、更长的会话序列、更复杂的用户偏好演化。其次，可以探索更强的 RSI 形式：当前工作聚焦于通过外部记忆和工具复用来改进未来行为，未来可考虑模型参数更新、学习算法调整或智能体架构变化，这些更接近‘递归’的深层含义。第三，机制证据目前只区分 save-retrieve-update 路径，可以进一步细分路径失败模式（如保存了但没检索、检索了但没正确更新、更新了但造成过时状态未被替换），以设计更精准的干预。第四，Hermes+ 的干预建议可以扩展到其他框架和基础模型，并研究干预的自动选择——不同模型/能力可能需要不同干预组合。第五，可探索把 PAST-Bench 与运行时强化学习（如 MemRL）结合，用基准引导记忆策略的在线优化。第六，可以研究如何把保留经验的收益从任务得分泛化到用户满意度、安全性或长期目标一致性等维度。

Q7: 总结一下论文的主要内容

这篇论文针对个人 AI 智能体是否真正从保留经验中受益这一递归自我改进的基础问题，构建了 PAST-Bench 基准和 Hermes+ 干预框架。论证主线是：递归自我改进（RSI）要求智能体把自身运行产生的经验转化为未来更好的行为，但现有系统虽然保留偏好、任务历史、工具例程和习得技能，却缺乏系统性测试来确认这些保留经验是否真的带来改进。技术主线上，作者设计了一种性能归因协议：在 26 个场景、204 个 episode 中，任务按任务族有序组成 fresh-session 序列，每个任务族内配对执行‘持久化开启’和‘持久化关闭’两种条件，控制基础模型、运行时等混淆因素；评估单元是智能体穿越任务族的完整轨迹，报告后期任务增益，并额外检查增益是否符合预期的 save-retrieve-update 机制路径。实验主线覆盖 7 个基础模型和 4 个智能体框架，发现改进是真实但不均匀的，且相同总体增益的智能体在机制路径证据上差异显著。基于诊断结果，作者提出 Hermes+，在智能体循环各阶段加入五项针对性干预，总体上提升平均增益并让路径证据更清晰，其中最强效果出现在需要替换过时状态的任务上，但效果依赖于具体能力和模型。结论上，论文强调评估 RSI 需要同时报告任务分数和机制证据，PAST-Bench 与 Hermes+ 共同构成‘从保留经验到系统性改进’的评估-诊断基础，并在未来工作中提出拓宽生态效度、时间跨度、探索参数级 RSI、细化路径失败模式等方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体方向（用户画像权重 0.10）直接匹配：论文研究的是个人 AI 智能体跨会话经验利用，属于 agent 学习与记忆的核心问题。

## 基本信息

- 作者：Shuhan Xue, Zixin Ding, Yichen Shen, Yinjie Wang, Zhenfei Yin, Yingcheng Wu, Yuxin Chen, Mengdi Wang, Ling Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-04
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.04003v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了检索到的 PDF 语义证据片段（包括 Introduction、Conclusion、Future Work、Contents 和 References 命中），并结合摘要信息进行了详尽归纳；由于 PDF 正文未完整提供，部分实验数值和具体干预细节为合理推断或需要回原文确认。
