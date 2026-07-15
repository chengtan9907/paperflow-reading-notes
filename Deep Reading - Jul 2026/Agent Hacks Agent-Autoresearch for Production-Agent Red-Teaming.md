---
user_id: "cheng tan"
paper_id: 3638
arxiv_id: "2607.11698v1"
title: "Agent Hacks Agent: Autoresearch for Production-Agent Red-Teaming"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11698v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11698v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:40:49"
---
# Agent Hacks Agent: Autoresearch for Production-Agent Red-Teaming

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：production agent red-teaming · autoresearch · vulnerability concept graph · llm agent safety

## 一句话总结

本文提出AHA，一个自动化红队框架，通过可证伪的漏洞概念发现循环和Vulnerability Concept Graph (VCG)，系统性地发现并记录生产级LLM Agent中可迁移的安全漏洞知识。

## 摘要

> Production LLM agents such as Claude Code and Codex act through deployed interfaces over untrusted content, files, commands, and workspace state, so a safety failure here is a real action: a written file, exfiltrated data, or a triggered workflow. Red-teaming these agents must keep pace with every model and tool update, yet today's tools optimize judged attack success and preserve surface artifacts: benchmark scores, payloads, archives, strategies, or attack programs. These artifacts record where an attack landed, but not the enabling condition that made the agent trajectory unsafe, so they are hard to audit, patch against, or reuse after the setting changes. We study autoresearch for production-agent red-teaming, using one agentic research environment to automatically discover reusable vulnerability knowledge about another production-style agent. We present AHA, a falsifiable discovery loop: it commits to a vulnerability hypothesis, creates a falsifier, instantiates a scenario-valid attack, executes it in a sandboxed agent harness, reflects on the trajectory, and promotes confirmed findings by an evidence rule into a Vulnerability Concept Graph (VCG). Each concept is an auditable unit linking an attacker-facing surface to an unsafe trajectory through a claim, enabling condition, falsifier, transfer prediction, and evidence. Across Claude Code and Codex on three scenarios spanning direct and indirect attacks, the discovered concepts share a core that recurs across victim models and agents, the frozen VCG is reusable with no further search, outperforming the strongest frozen discovery baseline by 14.2 percentage points under the same single-shot protocol, and the concepts transfer across scenarios and across direct/indirect attack channels. This makes the artifact directly useful for production triage: a safety team can inspect the enabling condition, patch the agent or workflow, rerun the concept as a check on the fix, and attach new internal concerns through the same build/import scenario workflows. As production agents proliferate, such a VCG turns one-off red-teaming into cumulative, auditable safety knowledge that compounds across models and products. Our code is available at https://github.com/henrymao2004/Auto-research-red-teaming.

Q1: 这篇论文试图解决什么问题？

当前生产级LLM Agent的红队测试工具仅记录攻击是否成功，保存的工件如攻击载荷、策略等只反映攻击结果，缺失导致不安全轨迹的使能条件（enabling condition），因此难以审计漏洞根因、制定补丁，或在环境变化后复用。随着Agent快速迭代，需要一种自动化方法不仅能发现漏洞，还能输出可审计、可迁移、可修补的漏洞知识，使安全团队能直接检查使能条件并验证修复。

Q2: 有哪些相关研究？

相关研究涵盖：1）Agent级安全基准，评估整体攻防性能；2）构造性红队基线，通过手工或模板生成攻击；3）聊天级提示优化器，如token-level attack discovery（Panfilov et al., 2026），将红队视为优化问题但输出仍为攻击程序而非漏洞知识；4）先前的autoresearch工作，利用Agent自动进行研究但未聚焦于漏洞知识的形式化。本文在此基础上，将生产Agent红队形式化为autoresearch问题，目标是输出可验证、可迁移的漏洞知识。

Q3: 论文如何解决这个问题？

