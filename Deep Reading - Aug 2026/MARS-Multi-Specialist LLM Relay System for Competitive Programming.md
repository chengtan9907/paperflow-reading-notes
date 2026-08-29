---
user_id: "cheng tan"
paper_id: 9175
arxiv_id: "2608.23918v1"
title: "MARS: Multi-Specialist LLM Relay System for Competitive Programming"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23918v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23918v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:11:11"
---
# MARS: Multi-Specialist LLM Relay System for Competitive Programming

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-agent system · competitive programming · code generation · retrieval-augmented generation

## 一句话总结

MARS 提出一种纯提示词驱动的多智能体中继框架：将竞赛编程任务分派给按算法主题（动态规划、图、字符串、几何等）专门化的 LLM 智能体，用检索增强生成选择专家团队，并在每一轮将候选代码在沙箱中跑公开样例后由当前专家保留、修复或移交，最终以显著低于 CodeSIM 的算力成本在 CodeContests 上逼近其通过率。

## 摘要

> Large Language Models excel at code generation, yet competitive programming exposes a persistent failure mode: existing multi-agent pipelines distribute work over generic planner, coder, and debugger roles and delegate the choice of algorithmic technique to the backbone alone. We present MARS (Multi-Agent Relay of Specialized LLMs), a prompt-only framework in which each agent is a topic specialist---dynamic programming, graphs, strings, geometry, and so on---grounded by retrieval-augmented generation over an algorithm-theory corpus. Given a problem, retrieval selects a small team of relevant specialists; a starter writes an initial C++17 solution, and each subsequent turn runs the candidate against public examples in a sandbox, lets the active specialist keep, repair, or hand off the draft, and forwards a structured packet to the next specialist. A single infrastructure-fixer pass normalizes boilerplate at the end. On the CodeContests test split with Gemma 4, MARS reaches $0.624 \pm 0.006$ pass rate at $2.3$ recorded pipeline stages per task ($+14.4$ percentage points over direct prompting), closing most of the gap to CodeSIM ($0.731$) at $3.3{\times}$ lower wall-clock cost and substantially smaller variance in per-task token spend. The source code is available on GitHub: https://github.com/fckand/mars.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在大语言模型参与竞赛编程时，现有的多智能体流水线是否真的有效利用了大模型的算法能力，而不是把关键决策都甩给 backbone。具体可以拆成以下几点：

1. 竞赛编程暴露的持续失败模式：LLM 在通用代码生成上已经很擅长，但竞赛编程要求算法设计、复杂度分析、边界条件处理和调试能力，普通代码生成流程很难稳定通过隐藏测试。现有方法常常能写出看起来合理的代码，但缺乏对题目背后算法主题的深入理解。

2. 现有多智能体流水线的角色分配过于笼统：常见的做法是让一个通用 planner 规划，一个通用 coder 写码，一个通用 debugger 修错。这种角色划分不包含算法知识，planner 不知道这道题应该用动态规划还是图论，coder 也不一定擅长实现线段树。算法技术选择事实上完全由 backbone 模型自己完成，相当于没有引入额外知识。

3. 缺少对外部算法知识的显式利用：竞赛题目经常对应经典算法模板和理论，但现有流水线没有显式的机制去检索或引用算法教科书、题解套路和复杂度分析。模型只能凭参数化记忆来处理，这在少见或新颖的题目上容易失效。

4. 反馈信号和修复过程割裂：很多系统使用 LLM 自评或人类反馈，不够确定。竞赛编程的优势在于有确定的公开样例，可以快速验证。现有流水线要么没有在每个阶段都利用这种确定性反馈，要么反馈形式嘈杂（如众包人类反馈），导致修复过程不稳定。

5. 成本和方差问题：多智能体系统往往让多个模型反复生成大量 token，墙钟时间和 token 消耗都不可控。MARS 的目标是在不牺牲太多通过率的前提下显著降低运行成本，并减小每个任务 token 消耗的方差。

6. 研究目标：MARS 想证明，通过把智能体按算法主题专业化、用 RAG 引入算法理论、用沙箱公开样例作为确定性的回合反馈，并采用结构化中继（relay）流程，可以以纯提示词方式、不需要微调或 RL，就把竞赛编程的通过率大幅提高，同时保持低成本和高稳定性。

