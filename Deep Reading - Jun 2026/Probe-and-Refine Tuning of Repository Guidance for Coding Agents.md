---
user_id: "cheng tan"
paper_id: 864
arxiv_id: "2606.20512v1"
title: "Probe-and-Refine Tuning of Repository Guidance for Coding Agents"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20512v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20512v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:26:34"
---
# Probe-and-Refine Tuning of Repository Guidance for Coding Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agents · repository guidance · probe-and-refine tuning · SWE-bench

## 一句话总结

本文提出probe-and-refine tuning方法，通过合成bug-fix探针迭代优化仓库指导文件，在SWE-bench Verified上将编码代理的resolve率从25.5%提升至33.0%，并揭示改进源于覆盖率提升而非补丁精确度变化。

## 摘要

> LLM-based coding agents need higher-level operational knowledge about a repository (which files house which subsystems, how to run the test suite, which workflows have historically led to wrong fixes) that does not exist in the code itself. Engineers typically maintain \texttt{AGENTS.md} files to supply this context as instructions for coding agents, but whether they help is contested: recent studies disagree on whether LLM-generated guidance improves or harms agent performance. In this paper we show that how the guidance is produced is the decisive variable, and introduce \emph{probe-and-refine tuning}: a procedure that uses synthetic bug-fix probes to iteratively diagnose and patch a repository's guidance file through single-shot LLM calls, with no agent loop or tool use during tuning. On SWE-bench Verified across four independent trials with Qwen3.5-35B-A3B at 200 steps, probe-and-refine achieves 33.0\,\% mean resolve rate vs.\ 28.3\,\% for the static knowledge base used to initialize it and 25.5\,\% for an unguided baseline ($p < 0.001$ for both probe-and-refine contrasts). The improvement comes from coverage rather than precision: refined guidance produces evaluable patches for 14.5 percentage points (pp) more instances while per-patch precision remains statistically constant ($\sim$59\,\%, $p = 0.119$), showing that improved guidance helps agents reach the correct file rather than improving the quality of the changes they make. Further, a step-budget experiment shows that guidance is what lets the agent use a larger step budget productively, and a cross-model experiment with NVIDIA-Nemotron-3-Nano-30B-A3B finds that the tuning loop degrades when the model cannot generate sufficiently diagnostic output, though per-patch precision remains constant even then.

Q1: 这篇论文试图解决什么问题？

编码代理在仓库级任务中缺乏高层操作知识，如子系统文件位置、测试套件运行方式、历史上的错误修复流程。工程实践通过AGENTS.md文件提供指导，但近期研究对此观点不一，有的认为LLM生成的指导有帮助，有的认为有害。本文问题：指导是否有效？如果有效，什么因素决定其效果？本文假设指导的生成方式是关键变量，并提出系统化的生成策略。

Q2: 有哪些相关研究？

相关研究包括：(1) 基于LLM的编码代理工作，如SWE-agent、Devin等，利用工具和环境执行代码修复；(2) 仓库级指导文件（AGENTS.md）的效果研究，部分工作发现随机或静态指导反而降低性能；(3) 指导质量对代理行为的影响，指导优化方法如离线RL蒸馏。本文与这些工作的区别在于聚焦指导生成过程本身，而非模型或工具使用。

Q3: 论文如何解决这个问题？

本文提出probe-and-refine tuning方法。流程：(1) 初始化一个静态知识库（从仓库上下文提取的默认指导）；(2) 合成一组bug-fix探针（人工构造的故障实例）；(3) 对每个探针，让LLM使用当前指导文件和探针信息生成诊断和修复建议；(4) 利用LLM的judge能力评估当前指导的不足，并输出修改意见；(5) 迭代更新指导文件。整个过程无需代理循环或工具调用，仅依赖单次LLM调用。核心设计：探针是合成生成的，避免了真实bug的标注成本；迭代通过LLM自我批判完成。

Q4: 论文做了哪些实验？

实验设置：(1) 基准：SWE-bench Verified，包含真实仓库的bug修复任务；(2) 模型：主实验使用Qwen3.5-35B-A3B，步数预算200（agent steps）；(3) 对比条件：probe-and-refine tuning vs. 静态知识库初始化 vs. 无指导（仅系统提示）；(4) 变异实验：步数预算（25, 50, 100, 200步）、跨模型（NVIDIA-Nemotron-3-Nano-30B-A3B）；(5) 测量：resolve rate、patch覆盖率（能生成可评估补丁的比例）、patch精确度（通过测试的补丁比例）。每个条件运行4次独立试验。

Q5: 发现了什么实验现象？

关键观察：(1) probe-and-refine达到33.0% resolve率，显著高于静态知识库（28.3%）和无指导（25.5%），p<0.001；(2) 改进来源于覆盖率提升（+14.5 pp），而patch精确度基本恒定（~59%，p=0.119），表明指导帮助代理找到正确文件而非提高补丁质量；(3) 步数预算实验中，无指导性能在25步后饱和，而probe-and-refine继续提升，说明指导使代理能有效利用更多步数；(4) 跨模型实验中，Nemotron代理的调优循环退化（resolve率低于静态基线），但patch精确度仍恒定，推测原因是Nemotron的输出格式与调优提示不匹配；(5) 额外分析显示，指导类型影响效果：添加具体操作知识（如导航策略、质量门控）比删除模糊规则更有效。

Q6: 有什么可以进一步探索的点？

未来方向：(1) 适应不同模型，设计更通用的调优提示；(2) 扩展到更大规模的模型和更多仓库；(3) 研究指导效果的表示层面机制，解释为何覆盖率而非精确度提升；(4) 将probe-and-refine应用于其他代理任务（如代码审查、文档生成）；(5) 探索主动学习策略选择探针；(6) 结合人类反馈进一步优化指导；(7) 测试不同初始指导的影响。

Q7: 总结一下论文的主要内容

论文研究了编码代理的仓库指导生成问题。现有AGENTS.md文件效果存在争议，作者提出指导的生成方式是决定变量，并设计了probe-and-refine tuning方法。该方法利用合成bug探针和LLM的自我诊断能力，迭代优化指导文件，无需代理循环。在SWE-bench Verified上的四个独立试验中，该方法将Qwen代理的resolve率从25.5%提升至33.0%，主要归因于覆盖率提升（+14.5 pp）而非精确度变化。步数预算实验表明指导使代理能有效扩展搜索；跨模型实验显示方法对模型能力敏感。论文强调指导质量是第一阶变量，与模型选择同等重要。局限性包括仅测试两个模型、步数不足时可能有害、未探索表示层面机制。作者指出后续需研究指导如何影响代理的内部探索过程。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：关于智能体（agent）方向，权重0.10，本文提出指导优化方法，直接相关；

## 基本信息

- 作者：Asa Shepard, Jeannie Albrecht
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.LG
- 日期：2026-06-18
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20512v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence和field_evidence_map），基于语义检索片段整合而成。
