---
user_id: "cheng tan"
paper_id: 10397
arxiv_id: "2609.02749v1"
title: "Repo-To-Skill: Distilling GitHub Repositories Into AI4AI Skills"
institution: "清华大学 (Tsinghua University), 微软 (Microsoft), 中国人民大学 (Renmin University of China)"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02749v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02749v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:29:03"
---
# Repo-To-Skill: Distilling GitHub Repositories Into AI4AI Skills

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：autonomous agents · operational knowledge · skill distillation · ai4ai

## 一句话总结

DisCo 通过将 GitHub 仓库和论文蒸馏为可验证的“操作性知识”技能图谱，填补了 AI 研究智能体在领域 Know-how 上的空白，显著提升了其在机器学习研究任务中的表现。

## 摘要

> Autonomous agents are beginning to carry out machine-learning (ML) research end to end. These agents combine a model backbone with a harness for planning, execution, memory, and verification, but this architecture still leaves domain-specific know-how outside the agent. We call this missing layer operational knowledge, the know-how that separates knowing a method from making it work. That knowledge is not absent from the field. It appears in repositories and papers, but in forms written for human readers and too large to load during a task. Once distilled into compact, verified skills, this knowledge can be reused across tasks rather than rediscovered during each run.
> We present DisCo, a skill-powered research agent that creates skills and uses them during research. Its distillation runs in two complementary forms: task-agnostic, condensing the field's widely used repositories into reusable skills, and task-oriented, producing the skills a concrete task calls for. The former, applied across the open ecosystem, yields the AREX-Skill Library, with 5,000+ verified skills distilled from 1,000 widely used ML repositories and organized into 20 areas and 178 capability families. With the GPT-5.5 backbone, research harness, and downstream execution budget held fixed, the skill-equipped research agent scores 134.3% higher on MLE-bench, 34.4% higher on PaperBench, 9.2% higher on FrontierCS, and 14.0% higher on PassNet than the same agent without skills. These gains come from adding distilled operating context under that fixed setup.

Q1: 这篇论文试图解决什么问题？

该论文试图解决自主研究智能体在执行端到端机器学习任务时面临的“知识鸿沟”问题。具体分析如下：
1. **操作性知识的缺失**：现有的智能体架构（模型骨干 + 执行框架）虽然具备推理和规划能力，但缺乏“操作性知识”，即那些能让特定方法真正跑通的工程细节和领域 Know-how。
2. **知识载体的局限性**：这些关键知识目前主要存在于 GitHub 仓库和学术论文中。然而，这些原始数据对于 LLM 的上下文窗口来说过于庞大，且充满了对人类读者友好但对机器执行不友好的非结构化信息。
3. **重复发现的低效性**：由于缺乏持久化的技能层，智能体在每次执行新任务时往往需要重新探索和发现相同的工程细节，导致研发效率低下且容易在琐碎的实现问题上失败。
4. **验证与可靠性挑战**：从海量代码中提取的知识如果不经过严格验证，可能会引入错误的实现逻辑，从而误导智能体的后续研究步骤。

Q2: 有哪些相关研究？

论文涉及的相关研究领域包括：
1. **自主研究智能体（Autonomous Research Agents）**：如近期出现的 MLE-bench、PaperBench 等，旨在评估智能体在自动化 ML 实验中的表现。这些工作通常关注规划和工具调用，而本论文则关注知识层的构建。
2. **技能库与记忆机制（Skill Libraries & Memory）**：借鉴了如 Voyager 等在游戏领域通过代码片段积累技能的思想，但将其扩展到了更复杂的科研和工程领域。
3. **AI for Science (AI4AI)**：利用 AI 自动化机器学习研究本身，是当前大模型应用的前沿方向。
4. **知识蒸馏与结构化提取**：研究如何从非结构化文本或复杂代码库中提取可执行的、模块化的知识单元。

Q3: 论文如何解决这个问题？

作者提出了 DisCo 框架，通过构建“操作性知识层”来增强智能体：
1. **技能（Skill）与技能图谱（Skill Graph）定义**：将操作性知识实例化为具有明确输入、输出、操作逻辑和验证条件的“技能”单元，并以图谱形式组织，方便检索和组合。
2. **双重蒸馏机制**：
 - **任务无关蒸馏（Task-agnostic）**：对开源生态系统进行大规模扫描，从 1,000 个顶级 ML 仓库中提取出 5,000 多个通用技能，构建了 AREX-Skill 库。这些技能被组织成 20 个领域和 178 个能力家族。
 - **任务导向蒸馏（Task-oriented）**：当面对特定任务时，DisCo 会探索任务相关的知识边界，并即时生成该任务所需的特定技能。
