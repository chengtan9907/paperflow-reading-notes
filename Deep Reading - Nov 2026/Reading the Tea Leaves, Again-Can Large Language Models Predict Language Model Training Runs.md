---
user_id: "cheng tan"
paper_id: 22665
arxiv_id: "2611.30141v1"
title: "Reading the Tea Leaves, Again: Can Large Language Models Predict Language Model Training Runs?"
institution: "卡内基梅隆大学（Carnegie Mellon University，CMU；依据通讯作者邮箱 andrew.cmu.edu 推断）"
publish_date: "2026-11-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.30141v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.30141v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-28T01:33:40"
---
# Reading the Tea Leaves, Again: Can Large Language Models Predict Language Model Training Runs?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：training run forecasting · learning curve extrapolation · llm agents · benchmark

## 一句话总结

论文提出 Tea Leaves 基准，用 682 条真实训练 run 检验 LLM 能否从 20% 早期曲线、配置、描述和代码预测完整训练轨迹，并系统比较 11 个 LLM 与曲线拟合、时间序列基线，揭示语义信息带来的收益、日志稀疏性的上限以及失败 run 预警的缺口。

## 摘要

> LLM agents now propose research ideas and run the experiments that test them, but they train every run to its end, whereas a researcher reads a run's configuration and the beginning of its curve and stops the runs that show no promise. Reading a run this way takes more than describing or implementing a training method: it takes knowing what a design choice does to a run. We build Tea Leaves to measure this knowledge on 682 real training runs across pretraining, fine-tuning, and reinforcement learning (RL): a forecaster sees the first 20% of a run with its configuration, description, and code, and predicts the rest. Across 11 LLMs and a range of forecasting baselines, the strongest LLMs match or exceed the best curve fitter on smooth loss curves (skill 0.66 against 0.62) and lead on noisy RL reward curves (0.43 against 0.13). What bounds every forecaster is what the log carries: on sparsely logged community runs all methods lose most of their skill, and none reliably flags a failing run in advance. Beyond the curve, information helps and effort does not: related runs in the prompt cut the error on loss curves by 39%, and knowledge of training dynamics lets the strongest LLMs order all 36 pairs of runs that differ by a described code change, which the early curves do not separate, whereas tools add little. This knowledge can be learned from logged runs: a small open model trained on public runs matches the best LLM on loss curves. Applied to the progress curves of research agents, the same reading ranks tasks by their remaining headroom, although for a single agent run it does not improve on the last value. We release the benchmark, the trained readers, and their training data to test and train LLMs' understanding of training and to help research agents decide which runs deserve their compute.

Q1: 这篇论文试图解决什么问题？

