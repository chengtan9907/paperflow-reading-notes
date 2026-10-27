---
user_id: "cheng tan"
paper_id: 18290
arxiv_id: "2610.32109"
title: "Context or Knowledge: How LLMs Attribute Their Own Answers"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.32109.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32109"
abs_url: "https://arxiv.org/abs/2610.32109"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-26T12:20:35"
---
# Context or Knowledge: How LLMs Attribute Their Own Answers

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：source attribution · kv cache intervention · context vs parametric knowledge · llm self-report

## 一句话总结

论文通过在答案生成后、来源追问前对 KV cache 做删除、移植和方向 steering，因果地表明 LLM 的「来源报告」主要依赖回答阶段写入 Q+A 缓存的键值状态，而不是文档缓存本身。

## 摘要

> 论文研究语言模型在回答已被生成之后，如何报告答案来自上下文文档还是自身参数知识。作者不把「来源报告」等同于「文档是否支持答案」，而是把它视为关于答案生产过程的元认知声明。方法核心是在答案生成完成之后、来源追问之前干预 transformer 的 key–value（KV）cache：改变或移除文档对应的缓存，移植问题与答案（Q+A）对应的缓存，以及沿拟合方向 steering Q+A 的 value 表示。实验使用四个 7B–32B 模型和一个受控事实任务，并扩展到部分 DynamicQA 条目。主要发现是：改变或移除文档 cache 从不改变来源报告；在问题与答案 token 相同的 run 之间移植 Q+A cache 会把报告转移给 donor，且 recipient 的文档保持不变；在两个较小模型上，沿预先拟合方向 steering Q+A values 可在四种文档条件下以固定强度逆转 98.9–100% 的报告，而 norm-matched 随机方向最多翻转 1.1%；复制 keys/values 沿拟合方向的分量几乎重现报告变化，固定这些分量几乎阻止变化；在答案生成期间施加同样的 value 方向也能改变答案由哪个来源提供，但效应更弱且依赖 prompt。作者强调，该工作不建立一般意义上的来源报告准确性，也不构成对该缓存状态的完整解释。

Q1: 这篇论文试图解决什么问题？

论文试图解决的核心问题是：当 LLM 已经给出一个答案，随后被追问「你是用了文档还是自己的知识」时，它内部依据什么产生这个来源报告？这个问题在 RAG、检索增强生成、工具调用和可信 AI 场景中很实际：同一答案可能同时被检索文档和参数知识支持；当文档更新或与模型知识冲突时，用户需要知道模型究竟依赖了哪一边。但现有归因研究大多评估「生成内容是否被上下文支持」，即 attribution evaluation，而不是模型对自身答案生产过程的报告。作者明确区分两类问题：文档是否支持答案，与模型如何得出「我用了文档/知识」这一声明。前者是文本支持关系，后者是关于计算过程的因果声明。论文的切入点是 KV cache：生成过程中，先前 token 留下的 keys/values 会被后续 token 注意；同一组问题 token 在不同文档条件下可能获得不同的缓存状态。作者提出，来源报告可能读取的是回答阶段写入的工作记忆，而非文档内容本身。为此，论文在答案已生成、来源问题尚未提出的时间窗口内对 KV cache 做干预，从而把「报告所依赖的状态」与「文档输入」分离。关键隐含假设包括：来源报告可以被离散化为 context/knowledge 二选一；受控事实任务中的 Q+A 缓存状态能够代表更一般的来源归因；移植 eligible run 时问题与答案 token 相同，因而可以排除答案文本差异；KV cache 干预不会引入与来源无关的全局破坏。论文也给出边界：不建立一般报告准确性或完整解释；steering 与投影测试主要限于 7–8B 模型；文档编辑与移植仅扩展到部分 DynamicQA 条目；来源问题措辞有限。反例和失败模式包括：文档 cache 改动完全不影响报告，说明报告不是简单读取文档；生成期 steering 效果较弱且依赖 prompt，说明该状态与答案生成过程的耦合不是单向的；模板任务上的效应接近 universal，而 DynamicQA 上为 97.2–100%，提示任务自然度会调节效应强度。

