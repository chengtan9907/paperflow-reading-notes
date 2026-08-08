---
user_id: "cheng tan"
paper_id: 6201
arxiv_id: "2608.02499v1"
title: "SWE-Touch: Benchmarking Coding Agents When Users Touch the Code"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02499v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02499v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:51:15"
---
# SWE-Touch: Benchmarking Coding Agents When Users Touch the Code

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agents · benchmark · shared workspace · counter-edits

## 一句话总结

SWE-Touch 提出一个共享工作区基准框架，通过注入与任务冲突的“Counter-Edits”来压力测试编码智能体对用户代码修改的感知与应对能力，结果表明当前强自主性能并不能保证共享协作所需的状态感知。

## 摘要

> Real-world software development requires coding agents to operate in shared workspaces where users may inspect and modify code during an ongoing task, yet existing repository-level benchmarks typically evaluate agents working alone or restrict user participation to messages. This leads us to ask: how do coding agents understand and respond to code changes in a shared workspace? We introduce SWE-Touch, a framework that stress-tests this setting through validated Counter-Edits: plausible edits to task-relevant code that conflict with task completion. SWE-Touch mines task-critical regions from multiple repair trajectories, uses a separate User Patch Generator to construct the edits, and injects them with contextual user messages when agents reach the relevant code. We evaluate nine coding models on SWE-bench Verified, with additional experiments on longer-horizon tasks from SWE-Bench Pro and DeepSWE. Counter-Edit lowers average resolve rate by 7.7 percentage points on SWE-bench Verified, with degradation also persisting on both longer-horizon benchmarks. Trajectory analysis links these failures to limited awareness of the evolving workspace: agents may retain conflicting code or replace it without sufficiently re-inspecting the repository and validating the revised code with targeted tests. These findings show that strong autonomous performance does not yet ensure the state awareness and adaptive behavior needed for shared-workspace collaboration, and point to detecting workspace changes, reconciling conflicting edits with the task, and verifying the affected behavior as key capabilities for future optimization.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在真实软件开发中，编码智能体（coding agents）常常需要在一个共享工作区中工作，用户可能在任务进行中检查和修改代码。然而，现有的仓库级基准（如 SWE-bench、Terminal-Bench 等）通常只评估智能体独自完成任务，或者将用户参与限制为消息交互，并没有模拟用户在代码层面进行主动修改。这种评估设置与真实部署场景存在明显差距：当智能体正在修复 bug 时，用户可能已经改了同一段代码，导致智能体的后续行为基于过时或不一致的状态。因此，论文提出一个关键研究问题：编码智能体如何理解并响应共享工作区中用户对代码的更改？特别是当这些更改与任务目标冲突时，智能体能否检测到变化、正确协调冲突并最终完成任务？论文进一步将这个问题拆解为三个能力维度：检测工作区变化、将冲突编辑与任务目标调和、验证受影响行为。作者指出，自主模式下表现优异的模型，在用户触碰代码的交互场景中可能并不具备相应的状态感知和自适应能力，而现有基准无法揭示这种能力缺失。

Q2: 有哪些相关研究？

论文的相关工作可归纳为几个脉络。首先是编码智能体基准的演进：从最初的自包含程序合成（给定自然语言规范生成代码），逐步扩展到复杂指令、真实的被指令编辑（如 EDIT-Bench 等）、多样的函数和库使用等。这些基准大多在仓库级别、环境级别评估智能体（如 SWE-bench、Terminal-Bench），但用户参与通常被限制为文本消息或完全缺席。其次是人机协作与交互式智能体评估：一些工作关注用户通过对话指导智能体，但很少让用户在代码层面直接修改仓库。此外，还有关于智能体状态感知、工作区变化检测的研究，但往往没有形成系统化的压力测试基准。SWE-Touch 的贡献在于填补了这一空白：它不仅让用户以代码编辑的形式参与，而且构造了经过验证的“对向编辑”，确保这些编辑与任务冲突、既不能单独解决问题也不能被简单忽略，从而系统化地测试智能体在共享工作区中的感知与调和能力。论文引用的相关文献包括 EDIT-Bench 等，但具体综述细节需要查阅原文确认。

Q3: 论文如何解决这个问题？

SWE-Touch 的解决方案是一个系统化基准框架，核心组件包括：1）任务关键区域挖掘：从多条修复轨迹中定位与任务最相关的代码区域，作为潜在的用户编辑注入点。2）独立的用户补丁生成器（User Patch Generator）：为这些关键区域构造“对向编辑”（Counter-Edit）——即局部看似合理、但与任务完成目标相冲突的代码修改。3）编辑验证：确保每个 Counter-Edit 既不能单独解决任务，也不是可以随意忽略的无关改动，以保证其与任务存在真实冲突且对智能体构成有效压力。4）注入机制：在智能体执行过程中到达相关代码时，将编辑与一条带上下文的用户消息一起注入，模拟真实用户“顺手改动”的场景。该框架支持在不同基准（SWE-bench Verified、SWE-Bench Pro、DeepSWE）上运行，并支持对九个编码模型进行受控对比实验（vanilla 条件 vs. Counter-Edit 条件）。论文还配有轨迹分析，用于检查智能体在遇到冲突编辑后的行为模式，例如是否保留冲突代码、是否重新检查仓库、是否用测试验证修改等。整体上，SWE-Touch 提供了一种可扩展的、带验证的对抗性测试协议，把“用户触碰代码”这一真实协作要素转化为可量化、可复现的评估维度。

Q4: 论文做了哪些实验？

