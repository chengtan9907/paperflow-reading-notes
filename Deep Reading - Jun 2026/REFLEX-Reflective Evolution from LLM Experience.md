---
user_id: "cheng tan"
paper_id: 506
arxiv_id: "2606.16496v1"
title: "REFLEX: Reflective Evolution from LLM Experience"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16496v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16496v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:09:25"
---
# REFLEX: Reflective Evolution from LLM Experience

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：evolutionary search · programmatic policy · multimodal llm · visual diagnosis

## 一句话总结

提出REFLEX，将视觉诊断与代码生成解耦，利用Critic-Actor架构和Skill Memory实现可审计、样本高效的进化策略搜索。

## 摘要

> Large multimodal language models (LLMs) have emerged as powerful tools for guiding evolutionary search toward interpretable programmatic policies. However, existing frameworks rely on a monolithic model call to simultaneously interpret visual behavioral evidence and synthesize corrective code. This diagnosis-repair entanglement creates an opaque feedback loop, obscuring the rationale behind mutations and preventing the retention of algorithmic insights across independent runs. To achieve auditable and efficient policy search, we argue that visual diagnosis must be structurally decoupled from code generation. We present REFLEX, a train-free evolutionary framework that operationalizes this decoupling. In REFLEX, a vision-enabled Critic first distills task-specific behavioral evidence into structured, auditable diagnoses. Subsequently, a text-optimized Actor synthesizes child policies using these diagnoses alongside a persistent, self-evolving Skill Memory of reusable code snippets. This architecture not only provides transparent mutation traces but also enables cross-run programmatic knowledge transfer. Extensive evaluations across control benchmarks (Lunar Lander, Acrobot, Pendulum) and a 36-dimensional antenna array synthesis task demonstrate exceptional sample efficiency. Notably, REFLEX solves Acrobot and Pendulum in under 10 LLM calls and reaches a best Normalized Weighted Score of 1.092 on Lunar Lander, achieving highly competitive final performance while significantly accelerating the early-stage discovery of transparent policies.

Q1: 这篇论文试图解决什么问题？

现有LLM引导进化搜索方法（如EoH、MLES）使用单一模型同时处理视觉行为证据和生成修正代码，导致诊断-修复过程耦合。这种耦合产生不透明的反馈循环：无法追溯每次变异的具体理由（为何修改、修改依据什么证据），且每次独立运行中获得的算法洞察无法保留和跨运行传递。这限制了搜索的可审计性和效率，特别是当需要解释策略演化过程或复用早期经验时。

Q2: 有哪些相关研究？

相关工作包括：1) 程序化策略进化：如EoH（Liu et al., 2024）使用LLM变异程序化策略，但诊断与修复耦合；MLES（Hu et al., 2025）也采用类似耦合方式。2) 基于LLM的代码修复：如RepairAgent（Bouzenia et al., 2025）利用LLM自动修复代码，但缺乏视觉诊断。3) 多模态LLM辅助进化：部分工作利用视觉语言模型进行上下文理解，但未解耦诊断与生成。4) 技能记忆：如Atlasva（2026）提出自进化视觉技能记忆，但侧重教师反馈。REFLEX通过显式解耦Critic（视觉诊断）和Actor（代码生成），并引入Skill Memory实现跨运行知识复用，区别于现有方法。

Q3: 论文如何解决这个问题？

REFLEX是一个免训练进化框架，核心包括三个模块：1) Critic：一个视觉可用的LLM，接收当前策略的视觉行为证据（如状态轨迹、奖励曲线），输出结构化的文本诊断，指出策略的优缺点和潜在改进方向。2) Actor：一个文本优化的LLM，基于Critic的诊断和Skill Memory中的可复用代码片段，合成子代策略代码。Actor不直接处理视觉输入，专注于代码生成。3) Skill Memory：一个动态更新的存储，包含从成功变异中提取的抽象代码片段（如“相位对齐能量泵送”“速度门控稳定”等控制哲学）。随着搜索进行，Skill Memory自进化，使新运行能继承先前经验。在每轮迭代中，Critic诊断当前策略，Actor结合诊断和Skill Memory生成新策略，评估后更新Skill Memory。所有变异理由通过Critic的诊断文本记录，实现可审计。

Q4: 论文做了哪些实验？

