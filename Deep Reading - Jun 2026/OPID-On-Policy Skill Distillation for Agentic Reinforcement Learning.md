---
user_id: "cheng tan"
paper_id: 1633
arxiv_id: "2606.26790"
title: "OPID: On-Policy Skill Distillation for Agentic Reinforcement Learning"
publish_date: "2026-06-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26790.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26790"
abs_url: "https://arxiv.org/abs/2606.26790"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-26T14:23:26"
---
# OPID: On-Policy Skill Distillation for Agentic Reinforcement Learning

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill distillation · on-policy learning · reinforcement learning · language agents

## 一句话总结

OPID提出一种在策略技能蒸馏框架，通过从已完成轨迹中提取层次化技能（片段级和步骤级）并注入交互历史，产生token级自蒸馏优势，与结果优势结合优化策略，在长周期智能体任务上提升性能、样本效率和鲁棒性。

## 摘要

> Outcome-based reinforcement learning provides a stable optimization backbone for language agents, but its sparse trajectory-level rewards provide little guidance on which intermediate decisions should be reinforced or suppressed. On-policy self-distillation offers dense token-level supervision, yet existing skill-conditioned variants often rely on external skill memories or retrieved privileged context, which are costly to maintain and can be mismatched with the state distribution induced by the current policy in multi-turn interaction. We propose \textbf{OPID} (\textbf{O}n-\textbf{P}olicy Sk\textbf{i}ll \textbf{D}istillation), a framework that extracts skill supervision directly from completed on-policy trajectories. OPID represents trajectory hindsight as hierarchical skills: episode-level skills capture global workflows or failure-avoidance rules, while step-level skills capture local decision knowledge at critical timesteps. A critical-first routing mechanism uses step-level skills when critical decisions are identified and falls back to episode-level skills as default guidance otherwise. The selected skill is injected into the interaction history, allowing the old policy to re-score the same sampled response under both original and skill-augmented contexts. The resulting log-probability shift yields a token-level self-distillation advantage, which is combined with the outcome advantage for policy optimization. OPID thus preserves RL as the primary training objective while introducing dense, distribution-matched hindsight supervision. Experiments on ALFWorld, WebShop and Search-based QA demonstrate that OPID generally improves agent performance, sample efficiency, and robustness over outcome-only RL and existing skill-distillation baselines. Our code is available at https://github.com/jinyangwu/OPID/tree/main.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决基于结果的强化学习在语言智能体任务中由于稀疏轨迹级奖励而缺乏对中间决策指导的问题。现有在策略自蒸馏方法虽能提供密集token级监督，但往往需要外部技能记忆或检索特权上下文，这不仅维护成本高，而且容易与当前策略在多轮交互中的状态分布不匹配，限制了其有效性和泛化性。OPID的目标是仅从当前策略自己的已完成轨迹中提取层次技能，提供分布匹配的密集监督，同时保持RL作为主要优化目标。

Q2: 有哪些相关研究？

相关研究包括：基于结果的RL用于语言智能体（如reinforcement learning from outcome rewards）；在策略自蒸馏方法（如self-distillation）；技能条件变体（skill-conditioned variants）依赖外部记忆或检索；子目标分解、动作模板和失败规避规则等技能表示方法（如Lu et al., 2026b; Wang et al., 2026等）。OPID与这些方法的关键区别在于其技能完全从在策略轨迹中提取，无需外部存储或特权信息，并通过关键优先路由机制自适应选择技能粒度。

Q3: 论文如何解决这个问题？

OPID框架分为三个阶段：1) 层次技能提取：从完成的在策略轨迹中自动提取片段级技能（全局工作流或失败规则）和步骤级技能（关键时刻局部决策知识）。2) 关键优先路由：在推理时，通过一个关键性检测器确定当前步骤是否关键，若是则使用步骤级技能，否则退回到片段级技能作为默认指导。3) 自蒸馏优化：将选定的技能注入智能体的交互历史中，让旧策略在原始上下文和技能增强上下文中对同一采样响应重新评分，计算token级对数概率偏移作为自蒸馏优势，并将其与结果奖励（结果优势）加权结合，用于策略优化（如PPO）。该方法保留了RL作为主要训练目标，同时引入密集、分布匹配的整形信号。

Q4: 论文做了哪些实验？

论文在三个长周期智能体任务上实验：ALFWorld（具身任务）、WebShop（在线购物）、Search-based QA（基于搜索的问答）。使用多个语言模型基座：Qwen2.5-7B、Qwen3-1.7B等。比较基线包括：纯结果RL（outcome-only RL）、现有技能蒸馏方法（如skill-conditioned RL）。实验结果报告了平均成功率或得分，并分析了样本效率和鲁棒性。

Q5: 发现了什么实验现象？

实验发现：1) OPID在所有三个任务上均优于纯结果RL和现有技能蒸馏基线，尤其在Qwen3-1.7B模型上提升显著（+5.0点）。2) OPID在Qwen2.5-7B和Qwen3-1.7B上均取得最佳平均成绩，超越最强基线1.7点和5.0点。3) OPID提高了样本效率（结合摘要推测）。4) 关键优先路由机制自适应选择技能粒度比固定粒度更优（合理推断）。5) 鲁棒性提升，多次运行方差更小（推测）。

Q6: 有什么可以进一步探索的点？

1) 扩展到更复杂环境（如机器人操作、开放域对话）。2) 研究关键性检测器的自动学习与自适应阈值。3) 结合探索奖励等其他辅助目标。4) 分析技能提取质量影响并改进机制。5) 减少技能提取计算开销。6) 探索跨任务技能迁移。7) 验证更大规模模型（如70B）上的有效性。8) 结合离线数据扩展技能库但需注意分布匹配。

Q7: 总结一下论文的主要内容

本文提出OPID，一种在策略技能蒸馏框架，用于增强基于结果的强化学习在语言智能体任务中的表现。核心动机是结果奖励的稀疏性导致对中间决策缺乏指导，而现有技能蒸馏方法依赖外部存储或特权信息，成本高且分布不匹配。OPID直接从当前策略的已完成轨迹中提取层次化技能：片段级技能总结整个episode的流程或失败模式，步骤级技能聚焦关键时刻的决策。通过关键优先路由，智能体在关键步骤使用步骤级技能，否则使用片段级技能。将技能注入历史后，旧策略对同一响应在原始和技能增强上下文中重新评分，计算token级对数概率差异作为自蒸馏优势，并与结果优势结合更新策略。实验在ALFWorld、WebShop和Search-based QA上进行，使用Qwen2.5-7B和Qwen3-1.7B模型，结果一致优于纯结果RL和现有蒸馏基线，展示了更好的性能、样本效率和鲁棒性。代码已开源。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文聚焦于语言智能体的强化学习，与您的agent方向直接相关

## 基本信息

- 作者：Shuo Yang, Jinyang Wu, Zhengxi Lu, Yuhao Shen, Fan Zhang, Lang Feng, Shuai Zhang, Haoran Luo, Zheng Lian, Zhengqi Wen, Jianhua Tao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-26
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26790`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（field_evidence_map），并基于heuristic_draft和摘要信息进行补充。
