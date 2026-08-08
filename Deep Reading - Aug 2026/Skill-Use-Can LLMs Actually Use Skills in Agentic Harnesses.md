---
user_id: "cheng tan"
paper_id: 6807
arxiv_id: "2608.04828"
title: "Skill-Use: Can LLMs Actually Use Skills in Agentic Harnesses?"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.04828.pdf"
pdf_url: "https://arxiv.org/pdf/2608.04828"
abs_url: "https://arxiv.org/abs/2608.04828"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-06T11:19:12"
---
# Skill-Use: Can LLMs Actually Use Skills in Agentic Harnesses?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · skill use · benchmark · progressive disclosure

## 一句话总结

论文提出 SKILL-USE 基准，在渐进披露条件下评估 LLM 智能体的技能使用能力，将技能使用分解为触发、遵循、边界三个维度，发现最强配置下 SU 分数仅 0.613，且技能使用能力依赖 harness，不是模型的固定属性。

## 摘要

> Large language model (LLM) agents increasingly rely on skills, structured documents that specify when to act, which procedure to follow, and which tools are allowed. Existing evaluations mostly judge the quality of a skill or its contribution to task success, leaving unexamined whether an agent can recognize a relevant skill and apply it on its own. We introduce SKILL-USE, a benchmark that evaluates skill use under progressive disclosure, where an agent sees only a skill's name and short description and must retrieve the full procedure before following it. SKILL-USE separates three facets of skill use. Trigger measures whether the agent invokes the relevant skill, Compliance measures how faithfully it follows the prescribed procedure, and Boundary measures whether it avoids forbidden operations. A Skill-Use (SU) score combines the three and credits execution only after the skill is triggered. SKILL-USE pairs 79 real skills with 177 executable tasks across nine domains, each grounded in real files, run in an isolated Docker sandbox, and scored by a trajectory-based rubric. Evaluating eight LLMs under two agent harnesses, we find that reliable skill use remains out of reach, as the strongest configuration reaches an SU of only 0.613. Triggering and procedural compliance fail as independent bottlenecks, and both scores and model rankings shift with the harness, so skill use behaves as a capability conditioned on the harness rather than a fixed property of the model.
> ![](images/f552548cfcbfc3408f377cb9e93009c8d87fde49544bd84506c125dad24e0937.jpg)
> (a)
> ![](images/738df0f39e649ed3838cdba87dc6055ff5d69f1736c4f807bfadbe82655a0cef.jpg)
> (b)
> ![](images/8cf51355f5099b011390195081b3f9691d16cf6bbb51fe9f61a66388c8927f18.jpg)
> ![](images/4d3b24fd4dec464ae0a1110a2f241c54569e0137effa9680647de9b50a4e68be.jpg)
> Figure 1: Composition and coverage of SKILL-USE. (a) Domain distribution of the 79 skills across nine categories, where the inner ring gives each category's share and the outer ring lists representative skills. (b) Skill and task counts per domain, where each bubble marks one domain and reports its skill and task totals. (c) Composition of the rubric items by type. (d) Distribution of skill-exclusive requirements per task, our proxy for difficulty, where the dashed line marks the mean.

Q1: 这篇论文试图解决什么问题？

**核心问题：** LLM 智能体越来越依赖“技能”——即结构化文档，其中规定了何时行动、遵循什么流程、允许哪些工具。然而，现有评估大多只判断技能本身的质量或技能对任务成功的贡献，未检验一个关键能力：智能体能否自己识别出相关技能并正确应用它。

**未检视的环节：** 技能使用包含多个环节：
1. 触发（Trigger）：在适当情境下主动调用相关技能；
2. 遵循（Compliance）：忠实地按技能规定的程序执行；
3. 边界（Boundary）：避免使用技能明令禁止的操作。
这三个环节在之前几乎没有被系统分离评估，因此我们不知道模型“会用技能”究竟是哪个环节在拖后腿。

**渐进披露的挑战：** 实际部署中，智能体通常只看到技能的元信息（名称、简短描述），需要自行检索完整说明。这种渐进披露（progressive disclosure）更符合真实使用场景，但也对模型提出了更高的主动检索和信息利用能力。现有指令跟随评估往往把完整指令直接给模型，没有模拟这种披露机制。

**Harness 的影响：** 智能体总是跑在某个框架（harness）上，框架可能提供不同的工具集、调用方式、上下文管理。技能使用是否受 harness 影响？如果受影响，那么“模型能否使用技能”就不是模型的固定属性，而是模型与 harness 交互的结果。这需要跨 harness 的评估来回答。

**问题总结：** 论文试图构建一个能够分离触发、遵循、边界三个维度的基准，在渐进披露条件下测试真实技能使用，并考察 harness 变化对结果的影响，从而填补“技能使用评估”这一空白。

Q2: 有哪些相关研究？

根据摘要和检索片段，相关研究工作可分为几类（以下归类基于领域常识与摘要线索推断，具体文献需查阅原文）：

