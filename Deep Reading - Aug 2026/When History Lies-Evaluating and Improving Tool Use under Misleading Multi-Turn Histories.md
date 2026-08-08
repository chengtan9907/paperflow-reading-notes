---
user_id: "cheng tan"
paper_id: 6950
arxiv_id: "2608.06057v1"
title: "When History Lies: Evaluating and Improving Tool Use under Misleading Multi-Turn Histories"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06057v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06057v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T13:59:57"
---
# When History Lies: Evaluating and Improving Tool Use under Misleading Multi-Turn Histories

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：tool use · multi-turn dialogue · history pollution · benchmark

## 一句话总结

本文提出ContextPollute-Bench基准和ORACLE-OPD方法，证明误导性多轮历史可劫持模型已有的工具调用策略，并通过Oracle条件策略迁移显著提升鲁棒性。

## 摘要

> Tool-calling agents infer task state from accumulated dialogue and tool traces. In persistent interactions, however, historical traces may remain structurally valid and semantically plausible after they cease to be authoritative for the current request. We show that such history can hijack a policy the model already possesses: on Qwen3-1.7B, pollution flips 32.1% of decisions that are correct under the original trajectory and frequently induces reuse of corrupted entities or interface conventions. We introduce bench, a paired benchmark with synchronized Original, Polluted, and Oracle State views that preserve the system policy, current tools, latest request, and gold next action. Eleven gold-preserving interventions isolate failures in decision state, entity binding, and interface execution across complete calls and non-call decisions. We further propose ours, which transfers an Oracle-conditioned teacher policy to a student observing only polluted history through soft supervision on student-generated prefixes. On Qwen3-1.7B, ours achieves 87.0% Balanced Tool-Use Accuracy, outperforming Gold-SFT (66.3%), Oracle sequence distillation (82.3%), and off-policy token distillation (85.0%). The method scales consistently: an 8B teacher raises the same compact 1.7B student to 91.9%, while an 8B student reaches 93.0%. The resulting policies further transfer to clean histories, unseen functions, independently regenerated evaluation contexts, external tool-use benchmarks, and noisy multi-hop question answering. These results establish history reliability as a distinct tool-use bottleneck and demonstrate reliable-state policy transfer as an effective and scalable solution.

Q1: 这篇论文试图解决什么问题？

本文聚焦于工具调用智能体在多轮持久交互中的一个关键失败模式：历史轨迹虽然仍保持结构上的有效性和语义上的合理性，但可能已经丧失了对当前请求的权威性。例如，某个工具的参数已被更新，或用户意图在后续消息中发生了改变，但模型仍可能基于旧历史推断状态，从而导致误用被污染的实体或接口约定。这种“历史污染”问题在现有工具使用研究中未被系统性地认识。现有工作大多假设累积的对话历史和工具痕迹是准确且权威的，主要关注如何从大规模API集合中选择工具、如何通过推理-动作循环完成任务，以及如何抵御自然查询变化或提示注入攻击，但很少研究“历史状态不再可信”这一更隐蔽的失败。

论文试图回答以下具体问题：
1）历史污染在多大程度上会劫持模型已有的正确决策策略？论文用实验数据表明，在Qwen3-1.7B上，污染导致32.1%的原本正确决策发生翻转，并且频繁引发对被污染实体或接口约定的重复使用。
2）如何系统性地评估和诊断这种失败？现有基准缺乏对历史权威性变化的控制，也无法区分模型是在决策状态判断、实体绑定还是接口执行环节出错。
3）如何训练模型在污染历史下仍保持可靠的工具使用能力？传统监督微调（如Gold-SFT）在干净数据上表现良好，但在污染场景下崩溃；简单的序列蒸馏或token蒸馏也无法应对学生与教师状态不一致的问题。

因此，论文的核心贡献在于：将“历史可靠性”确立为一个独立的研究瓶颈，设计可控基准来量化并定位失败，并提出一种能够在状态扰动下迁移教师策略的训练方法。

Q2: 有哪些相关研究？

与本文相关的已有研究主要分布在以下几个方向：