Q2: 有哪些相关研究？

论文的相关研究可以沿五条线组织。第一是归因评估（attribution evaluation）：已有工作评估生成内容是否被提供的上下文支持，典型如 Rashkin et al. 2023 一类对生成文本做支持性判断的研究。这类工作回答「答案有没有依据」，但不回答「模型自认为用了哪个来源」。论文明确把后者作为一个独立对象，避免把文本支持性误当成来源报告。第二是解释 faithfulness：Turpin et al. 2023、Lanham et al. 2023 等工作表明，模型给出的自然语言解释不一定直接暴露其真实计算过程；链式思考或口头说明可能是事后合理化。论文与此一致，但进一步用因果干预 statt 只做行为观察：如果报告真的读取某个内部状态，那么移植或操纵该状态应当改变报告。第三是 KV cache 与机制可解释性：transformer 在生成时把历史 token 的 keys/values 保存在缓存中，后续 token 通过 attention 读取。已有研究把 KV cache 视为工作记忆或动态状态，但较少把它作为「来源归因」的因果变量。论文的贡献在于把干预时间精确放在答案之后、来源问题之前，从而分离「答案计算」与「来源报告计算」。第四是 activation steering / representation engineering：通过对比条件拟合方向，在推理时加向量或做投影，已被用于控制语气、拒绝、事实性等。论文把这一范式用于 Q+A value 空间，并加入 norm-matched random direction、energy-matched subspace clamping 等对照，试图证明效应不是任意扰动。第五是 RAG 中的 context vs parametric knowledge 冲突：检索文档可能与模型参数知识一致、补充或矛盾；模型可能复制文档、忽略文档或混合两者。论文的受控任务用支持性文档与无关文档构造同一答案的不同来源条件，从而让「答案相同、来源不同」成为可干预的实验对象。与这些方向相比，论文的新意不是提出新的归因评分，而是给出一个因果框架：来源报告由回答阶段写入的 Q+A 缓存状态承载；文档缓存不是必要载体；Q+A 缓存中的特定方向分量接近充分且必要。论文的 Appendix A 声称更详细地回顾相关工作，但给定材料只提供了 Introduction 的概述，因此这里不展开未验证的具体文献清单。

Q3: 论文如何解决这个问题？

论文的解决方案是一个「受控事实任务 + 答案后 KV cache 干预 + 多级对照」的实验框架。首先构造对话：给模型一个文档、一个事实问题、模型生成答案，然后追问来源（文档还是自身知识）。关键设计是干预发生在答案生成之后、来源问题之前；此时答案已经固定，文档和所有对话 token 保持不变，只有缓存状态被编辑。这样可以把来源报告的变化归因于缓存干预，而不是答案文本或文档内容变化。第一类干预针对文档 cache：改变或移除文档对应的 keys/values，观察来源报告是否变化。第二类干预是 Q+A cache 移植：选择 eligible 的 run，要求问题与答案 token 相同，但文档条件不同（例如支持性文档 vs 无关文档），然后把 donor 的 Q+A 缓存移植到 recipient，recipient 的文档保持不变，看报告是否跟随 donor。移植还分解为完整 Q+A、仅 keys、仅 values、以及 question-only 等变体。第三类干预是方向 steering：在 Q+A value 空间中，用支持性文档与无关文档 run 的差异拟合方向；在固定强度下把 recipient 的 values 沿该方向推动，观察报告翻转率；对照包括 norm-matched random directions。第四类是必要性与充分性测试：复制 donor 的 keys/values 沿拟合方向的投影，看是否足以重现报告变化；把 recipient 的对应投影钳制住，看是否阻止报告变化；并用 energy-matched control subspace 做对照，排除「任意子空间扰动都有效」。第五类是在答案生成期间施加同样的 value 方向，检验该状态是否也影响答案由哪个来源提供，而不仅是事后的来源报告。实验覆盖四个 7B–32B 模型；steering 与投影测试使用两个较小模型（检索片段中出现 OLMo-3-7B 与 Qwen3-8B，其余模型名称在给定证据中未完整出现）；文档编辑和移植扩展到部分 DynamicQA 条目。评价指标主要是报告跟随 donor 的比例、报告翻转率、以及 read-back 联合事件：报告变为 knowledge 后模型是否仍能读取文档中的 false value。后者用于排除「报告改变只是因为文档信息被擦除」。整体上，论文用「删除文档 cache 无效、移植 Q+A cache 有效、方向分量接近充分必要」构成一条因果证据链。

