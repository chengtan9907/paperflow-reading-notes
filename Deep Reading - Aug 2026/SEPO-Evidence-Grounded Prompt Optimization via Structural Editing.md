---
user_id: "cheng tan"
paper_id: 9652
arxiv_id: "2608.28067"
title: "SEPO: Evidence-Grounded Prompt Optimization via Structural Editing"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.28067.pdf"
pdf_url: "https://arxiv.org/pdf/2608.28067"
abs_url: "https://arxiv.org/abs/2608.28067"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:14:36"
---
# SEPO: Evidence-Grounded Prompt Optimization via Structural Editing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：prompt optimization · structural editing · edit-effect lineage · interpretability

## 一句话总结

SEPO 是一种以'编辑-效应谱系反馈'为核心的多轨迹提示优化器，通过结构化编辑两层级提示模式中的稳定类型化单元，把每次局部编辑与它新修复或破坏的样本联系起来，使提示优化变得可定位、可归因、可操作，并在 14 个保留任务上超过最强基线 GEPA，同时位于优化时间和测试时间的帕累托前沿。

## 摘要

> Existing API-only prompt optimisers are often described as interpretable, but in practice, this usually means only post-hoc inspectability: each iteration still rewrites the prompt as one opaque string, leaving a trace of full-prompt diffs rather than localisable, machine-readable edits. This paper introduces SEPO (Structural, Evidence-grounded Prompt Optimization), a multi-trajectory prompt optimiser centred on edit-effect lineage feedback. Rather than treating each iteration as an isolated whole-prompt rewrite, SEPO locally edits stable, typed units in a two-layer prompt schema, links the target and realised structural operations of each edit to the examples it newly fixes or breaks, and carries this edit-effect record forward to guide later architect calls on the same search branch. This makes prompt optimisation addressable, attributable, and actionable. Across a 14-task held-out suite, SEPO improves over the strongest baseline, GEPA, by 3.1 pp on Llama-3.1-8B-Instruct and 2.2 pp on Qwen3-8B, reaching 61.9% and 73.3% macro accuracy. SEPO also lies on both the optimisation-time and test-time Pareto frontiers, spending 2.9M optimisation tokens versus 4.1M for GEPA and producing prompts over 5x shorter.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：现有 API-only（仅通过 API 调用，不访问内部权重）的提示优化方法虽然声称'可解释'，但实际上只是事后可检查性（post-hoc inspectability）。具体表现为：

1. **不透明整串重写**：每次迭代仍将提示视为一个不透明的字符串进行整体重写，用户看到的是整段提示的差异（full-prompt diff），而不是细粒度的、可定位的编辑记录。
2. **不可归因**：无法清晰地知道某次改动具体修复了哪些样本、又破坏了哪些样本，优化过程中的好与坏难以追溯到具体编辑操作。
3. **不可积累**：由于缺乏结构化的编辑记录，后续迭代容易重复累加规则（rule accretion），或者不得不再次进行大范围重写，导致提示膨胀（prompt bloat），并使有效改进难以在迭代中保持。
4. **缺乏机器可读性**：全量提示 diff 难以被自动化工具或后续流程直接消费，无法形成可组合、可复用的编辑历史。

本质上，这是一个'如何让提示优化过程本身变得结构化和可治理'的问题。SEPO 试图把优化过程从'黑盒重写'转变为'白盒编辑轨迹'，并利用编辑与效应之间的因果关系作为反馈信号来指导后续搜索。

Q2: 有哪些相关研究？

根据现有证据，论文的相关工作讨论并不完整，但可以合理推断其涉及以下领域：

1. **自动提示优化（Automatic Prompt Optimization, APO）**：如 APE、ProTeGi、OPRO 等，这些方法通过大语言模型迭代改进提示，但多采用整体重写策略，缺乏结构化编辑。
2. **可解释提示优化**：一些工作声称具备可解释性，但实际只提供事后分析或整段差异记录，无法做到局部可归因。
3. **结构化提示表示**：使用模式、模板或类型化单元表示提示，使编辑操作可以在语义单元级别进行，而不是在 token 级别。
4. **多层提示模式（two-layer prompt schema）**：这可能包含指令层和示例层（或全局层与局部层），每层有稳定的类型化单元（如规则、约束、示例等），这类似于一些基于组件的提示工程方法。
5. **多轨迹搜索（multi-trajectory search）**：在提示优化中并行探索多条不同的编辑路径，相比单轨迹贪心搜索能获得更好的探索-利用权衡。
6. **基于谱系的反馈（lineage-based feedback）**：将编辑历史作为上下文传递给后续优化器，类似进化算法中的祖先信息或强化学习中的轨迹回溯。

由于缺少完整的 Related Work 章节，上述只能作为合理推断。论文明确指出其区别于现有解释性的是'仅事后可检查'，表明相关研究包括了那些声称可解释但实际只提供事后检查的方法。