Q2: 有哪些相关研究？

根据摘要、引言和相关工作片段，MARS 涉及的相邻研究主要有以下几类：

1. 多智能体 LLM 系统：论文引言提到，多智能体系统最近成为复杂任务中的流行方案，包括软件开发、数学推理甚至科学发现（引用了 Guo et al., 2024; Tran et al., 2025; Chen et al., 2025a 等）。这类工作通常让多个 LLM 扮演不同角色或彼此讨论、评审，以提升整体表现。MARS 与之不同点在于，它不是按通用功能角色划分，而是按算法主题划分专家，并用中继（relay）而不是并行讨论来协作。

2. 竞赛编程作为 LLM 压力测试：相关工作中指出，竞赛编程是 LLM 代码生成能力的主要压力测试，因为题目需要精确的算法设计、复杂度和边界条件处理。CodeContests 数据集是常见的评测基准。MARS 选择在这个基准上评估，显示其面向的是难题而不是简单代码补齐。

3. 代码生成中的反馈机制：相关工作提到 noisy crowd-sourced human feedback 被用于基于强化学习的代码生成（Wong and Tan, 2024），说明社区在探索如何用反馈信号改进代码质量。MARS 刻意保持 prompt-only，并且反馈来自确定性公开样例执行，而不是嘈杂的人类反馈，这是一条不同的技术路线。

4. 阶段对齐的竞赛编程系统：论文中与 MARS 对比的主要系统之一是 CodeSIM，其通过率达到 0.731。从限制章节看，CodeSIM 是唯一的 stage-aligned 比较，也就是说它在 pipeline 结构上与 MARS 有可比性。另一个系统 PairCoder 仅支持 Python，因此无法直接比较 C++ 结果。这些系统的具体内部机制在本文片段中没有展开，可以合理推断它们也是面向竞赛编程的多智能体或多阶段代码生成方案。

5. 检索增强生成（RAG）在代码生成中的应用：MARS 的核心是对算法理论语料库做 RAG，以选择相关专家并给专家提供知识支撑。这属于 RAG 在代码领域的具体应用。通常代码 RAG 会检索代码片段或 API 文档，MARS 则检索算法理论内容，这个设定比较独特。

6. 纯提示词工程 vs 微调/RL：MARS 强调 prompt-only，不微调模型，也不使用 RL。这与利用 RL 或人类反馈优化代码生成的路线（如 Wong and Tan 的相关工作）形成对比，说明即使没有额外训练，也能通过结构化的多智能体流程带来稳定提升。

需要注意的是，检索片段只提供了引用的作者和年份，没有给出每篇工作的具体标题。上面对这些工作的归类属于合理推断，但核心对比点（通用角色 vs 主题专家、嘈杂人类反馈 vs 确定性样例反馈、微调 vs prompt-only）有论文摘要和结论的直接支持。

Q3: 论文如何解决这个问题？

MARS 的解决方案可以分成五个核心组件：主题专家智能体、RAG 检索选队、starter 初始方案、中继修复循环、基础设施修复器。

1. 主题专家智能体：每个智能体不再是通用 coder 或 debugger，而是专门针对一个算法主题，例如动态规划、图论、字符串、几何等。这种设计让模型在进入任务时就有明确的知识定位，prompt 中会包含该主题相关的算法理论背景。根据论文摘要，这些专家通过检索增强生成（RAG）在一个算法理论语料库上接地，也就是说专家在拿到题目时还能主动检索相关理论，而不是只依赖训练时记忆。

2. RAG 检索选择专家团队：对于一个具体问题，检索模块会从主题专家池中选出一个小的相关专家团队。这个选择是基于题目与算法主题的相关性，而不是固定使用所有专家。这是一种动态路由机制，可以控制 pipeline 的长度和成本。

3. starter 初始方案：选定团队后，由 starter（启动者）编写第一版 C++17 解决方案。starter 本身可能也接到检索到的主题信息，从而写出更符合题目算法类型的初始代码。

