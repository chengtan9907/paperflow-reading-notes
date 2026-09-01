---
user_id: "cheng tan"
paper_id: 9649
arxiv_id: "2608.28460"
title: "LayerRecall: A State-Conditioned Memory Router for Long-Horizon Consistency in Video Generation"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.28460.pdf"
pdf_url: "https://arxiv.org/pdf/2608.28460"
abs_url: "https://arxiv.org/abs/2608.28460"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:12:57"
---
# LayerRecall: A State-Conditioned Memory Router for Long-Horizon Consistency in Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：autoregressive video generation · diffusion transformer · long-horizon consistency · memory router

## 一句话总结

本文提出 LayerRecall——一种以当前生成为条件、按层选择的历史 K/V 记忆路由器，将检索到的历史状态仅注入对记忆敏感的 DiT 层，并用跨视角预测匹配（CHPM）以特权长上下文参考在预测空间监督训练，从而在不牺牲局部连续性的前提下显著提升自回归视频生成的长程一致性。

## 摘要

> Autoregressive video diffusion enables scalable long-video generation by producing chunks from a bounded recent context. While recency-based caching preserves local continuity, it evicts historical cues needed when subjects, objects, scenes, or attributes reappear. Existing memory mechanisms expose models to nonlocal history, but access alone does not ensure effective use. Our analysis reveals that video DiT layers exhibit distinct preferences for current, recent, and distant context, suggesting that long-range memory requires deciding both what to retrieve and where to use it. We introduce LayerRecall, a current-conditioned, layer-selective memory router that retrieves relevant historical K/V states and injects them only into backbone-specific memory-sensitive layers while preserving local attention elsewhere. To reduce reliance on scarce high-quality long-horizon videos and explicit memory-allocation labels, we further propose Cross-Horizon Prediction Matching (CHPM), which uses a privileged long-context reference to supervise the bounded-memory router in prediction space. Across 100 multi-shot evaluation prompts, LayerRecall achieves the best overall results on MemoBench and MovieBench while matching its backbone on VBench-Long, demonstrating stronger long-range recovery without sacrificing local continuity. Qualitative analyses further reveal memory-guided self-correction, whereby initially mismatched local attributes return to their historical appearance without resetting ongoing motion or scene structure. Additional analyses show cross-backbone portability and negligible inference overhead.

Q1: 这篇论文试图解决什么问题？

这篇论文聚焦于自回归视频扩散（autoregressive video diffusion）框架下的长视频生成一致性问题。该类方法通过将视频划分为多个块（chunk），每个块只依赖有界的最近上下文（bounded recent context）进行生成，从而将长视频生成扩展到任意长度，并天然保证了局部连续性。但这种基于“最近优先”的缓存策略存在结构性缺陷：当某个主体、物体、场景或属性在视频后期重新出现时，早期出现过的关键历史线索已经被驱逐出窗口，模型无从恢复其原始外观或状态。例如，一个角色在视频开头出现、中间消失、结尾再次登场时，模型只能基于最近几帧的信息来重新构建该角色，容易产生外观漂移或身份不一致。

已有记忆增强方法尝试缓解这一问题，例如通过压缩的全局状态（compressed global states）、在线参数记忆或记忆专家（memory experts）等方式保留超出局部窗口的信息。但论文明确指出一个关键观察：把非局部历史暴露给模型并不等于模型会有效利用它们（access alone does not ensure effective use）。换句话说，即使历史 K/V 状态被塞进上下文中，模型可能不知道在何时、何地、以何种权重去参考这些信息；不加区分地注入全部历史甚至会干扰局部时序建模，造成注意力被远距离不相关信息分散，反而损害生成质量。

论文进一步通过分析揭示，视频 DiT（Diffusion Transformer）的不同层在时序注意力中表现出对当前上下文、近期上下文和远期上下文的不同偏好：某些层主要负责局部运动连贯性，主要依赖当前与邻近帧；另一些层则承担更全局的语义一致性职责，需要访问远端历史信息。这说明长程记忆的利用不是一个简单的“多存点历史”的问题，而是一个需要同时决策“检索什么”（which historical K/V states are relevant to the current chunk）与“注入到哪里”（into which backbone layers should they be injected）的耦合问题。逐层统一注入策略既浪费计算，又可能引入噪声，导致模型无法在局部连续性与长程一致性之间取得平衡。

此外，训练这种记忆路由器还面临数据与标注的双重瓶颈：高质量的长视界（long-horizon）视频数据稀缺，且显式标注“某一步应该使用哪段历史记忆”的标签几乎不存在。因此，论文不仅需要设计一个层感知的记忆路由机制，还要解决在弱监督条件下如何训练该机制的问题。这正是 LayerRecall 与 CHPM 所要回应的核心挑战。

Q2: 有哪些相关研究？

