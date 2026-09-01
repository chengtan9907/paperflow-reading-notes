---
user_id: "cheng tan"
paper_id: 9701
arxiv_id: "2608.27960"
title: "When Teacher Guidance Misleads: Reward-Aligned On-Policy Distillation"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.27960.pdf"
pdf_url: "https://arxiv.org/pdf/2608.27960"
abs_url: "https://arxiv.org/abs/2608.27960"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:16:04"
---
# When Teacher Guidance Misleads: Reward-Aligned On-Policy Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · reward alignment · teacher guidance · large language models

## 一句话总结

针对在线策略蒸馏中教师指导可能与结果奖励不一致的问题，提出奖励对齐的在线策略蒸馏（RA-OPD），通过过滤未对齐轨迹提升学生模型性能。

## 摘要

> On-policy distillation (OPD) has recently emerged as a popular post-training paradigm for large language models (LLMs), providing an efficient way to transfer the knowledge and capabilities of teacher models into student models. However, teacher guidance on student-generated prefixes is not always reliable. Training should optimize the model to generate responses that are more likely to be correct, or equivalently, to get higher outcome rewards. But during OPD, the teacher model may provide guidance that discourages the student from moving toward correct trajectories or moves the student toward incorrect ones, which is misaligned with outcome reward. Such misaligned guidance is unreliable, as it would mislead the optimization process and ultimately degrade model performance. To mitigate misaligned teacher guidance, we propose Reward-Aligned On-Policy Distillation (RA-OPD). The key insight is to keep only trajectories whose induced updates move the student toward correct trajectories or discourage the student from moving toward incorrect ones. Specifically, for each sampled trajectory, RA-OPD checks whether its trajectory-level distillation return is consistent with its outcome reward and then filters out the misaligned trajectories. RA-OPD selects more reliable trajectories to improve student model performance without requiring additional computational cost. We evaluate RA-OPD on math and code benchmarks using models from the Qwen3 family and the DeepSeek-R1 family. Across seven math benchmarks and three code benchmarks, RA-OPD significantly outperforms standard OPD and other tested OPD variants.

Q1: 这篇论文试图解决什么问题？

在线策略蒸馏（OPD）是大语言模型后训练的一种流行范式，它利用教师模型对学生自行生成的前缀（prefix）进行引导，从而高效迁移能力。然而，这种教师指导并非总是可靠的。核心问题在于：教师模型在评估学生生成的部分轨迹时，给出的指导信号可能与“最终答案是否正确”这一结果奖励（outcome reward）不一致。具体而言，学生生成的前缀可能已经包含推理错误，或者该前缀形态落入了教师模型的训练分布之外（如不同于教师见过的自然文本分布），此时教师的 token 级建议既可能错误地劝阻学生继续一条本来正确轨迹，也可能错误地鼓励一条注定走向错误答案的轨迹。

这种“奖励不对齐”的指导会带来严重危害：优化过程被误导，学生模型可能被推向错误方向，导致最终性能退化，甚至出现训练不稳定的情况（已有研究指出这是 OPD 中性能下降甚至训练失败的关键因素）。现有的一些可靠性改进方法主要从局部 token 级指导入手，例如检测单个 token 上的教师置信度或梯度方向，但这些局部信号难以捕捉整个轨迹层面的系统偏差；一条轨迹中个别 token 的指导看似合理，但轨迹整体仍可能与结果奖励背道而驰。因此，需要一种能够从轨迹层面判断教师指导是否与结果奖励对齐的机制，从而筛选出可靠的训练信号。

本文正是在这一背景下提出问题：如何识别并剔除 OPD 中与结果奖励不对齐的轨迹级教师指导？更具体地说，在无法直接获得真实奖励分布的情况下，如何利用已有信号（如可验证任务的结果正确性）来判别一条轨迹的蒸馏回报是否与最终奖励一致？这一问题的难点在于：蒸馏回报与结果奖励可能存在复杂的关系，例如二者均为负但强度不同的情况，需要设计合适的判别准则，而不是简单地看符号是否一致。