4. 中继修复循环：之后进入多回合中继。每一回合：a) 把当前候选代码在沙箱中运行公开样例；b) 将题目、当前代码、测试反馈等打包成结构化数据包；c) 当前活动的主题专家决定是保留（代码已经不错）、修复（修改当前代码）还是移交（把草稿传给下一个专家）；d) 如果移交，数据包会被转发给下一个专家。这个循环保证每一步都有公开样例的确定性反馈，而不是让模型凭空自评。

5. infrastructure-fixer 收尾：在主题专家循环结束后，再由一个专门负责基础设施的单次 pass 来规范化样板代码，例如输入输出格式、常量定义、边界处理等，避免主题专家在算法上花精力处理琐碎 I/O 问题。

6. 纯提示词驱动：MARS 不修改模型权重，不依赖强化学习或人类反馈，所有行为都是通过 prompt、检索结果和结构化数据包实现的。这降低了部署门槛，也让结果更可复现。

7. 成本控制设计：每个任务平均只记录 2.3 个流水线阶段，说明 relay 并不是无限循环。RAG 选队和结构化数据包让每个专家拿到足够上下文，同时避免所有专家都参与同一个任务，从而控制 token 消耗和墙钟时间。相比之下 CodeSIM 虽然 pass rate 更高，但墙钟时间高 3.3 倍，说明 MARS 在效率和性能之间做了明显更优的权衡。

对机制的一些细节，比如专家 prompt 的具体措辞、结构化数据包的具体字段、检索语料库的规模和组织方式，论文的 abstract 和结论片段没有展开，这些属于合理推断或缺口，需要看全文的方法章节确认。

Q4: 论文做了哪些实验？

根据论文摘要、结论和限制章节，MARS 的实验设计可以重建如下：

1. 评测基准：使用 CodeContests 测试集，总共覆盖 165 个任务。CodeContests 是竞赛编程领域常用的 benchmark，包含 Codeforces 等平台的题目，每个任务有公开样例和隐藏测试。

2. 模型（backbone）：实验在三个 backbone 上进行，但检索片段明确提到的只有 Gemma 4。MARS 报告了 Gemma 4 上的详细结果：0.624 ± 0.006 pass rate。结论片段提到优势在三个 backbone 上都能保持，说明选择三个模型是为了验证框架的泛化性，而不是针对单一模型调优。

3. 语言：系统支持两种语言，即 C++17 和 Python。摘要中的主结果基于 C++17；限制章节提到 Python 复用了同一个算法语料库和索引，说明对 Python 的支持是通过相同的 RAG 检索完成的。

4. 比较对象：包括直接提示（direct prompting）、单智能体（single-agent）、集成（ensemble）baseline，以及 CodeSIM 和 PairCoder。其中 CodeSIM 是与 MARS 阶段对齐（stage-aligned）的主要比较对象，PairCoder 则仅支持 Python。直接提示是零基础基线，用来衡量多智能体中继带来的增益。

5. 评价指标：a) pass rate（通过率，即通过隐藏测试的比例）；b) 记录的流水线阶段数（recorded pipeline stages per task，MARS 为 2.3）；c) wall-clock 成本，MARS 比 CodeSIM 低 3.3 倍；d) 每个任务 token 消耗的方差，MARS 明显更小。

6. 标签与数据规模：评估使用 Codeforces tags（题目算法标签）来组织或分析 165 个任务，具体如何使用标签在片段中没有说明，可能是用于分析 RAG 选队是否和真实算法标签吻合。

7. 方差报告：Gemma 4 上的 pass rate 是 0.624 ± 0.006，说明重复运行多次并取均值，方差很小。这是对多智能体系统稳定性的一种证据。

实验设计整体呈现的是：在中等规模 benchmark（165 任务）、多 backbone、双语言、单语料库的设定下，对比 MARS 与多种 baseline，既报告效果（pass rate）又报告效率（wall-clock、token 方差），并强调与 stage-aligned 系统的成本对比。但因为检索片段有限，实验中的消融（比如去掉 RAG、去掉专家专业化、去掉沙箱反馈等）没有出现在证据里，无法确认是否做了完整消融。

Q5: 发现了什么实验现象？

从摘要、结论和限制章节可以提取以下实验现象和观察：

1. 明显的主效果：在 CodeContests 测试集上，Gemma 4 backbone 下 MARS 的 pass rate 达到 0.624 ± 0.006，比直接提示提高 14.4 个百分点。这说明多智能体中继和主题专家化对竞赛编程有实质帮助。