从检索到的证据片段来看，论文的 related work 主要覆盖两条研究线，可以结合摘要与常识背景进一步组织。

1. 自回归视频生成（Autoregressive Video Generation）：Diffusion Transformers（Peebles and Xie 2023）已经成为高质量视频生成的常见骨干架构。传统的双向扩散公式通常把一段固定剪辑作为一个整体处理，难以直接扩展到任意长度的视频。近年来的一种路径是采用自回归方式，将长视频切分为连续块，逐块生成，同时维护一个有限的近期上下文缓存。这类方法在计算上可行，且天然有助于保持局部时序连贯性，但其内存管理主要基于最近优先的驱逐策略，缺乏对长期一致性的显式保障。论文正是在此基础上提出记忆路由机制。

2. 记忆增强视频生成（Memory-Augmented Video Generation）：为了保留超出局部窗口的信息，近期研究提出了多种记忆机制，包括压缩全局状态（compressed global states，如 Chen et al. 2026b、Yu et al. 2025b 相关方向）、在线参数记忆或记忆专家（online parameter memories / memory experts，如 Hong et al. 相关方向）等。这些方法试图把更长的历史信息编码进某种可查询的记忆结构中。但论文指出，这些工作主要关注“如何存储和暴露历史”，而较少关注“如何选择性地、按层地利用历史”，因此实际生成时历史信息往往被低效地使用。

3. 长视频生成扩展（Long-Horizon Video Generation）：另一条研究线将预训练的短视频模型扩展到长视频场景，通常涉及窗口滑动、噪声调度调整、时序注意力修改等工程手段。这类方法在保留局部质量的同时，往往缺乏对远端跨场景一致性的显式建模，且长视频训练数据与评估基准的匮乏也是公认瓶颈。

需要说明的是，当前检索证据只捕获了 related work 的片段，具体方法名称、年份与细节并不完整（例如 Chen et al. 2026b、Yu et al. 2025b 及 Hong 等人的具体工作内容未能从片段中读取），因此上述描述部分基于片段与摘要的合理推断。若需精确梳理引用关系，建议回原文核对 related work 全文。

Q3: 论文如何解决这个问题？

论文解法分为两个核心组成部分：LayerRecall 记忆路由器与 Cross-Horizon Prediction Matching（CHPM）训练策略。

LayerRecall：论文提出一个以当前生成为条件（current-conditioned）的层选择性记忆路由器。在自回归视频扩散生成每个新块时，路由器接收当前块的条件信息，从历史记忆中检索相关的 K/V 状态（键值对），并决定将这些历史状态注入到 backbone 的哪些层。关键在于“层选择性”：不是把所有检索到的历史状态平均地注入所有层，而是只把它们注入到 backbone 中对记忆敏感的层（memory-sensitive layers），其余层保持原有的局部注意力计算不变。这样既能为需要全局语义信息的层提供远距离上下文，又避免干扰负责局部运动建模的层。检索与注入决策是由当前生成状态条件化的，意味着模型可以根据当前块的具体内容动态决定需要回忆哪些历史信息。

CHPM：为了在缺乏高质量长视频和显式记忆分配标签的情况下训练这个路由器，论文提出 Cross-Horizon Prediction Matching。其基本思想是利用一个“特权长上下文参考”（privileged long-context reference）作为教师信号：该参考模型能够访问完整长上下文，因而具备更准确的预测能力。在训练时，LayerRecall 作为学生模型，其记忆路由器在有限 K/V 缓存预算下进行预测；训练目标是在预测空间（prediction space）让学生的预测逼近教师的预测，从而引导学生路由器学会近似长上下文模型的预测行为，而无需显式标注“应该把哪段历史注入哪一层”。这种蒸馏式思路既回避了对长视频数据的强监督依赖，也绕开了记忆分配标签的缺失。

两个组件是耦合的：CHPM 负责教会路由器何时检索、按什么权重检索；LayerRecall 的结构则保证即使在有界物理 K/V 缓存下，检索到的历史信息也能被精确投递到最需要的层。整体框架在有界内存预算下工作，不会将全部历史一直保留，而是通过路由器做按需的、层感知的访问。

Q4: 论文做了哪些实验？

根据摘要与检索到的证据片段，论文的实验设计大致围绕以下维度展开，但具体的实验表格、超参数、基线列表等细节在本次检索材料中并未完整呈现，以下描述以论文已明确报告的内容为主，并以“合理推断”标注部分推测。

1. 层偏好分析：论文首先通过分析揭示视频 DiT 各层对当前/近期/远期上下文的不同偏好，这一分析是 LayerRecall 设计动机的直接来源。合理推断：该分析可能包括对不同层注意力权重随时序距离的变化统计、或不同层在访问历史记忆时的性能影响测试。