Q2: 有哪些相关研究？

与本文相关的已有研究主要包括以下几个方向：

1. **在线策略蒸馏（OPD）范式**：OPD 作为从教师模型到学生模型的知识迁移方式，近年来受到广泛关注。区别于离线蒸馏，OPD 让学生模型自行生成轨迹，再由教师模型对这些轨迹进行指导，从而更贴近学生当前的分布，有助于缓解暴露偏差。
2. **教师指导可靠性分析**：已有工作（如 Li et al. 2026b; Song and Zheng 2026）指出，教师模型对学生生成前缀的指导可能不可靠，会造成性能退化甚至训练失败。这些工作为本文的问题意识提供了依据，但多停留在现象观察或局部改进层面。
3. **基于 token 级指导的评估方法**：部分方法尝试通过评估教师在每个 token 上给出的指导信号（如概率对比、梯度方向）来判断其可靠性，并据此调整训练。然而，这类方法本质上仍是局部的，无法有效抓住轨迹层面的系统偏差；本文在摘要和引言中明确指出这些方法评估教师指导时主要依赖局部 token 级指导，因此仍可能在轨迹层面产生错误引导。
4. **奖励信号在蒸馏中的使用**：在可验证任务（如数学、代码）中，结果奖励可以清晰获得，已有一些方法利用结果奖励对蒸馏样本进行加权或筛选，但通常只考虑奖励符号或绝对值，而没有像本文这样将轨迹级蒸馏回报与结果奖励进行对齐性检查。
5. **数据过滤与课程学习**：广义上，本文属于“训练数据选择”思想在蒸馏中的延伸。与一般的数据清理不同，本文过滤的对象不是数据本身，而是“教师-学生交互产生的指导轨迹”，过滤依据是蒸馏回报与结果奖励的一致性。

总体而言，本文在现有工作的基础上，首次明确提出了“轨迹级教师指导与结果奖励不对齐”的问题，并给出了一个无需额外计算成本的过滤方案，这是与相关工作的主要区别。

Q3: 论文如何解决这个问题？

本文提出 Reward-Aligned On-Policy Distillation (RA-OPD)，核心思想是：在 OPD 过程中，只保留那些“诱导更新”能够将学生推向正确轨迹、或阻止学生走向错误轨迹的采样轨迹。具体实现分为以下几个步骤：

1. **聚合并计算轨迹级蒸馏回报**：对每条由学生采样出的完整轨迹，将教师在各个 token 上给出的指导信号（通常是某种有利于学生 token 概率的度量，如教师对数概率与学生对数概率之差，或 KL 散度减少量）聚合成一个轨迹级别的蒸馏回报（trajectory-level distillation return）。这一聚合操作使得指导信号从局部 token 提升到全局轨迹层面，便于与结果奖励进行对比。
2. **获取结果奖励**：对于数学、代码这类可验证任务，可以直接获得每条轨迹对应的结果奖励（例如答案是否正确、是否通过单元测试）。
3. **对齐性检查（Alignment Check）**：对每条轨迹，检查其轨迹级蒸馏回报与结果奖励是否“一致”。这里的“一致”并不是简单的同号判断，而是强调相对关系。由论文片段可知，RA-OPD 比较的是轨迹之间的相对更新方向，而不是确定每条轨迹的绝对更新方向。例如，如果两条轨迹的蒸馏回报均为负，那么一条结果正确的轨迹也可能在排序上更靠前但仍被“劝阻”，此时如果仅看符号会误判；反之，如果两条轨迹的回报均为正，错误轨迹也可能仍被“鼓励”。因此，RA-OPD 采用某种排序或成对比较机制，过滤掉那些相对结果奖励而言“方向错误”的轨迹。
4. **过滤并更新学生**：仅保留通过对齐性检查的轨迹，用它们的蒸馏梯度更新学生模型，丢弃不对齐的轨迹，从而避免误导性指导影响模型参数。

