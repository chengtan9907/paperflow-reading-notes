---
user_id: "cheng tan"
paper_id: 3236
arxiv_id: "2607.08124"
title: "TTHE: Test-Time Harness Evolution"
publish_date: "2026-07-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.08124.pdf"
pdf_url: "https://arxiv.org/pdf/2607.08124"
abs_url: "https://arxiv.org/abs/2607.08124"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-11T00:33:02"
---
# TTHE: Test-Time Harness Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test-time adaptation · LLM agent · harness evolution · execution trace

## 一句话总结

TTHE 是一种在测试时通过未标记执行轨迹演化 LLM agent 可执行控制程序（harness）的方法，无需更新模型权重或依赖真实标签。

## 摘要

> The behavior of an LLM agent is determined not only by the underlying model, but also by its harness: the executable program that constructs context, invokes tools, verifies intermediate results, and recovers from failures. Existing approaches optimize such harnesses before deployment, searching training or development data for a fixed agent workflow that is then frozen at test time. This limits adaptation when the test distribution, failure modes, or tool interactions differ from those seen during development. We ask whether the harness can instead be optimized during evaluation itself, using only the unlabeled execution traces the agent produces on the test inputs. We introduce Test-Time Harness Evolution (TTHE), which treats the executable harness as the state of test-time adaptation. During evaluation, TTHE maintains a population of candidate harnesses and refines them through an agentic proposer that reasons over their execution traces, without gold labels or task-specific supervision; a judge then commits an improved harness from execution-derived proxy signals, and the selected program persists to govern subsequent inputs. Crucially, TTHE does not update model weights, require gold labels, or train a separate adaptation model: solver, proposers, and judge are different roles and harnesses around the same frozen LLM, so all adaptation occurs through changes to the surrounding program. Across text-to-SQL, competitive programming, software engineering, data-science coding, and agentic tool-use tasks, TTHE improves fixed ReAct-style baseline harnesses, yielding persistent, inspectable improvements rather than a pre-searched workflow or per-query retries. These results recast test-time adaptation for LLM agents as evolution over executable control programs and identify execution-derived proxy reliability as a central challenge for robust unsupervised agent improvement.

Q1: 这篇论文试图解决什么问题？

LLM agent 的行为由底层模型和周围的可执行 harness（构造上下文、调用工具、验证中间结果、故障恢复的程序）共同决定。当前主流方法（如 ReAct、AutoGPT）通常在部署前离线搜索或人类设计一个固定的 agent 工作流，并在测试时冻结该工作流。但测试时的任务分布、工具行为、失败模式往往与开发环境存在差异，固定工作流无法适应。现有测试时适应方法多关注模型权重调整（如 TTA、扩展上下文）或每查询重试，但缺乏对 harness 本身的持续改进。核心问题：如何在不获取新标签、不更新模型权重、不训练额外适应模型的前提下，仅利用 agent 在测试输入上产生的未标记执行轨迹来演化 harness，使其更好地适应当前任务分布或环境？

Q2: 有哪些相关研究？

1. **Agent 框架与 Harness 设计**：ReAct、Reflexion、AutoGPT 等定义了推理‑行动循环，但它们的 harness 通常是手工固定的，缺乏测试时动态调整。 2. **测试时适应（Test-Time Adaptation）**：传统 TTA 通过自监督目标调整模型参数或归一化统计量（Sun et al., 2020; Wang et al., 2021; Zhang et al., 2022），但本文不触及模型权重。 3. **进化计算与程序合成**：EvoTest（He et al., 2025）通过进化搜索适应 agent 的配置空间，但其每轮交互后可修改，且依赖预定义配置空间。Cai et al. (2026) 利用强外部编码模型从失败轨迹修复源码，但需要外部标签/修复信号。 4. **基于执行反馈的调试**：大多数方法依赖 human-in-the-loop 或 oracle 反馈，TTHE 则利用执行衍生的代理信号实现无监督演化。

Q3: 论文如何解决这个问题？

