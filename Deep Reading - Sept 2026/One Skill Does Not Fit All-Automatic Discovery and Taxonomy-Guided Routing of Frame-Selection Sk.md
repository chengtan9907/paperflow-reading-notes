---
user_id: "cheng tan"
paper_id: 11404
arxiv_id: "2609.12517"
title: "One Skill Does Not Fit All: Automatic Discovery and Taxonomy-Guided Routing of Frame-Selection Skills for Long-Video Question Answering"
publish_date: "2026-09-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.12517.pdf"
pdf_url: "https://arxiv.org/pdf/2609.12517"
abs_url: "https://arxiv.org/abs/2609.12517"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-14T10:22:36"
---
# One Skill Does Not Fit All: Automatic Discovery and Taxonomy-Guided Routing of Frame-Selection Skills for Long-Video Question Answering

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-video question answering · frame selection · video mllm · skill discovery

## 一句话总结

AutoSkill 提出一个源监督、免训练的框架：用 LLM agent 在少量标注源池上通过 propose–implement–evaluate–refine 循环自动发现可执行的帧选择技能（frame-selection skills），再仅凭目标 benchmark 的无标注问题与选项文本诱导共享语义分类法、估计类别到技能的映射，从而为每道题路由单个技能、一次性选帧并送入冻结视频 MLLM 推理；在五个长视频 QA benchmark split 上分别提升 Qwen2.5-VL-7B 与 Qwen3.5-4B 2.4% 与 1.2%。

## 摘要

> Long-Video Question Answering (LVQA) requires locating decisive evidence in hour-scale videos under a limited frame budget. Most training-free methods apply the same frame-selection strategy to all questions, despite substantial variation in the evidence required by different question types. Our analysis shows that the relative effectiveness of frame-selection strategies varies across semantic categories and benchmarks, motivating adaptive evidence acquisition. In this paper, we introduce AutoSkill, a source-supervised framework for automatically discovering and routing executable frame-selection skills. Starting from a small labelled source pool, LLM agents iteratively propose, implement, evaluate, and refine candidate skills. For a target benchmark, AutoSkill uses only unlabelled question and option text to induce a shared semantic taxonomy, rewrite labelled source examples into the target style, and estimate a category-to-skill mapping. Neither target videos nor target answers are used in this process. At inference time, each question is assigned one skill, which selects the frames used in a single inference of the frozen video MLLM. Across five long-video benchmark splits, AutoSkill improves Qwen2.5-VL-7B and Qwen3.5-4B by 2.4% and 1.2%, respectively, demonstrating the effectiveness of our AutoSkill.

Q1: 这篇论文试图解决什么问题？

1. 任务层的核心矛盾：LVQA 要求在时长可达数十分钟乃至小时级的视频中定位「决定性证据」，但视频帧穷举式输入会让视觉 token 迅速超出当前 MLLM 的上下文容量。因此问题不是「如何看懂视频」，而是「在固定视觉 token 预算下，把预算分配给哪些帧」——这是一个证据获取（evidence acquisition）策略问题，而非单纯预处理。

2. 现有免训练方法的统一假设及其失效点：多数免训练路线对所有问题施加同一个帧选择策略（如均匀采样、关键帧检测、相似度检索等的某个固定实现）。论文在 Introduction 中明确给出其经验前提：不同问题所需的证据可能「短暂出现」「跨远距离片段重复」或「依赖事件的整体演化」。这三类证据形态对「采样密度、时间跨度覆盖、是否跨段聚合」的要求彼此冲突，因此单一固定策略必然在某些类别上系统性欠采样。

3. 实证观察：论文的分析表明，帧选择策略的相对有效性会随语义类别和 benchmark 而变化。注意这里的措辞是「相对有效性变化」，即不同策略之间存在互补性（skill complementarity），而不是某个策略全局占优。这是全文的问题起点，也是后续「工具箱 + 路由」设计逻辑的必要前提。

4. 二阶部署难题（论文明确点出的实践挑战）：即便知道「存在一个最强固定技能」，要识别它也需要标注；而在真实目标域上通常没有答案标注、也不应接触测试视频。于是问题从「选哪个固定策略」升级为「如何在只有无标注问题/选项文本的目标域上，把技能与问题类别对齐」。论文的解法是把「技能发现」与「技能分配」解耦：技能在源池上发现，分配在目标域文本上估计。

