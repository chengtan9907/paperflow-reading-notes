---
user_id: "cheng tan"
paper_id: 9133
arxiv_id: "2608.22753v1"
title: "Beyond Factual Knowledge: Benchmarking and Learning Step-Level Procedural Rule Reasoning in Large Language Models"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.22753v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.22753v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:05:09"
---
# Beyond Factual Knowledge: Benchmarking and Learning Step-Level Procedural Rule Reasoning in Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：procedural rule reasoning · rule-based benchmark · kv cache injection · step-level attention

## 一句话总结

本文提出 RuleWorld 基准和 DynaRule 框架：RuleWorld 将外部程序性规则重构为全局可复用的抽象单元，覆盖单规则、并行多规则和多跳推理等场景；DynaRule 将规则注入 KV cache，通过 Stacked Step-Level Attention Training 实现逐步、可学习的动态规则重注意与更新，在 10K 规则池下使平均 QA 准确率最高提升 19 个点、Recall@1 超过 85%。

## 摘要

> Large language models (LLMs) excel at text understanding and generation, yet still struggle to reliably understand and apply externally provided procedural rules at scale. To evaluate this capability, we introduce RuleWorld, a large-scale benchmark that reformulates rules as globally reusable abstract units rather than instance-specific facts. In RuleWorld, several scenarios, including single-rule, parallel multi-rule, and multi-hop reasoning, are settled for comprehensive evaluation. We further propose DynaRule, an end-to-end framework that injects the given rules into the KV cache and turns retrieval into an internal, learnable, step-wise process. Specifically, DynaRule employs Stacked Step-Level Attention Training with a special token to enable dynamic rule re-attention and updating during inference. In this way, the model can re-attend to the most relevant rules at each step, dynamically replacing outdated ones to support more stable multi-step reasoning. Experiments on RuleWorld show that existing LLMs face challenges under large rule pools, while DynaRule improves average QA accuracy by up to 19 points and achieves over 85% Recall@1 at 10K rules, outperforming strong baselines by large margins. We make our code and dataset available here: https://github.com/SharkSpicy-NLP/Beyond-Factual-Knowledge.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：本文针对的是 LLM 对“外部程序性规则”的可靠理解与应用能力，而不是一般的事实知识问答。程序性规则指协议、规章、操作流程、约束条件等抽象知识单元，它们跨实例可复用、需要按步骤组合，并且可能随推理进程变化。LLM 虽然擅长文本理解和生成，但在规则池规模变大时，如何识别、选择、组合、更新规则仍然不可靠。
2. 现有评估范式的缺陷：已有 QA/规则推理任务通常把相关规则作为每道题的上下文前提注入，规则被降级为“per-question hints”。这一设计忽略了规则知识跨 QA 实例的共享性，也无法回答模型到底是否真的掌握了规则应用能力。具体来说，它无法区分：(i) 模型能否从大规则池中识别相关规则；(ii) 能否在并行多规则下处理冲突和优先级；(iii) 能否在多步推理中动态换用更合适的规则；(iv) 能否真正把规则作为抽象单元进行组合。由于这些子能力被混在一起，误差来源难以定位。
3. 扩展性问题：当规则数量达到千万级（RuleWorld 包含数百万条抽象规则）时，把全部规则塞进上下文既不现实也不经济。传统的先检索后生成的方案把检索和推理割裂成两个模块，检索误差会直接污染推理。DynaRule 的出发点是让检索在模型内部、在推理步骤之间以可学习方式发生，从而绕开上下文长度瓶颈。
4. 动态性问题：多步推理中，不同步骤需要的规则可能不同；旧规则可能失效或需要被新规则替换。静态规则注入无法支持这种更新，而 DynaRule 通过 KV cache 中的特殊 token 驱动逐步重注意，尝试让模型在每一步重新关注最相关规则，用新规则替换过期规则。
5. 评估空白：已有的推理基准大多针对数学、常识、符号逻辑或事实问答，但缺少对“全局可复用程序性规则”的系统化、可控评估。RuleWorld 试图填补这个空白，用非常识规则避免模型靠参数记忆作答，从而真正测试规则应用能力。
（上述第 1-2 点有摘要和 Introduction 检索片段支持；第 3-5 点部分为结合领域常识的合理推断，需以原文为准。）