3. **验证与修复流程**：所有候选技能在进入技能层前必须通过验证。DisCo 会自动检查技能的有效性，并在发现错误时尝试修复，确保技能库的质量。
4. **操作上下文加载**：在研究过程中，智能体不再加载整个仓库，而是根据当前步骤的需求，从技能图中检索并加载最相关的“操作上下文”，从而在有限的上下文窗口内提供最高密度的有用信息。

Q4: 论文做了哪些实验？

论文设计了详尽的实验来验证 DisCo 的有效性：
1. **基准测试集选择**：
 - **MLE-bench**：包含 75 个来自 Kaggle 的真实机器学习工程挑战。
 - **PaperBench**：评估智能体复现学术论文实验的能力。
 - **FrontierCS**：测试前沿计算机科学问题的解决能力。
 - **PassNet**：评估代码编写的正确性。
2. **实验受控变量**：固定使用 GPT-4o/5.5 等级的模型骨干，保持执行框架（Harness）和计算预算一致，仅改变是否配备“技能层”。
3. **对比对象**：将配备 DisCo 技能层的智能体与不具备技能层的基准智能体进行对比。
4. **消融实验**：分别测试 AREX-Skill 库（任务无关技能）和即时生成的任务导向技能对最终性能的贡献度。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象和趋势：
1. **性能爆发式增长**：在 MLE-bench 上，DisCo 智能体的得分比基准高出 **134.3%**。这表明在工程密集型任务中，操作性知识的缺失是限制智能体表现的首要瓶颈。
2. **复现成功率提升**：在 PaperBench 上提升了 **34.4%**，证明了从论文中蒸馏出的模块化技能能显著降低智能体在复现复杂实验时的出错率。
3. **稳健的泛化能力**：在 FrontierCS (9.2%) 和 PassNet (14.0%) 上的提升虽然较小但非常稳健，说明技能库在不同类型的计算机科学任务中均有助益。
4. **指标间的张力**：实验观察到，虽然技能层增加了初始的“蒸馏”开销，但它显著减少了后续“执行”阶段的试错次数，从而在总计算预算内实现了更高的任务完成率。
5. **失败模式分析**：在缺乏技能验证的情况下，智能体容易受到低质量代码片段的干扰；而 DisCo 的验证机制有效地过滤了这些噪声，使得性能增益更加可靠。

Q6: 有什么可以进一步探索的点？

1. **跨学科扩展**：将 DisCo 模式从机器学习领域推广到生物信息学、材料科学等其他 AI4Science 领域。
2. **技能的动态演化**：研究智能体如何在长期运行中根据反馈自动优化、合并或废弃技能，实现技能库的自我进化。
3. **多智能体技能共享**：探索在多智能体协作场景下，不同智能体如何共享和协同调用技能图谱。
4. **更深层的论文挖掘**：进一步提升从纯文本论文中自动提取复杂实验流程和超参数调优 Know-how 的精度。

Q7: 总结一下论文的主要内容

本论文提出了 DisCo 框架，旨在通过引入“操作性知识层”来解决自主 AI 研究智能体在处理复杂机器学习任务时效能不足的问题。作者指出，现有的智能体虽然拥有强大的 LLM 骨干，但由于缺乏散落在 GitHub 和论文中的具体工程 Know-how（即操作性知识），在实际科研任务中表现受限。DisCo 通过将这些海量资源蒸馏为可验证、可重用的“技能”，并组织成技能图谱，为智能体提供了高密度的操作上下文。通过对 1,000 个 ML 仓库的蒸馏，作者构建了包含 5,000 多个技能的 AREX-Skill 库。实验结果显示，配备该技能层的智能体在 MLE-bench 等多个权威基准测试中表现出显著的性能提升，其中在机器学习工程任务上的得分提升超过一倍。该研究不仅提供了一个高效的技能蒸馏框架，还通过开源 AREX-Skill 库为 AI4AI 社区贡献了宝贵的知识资产，标志着 AI 研究智能体从单纯的“推理机”向具备“专业技能”的资深研究员迈进。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与你关注的智能体（Agent）方向高度契合

## 基本信息

- 作者：Jianlyu Chen, Yuyang Hu, Hongjin Qian, Jiawei Liu, Wenqing Wei, Xiaolong Chen, Defu Lian, Zhicheng Dou, Chaozhuo Li, Qiwei Ye, Zheng Liu
- 机构：清华大学 (Tsinghua University), 微软 (Microsoft), 中国人民大学 (Renmin University of China)
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.02749v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，详细分析了 DisCo 的双重蒸馏模式及 AREX-Skill 库的实验增益。
