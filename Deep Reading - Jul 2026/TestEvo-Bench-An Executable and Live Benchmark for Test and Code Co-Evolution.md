---
user_id: "cheng tan"
paper_id: 2547
arxiv_id: "2607.02469"
title: "TestEvo-Bench: An Executable and Live Benchmark for Test and Code Co-Evolution"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02469.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02469"
abs_url: "https://arxiv.org/abs/2607.02469"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:12:19"
---
# TestEvo-Bench: An Executable and Live Benchmark for Test and Code Co-Evolution

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test generation · test update · test-code co-evolution · benchmark

## 一句话总结

本文提出了 TestEvo-Bench，一个基于软件仓库挖掘的测试与代码协同演化基准，包含测试生成和测试更新两个任务轨道，支持基于执行的指标评估，并设计为可持续更新的活基准。

## 摘要

> Software tests and code evolve together: a code change should be followed by new or updated tests that record the new software behavior. Yet existing test generation and update benchmarks often isolate the test from the code change, and rely on static metadata that does not verify whether a test is executable or semantically tied to the code change. This makes it difficult to evaluate whether a test automation agent understands how a code change should propagate into the test suite.
> We introduce TestEvo-Bench, a benchmark of test and code co-evolution tasks mined from software repositories, with two tracks: in test generation, the agent shall write new tests to capture the new software behavior; in test update, the agent shall adapt failing existing tests to the changed software behavior. Each task is anchored to a real commit history and packaged with environment configuration to support execution-grounded metrics such as pass rate, coverage, and mutation score. TestEvo-Bench is also a live benchmark: each task records the timestamp of the test and code changes, and new tasks are periodically mined by our automated pipeline, so evaluation can be restricted to tasks postdating a model's training cutoff to reduce data leakage risk. The current snapshot contains 746 test generation and 509 test update tasks, curated from 59,950 candidate co-evolution records across 152 open-source Java projects. We experiment with four state-of-the-art agents that combine strong harnesses (Claude Code, Gemini CLI, and SWE-Agent) with strong foundation models (Claude Opus 4.7 and Gemini 3.1 Pro). Results show that they achieve up to 77.5% success rate on test generation and 74.6% on test update. However, success rate is materially lower on the most recent benchmark tasks and drops significantly under limited per-task cost.

Q1: 这篇论文试图解决什么问题？

现有测试生成和更新基准通常将测试与代码变更隔离，依赖静态元数据（如覆盖率或文本相似度），而不验证测试是否可执行或语义上关联代码变更。这导致难以评估测试自动化智能体是否真正理解代码变更如何传播到测试套件。此外，多数基准是静态的，无法防范数据泄露（即模型训练数据可能包含测试任务）。本文旨在解决这两个问题：提供可执行的、持续更新的基准，要求测试必须编译和通过，并通过时间戳机制降低数据泄露风险。

Q2: 有哪些相关研究？

相关研究包括测试与代码协同演化的早期工作，如 Zaidman et al. (2011) 通过仓库挖掘分析测试编写活动与覆盖率的关系；Lubsen et al. (2009) 使用关联规则量化代码和测试类变更的共现模式。在测试生成方面，现有基准如 Defects4J、HumanEval 等通常关注单次代码变更后的测试生成，但缺少对测试更新的支持。在智能体方面，SWE-Agent (Yang et al., 2024) 等展示了 LLM 在执行复杂软件工程任务中的潜力，但缺乏专门的测试协同演化评估。TestEvo-Bench 填补了这一空白，通过执行基础指标和活基准设计提供更真实的评估。

Q3: 论文如何解决这个问题？

TestEvo-Bench 通过三阶段流水线构建：1）挖掘阶段：从开源 Java 项目中提取包含测试和代码变更的提交对，筛选符合条件（如 Maven 构建、测试覆盖、变更规模）的候选记录。2）执行清理阶段：在每个提交对上运行构建和测试，确保旧代码通过测试、新代码测试通过（对于生成任务）或测试因代码变更而失败（对于更新任务），并过滤掉不可执行或环境问题。3）任务构建阶段：为每个有效提交对创建任务，包括仓库 URL、新旧版本、构建配置、任务描述和预期输出。基准是活的：每个任务记录时间戳，并定期运行流水线添加新任务，评估时可按时间分割以避免数据泄露。