Q2: 有哪些相关研究？

1. 上下文内规则/知识注入：一类主流做法是把外部知识或规则作为前缀 prompt 注入，例如 in-context learning、few-shot prompting、以及检索增强生成（RAG）。这些方法把规则当作上下文 token，受限于上下文长度，并且检索与推理通常是两段式；检索结果一旦不准确，后续推理就会被误导。（合理推断，具体文献清单需回原文确认）
2. 知识库问答与符号推理：KBQA、逻辑推理、程序性知识执行等任务要求模型依据外部 schema、规则或数据库进行推理。一些工作采用神经符号结合或符号规划器来保证规则执行的可控性，但在灵活性和端到端学习上存在折中。（合理推断）
3. KV cache 编辑与上下文压缩：近期的长上下文和推理效率工作尝试通过编辑 KV cache、上下文蒸馏或压缩历史来让模型在有限空间内使用更多信息。DynaRule 将规则注入 KV cache 的做法与这一趋势相关，但把“逐步可学习的规则检索”作为核心创新点。（合理推断）
4. 记忆增强网络：将外部知识写入显式记忆，在解码时进行检索读取，这与 DynaRule 的目标相似；但 DynaRule 强调完全内部化、端到端训练、以及推理过程中的动态更新，而非固定的记忆读取。
5. 推理基准：已有基准覆盖数学、常识、复杂指令跟随、多跳问答等，但大多把规则/前提作为给定实例的上下文。RuleWorld 的不同在于规则是全局一致、跨实例复用的抽象单元，并刻意使用非常识规则，防止模型依靠参数中的先验知识作答。
6. 强模型评估基线：摘要和检索片段明确提到 DeepSeek V3.2 和 GPT-5.5 等当前强模型在 RuleWorld 上也面临挑战，说明该能力仍是前沿模型尚未解决的问题。
（本文的 Related Work 原文未在检索片段中完整提供，以上分类属于基于摘要和领域知识的合理重建；具体引用和比较以原文为准。）

Q3: 论文如何解决这个问题？

1. RuleWorld 基准设计：规则被形式化为全局一致的抽象单元，以 FOL（一阶逻辑）和自然语言（NL）两种形式提供，覆盖四种规则类型、七种子类型，并构造 11 个 QA 子任务。评测场景包括：(i) single-rule：单条规则直接应用；(ii) parallel multi-rule：多条并行规则同时相关，需要选择和冲突处理；(iii) multi-hop：规则之间需要链式组合才能得到答案。规则刻意设计为非常识（non-commonsense）内容，使模型必须依赖外部规则而不是参数内记忆。
2. DynaRule 框架核心：端到端地把给定规则注入 KV cache，在解码时让模型从内部缓存中“检索”规则，而不是把规则文本重新拼接进输入 prompt。检索被转化为模型内部的、可学习的、逐步进行的过程：每一步模型可以重新注意最相关的规则，并动态替换过时规则，支持多步推理中的规则更新。
3. Stacked Step-Level Attention Training：训练策略配合特殊 token（检索片段中记为 <search>）在推理时驱动逐步检索与更新。通过对注意力进行逐步的、堆叠式的训练，模型学会在每一步决定需要什么规则、应关注 KV cache 中的哪些内容，从而实现检索与推理的联合优化。
4. 与现有方案的差异：不是两段式“先检索后生成”，也不是把所有规则一次性塞进上下文；检索发生在模型内部，跨步骤动态进行，规则可被替换。这种设计让模型在长链推理中不会一直被早期选中的规则束缚，理论上更接近人类逐步查阅规则手册的过程。
5. 实现层面的不确定性：由于 Method 原文未被完整提供，<search> token 的确切用法（例如是作为输入 token 还是内部特殊符）、KV cache 注入的具体组织方式、注意力训练损失形式和推理时的更新触发条件，仍需回原文确认。

