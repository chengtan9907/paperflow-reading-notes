---
user_id: "cheng tan"
paper_id: 7177
arxiv_id: "2608.07147"
title: "DiDPO: Diff-in-Diff Policy Optimization for Coding Agent Training"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07147.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07147"
abs_url: "https://arxiv.org/abs/2608.07147"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:12:09"
---
# DiDPO: Diff-in-Diff Policy Optimization for Coding Agent Training

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning with verifiable rewards · coding agents · credit assignment · code diff

## 一句话总结

DiDPO（Diff-in-Diff Policy Optimization）提出一种无评论家的强化学习算法，直接从代码差异结构中构建细粒度信用单元，以解决编码智能体训练中单步动作同时修改代码多区域导致的信用分配难题，并在长视野编码推理基准上显著超越强基线，且开源了 verl-code 训练代码库。

## 摘要

> Reinforcement learning with Verifiable Reward (RLVR) has emerged as a powerful paradigm for training coding agents, where the execution feedback from compilation and tests provides objective verification. However, unlike agent tasks, coding agents face a unique and finer-grained credit assignment challenge: at each step, coding actions simultaneously pack varying changes into different regions of a code version, which makes the contribution of independent change indistinguishable. Existing RLVR methods mostly leverage the outcome reward or step-level reward, which fails to dive into a code diff and makes unique properties of coding actions invisible to training. In this paper, we propose Diff-in-Diff Policy Optimization (DiDPO), a critic-free RL method that constructs fine-grained credit units directly from the structure of code diffs. DiDPO organizes multi-turn coding interactions into multiple thought-action steps and discovers code diffs across sampled trajectories. It then selects anchors by aggregating highly similar sub-diffs split from each whole diff by our “groupability score”, which provides the splitting schema that optimally balances the semantic scope of anchors and the group mass they may form. Finally these anchors form advantage groups and project the diff-level advantage back to individual response tokens. Experiments on long-horizon coding and reasoning benchmarks show that DiDPO significantly outperforms strong agentic RL baselines. On Qwen2.5-7B-Coder, DiDPO exceeds comparable methods by over 10% and narrows the gap with far larger models, offering a principled framework for fine-grained credit assignment in coding agent training. We also open-source verl-code, an agentic rl codebase that supports various RL methods and coding benchmarks.
> Github: https://github.com/xuc865/verl-code

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在编码智能体（coding agent）的强化学习训练中，如何为单步编码动作内部的不同代码变更分配细粒度的信用（credit assignment）。

1. **编码智能体的特殊性**：与普通智能体任务（如对话、棋类、简单控制）不同，编码智能体的一次动作往往生成一个代码差异（diff），这个 diff 可能同时包含对多个函数、多个文件或多个逻辑区域的修改。这些修改的目标不同、重要性不同、正确性也不同，但它们共享同一个可验证奖励（如编译是否通过、测试是否通过）。

2. **现有 RLVR 方法的粒度不足**：RLVR 通常用整体结果奖励（通过/失败）或步骤级奖励（每轮交互的反馈）来训练。结果奖励只能提供二值或稀疏信号，无法告诉模型哪个 hunk 是导致失败的原因；步骤级奖励虽然细一点，但仍把整个 diff 作为一个整体赋值，无法区分 diff 内部不同片段的贡献。这使得编码动作特有的“多区域同时修改”结构被丢弃，训练信号粗糙，模型难以学到精确的代码修改策略。

3. **利用跨轨迹的相似性**：论文观察到，在多次 rollout 中，编码智能体往往会产生相似或部分相似的代码差异（例如反复修改同一函数、同一逻辑片段）。这种跨轨迹的重复结构可以作为天然的信用聚类依据，但现有方法没有显式利用。

4. **问题分解**：论文把问题形式化为“如何从代码 diff 中构造动态、足够细粒度的信用单元”。这包括两个子问题：一是如何把每个 diff 拆分成有意义的子单元（子 diff）；二是如何跨轨迹聚合这些子 diff 形成优势组，以便计算组间相对优势，并把优势回传到具体 token。

5. **为什么这是难点**：代码 diff 是层次化结构（文件、hunk、行、token），直接对 token 分配优势会面临方差大、信号噪声高的问题；直接对整个 diff 分配又太粗。需要找到一种自动的、数据驱动的方法来平衡语义粒度与统计可靠性。DiDPO 的 groupability score 正是为自动切分而设计的。

