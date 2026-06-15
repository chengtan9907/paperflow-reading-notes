---
user_id: "cheng tan"
paper_id: 146
arxiv_id: "2606.13643v1"
title: "Recursive Agent Harnesses"
publish_date: "2026-06-11"
pdf_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/arXiv - Jun 2026/2606.13643v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.13643v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
report_version: "2026-06-04-v5"
daily_note_path: "/Users/mario/Documents/Obsidian Vault/Daily Note/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-15T01:59:41"
---
[[Daily Note - Jun 2026]]

- PDF：[https://arxiv.org/pdf/2606.13643v1](https://arxiv.org/pdf/2606.13643v1)

# Recursive Agent Harnesses

- PDF: https://arxiv.org/pdf/2606.13643v1
- 原文: https://arxiv.org/abs/2606.13643v1

> ★★☆☆☆ 按需阅读 · 约 20 分钟 · 模型 openai-compatible/gemini-3-flash-preview · 证据 PDF 全文 + 元数据

🏷 关键词：recursive agent harness · long-context reasoning · harness recursion · autonomous agents

## 一句话总结

论文提出了递归智能体装具（Recursive Agent Harness, RAH），通过将递归单元从单纯的模型调用扩展为包含工具和文件系统的完整智能体环境，显著提升了超长上下文下的复杂推理性能。

## 摘要

> Recursive language models (RLMs) showed that recursion over model calls is an effective strategy for long-context reasoning, and production coding agents have begun to write code that spawns subagents at scale, most recently in Anthropic's dynamic workflows. We name and study the pattern between these two lines of work, where the recursive unit is a full agent harness with filesystem tools, code execution, and planning rather than a model call with no tools. We call this the Recursive Agent Harness (RAH) and frame it as harness recursion, the code-first extension to the model recursion of RLMs. A parent agent generates and runs an executable script that spawns subagent harnesses in parallel for fine-grained workloads and uses structured function calls for small subtasks. We provide a controlled evaluation on long-context reasoning. With the backbone held fixed at GPT-5 to match the published Codex and RLM baselines, RAH improves the Codex coding-agent baseline from 71.75% to 81.36% on Oolong-Synthetic (199 samples, 13 context-length buckets up to 4M tokens), a gain attributable to the harness rather than the model. With a stronger backbone, Claude Sonnet 4.5, the same design reaches 89.77%.

Q1: 这篇论文试图解决什么问题？

该研究旨在解决长文本推理任务中，当需要对数千个独立条目进行逐一深度推理时，现有架构存在的效能瓶颈。目前的解决方案存在两个极端：
1. 传统的编程智能体（Coding Agents）通常将条目处理简化为正则表达式或简单的启发式循环，缺乏对复杂子任务的深度推理能力。
2. 递归语言模型（RLMs）虽然引入了递归机制，但其递归单元仅为不带工具的模型调用，无法利用文件系统或外部工具处理复杂的中间步骤。

当任务要求智能体在数百万 token 的上下文中进行跨条目分析、代码验证或多步规划时，单一的模型调用或固定的正则逻辑难以兼顾灵活性与准确性。论文试图通过引入“装具级”的递归来填补这一空白。

Q2: 有哪些相关研究？

论文将 RAH 定位为递归语言模型（RLMs）在代码层面的扩展。相关研究包括：
1. 递归语言模型（如 Zhang et al. [31]）：提出了模型调用的递归，但受限于缺乏外部工具支持。
2. 编程智能体框架（如 OpenDevin, SWE-agent）：提供了文件系统和工具装具，但在处理超长上下文任务时通常采用扁平化或固定的工作流。
3. 动态工作流（如 Anthropic 的实现）：在生产环境中已经开始应用类似的子智能体生成模式，但缺乏学术上的系统定义和受控实验验证。
4. 长文本基准测试：如 Oolong 和 AgentBench，为评估智能体在极端上下文下的表现提供了基础。

Q3: 论文如何解决这个问题？

核心方案是“装具递归”（Harness Recursion），其技术路线包含以下关键点：
1. 递归单元升级：将递归的最小操作单元从“LLM Call”提升为“Agent Harness”。这意味着每个子节点都拥有与父节点同等的工具访问权和执行环境。
2. 代码驱动生成（Code-Execution Spawning）：父智能体不通过预定义的 JSON Schema 调用子任务，而是编写一段 Python 脚本。该脚本在运行时通过装具 API 动态启动多个子智能体。这种方式允许智能体根据实时负载动态调整并行度和任务结构。
3. 结构化汇总：子智能体完成任务后，其路径和结果由父脚本进行聚合，最终生成完整答案。这种深度递归允许子智能体根据需要继续生成“孙智能体”，形成多层级的推理树。
4. 灵活性：相比于固定的工具调用，RAH 允许智能体在发现复杂条目时，自主决定是使用简单的正则处理还是启动一个完整的子智能体进行深度分析。

Q4: 论文做了哪些实验？

论文在 Oolong-Synthetic 数据集上进行了受控评估：
1. 数据规模：包含 199 个样本，覆盖了从短文本到 400 万 token 的 13 个上下文长度桶（Buckets）。
2. 实验控制：为了确保性能提升归功于架构而非模型，实验将骨干模型固定为 GPT-5，并与已发表的 Codex 编程智能体和 RLM 基准进行对比。
3. 性能表现：在 GPT-5 底座下，RAH 显著优于 Codex 基准（81.36% vs 71.75%）。在更换为更强的 Claude Sonnet 4.5 后，准确率提升至 89.77%。
4. 分析维度：实验涵盖了不同答案类型和上下文长度，验证了 RAH 在处理超大规模数据时的鲁棒性。

Q5: 有什么可以进一步探索的点？

论文提出了几个值得进一步探索的方向：
1. 递归深度与成本优化：研究在多层递归下，如何平衡推理深度带来的准确率提升与计算成本之间的关系。
2. 异构智能体协作：探索在 RAH 框架下，父智能体调用不同能力的专用子智能体（如专门负责检索或专门负责代码审计的智能体）的效果。
3. 自动化装具演进：研究智能体是否能根据任务需求，自主修改或扩展其子智能体的装具配置（如动态添加新工具）。
4. 评估指标改进：针对数值类问题在递归中容易失分的问题，开发更具鲁棒性的智能体推理评估协议。

Q6: 总结一下论文的主要内容

本文系统性地提出并验证了“递归智能体装具”（RAH）架构。该架构的核心创新在于将递归机制从单纯的模型预测提升到了系统装具层面。在处理超长上下文（最高 4M tokens）的复杂推理任务时，RAH 允许父智能体通过编写代码动态地生成并运行具备完整工具能力的子智能体。这种“装具递归”模式克服了传统编程智能体逻辑僵化和递归语言模型能力受限的问题。通过在 Oolong-Synthetic 基准上的受控实验，作者证明了 RAH 架构在相同模型底座下能显著提升推理准确率，并展示了其在 Claude Sonnet 4.5 等前沿模型上的强大潜力。该研究为构建能够处理海量数据和复杂逻辑的自主智能体系统提供了重要的理论支撑和实践路径。

## PDF 证据定位

- 研究背景：
  Abstract | score=1.201 | designed for long-context reasoning, such as Oolong, expose the Recursive language models (RLMs) showed that recursion over gap that motivates the comparison. Coding agents score…
  Introduction | score=1.154 | Scope of contribution. We make three contributions. First, we Modern coding agents operate within an agent harness, the tools, name Recursive Agent Harnesses and define harness…
  Introduction | score=1.137 | controlled evaluation dress. AgentBench [11] provides a multi-environment benchmark against model recursion that a product feature does not provide. for evaluating LLMs as…
- 核心方法：
  Introduction | score=0.995 | s-check the generates at runtime, adapting structure to the workload rather than two rather than committing to regex alone. Zhang et al. [31] introapplying a predetermined…
  Introduction | score=0.983 | ty as its parent, so the path, which the parent script aggregates into a final answer afterdecomposition is recursive rather than one level of fan-out. all subagent tasks…
  Introduction | score=0.958 | agents at scale and execute the LLMs can write and execute programs as a form of persistent skill orchestration as code rather than turn by turn [2]. This is the same…
- 主要结果：
  Introduction | score=0.953 | nswer types over a long-context task, should the recursive unit be a model call and context lengths, and relate the pattern to Anthropic’s dynamic with no tools or a full…
  Introduction | score=0.908 | hercall and the harness runs the subagent. JSON tool calling is capped than a model call. A subagent that meets a complex entry can writeby the per-turn parallel tool-call…
  Introduction | score=0.907 | ndividual wall-clock profiles for the GPT-5 configuration and leave precise design choices such as recursion depth, the number of entries per cost characterization to future…
- 局限性：
  Introduction | score=0.908 | the task as retrieval. Second, NUMERIC questions sands of independent entries, and prior approaches provide only lose score even when subagent reasoning is sound, because the…
  Introduction | score=0.904 | xt lengths including 4M tokens. Per-bucket a regex loop scores 81.36% when the recursive unit is a full harestimates rest on 14 to 16 instances and carry wide intervals. The…
  Introduction | score=0.894 | attern is already appearing in production weakness. systems such as dynamic workflows. Our aim is to name it, place it in the lineage of recursive language models, and measure…
- 相关性：
  Introduction | score=1.099 | agent loop, alternating between planning, toolaction rather than a fixed tool schema, following the code-as-action use, and reflection until a stopping condition is met [26, 29].…
  Introduction | score=1.097 | 026. Recursive Language Models. Zihan Yao, Rui Zhang, Jie Jia, Jie Tang, Yuxiao Liu, and Yuxiao Dong. 2024. https://arxiv.org/abs/2512.24601 AgentBench: Evaluating LLMs as…
  Introduction | score=1.086 | Agent Prompt 199-instance evaluation the extraction step succeeded on every The parent agent runs a general-purpose harness prompt. It coninstance, so the fallback was never…

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该研究直接关联智能体（Agent）架构的前沿探索，特别是递归与动态工作流方向。

## 基本信息

- 作者：Elias Lumer, Sahil Sen, Kevin Paul, Vamse Kumar Subbiah
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-11
- 推荐级别：**按需阅读**
- 预计阅读时间：约 20 分钟
- 解析来源：PDF 全文 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.13643v1`

## 代码与资源

- 代码：暂未发现公开链接
- 数据：暂未发现公开链接
- 项目主页：暂未发现公开链接
- 原文 PDF：https://arxiv.org/pdf/2606.13643v1
- arXiv：https://arxiv.org/abs/2606.13643v1
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索证据，特别是 Introduction 和 Abstract 中关于 RAH 定义、实验数据及与 RLM 对比的关键片段。由于 Method 和 Results 部分在检索片段中信息较碎，已结合 heuristic_draft 进行了逻辑补全和校准。
- 方法证据锚点：Introduction | score=0.995 | s-check the generates at runtime, adapting structure to the workload rather than two rather than committing to regex alone. Zhang et al. [31] introapplying a predetermined…
- 结果证据锚点：Introduction | score=0.953 | nswer types over a long-context task, should the recursive unit be a model call and context lengths, and relate the pattern to Anthropic’s dynamic with no tools or a full…
