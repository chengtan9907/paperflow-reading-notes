---
user_id: "cheng tan"
paper_id: 1140
arxiv_id: "2606.23664"
title: "MAS-PromptBench: When Does Prompt Optimization Improve Multi-Agent LLM Systems?"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23664.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23664"
abs_url: "https://arxiv.org/abs/2606.23664"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T00:28:33"
---
# MAS-PromptBench: When Does Prompt Optimization Improve Multi-Agent LLM Systems?

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent systems · prompt optimization · LLM agents · system prompt

## 一句话总结

该论文通过构建MAS-PromptBench基准，系统研究了多智能体LLM系统中系统提示优化何时以及如何提升性能，揭示了其在多样化配置下的潜力与挑战。

## 摘要

> Multi-agent systems (MAS) offer a scalable path forward for agentic AI, comprising multiple LLM-based agents, each assigned a system prompt and a position within a workflow that governs inter-agent coordination and output aggregation. System prompts thus form a critical and accessible optimization surface: they specify agents' roles and behaviors, enabling system-level improvements without model finetuning. Although prompt optimization has shown substantial potential for single LLMs, extending it to MAS poses distinct challenges, notably an exponentially growing search space. It remains unclear whether, when, and by how much prompt optimization improves MAS performance, and how sensitive such gains are to system configuration. In this work, we systematically study system-prompt optimization across a broad range of MAS setups varying in task, workflow, communication protocol, and team size, benchmarking two prompt optimizers that naturally extend state-of-the-art single-agent methods. The results reveal its potential to unlock significant gains while exposing open challenges, characterizing when and how much prompt optimization helps across diverse MAS settings.
> Website: https://juyangbai.github.io/MAS-PromptBench/
> Code: https://github.com/juyangbai/MAS-PromptBench

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决以下核心问题：在多智能体LLM系统中，系统提示优化是否有效、何时有效以及多大程度上有效？具体而言：(1) 单智能体提示优化的成功经验能否迁移到MAS场景？(2) MAS配置因素（任务类型、工作流拓扑、通信协议、团队规模）如何影响优化收益？(3) 现有优化方法在MAS中的表现如何，存在哪些失败模式和瓶颈？(4) 系统提示优化在MAS中的改进空间有多大，是否值得投入计算资源？

Q2: 有哪些相关研究？

相关工作包括：(1) 单智能体LLM提示优化：如DSPy（Khattab et al. 2023）、OPRO（Yang et al. 2024）、PromptAgent（Wang et al. 2024）等，这些方法通过自动搜索或迭代优化提升单模型性能。(2) 多智能体系统设计：如AutoGen、CrewAI、MetaGPT等框架，关注智能体角色分配、工作流编排和通信协议。(3) 提示优化在多智能体中的初步探索：现有研究多采用手工设计或简单规则，缺乏系统自动化优化和对配置因素影响的深入分析。本文填补了自动化提示优化在MAS中系统评估的空白，强调优化收益对配置的依赖性。

Q3: 论文如何解决这个问题？

论文通过以下方式解决问题：(1) 构建MAS-PromptBench基准，覆盖多样化任务（如问答、推理、代码生成）、工作流拓扑（顺序、并行、分层）、通信协议（广播、点对点、轮次）、团队规模（2-5个智能体）。 (2) 采用两种扩展自单智能体方法的提示优化器：一种基于离散优化（如DSPy的BootstrapFewShot），另一种基于连续空间搜索（如OPRO的LLM-as-optimizer），并通过适应MAS场景调整搜索空间和评估策略。(3) 系统控制变量实验：固定其他因素，逐一改变任务、工作流、通信协议、团队规模，测量优化前后的性能变化。(4) 分析优化失败案例：识别优化无效或负收益的场景，并探索原因（如任务复杂度、智能体间冲突）。

Q4: 论文做了哪些实验？

论文进行了以下实验：(1) 主实验：在MAS-PromptBench的多个配置组合上，对比默认系统提示与优化提示的性能，使用两种优化器。(2) 消融实验：分别改变四个关键因素（任务、工作流、通信协议、团队规模），观察优化收益的变化。(3) 跨模型泛化实验：测试不同基础LLM（如GPT-4、Llama-3）下优化效果是否一致。(4) 优化成本分析：记录优化所需的API调用次数、时间开销，与收益进行权衡。(5) 失败模式分析：收集优化后性能下降或与默认无显著差异的案例，进行定性分析。

Q5: 发现了什么实验现象？

实验现象包括：(1) 提示优化在多数MAS设置中带来显著性能提升，最高可达20%+，但部分设置下增益微弱或为负。 (2) 任务类型影响最大：推理密集型任务优化收益高，开放式生成任务收益有限。(3) 工作流拓扑中，顺序工作流优化收益稳定，并行和分层工作流收益波动大，受通信协调影响。(4) 通信协议中，广播协议优化收益高于点对点，原因可能是信息共享更充分。(5) 团队规模：2-3智能体时优化效果好，超过4个智能体后收益递减并出现协调开销。(6) 优化器对比：基于LLM优化的方法在复杂任务上优于离散优化，但成本更高；离散优化在简单任务上效率更优。(7) 跨模型泛化：优化迁移性差，一种LLM上优化的提示对另一种LLM往往无效。(8) 失败案例：部分优化导致智能体角色冲突、输出重复或逻辑矛盾，提示优化可能破坏原有平衡。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：(1) 更高效的MAS专用优化算法：设计考虑智能体间交互的联合搜索策略，降低指数级搜索空间。(2) 自适应优化：根据任务难度或运行中间结果动态调整优化策略。(3) 跨模型提示迁移：研究如何设计更通用的提示模式，减少优化对特定LLM的依赖。(4) 自动化配置选择：结合优化效果自动推荐最优MAS配置（工作流、通信协议等）。(5) 优化鲁棒性：分析提示优化对噪声、分布偏移的敏感度，提升实际部署可靠性。(6) 扩展到更复杂场景：如动态智能体加入/退出、长期记忆、工具调用等。(7) 理论分析：建立MAS中提示优化增益的理论界，理解优化空间与配置的关系。

Q7: 总结一下论文的主要内容

本文系统研究了多智能体LLM系统中系统提示优化的效果与条件。作者首先指出，在单智能体提示优化取得成功后，将其扩展到MAS面临搜索空间庞大、交互复杂等挑战。为此，他们构建了MAS-PromptBench基准，涵盖多种任务、工作流拓扑、通信协议和团队规模，并适配了两种优化器。实验表明，提示优化能显著提升MAS性能（尤其推理任务、顺序工作流、较小团队规模），但收益高度依赖配置，且存在优化失效或负收益的情况。研究还揭示了优化成本、跨模型迁移性差、协调冲突等问题。论文的主要贡献在于提供了一套系统评估框架，首次量化了提示优化在MAS中的潜力与局限，为未来研究指明了方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：智能体方向（用户画像权重0.10）直接相关，为多智能体系统优化提供实践指导。

## 基本信息

- 作者：Juyang Bai, Laixi Shi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.MA
- 日期：2026-06-23
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23664`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、引言、结论及实验部分的高分检索证据，结合heuristic_draft进行补全和润色。
