---
user_id: "cheng tan"
paper_id: 17933
arxiv_id: "2610.32770"
title: "On-Policy Self-Training: Data-Free Self-Improvement in Large Language Models"
publish_date: "2026-10-26"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.32770.pdf"
pdf_url: "https://arxiv.org/pdf/2610.32770v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:13:26"
---
# On-Policy Self-Training: Data-Free Self-Improvement in Large Language Models

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy self-distillation · self-training · data-free post-training · synthetic data

## 一句话总结

论文提出 On-Policy Self-Training（OPST）：只给粗粒度主题描述，让模型自生成问题与带候选答案的特权推理轨迹，再以同一模型在特权上下文下作教师、在纯问题下作学生进行 on-policy self-distillation，从而在无人工 QA、无更强外部教师的条件下实现文本推理与 VQA 的跨域自我提升，并尽量保留未自训练域的能力。

## 摘要

> On-policy self-distillation enables post-training foundation models without relying on stronger external teachers. However, existing approaches rely on ground-truth question-answer pairs, human-curated problem sets, or other supervised signals. In this work, we reformulate on-policy self-distillation as a self-training framework, driven by self-generated question-answer pairs and privileged reasoning demonstrations. We push supervision to the limit: guided solely by coarse, high-level topic descriptions, the model synthesizes its own curriculum of queries and privileged reasoning traces without access to any human-curated prompts or answers. We find that self-distillation on fully synthetic privileged information enables a model to improve across multiple domains while retaining its capabilities on domains it is not self-trained on. We demonstrate the generality of this framework across text and multimodal domains, achieving consistent self-improvement on standard language benchmarks and Visual Question Answering. Our findings demonstrate that foundation models possess rich latent capabilities that can be amplified without external task supervision or curated problem distributions.

Q1: 这篇论文试图解决什么问题？

1. 论文处在一条“外部监督不断被转移、但从未真正消失”的技术链条上。SFT 用人工示范提供密集 token 级目标，但训练轨迹是 teacher-forced 的，模型在推理时不会走到这些状态，因而存在 exposure bias；论文还引用既有工作指出 SFT 可能侵蚀模型已有能力。RLVR 改用模型自己的 rollout，避免轨迹分布不匹配，但把整条轨迹压缩成一个标量奖励，监督信号变稀疏。On-policy distillation（OPD）把两者结合：学生采样自己的轨迹，教师在学生访问到的状态上给出密集 next-token 目标。On-policy self-distillation（OPSD）进一步去掉更强教师，用同一组参数在非对称条件下构造教师和学生。

2. 但 OPSD 仍然要一个“特权上下文”（privileged context），通常是 ground-truth solution；已有的 OPSD 方法还依赖 ground-truth QA、人工 curated problem sets 或其他监督信号。也就是说，监督只是从“外部教师”转移到了“特权答案/人工问题”，并没有被消除。论文要解决的核心问题因此是：能否把 OPSD 的特权上下文也变成模型自己生成的东西，从而把 post-training 推到只给粗粒度主题描述、不给任何人工问题与答案的 data-free 设定。

3. 这个设定有两个同时成立的目标。第一，模型必须只靠自生成问题和特权信息，在种子主题所覆盖的领域内提升推理能力。第二，因为训练信号完全来自自身生成、没有人工校验，模型还必须保留未自训练领域上的通用推理与参数化知识，避免自我训练把已有能力“洗掉”或造成灾难性遗忘。这两个目标之间存在张力：越激进地拟合自生成数据，越可能放大噪声和自信错误；越保守，又难以获得实质提升。

4. 论文还必须解决三个技术缺口。其一是生成数据的良构性：自生成问题可能歧义、不可解、答案不唯一。其二是信息量：模型已经能稳定答对的问题对当前学生没有训练价值，需要一种不依赖 gold label 的“课程”机制来筛掉太容易的样本。其三是伪验证的可靠性：没有外部答案时，只能用自一致性、改写不变性等内在信号做代理，但自一致性衡量的是“是否稳定”，不是“是否正确”，一致错误仍可能进入训练。

