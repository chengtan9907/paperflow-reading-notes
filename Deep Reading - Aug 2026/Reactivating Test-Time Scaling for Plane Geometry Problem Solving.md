---
user_id: "cheng tan"
paper_id: 10072
arxiv_id: "2608.30156v1"
title: "Reactivating Test-Time Scaling for Plane Geometry Problem Solving"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30156v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30156v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:48:34"
---
# Reactivating Test-Time Scaling for Plane Geometry Problem Solving

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：test-time scaling · plane geometry problem solving · multi-trace synthesis · perception-augmented training

## 一句话总结

针对测试时扩展（TTS）在符号程序范式下难以有效扩展到平面几何问题求解的问题，本文提出 Multi-Trace Synthesis（MTS）将符号程序转换为异构推理轨迹、Perception-Augmented（PA）训练在符号演绎前将图形解析为结构化语义子句，以及 Consensus-Guided Multi-Trace Ensemble（CG-MTE）自适应推理，在三个几何基准上一致提升性能并降低采样成本。

## 摘要

> Plane geometry problem (PGP) solving has become a critical benchmark for multimodal reasoning because it requires accurate visual perception and precise multi-step symbolic deduction. Although test-time scaling (TTS) has demonstrated remarkable success in general mathematical reasoning, it fails to scale effectively under the symbolic-program paradigm for plane geometry. We identify two key obstacles: limited reasoning diversity induced by rigid symbolic programs and insufficient explicit visual grounding before symbolic deduction. To address these issues, we propose Multi-Trace Synthesis (MTS), which converts each symbolic program into heterogeneous reasoning traces, including executable Python scripts and CoT-augmented variants. We further propose Perception-Augmented (PA) training, which parses diagrams into structured semantic clauses before deduction, and Consensus-Guided Multi-Trace Ensemble (CG-MTE) for efficient self-adaptive inference. Experiments on three geometry benchmarks show that our method consistently improves PGP-solving across model scales and achieves strong performance against both general-purpose MLLMs and specialized geometry solvers. Under test-time scaling, CG-MTE achieves comparable accuracy to high-budget self-consistency while reducing sampling cost by up to 8x. Code and data are publicly available at https://github.com/Jason8Kang/ReTTS-PGPS.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：测试时扩展（Test-Time Scaling, TTS）在一般数学推理任务中已被证明能显著提升性能，但在平面几何问题（PGP）求解的符号程序范式中却无法有效扩展。论文需要解释为什么 TTS 在该领域失效，并提出新的机制来'重新激活'它的收益。
2. 障碍一：受限的推理多样性。现有符号程序范式通常基于固定的形式化程序（如定理证明步骤），生成路径相对刚性，导致采样多条轨迹时彼此高度相似，多样性不足，因此多数投票或自洽等 TTS 策略难以从多样化的推理候选中获益。
3. 障碍二：符号演绎前缺乏显式的视觉接地。平面几何问题必须将图形中的位置、角度、长度关系与文本条件对接。若在符号演绎前没有将图形显式解析为可用的语义表示，模型会在感知层面引入错误，这些错误会在符号链条中被放大，使增加采样次数也无法弥补。
4. 问题挑战的独特性：PGP 要求视觉感知 + 多步符号演绎的耦合，这比纯文本数学推理多了一个感知脆弱点，也比一般视觉问答多了一个严格逻辑约束，因此 TTS 的失效不能简单归因于模型规模或数据不足。
5. 问题价值：如果能在符号程序范式下恢复 TTS 的收益，就等同于在不引入更强模型的前提下提升几何求解精度，同时可降低推理预算，这对实际部署和基准推进都有意义。

Q2: 有哪些相关研究？

1. 神经符号几何求解：早期方法如 Inter-GPS（Lu et al., 2021）和 GeoDRL（Peng et al., 2023）将图形和文本解析为形式化语言（formal language）并借助外部工具或强化学习进行推理。它们依赖结构化的几何关系和定理库，为本文的符号程序范式提供了基础。
2. 多模态大语言模型（MLLM）在几何上的应用：近年通用 MLLM（如 Qwen-VL 系列）被直接用于几何问答，但往往缺少显式符号推理，导致精度不足；专门化的几何求解器则常牺牲泛化性。本文的基线包括这两类方法。
3. 测试时扩展（TTS）：在一般数学推理中，自洽性（Self-Consistency）、多数投票、Best-of-N、逐步验证等方法被证明能通过增加采样提高准确率。但本文指出这些方法对几何符号程序并不直接有效，原因与轨迹多样性和感知错误相关。
4. 程序化推理（Program-aided Reasoning）：PaL（Program-aided Language models）等方法用代码解释器执行推理步骤。本文的 MTS 将符号程序转换为可执行 Python 脚本正是这条路线的延伸，但并非简单照搬，而是作为异构轨迹之一。
5. 链式思考（CoT）增强：CoT 是通用的推理增强手段，本文将其作为符号程序的补充变体（CoT-augmented variants），说明它与符号程序互补，而非替代。
6. 数据增强与多轨迹合成：将单一程序展开为多种异构轨迹的方法与数据增强、蒸馏等有交集，但本文强调轨迹的异构性服务于 TTS 的多样性需求。
7. 自适配推理策略：CG-MTE 通过共识（consensus）动态决定是否继续采样，与计算量自适应分配（如 adaptive computation）的思路相关，目标是平衡预算与精度。
8. 领域空白：目前较少有工作专门诊断 TTS 在几何符号程序中的失败原因，本文属于对该问题的系统性分析与新方法设计。