值得注意的是，RA-OPD 本身不引入额外的计算开销——过滤基于已有的蒸馏回报和结果奖励，无需额外的前向或逆向传播。作者还强调，相较于仅使用局部 token 级指导的方法，RA-OPD 在轨迹层面进行判别，能够捕获更全局的不对齐现象。

Q4: 论文做了哪些实验？

论文在数学和代码基准上对 RA-OPD 进行了系统评估。实验设计如下（基于摘要和片段）：

- **基准与数据集**：使用了七个数学基准（具体名称未在摘要中列出）和三个代码基准（同样未列出）。这类可验证任务能够提供明确的结果奖励信号，适合检验方法效果。
- **模型**：采用了 Qwen3 系列和 DeepSeek-R1 系列模型，覆盖了不同规模和类型的开源 LLM，以验证方法的泛化性。
- **对照方法**：包括标准 OPD 以及作者测试的其他 OPD 变体（具体变体名称未在现有材料中列出），用于展示 RA-OPD 的优越性。
- **评测指标**：通常为数学题的正确率、代码通过率等，但具体指标数值缺失。
- **实验变量**：主要比较不同蒸馏方法下学生模型的最终性能，可能还有关于过滤比例的消融研究、不同蒸馏回报聚合方式的对比等（这些是合理推断，结合方法设计）。

由于原始摘要没有提供数值结果，具体实验数值无法在此复现，需要查阅原文确认数据集名称、baseline 设置和显著提升幅度。

Q5: 发现了什么实验现象？

根据摘要和检索到的段落，可以整理出以下实验观察：

1. **RA-OPD 显著优于标准 OPD**：在七个数学基准和三个代码基准上，RA-OPD 均显著超过了标准 OPD，表明过滤“奖励不对齐”的轨迹确实能够提升学生模型的最终能力。
2. **优于其他 OPD 变体**：RA-OPD 也优于论文测试的其他 OPD 变体，说明其轨迹级对齐检查比局部 token 级启发式方法更有效。
3. **对绝对方向判断的局限性**：论文片段中特别提到“trajectories rather than determining each trajectory's absolute update direction. If both returns are negative, a correct trajectory can rank higher while still being discouraged; if both are positive, the incorrect trajectory is still...”，这暗示在实验中作者可能发现，仅仅比较蒸馏回报的符号与结果奖励符号的一致性是不够的，需要相对排序。因此，RA-OPD 在设计上采用了成对或排序比较，这一观察可能来自消融实验中对不同判别准则的对比。
4. **无需额外计算成本**：摘要明确表示 RA-OPD 不增加计算开销，因此性能提升不是靠更多计算换来的。

由于缺乏具体数值，以上观察主要是定性的；具体的消融趋势、失败案例、指标间张力等信息需要阅读原文的实验部分才能补充。

Q6: 有什么可以进一步探索的点？

从本文问题和方法出发，以下方向值得进一步探索：

1. **扩展到非可验证任务**：当前方法依赖可验证任务中的明确结果奖励；对于开放式生成、对话等无法自动获取结果奖励的任务，如何定义“结果奖励”或借助奖励模型来近似，是一个重要挑战。
2. **更细粒度的对齐判别**：RA-OPD 在轨迹层面进行过滤，但 trajectory 内部可能存在部分良性、部分有害的指导；将对齐检查下沉到 segment 或 token 级别，同时保留全局视角，可能进一步提高样本利用率。
3. **自适应过滤阈值与软加权**：当前是二值过滤（保留或丢弃），可能导致部分有噪声但有信息的样本被完全抛弃。设计基于一致性的软权重，或者自适应地调整过滤强度，有望在可靠性与多样性之间取得更好平衡。
4. **与强化学习、偏好优化的结合**：RA-OPD 的过滤思想可以自然地融入 RLHF / RLAIF 流程，例如对“教师指导-结果奖励”不对齐的偏好数据进行剔除，从而提升策略优化稳定性。
5. **理论分析**：给出过滤准则下学生风险上界的变化，解释为什么丢弃部分样本反而能提升性能，以及是否在什么条件下严格保证改进。
6. **扩展到多教师或异构教师场景**：当教师模型不止一个，或教师能力分布不均时，如何联合判断指导的可靠性。
7. **与数据选择、课程学习的结合**：将轨迹过滤与课程学习结合，在训练早期允许更多探索，后期更严格过滤，可能加速收敛。

