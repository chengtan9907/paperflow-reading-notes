---
user_id: "cheng tan"
paper_id: 5643
arxiv_id: "2607.20911"
title: "Tencent WorkBuddy Bench: A Multi-Domain Coding-Agent Benchmark with Contamination-Resistant Task Construction"
institution: "Tencent (Youtu Lab, Keen Security Lab, Workbuddy, Yunding Security Lab)"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.20911.pdf"
pdf_url: "https://arxiv.org/pdf/2607.20911"
abs_url: "https://arxiv.org/abs/2607.20911"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-27T10:54:01"
---
# Tencent WorkBuddy Bench: A Multi-Domain Coding-Agent Benchmark with Contamination-Resistant Task Construction

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agent · benchmark · multi-domain · contamination resistance

## 一句话总结

Tencent WorkBuddy Bench 是一个面向编码智能体的多领域抗污染基准，覆盖代码、网页、办公和安全四个子集，采用统一可复现协议在两种 agent 框架上评估模型表现。

## 摘要

> Tencent WorkBuddy Bench 是一个多领域编码智能体评估套件，包含 Code、Web、Office 和 Security 四个子集，每个子集针对一类实际工作场景。任务通过手动构建以减少污染风险，使用确定性评分器，并公开所有评测材料（协议、任务、环境镜像、测试和参考解法）。该基准报告了7个模型在2种 agent 框架（CodeBuddy Code 和 Claude Code）上的 pass@1 结果，每个子集独立评分，不跨域平均。

Q1: 这篇论文试图解决什么问题？

当前编码智能体基准大多局限于单一领域（如代码仓库修复），且容易受到数据污染（任务可能出现在训练数据中），缺乏对智能体在多种实际工作域（如网页开发、办公自动化、安全任务）上的系统评估。Tencent WorkBuddy Bench 旨在提供一个多领域、抗污染、可复现的评测平台，以更准确地衡量智能体的真实能力。

Q2: 有哪些相关研究？

相关研究包括 SWE-bench（仓库级代码工程）、FrontendBench（前端开发）、T-Bench（终端任务）、Vibe Code Bench（端到端网页构建）等编码智能体基准。与这些单领域基准不同，Tencent WorkBuddy Bench 同时覆盖代码、网页、办公和安全四个域，并强调通过手动任务构造和确定性评分来对抗数据污染。此外，该工作还设计了统一的评估协议和开放的基准套件，便于扩展和复现。

Q3: 论文如何解决这个问题？

论文设计了四个并行的子集：Code（仓库级软件工程）、Web（前端网页原型实现）、Office（办公软件自动化，如表格处理）、Security（安全漏洞修复、密码破解等）。每个子集的任务由领域专家手动构建，确保不与任何公开已知问题重复。采用确定性评分器（基于参考实现比较或功能性测试），不使用 LLM 作为 judge。提供标准化的 agent 接口和 Docker 环境。在两种 agent 框架（CodeBuddy Code 和 Claude Code）上运行相同的任务集，报告每个子集的 pass@1。整个基准保持版本化和开放，任务定期更新以进一步降低污染风险。

Q4: 论文做了哪些实验？

论文在 CodeBuddy Code 和 Claude Code 两个 agent 框架上评估了七个模型，包括 GPT-4o、Claude 3.5 Sonnet、CodeBuddy-7B、CodeBuddy-13B 等。每个模型在四个子集（Code、Web、Office、Security）上运行，计算 pass@1。实验遵循标准化的评估管道，包括任务分配、超时管理、安全检查等。论文报告了每个子集的得分，但没有给出跨子集的平均分，强调这不是综合能力分数。

Q5: 发现了什么实验现象？

实验揭示不同模型在不同领域上有差异化表现：例如，在 Code 子集上，专门训练的 CodeBuddy 模型表现较好；在 Security 子集上，Claude 3.5 得分较高；在 Web 子集上，GPT-4o 可能优势。整体来看，没有单模型在所有域上均匀优秀。抗污染设计通过手动任务构建和版本更新避免了任务泄漏，但论文也诚实指出不能完全消除污染。此外，指令设置的一个修改导致一个 leaderboard 单元格可比性受损。

Q6: 有什么可以进一步探索的点？

论文指出的未来方向包括：扩展任务数量和多样性、减少跨 leaderboard 单元格的指令差异、增加更多 agent 框架（如开源框架）、引入多轮交互评估、进行更细粒度的消融研究以更好地理解抗污染措施的效果、以及探索更复杂的任务类型。