【核心问题】论文要回答的不是“LLM 能否解释 Adam 或实现 learning-rate warmup”，而是“给定一个训练 run 的配置、代码、实验描述和最初 20% 的曲线，LLM 能否像有经验的研究者一样预测这条曲线接下来会怎么走，并据此判断这个 run 是否值得继续投入算力”。作者把知道一个训练组件分为三层：描述它、实现它、预判它对 run 的影响；Tea Leaves 测的是第三层。这个问题的核心不是曲线拟合本身，而是设计选择与训练动态之间的条件关系：学习率、优化器、数据、并行策略、代码改动分别会把曲线推向哪里。
【为什么现在重要】LLM agents 已经能提出研究想法、写代码并执行实验，但当前 agent 和评测通常把每个 run 训练到结束。真实研究者会“babysit”一个 run：读配置和代码，看曲线的前一段，如果看不到希望就提前停掉，把算力留给下一个想法。缺少这种判断，agent 会为每个 run 全额付费，研究循环的算力效率很低。论文进一步指出，这种能力对“能改进自身训练”的系统也重要：提出优化器或 schedule 改动很便宜，用完整训练去学习其效果很贵。
【现有评测的缺口】解释和实现是静态知识，不能证明模型知道组件对 run 的因果影响；研究想法评估工作预测“想法是否有效”或“两个提案谁得分高”，但不读执行后的曲线；ML 工程 agent benchmark 以最终指标评分开环执行，奖励“把实验跑完”，不奖励“判断哪个 run 不值得跑完”；学习曲线外推和早停方法只看数值序列，不利用配置、代码和实验描述中的语义。Tea Leaves 试图把“实验 triage”从 agent 整体能力中隔离出来，作为一个可测量的预测任务。
【技术难点】第一，输入是异构的：数值曲线、结构化配置、自然语言描述、代码和可能的 lineage 必须联合使用。第二，曲线形态跨阶段差异大：预训练 loss 平滑，微调 loss 受数据/任务影响，RL reward 噪声大且非单调。第三，日志密度不一致：公开 run 可能记录密集，社区 run 稀疏，缺失点会直接破坏前缀和预测目标。第四，失败 run 的早期信号弱，失败定义本身可能主观。第五，真实 run 之间存在相关性和复用关系，模型可能记住相似 run 而不是理解设计选择。第六，评价需要同时覆盖绝对误差、排序、失败预警和决策价值，单一 skill 分数不够。
【评价难点与隐含假设】论文用 skill 聚合异质 run，这会掩盖分阶段差异；last-value 在噪声 RL 上是强基线，若没有正确对照，LLM 的优势可能被高估或低估；曲线预测准确不等于 stopping decision 正确，因为停止还涉及成本、风险和后续机会；20% 前缀的截断点本身是设计选择，换一个截断点可能改变结论；公开 run 可能被 LLM 预训练数据污染，代码/描述可能泄露结果；36 对代码变更排序虽然能检验方向性知识，但规模小，不能代表复杂代码改动；小模型在 loss 曲线上匹配最强 LLM，也不代表它在 RL 或失败预警上同样可用。论文的隐含假设是：20% 早期曲线加语义信息足以支持有意义的预判；公开 run 能代表私有训练；曲线误差/排序准确率是“理解训练”的有效代理。这些假设都需要在真实实验室的干预实验中进一步检验。
【边界条件】Tea Leaves 测的是“读曲线”而不是“做研究”；它不评估模型是否能在看到预测后选择正确的后续实验，也不评估停止决策带来的总算力节省。它覆盖 LM 训练，不外推到其他 ML 流程或科学实验。它的最强结论依赖于日志质量：当日志稀疏时，所有预测器都退化。因此，论文的贡献应理解为“把问题定义清楚并给出诊断工具”，而不是“LLM 已经可以替代研究者做 early stopping”。

Q2: 有哪些相关研究？

【研究智能体与想法评估】论文首先接入的是 LLM 研究智能体这条线：Lu et al. 2024、Yamada et al. 2025、Jansen et al. 2025、Schmidgall et al. 2025 让 agent 提出研究想法并执行；Wen et al. 2025、Luo et al. 2025b、Park et al. 2025 则训练/评估模型预测研究结果，比如一个想法是否有效、两个提案谁得分更高、一项研究可能发现什么。这些工作关注“想法层”的判断，默认执行阶段会把 run 跑完；Tea Leaves 把评价下沉到“执行中的曲线”，问模型能否从早期曲线和实验语义判断这个 run 是否值得继续。
【ML 工程与实验自动化基准】Huang et al. 2024、Chan et al. 2025、Nathani et al. 2025 以及 Si et al. 2025/2026 的 agent 基准，通常要求 agent 实现方法、运行实验、达到某个最终指标。它们衡量的是“能不能做出来”，不是“能不能提前判断做不做得出来”。Tea Leaves 与这些基准互补：它不要求 agent 写完整训练脚本，而是要求它读一个已经开始的 run，预测后续并做 triage。
【学习曲线外推与早停】经典方法是参数化曲线拟合、Bayesian optimization、successive halving/Hyperband 等多保真度优化，以及 learning-curve prior fitted network（LC-PFN）。论文还把 pool-trained shape regressor 和 kNN on prefix shape 作为基线：前者在池化 run 上学习曲线形状，后者按前缀形状找近邻。片段显示，shape regressor 预测的是“未来值除以上一次观测值”的比例，而不是原始值；这是一种让不同尺度 run 可比较的归一化。Tea Leaves 与这些方法的关键区别是：它把配置、代码、描述和相关 run 作为条件，测试语义知识是否超出纯数值外推。
【时间序列基础模型与 LLM 数值预测】TimesFM-2.5 等时间序列基础模型、LLM processes（Requeima et al. 2024）以及 Gruver et al. 2023 的 LLM 数值预测，提供了通用序列预测能力。论文将它们作为 zero-shot 基线；片段显示 TimesFM-2.5 接收十个原始点并预测 40 个 2% 步长的点，其上下文落在单个输入 patch 内。通用预测器可能擅长平滑曲线，但不一定理解“改学习率/换优化器/改代码”对曲线的方向性影响。
【LLM 对训练动态的知识】现有 LLM 训练知识评测多问“解释一个组件”或“实现一个方法”，这两层知识可以靠记忆和代码能力完成，不能证明模型知道组件对 run 的影响。Tea Leaves 的第三层知识——预判设计选择的后果——更接近研究者的实验判断。论文还用“代码变更后的 36 对 run 排序”来检验这种知识：早期曲线无法区分，但强模型在知道变更描述后能正确排序，说明它可能编码了方向性训练动态。
【定位】Tea Leaves 不是新预测器竞赛，而是诊断性 benchmark：它系统比较 11 个 LLM、经典曲线拟合、时间序列基础模型和小型训练 reader，并做信息/工具/训练干预。它的 novelty 在于任务定义、数据覆盖和“信息比工具更重要”的结论，而不是某个新模型结构。