Q3: 论文如何解决这个问题？

SEPO 的核心方法是'结构化、证据定位的提示优化'，具体机制包括：

1. **两层级提示模式（two-layer prompt schema）**：将提示组织成两层结构，每层包含稳定的、类型化的单元（typed units）。这种结构化表示允许对提示的局部单元进行编辑，而不是整体重写。

2. **局部编辑（local edit）**：每次迭代只应用一个局部编辑到结构化提示中，而不是生成全新提示。编辑操作具有'目标操作'（intended operation）和'实现操作'（realised operation）两个层面，即模型打算做什么与实际做了什么。

3. **编辑-效应谱系反馈（edit-effect lineage feedback）**：记录每个编辑连同其样本级别的结果（新修复或新破坏的样本），形成编辑-效应记录。这一记录会随着搜索分支传递，指导后续对同一分支的架构调用（architect calls）。这样，优化器可以知道哪些编辑有效、哪些无效，并据此决定下一步编辑。

4. **多轨迹搜索（multi-trajectory search）**：SEPO 同时维护多条搜索轨迹，每个轨迹有自己的编辑历史，允许探索不同的结构化提示变体，避免局部最优。

5. **可寻址、可归因、可操作**：由于编辑是针对类型化单元的，每个编辑可被精准定位（可寻址）；由于编辑与效应绑定，可归因于具体样本变化（可归因）；由于编辑记录是机器可读的，后续优化器可直接利用（可操作）。

具体实现层面，缺乏更多细节，但可以推测架构调用是由一个 LLM 根据当前提示、历史编辑-效应记录以及目标函数来生成下一个局部编辑。优化成本低于 GEPA，可能是因为局部编辑比整段重写更节省 token。

Q4: 论文做了哪些实验？

根据检索到的片段，实验部分设置了四个实验问题（Q1-Q4），其中 Q1 已明确为：在匹配的 API-only 优化和部署协议下，SEPO 是否比强提示优化器产生更强的最终提示？其他问题未在证据中具体列出，但根据摘要和常见实验设计，可合理推断包括：

- Q1: 保留集有效性（held-out effectiveness）——与强 baseline 比较最终提示在保留任务上的准确率。
- Q2: 可能涉及优化效率（optimisation-time cost，token 消耗、时间等）。
- Q3: 可能涉及提示简洁性/长度（prompt length）以及测试时间成本。
- Q4: 可能涉及编辑谱系的可解释性或消融研究（如去掉 edit-effect lineage 是否性能下降）。

具体实验设置如下（结合已知信息）：

- **任务套件**：14 个保留任务（held-out suite），覆盖多种 NLP 任务类型。
- **模型**：Llama-3.1-8B-Instruct 和 Qwen3-8B 作为被优化模型的部署模型（也可能作为优化器）。
- **基线**：最强基线为 GEPA，可能还有其他基线，但未列出。
- **指标**：宏平均准确率（macro accuracy）。
- **优化协议**：API-only，即只能通过 API 访问模型输出，不能访问内部梯度或权重。
- **帕累托分析**：同时考虑优化时间和测试时间，考察 SEPO 是否位于帕累托前沿。

实验比较了 SEPO 与 GEPA 的优化 token 消耗（290 万 vs 410 万）和生成提示长度（SEPO 提示短 5 倍以上）。

Q5: 发现了什么实验现象？

根据摘要和片段，主要实验观测包括：

1. **性能提升**：SEPO 在 14 个保留任务上，对 Llama-3.1-8B-Instruct 比 GEPA 高 3.1 个百分点（61.9% vs 58.8% 左右，推算），对 Qwen3-8B 高 2.2 个百分点（73.3% vs 71.1% 左右，推算）。这显示出结构化编辑+谱系反馈比整体重写更有效。

2. **优化成本更低**：SEPO 优化耗用 290 万 token，而 GEPA 需要 410 万 token，节约约 29% 的优化 token。这可能是因为局部编辑平均每次修改更少内容，且谱系反馈减少了无效重写。

3. **提示更短**：SEPO 生成的提示比 GEPA 短 5 倍以上，说明它避免了规则堆积（rule accretion）导致的提示膨胀，保持了提示的简洁性。这也可能带来测试时间的节省，从而处于测试时间帕累托前沿。

4. **双帕累托前沿**：SEPO 同时在优化时间和测试时间两个维度上位于帕累托前沿，表明它没有以牺牲测试性能为代价来节省优化成本，也没有以过长的提示来换取性能。

5. **可能的权衡或反直觉点**：虽然 SEPO 的提示更短但准确率更高，这反直觉地说明'更长的提示未必更好'，并且结构化的局部编辑可以更精准地保留有效信息，消除冗余。