1. **技能库与技能学习**：LLM agent 常借助技能库（如自动编写和复用的程序片段）提升能力。既有工作多聚焦于技能如何生成、组织、复用，以及每个技能的质量（例如可读性、可执行性）。但很少直接测量“模型是否在正确时候想起并调用合适的技能”。

2. **指令跟随评估**：如 IFEval 等基准，给模型完整指令，看它能否满足格式化或约束要求。它们通常不涉及技能检索或渐进披露，也不区分“知道该做什么”和“真的去做了”。SKILL-USE 明确将技能使用视为指令跟随的一种延伸，但把“披露程度”作为变量。

3. **Agent 端到端基准**：如 WebArena、AgentBench、Minecraft 类环境等，评估任务成功率，但往往把技能使用作为隐含能力，不单独计分。SKILL-USE 则专注于技能使用的专项指标。

4. **Harness 与提示工程研究**：有大量工作探索不同 prompt 结构、工具调用方式对 agent 表现的影响，但很少系统量化 harness 对技能使用各项能力的影响。本文通过双 harness 对比，把 harness 视为影响技能使用的重要变量。

**与其他方向的关系：** 论文明确提到“将技能基准从工件质量转向实际使用”，意味着对抗的是只评技能“写得好不好”的范式；同时“扩展指令跟随评估到渐进披露技能”，则是补上了披露条件这个维度。由于可见的检索片段有限，更具体的相关工作名单与对比结果需要回到原文确认。

Q3: 论文如何解决这个问题？

**总体思路：** 提出一个基于真实技能与真实任务的基准，让智能体在渐进披露条件下使用技能，并用轨迹级 rubric 分别给触发、遵循、边界打分。

**具体步骤（依据摘要与片段整合，合理推断）：**
1. **技能与任务配对**：收集 79 个真实技能条目，覆盖 9 个领域（软件研发、基础设施、数据科学、数据库、文档处理、安全等）。为每个技能设计多个可执行任务，共 177 个任务。每个任务都基于真实文件，确保环境和操作是真实的。
2. **渐进披露机制**：在交互开始时，智能体只能看到技能的名称和一句话描述；若想使用技能，必须先通过某种检索动作（如 cat 一个文件或调用查看函数）获取完整的技能程序。这模拟了只给索引不全文的现实场景。
3. **三维评分**：
 - Trigger：智能体是否在合适时机主动调用了该技能（或至少明确表示需要该技能）；
 - Compliance：执行轨迹是否遵循技能中规定的步骤顺序和细节；
 - Boundary：是否执行了技能中明确禁止的操作（如跳过校验、使用不安全的参数等）。
4. **SU 分数**：SU = 综合三维度的得分，且只有先触发（Trigger=1）时执行部分才被计入。否则即使操作正确，也因未使用技能而不得分。
5. **质量过滤**：使用 masked-skill screening 剔除那些不依赖技能本身也能完成的通用任务，保证任务确实需要对应技能。
6. **端到端验证**：对每个“技能-任务-rubric”三元组进行人工验证，确保任务可执行、rubric 判定合理。
7. **环境与自动化**：每个任务在隔离 Docker 沙箱中运行，智能体的行为通过轨迹记录，rubric 判定由程序或人工完成。

**评估协议：** 选取 8 个 LLM，在两种不同的 agent harness 下运行同一组任务，分别记录触发率、遵循率、边界违反率以及 SU 分数。通过对比 harness 间差异，判断技能使用能力的稳定性。

Q4: 论文做了哪些实验？

**基准构建层面的验证：**
- 对 79 个技能和 177 个任务做了“技能-任务-rubric”三元组的端到端验证，确保任务可执行且 rubric 能区分好坏行为。
- 进行了 masked-skill screening，即把技能名遮盖后测试任务仍否可解，来剔除不依赖技能的“通用要求”。这保证了 benchmark 测的是技能使用，而不是通用能力。

**主实验：**
- 评估 8 个 LLM（具体模型列表未在摘要中给出，推测包含 GPT-5.5 等前沿模型）在 2 种 agent harness 下的表现。其中一种 harness 简称 CC（完整名称未知），另一种未命名。
- 每个模型在每个任务上执行一次（或多次？），在 Docker 沙箱中产生轨迹，并根据轨迹打分。
- 报告了 SU 分数及其子维度（Trigger、Compliance、Boundary）的得分，以及模型排名。

**对比分析：**
- 分析触发失败与遵循失败是否独立出现。
- 比较同一模型在不同 harness 下的分数和排名，检验 harness 的影响。
- 从片段“Missed triggers alone do not explain...”推测，作者还分析了遗漏触发之外的失败原因，可能有更细粒度的错误类型划分。

**评估范围：** 9 个领域覆盖了软件研发、基础设施、数据科学、数据库、文档内容处理、安全等（从片段得知），但具体每个领域的任务数量分布未知。任务均基于真实文件，需要模型操作文件系统、运行命令、读写数据等。

Q5: 发现了什么实验现象？

