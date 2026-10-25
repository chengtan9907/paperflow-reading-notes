---
user_id: "cheng tan"
paper_id: 14304
arxiv_id: "2609.38349v1"
title: "MILO: Automated Harness Discovery via Orchestrated Multi-Agent Evolution"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38349v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38349v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/reading_reports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-08T23:43:00"
---
# MILO: Automated Harness Discovery via Orchestrated Multi-Agent Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent harness · multi-agent evolution · meta-evolution · island model

## 一句话总结

本文提出 MILO（Meta-evolutionary Island Orchestration），一个通过多智能体元进化自动发现 AI Agent harness（控制流与交互软件层）的框架，在 Terminal-Bench 2.1、PaperBench 和 DeepSWE 三个长时任务基准上，用前者优于八个现有 harness 和六个搜索方法，并以 +12.0%、+28.3%、+10.3% 的分辨率提升幅度超过已有搜索方法。

## 摘要

> 现代智能体系统通常由一个 AI 模型和一层 harness 组成，harness 负责管理模型的控制流、提示、工具、上下文和记忆，并决定子智能体生成、工具调用拦截、输出验证和终止条件。大量证据表明 harness 设计对智能体在长时任务上的性能影响与模型本身相当，但构建有效的 harness 需要大量人工努力，且搜索空间组合爆炸，模型更新后又需要重复劳动。已有的自动化方法要么只优化 prompt 或 skills 等局部部分，要么试图重写整个 harness，但往往受限于固定搜索策略和 LLM 驱动的搜索中的剥削性偏差，陷入局部最优。为此，本文提出 MILO，一种元进化框架，将 harness 与其自身搜索策略共同进化。三个组件驱动搜索：(i) 基于岛屿树的层级谱系内存，利用被拒绝的突变作为负证据剪枝无希望路径并引导向有希望的谱系；(ii) 每岛变异体智能体结合全局搜索历史与父代弱点反馈重写整个 harness；(iii) 编排者智能体通过嫁接谱系、物种形成、重新分配变异体和修订课程来适应搜索。MILO 在三个长时基准上超越多个强 baselines，并展现了结构级 harness 重设计与自适应性搜索相对于单纯 prompt 或 skill 精炼的价值，且 harness 无需进一步搜索即可迁移到 Frontier-Bench，并收紧 EinsteinArena 上三个已知最优边界。

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：如何自动化设计高质量、适配特定模型与任务的 AI Agent harness？这个问题的重要性源于两个观察：第一，harness 对智能体性能的影响不亚于底层模型——例如在 Terminal-Bench 上，同样的 GPT-5 模型在 Terminus 2 下只能解决 35.2% 的问题，在 Codex harness 下可解决 49.6%，且 token 消耗减少 35%；第二，现有 harness 设计几乎完全依赖人工：生产级 harness（如 Claude Code、Codex）需要团队数月才能构建，而开源 harness 也靠专家手工调教。随着模型频繁更新，这种人工劳动需要不断重复，成为智能体部署的瓶颈。现有自动化方法存在结构性问题：一些方法只优化 harness 的局部（如 prompt、skills），无法触及整体控制流；另一些试图重写整个 harness（如 Self-Harness、Meta-Harness），但其搜索策略固定，且基于 LLM 的变异体往往具有剥削性（exploitative）偏差——即过于聚焦当前已知较好的区域，缺乏探索新结构的动力，难以跳出局部最优。此外，这种搜索的样本效率也很关键，因为长时任务的评估代价昂贵，需要或acles 才能支持大量实例级搜索。因此，论文的目标是设计一种既能探索结构空间、又能利用已有知识、还能自适应调整搜索策略本身的自动化框架，从而以较低成本找到高性能、可复用的 harness。

Q2: 有哪些相关研究？