Q4: 论文做了哪些实验？

论文在 RuleWorld 上进行了系统评估和对比实验，从摘要和检索片段可重建的实验元素如下：
1. 评估模型：包括 DeepSeek V3.2、GPT-5.5 等现有强 LLM，说明这些模型在 RuleWorld 上表现明显受限；DynaRule 与多个强基线进行对比。
2. 评测场景：单规则、并行多规则、多跳推理，共 11 个 QA 子任务；规则池规模被系统变化，其中明确包含 10K 规则的设定。
3. 主要指标：QA 准确率（平均）和 Recall@1（从规则池中检索引出最相关规则并命中的比例）。
4. 已知定量结果：DynaRule 将平均 QA 准确率最高提升 19 个点；在 10K 规则下 Recall@1 超过 85%，优于强基线。
5. 信息缺口：具体规则池规模梯度、baseline 方法列表、每个子任务的逐项结果、消融实验设置、训练数据来源、DynaRule 的模型规模和训练成本等细节，在本摘要片段中没有出现，需要查阅原文 Experiments 章节。
（以上实验描述依据摘要和 Introduction 检索片段整理，具体协议以原文为准。）

Q5: 发现了什么实验现象？

1. 大型规则池对现有 LLM 构成显著挑战：摘要明确提到“existing LLMs face challenges under large rule pools”，合理推断为规则池规模增大时，模型性能明显下滑，说明把规则全部塞进上下文的做法难以扩展到大规模规则集。
2. DynaRule 带来显著增益：平均 QA 准确率最高提升 19 个点，说明“内部化检索 + 逐步更新”的设计确实有效；在 10K 规则下 Recall@1 超过 85%，表明模型在很大规则池中仍能比较准地找到关键规则。
3. 检索命中与最终答案之间的张力：Recall@1 反映的是规则检索能力，QA 准确率反映的是应用能力；两者不是同一件事。论文没有给出两者逐场景对照，可能的情况是：检索命中高但答案仍错（规则组合/执行失败），或检索命中低但答案碰巧对。这一张力值得在原文结果表中重点核查。
4. 推测性的场景差异：并行多规则场景可能暴露出规则冲突、优先级排序问题；多跳场景可能放大错误传播，即早期规则选择错误会导致后续所有步骤偏离。现有模型的失败大概率集中在这些复杂场景上。（推测，需要原文验证）
5. 负结果与消融：目前检索片段中没有提供消融实验、失败案例或指标间的系统对比；例如去掉 Stacked Step-Level Attention Training 或去掉特殊 token 后的性能变化未知，需回原文确认。

Q6: 有什么可以进一步探索的点？

1. 更丰富的规则表示：从 FOL 和自然语言扩展到可执行代码、伪代码或结构化规则语言，使规则可以被直接执行和验证（原文 Limitations 明确提到）。
2. 复杂触发模式：建模条件触发、冲突消解、规则优先级、默认值等更接近真实程序性规则的行为（原文 Limitations 明确提到）。
3. 扩展性改进：通过联合训练的规则编码器、层次化或聚类索引、更结构化规则表示来支持更大规模规则库（原文 Limitations 明确提到）。
4. 与 Agent 结合：DynaRule 的内部化、逐步规则检索非常适合作为 agent 的规则引擎，例如工具调用约束、任务流程步骤、安全策略执行等；这与用户画像中的 agent 方向直接相关。
5. 跨领域迁移：将 RuleWorld 的抽象规则评估思想迁移到生物实验协议、科学数据处理流程、法律法规条文、医疗操作规程等场景，需验证规则表达的迁移性和数据构建成本。
6. 动态规则库版本化：进一步研究规则时效性、版本更新、旧规则替换和新规则插入，把 DynaRule 的“更新”能力推向持续演化的规则库。
7. 可解释性与诊断：分析模型在每一步重新注意了哪条规则、为什么替换旧规则，可提供推理依据；也可设计规则违反率、步骤正确率等辅助指标，更精细地定位错误来源。
8. 训练效率与稳定性：Stacked Step-Level Attention Training 在大规则池、长序列上的训练成本、收敛性、以及 KV cache 占用问题有待研究；多 hop 场景的错误传播也可能需要专门的课程学习或引导机制。
（第 1-3 点有原文 Limitations 片段支持；其余为基于方法特点的合理推断或推测。）