论文的实验围绕三个基准展开：SWE-bench Verified（主要评估集）、SWE-Bench Pro 和 DeepSWE（两者用于更长时间跨度的任务）。共评估九个编码模型，比较 vanilla（无用户编辑）和 Counter-Edit（注入冲突编辑）两种条件下的解决率。实验协议包括：从修复轨迹中挖掘任务关键区域；用 User Patch Generator 生成 Counter-Edit；在智能体到达相关代码时注入编辑和上下文用户消息；统计各模型解决率的变化、模型排序变化，并进行轨迹级分析。此外，论文还分析了长视野任务上的退化是否持续，以及不同模型在保留冲突代码、重新检查仓库、执行验证测试等行为上的差异。实验设计强调受控变量：编辑经过验证，排除“直接解决任务”或“可忽略”的干扰，从而确保观察到的退化确实源于智能体对共享工作区变化的处理能力不足。

Q5: 发现了什么实验现象？

实验发现的核心现象是：在 Counter-Edit 条件下，九个模型在 SWE-bench Verified 上的平均解决率下降了 7.7 个百分点，且这种退化在 SWE-Bench Pro 和 DeepSWE 的更长时间跨度任务上也持续存在。关键的反直觉结果为：一些在自主模式下解决率相近的开源模型，在用户修改代码的共享工作区中表现明显下滑，说明自主解决率并不能可靠预测共享工作区协作性能。轨迹分析揭示了两种主要失败模式：一是智能体保留用户的冲突代码，未能识别其与任务目标的矛盾；二是智能体虽然替换了代码，但没有充分重新检查仓库状态，也缺乏用针对性测试验证修改后的行为。这表明失败根因不是单纯的代码生成能力，而是对演化工作区的状态感知不足和自适应调整缺失。此外，Counter-Edit 还改变了模型的排序，说明现有基准的模型能力排行榜在交互场景下可能发生重构。这些观察共同指向一个结论：当前编码智能体在“用户触碰代码”的真实协作条件下仍存在系统性短板。

Q6: 有什么可以进一步探索的点？

论文在结论部分明确指出的关键能力方向包括：检测工作区变化、协调冲突编辑与任务目标、验证受影响行为。基于此，可以进一步探索的方向有：1）更丰富的编辑类型扩展——目前的 Counter-Edit 是任务冲突型的，未来可覆盖中立编辑、部分相关编辑、多轮连续编辑等，以更全面刻画状态感知。2）更真实的用户行为建模——模拟用户的修改动机、编辑习惯、消息表达方式，研究不同用户风格对智能体表现的影响。3）可干预的智能体设计——在框架中测试显式的状态检测模块、冲突检测与解决策略、验证性测试生成等，推动算法层面的改进。4）与其他交互维度结合——将用户代码编辑与自然语言反馈结合，研究多模态协作信号下的智能体行为。5）跨任务泛化——将 SWE-Touch 的协议扩展到代码审查、重构、多智能体协作等场景。6）可解释性与安全——分析智能体为何忽略或错误处理冲突编辑，建立更细粒度的行为分类学，为安全部署提供依据。这些方向不仅能完善基准本身，也可能启发新的模型架构和训练目标，使编码智能体真正适应人与机器共事的现实软件开发流程。

Q7: 总结一下论文的主要内容

随着前沿模型驱动的编码智能体能力迅速增强，它们对仓库范围的修改越来越难以被用户跟踪。论文指出，真实软件开发总是发生在共享工作区中：用户可能随时查看、修改代码，而现有基准却通常让智能体独自工作，或只允许用户通过消息参与，没有让用户在代码层面直接动手。这一评估缺口导致我们无法回答：编码智能体在任务进行中遇到用户修改代码时，能否理解并正确响应？SWE-Touch 正是为压力测试这一设定而提出的框架。其核心思想是构造“Counter-Edits”——对任务相关代码的合理编辑，但与任务完成目标相冲突，这些编辑经过验证，确保不能单独解决问题也不能被忽略。框架从多条修复轨迹中挖掘任务关键区域，用一个独立的用户补丁生成器构造编辑，并在智能体执行到相关代码时注入带上下文的用户消息，以此模拟真实协作中的干扰。实验部分，论文在 SWE-bench Verified 上系统评估了九个编码模型，并在更长视野的 SWE-Bench Pro 和 DeepSWE 上做了补充实验。结果显示，Counter-Edit 使平均解决率平均下降 7.7 个百分点，长视野任务上退化依然显著。更值得注意的是，这一退化伴随着模型排名的变化，一些在自主评估中表现接近的模型在共享工作区条件下明显落后。轨迹分析进一步解释了失败原因：智能体可能保留冲突代码——没有意识到用户改动与任务目标的矛盾；或者替换代码但缺少足够的仓库重检和针对性的测试验证，导致修复质量不可靠。这些证据共同表明，自主解决率并不能可靠预测用户参与时的协作性能，当前编码智能体的状态感知和自适应行为仍不足以支撑真实的共享工作区协作。论文由此提出未来优化的三个关键能力：检测工作区变化、协调冲突编辑与任务要求、验证受影响行为。SWE-Touch 的贡献在于提供了首个系统化评估这些能力的基准框架，并给出了可复现的对抗性测试协议，为后续研究指明了方向。总体而言，这项工作揭示了一个被现有基准长期忽视的重要维度，并对编码智能体评估体系的完善和交互式智能体的开发具有推动作用。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中“agent”方向高度直接相关：论文聚焦编码智能体在共享工作区中的感知与自适应能力，是 agent 评估领域的前沿工作。

## 基本信息

- 作者：Yuqiao Tan, Jinxiang Meng, Fangyu Lei, Minzheng Wang, Shizhu He, Jun Zhao, Kang Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI, cs.CL
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02499v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段（Introduction、Conclusion、Related Work、实验小节等），并结合启发式草稿进行补全；部分细节（如局限与扩展方向）属于合理推断，需查阅原文进一步确认。