相关研究可以按几条主线梳理：
1. **Harness 工程与手工设计**：生产系统如 Claude Code、Codex 由团队手工构建，体现专家知识驱动；开源 harness 也依赖人工维护。已有证据表明 harness 对性能影响巨大（Anthropic 2025b, Tian et al. 2026）。
2. **局部优化方法**：许多工作专注于优化 harness 内的 prompt、few-shot examples、skills 或工具集，例如通过 prompt 搜索或 skill 库扩充来提升性能，但这类方法只探索了 harness 设计空间的小部分，无法改变整体控制流。
3. **全局 harness 搜索**：Self-Harness、Meta-Harness 等方法尝试用 LLM 作为变异算子重写整个 harness，但由于其固定搜索策略和 LLM 变异体固有的剥削性偏差，通常只能进行局部改进，探索能力有限。
4. **进化计算与多智能体搜索**：FunSearch 等实例级方法依赖快速 oracle 评估，在程序合成中有效；而基于岛屿模型、谱系树、元进化等进化算法则在更宽阔的搜索空间中保持多样性，并可通过记忆负面信息来引导搜索。MILO 借鉴这些思想，并将其应用到 harness 发现这一策略级搜索问题上。
5. **LLM 驱动的搜索与自适应搜索策略**：有工作使用 LLM 来生成候选解，但往往缺少对搜索策略的自我调整；MILO 通过编排者智能体动态调整变异、选择和课程，实现元进化。
6. **基准与评估**：Terminal-Bench、PaperBench、DeepSWE 等长时任务基准用于衡量 agent 在真实环境中的执行能力，这类基准使得自动 harness 发现可以被量化评估。

Q3: 论文如何解决这个问题？

MILO 采用元进化岛屿编排（Meta-evolutionary Island Orchestration）框架，将 harness 的搜索问题公式化为在结构空间中维护一组候选解（即谱系树中的节点），并同时进化搜索策略本身。具体而言，三个组件协同工作：

**(i) 层级谱系内存（Hierarchical Lineage Memory）**：MILO 维护一个基于岛屿的谱系树，每个岛屿保存一组相关的 harness 谱系。该内存将已评估的 harness 按父子关系和祖先层级组织，记录哪些突变被接受、哪些被拒绝。被拒绝的突变被明确保留为负证据，用于剪枝不希望的搜索分支，避免重复探索已知失败方向，并将搜索导向有希望的谱系区。这样既保留了多样性（不同岛屿代表不同选择压力下的谱系），又提供了结构性的记忆来指导后续搜索。

**(ii) 每岛变异体智能体（Per-Island Mutator Agents）**：每个岛屿配备一个或多个专门的变异体智能体，它们负责根据当前谱系节点的弱点信息以及整个搜索的全局统计，重写整个 harness 的结构，而不只是局部 prompt。这些变异体智能体结合两条信息源：一是全局搜索历史（例如哪些谱系表现出色、哪些模式频繁出现），二是父代 harness 的失败反馈（例如在验证中暴露的错误类型），从而生成结构上创新的后代。

**(iii) 编排者智能体（Orchestrator Agent）**：当搜索进展停滞时，编排者有权调整搜索策略本身，包括：嫁接（grafting）不同谱系以组合优点、物种形成（speciation）来保护有前途的远离主流区域的谱系、重新分配变异体智能体到不同岛屿以改变探索压力，以及修订训练课程（curriculum）——例如调整评估难度或切换任务子集。通过这种方式，MILO 将“搜索策略”视为可进化的对象，实现元进化：harness 与搜索策略共同适应。

整体上，MILO 在探索与利用之间动态平衡：谱系内存保留多样性并利用负面信息，变异体智能体利用全局与父代信息进行结构重写，编排者则根据搜索进度调整宏观策略。搜索过程以 resolution rate（任务解决率）或类似的收益函数 F(h) 为优化目标，并采用接受准则（如扩大支配区域）来决定后代是否进入下一代。

Q4: 论文做了哪些实验？

论文在三个长时任务基准上进行实验：Terminal-Bench 2.1、PaperBench 和 DeepSWE。这些基准分别涵盖终端命令执行、论文复现/数据科学工作流、以及软件工程任务，要求智能体在长时间内进行多步规划和工具调用。

