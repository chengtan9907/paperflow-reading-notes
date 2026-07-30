---
user_id: "cheng tan"
paper_id: 5918
arxiv_id: "2607.25560v1"
title: "Agent Skills Matter: Inferring Proprietary Skills from Execution Trajectories"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25560v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25560v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:58:37"
---
# Agent Skills Matter: Inferring Proprietary Skills from Execution Trajectories

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent skills · skill leakage · execution trajectories · black-box inference

## 一句话总结

本文定义技能泄露（Skill Leakage）问题，并提出SigLeak框架，利用对比轨迹分析从良性查询的执行轨迹中重构隐藏的专有技能，无需参考答案或成功标签。

## 摘要

> Agent skills package reusable procedures that improve downstream performance. Their lightweight, portable form enables marketplace monetization and private deployment behind cloud-hosted agent interfaces, giving providers incentives to keep high-value skills proprietary. Yet hiding the artifacts does not conceal their behavioral effects, which remain observable in execution trajectories and form a behavioral side channel. We define this exposure as Skill Leakage: reconstructing proprietary skills from trajectories elicited by benign queries, without reference answers or success labels. We introduce SigLeak, a black-box framework that exploits recurring skill signatures in agent behavior. It constructs diverse, decision-rich diagnostic tasks, contrasts matched skill-enabled and skill-disabled trajectories, and iteratively refines a reconstructed skill from the isolated patterns. Across five scenarios, three model families, and three agent frameworks, SigLeak outperforms or matches three baselines in nearly every setting. It raises the success rate by 6.88 percentage points over the skill-disabled reference on average and achieves the highest overall SkillSim, our metric for coarse- and fine-grained semantic similarity. These results show that benign execution trajectories can expose proprietary procedural knowledge. The code is available at https://anonymous.4open.science/r/SigLeak-D1DB.

Q1: 这篇论文试图解决什么问题？

当前Agent技能市场允许提供商将高价值技能作为闭源产品部署，技能工件（如提示、代码）被隐藏，但技能的行为效果在执行轨迹中仍然可观测。本文系统性地研究了这一暴露问题，称为技能泄露（Skill Leakage）：攻击者仅通过向目标Agent发送良性查询并观察其执行轨迹，即可重构其专有技能。关键在于，从轨迹中推断技能不需要参考答案或成功标签，这使得攻击门槛较低（合理推断）。作者认为，不同的技能具有独特的功能印记（functional profiles）和轨迹级行为签名（trajectory-level skill signatures），这些可作为侧信道用于技能推断。该问题涉及Agent安全、知识产权保护和市场设计等交叉领域。

Q2: 有哪些相关研究？

相关研究主要分为两类：1）技能提取（Skill Extraction）：假设可访问私有API来获取演示（如Zhou et al., 2026b），但本文设置中技能被隐藏，无法直接获取；2）技能演化（Skill Evolution）：利用显式成功/失败反馈优化可用技能（如Yang et al., 2026a; Ni et al., 2026），但本文没有成功标签。此外，从执行轨迹中逆向工程能力也与逆向增强学习（IRL）和示范学习有关，但本文关注的是显式可执行技能指令的重构，而非底层策略。本文是首个系统研究黑盒设置下Agent技能泄露问题的（论文声称）。

Q3: 论文如何解决这个问题？

本文提出SigLeak，一个两阶段黑盒框架。第一阶段（Generator）：自动构造多样化的诊断任务（probes），这些任务设计用于激发目标技能的行为模式。Generator通过上下文学习生成覆盖技能多个方面的probe，确保轨迹中包含决策丰富的信息。第二阶段（Skill Synthesizer）：对每个probe，收集启用技能和禁用技能（通过匹配的初始状态和指令）下的成对轨迹。Synthesizer对比这两类轨迹，识别技能特有的行为差异，然后从差异中提炼出可执行的技能指令。这一过程可以迭代多轮，每轮使用前一轮的重构技能作为先验，逐步精炼。最终输出一个自然语言技能描述，未来可集成到相同或不同的Agent框架中。该框架不依赖任何参考答案或成功标签，完全通过对比分析进行。

Q4: 论文做了哪些实验？

实验设置涵盖五个任务场景（具体场景未公开，合理推断包括表格操作、网页导航等）、三种模型家族（合理推断包括GPT-4、Claude等）和三种Agent框架（合理推断包括LangChain、CrewAI等）。基线包括三个方法（具体未公开，合理推断为行为克隆等）。评估指标为下游任务成功率（SR）和技能语义相似度SkillSim（粗粒度和细粒度F1）。主要发现：SigLeak在几乎所有设置中优于或匹配基线，平均SR比技能禁用参考高6.88个百分点，SkillSim最高。消融实验验证了对比配对和迭代精炼的有效性（来自片段）。

Q5: 发现了什么实验现象？

实验揭示以下现象：（1）对比轨迹配对有效，差异分析能捕捉技能特有模式（合理推断）。（2）迭代精炼在第二轮达到SR和细粒度F1峰值，超过两轮后稳定（来自消融evidence）。（3）在不同模型家族和Agent框架下SigLeak表现稳健（来自abstract）。（4）SkillSim指标表明重构技能在执行和语义上都接近真实（来自abstract）。（5）固定probe预算可能在复杂场景下不足（来自limitations）。总体证实轨迹侧信道可泄露技能。

Q6: 有什么可以进一步探索的点？

基于现有局限，未来可探索的方向包括：（1）动态适配probe预算和合成策略，根据目标技能复杂度自动调整。（2）使用可变或优化的Generator和Synthesizer提示，而非固定模板。（3）扩展到更复杂的技能，如条件分支技能、多步骤技能或组合技能。（4）研究防御机制，如引入轨迹混淆、屏蔽技能签名，防止技能泄露。（5）探讨技能泄露对Agent市场和隐私的实际影响，以及相应的检测与防护策略。（6）将技能泄露问题形式化，建立理论保证。（7）跨Agent迁移重建技能的能力，如将从一个Agent重构的技能应用于另一个。

Q7: 总结一下论文的主要内容

本文针对Agent技能商业化中出现的专有技能泄露风险，首次系统定义了技能泄露问题——通过观察执行轨迹逆向推断被隐藏的技能。作者提出了SigLeak，一个基于对比轨迹分析的黑盒框架。SigLeak包括Generator（构造诊断性任务probe）和Skill Synthesizer（对比有无技能启用下的轨迹，迭代提炼技能）。实验在五个场景、三种模型家族和三种Agent框架上进行，对比三个基线。结果表明，SigLeak在几乎所有设置下优于或匹配基线，平均成功率提升6.88个百分点，SkillSim指标最高。消融实验验证了各组件有效性。本文揭示了行为侧信道在Agent技能安全中的重要性，并提供了第一组实证结果。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文研究Agent技能的安全问题，与用户的智能体（agent）方向高度相关。

## 基本信息

- 作者：Jianing Geng, Ruiqi He, Zekun Fei, Biao Yi, Ruijie Wang, Zheli Liu, Xia Hu, Xuansheng Wu, Qingkai Zeng
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25560v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，优先使用了Abstract和Introduction的语义匹配片段，并根据field_evidence_map分配了证据锚点。