Q7: 总结一下论文的主要内容

1. 问题背景与动机：LLM 在文本理解和生成上很强，但可靠地理解和应用外部提供的程序性规则的能力仍不足。程序性规则不是简单事实，而是跨实例可复用的抽象知识单元，需要在多步推理中被选择、组合和更新。现有评估和建模方式通常把规则作为每道题的上下文提示，忽略了规则的全局共享性；同时，当规则池很大时，上下文注入方式难以扩展。
2. RuleWorld 基准：论文提出 RuleWorld，把规则从“实例特定前提”重构为“全局可复用的抽象单元”。规则以 FOL 和自然语言两种形式构建，覆盖四种类型、七种子类型，对应 11 个 QA 子任务；评测场景包括单规则、并行多规则和多跳推理。规则设计为非常识内容，以避免模型从参数记忆中获得答案；规则的全局一致性保证同一规则可以在多个问题中复用，从而真正考察规则检索、选择、组合和更新能力。
3. DynaRule 框架：针对大规则池和动态更新需求，论文提出端到端框架 DynaRule。它把给定规则注入 KV cache，在解码过程中通过可学习的逐步注意力机制动态检索规则。具体采用 Stacked Step-Level Attention Training，配合特殊 token（<search>）驱动模型在每一步重新注意最相关规则，并替换过期规则，使检索与推理在模型内部统一、联合优化。这避免了显式两段式检索，也减轻了上下文长度压力，支持更稳定的多步推理。
4. 实验结果：在 RuleWorld 上，现有 LLM（如 DeepSeek V3.2、GPT-5.5）在大型规则池下表现不佳，而 DynaRule 平均 QA 准确率最高提升 19 个点，10K 规则下 Recall@1 超过 85%，大幅优于强基线。这验证了将规则检索内部化、逐步化的有效性。
5. 局限与展望：作者指出当前规则仅支持 FOL 和自然语言形式，未来可探索可执行代码、更复杂触发模式、联合训练的规则编码器、层次化/聚类索引等。更广泛地看，该工作为程序性规则推理提供了新的系统化基准和方法，也为 Agent、科学流程、法律法规等需要外部规则约束的落地场景提供了基础。
6. 总体评价：这是一项系统性强的工作，把“规则检索—规则更新—规则推理”统一进模型内部，既贡献了大规模基准，也贡献了新的训练推理框架；但关于规则生成质量、训练细节、失败模式等，仍需要结合原文展开验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 generation 方向直接重合：论文研究 LLM 的规则生成与应用能力，DynaRule 在解码过程中逐步生成并更新注意力。

## 基本信息

- 作者：Bohan Yu, Pengfei Cao, Chen Han, Chenxi Zhou, Zhiheng Zhang, Zhiyang Xie, Wenhao Teng, Xiangwen Liao, Jun Zhao, Kang Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.22753v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Abstract、Introduction、Conclusion 和 Limitations 片段（Qwen3-Embedding-8B 检索），并整合了论文元数据；但原始 PDF 的 Method、Experiments 等内容未完整提供，部分机制与实验细节为合理推断或推测，需回原文确认。
