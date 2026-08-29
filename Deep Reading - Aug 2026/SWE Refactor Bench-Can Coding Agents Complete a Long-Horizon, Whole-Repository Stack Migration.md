---
user_id: "cheng tan"
paper_id: 9584
arxiv_id: "2608.23564v1"
title: "SWE Refactor Bench: Can Coding Agents Complete a Long-Horizon, Whole-Repository Stack Migration?"
institution: "Navers Lab, Einsia.AI; 清华大学（依据首页脚注推断）"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23564v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23564v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:25:12"
---
# SWE Refactor Bench: Can Coding Agents Complete a Long-Horizon, Whole-Repository Stack Migration?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agents · software migration · benchmark · technical debt

## 一句话总结

本论文提出 SWE Refactor Bench，一个包含 20 个整库栈迁移任务、覆盖 4 类技术债的基准，并通过三阶段评估协议（迁移审计、行为测试、智能体验证）同时测量迁移完整性与行为正确性，发现当前前沿编码智能体在 520 次运行中仅有 5.4% 通过全部阶段。

## 摘要

> Modern software systems accumulate technical debt over decades of development, which makes migration expensive and largely manual. As coding agents become increasingly capable at bug fixing, can they autonomously perform such migrations? Existing benchmarks cannot answer this question because they evaluate only behavioural correctness, not whether the migration actually occurred. This leads an easy hack: agents copy the original implementation to make tests pass. We call this Blindness. To address this problem, we introduce SWE Refactor Bench, a benchmark comprising 20 whole-repository migrations, covering 4 kinds of technical debt. A three-stage evaluation protocol measures both migration completeness and behavioural correctness. (1) Migration Audit verifies that the migration occurred. (2) Behavioural Tests measure correctness with a fixed test suite. (3) Agentic Verification uses 6 independent coding agents to generate targeted tests for hidden behavioural differences. Across 520 runs from 8 frontier models and 26 model-effort configurations, only 28 of 520 runs ($5.4\%$) pass all three stages, 13 of the 20 tasks receive no accepted solution, and the best model (claude-opus-5) scores $47.0/100$. Migration completeness and behavioural correctness are distinct abilities: a few runs preserve behaviour by skipping the migration and are stopped at Migration Audit; most attempt it and break behaviour, and are stopped at Behavioural Tests. Agents cannot deliver a perfect migration: among the 340 runs that pass Migration Audit, $58\%$ reach $99\%$ of the fixed checks, yet only $26\%$ reach $100\%$. Agent capability differs across migration categories: agents score $31.4$ on build toolchain rewrites but only $5.6$ on language rewrites. Together, these findings position SWE Refactor Bench as a rigorous testbed for developing coding agents for reliable whole-repository migrations.

Q1: 这篇论文试图解决什么问题？

论文聚焦的核心问题是：编码智能体能否自主完成长期的、整库级别的技术栈迁移？这一问题的难点在于现有基准的评估设计存在根本缺陷。大多数现有编码基准（如 SWE-bench 等）只检查行为正确性，即测试是否通过，而不验证代码是否真正发生了预期的结构性变化。这导致一个严重的漏洞：智能体可以复制原始实现来通过测试，从而在不做任何迁移的情况下获得满分，作者将这种评估失效称为“盲目性”（Blindness）。此外，整库迁移任务具有长时程、大规模、跨模块等特点，远比单文件 bug 修复复杂，现有基准无法衡量这类能力。论文试图通过构造成熟的、真实世界风格的迁移任务，并设计能同时检测“是否迁移了”和“是否正确”的评估协议，来填补这一空白。

Q2: 有哪些相关研究？

论文的相关工作可归为几类。一是编码智能体评估基准，如 SWE-bench 系列，它们主要衡量 bug 修复能力，通常只依赖单元测试或集成测试的行为正确性，未考虑代码结构或技术栈迁移的完成度。二是软件维护相关评估，如 SWE-CI 关注持续集成场景下的代码库维护能力（见参考文献 [10]）。三是通用编码智能体平台，如 OpenHands（参考文献 [64]）等，作为智能体开发与测试的基础设施。四是迁移相关的工具和研究，如针对特定语言或框架迁移的自动化工具，但大都局限于辅助人工。本论文的创新点在于：首次将“迁移完整性”纳入可执行的评估指标，采用三阶段协议将行为正确性和迁移完成度分离，并用智能体互验的方式捕捉固定测试集无法覆盖的隐藏行为差异。

Q3: 论文如何解决这个问题？

论文提出 SWE Refactor Bench 及配套评估协议，核心思路是同时强制要求“迁移发生”和“行为正确”。具体而言：
1. 任务构建：从真实开源基础设施中选取 20 个长期整库迁移任务，覆盖 4 类技术债（language rewrites、framework rewrites、platform ports、build toolchain rewrites）。每个任务要求仓库在整个范围内采用目标技术栈，并保证行为正确性。
2. 三阶段评估协议：
 - 阶段一：迁移审计（Migration Audit）——验证迁移是否实际发生，例如检查是否仍存在旧技术栈的残留、是否缺少关键迁移步骤。这能有效拦截“复制原实现”的作弊行为。
 - 阶段二：行为测试（Behavioural Tests）——使用固定测试套件（130,118 个固定检查）验证行为正确性，要求全部通过。
 - 阶段三：智能体验证（Agentic Verification）——用 6 个独立编码智能体在提交后主动搜索隐藏行为差异，并且只有在找到可执行反例时才能拒绝迁移。这一步骤用来捕获固定测试集未覆盖的回归。
