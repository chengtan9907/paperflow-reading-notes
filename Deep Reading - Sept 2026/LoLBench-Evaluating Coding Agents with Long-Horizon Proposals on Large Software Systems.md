---
user_id: "cheng tan"
paper_id: 14847
arxiv_id: "2609.37143v1"
title: "LoLBench: Evaluating Coding Agents with Long-Horizon Proposals on Large Software Systems"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.37143v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37143v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T13:08:46"
---
# LoLBench: Evaluating Coding Agents with Long-Horizon Proposals on Large Software Systems

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agents · long-horizon tasks · benchmark · code localization

## 一句话总结

LoLBench 是一个面向大型软件系统、以人工撰写长篇幅增强提案（proposal）为任务输入的编码智能体基准，覆盖从用户意图与高层设计感知到代码实现的全流程，评测发现 28 个智能体组合中最佳仅解决 14% 任务，代码定位不完整是主要瓶颈。

## 摘要

> Modern coding agents can deliver increasingly large repository-level changes, and recent benchmarks reflect this by emphasizing long-horizon tasks with large reference implementations. Many benchmarks evaluate coding agents' implementation capability to produce correct code edits from detailed specifications. However, practical modular development tasks also require the perception capability of grounding user intent and high-level design to derive a specification. We introduce LoLBench to evaluate both capabilities through the entire proposal-to-implementation process on large software systems. It is a multilingual benchmark of 100 tasks across 29 software systems in five domains. Each task provides a human-written enhancement proposal with user intent and high-level design. On average, proposals contain about 5,000 words, software systems contain 2.4 million source lines of code (LoC), and implementation pull requests (PRs) change approximately 5,500 LoC. Across 28 agents we evaluated, the best agent resolves only 14% of tasks and achieves a 52.7% Fail-to-Pass (F2P) pass rate. Failure analysis identifies incomplete code localization as a major bottleneck, while providing reference-derived file trees alongside API specifications improves resolved rates by 16--22 percentage points (2.4--17$\times$), reaching at most 34%. These results show that both perception and implementation remain central challenges for coding agents in practical modular development on large software systems. LoLBench is available at https://huggingface.co/datasets/lolbench26/LoLBench.

Q1: 这篇论文试图解决什么问题？

该论文针对的核心问题是：现有编码智能体基准大多从详细规格说明出发评测实现能力，但实际软件工程中的模块化开发往往始于用户意图和高层设计，开发者需要先把模糊意图 grounding 到现有大型代码库中，再制定并执行实现计划。具体来说，论文试图回答以下问题：1）在大型软件系统（平均 240 万行源码）上，从数千词的人工增强提案到完整实现 PR（平均改动约 5500 行），当前最强编码智能体究竟能完成多少；2）感知（把用户意图和高层设计转化为规格与定位）与实现（把规格转化为正确代码编辑）两者中，哪个是当前更主要的瓶颈；3）在给定 API 规格之外，额外提供参考实现导出的文件树等定位信息，能否显著提升解决率；4）失败模式在各阶段（需求理解、任务规划、代码定位、代码编辑、代码验证）的分布，以及不同模型和 scaffold 的差异。论文隐含的立场是：仅评测实现能力会高估智能体在实际模块化开发中的可用性，因为规格本身往往不存在，需要智能体从提案中自行推导；同时，现有长时程基准的参考实现规模虽大，但大多把实现路径的信息（如文件树）直接或间接给出，低估了感知的难度。

Q2: 有哪些相关研究？

论文与以下研究方向相关：1）编码智能体基准，如 SWE-bench Verified（Jimenez et al., 2024）及其后续扩展（Deng et al., 2025；Huang et al., 2026；Li et al., 2025；Zhou et al., 2026），这些基准通过更长的规格说明和更大的参考实现来扩大评估规模；2）面向长时程任务的基准，强调 agent 在大型仓库中做大规模改动，如引入大参考实现和长规格的系列工作；3）NL2Repo-Bench（Ding et al.）的指令格式，论文在补充实验/干预实验时遵循其指令格式；4）代码定位/感知能力研究，包括从 issue 或提案中定位相关文件与编辑位置的方法；5）编码 agent 框架/scaffold，如 Claude Code、Codex、OpenCode、Pi、mini-SWE-agent 等，论文用这些框架做系统化评测；6）specification 生成与需求工程，即从高层设计推导可执行规格；7）测试驱动评估，使用 F2P（Fail-to-Pass）和 P2P（Pass-to-Pass）测试来判定实现正确性。论文与 SWE-bench 系列的关键区别是：不以详细 issue/规格为输入，而以人工撰写、平均 5000 词、包含用户意图和高层设计的增强提案为输入，并把“提案→规格→实现”全流程纳入评测，从而把感知能力作为一等公民纳入评估。