2. 对单智能体和集成的稳定优势：结论片段指出，MARS 比单智能体和集成 baseline 高 6–14 个百分点，而且这一优势在三个 backbone 和两种语言上都保持。集成 baseline 通常能通过投票提高可靠性，MARS 仍然超过它，说明不是简单的多次尝试效应。

3. 与 CodeSIM 的差距和成本权衡：CodeSIM 的 pass rate 是 0.731，比 MARS 高约 10.7 个百分点；但 MARS 的墙钟时间比 CodeSIM 低 3.3 倍。也就是说 MARS 没有完全追平最强系统，但用显著更低的成本接近了它。这是系统设计中效率与效果的一个明确张力。

4. 极低的流水线深度：MARS 每个任务平均只记录 2.3 个流水线阶段。考虑到它包含 starter 加上至少一轮 relay，这个数字说明大多数任务只需要很少的中继步骤，并不会无限循环。这可能意味着 RAG 选队和结构化数据包让专家在第一二次转手时就能解决大部分问题。

5. token 消耗方差小：MARS 在 per-task token 消耗上具有比 baseline 更小的方差，说明框架在资源使用上更可预测，这对实际部署很重要。

6. 跨语言一致性：Python 复用了同一个语料库和索引，结论说优势仍然保持。这说明 MARS 的方法不是 C++ 专用，也能迁移到 Python 代码生成，但限制章节提示这种迁移是因为共享索引，而不是为 Python 单独设计知识库。

7. 未见详细消融和失败案例：检索证据没有提供消融实验的数值，也没有具体的阴性结果。因此我们只知道整体现象，不知道是 RAG、专家角色还是沙箱反馈哪一项贡献最大。这是评估中的一个信息缺口，需要在原文中确认。

整体观察：MARS 的主效应稳健，成本优势显著，但最先进方法的 pass rate 仍有明显差距；另外，实验规模（165 任务）相对有限，加上重复方差报告明确，说明作者关注统计可靠性，但统计功效依然受任务数限制。

Q6: 有什么可以进一步探索的点？

基于论文当前版本的限制和设计，可以提出以下进一步探索方向：

1. 多语言支持扩展：论文只在 C++17 和 Python 上验证，而且 Python 复用了同一个语料库和索引。限制章节明确说，更多语言需要各自独立的 prompt、代码提取（extraction）逻辑、沙箱和基础设施。未来可以研究如何构建跨语言的算法知识共享抽象层，减少为每种语言重新设计 prompt 和 I/O 处理的工作量。

2. 更大规模的评测：当前评估只有 165 个 CodeContests 任务，即使通过率方差较小，任务多样性仍可能不足。可以在更大范围或更多平台（如 AtCoder、LeetCode、ICPC 真题集）上评测，并检查 RAG 选队与真实题目 algorithm tag 的吻合度。

3. 更细粒度的算法知识组织：当前 RAG 检索的是算法理论语料库，粒度是主题。可以进一步把语料库结构化到具体算法模板、复杂度分析方法、常见坑、甚至典型 bug 模式，让专家在修复时能检索到更具操作性的知识。

4. 动态 pipeline 深度控制：MARS 平均只有 2.3 个阶段，但未必对所有题目都最优。可以训练或设计一个控制器，根据当前代码的测试通过情况动态决定是否继续 relay，以及选择哪个专家，让简单题更早退出、难题更深探索。

5. 与强化学习和可学习反馈结合：MARS 是 prompt-only，反馈来自确定性公开样例。可以探索把中继策略用 RL 或 beam search 优化，或在保留确定性的基础上加入学习到的启发式判断（何时保留 vs 修复 vs 移交）。相关工作中提到的 noisy human feedback 也可以作为补充信号，但需要设计好融合方式。

6. 消除对 RAG 语料库的依赖：当前系统需要一个手工整理的算法理论语料库，这限制了部署。可以探索从题目本身和题解中自动构建检索库，或者让专家在循环中自行搜索外部资源。

7. 与代码理解和形式验证结合：除了跑公开样例，还可以用静态分析、覆盖率引导或形式化验证来补充反馈，帮助专家发现隐藏边界情况，尤其对需要高鲁棒性的竞赛题目。