TTHE 将 harness 视为测试时适应状态。核心流程：
- **Harness 定义**：可执行控制程序，指定上下文构建、工具调用、中间验证、故障恢复逻辑，在本文中表示为伪代码或 Python 函数片段。
- **种群维护**：在测试时保持多个候选 harness 的种群（例如 4 个），初始化为一个基础 ReAct 样式 harness。
- **测试阶段**：对每个测试输入，从种群中选出一个 harness（如按轮次或随机），运行 solver（冻结 LLM）生成执行轨迹（含中间输出、工具调用结果、错误等）。
- **Proposer（提议者）**：同样是冻结 LLM，以选中的 harness 和当前测试的执行轨迹作为上下文，推理并提出 harness 改进方案（如修改验证条件、增加重试逻辑、调整上下文构造方式）。输出是一个新的 harness 程序。
- **Judge（裁判）**：评估 Proposer 提出的新 harness。由于无真实标签，Judge 使用执行衍生的代理信号（例如任务是否成功完成、编译/运行通过的测试数、中间步骤的合理性，多种启发式信号组合）给出得分。若新 harness 的代理得分高于当前种群中的最差 harness，则替换之。
- **演化循环**：上述过程在测试流上不断重复，harness 种群逐渐优化，且改进可以在后续输入中持久使用。整个过程不更新模型权重，不使用真实标签（仅用于最终评估），不训练额外适应模型。

Q4: 论文做了哪些实验？

论文在五个执行密集型任务上评测 TTHE：
1. **Text-to-SQL**：在 Spider 数据集上，衡量执行准确率。
2. **竞赛编程**：来自 Codeforces 的问题，评测通过测试用例的比例。
3. **软件工程**：基于 SWE-bench 的 bug 修复任务，判断是否正确修复。
4. **数据科学编码**：涉及数据分析、可视化、模型训练等综合任务。
5. **Agentic 工具使用**：在支持多工具调用的环境（如网页浏览、API 调用）中评测成功率。
基线包括固定 ReAct 样式 harness、随机重试、Reflexion 等。对比时，TTHE 与基线的 solver 使用相同冻结 LLM（如 GPT-4o）。

Q5: 发现了什么实验现象？

1. **持续改进**：TTHE 在所有五个任务上系统性地提升了固定 ReAct 基线的性能，并且改进幅度随测试实例数增加而增长（而非每查询独立）。
2. **可检视策略**：演化出的 harness 可读，可检查具体在哪些环节（如验证、重试策略、上下文构造）发生了变化。
3. **代理信号的有效性**：单一的代理信号（如通过测试数）可以提供有效选择压力，但信号可靠性是核心挑战：当代理信号与真实目标不完全对齐时，可能引入次优更新。
4. **计算开销**：TTHE 引入额外的 LLM 调用（proposer 和 judge），但通过种群管理和延迟选择机制控制成本，平均每个任务的额外 LLM 调用数约为基线的 2-3 倍。

Q6: 有什么可以进一步探索的点？

1. **更可靠的执行衍生代理信号**：设计更鲁棒的、与真值对齐的代理信号，减少错误选择。
2. **混合进化与学习**：结合 TTHE 与模型微调，可能取得互补增益。
3. **复杂 harness 表示**：当前 harness 为简单脚本，可探索结构化程序或神经‑符号表示。
4. **多任务共享适应**：在一个测试流中跨任务类型共享 harness 演化经验。
5. **理论研究**：分析 TTHE 的收敛性与种群动态，理解为何代理信号能引导有效改进。
6. **扩展至具身 agent**：将 harness 适应于机器人控制等物理交互环境。

Q7: 总结一下论文的主要内容

论文提出 Test-Time Harness Evolution (TTHE)，一种在测试时通过未标记执行轨迹演化 LLM agent 周围的可执行程序（harness）的新范式。现有 agent 系统通常依赖预定义的固定工作流，但测试时环境变化导致性能下降。TTHE 将 harness 视为适应状态，维护一个候选 harness 种群，利用 agent 自身的执行轨迹（无需真实标签）通过 Proposer 和 Judge 构成进化循环。Proposer 分析当前 harness 与执行轨迹并生成改进版本，Judge 利用执行衍生的代理信号评估并选择更好的 harness 进入种群，从而实现对后续输入的持久适应。整个过程中 solver、proposer、judge 皆使用相同冻结 LLM，不更新权重、不依赖额外标记数据。在 text-to-SQL、竞赛编程、软件工程、数据科学编码、agentic 工具使用五个执行密集型任务上，TTHE 稳定改进 ReAct 基线，产出可检视的策略。论文还分析了代理信号可靠性这一核心挑战。该工作将 LLM agent 的测试时适应重定义为可执行控制程序的演化，揭示了无监督 agent 改进的关键问题。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户方向「agent」直接相关：论文聚焦 LLM agent 的测试时适应，提出新颖的 harness 演化范式，是该方向的重要进展。

## 基本信息

- 作者：Jun Nie, Yonggang Zhang, Jun Song, Qianshu Cai, Dahai Yu, Yike Guo, Xinmei Tian, Bo Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.LG
- 日期：2026-07-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.08124`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 文本检索得到的证据片段，并结合摘要和启发式草稿进行了补全。