5. 论文的隐含假设是：基础模型的参数中已经潜藏了足够多的可被激发的推理能力，自生成问题与特权推理轨迹能够把这些能力“调出来”，而不是创造全新的能力。这个假设也解释了作者在 limitation 中明确承认 OPST 只能放大已有知识。另一个隐含假设是粗粒度主题描述足以锚定课程分布；但主题描述是人工选择的，因此“data-free”并不是绝对零人工输入，而是“无人工 QA / 无人工问题集”，边界条件需要读者自己校准。

6. 评价设计试图回答三个问题：在只给主题描述的条件下，模型能否在 in-domain benchmarks 上自我提升；当混合多个生成领域时，未自训练的 OOD benchmarks 是否保留甚至受益；以及同一套框架能否迁移到多模态 VQA。论文用同一批自生成数据比较 SFT、GRPO 和 OPST，实际上是在控制“数据相同”的前提下比较监督信号形态：密集目标、稀疏奖励、密集特权上下文，三者谁更能把合成数据转化为能力。

Q2: 有哪些相关研究？

1. 监督微调（SFT）是论文对比的第一条路线。SFT 在人工 curated demonstrations 上做 token 级最大似然，提供密集监督，但训练时使用 teacher forcing，模型在推理时自回归访问的状态可能与训练状态不一致，产生 exposure bias；论文还引用 Shenfeld et al. 2026b 指出 SFT 可能侵蚀已有能力。本文的 SFT baseline 正是在同一批自生成数据上做 dense target，用来检验“有密集目标是否就足够”。

2. 可验证奖励强化学习（RLVR）是第二条路线。RLVR 在模型自己的 rollouts 上训练，避免 SFT 的轨迹分布不匹配，但每条轨迹只有一个标量奖励，监督被高度压缩。论文引用 Guo et al. 2025 与 Shao et al. 2024 作为这一范式的代表，并把 GRPO 作为同数据下的 sparse reward baseline。OPST 的定位不是否定 RLVR，而是主张在“没有可验证奖励、只有自生成特权信息”的场景里，密集上下文蒸馏比稀疏奖励更能利用合成数据。

3. On-policy distillation（OPD）是第三条路线。OPD 让学生采样自己的轨迹，教师在学生访问到的状态上提供 dense next-token targets，因此同时具备 on-policy 状态分布和密集监督。论文引用 Agarwal et al. 2024 与 Gu et al. 2024。OPD 仍然需要更强教师，这是它被本文继续推进的起点。

4. On-policy self-distillation（OPSD）是第四条、也是最直接的祖先。OPSD 用同一组参数构造教师和学生，通过非对称条件化让教师看到学生看不到的特权上下文，通常是 ground-truth solution；论文引用 Zhao et al. 2026a 与 Shenfeld et al. 2026a。后续工作还会变化特权上下文，例如 expert demonstrations，或把方法用于多领域 continual learning。本文与 OPSD 的关键差别不是教师结构，而是监督来源：把“人工问题 + ground-truth solution”替换成“自生成问题 + 自生成 rationale 与候选答案”。

5. 与数据整理成本相关的工作构成论文动机的一部分。论文引用 Lightman et al. 2024 的例子：为一个数学语料做 process-level annotation 需要约 800,000 个人类步骤标签。这个数字被用来说明，若 post-training 继续依赖细粒度人工监督，扩展到多领域会非常昂贵。本文的 data-free 设定正是对这条成本曲线的回应。

6. 与 self-training、self-generated curriculum、pseudo-verification 相邻的工作是本文的方法土壤。论文的方法节明确使用 paraphrase consistency 来检查问题良构性，并用 consensus gate 检查问题对当前学生是否有信息量；这两类内在筛选都不需要 gold answer。更广义的 self-instruct、STaR 式自生成数据、self-rewarding 等范式与本文问题意识相近，但提供的检索片段没有展开这些引用，因此这里只能作为相邻研究传统提示，不能当作论文明确对比的 baseline。

7. 多模态方向的相关工作是 Visual OPST 的背景。论文把同一套自生成问题、特权信息和 on-policy self-distillation 扩展到 vision-language models，并在 VQA 上评估。相关研究传统包括 VLM 的指令微调、视觉问答蒸馏和多模态 self-training；论文的贡献在于把“无人工 QA”的自我训练循环搬到图文输入，而不是提出新的视觉编码器或对齐目标。