5. 三阶问题——技能从何而来：如果技能由人工设计，则受限于研究者的先验，难以覆盖跨 benchmark 的多样性。论文因此引入 LLM agent 的自动技能发现：从共享标注源池出发，迭代 propose、implement、evaluate、refine 候选技能，产出的是一个「紧凑（compact）工具箱」而非单个最优技能。

6. 关键约束与隐含假设（供批判性阅读）：
 - 假设帧选择可与答题解耦，是一个推理前的一次性决策；论文刻意把选帧与「冻结视频 MLLM 的单次推理」绑定，避免多轮重选——这既是效率优势，也是能力上界（一旦技能漏掉证据，模型无法回看）。
 - 假设技能是「可执行」的，即能被程序化实现并在给定视频/问题上运行，这暗示技能是代码级或工具级对象，而非自然语言启发式（具体表征形式需回原文确认）。
 - 假设源域与目标域之间存在可对齐的共享语义分类法，且用 LLM 重写 query 风格即可完成域适配；这是一个较强的迁移假设，域偏移过大时可能失效。
 - 假设「不接触目标视频与目标答案」足以避免泄漏，但目标 benchmark 的问题与选项文本本身仍被用于诱导分类法与估计映射，因此论文的「无监督」严格来说是「无标签、无视频」，而非「零目标信息」。

7. 证据缺口：现有检索片段未给出定量分析的具体形式（策略数量、类别定义、相关性度量、涉及哪些 benchmark）、也未给出源池规模与技能数量，这些是判断「问题设定是否被充分证实」的关键，需要回原文核对。

Q2: 有哪些相关研究？

1. 高效长视频理解（Efficient Long-Video Understanding）——论文在 Related Work 中将此作为第一大脉络：长视频理解受限于时间冗余与有限视觉 token。该脉络下的训练式方法通过长上下文适配（long-context adaptation）、token 压缩（token compression）、层级化（hierarchical）表示等途径解决。这类方法效果明确，但需要训练/微调，成本高且与冻结模型设定不兼容；AutoSkill 明确把自己定位为免训练一侧。

2. 推理前的视觉输入缩减 / 帧选择：免训练路线在推理前减少视觉输入。论文的论点是在此路线内部，多数工作仍使用统一策略，未按问题类型自适应。需要注意：检索证据只覆盖到段落级别的概括，未列出该路线的具体代表工作名与对比设置，若要精确判断 AutoSkill 相对哪些具体方法有增益，需要回原文的 Related Work 与实验表核对。

3. LLM agent 的技能自动发现（automatic skill discovery）：证据片段明确写道「AutoSkill brings automatic skill discovery into long-video frame selection」，并且「An LLM agent discovers frame-selection skills on a capability-focused development set. AutoSkill then distills their execution history into a …」。这提示存在一个 agent 侧的前置文献脉络（agent 自动创建/发现可执行技能、把执行历史蒸馏为可复用策略），而 AutoSkill 的贡献是把这一范式搬到长视频帧选择上。片段中出现的「capability-focused development set」与「execution history 蒸馏」暗示其设计受过 agent 技能库/工具库研究的直接影响；具体引用文献在现有证据中不可见。

4. 路由与专家选择（routing / mixture-of-experts 式思路）：论文的核心机制是 category-to-skill mapping 的「路由」。这与「按输入选择专家/工具」的研究范式同源，但 AutoSkill 的独特之处是：路由键不是隐层表示，而是由 LLM 从无标注问题文本诱导出的语义类别；路由表不是端到端训练出来的，而是由源池重写后的标注样例估计出来的。这一「离散、可解释、免训练」的路由设计与基于门控网络的路由形成明显范式对照。

5. 长视频问答 benchmark 生态：论文在五个长视频 benchmark split 上评测，说明其关注跨 benchmark 的泛化而非单点刷榜；但证据未给出具体 benchmark 名称与 split 构造方式，无法判断其覆盖了哪些证据形态（如教学、电影、自我中心录像在 Introduction 中被提到作为现实场景来源，但未确认它们是否对应评测集）。