Q4: 论文做了哪些实验？

论文的实验可以分成七组。第一组是文档 cache 编辑：在答案生成后改变或移除文档对应的 KV cache，检查来源报告是否改变。摘要给出的结论是「never changes the report」，即文档缓存不是报告的必要载体。该实验覆盖四个模型和多种文档条件。第二组是 Q+A cache 移植：在问题与答案 token 相同的 eligible run 之间，把 donor 的 Q+A 缓存移植到 recipient，文档保持不变。摘要称「every eligible report follows the donor」。检索片段还显示，joint Q+A transplants 在 16 个 model–direction cells 中使 97.2–100% 的报告跟随 donor；这被描述为「nearly complete, rather than universal as on the templated task」，说明模板任务上效应更接近全覆盖，而自然一些的 DynamicQA 上略弱。第三组是缓存成分消融：比较完整 Q+A、仅 keys、仅 values、question-only 等变体。片段指出 value-only transplants 在多数情况下能创建报告，而移除报告时 keys 与 values 一起更有效；question-only transplants 也能产生报告，但效果依赖模型和方向。第四组是方向 steering：用支持性文档与无关文档的 Q+A value 差异拟合方向，在来源问题之前以固定强度施加。两个较小模型上，沿拟合方向 steering 逆转 98.9–100% 的报告，覆盖四种文档条件；norm-matched 随机方向最多翻转 1.1%。第五组是必要性与充分性：复制 donor 的 keys/values 沿拟合方向的投影，报告变化几乎被重现；固定 recipient 的对应投影，报告变化几乎被阻止。检索片段中的表格残片显示，仅复制 donor K+V 投影时报告跟随率为 97.9%/100%，而钳制 recipient K+V 投影并加 energy-matched subspace 对照时为 1.1%/0% 与 100%/94.7%；这些数字的具体行列含义需要回原文核对，但方向与摘要一致：拟合方向分量接近充分且必要，而能量匹配的对照子空间不能阻止。第六组是生成期干预：在答案生成期间施加同样的 value 方向，观察答案由哪个来源提供。摘要称这也会改变来源，但更弱且依赖 prompt。第七组是 read-back 与扩展：read-back 测量联合事件——报告变为 knowledge 后，模型是否仍能从文档中读出 false value；部分 DynamicQA 条目用于检验文档编辑和移植的泛化。模型方面，摘要称四个 7B–32B 模型；检索片段明确出现 OLMo-3-7B 与 Qwen3-8B，其余模型名称在给定材料中不完整。数据集以受控事实任务为主，示例包括法国首都、加拿大首都等；DynamicQA 仅覆盖部分条目。评价以报告分类和翻转比例为主，随机对照取三次抽样的均值。

Q5: 发现了什么实验现象？