6. **隐含假设**：该问题假设代码 diff 本身是训练中可观测、可解析的，且相似子 diff 在语义上具有可比性。此外，它假设跨轨迹的相似 diff 携带可迁移的信用信号，即相同代码模式的差异在相似环境下应获得相似的相对优势。

综上，论文要解决的不是“如何奖励编码智能体”，而是“如何从编码动作产生的结构化产物（diff）中提取更精细、更稳定的训练信号”，以提升长视野编码任务中的策略学习效率。

Q2: 有哪些相关研究？

论文的相关工作主要围绕 RLVR（带可验证奖励的强化学习）和组策略优化方法展开。根据摘要与检索到的片段，可以梳理出以下几个相关方向：

1. **RLVR 范式**：在代码生成、数学推理等领域，RLVR 用环境反馈（编译器输出、测试用例结果）作为可验证奖励，替代人工标注。该方法在这些任务中已成为主流，尤其适合编码智能体这种有客观评判标准的场景。

2. **基于组的策略优化（Group-based Policy Optimization）**：
 - **GRPO（Group Relative Policy Optimization）**：在生成多个采样轨迹后，计算组内相对优势，避免使用 critic 模型。DiDPO 将其作为强基线之一，并在实验中与之对比。
 - **GSPO [60]**：论文片段提到 GSPO 改进了基于组的策略优化的稳定性和效率。GSPO 可能通过某种分组或归一化机制让优势估计更稳定。
 - **GiGPO [14]**：针对长视野智能体，GiGPO 将访问相同环境状态的动作分组，从而在时间维度上共享信用。DiDPO 在实验中与 GiGPO 对比，结果显示 DiDPO 的 diff 级信用分配更优。

3. **细粒度信用分配方法**：
 - 传统方法如过程奖励（process reward）或 token 级奖励，试图对每一步或每个 token 提供反馈，但在编码任务中，这些方法无法区分同一个 diff 内不同 hunk 的贡献，且往往需要额外的训练或人工标注。
 - 基于蒙特卡洛或优势函数的方法（如 A2C/PPO 中的 advantage）通常需要 critic 网络估计价值函数，而 DiDPO 免评论家，避免了价值估计偏差。

4. **代码智能体训练框架**：论文开源了 verl-code，这是基于 verl 的 agentic RL 代码库，可能集成了多种 RL 算法和编码基准。这表明相关领域已有较多训练基础设施，但缺少考虑 diff 结构的信用分配模块。

5. **我们与这些工作的区别**：DiDPO 的独特之处在于它利用代码 diff 自身的结构（子 diff 的相似性）作为信用分组的依据，而不是像 GiGPO 那样按环境状态分组，也不是像 GRPO 那样人为设置组数或采样顺序。它在保留轨迹级优势的同时，进入 diff 内部进行二次分配，从而在不增加环境交互和评论家的情况下获得更细的信号。

需要指出的是，上述对现有工作的介绍大多来自摘要片段和引用编号，具体细节（如 GSPO、GiGPO 的确切公式）需要查看原文引用才能完全确定。

Q3: 论文如何解决这个问题？

DiDPO（Diff-in-Diff Policy Optimization）的具体方法可以从以下几个层次来理解：

1. **整体框架**：DiDPO 是一个免评论家（critic-free）的强化学习算法，使用组相对优势（group-relative advantage）进行策略更新。它不引入额外的价值网络，也不要求额外的环境回滚，而是在已有的多轨迹采样中构造信用结构。

2. **轨迹与步骤组织**：将多轮编码交互组织成多个“思想-动作步骤”（thought-action steps）。每个动作产生一段代码变化，即一个 diff。这样，一条轨迹可以表示为多个 diff 序列，每个 diff 都有对应的上下文。

3. **发现 code diff**：在采样得到的多条轨迹中，DiDPO 会对每个动作提取其产生的代码 diff。这里“发现”可能意味着解析 git-style diff 或通过代码分析识别变更区域。由于代码 diff 通常有结构（文件、hunk、行），DiDPO 可以在此基础上进一步分割。

4. **子 diff 分割与可分组性分数（Groupability Score）**：每个完整 diff 需要被分割成若干子 diff，使得每个子 diff 近似对应一个语义上独立的变化。作者提出“groupability score”来衡量一种分割方案的质量：它需要平衡两个要素——锚点（anchor）的语义范围（即子 diff 不能太小导致噪声，也不能太大导致粒度粗）和组质量（即属于同一组的子 diff 应当足够相似，形成有统计意义的集合）。论文声称该分数提供了“最优平衡的切分方案”。具体来说，可能通过优化一个目标函数来选择分割点，但公式细节需参考原文。

