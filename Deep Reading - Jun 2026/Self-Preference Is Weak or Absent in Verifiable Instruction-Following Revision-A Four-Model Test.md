---
user_id: "cheng tan"
paper_id: 776
arxiv_id: "2606.20093v1"
title: "Self-Preference Is Weak or Absent in Verifiable Instruction-Following Revision: A Four-Model Test Under Genuine Authorship"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.20093v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.20093v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:34:25"
---
# Self-Preference Is Weak or Absent in Verifiable Instruction-Following Revision: A Four-Model Test Under Genuine Authorship

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-preference · instruction-following · IFEval · llm-as-judge

## 一句话总结

在可验证的指令遵循修订任务中，LLM作为原作者与作为新鲜模型相比，拒绝有效修正的比率无显著差异，表明自我偏好偏倚很弱或不存在。

## 摘要

> Large language models (LLMs) increasingly review and revise text, including their own. A documented self-preference bias (models favoring their own generations when acting as judges) raises the question of whether models also resist valid corrections to their own writing. We test this in a setting where "valid" is decided not by another model but by a deterministic verifier: instruction-following revision on IFEval. A model writes a draft; the official IFEval checker confirms the draft violates a constraint and that a candidate edit fixes it; the model then accepts or rejects that edit either as the genuine in-context author or as a fresh model that sees the draft neutrally. Across four mid-tier model families and 85 author-versus-fresh comparisons, we find no detectable self-preference: authors reject verified-good fixes to their own drafts at essentially the same rate as fresh models judging the same drafts (gap -5.1 pp, 95% CI [-12.9, +2.7]). A self-skepticism hint from a smaller pilot did not replicate at scale. The one robust observation is qualitative: when authors do reject a verified-good fix, 97% of their stated reasons are flaw-catching rather than preference, that is, about the character of rejections, not an elevated rate. Effects smaller than ~13 pp cannot be excluded at this sample size.

Q1: 这篇论文试图解决什么问题？

已有研究表明LLM作为判断者时偏爱自己生成的文本（自我偏好）。本文旨在探究在可验证的指令遵循修订任务中，这种自我偏好是否会导致模型拒绝有效的修正——即模型是否因为自己是原作者而更抗拒接受正确的修改，而如果以中立身份看到同一修改则更可能接受。具体问题：在确定性验证器（IFEval）保证修正有效的前提下，原作者 vs 新鲜模型的拒绝率是否存在系统性差异？

Q2: 有哪些相关研究？

1. LLM-as-judge 中的自我偏好：Panickssery等人（2024）发现模型在判断时偏爱自己的输出；Choi等人（2026）表明此偏见可通过匿名化减弱。2. 奉承（sycophancy）现象：模型倾向于同意用户的观点，与自我偏好有时相互交织。3. 模型在可验证任务中的修正行为：Huang等人（2024）指出LLM在推理任务中无法自我修正，可能受到生成文本的执着影响。本文区别于上述工作：聚焦于可验证的指令遵循修改任务，使用确定性检查器避免主观判断，并直接比较“作者”与“新鲜”视角。

Q3: 论文如何解决这个问题？

设计三个逐步递进的实验（Pilot 1-3）：P1（告诉标签代理）使用已告知作者身份的代理进行测试；P2（干净作者测试）让同一模型分别作为作者和新鲜模型进行对比；P3（四模型测试）扩展至四个不同模型家族（共85次对比）。核心流程：1. 模型生成草稿；2. IFEval检查器确认草稿违反约束；3. 生成一个修复编辑，经检查器确认有效；4. 展示草稿+修复给模型，分别在“作者”（原模型，具有上下文提示其为作者）和“新鲜”（模型无上下文，仅看到草稿和修复）条件下要求接受或拒绝；5. 比较两种条件下的拒绝率差异（自我偏好差距），使用95%置信区间估计。

Q4: 论文做了哪些实验？

1. Pilot 1（告诉标签代理）：明确告知模型草稿由自己生成，测试是否有更强拒绝倾向。2. Pilot 2（干净作者测试）：使用两个独立会话模拟作者和新鲜视角，避免标签效应。3. Pilot 3（四模型测试）：在四个中档模型家族（每个家族一个代表模型）上重复P2设计，总85个草稿-修复对。每个条件均使用确定性IFEval检查器验证修复必然修复约束违反。主要指标：作者拒绝率 - 新鲜拒绝率（自我偏好差距）。预注册分析计划（若差距≥5pp则支持自我偏好）。此外，在P2中尝试加入“自我怀疑提示”以观察是否减少拒绝，但该操作在P3中因无效应而放弃。

Q5: 发现了什么实验现象？

1. 无显著自我偏好：三个Pilot的自我偏好差距均≤0（P3差距-5.1pp，95% CI [-12.9, +2.7]），不拒绝零假设。2. 定性发现：原作者拒绝有效修正时，97%的理由指向草稿本身存在其他缺陷（如格式问题、语法错误），而非对修改的原始作者的固执。3. 自我怀疑提示：在P2中该提示使作者拒绝率略有下降，但在P3中未产生显著影响，且因效应不稳定而放弃。4. 效应量上限：当前样本量无法排除小于13pp的效应，因此不能完全否认存在微小自我偏好。5. 模型间差异：未发现某个模型家族表现出显著自我偏好趋势，但置信区间较宽。

Q6: 有什么可以进一步探索的点？

1. 增大样本量以检测更小的效应（<13pp）。2. 测试更强模型（如GPT-4级别）或不同架构（MoE等）。3. 在不可验证任务（如风格修改、观点改写）中重复，因为确定性检查器保证了客观正确，自我偏好可能在主观判断中更明显。4. 探索更精细的作者身份提示（如明确显示历史生成记录）对拒绝率的影响。5. 研究模型拒绝时的解释机制：是否内在监控到文本的其他问题，还是单纯偏好。6. 结合奉承与自我偏好的交互：当用户（实验者）明确表示希望模型接受修改时，结果是否不同。

Q7: 总结一下论文的主要内容

本文系统测试了LLM在可验证指令遵循修订任务中的自我偏好偏差。具体问题：当模型作为原作者时，是否比作为新鲜判断者更可能拒绝一个已验证有效的修正？作者设计了三个渐进的实验：P1使用揭示作者身份的代理；P2采用严格的作者-新鲜对比；P3在四个中档模型（共85个示例）上重复。主要发现：在确定性IFEval检查器确保修正有效的条件下，原作者拒绝有效修正的比率与新鲜模型无显著差异（差距-5.1pp，95% CI包含0）。定性分析表明，当原作者拒绝时，几乎全部因为草稿本身存在其他不符合约束的问题（即“捕捉缺陷”），而非对修正本身的抗拒。唯一暗示自我怀疑的小规模效应未能在大规模实验中复现。作者据此认为，在可验证的修改任务中，自我偏好偏倚很弱甚至不存在，但更小的效应（<13pp）可能因统计功效不足而未被检测到。论文还讨论了与奉承、匿名化、模型大小等相关的文献，并强调了使用确定性验证器相比主观判断的优势。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文与你当前关注的“生成”方向相关（权重0.10），探讨了LLM在修改自身生成时的行为，对理解自回归模型的自我认知有启示。

## 基本信息

- 作者：William Guey, Pierrick Bougault
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-18
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.20093v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，所有字段优先使用证据片段，信息不足时基于heuristic_draft和常识补充。