第一，最反直觉的现象是文档缓存与来源报告的解耦：改变甚至移除文档的 KV cache 完全不改变报告。这说明模型在回答「你用了文档还是知识」时，并不是在读取文档缓存中残留的内容标记；文档是否在上下文中、文档说了什么，与报告的产生在这条通路上是分离的。第二，Q+A 缓存移植具有近乎决定性的作用：在问题与答案 token 相同的 run 之间移植 Q+A cache，eligible 报告会跟随 donor，而 recipient 的文档保持不变。这意味着报告读取的是回答阶段写入 Q+A 的状态，而不是文档输入本身。第三，方向 steering 显示出很高的比例效应：在两个较小模型上，沿拟合的 Q+A value 方向以固定强度推动，可在四种文档条件下逆转 98.9–100% 的报告；norm-matched 随机方向最多翻转 1.1%。这不是「任意扰动都能改变报告」的弱效应。第四，必要性与充分性出现对称趋势：复制 donor 的 keys/values 沿拟合方向的投影，几乎重现报告变化；固定 recipient 的对应投影，几乎阻止报告变化。检索片段中的表格残片给出 97.9%/100% 与 1.1%/0% 一类数字，以及 energy-matched control subspace 仍保持 100%/94.7% 翻转；这支持「特定方向分量」而非「任意能量扰动」的解释，但具体数值需回原文核对。第五，缓存成分之间存在张力：value-only transplant 在多数情况下能创建报告，但移除报告时 keys 与 values 一起更有效；question-only transplant 也能产生报告，但依赖模型与方向。这说明来源信息不是单一成分的简单开关，keys 与 values 在不同方向上有分工。第六，生成期 steering 的效应更弱且依赖 prompt：同样的 value 方向在答案生成期间施加，会改变哪个来源提供答案，但不如答案后 steering 那样接近全覆盖。这提示该状态同时参与答案生成和来源报告，但两条通路的耦合强度不同；也提示「来源报告」不是纯粹事后附加的标签。第七，任务自然度调节效应：模板任务上 Q+A 移植接近 universal，而 DynamicQA 上为 97.2–100%，被描述为「nearly complete, rather than universal」。这说明受控模板可能放大效应，自然检索场景存在额外变异。第八，read-back 对照排除了简单遗忘：报告变为 knowledge 后，模型仍能读取文档中的 false value，说明报告改变不是因为文档信息被擦除，而是来源归因状态被改变。第九，负结果与边界：文档 cache 编辑无效是一个强负结果；生成期 steering 的弱效应是另一个边界；steering 与投影仅覆盖 7–8B 模型，不能直接外推到 13B–32B；来源问题措辞有限，可能影响报告行为。整体上，实验揭示的是一条「回答时写入、来源追问时读取」的因果通路，而不是「文档内容直接决定报告」的朴素图景。

Q6: 有什么可以进一步探索的点？

第一，扩展到自然 RAG 场景：多文档、长文档、噪声检索、冲突文档、文档更新、多轮对话，以及不同来源问题措辞。当前受控任务和有限 DynamicQA 条目可能高估效应强度；需要检验在真实检索分布下，Q+A 缓存是否仍是主要载体。第二，覆盖更大模型：steering 与投影测试目前只在 7–8B 模型上做，13B–32B 模型是否具有同样方向结构、是否出现更分布式或多头冗余的编码，是开放问题。第三，细粒度来源报告：当前主要是 context/knowledge 二选一；可以研究 mixed、uncertain、部分使用、逐句归因、多源归因等更丰富的报告空间，并检验同一缓存状态是否支持这些区分。第四，时间分辨率：在答案生成的哪个 token、哪一层、哪个 attention head 写入来源相关状态；答案后干预与生成期干预的差异提示存在多个时间窗口，需要逐层逐位置剖析。第五，机制解释：keys/values 沿拟合方向的分量究竟编码了什么？是文档存在性、答案来源置信度、还是回答计算的整体模式？需要与 probing、attention 分析、因果追踪结合，避免把「方向」黑箱化。第六，训练动态：这种来源报告机制是否由预训练、指令微调、RLHF 或 RAG 训练塑造；不同训练配方是否改变缓存状态的编码方式。第七，可信应用：能否用该状态做在线来源归因检测、RAG 可信度校准、冲突文档告警；但论文明确不建立一般报告准确性，因此应用前需要独立验证。第八，对抗与失败模式：对抗性文档、提示注入、来源问题诱导、长上下文位置偏差是否会让报告与真实来源脱钩；当前 read-back 只覆盖有限联合事件。第九，跨语言与跨任务：事实任务之外，数学、代码、科学文献问答中的来源报告是否共享同一 KV 机制。第十，与 agent 场景结合：工具调用、检索调用、记忆读取的归因可能同样依赖「动作后状态」，可以把该范式迁移到 agent 的 source monitoring。第十一，理论层面：把来源报告视为一种 source monitoring / metacognition，与认知科学中的来源记忆框架对话，可能帮助解释为什么文档缓存无效而回答缓存有效。

Q7: 总结一下论文的主要内容

