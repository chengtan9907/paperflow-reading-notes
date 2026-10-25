---
user_id: "cheng tan"
paper_id: 14843
arxiv_id: "2609.38332v1"
title: "Hermes: Learning Contextual Reasoning Unlocks Test-Time Scaling"
institution: "根据作者邮箱（xinyul2@andrew.cmu.edu 和 mononito@amazon.com）及脚注（部分作者在 AWS 实习），主要机构为卡内基梅隆大学（CMU）与亚马逊 AWS（Amazon）；具体单位以原文为准。"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.38332v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38332v1.pdf"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-09T13:07:58"
---
# Hermes: Learning Contextual Reasoning Unlocks Test-Time Scaling

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：contextual reasoning · test-time scaling · multi-context reasoning · agent delegation

## 一句话总结

本文提出 Hermes 系列可配置 harness 与 Hermes-Learn 两阶段训练框架，将测试时扩展中上下文分配与重用的控制权从 harness 转移给模型，证明上下文推理是可学习的模型能力，训练后较小的开源模型也能随额外推理计算有效扩展。

## 摘要

> Test-time scaling improves model performance by allocating additional compute during inference. Using this compute effectively across multiple context windows requires deciding how to allocate fresh contexts and what information to carry between them. We call a model's ability to make these decisions contextual reasoning. Existing approaches largely prescribe these decisions through their harness; we instead shift them to the model. We introduce 1) Hermes, a family of simple, configurable harnesses that progressively varies model control over context allocation and reuse, and 2) Hermes-Learn, a two-stage framework for learning these capabilities. We find that capable models can exploit this flexibility to scale with additional inference-time compute, while smaller open-source models initially struggle to do so. Training with Hermes-Learn closes this gap, inducing adaptive contextual reasoning strategies that vary with both the problem and the progress of reasoning. These gains generalize across benchmarks and models, extrapolate beyond the inference-time compute seen during training, and transfer to complementary test-time scaling methods beyond Hermes.

Q1: 这篇论文试图解决什么问题？

论文试图解决的问题是：在测试时扩展的背景下，如何有效分配和复用多个上下文窗口中的推理资源。具体而言，长视野推理往往需要多个独立的上下文窗口，例如数千个 agent 在各自上下文中并行推理，再由某个 harness 协调哪些上下文被创建、哪些信息被保留。现有方法几乎都把这类决策预先编码在 harness 中，比如固定树搜索的分支数、固定的摘要策略或固定的聚合方式。这种做法有两个弊端：第一，模型本身无法根据问题难度和推理进展动态调整上下文使用方式，导致灵活性和上限受限；第二，较小的开源模型缺乏自主做出这些决策的能力，即使给了它们额外的计算预算，它们也不懂得如何利用多个上下文。此外，将多个窗口的信息通过 digest（消化）汇总后再用于下游任务，会面临跨多个后续上下文窗口的信用分配困难，难以通过最终结果端到端地学习。因此，论文的核心问题是：如何让模型自己学会“何时开辟新上下文、何时继续当前思路、把哪些信息带到下一个上下文”这类上下文推理能力，并让这种能力能够训练获得、泛化并迁移。

Q2: 有哪些相关研究？

论文属于测试时扩展与多上下文推理的交叉领域。相关研究大致可分为几类：第一类是基于 harness 的测试时扩展方法，例如并行采样多个推理链，再通过投票或汇总选择答案；这些方法通常由外部流程决定上下文如何使用，模型只负责生成内容。第二类是多智能体协调与分层推理，例如让多个 agent 在不同上下文中分工探索、互相验证或迭代细化，但 agent 的调用结构和信息传递通常由人设计或由高层控制器预定。第三类是上下文压缩与记忆方法，例如用摘要、反思或记忆槽把已有推理结果打包到新的上下文中，以减少重复计算，但这些方法往往固定了摘要形式。第四类是推理策略学习，例如用强化学习训练模型在单上下文内选择思考路径或调用工具，但很少处理跨上下文的分配问题。本文的独特之处在于将上下文分配决策本身作为学习目标，提出从 harness 完全控制到模型自主控制的连续谱系（Hermes），并配套训练方法（Hermes-Learn）。与已有工作在概念上最接近的是“自适应计算”类方法，但本文聚焦于多上下文窗口间的协调，而不是单窗口内的步骤数。

Q3: 论文如何解决这个问题？

