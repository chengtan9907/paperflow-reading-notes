---
user_id: "cheng tan"
paper_id: 16101
arxiv_id: "2610.18688v2"
title: "RSIgen: Reciprocal Self-Improvement for Agentic Visual Generation"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18688v2.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18688v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:07:14"
---
# RSIgen: Reciprocal Self-Improvement for Agentic Visual Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic visual generation · reciprocal self-improvement · dynamic responsibility routing · unified multimodal backbone

## 一句话总结

RSIgen 提出一种以任务为中心的互惠自我改进框架，通过动态责任路由（DRR）在学习分配、监督刷新与递归课程构建之间建立闭环，解决统一多模态骨干中 agent 与 generator 双角色学习目标不明确及策略过时问题，在多个视觉生成基准上显著提升任务完成与视觉质量。

## 摘要

> Agentic visual generation produces experience that can improve the native understanding (agent) and generation roles of a unified multimodal backbone, but task success alone does not determine which role should learn. Generator updates can also make the agent's action supervision stale. We introduce RSIgen, a task-centric reciprocal self-improvement framework linking learning allocation, supervision refresh, and recursive curriculum construction. Dynamic Responsibility Routing (DRR) uses matched interventions on evidence, inputs, and supported actions to select learning recipients and construct compatible training targets. After generator learning, RSIgen re-evaluates the relative utility of tested actions with branch inputs held fixed to refresh agent supervision; the retained roles then construct the next curriculum. Across BAGEL, Qwen-Image, and Wan-Image-7B, RSIgen improves AgentGen-Bench Full Overall9 by 3.2--3.3 points over a three-cycle SearchGen adaptation with action costs. Wan controls support allocation gains at matched role-level training exposure, refresh gains at a matched adaptation budget, and benefits from updated curriculum sources while recipients continue learning. In blind evaluation on 200 real-user requests, pooled tie-adjusted preference scores against the same baseline reach 70.0% for task completion and 64.9% for visual quality.

Q1: 这篇论文试图解决什么问题？

1. 学习接收者不确定性问题：任务成功是 agent 和 generator 共同作用的结果，但最终图像无法区分哪个角色的贡献，因此无法直接决定应该更新哪个角色。
2. 训练目标不兼容问题：即使确定了接收者，也需要构建与该角色能力相匹配的训练目标。例如，修正指令可以训练 agent，但同样的修正对 generator 可能无意义。
3. 策略过时问题：生成器学习后，其能力发生变化，agent 之前学到的动作偏好（如搜索）可能不再必要，导致监督信号过时。共享架构不会自动校准这种能力-策略错位。
4. 现有方法的不足：DAgger 等处理由策略变化引起的分布偏移，但不考虑由生成器学习引起的动作效用变化；其他 agentic generation 方法（如 GEMS、Mind-Brush）扩展了能力，但没有解决学习分配和互惠校准问题。
5. 需要一种机制来动态决定谁学、学什么、何时刷新监督，并递归构建课程。

Q2: 有哪些相关研究？

1. Agentic visual generation 的评估和重新提示：论文引用了 Jin et al. (2025)、Tang et al. (2026) 等工作，涉及评估和重新提示。
2. GEMS 和 Mind-Brush：通过记忆、技能、搜索和推理扩展 agentic generation（He et al., 2026b,a）。
3. DAgger：处理由策略变化引起的分布偏移（Ross et al., 2011），但 RSIgen 额外考虑由生成器学习引起的动作效用变化。
4. 其他可能相关：统一多模态骨干、视觉生成、自我改进等，但具体文献需回原文确认。

Q3: 论文如何解决这个问题？

1. 整体框架：RSIgen 是一个任务中心的互惠自我改进框架，连接学习分配、监督刷新和递归课程构建。
2. DRR（Dynamic Responsibility Routing）：使用匹配干预（matched interventions）在证据、输入和支持动作上选择学习接收者，并构建兼容的训练目标。具体来说，通过干预某个角色（如修正指令）观察效果，如果修正指令在证据和生成器不变的情况下修复图像，则说明问题在 agent 侧；如果更换证据或输入能改善，则可能涉及 generator。DRR 根据干预结果决定哪个角色应该学习，并构造相应的训练目标。
3. 监督刷新：在生成器学习后，RSIgen 保持分支输入固定，重新评估测试动作的相对效用，以刷新 agent 的监督信号。这解决了生成器更新导致旧监督过时的问题。
4. 递归课程构建：保留的角色（即被选中学习的角色）构建下一个课程，形成互惠循环。课程来源可以是新的任务或数据，且更新的课程来源在接收者继续学习时有益。
5. 与现有方法的区别：DAgger 只处理策略变化引起的分布偏移，RSIgen 还处理动作效用变化；且 RSIgen 是任务中心的，以任务成功为导向。