Q3: 论文如何解决这个问题？

【任务定义】Tea Leaves 把每个训练 run 表示为一个四元组：配置、实验描述、代码、早期曲线。预测器只看到前 20% 的曲线，任务是输出剩余曲线或未来检查点的数值。片段显示，曲线按 2% 的进度间隔记录，因此 20% 前缀约十个点，剩余约 40 个点需要预测；这是从 TimesFM 基线的输入描述推断的，具体分箱规则需回原文核对。评价用 skill 分数，数值越高表示相对基线预测越好；论文没有在摘要中给出 skill 的完整归一化公式，阅读时应核对它相对哪个 reference。
【数据】682 条真实公开 run，覆盖预训练、微调、RL 三类阶段。片段显示社区子集包含 232 条 run、184 位 owner，说明数据同时包含较密集的公开实验和较稀疏的社区日志；不同 run 的元数据完整度不均，模型规模、任务域等字段只对部分 run 可识别。论文按阶段划分 pool，并区分平滑 loss 与噪声 reward，以便观察预测器在不同曲线形态下的行为。
【输入与输出】LLM reader 接收配置、描述、代码和前缀曲线，可能以文本/表格形式序列化；输出未来检查点的数值。为了让不同尺度 run 可比较，部分基线预测“未来值/最后观测值”的比例；LLM 是否也采用这种归一化，需查原文。论文还测试了把相关 run 放入 prompt、提供代码变更描述、允许工具调用等条件，试图区分“模型参数里的知识”和“上下文提供的信息”。
【预测器与基线】基线覆盖几类：last-value（把最后观测值外推到未来）、参数化曲线拟合、时间序列基础模型 TimesFM-2.5（zero-shot）、LC-PFN、pool-trained shape regressor、kNN on prefix shape，以及 11 个 LLM reader。片段显示 TimesFM-2.5 使用 200M 模型、接收十个原始点并预测 40 个 2% 步长的点；LC-PFN 和 kNN 等按前缀形状做预测，且不按每个实验臂重新训练。LLM 侧既有直接数值预测，也有 agent 式变体：把同样材料作为文件夹，给模型文件访问和 Python sandbox，最多二十轮；另一个变体增加文献和代码搜索。
【评价】主指标是 skill，辅以误差、pairwise ordering、failure detection 和 headroom ranking。Pairwise ordering 用于检验模型是否知道两个 run 的相对好坏，即使绝对曲线预测不准；failure detection 问模型能否在 run 结束前标记失败；headroom ranking 用于研究 agent 的 progress curve，按任务剩余提升空间排序。论文还按曲线类型（平滑 loss/噪声 RL）和日志密度（公开密集/社区稀疏）分层报告，避免单一平均值掩盖异质性。
【训练 reader】论文训练小型开源 reader，用公开 run 作为监督数据，目标可能是归一化后的未来曲线；片段提到一个 2B reader 在 RL 上受影响，说明小型 reader 的规模在 2B 量级。关键结果是它在 loss 曲线上匹配最强 LLM，说明“读曲线”的知识可以从 logged runs 学习，而不只依赖闭源大模型。论文 release benchmark、trained readers 和 training data，支持复现和后续训练。
【干预与消融】信息干预包括：在 prompt 中放入相关 run、提供代码变更的描述、让模型知道训练动态；工具干预包括 Python、文件访问、文献/代码搜索。论文还比较不同 stage 的训练/池化设置，片段显示某些消融造成约 0.02–0.12 的 skill 变化，主要集中在 fine-tuning 和 RL，并使 2B reader 在 RL 上变差；具体消融项和方向需要回原文核对。整体设计想回答：提升来自信息、模型规模、训练数据，还是测试时工具/交互。