6. 论文的相对定位（可复核的表述）：AutoSkill = 免训练 + 帧选择 + 技能自动发现 + 分类法引导路由 + 冻结 MLLM 单次推理 + 无目标视频/答案的域适配。相较训练式方法，它不更新权重；相较固定策略的免训练方法，它引入问题级自适应；相较人工设计技能库，它自动化技能发现；相较端到端路由学习，它把路由建立在一个由源池监督、可在目标域文本上复用的语义分类法之上。

7. 证据限制说明：本字段主要依据 Introduction / Related Work 的少量语义命中片段，无法列举论文引用的具体工作名与其差异论述。若需要厘清「哪些工作是它真正超越的对象」「哪些是同期工作」，建议直接核对 Related Work 原文与实验 baseline 列表。

Q3: 论文如何解决这个问题？

1. 总体架构：AutoSkill 由三个阶段组成（论文 Fig. 2 对应），加上一个推理阶段。核心思想是把「通用帧选择器」替换为「自动发现的技能工具箱 + 分类法引导的路由」。冻结的视频 MLLM 始终不更新权重。

2. Stage 1——技能自动发现（source-side skill discovery）：
 - 输入：一个共享的、带标注的源池（small labelled source pool）。
 - 过程：LLM agent 迭代地 propose（提出候选技能）→ implement（实现为可执行技能）→ evaluate（在源池上评估）→ refine（根据执行反馈精炼）。证据片段明确提到「LLM-agent-driven exploration and execution feedback」，说明反馈信号来自技能实际执行的后果，而非离线打分。
 - 输出：一个紧凑的工具箱（compact toolbox）而非单一最优技能；目标是让技能之间互补，覆盖不同证据获取需求。
 - 关键的未确认细节：技能的具体表征（Python 函数？可调用工具？参数化采样器？）、候选池规模、评估指标、精炼的收敛/停止准则，均需回原文确认。

3. Stage 2——目标域适配与分类法诱导（taxonomy induction + style rewriting）：
 - 只使用目标 benchmark 的无标注问题（question）与选项（option）文本，不使用目标视频、不使用目标答案。
 - 先用这些文本诱导一个共享语义分类法（shared semantic taxonomy），即把目标问题的语义空间划分为若干类别；该分类法被称为「共享」，意味着源域与目标域共用同一套类别标签。
 - 同时把标注源样例「重写」为目标 benchmark 的查询风格（rewrite labelled source examples into the target style），但保留原有的视频 grounding 与答案标签。这一步的目的是缓解源/目标 query 分布差异造成的类别统计偏移，同时确保监督信号（该类别下哪个技能更好）仍然有据可依。

4. Stage 3——类别到技能的映射估计（category-to-skill mapping）：
 - 在重写后的源样例上运行各技能，估计「类别 → 技能效用」的映射。
 - 证据片段提到 AutoSkill 会把技能的「执行历史（execution history）」蒸馏成某种可复用形式（该片段被截断，蒸馏产物可能是路由表、启发式决策规则或技能说明书），这一蒸馏步骤是 Stage 3 的核心机制之一，但具体形式需回原文确认。

5. 推理阶段：
 - 对每道目标问题，先归类到分类法中的某个类别，再查表分配一个技能。
 - 该技能在视频上选择帧，选出的帧只进入冻结视频 MLLM 的一次推理（single inference），不进行多轮重选、不做迭代式证据补充。
 - 这带来两个直接特性：推理成本与「一次选帧 + 一次前向」基本同构；系统瓶颈被显式放在选帧策略上。

6. 与固定策略基线的机制性差异：固定基线为所有问题求解同一个「平均最优」策略，因此在不同类别的证据需求之间做隐式折中；AutoSkill 允许按类别切换专门的技能，从而把「类别内最优」替换掉「全局平均最优」。论文结论中的表述是「exploits skill complementarity to outperform fixed-skill baselines」，即增益来源被归因于技能互补性而非单个技能更强。

7. 设计权衡（批判性视角）：
 - 硬路由 vs 软路由：每题只分配一个技能，实现简单、可解释、成本低，但路由错误没有回退机制；证据中未见置信度或候选集机制。
 - 无目标监督 vs 域偏移：不接触目标视频与答案降低泄漏风险和标注成本，但也意味着适配质量完全取决于「重写后的源样例」与「由目标文本诱导的分类法」的保真度。
 - 一次性选帧 vs 可迭代检索：效率高，但把召回失败变成不可恢复错误。
 - 免训练 vs 上界：不更新 MLLM 与选择器权重，部署友好，但也放弃了任务特定的表征适配空间。