Q3: 论文如何解决这个问题？

论文构建 LoLBench 基准并设计系统化评测协议。任务构造：从 29 个真实软件系统、5 个领域、8 种编程语言中选取 100 个任务；每个任务包含人工撰写的增强提案，平均约 5000 词，涵盖用户意图与高层设计；参考实现来自真实合并的 PR，平均改动约 5500 行。评测环境：使用 Harbor 沙箱执行任务，禁用公网访问以防止作弊；每次尝试有 6 小时墙钟限制，若 1 小时无可见进展则提前终止；每个任务给每个 agent 一次尝试（基础设施故障除外）。Agent 配置：评测 6 个模型（DeepSeek V4 Flash、Opus 5、Kimi K3、GPT-5.6 Sol、GLM-5.2、MiniMax M3），通过 OpenRouter 访问，使用第二高推理强度；搭配 5 个 scaffold（Claude Code、Codex、OpenCode、Pi、mini-SWE-agent），因提供商问题缺少 Codex+Opus 5 和 Claude Code+GPT-5.6 Sol 两个组合，共 28 个 agent。评估指标：Resolved rate（解决率）与 F2P/P2P 测试通过率；解决方案经过 F2P 测试和 P2P 回归检查，但论文明确不单独评估可读性、可维护性、与项目设计约定的一致性。失败分析：把解决失败分解为五阶段失败模式（需求理解、任务规划、代码定位、代码编辑、代码验证），用逐阶段归因表报告各阶段失败占比，并采用最大余数法四舍五入到一位小数，保证各阶段归因值之和等于阶段总计。干预实验：在 API 规格之外，额外向 agent 提供由参考实现导出的文件树，检验代码定位信息对解决率的因果影响；论文指出当前 agent 在规格输入上显著优于抽象提案输入，说明感知是核心短板。

Q4: 论文做了哪些实验？

论文做了四组主要实验：1）主评测：在 100 个 LoLBench 任务上评测 28 个 agent（6 模型 × 5 scaffold 减去 2 个不可用组合），记录 Resolved rate、F2P 通过率和 P2P 回归通过率，每个任务 6 小时限制；2）失败模式分析：把未解决任务按五阶段（需求理解、任务规划、代码定位、代码编辑、代码验证）归因，生成逐阶段失败模式表（按 agent 汇总）和阶段汇总表，并报告修复循环（初始方案生成后的验证失败修复）的影响；3）干预/消融实验：在 API 规格基础上附加参考实现导出的文件树，对比解决率提升；论文还提到遵循 NL2Repo-Bench 的指令格式进行规格化输入实验，对比抽象提案与规格输入下的表现；4）按语言与按模型的分解分析：按编程语言（Java、Python、Go、Rust、C/C++、TypeScript、C#）报告 Resolved% 和 Completed%，按模型与 scaffold 交叉展示解决率；图中还报告了“All PLs: 0%”的极端情况，即某些模型在某些 scaffold 下所有语言解决率均为 0。

Q5: 发现了什么实验现象？