Q3: 论文如何解决这个问题？

1. 问题形式化。论文定义 data-free post-training：没有 curated dataset D={(xi, y*i)}，也没有 curated prompt distribution D_prompts={xi}，只给一组粗粒度主题描述 T={τ1,…,τK}，每个 τk 是一个宽泛学科名，例如 Abstract Algebra 或 Commonsense Causal Reasoning。模型要从 T 生成合成训练集 D_syn={(xi, zi)}，其中 xi 是生成查询，zi 是特权信息，由 rationale 和 candidate answer 组成。训练目标是在种子领域提升推理，同时保留未自训练领域的能力。

2. 自生成课程。模型根据主题描述生成问题、推理 rationale 和候选答案。这里的“课程”不是人工安排的难度序列，而是模型自己合成的问题分布；主题描述只提供领域锚点，不提供具体题目或答案。论文的方法节把这一步放在 3.3 节，强调生成的 privileged information 是“完全合成”的，后续教师条件化使用的就是这个自生成答案与推理。

3. 内在筛选之一：伪验证（pseudo-verification）。论文先估计一个样本是否 well-formed。一个可操作的检查是把问题 x 改写成 x′，要求保持语义不变，然后在相同条件下求解，要求答案不变，即 â(x′)=a；一个良构问题应当在不同改写下给出相同答案。这个信号不需要 gold answer，但它只能排除一部分歧义或不可解样本，不能证明答案事实上正确。

4. 内在筛选之二：共识门（consensus gate）。伪验证估计“样本是否良构”，共识门估计“样本对当前学生是否有信息量”。具体做法是对生成问题做 on-policy rollouts，如果学生已经能稳定地复现同一答案，说明这道题太容易、对当前学生没有训练价值，就丢弃；保留那些学生尚未稳定掌握的问题。因为门控是相对当前学生评估的，随着学生变强，课程会自动向更难或更未掌握的区域移动。这个机制是本文“自生成课程”能够持续提供学习信号的关键。

5. 训练：非对称条件化的 on-policy self-distillation。学生只看到问题 x，教师看到问题 x 加上特权信息 z（rationale 与候选答案）。学生在自己访问到的状态上采样，教师在这些状态上给出 dense next-token 分布，学生通过蒸馏匹配教师。论文把散度 D 实例化为 skewed Jensen–Shannon divergence，并用插值混合分布 q̄ 调节教师与学生分布之间的偏斜；具体 α 取值与完整公式需要回原文核对。关键设计是：教师条件化在生成答案上，而不是把生成答案当作学生要拟合的硬目标。这样教师可以在学生省略推理步骤时“补全”推理，同时降低噪声答案作为逐 token 目标带来的伤害。

6. 多轮自我训练。生成、筛选、训练可以重复多轮：上一轮训练后的模型生成新一轮问题与特权信息，共识门相对更新后的学生重新评估，再训练。论文在 3.6 节把这一过程描述为 multiple rounds of self-training。多轮的理论收益是课程随能力更新；风险是错误累积、分布收缩和计算成本上升，论文的 limitation 也承认需要 label-free stopping criteria。

7. 扩展到视觉语言模型。Visual OPST 把同一框架用于 VLM：主题描述对应视觉问答领域，模型生成图文问题与特权推理，经过内在筛选后做 on-policy self-distillation。论文在结果节报告 VQA 上的自提升，用来说明框架不局限于纯文本。这里的具体视觉数据构造、图像来源和筛选细节在提供的片段中没有完整展开，属于需要回原文补读的部分。

8. 设计权衡。与 SFT 相比，OPST 不把生成答案当硬目标，而是把它作为教师的条件上下文；与 GRPO 相比，OPST 提供 token 级密集信号而不是轨迹级标量奖励；与 OPD 相比，OPST 不需要更强外部教师；与 OPSD 相比，OPST 不需要 ground-truth solution。四组对比共同定义了它的方法位置：用自生成的特权上下文替代外部监督，同时保留 on-policy 状态分布与密集蒸馏信号。

Q4: 论文做了哪些实验？