8. 跨领域迁移：论文的 motivation 提到多智能体用于科学发现。MARS 的模块化结构（主题专家 + 检索选队 + 确定性反馈中继）可能可以迁移到科学计算、数据科学代码生成或 AI for Science 中的数值方法选择，比如把算法主题换成数值方法、生物信息学工具库等。不过这是推测，需要进一步实验验证。

9. 可解释性和失败分析：目前没有看到失败案例的分类。未来可以分析 MARS 在哪些主题或难度层级上仍输给 CodeSIM，是选队错误、修复不足还是复杂度超限，以指导下一版设计。

Q7: 总结一下论文的主要内容

MARS（Multi-Agent Relay of Specialized LLMs）是一篇面向竞赛编程的多智能体 LLM 系统论文，核心主张是：与其让通用 planner/coder/debugger 角色协作并把算法选择都交给 backbone，不如让每个智能体成为某个算法主题的专家，并用检索增强生成（RAG）和确定性沙箱反馈来组织一个高效的中继流程。

论证主线：论文从 LLM 代码生成的成功和竞赛编程的失败模式切入。竞赛编程是 LLM 综合能力的压力测试，现有多智能体系统虽然在软件开发、数学推理和科学发现上有进展，但它们在竞赛编程上采用的角色划分过于通用，缺少算法知识支撑。作者认为，算法技术选择被不必要地留给了 backbone 自己，导致模型在不熟悉的题目上容易写出表面合理但实际错误的方案。MARS 的回应是：通过主题专家化和 RAG 在算法理论语料库上的检索，把领域知识显式注入到多智能体流程中；同时，用公开样例执行的确定性反馈来代替不可靠的自评或嘈杂人类反馈。

技术主线：MARS 的具体流程是：面对一个题目，先由 RAG 检索选择一个相关专家小组；starter 写一个 C++17 初始方案；此后每一回合把候选代码放到沙箱跑公开样例，当前活动专家查看测试结果并选择保留（认为已足够好）、修复（改写代码）或移交（把数据包传给下一个专家）；数据包是结构化的，包含题目、当前代码、测试反馈等信息；最后有一个 infrastructure-fixer 单次处理样板代码。整个系统是 prompt-only，不微调模型，不依赖 RL。这种设计的优势是：a) 专家知识可以即时通过检索更新，不需要重新训练；b) 每步都有确定性反馈，中继不会盲目发散；c) 通过选队和早期保留机制控制流水线深度，平均每任务只有 2.3 个阶段；d) 整体成本低、token 消耗方差小。

实验主线：论文在 CodeContests 测试集上评估，共 165 个任务，用三个 backbone、两种语言（C++17 和 Python），并使用 Codeforces tags 作为题目标签体系。主要结果是：Gemma 4 backbone 下 MARS 的 pass rate 为 0.624 ± 0.006，比直接提示高 14.4 个百分点；比单智能体和集成 baseline 高 6–14 个百分点；这一优势在三个 backbone 和两种语言上保持。与 stage-aligned 的 CodeSIM（0.731）相比，MARS 仍有约 10.7 个百分点的差距，但墙钟成本低 3.3 倍，且 per-task token 消耗方差更小。限制方面，评测规模有限，Python 复用 C++ 的语料库和索引；扩展更多语言需要重新设计 prompt、提取逻辑、沙箱和基础设施。

总体评价：MARS 是一个结构清晰、成本导向的多智能体代码生成系统。它的主要贡献可能不在于超越最强系统，而在于展示了一种以检索选队和中继修复为核心的 prompt-only 框架，能在效果和成本之间取得更好的平衡，并提高资源消耗的可预测性。由于本次检索证据只覆盖摘要、引言、结论和限制，方法内部细节（如数据包具体结构、语料库构建方式、检索算法）和完整消融结果仍需从全文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向直接相关：MARS 是多智能体系统中角色专业化与路由协议的研究实例。

## 基本信息

- 作者：Andrei Mikhailov, Mikhail Burtsev, Alsu Sagirova
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.MA, cs.PL
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23918v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、引言、结论和限制章节片段，并结合元数据与摘要进行推断；部分方法细节和实验组织属于合理推断，需原文确认。