实验发现：1）整体解决率极低：28 个 agent 中最佳仅解决 14% 任务，F2P 通过率最高 52.7%，说明从长提案到大型仓库实现的全流程对当前 agent 仍非常困难；2）代码定位不完整是主要瓶颈：失败分析把最大失败归因指向代码定位阶段，抽象增强提案下的 agent 无法全面覆盖编辑位置；3）文件树干预显著有效：在 API 规格之外提供参考实现导出的文件树，解决率提升 16–22 个百分点（2.4–17 倍），最高达到 34%，说明定位信息的缺失是感知瓶颈的核心成分；4）规格 vs 抽象提案差异明显：agent 在详细规格输入上的表现显著优于抽象提案输入，说明感知（proposal→specification）是当前短板；5）模型与 scaffold 交互显著：不同 scaffold 下同一模型的解决率差异很大，Claude Code、Codex、OpenCode、Pi、mini-SWE-agent 的排名并不一致；个别模型在某些 scaffold 下所有编程语言解决率均为 0%；6）语言间差异大：Java、Python 等语言的解决率明显高于 Rust、C/C++ 等语言，按语言分解的柱状图显示 Completed 与 Resolved 之间存在较大落差；7）不同阶段失败占比因 agent 而异，但“代码编辑”对剩余 9 个（未定位失败之外的）agent 是最大失败阶段，说明实现能力也远未饱和；8）评测仅以 F2P/P2P 为准，不评估可读性、可维护性和与项目设计约定的一致性，因此即使解决的任务也可能在工程质量上不合格。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：1）缓解感知瓶颈：研究从高层提案自动生成结构化规格（如接口、约束、验收标准）的方法，缩小 proposal-to-specification 的差距；2）定位增强：把参考实现导出的文件树换成更现实的定位信号（如检索增强、agent 自身探索、静态分析、依赖图、调用图），研究无参考信息时的定位能力提升；3）扩展任务规模与领域：当前 100 个任务、29 个系统、5 个领域，可扩展更多语言、更多系统类型（如嵌入式、分布式系统、科学计算软件）和更多提案形式；4）工程属性评估：在 F2P/P2P 之外加入可读性、可维护性、设计约定一致性、性能回归等维度，避免 verifier 分数相同但方案质量差异被掩盖；5）失败模式的形式化与预测：基于五阶段失败归因，训练或分析能预测 agent 将在哪个阶段失败的方法，用于自适应资源分配；6）多轮修复与学习：研究 agent 在验证失败后的 repair loop 中如何利用测试反馈，以及是否可以通过少量示范或反思提升修复效率；7）Scaffold 与模型的联合优化：探索 scaffold 的探索策略（如并行搜索、上下文压缩、文件树引导）与模型的感知能力之间的匹配；8）成本与效率分析：在 6 小时限制下，比较各 agent 的 token 消耗、调用次数与解决率的关系（论文图中出现 MiniMax M3 在多个 scaffold 下耗时明显高于其他模型的现象，可做进一步解释）；9）人类基线对照：邀请开发者完成同样任务，量化人类在感知和实现上的表现上限，为 agent 提供参照。

Q7: 总结一下论文的主要内容

论文提出 LoLBench，一个面向大型软件系统的编码智能体基准，核心创新是把评测从“详细规格→实现”扩展到“用户意图/高层设计（增强提案）→规格推导→实现”的完整流程。基准包含 100 个任务，覆盖 29 个真实软件系统、5 个领域、8 种编程语言；每个任务提供人工撰写的平均约 5000 词的增强提案，软件系统平均 240 万行源码，参考实现 PR 平均改动约 5500 行。评测使用 Harbor 沙箱、禁用公网、6 小时限制，对 6 个最新模型（DeepSeek V4 Flash、Opus 5、Kimi K3、GPT-5.6 Sol、GLM-5.2、MiniMax M3）和 5 个 scaffold（Claude Code、Codex、OpenCode、Pi、mini-SWE-agent）组成的 28 个 agent 组合进行系统评测。结果显示最佳 agent 仅解决 14% 任务、F2P 最高 52.7%，说明从长提案到大型仓库实现的端到端能力仍然很弱。失败分析把失败归因到需求理解、任务规划、代码定位、代码编辑、代码验证五个阶段，发现不完整的代码定位是最主要瓶颈；在 API 规格之外提供参考实现导出的文件树，解决率提升 16–22 个百分点（2.4–17 倍），最高达 34%，验证了定位信息的因果作用。论文还发现 agent 在规格输入上显著优于抽象提案输入，说明感知能力是关键短板；模型与 scaffold 交互明显，部分模型在某些 scaffold 下所有语言解决率为 0%。论文的结论是：在大型软件系统的实际模块化开发中，感知（grounding 用户意图与高层设计）和实现（从规格到正确代码编辑）都仍是编码智能体的核心挑战，现有基准因直接给出规格而低估了感知难度。论文同时承认评测只覆盖 F2P/P2P，不评估可读性、可维护性和设计约定一致性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中 agent 方向（权重 0.10）直接相关：论文系统评测 LLM 编码智能体在长时程、大规模软件系统上的能力，并给出失败归因

## 基本信息

- 作者：Yun Peng, Zihan Wu, Zeyang Zhuang, Xin Zhou, Rui Shu, Xu Han, Chun Yong Chong, Yuan Wang, Jiakun Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37143v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 retrieved_evidence 中 Introduction、Abstract、Method、Results 的语义检索片段，并结合论文摘要与部分图表文本做了补充；heuristic_draft 中 core_method 和 main_contributions 的片段只覆盖了 prompt 模板细节，已在实际字段中扩展到任务构造、评测协议和失败归因；部分精确数值（如 16–22 个百分点对应的统计检验、按语言的具体任务数）在现有证据中不足，已在对应字段标注需回原文确认。