5. **锚点与优势组构建**：跨轨迹聚合高度相似的子 diff，形成锚点（anchor）。锚点可以看作某一类常见编码修改抽象。具有相似子 diff 的轨迹片段被归入同一个锚点组，这些组作为计算优势的基本单元。这样，原本分散在不同轨迹中的类似修改被集中在一起，便于进行组间比较。

6. **组相对优势计算**：对每个锚点组，计算组内各实例的相对优势（例如减去组平均奖励，或使用类似 GRPO 的组内归一化）。由于组内样本可能来自不同轨迹，这种优势衡量的是“在类似修改场景下，当前修改相比其他修改的相对好坏”。

7. **层级优势结合与投影**：DiDPO 保留了轨迹级（episode-level）的组相对优势，同时叠加 diff 级优势。具体地，它“将 f-level advantage 与 trajectory-level advantage 结合，并投影回生成对应 diff 的响应 token 上”。这意味着最终每个 token 获得一个信用分数，该分数既包含该 token 所在 diff 的组优势，也包含整个轨迹的全局优势。这种多层结合可以同时保持全局一致性（不偏离任务目标）和局部精细性（捕捉 diff 内部差异）。

8. **更新**：得到 token 级优势后，使用策略梯度（如 PPO 风格或直接策略优化）更新语言模型策略。由于不需要 critic，训练稳定性和计算成本优于 actor-critic 方法，且无需额外的价值模型。

9. **动态性**：锚点和组是基于当前采样批次动态构建的，不是预先定义的固定结构。这保证了方法可以适应不同任务和不同编码模式，也可以随训练进程不断变化（因为策略在变，产生的 diff 也会变）。

10. **设计权衡**：groupability score 需要权衡“锚点语义范围”和“组质量”。如果分割太细，锚点可能只包含少数样本，导致优势估计方差大；如果分割太粗，则回到整个 diff 的粒度，失去细粒度优势。DiDPO 通过可学习的或启发式的分数来自动选择平衡点。

总之，DiDPO 的方法核心在于“让代码 diff 的结构成为训练信号的结构”，实现了从轨迹级到子 diff 级再到 token 级的层次化信用分配。

Q4: 论文做了哪些实验？

论文的实验围绕验证 DiDPO 在长视野编码与推理任务上的有效性展开。根据摘要和检索到的片段，可以整理出以下实验设计信息：

1. **目标任务**：长视野编码和推理基准（long-horizon coding and reasoning benchmarks）。值得注意的是，摘要没有给出具体基准名称，这可能是 SweetBench、Terminal-Bench、SWE-bench 等，但需以原文为准。这些任务通常要求智能体在多轮交互中修改代码、运行测试、迭代修复，具有较长的时间跨度和稀疏的最终奖励。

2. **模型与骨干**：实验使用了两个骨干模型：
 - Qwen2.5-Coder-7B（摘要中称为 Qwen2.5-7B-Coder）
 - Qwen3.5-4B（注意：这个模型名称看起来像未来的版本，可能是笔误或新发布模型；原文片段中确实写了 Qwen3.5-4B，需要确认。）

3. **对比基线**：至少包括 GRPO（组相对策略优化）和 GiGPO（按环境状态分组的组策略优化）。GiGPO 被描述为“最强基线”。可能还有其他 baseline，如 PPO 或额外的结果奖励方法，但证据没有明确列出。

4. **评估指标**：采用各任务的平均成功率或平均得分百分比。片段中给出了具体数值：在两个骨干上分别达到 48.4% 和 58.6%（平均），并指出超过 GiGPO 4.2 和 4.9 个百分点。

5. **消融与分析**：片段提到“Since both share the same episode-level advantage, these gains are directly attributable to sub-diff credit”。这说明论文做了与 GRPO 的对比消融（两者使用相同的 episode 级优势，区别仅在是否有 sub-diff 信用），从而证明提升来源于子 diff 信用机制。此外，可能还有关于 groupability score 的超参数敏感性实验，但未见证据。

6. **代码库与开源**：论文开源了 verl-code，这是一个 agentic RL 代码库，支持多种 RL 方法和编码基准。这意味着实验可能是在这个库上统一完成的，增加了可复现性。

