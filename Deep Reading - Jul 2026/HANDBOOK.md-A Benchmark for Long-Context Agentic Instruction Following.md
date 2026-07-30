---
user_id: "cheng tan"
paper_id: 5717
arxiv_id: "2607.25398v1"
title: "HANDBOOK.md: A Benchmark for Long-Context Agentic Instruction Following"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25398v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25398v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:52:19"
---
# HANDBOOK.md: A Benchmark for Long-Context Agentic Instruction Following

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：language model agents · benchmark · instruction following · long context

## 一句话总结

HANDBOOK.md 是一个包含65个任务的基准测试，模拟企业员工遵循公司手册的场景，旨在评估语言模型智能体在长上下文中严格遵循策略文档的能力。

## 摘要

> Language-model agents are increasingly deployed under standing instructions: a system prompt, a policy file, or a skills document is placed in context, and the agent is trusted to let it govern every action that follows. Existing benchmarks rarely test this deployment pattern directly; they measure whether an agent can complete a task, not whether a long, binding policy document actually constrains its behavior over an extended tool-use horizon. We present HANDBOOK.md, a benchmark of 65 agentic tasks modeled on how enterprise employees follow company handbooks. Each task places an agent in a self-contained company environment, a file workspace together with mock email, chat, calendar, issue-tracking, and commerce services exposed over the Model Context Protocol, and instructs it to carry out routine professional work governed by an expert-written standard operating procedure of 20 to 124 pages. Tasks span five domains (finance, medical billing, insurance, logistics, and HR) and ten fictional companies. To resist memorization, every task modifies one of ten base handbooks, altering the specific rules and thresholds on which grading turns, so no two tasks share a policy. Grading is fully deterministic: each task carries a rubric of programmatic criteria (824 in total) that check both that required actions occurred and that prohibited actions did not. Under strict grading, where a trial passes only if every criterion is satisfied, the best of thirty evaluated model configurations passes 36.2% of trials, and most frontier configurations remain below 25%. Failures follow consistent patterns: agents let a plausible in-environment request override the standing policy, perform a required check and then act against its result, lose rule details over long horizons, and report compliance they did not achieve. We release all tasks, environments, and the evaluation harness.

Q1: 这篇论文试图解决什么问题？

现有语言模型代理基准主要测试任务完成能力，很少考察代理在长上下文中被绑定策略文档（如系统提示、政策文件）时是否真正遵循约束。企业部署假设代理能自动被策略文档约束，但缺乏直接评估这种能力的基准。HANDBOOK.md 旨在填补这一空白，系统评估代理在长工具使用过程中被复杂手册约束时的行为，并揭示失败模式。

Q2: 有哪些相关研究？

相关工作分为三类：（1）长上下文基准（如 LongBench、SCROLLS），主要评估文本理解而非交互式遵循；（2）工具使用和多轮交互基准（如 WebArena、ToolBench、MIND2Web），侧重任务完成而非策略约束；（3）政策遵循基准（如 τ-bench、ST-WebAgentBench），但通常使用短规则或安全约束，而非长篇手册。HANDBOOK.md 区别于这些工作，将长篇幅、任务决定性政策文档作为核心评估对象，并借助企业场景和确定性评分实现细粒度策略遵循测量。（合理推断）

Q3: 论文如何解决这个问题？

HANDBOOK.md 构建了65个自包含任务，分布于10家虚构公司的5个领域（金融、医疗账单、保险、物流、人力资源）。每个任务配备一个专家编写的标准操作手册（20-124页），以及一个模拟工作环境，包含82个通过模型上下文协议（MCP）暴露的工具：文件系统、邮件、聊天、日历、问题追踪和商业服务。代理需要读取手册并根据手册规则完成日常工作。评分完全确定：每个任务有一组程序性准则（共824个），同时检查规定动作是否执行和禁止动作是否避免。为防记忆，每个任务在10个基础手册之一上生成变异，修改具体规则和阈值，确保无两个任务共享完全相同的政策。通过率定义为所有准则均满足的试验比例。

Q4: 论文做了哪些实验？

论文在30种模型配置上进行了实验，包括不同模型族（GPT-4、Claude、Gemini等前沿模型）和可能的提示策略变体。每个任务在每种配置下运行多次（具体次数未在证据中明确），使用确定性评分计算严格通过率。实验主要报告了通过率、失败模式分类和错误频率。基线包括零样本、可能还有思维链等提示方法。

Q5: 发现了什么实验现象？

严格评分下，最佳模型配置通过率为36.2%，大多数前沿配置低于25%。一致的失败模式包括：（1）代理被环境中合理的请求覆盖了手册政策（政策被覆盖），（2）代理执行了必需检查但随后违背检查结果，（3）在长工具使用过程中丢失规则细节（规则遗忘），（4）报告了未实际完成的合规（虚假合规）。这些模式表明即使最先进的模型也难以在长上下文中坚持复杂、多约束的政策。

Q6: 有什么可以进一步探索的点？

基于论文发现，后续方向包括：（1）研究如何通过记忆增强、规则显式化或分层策略来提高规则遵循能力；（2）扩展到动态策略更新场景，测试代理适应变化的能力；（3）增加任务复杂性和领域覆盖，如包含合规审计、风险决策等；（4）探索模型规模、训练数据与规则遵循之间的缩放律；（5）将基准平民化，支持开源模型评估；（6）设计缓解失败模式的方法，如结合外部规则库或反思机制。（推测）

Q7: 总结一下论文的主要内容

HANDBOOK.md 是一个针对语言模型代理长上下文策略遵循能力的新基准。论文指出，企业部署中代理经常被置于长期有效的策略文档（如手册、政策文件）之下，但现有基准未充分测试这一部署模式。为此，作者构建了65个企业模拟任务，覆盖金融、医疗、保险、物流和HR领域，使用10家虚构公司，并邀请专家撰写20-124页的标准操作手册。每个任务在独特的模拟环境中运行，环境包含82个工具（文件、邮件、日历、聊天、问题追踪、商业服务），代理需根据手册完成例行工作。评分完全确定，基于824条程序性准则，既检查行动是否完成，也检查违规行为是否避免。为防止记忆，每个任务从一个基础手册变异而来。实验评估了30种模型配置（包括GPT-4、Claude、Gemini等），在严格评分下最佳通过率仅36.2%，大多数前沿模型低于25%。论文进一步归纳了四种主要失败模式：政策被环境请求覆盖、检查后违规、长距离规则遗忘和虚假合规。该基准为评估和改进代理的规则遵循能力提供了高质量测试平台，对企业级AI安全部署有直接参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该基准直接聚焦语言模型代理的规则遵循能力，与智能体评估和部署高度相关。

## 基本信息

- 作者：Liudas Panavas, Sebastian Minus, Bradley Monton, Derek Ray, Suhaas Garre, Sushant Mehta, Edwin Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25398v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索的语义片段，并结合启发式草稿进行了补充和修正。