Q4: 论文做了哪些实验？

1. 骨干模型：BAGEL、Qwen-Image、Wan-Image-7B。
2. 基准：AgentGen-Bench Full Overall9。
3. 基线：三周期 SearchGen 适应（带动作成本）。
4. 实验臂：每个臂使用 1,800 个 agent 记录和 2,200 个 generator 记录，每次尝试总计 5,400 和 6,600（可能指三个周期）。单独角色评估使用 200 个请求和每个请求/状态四个图像种子；agent 评估固定 G0（初始生成器），generator 评估固定 agent 策略。
5. 控制实验：Wan 控制支持分配增益（匹配角色级训练暴露）和刷新增益（匹配适应预算）；源控制连接递归改进与目标有效性和监督新鲜度。
6. 盲评：200 个真实用户请求，W/T/L 记录，偏好分数计算为 100(W+0.5T)/(W+T+L)。
7. 分支示例：测量不同动作之间的转移和增益，如 rate→LocalEdit、ReferenceRegenerate→LocalEdit 等。

Q5: 发现了什么实验现象？

1. 主要结果：RSIgen 在三个骨干上比基线提高 3.2-3.3 点（AgentGen-Bench Full Overall9）。
2. 盲评：任务完成偏好 70.0%，视觉质量 64.9%（合并平局调整）。BAGEL 上任务完成 68.5%，视觉质量 63.5%（W/T/L 分别为 312/198/90 和 264/234/102）。
3. 控制实验：Wan 控制显示，在匹配角色级训练暴露下，分配增益仍然存在；在匹配适应预算下，刷新增益存在；更新的课程来源在接收者继续学习时有益。
4. 分支示例：不同动作之间的转移显示差异化的动作增益，例如某些转移（如 rate→LocalEdit）有较高的计数和百分比，而其他转移较低。这支持了动作效用会随能力变化而变化的观点。
5. 策略过时现象：图注显示“Policy can become stale as capabilities evolve”，即随着能力演化，策略会过时，需要刷新。
6. 负结果或张力：论文没有明确提到负结果，但控制实验表明，如果不匹配暴露或预算，增益可能消失，说明分配和刷新的重要性。

Q6: 有什么可以进一步探索的点？

1. 扩展到更多骨干和任务，验证框架的通用性。
2. 更细粒度的责任路由，例如在子模块级别进行干预。
3. 动态课程设计：如何自动选择课程来源和难度。
4. 与其他自我改进方法（如自我对弈、反思）结合。
5. 理论分析：为什么匹配干预能正确识别学习接收者，以及监督刷新的收敛性。
6. 真实用户反馈的更多维度，如安全性、偏见等。
7. 在生物科学等应用场景中测试（与用户画像相关）。

Q7: 总结一下论文的主要内容

本文针对 agentic visual generation 中统一多模态骨干的双角色（agent 和 generator）学习问题，提出 RSIgen 框架。作者首先指出任务成功不能决定学习接收者，因为最终图像是两者共同作用的结果；同时，生成器更新会使 agent 的监督过时。为此，他们设计了三个互惠机制：DRR 通过匹配干预选择学习接收者并构建训练目标；监督刷新在生成器学习后重新评估动作效用；递归课程构建由保留角色生成下一轮课程。实验在三个骨干上进行，显示一致的性能提升，并通过控制实验分离了分配、刷新和课程来源的贡献。盲评进一步验证了真实用户偏好。论文的贡献在于提出了一个系统性的互惠自我改进框架，并展示了其在多个模型上的有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent（权重 0.10）和 generation（权重 0.10）方向直接相关。

## 基本信息

- 作者：Yidi Liu, Haozhe Wang, Jie Xiao, Kai Zhu, Xueyang Fu, Zheng-Jun Zha
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18688v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（主要来自 Introduction 和 References），并结合摘要与启发式草稿进行补全；部分细节因证据不足需回原文确认。