1. 工具使用语言模型：大量工作将工具使用与语言模型结合，例如通过推理-动作框架（如ReAct）、自监督工具学习（如Toolformer）以及在大规模API集合上的工具选择（如Gorilla、ToolLLM）等（Yao et al., 2023; Schick et al., 2023; Patil et al., 2024）。这些工作训练或提示模型根据对话历史和工具描述产生调用，但通常隐含地假设历史信息是准确且按时间顺序有效的。

2. 工具使用评估与鲁棒性：已有评估研究关注自然语言查询的变体对工具选择的影响，也有工作探讨提示注入、恶意指令等安全鲁棒性。但针对“历史状态过时”这一特定扰动的研究相对较少，尤其缺乏能够精细控制状态变化的成对基准。

3. 对话状态跟踪与知识编辑：传统对话系统常显式建模对话状态（如belief state），但本文的场景是隐式的，模型需要从自由文本的工具痕迹中推断当前状态，且状态可由外部工具动态改变。知识编辑研究关注模型内部事实的更新，但与多轮交互中历史权威性的动态丧失并不等同。

4. 策略蒸馏与模型压缩：序列蒸馏（sequence distillation）和token级蒸馏被用于从大模型向小模型迁移工具调用能力，但这类方法通常假设教师和学生观察到的上下文一致。在本文的问题中，教师使用Oracle状态，学生只能看到污染历史，直接模仿教师token输出会导致状态错配，这为新的蒸馏设计提出了挑战。

总之，本文填补了工具使用鲁棒性研究中对“历史权威性”这一维度的空白，并扩展了蒸馏方法在观测分布不一致时的适用性。

Q3: 论文如何解决这个问题？

论文的解决方案分成两条主线：一是构建可控的评测基准ContextPollute-Bench，二是提出训练方法ORACLE-OPD。

（1）ContextPollute-Bench：
该基准以“配对”（paired）方式构造，对每个任务提供三个同步的状态视图：
- Original视图：原始的、未受污染的完整历史轨迹；
- Polluted视图：在原始历史中注入特定污染后的轨迹，污染方式包括修改实体、改变接口约定或插入与当前请求矛盾的状态信息；
- Oracle State视图：提供了当前请求状态下真正权威的完整状态信息（但不会直接泄露gold action）。
这三种视图共享完全相同的系统策略、当前工具列表、最新用户请求以及黄金下一动作（gold next action），从而保证性能差异完全由历史状态的可信度造成，而不是任务难度或工具集合的变化。

为了精确诊断失败来源，基准设计了十一种“黄金保持式”（gold-preserving）干预方法，它们分别作用于三个层面：
- 决策状态：例如，改变工具的执行结果或参数状态，使历史记录与真实状态冲突；
- 实体绑定：例如，置换或替换历史中出现的实体标识，使模型可能将当前请求绑定到错误实体；
- 接口执行：例如，修改工具调用的格式、参数顺序或返回结构，使模型沿用旧接口约定。
此外，干预同时覆盖完整工具调用（complete calls）和“不调用”（non-call）决策（例如，模型可能决定不调用工具而直接回答），确保评测覆盖面广。

（2）ORACLE-OPD训练方法：
核心思想是让学生模型仅从污染历史中学习，而教师模型能够访问Oracle状态。由于学生和教师观察到的状态不一致，直接模仿教师的完整动作序列会传播错误。因此，ORACLE-OPD采用“学生生成前缀”的软监督方式：
- 学生首先基于污染历史解码出一个候选前缀（若干步动作或文本）；
- 教师看到学生的前缀后，利用Oracle状态计算该前缀在正确状态下的合理性（例如，通过计算概率或价值），生成软标签；
- 学生根据软标签进行梯度更新，从而学会在面对污染历史时，仍然能够做出与Oracle状态一致的决定。
这种软监督对学生来说是“离策略”的，但通过不断收集学生自己的前缀，可以缓解暴露偏差。训练时教师可来自更大规模模型（如8B），而学生保持紧凑（如1.7B），实现知识迁移与模型压缩。

在推理阶段，只使用学生模型和普通（可能被污染）的历史，不依赖Oracle状态，因此方法在实际部署中是可用的。