8. 证据缺口提示：三阶段之间的耦合方式（分类法是否跨阶段共享同一份 prompt/标签体系）、技能数量与工具箱压缩准则、以及「execution history 蒸馏」的全部内容，是复现该方法时最需要回原文核对的三处。

Q4: 论文做了哪些实验？

1. 评测规模与骨干：论文在五个长视频 benchmark split 上进行评测，使用两个冻结的视频 MLLM 作为答题器——Qwen2.5-VL-7B 与 Qwen3.5-4B（按摘要原文表述）。结果分别为 +2.4% 与 +1.2% 的提升。

2. 对比对象：从结论「outperform fixed-skill baselines」可知，主要对照是使用固定（通用）帧选择策略的免训练基线，即「对所有问题用同一策略」这一族方法。这直接对应论文的问题动机，因此实验设计的主要作用是验证「自适应路由优于固定策略」。

3. 评测目标域设置的关键性质：目标 benchmark 的视频与答案在整个适配过程中都不被使用；用于诱导分类法与估计映射的只有无标注的问题文本与选项文本。因此实验同时检验了「源监督技能能否迁移到目标域」这一命题，而不只是「技能本身是否有效」。

4. 可能存在的分析/消融（以下为合理推断，需回原文核对）：
 - 有无路由的消融（单一最强技能 vs 分类法路由），用于支撑「技能互补性」这一归因。
 - 技能数量/工具箱规模的敏感性分析。
 - 分类法粒度（类别数）的影响。
 - query 风格重写是否必要的消融。
 - 每类问题的技能选择分布与类别级增益分解。
 - 跨 benchmark split 的一致性（五组结果是否同向）。
 - 帧预算的敏感性。

5. 证据缺口（重要）：
 - 五个 split 的具体 benchmark 名称、视频时长分布、问题类型构成均未出现在检索片段中；
 - 帧预算的具体数值、是否固定、是否与基线对齐，未给出；
 - 提升的绝对数值含义（是 accuracy 还是其他指标、基线绝对值多少）未给出，因此无法判断 2.4% 是相对提升还是绝对提升——从常见表述习惯推测为绝对百分点，但需回原文确认；
 - 是否包含与其他免训练帧选择方法的横向比较、是否包含 oracle 上界（如「每类选最优技能」）也未在证据中出现；
 - 未见到统计显著性或多次运行方差的信息。

6. 复现相关信息缺口：源池规模、技能实现语言/接口、LLM agent 使用的模型、推理时分类的调用成本（是否每题一次 LLM 调用）均需从原文补充，这些直接影响该方法的实际部署成本判断。

Q5: 发现了什么实验现象？

1. 技能互补性是可观测的主要现象：结论明确把增益归因于「利用技能互补性超越固定技能基线」。这意味着实验中应能观察到「不同技能在不同类别上各有所长」的模式；如果某个技能全局占优，则路由带来的增益应消失，因此该现象是方法成立的必要条件。

2. 策略的相对有效性随语义类别与 benchmark 变化：这是论文动机层面的实证发现，也构成实验层面的核心观察——同一技能在不同类别/split 上排序会变化。其反直觉之处在于：帧选择长期被视为通用预处理，而该观察说明它更像一个需要按问题类型实例化的策略族。

3. 提升幅度随骨干不同而不同：Qwen2.5-VL-7B 上 +2.4%，Qwen3.5-4B 上 +1.2%。一个合理的解读（合理推断）是：更强/更大的骨干本身对帧缺失更鲁棒，或能从未对齐的帧中恢复更多信息，因此选帧策略的边际收益下降。但论文证据未给出机制解释，也未排除采样波动或两类模型对视觉 token 预算敏感性差异等替代解释。

4. 目标域无视频/无答案的适配是有效的：这是实验中最具方法论意义的观察——在没有任何目标监督的情况下，通过「源样例风格重写 + 目标文本诱导分类法」就能获得正向迁移。这支持了「证据需求可按问题语义类型预测」这一假设，即问题的语义线索（文本层面）与所需帧选择策略之间存在可学习的对应关系。