7. **实验范围**：仅报告了两个背款，覆盖的模型规模有限（7B 和 4B），没有对更大模型（如 70B）进行实验。这使得“缩小与更大模型差距”的结论只能通过与其他已公开的更大模型成绩间接对比得出。

总体来说，实验设计比较直接：在标准 agentic coding 任务上对比多个 RL 训练方法，用平均得分衡量性能，并通过控制变量（与 GRPO 共享 episode 优势）突出方法的贡献。但论文摘要提供的信息不足以让我们看到具体任务列表，读者需要查看原文表格才能获得完整实验细节。

Q5: 发现了什么实验现象？

从检索到的实验片段中，可以提取出以下具体观察和数字：

1. **总体性能**：DiDPO 在两个骨干模型上均取得了最高平均分：
 - 使用 Qwen2.5-Coder-7B 时，平均分 48.4%；
 - 使用 Qwen3.5-4B 时，平均分 58.6%。
 - 比最强基线 GiGPO 分别高出 4.2 和 4.9 个百分点。

2. **与 GRPO 的比较**：DiDPO 在 7B 骨干上比 GRPO 平均提高 5.6 个百分点。由于 DiDPO 与 GRPO 共享同样的 episode-level 优势，论文指出这些增益直接归因于 sub-diff 信用机制。这是一个重要的因果归因：表明更细粒度的信用分配是提升的主要来源。

3. **与 GiGPO 的比较**：DiDPO 领先 GiGPO 4.2（平均），说明基于代码 diff 结构的分组比基于环境状态的分组在编码任务中更有效。这体现了领域特定知识（代码 diff 语义）对信用分配的重要性。

4. **相对提升比例**：摘要还提到在 Qwen2.5-7B-Coder 上，DiDPO “超过可比方法 10% 以上”。注意，这个 10% 指的是相对提升还是绝对提升？从措辞“exceeds comparable methods by over 10%”看，很可能是相对提升（比如从 40% 提升到 44% 就是 10% 相对提升）。结合平均分 48.4%，如果 baseline 在 42-44% 左右，那么 10% 的相对提升是合理的。但没有具体 baseline 数值，只能推测。

5. **缩小与更大模型的差距**：论文声称 DiDPO 缩小了与更大模型之间的差距。这意味着用 7B 模型训练出的智能体在任务上可以接近或达到更大模型（也许 70B）经过同样训练后的表现。不过摘要没有给出更大模型的具体成绩。

6. **消融暗示**：由于与 GRPO 共享 episode 级优势，而 GRPO 通常已经包含轨迹级相对优势，DiDPO 的提升完全来自 diff 级信用的贡献。这提示在编码任务中，细粒度的信用分配比单纯的轨迹级归一化更关键。

7. **组质量与锚点的作用**：论文强调 groupability score 平衡了锚点语义范围和组质量，但没有给出消融数据。合理推测：不同的切分粒度会影响性能，过大或过小都会降低优势估计质量。这部分需要原文提供敏感性分析。

8. **潜在失败模式**：如果某个 diff 在批次中找不到相似子 diff（出现频率过低），它可能无法形成有效组，导致这些 token 获得的优势信号较弱。这种冷启动问题在早期训练中可能出现，论文未提及是否专门处理。

总之，实验结果支持 DiDPO 带来的提升主要得益于其子 diff 信用分配机制，但具体任务层面的表现、失败案例以及不同超参数下的鲁棒性还需参考原文。

Q6: 有什么可以进一步探索的点？

基于 DiDPO 的方法和现有实验，可以推测以下进一步探索的方向：

1. **更优的分割与分组策略**：目前的 groupability score 是一种启发式或学习式平衡方案。未来可以探索更精细的分割算法，例如基于代码语义（AST、控制流图）或基于语言模型嵌入的自动切分，甚至用可微分方式端到端学习分割与分组。

2. **扩展到多文件/多仓库级 diff**：目前处理的是单次动作产生的 diff，但如果动作涉及多个仓库或大规模重构，子 diff 的粒度可能更大，需要更复杂的分层信用结构。

3. **与其他细粒度信号结合**：可以引入测试失败信息（哪个测试失败、失败在哪个函数）来引导 diff 的信用分组，让优势不再单纯依赖于结果奖励，而是利用更丰富的执行轨迹。

4. **理论分析**：目前方法缺乏理论保证。可以分析 DiDPO 优势估计的方差、偏差，以及在何种条件下（如分布偏移、组大小变化）能保持无偏或低方差。