Q4: 论文做了哪些实验？

论文在Qwen3系列模型上进行了系统实验。

1. 主要实验（Qwen3-1.7B）：
- 使用ContextPollute-Bench评测。
- 对比方法包括：
 - Gold-SFT：使用原始的黄金轨迹进行标准监督微调；
 - Oracle序列蒸馏（Oracle sequence distillation）：教师基于Oracle状态产生完整序列，学生模仿整个输出序列；
 - 离策略token蒸馏（Off-policy token distillation）：在某种固定策略下进行token级蒸馏；
 - ORACLE-OPD（本文方法）。
- 指标：Balanced Tool-Use Accuracy（BTA），该指标可能综合了完整调用与非调用决策的准确率，并考虑了类别平衡。
- 结果：ORACLE-OPD达到87.0%，显著高于Gold-SFT（66.3%）、Oracle序列蒸馏（82.3%）和离策略token蒸馏（85.0%）。

2. 规模扩展实验：
- 使用8B教师蒸馏1.7B学生，ORACLE-OPD将学生提升至91.9%；
- 直接训练8B学生（可能使用8B教师或同规模教师），达到93.0%。

3. 迁移与鲁棒性实验：
- 冻结在污染数据上训练好的检查点，在以下场景中重新评估而不做额外微调：
 - 干净历史（无污染）；
 - 未见过的函数（工具）；
 - 独立重新生成的评估上下文（可能是不同的任务配置）；
 - 外部工具使用基准；
 - 嘈杂的多跳问答（noisy multi-hop QA）。
这些实验检验模型是否学到了通用的“可靠状态判断”能力，而非针对特定污染模式的修正规则。

4. 诊断实验：利用十一种干预方法，分析模型在决策状态、实体绑定、接口执行三类失败上的表现差异（具体数值在摘要中未给出，但论文中应有详细表格）。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：

1. 污染的高破坏性：在Qwen3-1.7B上，污染翻转了32.1%的原本正确决策，说明即使模型已经掌握了正确的策略，误导性历史也能显著改变其行为。这验证了历史可靠性的独立重要性。

2. 基线方法对比：
- Gold-SFT尽管使用黄金数据，但在污染历史下性能骤降（66.3%），表明简单监督微调过拟合了干净状态分布，无法推广到状态不一致的情况。
- Oracle序列蒸馏（82.3%）优于Gold-SFT但低于ORACLE-OPD，说明仅向学生展示Oracle期望序列还不够，因为学生无法在推理时获得Oracle状态，直接模仿会导致状态错配。
- 离策略token蒸馏（85.0%）比序列蒸馏略好，但仍低于本文方法。

3. ORACLE-OPD的显著优势：在相同模型规模下，软监督方式比硬序列模仿高出约4.7个百分点，说明利用学生生成前缀、教师基于Oracle状态提供软标签，能够更有效地规避观测状态不一致问题。

4. 规模扩展的一致性：8B教师将1.7B学生从87.0%提升到91.9%，8B学生达到93.0%，表明方法在模型规模上有正收益，教师越强，学生表现越好。

5. 迁移性表明无过拟合：训练后的模型在干净历史、未见函数、独立重新生成的上下文和外部基准上表现良好，这说明模型学习到的是“判断历史权威性”的通用策略，而不是针对训练时污染模式的机械修正。如果模型只是学会了忽略特定类型的注入痕迹，那么在干净历史上的性能会下降，但实验并未出现这种情况。

6. 指标间张力：虽然BTA是综合指标，但11种干预可能单独展现出不同的脆弱性。推测某些干预（如接口执行改动）可能比实体绑定更难应对，因为接口格式变化可能影响模型对工具调用的语法生成（此为合理推断，具体数据需参考论文全文）。

Q6: 有什么可以进一步探索的点？

基于此工作，可以进一步探索以下方向：

1. 扩展到更长的多轮交互：当前基准可能主要针对中等长度的对话，未来可研究数十轮甚至百轮以上的历史，以及污染随时间逐渐累积的情况。