3. 实验规模：每个任务给智能体 6 到 30 小时的自主工作时间，评估 8 个前沿模型在 26 种模型-努力配置下的表现，共 520 次运行。

Q4: 论文做了哪些实验？

论文进行了大规模系统性评估实验。具体包括：
- 数据集：20 个整库迁移任务，来源为真实开源基础设施，覆盖语言重写（language rewrites）、框架重写（framework rewrites）、平台移植（platform ports）、构建工具链重写（build toolchain rewrites）四类。
- 模型与配置：8 个前沿模型，每种模型搭配多种努力水平（effort），共 26 种模型-努力配置，总运行次数 520 次。
- 评估协议完整性：每个任务包含 130,118 个固定行为检查，全部通过才视为行为正确；此外还有 6 个独立智能体用于生成额外的针对性测试。
- 评估指标：每个运行按照三阶段规则评分，最终得到综合得分（例如最佳模型 claude-opus-5 得 47.0/100）。
- 分析维度：分阶段通过率、各任务上的接受率、不同迁移类别的能力差异、迁移完整性与行为正确性的相关性等。

Q5: 发现了什么实验现象？

论文的实验揭示了一系列关键现象：
1. 整体成功率极低：在 520 次运行中仅 28 次（5.4%）同时通过三阶段，13/20 个任务不存在被接受的解决方案，最佳模型也不超过 47.0/100。
2. 迁移完整性和行为正确性是两种独立能力：部分运行选择跳过迁移来保持行为正确，但被迁移审计拦截；多数运行尝试迁移却导致行为回归，被行为测试拦截。说明“重写代码”和“保持正确”之间存在突出的张力。
3. 即使迁移审计通过，行为也未必全对：通过迁移审计的 340 次运行中，58% 能通过固定测试的 99%，但只有 26% 能通过 100%。这表明智能体在收尾阶段容易遗漏边缘情况。
4. 在通过固定测试的运行中，仍有相当比例（约三分之二）被智能体验证发现反例——即固定测试套件覆盖不足，智能体验证能有效补充隐藏回归的检测。
5. 不同迁移类别的难度差异巨大：构建工具链重写平均得分 31.4，平台移植 17.2，框架重写 12.0，语言重写仅 5.6。语言重写（如从一种编程语言迁移到另一种）是最难的任务，现有模型几乎无法胜任。

Q6: 有什么可以进一步探索的点？

基于本文的结果和局限，可探索的后续方向包括：
1. 改进智能体对长时程整库任务的处理能力，例如更好的任务规划、进度自检与回溯机制。
2. 研究如何在保证迁移完整性的同时避免行为回归，特别是语言重写这类高难度场景，可能需要跨语言语义分析或更细粒度的中间表示。
3. 扩展基准规模与多样性，纳入更多技术债类型（如依赖升级、架构重构），以及不同规模的仓库。
4. 改进评估协议本身，如自动化迁移审计的完备性，减少误判或漏判；引入更强大的验证智能体。
5. 探索多智能体协作或人类-智能体混合工作流，以分担复杂的迁移决策。
6. 将三阶段协议迁移到其他软件工程任务（如大规模重构、安全加固）或科学计算代码迁移等应用领域。

Q7: 总结一下论文的主要内容

论文针对编码智能体在长期、整库技术栈迁移任务上的能力评估问题，提出了 SWE Refactor Bench 基准。主要动机是现有基准只检查行为正确性，导致智能体可以“复制原实现”通过测试而逃避迁移，作者称之为 Blindness（盲目性）。为了消除这一缺陷，论文设计了三阶段评估协议：迁移审计、行为测试、智能体验证。迁移审计检查迁移是否真实发生；行为测试用 130,118 个固定检查验证行为正确性；智能体验证则由 6 个独立编码 agent 搜索隐藏行为差异并以可执行反例为主要拒绝依据。

数据集方面，论文从真实开源基础设施中精选 20 个整库迁移任务，覆盖语言重写、框架重写、平台移植、构建工具链重写 4 类技术债，每项任务要求智能体在 6 到 30 小时内完成迁移并保证行为正确。实验对 8 个前沿模型、26 种模型-努力配置进行了 520 次运行，结果显示只有 28 次（5.4%）能通过全部三阶段，13/20 个任务没有任何被接受的解决方案，最佳模型 claude-opus-5 得分为 47.0/100。

进一步分析表明，迁移完整性与行为正确性是分离的能力：部分运行通过跳过迁移维持行为，被迁移审计拦截；大多运行尝试迁移却破坏行为，被行为测试拦截。在通过迁移审计的 340 次运行中，58% 能达到固定检查的 99%，但只有 26% 达到 100%。即使通过固定测试，仍有约三分之二的运行被智能体验证发现反例，说明固定套件存在盲区且智能体验证不可或缺。智能体在不同迁移类别上能力差异显著：构建工具链重写得分 31.4，语言重写仅 5.6。这些发现表明现有编码智能体还不能实现可靠的整库栈迁移，同时本文基准为后续开发与测评提供了严格的测试平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与“agent”研究方向直接相关：论文系统性地评估了编码智能体的长期任务执行能力，并使用了多智能体验证的评估思路。

## 基本信息

- 作者：Deyao Hong, Yizhe Chi, Wenyi Li, Xiaoqiu Wang, Mingju Gao, Kaisen Yang, Bingxiang He, Youjie Zheng, Calvin Xiao, Qinhuai Na
- 机构：Navers Lab, Einsia.AI; 清华大学（依据首页脚注推断）
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.SE
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23564v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、方法、结果和结论片段，并结合启发式草稿进行补全；部分细节（如任务列表、具体迁移案例）因证据不足未展开。