Q3: 论文如何解决这个问题？

本文提出三部分方法，整体流水线为：先由符号程序生成多种推理轨迹，再用感知增强训练让模型在演绎前获得结构化图形语义，最后用共识引导的集成策略在测试时自适应选择结果。
1. Multi-Trace Synthesis (MTS)：将每个符号程序（symbolic program）转换为异构推理轨迹（heterogeneous reasoning traces），至少包含两类：
 - 可执行 Python 脚本（executable Python scripts）：把几何关系写成代码，通过计算得到结论，利用解释器执行保证数值一致性；
 - CoT 增强变体（CoT-augmented variants）：在原始符号推导上加入自然语言链式思考，增强中间步骤的语义可读性和逻辑过度。
 这样，同一个问题可以有多条表达形式不同、但逻辑相同的轨迹，从而提升采样时的推理多样性，避免刚性程序导致的轨迹重合。
2. Perception-Augmented (PA) Training：在符号演绎前，先将图形解析为结构化语义子句（structured semantic clauses），例如‘线段 AB 与 CD 垂直’‘角 ABC = 45°’等。这些子句作为显式的视觉接地，注入到模型的输入或中间表示中，迫使模型在开始符号推理前先建立对图形的正确理解，减少感知错误向后续演绎传播。
3. Consensus-Guided Multi-Trace Ensemble (CG-MTE)：一种自适应的测试时推理策略。具体地，对多条异构轨迹（由 MTS 生成）进行采样，并计算它们输出答案的共识程度。若共识充分（例如一致比例达到阈值），则提前停止并返回共识答案；若共识不足，则继续增加采样，直到预算耗尽。该设计让模型在简单问题上少采样、在困难问题上多采样，从而在给定预算下最大化精度。
4. 训练与推理流程：基于 MTS 构造 MTS-All 数据集（对应 PGPS9K-All、Geometry3K-All、GeoQA-All），在 Qwen-VL 系列模型上微调，再进行 PA 训练和 CG-MTE 推理。实验发现 PA 训练在这些数据集上带来了显著提升，说明显式视觉接地是弥补感知缺陷的关键。

Q4: 论文做了哪些实验？

1. 基准数据集：PGPS9K、Geometry3K、GeoQA 三个平面几何基准。论文基于 MTS 为每个数据集构造了增强版 MTS-All（PGPS9K-All、Geometry3K-All、GeoQA-All）。
2. 模型系列：微调 Qwen-VL 家族（具体版本未在摘要中明确，但合理推断包括不同规模以评估模型规模的扩展性）。
3. 对比基线：
 - 通用多模态大语言模型（general-purpose MLLMs）；
 - 专用几何求解器（specialized geometry solvers），如 Inter-GPS 等神经符号方法。
4. 测试时扩展设置：
 - 标准 TTS（如 Self-Consistency, SC）作为高预算基线；
 - 本文的 CG-MTE 作为低预算自适应策略，对比精度与采样成本。
5. 评估协议：在不同模型规模下比较准确率，并报告采样成本（采样数或推理步数）。摘要明确提到 CG-MTE 与高预算自洽可比（comparable accuracy），而采样成本降低最多 8 倍。
6. 消融与诊断（合理推断）：论文识别出两个障碍，因此可能包含以下消融：
 - 仅用原始符号程序（无 MTS）时的 TTS 效果；
 - 仅用 MTS（无 PA）时的效果；
 - 仅用 PA（无 MTS）时的效果；
 - 完整方法的效果。
 （这些细节未被检索证据直接支持，但符合方法设计逻辑。）
7. 数据集构建：MTS-All 数据集的规模、生成方式等未在摘要中给出，需要查看原文实验部分。

Q5: 发现了什么实验现象？