1. 模型与基准。实验使用 Gemma 4 E2B 与 E4B。In-domain benchmarks 为 MMLU、GPQA、GSM8K、HellaSwag；数据生成以这四个领域的主题描述为种子。OOD 评估使用三个没有任何主题描述覆盖的基准，片段明确出现 ARC-Challenge，另外两个名称未在检索片段中完整保留，需要回原文核对。多模态部分在 VQA 上评估 Visual OPST。

2. Baselines。论文在同一批自生成数据上比较三类方法：SFT（dense target）、GRPO（sparse reward）、OPST（dense context）。表 1 的列还区分了是否使用 thinking 模式。结论节还声称 OPST 优于依赖 curated questions 的方法，但检索片段没有给出这些方法的完整名单与逐项数值，引用时需要谨慎。

3. Gemma 4 E2B 无 thinking 模式的主要结果：MMLU Base 37.5，SFT 38.3（+0.8），GRPO 37.3（−0.2），OPST 47.7（+10.2）；GPQA Base 28.8，SFT 30.8（+2.0），GRPO 30.3（+1.5），OPST 32.8（+4.0）；HellaSwag Base 48.7，SFT 50.8（+2.1），GRPO 50.3（+1.6），OPST 56.2（+7.5）；Average Base 38.3，SFT 40.0（+1.7），GRPO 39.3（+1.0），OPST 45.6（+7.3）。GSM8K 在该设置下未报告。

4. Gemma 4 E2B 有 thinking 模式的主要结果：MMLU Base 69.9，SFT 71.0（+1.1），GRPO 70.7（+0.8），OPST 71.1（+1.2）；GPQA Base 27.8，SFT 30.3（+2.5），GRPO 32.3（+4.5），OPST 34.3（+6.5）；GSM8K Base 72.7，SFT 75.4（+2.7），GRPO 75.0（+2.3），OPST 80.9（+8.2）；HellaSwag Base 63.1，SFT 64.5（+1.4），GRPO 62.5（−0.6），OPST 65.6（+2.5）；Average Base 58.4，SFT 60.3（+1.9），GRPO 60.1（+1.7），OPST 63.0（+4.6）。

5. Gemma 4 E4B 的结果在片段中只部分可见：Base 为 MMLU 54.3、GPQA 30.3、HellaSwag 53.7、Average 46.1；SFT 为 58.1（+3.8）、35.9（+5.6）、58.6（+4.9）、50.9（+4.8）；GRPO 只显示到 MMLU 55.8（+1.5），后续列被截断，不能据此计算完整平均或断言 OPST 的 E4B 增益。检索片段中另有一行包含 93.0+8.6、83.0+2.1 等数值，但列名缺失，不能安全归属到具体基准。

6. 跨域迁移实验。论文让不同模型分别只在各自 benchmark 的主题上自训练，再在 in-domain 与 out-of-domain 上评估。一个关键现象是：即使没有任何 MMLU 主题，MMLU 仍提升 5.1 分，说明部分增益不是种子领域特有的，可能存在从数学推理到通用推理的正迁移。这个结果也可能受到主题描述与评测域语义相近的间接引导，不能单独证明“完全领域无关的推理提升”。

7. OOD 保留实验。论文在混合多个数据生成领域时评估未自训练的 OOD benchmarks，结论是模型能够保留未自训练域的能力。检索片段没有给出全部 OOD 数值，因此无法核对每个 OOD 基准的升降幅度；从摘要与结论的措辞看，作者主张的是“retaining capabilities”而非“显著提升所有 OOD”。

8. 多模态实验。Visual OPST 在 VQA 上报告一致自提升，说明框架可以扩展到图文输入。提供的片段没有完整的多模态表格列名、图像数据来源和 baseline 细节，具体增益幅度与统计显著性需要回原文核对。

9. 消融缺口。方法节明确有两个内在筛选（伪验证与共识门）、教师条件化、多轮训练等组件，但检索片段没有包含完整的组件消融或超参数敏感性表。因此无法从现有证据判断：提升主要来自教师条件化、共识门课程、多轮迭代，还是三者共同作用；这是精读时应优先回原文补看的证据缺口。

Q5: 发现了什么实验现象？