2. 长程一致性基准评估：论文在 MemoBench、MovieBench 和 VBench-Long 三个基准上评估了 LayerRecall。摘要明确提到“100 multi-shot evaluation prompts”，表明评估面向多镜头（multi-shot）视频提示词，即需要跨镜头保持主体、场景和属性一致的任务。在这 100 个提示词上，LayerRecall 在 MemoBench 和 MovieBench 上取得了最佳总体结果，而在 VBench-Long 上与 backbone 持平。

3. 局部连续性检查：论文强调长程一致性提升不以牺牲局部连续性为代价，VBench-Long 上与 backbone 持平的结果可以理解为局部质量保持在基线水平。

4. 定性分析：论文展示了 memory-guided self-correction 现象，即生成过程中初始不匹配的局部属性随着生成推进而恢复到历史外观，且不重置运动或场景结构。合理推断：这通过由历史记忆引导的注意力注入实现，属于定性案例分析。

5. 跨 backbone 可移植性：论文报告了 LayerRecall 可迁移到不同 backbone，说明其层选择机制不是针对单一架构过拟合的。

6. 推理开销：论文报告可忽略的推理开销，表明记忆路由的额外计算成本很小。

需要指出的是：目前检索证据中并未包含具体数值结果（如 FVD、CLIP 分数、人工评估得分等），也未包含消融实验的具体设置与表格。若需要精确对比数据、baseline 列表、评估协议，应回原文核对 Experiments 章节。

Q5: 发现了什么实验现象？

论文所报告的关键实验现象可以归纳为以下几点：

1. 层偏好差异是真实且可利用的：论文观察到视频 DiT 不同层对当前、近期和远期上下文具有不同的敏感度。这意味着在长视频生成中，全局一致性的恢复并不是所有层都需要的，某些层更适合处理近程时序信息，另一些层则必须接收远距离历史才能维持语义一致。这是 LayerRecall 层选择性设计的基础，也解释了为什么简单地把全部历史暴露给所有层不能取得理想效果。

2. 选择性记忆注入优于统一暴露：LayerRecall 在 MemoBench 和 MovieBench 上取得最佳总体结果，而从 related work 片段可以看出，作者强调现有方法“低效是因为缺乏选择性（selective access）而非不能访问远距离历史证据”。这实际上是一个反直觉的结论：已有记忆增强方法把历史信息暴露给模型，但效果依然不好，说明“能访问”不等于“会使用”；访问的时刻与位置同样关键。

3. 长程恢复与局部连续性之间的张力可以被解除：LayerRecall 在长程一致性基准上领先，同时在 VBench-Long 上与 backbone 持平，说明它没有像一些记忆机制那样通过牺牲局部质量来换取长程一致性。这表明层选择性注入可以在不扰乱局部注意力建模的前提下增强远距离信息利用。

4. 记忆引导的自我修正（memory-guided self-correction）：定性分析显示，生成过程中如果局部属性（如人物外观）在一开始没有匹配历史记忆，后续在历史记忆的引导下可以“自我修正”回到历史外观，而且这种修正不会重置已经生成好的运动或场景结构。这个现象很有意思，说明历史记忆的作用不仅是初始化时的参考，还可以在生成过程中持续提供纠错信号。合理推断：这得益于路由器在当前块每个去噪步骤中都能动态访问相关历史 K/V，而不是一次性注入后就不再更新。

5. 可移植性与开销：跨 backbone 实验表明 LayerRecall 的机制可以在不同 backbone 上生效，说明其设计抓住了某种一般性的 DiT 记忆利用规律，而不是某个特定模型的 hack。推理开销可忽略则说明层选择性带来的收益并不是以显著计算代价换取的。

6. 负面/边界现象：摘要与检索证据未提及明显失败案例或负结果。但从问题设定可以合理推测，当历史信息与当前块完全无关或历史本身存在冲突时，路由器需要依赖 current-conditioned 判断来进行取舍，这一机制在面对“记忆污染”时的鲁棒性可能是一个潜在风险点，但论文未在本次检索材料中展开讨论。

Q6: 有什么可以进一步探索的点？

基于论文的设定与结果，以下方向值得进一步探索（部分为合理推断，需结合原文判断）：

1. 更细粒度的层选择与注意力头级路由：当前 LayerRecall 做的是层级的记忆注入决策，未来可以进一步细化到注意力头（attention head）级别。由于不同 head 可能承担不同的语义角色（如位置、外观、交互），头级路由有望带来更精确的长程一致性控制。

2. 动态记忆预算与自适应缓存管理：LayerRecall 在有界 K/V 缓存下工作，但缓存大小与驱逐策略仍有手动设定的成分。可以探索让路由器本身学习何时保留、何时压缩、何时驱逐历史状态，从而在固定内存下最大化信息保留。