Q4: 论文做了哪些实验？

【主预测实验】在 682 条 run 上比较 11 个 LLM 与 last-value、曲线拟合、TimesFM-2.5、LC-PFN、pool-trained regressor、kNN 等基线。输入统一为前 20% 曲线加配置/描述/代码，输出剩余曲线，按 stage 和 pool 报告 skill。这个实验回答“LLM 是否超过纯数值外推”，并暴露模型之间的差异。
【曲线类型分层】论文把平滑 loss 曲线和噪声 RL reward 曲线分开报告。摘要给出的关键对比是：平滑 loss 上最强 LLM 0.66 skill，最佳曲线拟合器 0.62；噪声 RL 上 LLM 0.43，曲线拟合器 0.13。这个分层很重要，因为 RL reward 非单调、方差大，纯拟合更容易失败，语义知识可能更有价值。
【日志密度分层】论文比较较密集的公开 run 与稀疏的社区 run。社区子集片段显示 232 条 run、184 位 owner，日志稀疏且元数据不全。实验问：当曲线点变少、记录不规律时，LLM 和基线是否还能保持 skill。结果是所有方法在稀疏社区 run 上丢失大部分 skill，说明日志质量是共同上限。
【失败预警】论文测试模型能否在 run 结束前标记“这个 run 会失败”。这不是曲线预测的简单副产品：失败可能表现为曲线发散、平台化或 reward 不涨，早期信号弱。摘要说没有方法能可靠提前标记失败 run，这是一个重要的负结果，直接限制 autonomous early stopping 的安全性。
【信息干预】一组实验在 prompt 中加入相关 run，看是否能提升预测。摘要报告 loss 曲线误差降低 39%。另一组实验给模型提供代码变更的描述，构造 36 对只差一个代码改动的 run，问模型能否排序；早期曲线无法区分这些 run，但最强 LLM 在知道变更后能正确排序全部 36 对。论文还比较工具调用（Python、文件访问、搜索）是否提升，结论是工具收益很小。
【agent 式评测】片段显示，论文让 GPT-5.6 Sol 以 agent 方式在 100 条预训练、100 条微调、50 条 RL 问题上工作：同样材料作为文件夹，给文件访问和 Python sandbox，最多二十轮；另一个变体增加文献和代码搜索。这个设置区分“模型一次性预测”与“模型可以主动查文件、写代码分析”的 agent 能力。
【训练 reader】论文用公开 run 训练小型开源 reader，并在 held-out 上比较它和最强 LLM。摘要说小型模型在 loss 曲线上匹配最强 LLM；片段提到 2B reader 在 RL 上受影响，说明规模/阶段泛化仍是问题。这个实验检验知识是否可从 logged runs 学习，以及 release 的训练数据是否有用。
【研究 agent progress】论文把同样的“读曲线”用于研究 agent 的 progress curve：给定多个任务上 agent 的进展曲线，按剩余 headroom 排序任务。结果显示任务级排序有效，但对单个 agent run 的预测不优于 last-value。这个实验把 benchmark 从 LM 训练扩展到 agent 行为曲线，同时暴露“任务级排序”和“单 run 预测”的差异。
【消融与稳健性】论文还比较不同 stage 的池化/训练设置、形状归一化、近邻基线等。片段显示某些消融造成约 0.02–0.12 的 skill 变化，主要集中在 fine-tuning 和 RL；具体消融项需回原文核对。整体实验设计是多基线、多阶段、多日志密度、多干预的矩阵，而不是单一榜单。

Q5: 发现了什么实验现象？