论文研究一个看似简单但机制上未被回答的问题：当 LLM 已经给出答案，随后被问「你用了文档还是自己的知识」时，它凭什么给出这个来源报告？作者首先做一个关键概念区分：文档是否支持答案，不等于模型报告答案来自哪里。前者是文本支持性判断，后者是关于自身计算过程的因果声明。在 RAG 场景中，同一答案可能同时被检索文档和参数知识支持；当文档与模型知识冲突时，来源报告对用户判断可信度很重要。但已有归因评估主要看生成内容是否被上下文支持，解释 faithfulness 研究则表明模型的口头解释不一定反映真实计算。论文选择 KV cache 作为切入点：生成过程中历史 token 留下的 keys/values 构成后续 token 可读取的工作记忆；同一问题在不同文档条件下可能获得不同缓存状态。作者把干预精确放在答案生成之后、来源问题之前，从而在答案固定、文档固定的条件下操纵缓存，观察报告是否改变。实验使用四个 7B–32B 模型和受控事实任务，示例包括法国首都、加拿大首都等；文档条件包括支持性文档与无关文档，部分实验扩展到 DynamicQA 条目。第一组结果是强负结果：改变或移除文档对应的 KV cache 从不改变来源报告。这说明报告不是简单读取文档缓存中的内容标记。第二组结果是强正结果：在问题与答案 token 相同的 eligible run 之间移植 Q+A cache，报告会跟随 donor，而 recipient 的文档保持不变。检索片段显示 joint Q+A transplants 在 16 个 model–direction cells 中使 97.2–100% 的报告跟随 donor；模板任务上接近 universal，DynamicQA 上略弱。第三组结果把移植效应定位到 keys/values 沿拟合方向的分量：复制这些分量几乎重现报告变化，固定这些分量几乎阻止报告变化；value-only transplant 在多数情况下能创建报告，移除时 keys 与 values 一起更有效；question-only transplant 也能产生报告但依赖模型与方向。第四组是方向 steering：用支持性文档与无关文档的 Q+A value 差异拟合方向，在来源问题之前以固定强度施加；两个较小模型上逆转 98.9–100% 的报告，覆盖四种文档条件，而 norm-matched 随机方向最多翻转 1.1%。必要性与充分性对照显示，拟合方向分量接近充分且必要，energy-matched control subspace 不能阻止报告翻转。第五组是生成期干预：在答案生成期间施加同样的 value 方向，会改变答案由哪个来源提供，但效应更弱且依赖 prompt。这说明来源相关状态与答案生成过程耦合，但答案后报告读取的通路更可操纵。第六组是 read-back 对照：报告变为 knowledge 后，模型仍能读取文档中的 false value，说明报告改变不是因为文档信息被擦除。论文的论证主线是：如果报告读取文档 cache，删除文档 cache 应当改变报告，但没有；如果报告读取回答阶段写入的 Q+A 状态，移植该状态应当转移报告，确实转移；如果该状态中的特定方向分量承载来源信息，复制/固定这些分量应当充分/必要地改变报告，确实如此；如果该状态也参与答案生成，生成期 steering 应当影响答案来源，确实影响但更弱。由此作者得出结论：在该受控设定下，来源报告因果依赖回答时产生的缓存状态。论文同时明确限制：不建立一般报告准确性或完整状态解释；不覆盖自然检索的全部复杂性；steering 与投影主要限于 7–8B 模型；来源问题措辞有限；报告是离散二选一。整体上，这是一篇机制解释取向的工作，用因果干预而非仅行为评估来研究 LLM 的来源归因，并为 RAG 可信度、agent 来源监控和 KV cache 控制提供了可复用的实验范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向相关：论文研究 KV cache 中的状态如何被后续解码读取，并用 value 方向 steering 控制报告，属于生成过程控制与机制解释的交叉。

## 基本信息

- 作者：Kirill Afendulev, Alexey Dontsov, Alla Chepurova, Elena Tutubalina, Anton Korznikov
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-10-26
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.32109`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction 与 References 片段，并在数值和条件上优先采用检索证据；未能从给定材料确认的完整模型清单、机构信息和部分表格行列含义已标注为缺口。