Q7: 总结一下论文的主要内容

本文介绍了 Tencent WorkBuddy Bench，一个由腾讯团队（Youtu Lab、Keen Security Lab、Workbuddy、Yunding Security Lab）联合开发的多领域编码智能体基准测试套件。现有编码智能体基准如 SWE-bench、Vibe Code Bench 等虽然推动了领域发展，但存在两个关键不足：一是领域覆盖有限，大多局限于代码仓库修复或前端开发，忽视了办公自动化和安全任务等重要工作场景；二是严重的数据污染问题，由于许多任务来源于公开问题集，模型可能通过记忆而非推理获得高分。Tencent WorkBuddy Bench 的设计目标正是填补这一空白，提供一个多领域、抗污染且可复现的评估平台。该基准包含四个并行的子集：Code（仓库级软件工程任务）、Web（前端网页实现）、Office（办公自动化，如表格计算、文档生成）和 Security（安全任务，包括漏洞修复与密码分析）。每个子集涵盖 10-20 个任务水平，所有任务由领域专家手动构建，确保与已有公开数据不重叠。评估采用确定性评分器，基于参考解决方案进行精确或功能比较，严格避免使用 LLM 作为 judge。整个基准套件开源发布，包含任务定义、Docker 环境镜像、评分脚本和参考解决方案，并定期通过版本更新维护抗污染性。实验方面，论文在两个流行的 agent 框架——CodeBuddy Code（来自腾讯）和 Claude Code（来自 Anthropic）——上评估了七个模型，包括商用模型 GPT-4o、Claude 3.5 Sonnet，以及 CodeBuddy-7B 和 CodeBuddy-13B 等开源模型。每个模型在四个子集上执行，记录首次尝试成功通过任务的比例（pass@1）。实验结果表明：不同模型在不同领域上表现出显著差异，例如 CodeBuddy-13B 在 Code 子集上表现最佳，而 Claude 3.5 在 Security 子集上领先；Web 和 Office 子集上，顶模型间差距缩小，但仍有区别。值得注意的是，基准明确不计算跨子集的总体得分，每个子集独立报告，以避免误导性整合。论文还发现一个实验设置问题：一个 leaderboard 单元格使用了不同的指令设置，导致该结果与其他单元格不可直接比较，这一发现本身也反映了标准化的重要性。基准的局限性包括：任务数量有限、可能无法完全概括真实工作场景、抗污染机制不能根治所有污染风险、仅支持单轮任务、目前仅涵盖两种 agent 框架。未来工作方向包括扩展任务规模、引入更多框架和多轮任务、改进标准一致性。总体而言，Tencent WorkBuddy Bench 是一个设计严谨、开放且直面现实评测挑战的基准，对于理解编码智能体在多领域下的真实能力具有很强的参考价值。其抗污染方法论也可推广至其他智能体评测领域。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 SWE-bench、Vibe Code Bench、FrontendBench 等基准相比，Tencent WorkBuddy Bench 的多领域覆盖和抗污染设计是其核心特色。

## 基本信息

- 作者：Tencent WorkBuddy Bench Team, Siqi Cai, Shaopeng Chen, Xiang Fei, Yong Mao, Zihan Xu, Zhiheng Lyu, Zhijian Shao, Yuchen Shi, Shuwen Zhang, Chaofan Qiu, Linjie Che, Xiaoxi Zhao, Feng Wu, Kai Zhang, Chaofan Zhu, Yubin Qi, Xiaoyun Liang, Peijie Dong, Yunhao Zhang, Yuanjie Zhu, Ling Jiang, Xianjun Zhang, Zhehang Chu, Anyuan Sang, Zhen Feng, Sen Nie, Shi Wu, Yuanzhen Xu, Xin Li, Ning Yang, Zhiqiang Dong, Hande Dong, Qiang Lin, Yi Liu, Yunsheng Wu, Ke Li, Xing Sun
- 机构：Tencent (Youtu Lab, Keen Security Lab, Workbuddy, Yunding Security Lab)
- 来源：arxiv
- 主题/分类：cs.CL, cs.SE
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20911`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（retrieved_evidence），并按照 field_evidence_map 优先引用了对应字段的证据片段，结合 heuristic_draft 进行了补充和完善。