5. **自适应组大小与温度缩放**：组的样本数受采样批次影响，未来可以研究自适应采样或温度参数，确保每个锚点组有足够样本，减少极端情况。

6. **跨任务迁移**：在预训练或多任务学习中，编码智能体可能面对不同语言、不同框架的代码。DiDPO 的锚点可以积累成跨任务可复用的“常见修改模式”，形成先验知识，这值得进一步挖掘。

7. **整合到更大的模型和更长的交互**：论文仅验证了 7B 和 4B 模型，未来可以扩展到更大模型和更长交互（如多轮 agent 对话），观察细粒度信用分配在大尺度上的收益。

8. **减少对解析的依赖**：如果代码 diff 无法可靠解析（如非标准格式、二进制文件），DiDPO 可能失效。可以探索不依赖显式 diff 解析的序列级信用分配方法。

9. **与其他 RL 范式结合**：如反向 RL、课程强化学习，用 DiDPO 作为底层 reward shaping 模块；或者在离线 RL 中利用已有数据构造 diff 组。

10. **应用于其他生成领域**：代码 diff 可以类比为文档编辑、分子编辑或图结构的局部修改。DiDPO 的核心思想（从生成产物的局部结构构造信用单元）可能推广到这些领域，但需要克服表示差异。

这些方向大多基于合理推断，具体可行性需结合代码库和实验验证。

Q7: 总结一下论文的主要内容

本论文针对编码智能体强化学习训练中的细粒度信用分配问题，提出了一种名为 DiDPO（Diff-in-Diff Policy Optimization）的新算法。

**研究背景**：RLVR（带可验证奖励的强化学习）利用编译器和测试用例的执行反馈作为奖励信号，已成功用于编码智能体训练。但编码智能体的每一步动作通常产生一个包含多个修改的代码差异，这些修改对最终成功与否的贡献各不相同。现有 RLVR 方法要么使用结局奖励（二值且稀疏），要么使用步骤级奖励（粒度仍然太粗），都无法区分同一个 diff 内部不同部分的价值，使得大量的训练信号被浪费。

**方法核心**：DiDPO 免除了 critic 模型，直接从代码 diff 结构中构建细粒度的信用单元。具体流程包括：
1. 将多轮编码交互组织为 thought-action 步骤，每个动作对应一个 diff；
2. 在多个采样轨迹中提取并解析 diff；
3. 用 groupability score 将每个完整 diff 分割成语义上相对独立的子 diff，该分数在锚点语义范围和组质量之间取得平衡；
4. 跨轨迹聚合高度相似的子 diff 形成锚点组；
5. 计算每个锚点组的组相对优势，并与轨迹级优势结合；
6. 将综合优势投影回生成相应 diff 的响应 token 上，用于策略更新。

**实验结果**：在长视野编码与推理基准上，DiDPO 使用 Qwen2.5-Coder-7B 和 Qwen3.5-4B 两个骨干，平均成绩分别为 48.4% 和 58.6%，超过最强基线 GiGPO 达 4.2 和 4.9 个百分点；相比 GRPO 平均提升 5.6 个百分点。由于与 GRPO 共享 episode 级优势，这些提升被直接归因于子 diff 信用机制。此外，论文称在 7B 骨干上超过可比方法 10% 以上，缩小了与更大模型的差距。为促进研究，作者开源了 verl-code，一个支持多种 RL 方法和编码基准的 agentic RL 代码库。

**贡献与影响**：DiDPO 提供了编码智能体训练中一种原理性的细粒度信用分配框架，使模型能够从代码修改的局部结构中学习，而不是仅依靠粗粒度结果奖励。该工作展示了领域结构（代码 diff 的层次性和重复性）如何被用于改进强化学习信号，对后续 agentic RL 研究具有参考价值。

**局限**：目前实验仅覆盖两个中小规模模型，具体任务列表未知；groupability score 设计的理论依据和超参数敏感性未在摘要中展示；论文未明确处理低相似度子 diff 的信用缺失问题。这些方面有待进一步研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与用户画像中的 agent 方向（权重 0.10）直接相关，提供了 coding agent 训练的一种新 RL 算法。

## 基本信息

- 作者：Xucong Wang, Zhe Zhao, Liheng Yu, Di Wu, Xiaofeng Cao, Pengkun Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07147`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索证据（摘要、结论、实验片段），并结合启发式草稿进行了扩展；部分内容（如未来方向、具体限制）属于基于摘要的合理推断或推测，已在相应位置标注。
