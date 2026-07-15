---
user_id: "cheng tan"
paper_id: 3612
arxiv_id: "2607.11312v1"
title: "SLVMBench: Skill Learning from Video Memory"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11312v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11312v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:37:19"
---
# SLVMBench: Skill Learning from Video Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill learning · video language models · benchmark · long video memory

## 一句话总结

SLVMBench 是首个评估视频大语言模型从长视频记忆学习技能并应用于实时任务的基准，揭示了现有模型在此任务上的显著不足。

## 摘要

> We introduce Skill Learning from Video Memory (SLVMBench), the first benchmark that jointly evaluates whether video large language models (video-LLMs) can learn skills from long video memory and apply them to real-time tasks. SLVMBench presents models with 2-3 hour video streams that contain a tutorial video embedded in a stream of arbitrary irrelevant videos, resembling real-world human learning practices. Video-LLMs are asked to apply the acquired skill to answer real-time questions about an ongoing video. Unlike long-video understanding benchmarks that emphasize passive comprehension and skill-learning benchmarks that rely on short, immediate demonstrations, SLVMBench tests the full pipeline of memorizing and extracting procedural knowledge, as well as transferring it to real-time tasks. Moreover, rigorous human annotations feature sub-second-level temporal calibration, manually engineered questions eliminating common-sense guessing, and collated tutorials to ensure coverage of the required skills. Evaluations on state-of-the-art proprietary and open-source video LLMs show that video-LLMs struggle substantially with learning and applying skill knowledge from videos. Moreover, performance degrades markedly when the skill knowledge is placed within a long video memory. These results reveal a key limitation of existing video LLMs and position SLVMBench as the first benchmark for studying real-time skill acquisition and application from long-context video memory.

Q1: 这篇论文试图解决什么问题？

现有视频LLM评估基准主要集中于短时间视频的被动理解（如问答、字幕），或技能学习依赖短时即时演示。缺乏评估模型从长视频记忆中学习程序知识并实时迁移的能力，这正是人类学习的关键方式。SLVMBench 旨在解决这一评估缺口。

Q2: 有哪些相关研究？

现有相关研究包括：①长视频理解基准（如VideoMMLU、LVBench等）评估被动理解；②技能学习基准（如DemoICL、VideoMMMU）依赖短时演示或少样本学习；③视频LLM在短时间任务上取得进展。但与SLVMBench不同，它们并未测试从长视频记忆中提取和迁移技能的过程。SLVMBench 填补了同时评估记忆和技能迁移的空白。

Q3: 论文如何解决这个问题？

SLVMBench 构建了包含教程视频（2-3分钟）和持续时间长的无关干扰视频（总计2-3小时）的视频流，模拟真实学习环境。任务要求模型在观看流后，对正在播放的新视频片段回答实时问题，这些问题需要应用教程中的技能。数据构建遵守原则：①模拟多模态情景记忆，确保技能知识在时间上被干扰间隔（至少30分钟）；②人工标注包括亚秒级校准、反常识性猜测问题；③教程与问题对应保证知识覆盖。评估多种视频LLM，包括专有和开源模型。

Q4: 论文做了哪些实验？

论文对多种当前最先进的视频LLM进行了评估，包括闭源模型（如GPT-4V、Gemini等）和开源模型（如Video-LLaVA、LLaMA-VID等）。实验设置包括三种条件：条件一（无教程）、条件二（有教程但无记忆间隔）、条件三（完整SLVMBench设置，即教程嵌入长视频流且有记忆间隔）。测量模型在实时问答任务上的准确率。此外，通过消融分析评估记忆间隔长度和干扰视频影响。

Q5: 发现了什么实验现象？

实验发现：①所有视频LLM在SLVMBench上表现挣扎，准确率远低于人类水平；②性能在技能知识位于长视频记忆中时显著下降，表明现有模型在学习程序知识并长期保持方面存在严重不足；③提供教程视频但无记忆间隔可带来一定提升，但提升有限；④不同模型间差异较大，闭源模型通常优于开源模型，但总体水平仍不令人满意；⑤消融实验显示，增加干扰视频长度和数量会进一步降低性能。

Q6: 有什么可以进一步探索的点？

SLVMBench 为后续研究提供了基准。未来工作可以探索：①增强视频LLM的长视频记忆机制，如引入记忆增强模块或更好的长上下文编码；②改进技能提取和迁移能力，例如通过更有效的教程学习或元学习；③扩展到更复杂的技能和交互任务；④将SLVMBench扩展到真实场景，如机器人操作学习等。此外，论文未涉及的方向包括利用外部记忆库和结构化知识表示。

Q7: 总结一下论文的主要内容

SLVMBench 是由来自（机构未知）的研究者提出的首个评估视频大语言模型从长视频记忆学习技能并实时应用的基准。该基准的核心是：模型被给予一个2-3小时的视频流，其中嵌入了一个2-3分钟的教学视频（演示某项技能，如折纸、烹饪等），其余为无关干扰视频。随后，模型需要观看一个新的视频片段，并实时回答有关该片段的问题，这些问题需要运用从教学视频中学到的技能。基准的关键设计包括：通过保证至少30分钟的干扰间隔来测试长期记忆；通过人工标注实现亚秒级时间对齐；问题设计避免常识性猜测确保必须运用技能；教程与问题配对以保证知识覆盖。论文评估了包括GPT-4V、Gemini、Video-LLaVA等在内的多种视频LLM，发现它们在技能学习任务上表现显著不足，特别是在长视频记忆条件下性能急剧下降。这表明现有视频LLM在从长期视频记忆中抽取和迁移程序知识方面存在严重局限。SLVMBench 作为第一个此类基准，为未来研究实时技能获取和长上下文视频记忆提供了平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：SLVMBench 与智能体研究相关，因为智能体需要从视频中学习技能并实时应用。

## 基本信息

- 作者：Yudong Yang, Guangzhi Sun, Yixuan Li, Chao Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11312v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成依赖于PDF语义检索命中的证据片段（包括Abstract、Introduction、Conclusion、Related Work等部分），并结合启发式草稿进行润色和补全。部分字段（如实验设置细节）基于合理推断，因原文未完全提供，建议回查原文确认。