3. 更强的当前条件编码：路由器依赖当前生成状态来检索历史。未来可以研究如何从当前块的隐状态中提取更丰富的查询信号，例如结合文本提示、物体轨迹或者 attention map 的语义线索，从而提升检索相关性。

4. 减少对特权长上下文参考的依赖：CHPM 在训练时使用特权长上下文作为教师信号，虽然绕过了显式标签，但仍然隐式地假设存在可以访问完整上下文的模型。未来可以探索自蒸馏、循环蒸馏或在线学习方式，让模型在没有特权教师的条件下也能逐渐学会长程记忆使用。

5. 扩展到更长视频与更大记忆规模：当前评估为 100 个 multi-shot 提示词，未来可以测试在更长视频（如数分钟级）条件下，层选择性机制在记忆检索复杂度与信息衰减方面的表现；也可以探索分层记忆——近期用高分辨率缓存、远期用压缩状态——与 LayerRecall 的组合。

6. 更深入的理论分析：论文观察到层偏好差异，但对“为什么 DiT 不同层会形成这种偏好”仍缺乏理论解释。未来可以通过 probing、因果干预、线性表征分析等手段研究 DiT 内部的时间感受野结构，进而指导更合理的记忆注入位置设计。

7. 泛化到其他生成任务：LayerRecall 的“where to use”思路具有一定的通用性，可以迁移到图像序列生成、3D 场景生成、音频-视频联合生成等需要跨时间步保持一致的任务。用户画像中如果重视 AI for science，这种“有界状态下选择性地利用长距离信息”的机制也可能对科学数据的时间序列建模有启发。

Q7: 总结一下论文的主要内容

这篇论文面向自回归视频扩散模型的长视频生成一致性挑战，提出了一套完整的“层感知记忆路由”方案。

论证主线：论文首先指出现有自回归视频生成系统普遍采用“最近上下文缓存”策略，这虽然使逐块生成在计算上可行并保持了局部连续性，但会把更早的历史线索驱逐出窗口，导致当人物、物体、场景或属性重新出现时，模型无法恢复其历史外观，从而产生长程不一致。随后论文分析了已有记忆增强方法，指出这些方法的主要贡献在于“把非局部历史暴露给模型”，例如通过压缩全局状态、参数记忆或记忆专家，但“访问本身并不能保证有效使用”——因为模型并不知道应该何时、以什么权重、在哪一层去利用这些历史信息。通过对视频 DiT 各层时序注意力的分析，论文发现不同层对当前、近期和远期上下文的偏好存在系统性差异，因此主张长程记忆管理必须从“层”的角度重新设计，同时回答“检索什么”和“在哪里使用”两个耦合问题。

技术主线：基于上述分析，论文提出 LayerRecall，一个以当前生成为条件、层选择性的记忆路由器。它从有界历史 K/V 缓存中检索与当前块相关的状态，并将这些状态仅注入到 backbone 中对记忆敏感的层，其他层继续使用局部注意力。这种设计在保留局部运动建模能力的同时，为需要全局语义的层补充了远距离历史证据。为了训练这样一个路由器，论文提出 Cross-Horizon Prediction Matching（CHPM）：利用一个能够访问完整长上下文的特权参考模型作为教师，让 LayerRecall 在预测空间逼近教师的预测行为。该策略避免了直接依赖稀缺的高质量长视频数据与显式的记忆分配标签——路由器只需要学会“像长上下文模型那样预测”，而不需要人工标注每步该用哪段记忆。

实验主线：论文在 100 个 multi-shot 评估提示词上比较了 LayerRecall 与多个基线，结果显示它在 MemoBench 和 MovieBench 上取得最佳总体结果，同时在 VBench-Long 上与骨干模型性能持平，说明长程一致性提升不以牺牲局部连续性为代价。定性分析展示了 memory-guided self-correction：初始不匹配的局部属性在生成过程中可以回到历史外观，且不重置运动与场景结构。论文还报告了跨 backbone 的可移植性与可忽略的推理开销，表明方法的通用性和高效性。

整体而言，论文的贡献不仅在于提出一个新的记忆注入模块，更在于它重新定义了对“记忆利用”的思考方式——不是简单地存储更多历史，而是理解模型内部各层对历史信息的需求差异，并在正确的位置、以正确的方式提供历史信息。CHPM 则解决了在弱监督条件下学习这种复杂路由行为的训练问题。该工作为长视频生成中的一致性控制提供了一个有说服力的设计范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的“generation”方向直接重合，属于视频生成中长程一致性建模的前沿工作。

## 基本信息

- 作者：Yixuan Ding, Jiahao Kong, Wei Huang, Ruijie Quan, Yi Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.28460`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要与提供的 PDF 语义检索证据片段（Abstract、Related Works、CHPM 方法节、Conclusion），并基于这些证据进行归纳与标注推断；部分实验细节因证据不足未展开，已在对应字段中明确说明。