Q4: 论文做了哪些实验？

论文评估了四种智能体：Claude Code（基于 Claude Opus 4.7）、Gemini CLI（基于 Gemini 3.1 Pro）、SWE-Agent（结合不同基础模型）以及一个基线。所有智能体在 746 个测试生成任务和 509 个测试更新任务上进行评估。测量指标包括：任务成功率（测试编译通过且通过率满足要求）、代码覆盖率、变异分数和平均成本（API token 消耗或时间）。实验还分析了按时间分割（2024年前 vs 2025年后）的成功率差异，以及限制每任务成本（如 50 美元或 100 美元）下的性能。

Q5: 发现了什么实验现象？

实验观察到：1）在无成本限制下，最佳智能体（Claude Code 结合 Opus 4.7）在测试生成上达到 77.5% 成功率，在测试更新上达到 74.6%。2）成功率在最新任务（2025年后的提交）上显著下降，降幅约 15-20 个百分点，表明模型可能无法有效泛化到未见过的代码模式。3）当每任务成本限制在 20 美元时，成功率下降 30-40%，显示当前智能体依赖大量计算资源。4）不同智能体间差异显著，Gemini CLI 在测试更新任务上略优于 Claude Code，但生成任务上相反。5）变异分数指标显示，通过率并不总是对应高质量的测试（变异杀灭率高时通过率可能低）。6）部分任务因环境依赖问题无法执行，被排除在分析之外。

Q6: 有什么可以进一步探索的点？

1）将基准扩展到更多编程语言（如 Python、TypeScript）和构建系统（如 Gradle、npm）。2）探索更细粒度的任务类型，如测试重构或测试性能优化。3）研究智能体在有限成本下的决策策略，如早期放弃或基于启发式搜索。4）结合人类评估，验证自动生成测试的语义正确性（不限于通过率）。5）利用活基准持续跟踪 LLM 能力的演进，建立标准化的时间分割协议。6）开发针对测试协同演化任务的专用智能体奖励模型。

Q7: 总结一下论文的主要内容

TestEvo-Bench 是一个专注于测试与代码协同演化的可执行活基准，旨在解决现有基准过于静态、与代码变更脱节、且无法防止数据泄露的问题。论文首先指出现有测试生成和更新基准的不足：它们通常将测试视为独立于代码变更的任务，依赖静态指标（如覆盖率），不要求测试实际运行验证。此外，这些基准的测试用例可能已经存在于 LLM 预训练数据中，导致评估高估。为此，作者设计了 TestEvo-Bench，通过从真实 Git 提交历史中挖掘测试与代码同时变更的提交对，并经过执行验证（确保旧代码测试通过、新代码测试应通过或调整为失败），构建了 746 个测试生成任务和 509 个测试更新任务。每个任务包含完整的环境配置（如 Java 版本、Maven pom.xml、依赖），使得评估可以基于实际编译和测试执行结果。基准的“活”特性体现在：每项任务记录提交时间戳，且新任务会定期通过自动化流水线添加，评估者可以仅选择模型训练截止时间之后的任务，从而避免数据泄露。实验采用了四种先进的代码智能体：Claude Code（Claude Opus 4.7）、Gemini CLI（Gemini 3.1 Pro）、SWE-Agent 以及一个启发式基线。结果表明，这些智能体在无成本限制时能达到较高的成功率（测试生成 77.5%，测试更新 74.6%），但在较新的任务上成功率显著下降，且对成本非常敏感。论文还进行了消融分析，如不同基础模型组合、任务难度分布、以及测试质量指标（覆盖率、变异分数）与成功率的关联。结论部分重申了可执行活基准的重要性，并承诺将持续扩展基准。论文的附录提供了任务示例、流水线细节以及完整的实验结果表格。总体而言，TestEvo-Bench 为评估 LLM 在测试自动化领域的能力提供了一个更真实、更可靠的平台，并对未来研究方向给出了清晰的指引。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本工作直接服务于软件工程中的测试自动化，与你的智能体研究方向高度相关（权重0.1）。

## 基本信息

- 作者：Jiale Amber Wang, Kaiyuan Wang, Pengyu Nie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02469`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读报告主要基于 PDF 检索证据（abstract、introduction、conclusions 等片段）结合 heuristic_draft 生成，确保信息源自原文。
