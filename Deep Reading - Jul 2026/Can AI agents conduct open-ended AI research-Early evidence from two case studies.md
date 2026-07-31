---
user_id: "cheng tan"
paper_id: 6009
arxiv_id: "2607.27191v1"
title: "Can AI agents conduct open-ended AI research? Early evidence from two case studies"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.27191v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.27191v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:20:34"
---
# Can AI agents conduct open-ended AI research? Early evidence from two case studies

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：ai agents · open-ended ai research · shadow evaluation · research automation

## 一句话总结

通过影子评估方法，对两篇未发表的NeurIPS 2026投稿论文进行案例研究，发现当前AI智能体虽能独立完成工程任务，但无法在开放性研究问题上取得实质性进展，并揭示了五种系统性的失败模式。

## 摘要

> Forecasts of explosive AI progress hinge on AI agents automating AI research. But evidence on whether agents can carry out open-ended AI research is thin. Current evaluations either test agents on narrow, verifiable tasks, which excludes open-ended research, or submit AI-generated papers to blind peer review, which is overstretched, stochastic, and suffers from poor review quality. We introduce a third way to measure progress towards AI R\&D automation. An agent takes on the central, open-ended research question of a high-quality unpublished paper, and the paper's original authors grade its output. We call these shadow evaluations. We ran shadow evaluations on two unpublished NeurIPS 2026 submissions, giving frontier agents six days and thousands of dollars of compute. The agents completed all of the engineering without human help, yet could not make substantial progress towards answering the research questions. As a result, both papers were unambiguously rejected by the authors. We identify five recurring failure modes: poor judgment about the bar for publishable research, uncreative responses to shortcomings in the research design, ineffective backtracking from dead ends, poor resource awareness, and instruction drift. A robustness check with a second model and scaffold reproduced these failures. We release the expert reviews, survey responses, agent repositories, and logs. Our results provide early evidence that today's agents can do the engineering of AI research, but struggle with critical parts of the research lifecycle.

Q1: 这篇论文试图解决什么问题？

当前AI研究自动化预测的核心在于AI智能体能否自主进行AI研究。然而，现有评估方法存在不足：要么在狭窄的可验证任务上测试智能体（无法体现开放性），要么通过盲审评估AI生成的论文（审稿质量不稳定、随机性大）。因此，缺乏一种有效的方法来评估智能体在开放性、探索性研究场景中的能力。论文旨在解决这一评估缺口，提供早期证据说明当前智能体在开放性AI研究中的实际表现。

Q2: 有哪些相关研究？

相关工作分为两类：一是基于可验证任务的评估，如EXP-Bench等，智能体需在固定指标上改进，并由自动验证器评分；二是基于盲审的评估，即AI生成的论文提交给会议或期刊审阅。这两种方法各有局限：前者无法涵盖开放性研究，后者因审稿质量波动和随机性难以可靠测量。论文提出影子评估作为第三种方法，由原论文作者直接评估智能体的输出，避免了上述问题。此外，还有研究关注AI在特定科学领域（如生物学、化学）的自动化，但本工作聚焦于AI研究本身的自动化。

Q3: 论文如何解决这个问题？

论文引入影子评估方法。具体流程：选取两篇尚未发表但质量等同于NeurIPS 2026接受水平的论文；将每篇论文的核心研究问题作为智能体的任务描述，并去除最终解决方案细节；智能体在6天内利用价值数千美元的计算资源独立执行所有工程和研究步骤，生成最终研究报告；然后由原论文作者根据他们自己的研究目标对智能体输出进行评分，判断是否达到发表水平。智能体使用了大语言模型（如GPT-4）和定制脚手架进行代码编写、实验运行和结果分析。评估过程记录了智能体的完整日志和代码，并收集了作者详细评审。

Q4: 论文做了哪些实验？

实验设计：作者选取了两篇高质量未发表的NeurIPS 2026投稿，每篇代表一个典型的AI研究问题。智能体从零开始，需理解问题、设计实验、编写代码、运行实验、分析结果并撰写报告。实验标准为：智能体是否能自主产生足以发表的创新性结果。作者采用多个指标：原创贡献度、实验完整性、结论可靠性等。此外，作者还进行了稳健性检查，使用另一款大模型（可能如Claude）和不同脚手架重复实验，观察失败模式是否重现。实验消耗：每个任务分配6天时间和数千美元计算预算。

Q5: 发现了什么实验现象？

智能体在工程任务上表现优异：独立编写了所有代码，搭建了实验流程，完成了基线实现，甚至运行了初步实验。但在研究关键环节失败：无法判断什么构成可发表的研究；面对研究设计中的问题缺乏创造性改进；不能有效从死胡同中回溯；对资源消耗（时间、计算）缺乏意识；以及指令漂移，即逐步偏离原始研究目标。作者的稳健性检查使用不同模型/scaffold，观察到相同的失败模式，说明失败具有系统性。最终，两篇论文的原始作者均明确拒绝了智能体的输出，认为未达到NeurIPS标准。

Q6: 有什么可以进一步探索的点？

未来工作可以从以下方面展开：1) 提升智能体的研究创造力，特别是在面对设计不足时的改进能力；2) 改进智能体的回溯和资源管理能力；3) 扩大影子评估范围，包括更多领域和不同类型的研究问题；4) 探索智能体在辅助而非完全替代角色时的价值；5) 结合其他评估方法（可验证任务、盲审）形成更全面的评估框架；6) 研究智能体在长期、复杂的科学发现任务中的表现；7) 开发可自动识别和缓解指令漂移的机制。

Q7: 总结一下论文的主要内容

论文核心关注点是AI智能体能否进行开放性AI研究（即不需要预先定义狭窄指标、需要探索未知答案的研究）。作者指出现有评估方法（可验证任务评估和盲审评估）的局限性，并提出影子评估作为替代。影子评估的核心思想：让智能体面对一个真实但未发表的顶级会议论文的研究问题，并由原论文作者评估智能体的输出。论文选取了两篇NeurIPS 2026投稿，赋予智能体6天时间和充沛计算资源。智能体独立完成了工程实现（代码编写、基线运行），但在研究贡献上毫无进展：无法提出有意义的创新、无法有效应对实验失败、缺乏资源意识、以及偏离原始问题。五个失败模式被清晰识别，并通过更换模型/脚手架确认其稳健性。作者公开所有材料以保证透明性与可复现性。研究表明，当前AI智能体可以承担研究中的工程部分，但关键的研究设计、问题发现和创造性工作仍需人类主导。论文为评估AI研发自动化提供了一种新方法和初步证据。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文直接涉及AI智能体能力评估，对智能体研究社区具有重要参考价值，特别是评估方法论和实践洞察。

## 基本信息

- 作者：Peter Kirgis, Sayash Kapoor, Andrew Schwartz, Stephan Rabanser, David Africa, Konstantinos Voudouris, Viet Nguyen, Toby Pilditch, Magda Dubois, Harry Coppock, Cozmin Ududec, Nitya Nadgir, Matilda Orona, Tilman Bayer, Derrick Chan-Sew, Yue Ling, Abhishek Shetty, Helen Toner, Gillian Hadfield, Seth Lazar, Steve Newman, Shoshannah Tekofsky, Rishi Bommasani, Arvind Narayanan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CY, cs.LG
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.27191v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索击中的证据片段，并结合heuristic_draft进行了补全。