1. 最核心的现象是“信号形态”比“数据量”更重要。在完全相同的一批自生成数据上，SFT 的 dense target 只带来小幅提升，GRPO 的 sparse reward 更不稳定甚至出现负增益，而 OPST 的 dense context 带来显著更大的平均提升。Gemma 4 E2B 无 thinking 模式下，Average 从 38.3 到 45.6（+7.3），对比 SFT 的 +1.7 与 GRPO 的 +1.0；有 thinking 模式下，OPST +4.6，对比 SFT +1.9 与 GRPO +1.7。这说明合成数据的价值不只取决于“有没有数据”，更取决于教师信号以什么形式进入训练。

2. 困难基准上的增益尤其明显。E2B 有 thinking 模式下 GPQA +6.5、GSM8K +8.2；无 thinking 模式下 HellaSwag +7.5。GPQA 是高难度科学问答，GSM8K 是数学多步推理，二者都不是简单的表面模式匹配，因此作者把结果解释为推理能力的提升，而不仅是领域知识记忆。

3. MMLU 上出现明显的“天花板/模式差异”。无 thinking 模式 Base 只有 37.5，OPST 提升 +10.2；有 thinking 模式 Base 已经 69.9，OPST 只提升 +1.2。这不等于 OPST 对 MMLU 无效，而是说明可用头部空间和评测模式会强烈影响相对增益。读者不应只看绝对提升，还要看 base 水平与饱和程度。

4. 跨域正迁移是一个反直觉但重要的现象。没有 MMLU 主题时，MMLU 仍提升 5.1 分，说明 self-training 可能改善了某种通用推理或应试能力，而不是只记住种子领域的题目。反过来，这也提示“in-domain 提升”不能完全归因于领域特定课程；其中可能混入了通用格式适应、推理链风格变化或评测污染等成分，需要更细的对照实验才能拆开。

5. 教师条件化在生成答案上、而不是把答案当硬目标，是解释鲁棒性的关键机制。若把可能含噪的生成答案直接作为 SFT 目标，错误答案会被逐 token 强化；OPST 让教师看到答案后生成分布，学生匹配的是教师在这些状态上的分布，教师有机会补全省略的推理步骤。这个设计可以降低噪声答案的直接影响，但不能消除错误：如果生成答案本身系统性错误，教师也可能围绕错误答案给出自洽的推理。

6. 共识门带来课程自适应，但也带来分布收缩风险。门控丢弃学生已经稳定答对的问题，保留下来的多是学生尚未掌握、可能更难或更歧义的样本。好处是训练信号集中在“最近发展区”；风险是随着训练推进，可用样本越来越少，或者保留下来的样本系统性偏向某些失败模式。论文的片段没有报告每轮保留率、难度分布和门控阈值敏感性，这些是判断课程是否健康的关键指标。

7. 负结果与失败模式在表中可见一部分：GRPO 在 E2B 有 thinking 模式的 HellaSwag 上为 −0.6，说明稀疏奖励在合成数据上并不稳定；SFT 虽然多数为正，但增益远小于 OPST。OPST 在报告的主表中没有明显负增益，但作者在 limitation 中明确承认 consistent errors 会进入训练，且方法只能放大已有知识。这意味着“没有负增益”不等于“没有错误累积”，只是当前评测没有把错误类型暴露出来。

8. 多模态结果支持框架通用性，但证据强度弱于文本。VQA 上的自提升说明同一套生成—筛选—蒸馏流程可以处理图文输入；然而片段没有展示视觉领域的失败案例、图像来源偏差、空间推理或计数类问题的分层结果。对于多模态自训练，读者应重点关注视觉 grounding 是否真的提升，还是模型只是学会了更符合答案风格的文本推理。

9. 规模趋势无法从现有片段判断。E4B 的 SFT 平均增益（+4.8）大于 E2B 的 SFT（+1.7），但 OPST 的 E4B 完整结果被截断，不能断言“模型越大，OPST 增益越大”。论文自己也承认实验限于 small models，因此 scaling 行为仍是开放问题。

Q6: 有什么可以进一步探索的点？

1. 无标签停止准则。多轮 self-training 最大的现实问题是何时停：继续训练可能放大自信错误、造成分布收缩或浪费算力；过早停止又浪费潜在增益。可以探索基于 held-out 自一致性、预测熵、教师学生分歧、问题保留率、跨轮性能平台期、参数更新幅度等内在信号的 stopping criteria，并用少量人工标注做校准。