【平滑 loss 上 LLM 略胜，但优势有限】最强 LLM 的 skill 0.66 对最佳曲线拟合器的 0.62，差距只有 0.04。这说明在平滑、单调的 loss 曲线上，经典曲线拟合已经很强，LLM 的配置/代码/描述知识只带来边际提升。论文的结论应读作“LLM 至少不输给强曲线拟合器”，而不是“LLM 大幅超越数值方法”。
【噪声 RL 上 LLM 优势扩大，但 last-value 仍是强基线】RL reward 曲线更噪声、更非单调，最佳曲线拟合器只有 0.13，LLM 达到 0.43。这个对比说明语义知识在噪声场景更有价值。但片段指出，在 RL 测试集上某些训练型 reader 没有超过 last-value，提示 last-value 在噪声曲线中非常难打败，且不同 reader 的 RL 泛化差异很大。摘要层面的“LLM 领先”不能推广到所有方法；需要区分 LLM reader、pool-trained regressor 和小型训练 reader。
【日志稀疏性压过模型能力】在稀疏的社区 run 上，所有方法丢失大部分 skill。这不是某一个模型的失败，而是输入信息量的硬约束：当前缀点太少、记录不规律、元数据缺失时，模型无法可靠推断后续。这个现象支持论文的核心论点“what bounds the reading is what the log carries”，也说明 benchmark 在密集公开日志上的高分可能高估真实社区场景。
【失败 run 预警是负结果】没有方法能可靠提前标记失败 run。这意味着即使曲线预测的平均误差可接受，autonomous agent 也不能安全地依据预测 kill run；错误 kill 的代价可能很高。失败预警可能比曲线预测更难，因为失败是稀有事件、定义主观、早期信号弱，且不同失败模式（发散、平台、reward 不涨）形态不同。
【相关 run 的信息收益很大】在 prompt 中加入相关 run 使 loss 曲线误差降低 39%。这说明模型可以利用“相似实验的结局”做类比，而不是只靠参数记忆；对 agent 系统而言，检索/lineage/实验记录比增加测试时工具更值得投入。这个结果也提示 benchmark 的“相关 run”构造需要防止泄漏：如果相关 run 与目标 run 太近，收益可能来自复制而非泛化。
【代码变更排序显示方向性知识】36 对 run 只差一个被描述的代码改动，早期曲线无法区分，但最强 LLM 在知道改动后能正确排序全部 36 对。这是一个很强的信号：模型可能编码了“改这个组件会让曲线变好/变差”的方向性知识，而不是单纯拟合数值。但样本只有 36 对，且改动是“被描述的”，不能外推到复杂、未描述的代码重构；工具调用收益很小，说明这种知识主要来自模型/上下文，而不是现场计算。
【知识可学习，但阶段泛化有限】用公开 run 训练的小型开源模型在 loss 曲线上匹配最强 LLM，说明“读曲线”不是闭源大模型独有的涌现能力，logged runs 可以提供监督。但片段提到 2B reader 在 RL 上受影响，说明小模型在噪声/RL 阶段泛化仍弱；训练数据的阶段覆盖和规模可能是瓶颈。
【研究 agent：任务级排序有效，单 run 预测无效】把同样读法用于研究 agent 的 progress curve，可以按剩余 headroom 排序任务；但对单个 agent run，预测不优于 last-value。这揭示了一个尺度差异：跨任务比较可能依赖任务难度先验，而单 run 的早期 progress 不足以预测最终结果。对 agent 调度，任务级排序已经有用（决定先做哪个任务），但不能据此判断某个 run 是否应该停止。
【信息与努力的非对称】论文的总括是“information helps and effort does not”：相关 run、代码变更语义等信息带来明显收益；工具、文件访问、搜索、更多交互轮次收益很小。这个现象对 agent 设计有直接含义：与其给模型更多工具和测试时算力，不如给它更好的上下文、相关实验记录和结构化实验语义。
【指标间张力】绝对误差低不等于排序正确；平滑曲线上的高 skill 不等于噪声 RL 上高 skill；密集日志上的排名不等于稀疏社区日志上的排名；任务级 headroom 排序不等于单 run 预测；小模型匹配大模型只发生在 loss 曲线。阅读时应把这些张力当作论文的主要发现，而不是把它们压成一个平均分。

Q6: 有什么可以进一步探索的点？