实验设置：在三个连续/离散控制基准（Lunar Lander、Acrobot、Pendulum）和一个36维天线阵列合成任务上评估。对比方法包括DQN、PPO、EoH、MLES等。使用相同LLM模型（如GPT-4？具体未明确）、评估预算和随机种子。指标包括归一化加权分数（NWS）、平均奖励、成功率等。天线任务使用副瓣电平（SLL）和方向性。实验包括：1) 主实验：REFLEX与基线对比，记录最终性能和收敛速度。2) 消融实验：去除Critic、去除Skill Memory等变体对比。3) 热身启动实验：用Skill Memory初始化新运行（warm-start） vs 冷启动。4) 跨任务转移实验：在Acrobot上学到技能迁移至Pendulum。

Q5: 发现了什么实验现象？

1) 样本效率：REFLEX在Acrobot和Pendulum上10次LLM调用内求解（达到可接受奖励），而MLES需要更多调用。2) 最终性能：Lunar Lander上REFLEX最佳NWS为1.092，接近MLES最佳（1.098），但早期收敛更快；天线任务中REFLEX-best设计副瓣低于Dolph-Chebyshev和Taylor三阶。3) 消融发现：去除Critic后，变异质量下降（更低NWS），且失去审计轨迹；去除Skill Memory后，跨任务转移效果消失。4) 热身启动：预填充Skill Memory的warm-start在早期收敛速度显著快于冷启动（如在Acrobot上，第2次评估时收益Δ=+1.327）。5) 诊断突变优势：Critic提供了高质量编辑和可审计理由，相比之下直接代码突变缺乏理性。6) 超参数敏感性：对LLM温度、种群大小等不敏感（已测试），鲁棒性好。

Q6: 有什么可以进一步探索的点？

1) 扩展至更复杂的机器人控制任务（如六足行走、灵巧操控）和组合任务（需要多技能协作）。2) 研究Skill Memory的自动抽象机制：当前可能依赖人类设计或LLM内隐知识，未来可探索更自主的提取方式。3) 跨领域迁移：测试在游戏、电路设计等非连续控制领域的适用性。4) 结合强化学习反馈：使Critic能利用环境奖励以外的信号（如安全性、鲁棒性）。5) 审计追踪的形式验证：将诊断文本与形式规范对齐，保证解释可靠性。6) 多模态输入扩展：处理更复杂的传感器数据（如激光雷达、触觉）。

Q7: 总结一下论文的主要内容

REFLEX论文针对LLM引导进化搜索中诊断与修复耦合的问题，提出了一个解耦架构。核心贡献在于：1) 将视觉行为诊断（Critic）与代码生成（Actor）分离，使得每次变异的理由透明可审计；2) 引入Skill Memory，存储从搜索中提炼出的可复用控制哲学，支持跨运行知识迁移，显著提升样本效率。方法上，REFLEX是一个免训练进化框架（无需额外训练LLM），Critic接收当前策略的视觉行为证据（如状态轨迹图、奖励曲线），输出结构化的诊断文本（如“奖励过早停滞，需要更激进的探索”）；Actor基于诊断和Skill Memory中的代码片段生成子代策略。Skill Memory随着进化动态更新，自动积累高质量代码模式。实验在Lunar Lander、Acrobot、Pendulum控制基准和36维天线阵列合成任务上进行。结果显示：REFLEX在Acrobot和Pendulum上10次LLM调用内即找到可行策略，在Lunar Lander上达到1.092的归一化加权分数（接近MLES的1.098但收敛更快）；在天线任务中，REFLEX-best设计副瓣电平低于经典Dolph-Chebyshev和Taylor锥度。消融实验验证了Critic和Skill Memory各自的关键作用：去除Critic导致性能下降和审计缺失；去除Skill Memory则失去跨任务转移能力。热身启动实验表明，用先前运行积累的Skill Memory初始化新运行可大幅加速早期搜索（第2次评估收益Δ=+1.327）。此外，REFLEX还提供了完整的变异轨迹文本，增强了可解释性。总体而言，REFLEX以简单有效的解耦思路，在保持或提升最终性能的同时，大幅提高了进化搜索的透明度和样本效率，为可审计AI策略搜索提供了新范式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与生成方向直接相关（权重0.10）：涉及程序化策略的自动生成和进化

## 基本信息

- 作者：Pan Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16496v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段