2. 工具与检索接地的验证。当前 pseudo-verification 主要靠改写不变性与共识，衡量的是自一致性而非正确性。未来可以把代码执行、单元测试、形式化求解器、数据库检索、网页检索、科学计算工具接入验证环节，把“一致”升级为“可验证”。这对数学、代码、科学问答尤其重要，也最接近用户画像中的 ai-for-science 方向。

3. 更细的课程控制。共识门只做了一个粗粒度的“是否已经会做”的判断，未来可以研究难度分层、信息增益估计、主动学习式选题、对抗性问题生成、按能力维度覆盖的课程平衡。还可以研究门控阈值、每轮保留率、问题多样性约束对最终性能的影响，避免课程塌缩到少数失败模式。

4. 理论理解。为什么 on-policy self-distillation 能放大潜在能力，而在某些条件下会自我强化错误？需要从分布匹配、bias-variance、policy gradient 与蒸馏的差异、教师条件化的信息增益、共识门引起的分布偏移等角度给出解释。特别是要区分“学生学会了教师补全的推理”与“学生只是模仿了答案风格”这两种机制。

5. 规模律与模型规模。论文只做了 Gemma 4 E2B/E4B，未来需要在更大模型、更多领域、更长训练轮数上验证：OPST 的增益是否随规模上升、是否在强模型上饱和、是否会出现更强的自我确认偏差。也需要报告计算成本、每轮数据生成成本与教师前向成本，给出性价比分析。

6. 持续学习与遗忘控制。OPST 的一个卖点是保留未自训练域能力，但现有证据主要是几个 OOD benchmark。未来可以研究多领域顺序自训练、模型合并、经验回放、参数正则化、LoRA 隔离等方案，系统测量灾难性遗忘与反向迁移。

7. 多模态与 agentic 扩展。Visual OPST 只覆盖 VQA；可以扩展到视频、GUI、机器人、文档理解、多步工具调用等场景。对 agent 而言，自生成任务、自生成特权轨迹和共识筛选天然适合“无环境奖励”的自我改进，但需要解决长程信用分配、环境非平稳、工具调用错误传播等问题。

8. 安全与评测。自训练可能放大偏见、幻觉和有害推理；需要研究安全过滤器、价值对齐约束、红队式自生成问题、以及自训练后的行为漂移。评测上要警惕合成数据与预训练语料的污染、benchmark 泄漏、以及“自一致性提升被误读为正确性提升”。

9. 混合范式。在可验证领域，OPST 可以与 RLVR 结合：用 RLVR 提供正确性奖励，用 OPST 提供 dense privileged context；在不可验证领域，则用工具或检索弱验证。还可以探索多模型互相提供特权上下文、跨模型蒸馏、以及把 OPST 作为持续预训练后的轻量后训练层。

10. 重新界定 data-free。当前主题描述仍是人工选择的，未来可以研究自动主题发现、从模型自身知识图谱中采样领域、用无监督聚类生成课程，向“真正零人工输入”推进；同时要诚实报告哪些环节仍有人工先验。

Q7: 总结一下论文的主要内容

这篇论文的主线可以概括为：把 on-policy self-distillation 从“需要 ground-truth 特权答案”推进到“特权信息也由模型自己生成”的 self-training 框架。作者先梳理 post-training 的谱系：SFT 提供密集人工示范但有 exposure bias 和能力侵蚀问题；RLVR 用自身 rollout 但只有稀疏标量奖励；OPD 结合 on-policy 状态与密集教师信号，却需要更强教师；OPSD 去掉更强教师，用同一模型在非对称条件下构造教师与学生，但仍依赖 ground-truth solution 或人工问题集。论文的切入点是：外部监督在这些范式里只是被转移，没有被消除，而整理这些监督的成本很高，例如数学语料的 process-level 标注可能需要约 80 万个人类步骤标签。

方法上，OPST 只给一组粗粒度主题描述，例如 Abstract Algebra 或 Commonsense Causal Reasoning；模型据此自生成问题、推理 rationale 和候选答案，形成合成训练集。生成之后有两道内在筛选。伪验证检查问题是否良构：把问题改写为语义等价形式，要求在相同条件下答案不变；如果答案随改写而变，说明问题可能歧义或不可解。共识门检查问题对当前学生是否有信息量：对问题进行 on-policy rollouts，如果学生已经能稳定复现同一答案，就丢弃；保留学生尚未稳定掌握的问题。因为共识门是相对当前学生评估的，随着学生变强，课程会自动向更难或更未掌握的区域移动。