Q7: 总结一下论文的主要内容

本文聚焦大语言模型在线策略蒸馏（OPD）中的教师指导可靠性问题。OPD 是一种高效的后训练范式，它让教师模型指导学生从自身采样出的前缀继续生成，从而将知识迁移给学生。然而，这种“贴身指导”并不总是利于学生的：教师看到的只是学生生成的不完整前缀，这些前缀可能包含推理错误，或与教师训练分布差异较大，导致教师给出的逐 token 建议与最终答案的对错（结果奖励）不一致。这种“奖励不对齐”的指导会误导优化，使模型性能下降，甚至导致训练崩溃。

作者首先识别了这一问题，批评现有方法大多只关注局部 token 级的指导可靠性，而忽略了轨迹层面的系统性偏差。为此，他们提出了奖励对齐的在线策略蒸馏（RA-OPD）。RA-OPD 的出发点非常直观：只保留那些“诱导更新”能让学生朝正确轨迹前进，或防止其滑向错误轨迹的样本。技术实现上，RA-OPD 将教师在各 token 的指导信号聚合成一个轨迹级蒸馏回报，然后将每条轨迹的蒸馏回报与其结果奖励进行一致性检查。这里的关键设计在于，不能简单地比较两者的符号是否相同，而应采用相对比较（例如排序）来判别方向性。文中举例说明：如果两条轨迹的蒸馏回报都是负的，其中一条结果正确的轨迹虽然相对排名更高，但可能仍受到抑制；如果两条轨迹的蒸馏回报都是正的，则结果错误的轨迹可能仍受鼓励。因此，绝对符号不是可靠信号，必须用相对顺序或成对比较。

在实验方面，论文选择了数学和代码这两类可验证任务，使用 Qwen3 和 DeepSeek-R1 系列模型，在七个数学基准和三个代码基准上比较了 RA-OPD 与标准 OPD 及其他 OPD 变体。结果显示，RA-OPD 在所有基准上都显著优于对照方法，且没有增加额外计算成本。这一结果表明，通过轨迹级过滤来剔除与结果奖励不一致的指导，能够有效提升蒸馏质量，是一种简单而有效的改进方式。

本文的主要贡献可以概括为：第一，明确提出了“奖励不对齐的教师指导”是 OPD 性能退化和训练失败的关键因素；第二，设计了 RA-OPD 算法，通过轨迹级蒸馏回报与结果奖励的一致性检查，实现无需额外开销的轨迹筛选；第三，通过大量实验验证了方法在数学和代码任务中的有效性。

总体而言，本文为 OPD 的可靠性问题提供了一个轻量且有效的解决方案，也为后续在更复杂任务中设计更细致的过滤机制开辟了方向。不过，当前方法依赖于可验证任务中的结果奖励，对于更开放的任务形态仍需进一步研究。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与大语言模型后训练、在线蒸馏、知识迁移直接相关。

## 基本信息

- 作者：Siyuan Gan, Yuhan Li, Xiran Wang, Linjian Meng, Boyan Wang, Zhen Zhao, Jing Huo, Yang Gao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.27960`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，主要来自摘要、方法、结论和可靠指导讨论部分，并结合了启发式草稿进行补全；部分实验数值和具体基准名称因证据不足未作推断。