【日志与数据基础设施】论文最强的边界是日志。未来可以研究：在稀疏、不规则、缺失的日志上做插值/不确定性建模；设计对预测友好的 logging policy（记录哪些点、哪些元数据）；把配置、代码、环境、数据版本、lineage 结构化；评估日志密度与预测 skill 的 scaling relation。对真实实验室，这意味着“为了可预测性而记录”本身就是投资。
【失败预警与不确定性】当前没有方法可靠提前标记失败 run。未来需要定义失败类型（发散、平台、reward 不涨、过拟合、数值不稳定），收集稀有失败样本，做代价敏感的预警（false kill 的代价高于漏判），并输出校准的不确定性。可以借鉴生存分析、异常检测、early warning 和 risk-sensitive decision 的方法。
【代码变更的因果效应】36 对排序是方向性知识的强证据，但规模小。未来可以构造更大规模的“单变量代码变更”配对，控制其他因素，检验模型是否能预测绝对曲线而不只是相对排序；也可以研究模型是否真正理解代码语义，还是依赖变更描述中的关键词。反事实预测（如果换优化器/学习率/并行策略，曲线会怎样）是一个更严格的测试。
【相关 run 的检索与关系建模】相关 run 带来 39% 误差下降，说明关系信息很重要。未来可以研究：如何检索真正相关但不泄漏的 run；如何表示 lineage、超参搜索历史和失败 run；如何让模型比较多条曲线而不是拼接文本；如何做跨规模、跨数据、跨任务的类比。对 agent 系统，这可以变成“实验记忆”模块。
【训练 reader 与规模泛化】小型 reader 在 loss 上匹配最强 LLM，但在 RL 上弱。未来可以扩大训练数据、平衡阶段、研究模型规模/数据规模/曲线数量的 scaling；用蒸馏、continual learning、meta-learning 让 reader 适应新实验室的日志分布；研究 reader 的不确定性校准和跨领域迁移。
【从预测到决策】论文测的是预测，不是停止策略。未来需要把预测接入一个 cost-aware policy：给定剩余算力预算、run 的预期收益、预测不确定性，决定继续/停止/修改配置；在真实训练队列中做干预实验，测量总算力节省和错杀率。这需要把“曲线预测”与“后续实验选择”联合建模。
【RL 与多目标曲线】RL reward 噪声大、last-value 强，未来需要更好的统计基线、方差缩减、多 seed 聚合、置信区间预测；也可以预测多个指标（reward、KL、entropy、成功率）和约束，而不是单一曲线。对生成模型训练，loss、reward、下游评测之间可能不一致，需要多目标预测。
【agent 集成】研究 agent 的 progress curve 可用于任务级 headroom 排序，但单 run 不优于 last-value。未来可以把 Tea Leaves 作为 agent 的“实验 triage”模块，在 agent 循环中在线更新预测；研究 agent 何时应该停下来读曲线、何时应该继续跑；以及多 agent 之间如何共享实验记忆。
【评测有效性】需要检查预训练污染：公开 run 的配置/代码/结果可能已在 LLM 训练数据中；需要 held-out 实验室、时间切分、私有日志；需要人类专家基线，比较研究者的预测与模型预测；需要报告不同截断点（10%、20%、30%）的敏感性；需要把 skill 分解为数值外推、语义条件、记忆三个来源。
【跨领域扩展】Tea Leaves 目前覆盖 LM 训练。未来可以扩展到其他 ML 流程（数据清洗、检索、推理系统）、科学实验（湿实验、仿真）、系统调优（分布式训练、编译）等；这些领域的曲线语义不同，但“读早期信号决定是否继续”的问题结构相似。
【解释性与安全性】最后，需要理解模型到底学到了什么：是真正的训练动态，还是“大学习率通常更差”等浅层启发式；在分布外配置上是否会自信地给出错误预测；autonomous kill run 的失败模式是什么。可解释性、校准和 human-in-the-loop 是落地前提。

Q7: 总结一下论文的主要内容