训练阶段，学生只看到问题，教师看到问题加自生成的特权信息；学生在自己访问到的状态上采样，教师在这些状态上给出 dense next-token 分布，学生通过蒸馏匹配教师。论文把散度实例化为 skewed Jensen–Shannon divergence，并用插值混合分布调节教师与学生分布之间的偏斜。最关键的设计是：教师条件化在生成答案上，而不是把生成答案当作学生要拟合的硬目标。这样，教师可以在学生省略推理步骤时补全推理，同时降低噪声答案作为逐 token 目标带来的伤害。生成、筛选、训练可以重复多轮；Visual OPST 把同一框架扩展到视觉语言模型。

实验使用 Gemma 4 E2B 与 E4B，在 MMLU、GPQA、GSM8K、HellaSwag 上做 in-domain 自提升，并用 ARC-Challenge 等三个未覆盖主题的 OOD benchmark 测保留，多模态在 VQA 上评估。核心结果是：在同一批自生成数据上，SFT 的 dense target 和 GRPO 的 sparse reward 只带来有限提升，OPST 的 dense context 提升最大。以 E2B 无 thinking 模式为例，Average 从 38.3 到 45.6（+7.3），而 SFT 为 +1.7、GRPO 为 +1.0；有 thinking 模式下 OPST Average +4.6，SFT +1.9、GRPO +1.7。困难基准上，GPQA +6.5、GSM8K +8.2、HellaSwag 在无 thinking 模式下 +7.5。MMLU 在无 thinking 模式下 +10.2，但有 thinking 模式下 base 已 69.9，OPST 只 +1.2，显示明显的头部空间/饱和效应。

论文还报告了跨域迁移：即使没有任何 MMLU 主题，MMLU 仍提升 5.1 分，说明部分增益不是种子领域专属，可能存在从数学推理到通用推理的正迁移；但这也可能混入主题描述语义引导、格式适应或评测污染，需要更细的对照。OOD 方面，作者主张混合领域自训练能够保留未训练域能力，而非造成灾难性遗忘；多模态方面，Visual OPST 在 VQA 上报告一致自提升。

论文的贡献可以概括为四点：第一，把 OPSD 重构为 data-free self-training，只靠主题描述驱动自生成课程；第二，提出伪验证与共识门两道内在筛选，在没有 gold label 的情况下同时估计良构性与信息量；第三，用“教师条件化于生成答案而非直接监督答案”的 dense context 蒸馏，把合成特权信息转化为密集监督；第四，在文本与 VQA 上验证框架的跨域与多模态通用性，并在同数据对比中显示其优于 SFT 与 GRPO。

局限也很明确。OPST 只能放大已有知识，不能凭空创造新能力；伪验证衡量自一致性而非正确性，一致错误仍会进入训练；主题描述是人工选择的，因此并非绝对零人工；实验限于小型模型，scaling 行为未知；多轮训练缺少 label-free 停止准则，存在错误累积与分布收缩风险；多模态证据目前主要是 VQA，视觉 grounding 的细节和失败模式仍需补足。总体而言，这篇论文的价值不在于证明“完全无监督自我进化”已经实现，而在于给出一个可操作、可复现的中间路线：把外部监督压缩到最粗粒度的领域描述，把其余部分交给自生成课程、内在筛选和 on-policy 密集蒸馏，并用同数据 baseline 把“信号形态”的贡献单独拎出来。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的 generation 方向直接相关：论文把“数据生成”做成了训练循环的一部分，主题描述→自生成问题→内在筛选→蒸馏→再生成，是一套可复用的合成数据引擎范式。

## 基本信息

- 作者：Leon Sick, Lukas Hoyer, Yannick Strümpler, Christian Reisswig, Timo Ropinski, Federico Tombari
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-26
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.32770`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Method、Results 与 Conclusion 片段，并对片段未覆盖的消融、部分 OOD 数值和多模态表格列名保留了证据缺口说明。