由于缺少详细实验表格，无法提供每个任务的细粒度结果、消融实验的具体数字以及失败案例。以上观测均基于摘要和片段，部分数值为合理推断而非原文直接给出。

Q6: 有什么可以进一步探索的点？

根据 Limitations 片段和论文逻辑，进一步探索方向包括：

1. **指令空间可扩展性**：SEPO 的效率优势是否在显著更长的提示或更大规模的指令空间中依然成立？需要测试数万甚至数十万 token 的提示场景。
2. **组合式智能体技能**：论文提到'涉及多步骤、工具调用或状态跟踪的更具组合性的智能体技能'是未来工作的一部分。SEPO 目前可能只适用于单轮文本任务，能否扩展到多轮工具调用场景需要研究。
3. **跨任务迁移**：SEPO 的编辑记录可能包含可迁移的通用编辑策略，但目前未做跨任务迁移，未来可以探索将从一组任务中学到的编辑模式迁移到新任务。
4. **更丰富的编辑类型**：当前编辑可能限于指令改写、示例增删等，未来可以探索更复杂的编辑如重排、抽象化、结构化重组等。
5. **谱系反馈的长期效应**：编辑-效应记录在长轨迹中如何影响探索-利用平衡？是否可以主动遗忘无效历史或对历史做摘要？
6. **结合其他优化信号**：如利用数据集特征、分布外样本、错误模式分析等作为谱系反馈的补充。
7. **多模型泛化**：SEPO 的优化策略是否对不同规模的模型（如 70B 级）或不同家族（如 GPT、Mistral）同样有效？
8. **生产环境应用**：在持续集成/持续部署场景中，SEPO 能否支持提示的版本控制和回滚？

Q7: 总结一下论文的主要内容

SEPO 论文的核心论点是：现有 API-only 提示优化器虽然宣称可解释，但实际只提供事后可检查性——每次迭代把提示当作一个不透明字符串整体重写，留下的只有整段提示的差异记录，缺乏可定位、机器可读的局部编辑信息。这种实践导致优化过程难以归因、容易产生规则堆积和提示膨胀，有效改进也难以在迭代中保持。

为了解决这个问题，论文提出 SEPO（Structural, Evidence-grounded Prompt Optimization），一种多轨迹提示优化器，核心机制是'编辑-效应谱系反馈'。SEPO 不是重写整个提示，而是在两层级提示模式中局部编辑稳定的、类型化的单元。每次编辑都被记录，并与它新修复或破坏的示例实例关联起来，形成编辑-效应记录。这个记录沿同一搜索分支传递给后续的架构调用（architect calls），让优化器能基于证据决定下一步操作。这使得优化过程具有三个属性：可寻址（editable units are addressable）、可归因（edits are linked to effects）、可操作（machine-readable records guide future edits）。

方法上，SEPO 维护多条搜索轨迹，每个轨迹有自己的编辑历史，从而在保持局部性的同时进行全局探索。两层级提示模式提供了结构化的编辑空间，而类型化单元保证了编辑的语义完整性。优化器根据历史谱系决定编辑的目标位置和操作类型，然后由 LLM 执行具体编辑。

实验方面，SEPO 在 14 个任务的保留套件上进行评估，使用 Llama-3.1-8B-Instruct 和 Qwen3-8B 作为部署模型，与最强基线 GEPA 比较。结果发现 SEPO 在两个模型上分别比 GEPA 提高 3.1 和 2.2 个百分点，达到 61.9% 和 73.3% 的宏平均准确率。同时，SEPO 优化只消耗 290 万 token，而 GEPA 需要 410 万；生成的提示比 GEPA 短 5 倍以上。这些结果说明 SEPO 同时在优化效率和提示简洁性上具有优势，且没有牺牲最终性能，因此位于优化时间和测试时间两个帕累托前沿。

论文还讨论了局限性，包括指令空间的可扩展性（是否适用于更长提示）和更复杂的智能体技能（多步骤、工具调用、状态跟踪）尚未验证，跨任务迁移留待未来工作。整体上，SEPO 展示了结构化编辑+谱系反馈可以提升提示优化的有效性、效率和可解释性，为自动提示优化提供了一种新的范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向的关联：SEPO 的结构化提示编辑和谱系反馈可类比于智能体的技能编辑和反思机制，值得关注其如何将编辑历史用于后续决策，但论文尚未验证在工具调用、多步状态等更复杂场景中的适用性。

## 基本信息

- 作者：Xiaoyu Ma, Haoyue Liu, Yiwen Li, Jionghao Zhu, Zhichao Wang, Ye Chen, Xiaoying Tang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.28067`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、method、experiments、limitations 等片段），并结合摘要信息进行合理推断和补全，部分具体数值和实验细节未在检索证据中直接出现，已在相应字段标注。