一、论文在问什么
这篇论文把“研究者如何判断一个训练 run 是否值得继续”形式化为一个可测量任务。当前 LLM agents 能提出研究想法、写代码、跑实验，但通常把每个 run 训练到结束；人类研究者则会读配置、代码和曲线的前一小段，提前停掉没有希望的 run。作者认为，这种能力不是“解释 Adam”或“实现 warmup”，而是知道一个设计选择会把曲线推向哪里，即对训练动态的预判知识。论文用 Tea Leaves 基准测量这一层知识：给模型一个真实 run 的前 20% 曲线、配置、描述和代码，让它预测剩余曲线。
二、为什么现有评测不够
现有研究 agent 评测多把实验执行到终点，以最终指标评分；研究想法评估工作预测“想法是否有效”或“两个提案谁得分高”；学习曲线外推工作只看数值序列，不利用配置/代码语义；时间序列基础模型和 LLM 数值预测器是通用工具，不证明模型理解训练动态。Tea Leaves 的定位是诊断性 benchmark：它不是提出新预测器，而是把“读 run”拆成可比较的预测任务，并用多种基线、信息干预和 agent 决策场景来定位模型能力。
三、技术主线
数据侧，论文收集 682 条真实公开训练 run，覆盖预训练、微调、RL，并区分较密集的公开 run 与稀疏的社区 run（片段显示社区子集 232 条、184 位 owner）。任务侧，模型看到前 20% 曲线和实验语义，输出未来曲线；从片段推断，曲线按 2% 进度分箱，前缀约 10 个点，需预测后续约 40 个点。基线侧，包括 last-value、参数化曲线拟合、TimesFM-2.5、LC-PFN、pool-trained shape regressor、kNN on prefix shape，以及 11 个 LLM reader。评价侧，用 skill 分数、误差、pairwise ordering、failure flag、headroom ranking 等多维指标；并做相关信息注入、工具使用、代码变更知识、小模型训练等干预。
四、实验主线与关键发现
主实验显示，最强 LLM 在平滑 loss 曲线上 skill 0.66，略高于最佳曲线拟合器的 0.62；在噪声 RL reward 曲线上 0.43，远高于 0.13。这说明语义知识在噪声场景更有价值，但优势并非全面：last-value 在 RL 上是强基线，片段还指出部分训练型 reader 在 RL 测试集没有超过 last-value。日志密度是硬边界：在稀疏社区 run 上，所有方法丢失大部分 skill；没有方法能可靠提前标记失败 run。信息干预的收益明显：提示中放入相关 run 使 loss 曲线误差降低 39%；让强模型知道代码变更后，能在早期曲线无法区分的情况下正确排序全部 36 对 run；而工具使用收益很小。知识可学习：用公开 run 训练的小型开源模型在 loss 曲线上匹配最强 LLM。向 agent 决策延伸：研究 agent 的 progress curve 可用于按剩余 headroom 排序任务，但对单个 agent run 不优于 last-value。
五、机制解释
作者把“知道一个训练组件”分为三层：描述、实现、预判其对 run 的影响。第三层是研究者决定是否值得跑的依据，也是自我改进训练系统需要的能力。曲线是这种知识的监督信号：每条 logged run 把设计选择与实测后果配对。论文的结论是“information helps, effort does not”：给模型相关 run、代码变更语义等信息会提升；给工具、增加测试时交互则收益有限。另一个结论是“what bounds the reading is the log”：预测上限由日志承载的信息决定，模型容量不是唯一瓶颈。
六、贡献与边界
贡献包括 benchmark、系统评测、信息/工具干预、可训练 reader、agent 决策延伸和公开数据。边界包括：数据以公开 run 为主，社区日志稀疏；失败预警不可靠；RL 上 last-value 强；曲线预测不等于下游性能；36 对排序规模小；潜在预训练污染；尚未证明能直接改进真实实验室的 stopping policy。整体上，这是一篇把“实验 triage”做成可测量研究问题的系统性工作，适合 agent、AI-for-Science 和训练动态理解方向精读。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 系统：把 early stopping/compute allocation 变成可学习预测任务，适合 budget-aware research agent 的实验 triage 模块。

## 基本信息

- 作者：Xinran Zhao, Xiang Li, Tong Chen, Jinchuan Tian, Zichun Yu, Tongshuang Wu
- 机构：卡内基梅隆大学（Carnegie Mellon University，CMU；依据通讯作者邮箱 andrew.cmu.edu 推断）
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-11-23
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.30141v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Discussion、Conclusion 与 References 片段，并对摘要未展开的曲线分箱、基线实现和消融细节标注了推断或待核对。