**主要发现：**
- **整体能力远未成熟**：最强配置（GPT-5.5 在 CC harness 下）SU 分数仅为 0.613，意味着可靠的技能使用仍遥不可及。大多数模型在只给技能名称和描述的情况下，不能稳定地触发并正确执行。
- **触发和遵循是独立瓶颈**：识别出正确技能但执行偏离程序的情况很常见；反过来，有能力按程序执行却不能在正确时机触发的模型也存在。这意味着触发和遵循是两个需要分别提升的能力，而不是同一个“技能使用”能力的两个侧面。
- **Harness 依赖性强**：同一模型在不同 harness 下的绝对分数和相对排名都会变化。技能使用表现像是“模型×harness”的联合属性，而不是模型固有的能力。这对“哪个模型更会用技能”的常规比较提出了质疑——结论取决于你给它搭了什么框架。
- **错过触发不是唯一失败原因**：摘要片段提到“Missed triggers alone do not explain...”，推测存在因错误触发、过早触发或触发后未正确执行等复合原因，需要看原文的详细错误分析。
- **边界维度的表现未被单独报告**：从现有材料无法得知边界（禁止操作）的遵守程度，可能各模型在禁区面前的失误率不同，这有待原文补全。

**反直觉点：** 人们可能认为只要技能写得好，模型就能用。但实验表明，即使面对真实技能，模型仍然在“想起来调用”和“按规矩办”两个环节上大量失败，且这种失败高度依赖外部架子。这暗示技能使用不是开箱即用的能力，而是需要与 harness 协同设计的系统问题。

Q6: 有什么可以进一步探索的点？

基于论文结论和当前证据，可以探索以下方向：
1. **改进触发机制**：如何让模型在只有技能名称和描述时更可靠地识别相关场景？能否通过训练或检索增强提升触发率？
2. **解耦 harness 影响**：设计一个“harness 无关”的技能使用评估协议，或者研究哪些 harness 设计（如工具列表、动作空间、上下文长度）最能发挥模型潜力。
3. **提升遵循稳定性**：技能程序往往很长，模型容易在中途偏离。如何利用结构化提示、子目标分解或外部记忆来增强遵循？
4. **边界安全的强化**：对禁止操作的高代价场景，能否在 harness 层面加装硬约束，而不是只依赖模型自觉？
5. **扩展到更多技能形态**：当前技能是文本程序，未来可包含多模态、人机交互或需要外部资源的技能。
6. **将技能使用与其他能力关联**：比如规划、工具选择、错误恢复，这些能力如何与触发和遵循交织？
7. **基准自身的完善**：增加更多领域、更多技能类型、更多 harness，并研究任务难度与技能长度的变化趋势。
8. **训练导向**：能否设计专门训练目标让模型适应渐进披露，比如在 SFT/RHLF 中引入“先检索再执行”的课程？

这些方向大多基于论文结论合理推断，具体可行性需原文进一步讨论。

Q7: 总结一下论文的主要内容

**论证主线：** 论文从一个现实观察出发：LLM 智能体越来越多地依赖“技能”这种结构化文档，但现有评估只关注技能自身质量或任务最终成功，对智能体“会不会用技能”这个中间环节几乎没有刻画。作者认为，技能使用可分解为三个可独立测量的方面：触发、遵循、边界。如果这三个方面都能被可靠评估，我们才能真正回答“模型是否掌握了技能”的问题。

**技术主线：** 为此，作者构建了 SKILL-USE 基准。其核心设计是渐进披露：智能体只看到技能名称和一句描述，必须先自行检索完整技能文档才能执行。基准用 79 个真实技能配对 177 个真实文件任务，覆盖 9 个领域，所有任务运行在 Docker 沙箱中，通过轨迹级 rubric 评分。SU 分数把三维度整合，触发是执行计分的前提。构建过程中采用 masked-skill screening 过滤通用性任务，并对每个技能-任务-rubric 三元组做端到端人工验证，尽量保证任务必须依赖技能才能完成。

**实验主线：** 论文选取 8 个 LLM 和 2 种 agent harness，逐一跑完 177 个任务，记录触发、遵循、边界得分与 SU。结果显示最强配置（GPT-5.5 × CC）SU 只有 0.613，说明可靠技能使用尚未实现。更关键的是，触发与遵循是彼此独立的瓶颈：识别出技能不等于能按流程执行，能执行也不等于会主动触发。而且同模型在不同 harness 上的分数和排名变化显著，说明技能使用不是模型固有属性，而是模型与 harness 交互出来的系统能力。

**整体评价：** 这项工作把“技能使用”从技能质量和任务成功中独立出来，给出了一个可操作、可分维度的评估框架，并且第一次系统暴露了 harness 对技能使用的影响。它为后续提升 LLM 智能体的技能运用能力提供了清晰的度量基准和失败模式分析。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你画像中的“agent”方向直接相关：它专门评估 LLM 智能体的技能使用能力，是 agent 评估体系的重要一环。

## 基本信息

- 作者：Jinyi Han, Yuanjian Xu, Ying Liao, Xinyi Wang, Zishang Jiang, Zixiang Di, Fanyang Lu, Zhichao Hu, Yanghua Xiao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.04828`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要及检索到的若干 PDF 语义片段（包括方法、结果、局限等），但未能访问全文，部分细节为合理推断。
