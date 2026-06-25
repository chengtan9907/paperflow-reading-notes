---
user_id: "cheng tan"
paper_id: 1242
arxiv_id: "2606.23654"
title: "EnterpriseClawBench: Benchmarking Agents from Real Workplace Sessions"
institution: "Horizon Research, Frontis.AI"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23654.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23654"
abs_url: "https://arxiv.org/abs/2606.23654"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:21:37"
---
# EnterpriseClawBench: Benchmarking Agents from Real Workplace Sessions

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：enterprise agent · benchmark · real-world sessions · artifact delivery

## 一句话总结

EnterpriseClawBench是一个从专有真实企业工作会话构建的代理基准测试，包含852个可复现任务及多维评估协议。

## 摘要

> Enterprise agents increasingly operate inside workspaces: they read heterogeneous files, invoke tools, and deliver business artifacts. We introduce EnterpriseClawBench, an enterprise agent benchmark constructed from proprietary, real-world agent sessions. Starting from a large archive of workplace sessions, the EnterpriseClawBench produces 852 reproducible tasks, each paired with recovered fixtures, rewritten prompts, role classes, skill subclasses, hard rules, and semantic rubrics. Because the sessions contain internal enterprise content, we do not release the benchmark data; instead, our reusable contribution is the construction and evaluation protocol. On EnterpriseClawBench, the best configuration reaches only 0.663 (Codex with GPT-5.5). These results show that enterprise agent evaluation must report harness--model combinations, artifact delivery, visual quality, cost, runtime, and skill-transfer behavior, rather than collapsing performance into a single score. Code: https://github.com/FrontisAI/EnterpriseClawBench

Q1: 这篇论文试图解决什么问题？

目前企业代理的评估大多基于合成环境或简单任务，缺乏从真实工作会话构建的、包含复杂工件交付和多维度指标的基准。论文旨在通过从企业内部代理会话记录自动构建可复现任务集，并提出包含harness-model组合、工件交付质量、视觉质量、成本、运行时和技能迁移在内的多维度评估协议，填补这一空白。

Q2: 有哪些相关研究？

相关基准如TheAgentCompany（2024）评估LLM代理在实际任务上的表现，EntWorld（2026）关注企业GUI代理的验证环境。此外，还有SWE-bench等软件工程基准。但现有工作很少使用真实企业工作会话中的原始代理日志，也缺乏对工件交付视觉质量、技能迁移等维度的评估。

Q3: 论文如何解决这个问题？

论文设计了一个自动流水线：从企业内部代理会话存档中提取任务，恢复工作空间fixtures（文件、工具痕迹等），重写提示以去除敏感信息并保证可复现，为每个任务标注角色类（如行政、工程）和技能子类（如文件操作、工具调用），定义硬规则（如输出格式）和语义评价标准（如LLM判断）。然后使用多个harness（如Claude Code、Codex、DeepAgents）与不同模型组合进行评测，记录工件交付质量（通过视觉LLM判断）、成本、运行时和技能迁移表现。

Q4: 论文做了哪些实验？

论文在手动审核的120任务Lite子集上进行了实验，评测了32种harness-model组合（含Claude Code、Codex、DeepAgents等），使用Sonnet 4.6作为文本和视觉评判。报告了主排行榜（Figure 1），包含综合得分、工件交付质量、成本、运行时等。此外还进行了技能迁移分析、LLM评判与人工评判的一致性分析等。

Q5: 发现了什么实验现象？

最佳配置Codex with GPT-5.5仅达到0.663，说明企业代理任务难度大。视觉评判显示部分模型生成的工件视觉质量差。某些模型（如Claude Code）在处理长轨迹时出现截断，导致工件未写出。技能迁移分析表明，不同模型在跨技能类上的表现不均匀。

Q6: 有什么可以进一步探索的点？

未来可扩展至多企业部署以增强泛化性；改进LLM评判的可靠性，特别是视觉评判；研究技能迁移的机制；探索更高效的代理架构以降低成本；发布部分去标识化数据以促进社区研究。

Q7: 总结一下论文的主要内容

论文提出了EnterpriseClawBench，一个从真实企业代理会话自动构建的基准测试。包含852个可复现任务，每个任务有fixtures、提示、角色类、技能子类、硬规则和语义评价。由于数据敏感性，论文不发布数据，而是发布构建和评估协议。实验在120任务子集上评测了32种harness-model组合，最佳得分0.663。论文强调企业代理评估应报告多维指标，而非单一分数。论文还讨论了技能迁移、成本、运行时等因素。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文与智能体方向直接相关（用户画像权重0.10）

## 基本信息

- 作者：Jincheng Zhong, Weizhi Wang, Che Jiang, Kai Tian, Zhenzhao Yuan, Junlin Yang, Dianqiao Lei, Kaiyan Zhang
- 机构：Horizon Research, Frontis.AI
- 来源：arxiv
- 主题/分类：cs.CL, cs.SE
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23654`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段