AHA采用可证伪发现循环：1）漏洞假设（Vulnerability Hypothesis）：研究Agent基于初始观察提出一个关于特定攻击面可能被利用的假设；2）创建伪造器（Falsifier）：自动生成一个能检验该假设的攻击程序（falsifier）；3）实例化场景攻击（Scenario-Valid Attack）：将伪造器适配到具体场景（如直接提示注入、间接工具滥用）；4）沙盒执行（Sandboxed Execution）：在隔离的Agent harness中运行攻击，记录完整轨迹；5）反思（Reflection）：分析轨迹判断假设是否被证伪（即攻击是否成功），并识别使能条件；6）证据提升（Promotion）：若满足证据规则（如攻击成功且可复现），则将发现提升为Vulnerability Concept，存入VCG。每个概念包含claim（声明）、enabling condition（使能条件）、falsifier（伪造器）、transfer prediction（迁移预测）和evidence（证据）。VCG支持增量构建和跨场景复用。

Q4: 论文做了哪些实验？

实验设置：受害者Agent包括Claude Code和Codex，受害者模型包括Minimax-M2.7、Kimi-K2.6、Deepseek-V4-Pro。覆盖三个场景：直接攻击（如恶意指令）、间接攻击（如污染文件）。每个场景包含多个实例。基线包括：1）单次直接攻击（single-shot direct attack）；2）单次重排攻击（single-shot jig attack）；3）基于遗传算法优化的攻击；4）冻结发现基线（frozen discovery baseline）。主要评估指标为攻击成功率（ASR）和迁移成功率（transfer ASR）。关键结果：AHA发现的VCG在冻结状态下（无进一步搜索）比最强冻结发现基线平均提高14.2%的ASR。概念在Claude Code与Codex间共享核心，且在不同场景和攻击通道间迁移。

Q5: 发现了什么实验现象？

1）不同受害者模型（Minimax-M2.7、Kimi-K2.6、Deepseek-V4-Pro）下发现的漏洞概念共享大量共同模式，表明存在模型无关的脆弱性；2）冻结的VCG无需重新搜索即可直接用于新实例，在单次攻击协议下持续优于其他冻结基线，最高提升14.2个百分点；3）概念在直接攻击与间接攻击场景间有效迁移，一个场景下发现的使能条件可预测另一个场景的攻击面；4）VCG的可审计单元使安全团队能直接检查使能条件并验证补丁。

Q6: 有什么可以进一步探索的点？

1）扩展VCG自动更新机制，使其能随着Agent和模型更新持续进化；2）将方法应用到更多类型的生产Agent，如聊天Agent、多Agent协作系统；3）探索更复杂的攻击面，如多步推理链、组合漏洞；4）研究对抗VCG的防御策略，即如何利用VCG强化Agent安全；5）将VCG与安全编排工具集成，实现自动化分流（triage）；6）在更开放的沙盒设定下测试方法的鲁棒性，减少对特定Agent harness的依赖。

Q7: 总结一下论文的主要内容

本文针对生产级LLM Agent的安全漏洞可迁移性问题，提出AHA（Agent Hacks Agent）框架。当前红队测试仅保留攻击表面工件，缺乏对漏洞使能条件的记录，导致难以审计和修补。AHA将黑盒红队形式化为autoresearch问题，以输出可审计、可迁移的漏洞知识为目标。核心是Vulnerability Concept Graph (VCG)，其中每个概念包含攻击面、使能条件、伪造器、迁移预测和证据。AHA采用可证伪循环：研究Agent提出假设，自动生成伪造器，在沙盒中执行攻击，然后反思轨迹并提升成功发现为VCG中的概念。实验在Claude Code和Codex上，针对三种模型和三个场景（含直接/间接攻击）进行。结果显示，发现的漏洞概念在不同模型间共享核心，冻结的VCG在单次攻击协议下比最强冻结发现基线高14.2%的ASR，且概念可跨场景和攻击通道迁移。该工作使得安全团队能直接检查使能条件、修补Agent并复用VCG验证修复，将红队产出从一次性攻击程序转向可积累的漏洞知识库。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）安全性方向直接相关，关注生产级Agent的红队测试。

## 基本信息

- 作者：Xutao Mao, Xiang Zheng, Cong Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.AI
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11698v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，包括Abstract、Method和Results部分，并基于heuristic_draft进行了补充和结构优化。