5. 评测覆盖的张力点：论文强调跨五个 split 一致提升，但未在可见证据中给出逐 split 的细分结果；若某些 split 增益接近 0 或为负，会直接影响「跨 benchmark 稳健性」这一主张的强度。这属于需回原文核实的潜在张力。

6. 负结果与失败模式在可见证据中缺失：没有关于「路由错误后模型答错」的案例分析、没有技能失效类别的清单、也没有对分类法错分的敏感性报告。从方法结构可推测两类失败模式（推测）：一是分类法边界模糊导致的系统性错路由；二是目标域与源域的 query 风格重写失真，使类别统计不再反映目标分布。

7. 效率面的观察缺口：论文把方案定位为免训练、单次推理，但技能发现阶段的 LLM agent 迭代成本、推理时的分类开销、以及相对固定策略的额外时延，均未出现在证据中；这些是判断「免训练是否等于低成本」的关键张力点。

8. 上界信息缺口：没有出现「oracle 路由」或「每类最佳技能」的上界结果，因此无法判断当前 2.4%/1.2% 距离「路由完全正确时的潜力」还有多远，也无法区分增益瓶颈来自技能质量还是路由质量。

Q6: 有什么可以进一步探索的点？

1. 路由从硬分配走向软分配与不确定性校准：当前每题只选一个技能，建议引入类别置信度、技能效用的概率化估计或 top-k 候选融合，并评估路由错误率与最终 QA 指标之间的相关性。这是把「路由正确率」从隐含中间量变成显式可诊断量的最直接路径。

2. 技能的组合与分层：既然技能互补性已被观察到，多技能协同（按时间片/证据类型分段调用不同技能）可能优于单技能独占预算；也可探索技能的参数化扩展（同一技能族在不同帧预算下实例化）以形成「技能 × 预算」的二维策略空间。

3. 让选帧不再是一次性决策：与迭代式检索/多轮观察结合（先粗筛、再针对候选时段加密采样），并研究在单次推理约束下如何用交互式预算换取证据召回。这直接针对「漏掉证据不可恢复」这一结构性问题。

4. 分类法质量与稳定性研究：共享语义分类法由 LLM 从目标文本诱导，其可复现性、类别数敏感性、跨 benchmark 可迁移性都值得系统评测；可探索分类法是否为最优粒度，或引入层级分类法与类内细分。

5. 降低与替代源监督依赖：当前需要一个小规模标注源池。可研究少样本/主动学习式的源池构造、跨源池迁移、以及用合成或模型生成监督代替人工标注，衡量性能随源池规模与域相似度的缩放曲线。

6. 域偏移的鲁棒性边界：系统刻画「目标域文本与源域分布差异」与增益之间的关系，包括风格重写是否充分、何时适配失败，并给出适配可行性的判据（例如是否可预测目标域上的增益为正）。

7. 把发现好的技能用于训练：把自动发现的技能作为监督信号蒸馏回轻量选择器（如小型网络或 LoRA），在保持推理期无需 LLM 调用的同时保留适应性，从而在「免训练」与「可学习」之间寻找成本-性能折中点。

8. 成本-收益的完整核算：报告 agent 发现阶段的 token/时间成本、推理期分类开销、以及相对基线的端到端时延，给出「每提升 1% 指标需要多少额外成本」的量化口径。

9. 扩展到帧选择之外：该方法论（源监督技能发现 + 无目标文本分类法路由）原则上可迁移到长文档 QA、视频检索、多模态 RAG、乃至 agent 工具路由；验证其在非视频模态上的普适性会显著提升该框架的解释价值。

10. 评测协议扩展：引入对抗性问题（证据只出现一帧、跨长距离重复、依赖全局演化）构造的分类诊断集，并发布路由决策与失败案例，使「自适应证据获取」这一主张可被独立复核。

11. 与其他模态证据源协同：长视频中音频、字幕、ASR 往往是低成本且高召回的线索，可作为选帧的先验；研究多模态线索参与路由是否能把视觉预算进一步释放。

Q7: 总结一下论文的主要内容