作为对比，论文使用了八个现有 harness（包括生产级和开源 harness）作为基线，并列举了六个搜索方法：GEPA、Meta-Harness（以及其他未在摘要中显式列出的方法）作为自动 harness 搜索的竞争对手。测试的 backbone 包括前沿模型 Opus 4.8 和开放权重模型 gpt-oss-120b。评估指标主要为分辨率（resolution rate），即成功完成任务的占比。

主要实验结果显示，使用 Opus 4.8 时，MILO 发现的 harness 相对其初始 harness 将分辨率分别提升 +12.0%（Terminal-Bench 2.1）、+28.3%（PaperBench）和 +10.3%（DeepSWE），而之前最佳搜索方法的提升幅度仅为 +4.5%（GEPA）、+18.3%（Meta-Harness）和 0%（无提升）。这表明 MILO 在所有基准上均优于现有搜索方法。

除了主要基准外，论文还进行额外验证：将 MILO 搜索到的 harness 直接迁移到 Frontier-Bench，无需进一步搜索即表现良好；在 EinsteinArena 上，MILO 发现的 harness 收紧了三项已知最优上界（例如一个自相关不等式上界从 1.4508066 降至 1.4488860），这些成果可能来自 MILO 搜索到的高效优化程序（如 574 行的优化器）。此外，实验还揭示了结构级 harness 重设计（如引入子代理的明确职责划分、复制 skills 目录等）对性能提升的关键作用。

Q5: 发现了什么实验现象？

实验发现的核心现象可归纳如下：

1. **结构重设计带来的增益远超局部优化**：MILO 通过在搜索中引入被拒绝突变作为负证据，并允许变异体智能体重写整个 harness 结构，相比仅优化 prompt 或 skill 的已有方法，获得了显著更高的分辨率提升。在 Terminal-Bench 2.1 上，MILO 的增量 +12.0% 是 GEPA 的 +4.5% 的近三倍；在 PaperBench 上 +28.3% 超出 Meta-Harness 的 +18.3%；在 DeepSWE 上，先前最好方法完全没有提升（0%），而 MILO 达到 +10.3%。这显示出在长时任务中，整体控制流与工具调用决策的改变比局部提示调整影响更大。

2. **剥削性偏见导致现有 LLM 搜索陷入局部最优**：论文指出固定策略的 LLM 变异体倾向于剥削已知良好区域，从而无法探索足够的结构多样性。MILO 通过岛屿谱系内存和编排者的物种形成机制来保持不同区域的探索，从而缓解了这一问题。

3. **负面信息（被拒绝的突变）的价值**：MILO 明确保留并利用被拒绝的突变作为剪枝依据，这显著提高了搜索的样本效率，避免了重复探索失败路径。这在昂贵的长时任务评估中尤其重要。

4. **跨基准迁移能力**：MILO 在某一基准上发现的 harness 可以直接迁移到 Frontier-Bench，无需进一步搜索，表明这些 harness 具有跨任务泛化能力，而非对特定基准的过拟合。

5. **在数学/优化任务上的意外收益**：尽管论文主要聚焦于 agent 任务，但 MILO 方法还能发现高水平的优化器程序（如 574 行、基于 log-sum-exp 和 FFT 梯度的优化器），并在 EinsteinArena 上改进已知最优边界。这可能反映了 harness 搜索中发现的通用编程模式。

6. **搜索策略的自适应是关键**：编排者通过嫁接、物种形成、重新分配变异体、修订课程来调整搜索，这种元进化层面的自适应使得 MILO 能在不同基准上持续取得进展，而固定策略方法容易停滞。

Q6: 有什么可以进一步探索的点？

基于目前的结果和推断，后续可以在以下方向进一步探索：