1. TTS 在符号程序范式下失效：直接应用 TTS（如增加采样次数）并不能带来预期收益，这是论文的起点观察，也是障碍分析的依据。
2. 多样性不足是失效主因之一：当程序是刚性符号时，多次采样结果高度一致，多数投票等策略失去意义；MTS 通过引入异构轨迹恢复了采样多样性。
3. 感知错误显著影响符号演绎：如果没有 PA 训练，模型可能在解析图形时出错，错误会沿符号链传播，即使增加采样也难以下降错误率。PA 训练带来的视觉接地能大幅降低此类错误。
4. 跨模型规模的一致性：论文称方法在不同模型规模上都持续改进，说明收益不依赖于某一特定容量模型，而是机制性的。
5. CG-MTE 的性价比优势：在相似精度下，CG-MTE 的采样成本比高预算 SC 低最多 8 倍，体现了自适应共识停止的有效性。
6. PA 训练是主要增益来源（合理推断）：摘要特别强调 PA 训练在数据集上的效果，暗示其贡献可能高于 MTS 单独带来的多样性增益。
7. 与专用求解器对比依然有优势：相对 Inter-GPS 等符号方法，神经网络 + 程序结合的混合方案在精度上更有竞争力，本文方法优于这些强基线。
8. 负结果或失败案例：论文没有在摘要中列出明确失败案例，但根据问题分析，可以推测某些高度依赖复杂空间构型的题目仍可能因感知子句不完整而出错，需要原文确认。

Q6: 有什么可以进一步探索的点？

1. 降低结构化标注依赖：论文指出当前 MTS 依赖可靠的正式程序标注。未来可以研究利用弱监督或自监督方式自动生成符号程序，减少人工标注成本。
2. 扩展到 3D 几何：当前研究限于 2D 平面几何，未来可将 MTS 与 PA 思想扩展到 3D 几何、立体几何或空间推理问题，但需面对更复杂的视觉解析和符号化。
3. 更丰富的异构轨迹类型：除了 Python 脚本和 CoT 变体，可以探索自然语言证明、形式化验证脚本、混合符号-数值轨迹等更多形态，进一步提升多样性。
4. 动态共识阈值的自适应优化：CG-MTE 中的共识阈值可能需要针对不同难度或领域自适应调整，未来可学习元策略来预测难度并动态设置阈值。
5. 扩展到其他数学子领域：该方法可能适用于代数、解析几何、三角学等也需要符号演绎的数学分支，验证其泛化性。
6. 与更强推理模型结合：在 Qwen-VL 之外，尝试更大规模或专门训练的推理模型，看 MTS/PA/CG-MTE 是否能带来正交增益。
7. 可解释性与形式验证：结构化语义子句使得推理过程更透明，未来可以探索用这些子句生成可验证的证明或解释，服务于教育场景。
8. 多模态感知与程序执行的端到端联合学习：当前 PA 是前置解析，未来可以设计为端到端可微的感知-演绎联合训练，减少信息损失。

Q7: 总结一下论文的主要内容

这篇论文聚焦于平面几何问题求解中的测试时扩展（TTS）失效问题。作者首先指出现有 TTS 技术在一般数学推理中有效，但在符号程序范式下无法有效扩展，并归因于两个障碍：刚性符号程序导致的推理多样性不足，以及符号演绎前缺乏显式视觉接地。基于此，他们提出了一套三组件方法：（1）Multi-Trace Synthesis (MTS)，将单一符号程序转化为多种异构推理轨迹（可执行 Python 脚本和 CoT 增强变体），以增强采样多样性；（2）Perception-Augmented (PA) training，在演绎前将图形解析为结构化语义子句，提供明确的视觉证据，减少感知性符号错误；（3）Consensus-Guided Multi-Trace Ensemble (CG-MTE)，一种自适应推理策略，根据多轨迹共识程度动态调整采样预算，实现精度与成本的平衡。实验在 PGPS9K、Geometry3K、GeoQA 三个基准上进行，作者基于 MTS 构造了增强数据集 MTS-All，并微调 Qwen-VL 系列模型。结果显示，该方法在不同模型规模下都能一致提升 PGP 求解性能，并优于通用 MLLM 和专用几何求解器；在测试时扩展场景中，CG-MTE 以最高 8 倍的成本降低达到与高预算自洽相当的精度。论文还提出了三个主要贡献点：识别 TTS 失效的障碍、提出 MTS 与 PA 训练、提出 CG-MTE 推理策略。局限性方面，论文坦白指出当前方法依赖结构化标注、符号程序质量受限，且只评估了三个 2D 几何数据集。总体而言，这是对几何推理中 TTS 机制的一次系统分析与改进尝试，兼具诊断价值和工程收益，并公开了代码与数据。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：适用于关注多模态推理、视觉感知与符号演绎结合的读者，特别是几何 AI、数学推理方向。

## 基本信息

- 作者：Xiaoqiang Kang, Shengen Wu, Maizhen Ning, Xiaobo Jin, Kaizhu Huang, Yutao Yue, Xiaowei Huang, Qiufeng Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30156v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（摘要、引言、相关工作、结论、局限性等片段），并结合启发式草稿进行补全和润色；部分实验细节为基于方法逻辑的合理推断，已在对应位置标注。