1. 论文要解决的问题：长视频问答（LVQA）需要在小时级视频上、在有限视觉 token 预算内定位决定性证据。穷举帧不可行，因此帧选择实际上是推理管线中的「证据获取策略」。现有免训练方法普遍对所有问题使用同一个帧选择策略，但论文的分析显示，帧选择策略的相对有效性会随语义类别（semantic category）与 benchmark 变化：不同问题所需证据可能只是短暂出现、可能跨远距离片段反复出现、也可能依赖事件的整体演化，这三类需求对采样密度与时间覆盖的要求互相冲突。因此「一个技能适配所有问题」的假设不成立，需要按问题自适应的证据获取。

2. 二阶挑战：即便承认需要自适应，实践上也有约束——最强固定技能只能靠标注识别，而目标 benchmark 上通常既没有答案标注，也不应使用目标视频。因此论文的目标是：在不接触目标视频与目标答案、不更新任何模型权重的条件下，为每道目标问题选择一个合适的帧选择策略。

3. 方法主线（AutoSkill，三阶段 + 推理）：
 - Stage 1，自动技能发现：从一个小的带标注源池出发，LLM agent 迭代地提出、实现、评估、精炼可执行的帧选择技能，最终形成一个紧凑的、彼此互补的技能工具箱。反馈来自技能实际执行的结果，而非离线先验。
 - Stage 2，目标域适配：仅使用目标 benchmark 的无标注问题与选项文本，诱导一个源域与目标域共享的语义分类法；同时把标注源样例重写成目标 benchmark 的查询风格，保留原有的视频 grounding 与答案标签。这一阶段完全不使用目标视频与目标答案。
 - Stage 3，类别到技能的映射：在重写后的源样例上估计「语义类别 → 技能效用」的映射，并把技能的执行历史蒸馏为可复用的路由依据。
 - 推理：每道题先归类，再分配一个技能；该技能选出帧，帧只进入冻结视频 MLLM 的一次推理，不做多轮重选。

4. 实验主线：在五个长视频 benchmark split 上评测，答题器为冻结的 Qwen2.5-VL-7B 与 Qwen3.5-4B，对比对象为固定技能的免训练基线。结果分别提升 2.4% 与 1.2%。论文把增益归因于「技能互补性」的被利用，而非单个技能的普遍优势；同时强调适配过程不使用目标答案与目标视频，说明源监督技能可以跨域迁移。

5. 论证结构小结：经验观察（策略有效性随类别/benchmark 变化）→ 动机（需要自适应证据获取）→ 部署约束（无目标标注、无目标视频）→ 设计（源侧自动发现 + 目标文本侧路由）→ 结果（两骨干、五 split 一致提升）→ 结论（自动发现技能工具箱 + 分类法路由，是长视频 QA 的有效免训练方案）。

6. 贡献定位：论文把「agent 自动化技能发现」这一范式引入长视频帧选择，并与「分类法引导的、无目标监督的路由」结合，形成一个不改权重、单次推理可部署的系统。其价值主张不只是分数增益，而是「帧选择策略族可以自动生成并按问题类型自适应调用」这一方法论转变。

7. 需要谨慎对待的证据薄弱处：一是提升幅度的确切口径（绝对百分点还是相对提升）在可见证据中未明确；二是实验细节（benchmark 名称、帧预算、指标定义、是否含 oracle 上界与显著性检验）缺失；三是论文未在可见证据中给出显式 limitations 与失败案例分析，尤其是路由错分、分类法不稳定、域偏移过大时的行为；四是「无监督适配」严格来说是「无答案、无视频」，目标问题与选项文本仍被使用，其潜在信息泄漏边界值得在原文中进一步核实；五是技能表征、源池规模、agent 迭代成本与推理期额外开销等工程参数未在证据中出现，而这些直接决定方法的实际可用性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接相关：本文把 LLM agent 的技能发现能力用于构建可执行工具箱，是 agent-as-tool-maker 范式在视频理解任务上的具体落地。

## 基本信息

- 作者：Jian Hu, Zixu Cheng, Da Li, Wei Li, Ziquan Liu, Shaogang Gong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.12517`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索命中的 Abstract、Introduction、Related Work 与 Conclusion 片段（并以 field_evidence_map 对齐各字段证据），同时明确指出实验细节、技能表征与局限性等证据缺口，未编造未在证据中出现的数值或设置。