1. **更大范围的结构搜索空间**：当前 MILO 的 harness 表示可能仍局限于特定模板，未来可支持更抽象的控制流原语（如循环、并行、条件分支、记忆检索）以实现更复杂的 agent 架构。
2. **样本效率与成本优化**：长时任务评估非常昂贵，可以研究基于代理模型或早期停止策略来进一步减少评估开销，或者利用 faster oracles 来扩大搜索规模。
3. **更多 backbones 与模型家族适配**：实验只使用了两个 backbone（Opus 4.8、gpt-oss-120b），未来可验证方法对更多模型（包括小模型、多模态模型）的有效性，并研究 harness 在不同模型间的迁移。
4. **跨任务与跨生态系统的迁移**：论文展示了到 Frontier-Bench 的零搜索迁移，但尚未系统研究哪些 harness 组件具有通用性，哪些需要针对任务重设计；可分析谱系树中共享的结构模块。
5. **自动课程与多任务协同**：编排者已经可以修订课程，未来可进一步让课程学习根据运行时反馈动态选择训练任务分布，甚至自动生成新的任务来暴露弱点。
6. **可解释性与安全性**：搜索到的 harness 可能包含复杂嵌套逻辑，需要研究其可解释性和可审计性，以及是否可能引入安全隐患（例如过度授权子代理）。
7. **将元进化思想应用于其他软件工程场景**：MILO 的框架思路（负证据记忆、岛屿谱系、元级编排）可推广到其他自动化设计问题，例如 API 设计、工具规划、人机交互界面等。
8. **更深入的理论分析**：目前对探索-利用平衡的保证仍是经验性的，可以尝试分析岛屿谱系内存的剪枝效果和编排策略的收敛性。

Q7: 总结一下论文的主要内容

本论文的核心贡献是提出一个自动化的、自适应的 harness 发现框架 MILO，并展示了结构级 harness 重设计结合元进化搜索能显著超越传统局部优化方法。论证主线从“harness 对 agent 性能影响巨大”这一经验观察出发：作者引用多项证据，如 GPT-5 在 Terminal-Bench 上使用不同 harness 的解决率差异（35.2% vs 49.6%，且 token 消耗减少 35%），以及生产 harness 需人工数月构建的高成本，提出 harness 工程应该被自动化。进而指出现有自动化方法的不足：只优化局部（prompt/skills）方法探索空间有限；整个 harness 重写方法（Self-Harness, Meta-Harness）受限于固定搜索策略和 LLM 变异体的剥削性偏差。技术主线上，MILO 以“元进化”为核心理念，将搜索策略本身也作为进化对象。具体构成包括三层：① 层级谱系内存——将 harness 候选组织成岛屿化的树，既保持多样性，又通过保留被拒绝突变作为负证据来剪枝失败方向；② 每岛变异体智能体——它们综合全局搜索历史和父代弱点反馈，重写整个 harness 的结构；③ 编排者智能体——当进展停滞时，动态调整记忆结构（嫁接/物种形成）、变异体分配和训练课程。这种三层设计使搜索在全局与局部、探索与利用之间自适应平衡。实验主线围绕三个长时任务基准（Terminal-Bench 2.1、PaperBench、DeepSWE）展开，以八个现有 harness 和六个搜索方法为基线。使用 Opus 4.8 和 gpt-oss-120b 两个 backbone，结果显示 MILO 相对于初始 harness 的分辨率提升分别为 +12.0%、+28.3%、+10.3%，而最佳既有搜索方法的提升只有 +4.5%（GEPA）、+18.3%（Meta-Harness）和 0%。这证明了方法在三个不同任务类型上的优越性和稳定性。此外，MILO 发现的 harness 能零搜索迁移到 Frontier-Bench，并在 EinsteinArena 数学问题上收紧已知上界，显示其发现的解具有一定通用性。结论部分强调：自动 harness 发现是一个值得深入研究的范式，可提供比 prompt 精炼更强的性能提升；智能体无需重新训练模型，就能通过更好的 harness 获得更强、更高效、更可复用的行为。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向高度相关：论文直接面向 AI Agent 的 harness 自动化设计，涉及控制流、工具调用、子代理等核心机制。

## 基本信息

- 作者：Prithwish Jana, Mononito Goswami, Hao Liu, Xinyu Li, Langlin Huang, Zhehui Huang, Zhishen Huang, Patrick Blöbaum, Anoop Deoras, Purak Jain, Nikos Kanakaris, Sahika Genc
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38349v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文的 PDF 语义检索证据片段（包括 Introduction、Method、Conclusion 等），并结合摘要与目录信息进行推断和补全。