2. 多种污染类型的组合：现实中的历史可能同时包含实体、接口和决策状态的多重污染，研究这些污染的叠加效应和联合诊断方法。

3. 多模态工具调用：将基准和方法扩展到包含图像、代码或结构化数据的多模态环境，检验历史权威性在多模态上下文中的表现形式。

4. 更细粒度的训练目标：ORACLE-OPD使用软标签，但如何设计更好的软标签（例如考虑不确定性或状态置信度）仍有空间。

5. 与其他鲁棒性方法结合：如将ORACLE-OPD与对抗训练、提示工程或记忆增强机制结合，以进一步增强对不可见污染的抵抗。

6. 可解释性分析：研究模型如何内部表征“历史权威性”，例如通过注意力分析或探针实验，识别哪些上下文特征导致了可信度判断。

7. 实际部署场景的验证：在未来交互中，历史污染可能源于工具版本升级、用户意图修正或系统重启，将方法应用于这些真实演化场景。

8. 跨语言、跨域泛化：在多种语言的对话数据上评估，并测试在垂直领域（如金融、医疗）中的有效性。

Q7: 总结一下论文的主要内容

本文探讨了多轮交互中工具调用智能体的一个被忽视的失败模式：历史痕迹虽然结构有效、语义合理，却已不再对当前请求具有权威性。作者称之为“历史污染”（history pollution），并证明这种污染能够劫持模型已经具备的正确决策策略，导致模型误用被污染的实体或接口约定。在Qwen3-1.7B上的实验显示，污染使32.1%的原本正确决策发生翻转，凸显了问题的严重性。

为了系统研究并解决这一问题，作者提出了两大部分贡献。第一是基准ContextPollute-Bench。该基准以配对方式构造，对每个任务提供三种同步视图：Original（原始、干净的历史）、Polluted（注入污染的历史）和Oracle State（当前真实状态的权威视图）。三者共享相同的系统策略、当前工具、最新请求和黄金下一动作，从而确保性能差异完全由历史状态造成，而不是任务本身的变化。基准还设计了十一种“黄金保持式”（gold-preserving）干预方法，从决策状态、实体绑定和接口执行等不同层面注入污染，覆盖完整工具调用和非调用决策两种场景。这一设计允许研究者精细地定位模型在哪一类状态误导下最脆弱。

第二是训练方法ORACLE-OPD（Oracle条件策略蒸馏）。核心思想是利用在Oracle状态下可用的教师策略，来指导仅能观察污染历史的学生模型。具体而言，学生先基于污染历史生成自己的前缀（而不是逐token模仿教师的完整输出），然后教师根据Oracle状态和学生的前缀提供软监督信号。这种基于学生生成前缀的软监督避免了将教师期望的轨迹直接强加给学生，因为学生和教师所处的状态不同。实验表明，在Qwen3-1.7B上，ORACLE-OPD达到87.0%的平衡工具使用准确率（BTA），大幅优于Gold-SFT（66.3%）、Oracle序列蒸馏（82.3%）和离策略token蒸馏（85.0%）。方法还表现出良好的规模扩展性：使用8B教师可以将相同的1.7B学生提升至91.9%，而8B学生本身可达到93.0%。

更重要的是，作者验证了训练得到的策略具有跨环境迁移能力。在冻结训练后检查点的情况下，模型能够处理干净历史（无污染）、未见过的函数、独立重新生成的评估上下文、外部的工具使用基准以及嘈杂的多跳问答任务。这表明模型学习到的是真正可靠的状态判断策略，而不是针对特定污染痕迹的简单修正规则。

综上所述，本文将“历史可靠性”确立为工具使用中的一个独立瓶颈，并提供了可量化的基准和有效的训练解决方案。这项工作对构建长期运行、需要持续交互的智能体具有重要的实际意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文直接研究agent的工具使用可靠性问题，与用户画像中的agent方向（权重0.10）高度相关。

## 基本信息

- 作者：Xiaoqing Wu, Xingyu Fan, Feifei Li, Wenhui Que
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06057v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段（Abstract、Introduction、Related Work、Conclusion等），结合摘要与启发式草稿进行了补全与校准。