论文提出一个两部分的解决方案：（1）Hermes，一系列可配置的 harness，用于测试时扩展；（2）Hermes-Learn，一个两阶段训练框架。Hermes 通过两种机制——delegation（委托）和 digestion（消化）——逐步将上下文分配和重用的控制权从 harness 让渡给模型。在 delegation 中，主 agent 在推理至某一步时可以决定启动子 agent，并将当前推理状态作为提示传入；子 agent 使用相同的 delegation 接口，因此可以递归地再启动新的子 agent，直到达到最大深度 D；达到最大深度后，子 agent 只能内联推理（inline reasoning）而不能再委托。这种设计把“何时扩展新上下文”交给模型自己判断。在 digestion 中，模型可以把已探索过的上下文中的信息进行压缩或摘要，再携带到新的上下文中，以支持后续步骤。论文显示，digestion 通过下游任务结果进行学习可能面临困难的信用分配，因此 Hermes-Learn 特别关注学习 delegation。Hermes harness 家族通过在 delegation 和 digestion 上设置不同的控制级别，形成一个从完全对接线（harness 决定一切）到完全由模型自主的谱系。Hermes-Learn 是两阶段框架：第一阶段学习 delegation 策略，可能通过指令微调或强化学习，让模型学会基于当前问题和已有推理选择是否委托、委托给谁（比如委托给另一个擅长某类子任务的角色）；第二阶段或并行地学习 digestion 策略，但由于信用分配问题，需要设计辅助信号或课程。训练目标是在保持单上下文内推理能力不下降的同时，提升多上下文下的扩展性。

Q4: 论文做了哪些实验？

论文在数学推理基准上评估 Hermes 和 Hermes-Learn。摘要中提到的基准包括 AIME 和 HMMT（数学竞赛集合），实验采用“AIME + HMMT Pooled”的合并评估方式。实验设计大体包括：1）比较不同 Hermes 配置（从完全 harness 控制到模型完全自主）在多个模型上的表现，重点观察模型性能随推理时计算量（例如可用的上下文窗口数量或总 token 预算）增加如何扩展；2）识别上下文推理差距：对比能力较强的前沿模型和较小的开源模型在同样 Hermes 配置下的扩展曲线，确认小模型是否无法有效利用额外上下文；3）使用 Hermes-Learn 训练较弱模型，再评估其在相同基准上的表现；4）泛化实验：把训练好的上下文推理策略迁移到未参与训练的基准任务上，检验跨基准泛化；5）外推实验：在训练时见过的推理计算量之外提供更大的计算预算，观察模型性能是否继续随计算提升；6）迁移实验：将 Hermes-Learn 训练得到的策略能力应用于 Hermes 之外的其他测试时扩展方法（例如自一致性或 Best-of-N），检验迁移性。由于原文未提供完整的实验表格和具体数字，上述实验环节是基于摘要和结论的合理推断；具体协议和结果需以原文为准。

Q5: 发现了什么实验现象？

从摘要和结论可以归纳出以下实验现象：1）能力较强的前沿模型在 Hermes 提供的更大控制权下，能够随额外推理计算量的增加而持续扩展性能；2）较小的开源模型即使获得相同的额外计算和 Hermes harness 灵活性，也不能自动转化为性能提升，说明它们存在明显的上下文推理差距；3）使用 Hermes-Learn 训练后，小模型的差距被大幅缩小或消除，说明上下文推理是一件可学习的技能，而不是先天能力；4）训练诱导出的上下文推理策略是自适应的，会随着问题类型和推理进展发生变化，例如在简单问题上可能更快停止扩展，在复杂问题上主动启动子 agent 或反复消化中间结果；5）这些收益具有泛化性：不仅能泛化到训练时未见过的数学基准，也能迁移到不同模型家族和不同模型规模，表明习得的策略具有一定通用性；6）模型在训练中见过的推理计算范围之外仍能继续受益于更多计算，说明不是简单地记忆了计算规模与策略的对应关系；7）从 Hermes 中习得的上下文推理能力可以迁移到其他测试时扩展方法（如自一致性等），说明该能力不是绑定在特定 harness 上的。另一个值得注意的现象是纯数学推理（即单上下文内的推理步骤）本身不足以实现多上下文的收益，必须配合上下文推理能力。

Q6: 有什么可以进一步探索的点？

