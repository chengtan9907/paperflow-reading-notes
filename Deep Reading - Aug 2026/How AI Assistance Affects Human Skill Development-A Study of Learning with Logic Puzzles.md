---
user_id: "cheng tan"
paper_id: 9553
arxiv_id: "2608.23543v1"
title: "How AI Assistance Affects Human Skill Development: A Study of Learning with Logic Puzzles"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23543v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23543v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:22:58"
---
# How AI Assistance Affects Human Skill Development: A Study of Learning with Logic Puzzles

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：ai assistance · skill development · logic puzzles · human learning

## 一句话总结

通过受控逻辑谜题实验，研究发现低成本AI辅助会促使参与者更频繁地使用AI，但使用AI的辅助的参与者在移除辅助后任务表现更差，且独立推理努力与技能发展正相关。

## 摘要

> While AI assistance can improve human task performance in the short term, it may also undermine the development of skills in the longer term. We examine this tension in a controlled logic-puzzle experiment involving on-demand AI assistance, where participants complete tasks before, during, and after AI is available. By experimentally varying AI request costs, we find that lower-cost assistance induces more frequent AI use. We also find that participants who request AI assistance during the AI-access phase perform worse at the task after assistance is removed, and their subsequent unassisted performance is overestimated when predicted from earlier AI-assisted performance. We use a Bayesian latent ability model to separate initial ability, post-AI ability, and participant-specific skill change, while estimating how independent reasoning during the AI-access phase relates to skill development. The results show that greater independent problem-solving effort is associated with larger gains in latent ability, consistent with the interpretation that skill development is weaker when AI assistance substitutes for independent reasoning.

Q1: 这篇论文试图解决什么问题？

论文尝试解决的核心问题是：AI辅助虽然能在短期内提升人类任务表现，但长期来看是否可能削弱技能发展。具体而言，作者关注按需AI辅助（on-demand AI assistance）情境下，AI使用的成本如何影响使用频率，以及这种使用如何影响移除AI后的独立任务表现和能力习得。研究还希望厘清AI辅助与独立推理之间的替代或互补关系——即AI是否仅仅提供了替代认知努力的出路，从而阻碍了技能的内化。此外，从方法论上，论文试图解决一个测量问题：如何从纵向任务表现中分离出初始能力、训练后的能力以及个体技能变化，并识别独立推理对技能增长的贡献。

Q2: 有哪些相关研究？

相关工作涉及AI辅助学习、技能习得、认知卸载（cognitive offloading）以及AI辅助的替代与互补作用。已有研究指出，技能发展依赖主动参与，需要反复推理、测试策略、修正错误并内化问题结构。AI辅助的效果取决于其是否规定动作或仅引导注意力，这强化了AI替代人类推理与补充人类推理之间的区分。作者还提及自己的前期工作[35]探索了AI辅助的角色。此外，该领域研究通常关注短期性能提升，而较少考察长期技能影响；本文通过受控实验和贝叶斯建模，试图弥补这一空白。

Q3: 论文如何解决这个问题？

论文采用受控实验与贝叶斯统计建模相结合的方法。实验设计为三阶段逻辑谜题任务：阶段一（无AI）建立基线；阶段二（有AI）允许参与者按需请求AI辅助，并实验性地设置不同请求成本（低成本 vs 高成本）；阶段三（无AI）再次在无辅助条件下测试。通过比较不同成本组的AI请求频率和阶段三的表现，评估AI使用对技能发展的因果影响。为处理个体能力差异和技能变化，作者引入了贝叶斯潜能力模型（Bayesian latent ability model），将观测表现分解为初始能力、后AI能力和个体专属的技能变化量，同时将AI辅助阶段中未请求辅助的独立推理次数作为与技能发展相关的协变量。该模型可以估计独立推理与潜能力增长之间的关联，并校正因辅助表现带来的预测偏差。

Q4: 论文做了哪些实验？

论文进行了一个受控在线用户实验，以逻辑谜题（如数独类或不依赖外部知识的谜题）为任务，分为三个任务阶段：第一阶段收集无辅助的表现，第二阶段提供可请求的AI辅助，并随机分配参与者的AI请求成本（低成本或高成本），第三阶段移除AI辅助再次测试。关键测量指标包括：AI请求频率、每阶段的任务正确率和/或完成时间、参与者在AI辅助阶段的独立推理次数（即未请求AI时的解题次数）。作者还将实验数据拟合到贝叶斯潜能力模型中，比较不同组别的技能变化，并检验从AI辅助阶段表现预测未辅助阶段表现的校准性。具体实验参数（如成本数值、题目数量、样本量）在摘要和检索片段中未给出，需查阅正文。

Q5: 发现了什么实验现象？

实验现象包括：1) 低成本的AI请求导致参与者更频繁地使用AI；2) 在AI辅助阶段请求AI的参与者在AI移除后的阶段三表现更差；3) 使用早期AI辅助阶段的表现预测后续未辅助表现时存在系统性高估（overestimation）；4) 在AI辅助阶段，独立解决问题的努力（即未求助AI的尝试）与潜能力的增长呈正相关。这些观察与主动学习理论一致，表明AI辅助替代独立推理会削弱技能内化。作者没有报告显著的意外反例，但高估现象说明辅助表现可能掩盖真实的技能水平。

Q6: 有什么可以进一步探索的点？

进一步探索的方向包括：1) 将实验扩展到更复杂的任务和更长的训练周期，以检验技能发展的长期轨迹；2) 变化AI辅助的内容（例如直接给出答案 vs 提供提示或引导），以更细致地刻画替代与互补的边界；3) 研究个体差异（如初始能力、元认知策略）如何调节AI辅助的影响；4) 设计自适应成本机制，在不牺牲短期表现的前提下鼓励独立推理；5) 将贝叶斯潜能力模型应用于其他教育或人机交互领域，用于更准确评估学习成果；6) 探索AI辅助在团队或协作场景中的技能影响；7) 结合神经影像或过程性日志，进一步理解技能内化的机制。

Q7: 总结一下论文的主要内容

论文针对AI辅助的长期影响提出问题：虽然AI辅助能提高短期任务表现，但它可能替代独立思考，从而阻碍技能发展。作者设计了一个三阶段逻辑谜题实验，让参与者分别在无AI、有AI可请求（成本随机分为高低）、以及再移除AI的情况下完成任务。通过变动AI请求成本，实验表明更低成本会诱导更多AI请求。关键发现是，请求AI辅助的参与者在移除辅助后表现更差，并且从辅助阶段的表现预测未辅助表现时会高估其真实能力。为了更细致地刻画这一过程，作者构建了贝叶斯潜能力模型，把观测表现分解为初始能力、后AI能力和个体技能变化，并纳入独立推理次数的变量。模型估计结果显示，独立解题努力与潜能力增长正相关，支持了“AI替代推理会削弱技能发展”的理论。论文的贡献在于：提供了因果证据表明AI使用成本影响求助行为，用纵向实验设计分离短期表现与长期技能，并通过潜模型校正了绩效预测偏差。该研究对AI辅助工具的设计（如避免过度替代）、教育策略以及人工智能对人类技能影响的评估都有启示意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体（agent）研究具有参考性：如何设计辅助机制以避免过度依赖并促进自主技能习得是智能体与人类协作的核心问题。

## 基本信息

- 作者：Shang Wu, Catarina G Belem, Shuyuan Fu, Mark Steyvers, Padhraic Smyth
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23543v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段，并结合摘要和元数据进行了补全和推断。