论文在结论中明确指出当前 Hermes 的层级（steering hierarchy）是手动指定的，未来工作可以：1）学习新的上下文推理策略，而不是依赖预设的层级，例如让模型自主设计新的 delegation 和 digestion 组合；2）动态组合不同策略，根据问题类型或实时推理状态在多种上下文策略间切换；3）在推理过程中自适应调整模型控制的级别，即让模型自己决定何时接管更多控制权或何时交给 harness；4）深入学习 digestion 策略，因为当前主要聚焦于 delegation，而 digestion 端到端学习存在信用分配困难；5）将 Hermes-Learn 扩展到更多模型家族和更大规模，验证其可扩展性；6）把上下文推理应用到数学以外的领域，如代码生成、科学推导、长期决策等；7）在更接近真实应用的设置下研究上下文推理，例如与外部工具包交互、多模态信息携带等。从方法角度看，还可以探索用强化学习替代监督式微调，利用过程奖励或更细粒度的反馈来缓解 digestion 的信用分配问题。

Q7: 总结一下论文的主要内容

这篇论文针对测试时扩展的核心操作问题展开：当推理计算预算从单上下文扩展到多上下文时，谁来决定新上下文如何生成、哪些旧信息如何保留？作者将模型在这种跨上下文协调中的决策能力命名为“上下文推理”（contextual reasoning），与传统的“数学推理”（in-context reasoning）区分开来。数学推理是在给定上下文内逐步推导出答案；上下文推理则是在多个上下文之间规划、分配和复用信息。论文的核心主张是：上下文推理不应由外部 harness 硬编码，而应是模型本身可学习、可调用的能力。

技术上，论文提出 Hermes 作为一族简单、可配置的 harness，通过在两个维度上逐步增加模型控制权来构造谱系：一是 delegation（委托），即主 agent 在需要时可以启动子 agent，把当前推理状态交给子 agent 在独立上下文中继续，并且子 agent 也能递归再委托，直到最大深度 D；二是 digestion（消化），即把先前上下文的信息压缩或摘要后带入新的上下文。Hermes 的不同配置对应着从 harness 完全控制（例如固定树结构）到模型完全自主（模型可自由决定是否委托、何时委托、委托给谁）的中间状态。为了让模型习得这种控制能力，作者提出 Hermes-Learn 两阶段训练框架：第一阶段学习 delegation 决策，第二阶段或同时学习 digestion 策略。由于 digestion 通过下游结果进行学习存在跨上下文的信用分配困难，论文明确将学习重点放在 delegation 上。

实验部分，作者在 AIME 和 HMMT 等数学竞赛基准上构建评估套件，比较不同规模模型在不同 Hermes 配置下性能随推理计算扩展的曲线。主要发现是：前沿模型能够利用 Hermes 提供的灵活性，实现与额外计算量匹配的性能提升；而较小的开源模型即使把灵活性给到它们，也不懂得使用，形成明显的上下文推理差距。Hermes-Learn 训练填补了这一差距，使得小模型学会根据问题难度和当前进度动态决定是否扩展上下文、是否委托子 agent。训练后的收益跨基准、跨模型家族和规模都能泛化，也能外推到训练时未覆盖的更大计算预算，并且迁移到 Hermes 之外的测试时扩展方法上，说明习得的是通用能力而非特定 harness 的记忆。

论文最后将上下文推理确立为一种可学习的模型能力和长期测试时扩展的重要组件。其局限包括 harness 层级仍需人工设定、digestion 学习不足、当前实验集中在数学推理领域等，作者还把未来方向指向学习更开放的控制策略、学习 digestion、以及扩展到数学之外的领域。整体上，这项工作重新定义了多上下文推理中的“控制权归属”问题，并用一套系统的工具和方法论证了模型自主上下文协调的可行性与价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与智能体（agent）方向直接相关，其 delegation 机制本质上是一种分层多智能体协调方法，可启发 agent 的设计。

## 基本信息

- 作者：Xinyu Li, Mononito Goswami, Hao Liu, Nikos Kanakaris, Langlin Huang, Prithwish Jana, Patrick Blöbaum, Purak Jain
- 机构：根据作者邮箱（xinyul2@andrew.cmu.edu 和 mononito@amazon.com）及脚注（部分作者在 AWS 实习），主要机构为卡内基梅隆大学（CMU）与亚马逊 AWS（Amazon）；具体单位以原文为准。
- 来源：arxiv
- 主题/分类：high_relevant
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38332v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的证据片段，并结合摘要、Introduction 与 Conclusion 内容进行归纳；部分实验细节因原文不可得而标注为合理推断。
