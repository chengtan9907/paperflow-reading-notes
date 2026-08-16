---
user_id: "cheng tan"
paper_id: 7301
arxiv_id: "2608.07449"
title: "SkillProx: Self-Evolving Agent Skills via Proximal Textual Gradient Descent"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.07449.pdf"
pdf_url: "https://arxiv.org/pdf/2608.07449"
abs_url: "https://arxiv.org/abs/2608.07449"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-11T01:17:05"
---
# SkillProx: Self-Evolving Agent Skills via Proximal Textual Gradient Descent

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · skill learning · textual gradient descent · proximal refinement

## 一句话总结

SKILLPROX 是一个受近端梯度下降启发的技能演化框架，通过前向闭环诊断与后向效用感知近端精炼，在文本空间中迭代改进 LLM 代理的可复用技能，无需任何权重更新。

## 摘要

> LLM agents increasingly adapt to recurring tasks by accumulating procedural knowledge in skills. These skills are lightweight, reusable textual artifacts that are loaded into the agent's context without weight updates. Recent methods refine skills through iterative task execution, failure diagnosis, and trajectory-guided text-space updates. However, existing frameworks lack explicit diagnosis–outcome feedback and treat deletion as a generic edit operation rather than a dedicated mechanism for consolidating accumulated knowledge. We introduce SKILLPROX, a proximal-gradient-inspired forward-backward framework that couples closed-loop diagnostic evolution with utility-aware proximal refinement. Motivated by a composite objective balancing task loss and skill complexity, the forward stage re-executes diagnosis-driven edits on the same task batch, rolls back regressions, and feeds measured outcomes into subsequent diagnoses. The backward stage decomposes the resulting skill into auditable knowledge units, estimates their contributions using a frozen leave-one-out utility audit, and applies validation-gated consolidation, demotion, or removal. Experiments on in-distribution and out-of-distribution benchmarks across multiple backbone LLMs show that SKILLPROX improves average accuracy by 3.0 percentage points over the strongest gradient-based baseline. Component ablations demonstrate the complementary effects of closed-loop diagnosis and proximal refinement. $^{1}$

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的核心问题是：LLM 代理如何在不更新权重的前提下，通过文本形式的'技能'制品持续自我进化，并保证这种进化是稳定、可审计、可收敛的。具体拆解如下：

1. 现有技能精炼方法的反馈回路是断裂的。典型做法是'执行任务 → 诊断失败 → 生成文本编辑 → 更新技能'，但这个流程缺少一个关键环节——更新后的技能是否真的在同一个任务批次上带来了可测量的改善，这个结果没有被显式地回收并用于指导下一轮诊断。导致的结果是：系统无法区分一次编辑是真实进步还是偶然波动，也无法阻止有害编辑在技能中长期残留。

2. 删除操作被'降级'为通用编辑操作。当技能不断积累，旧知识可能与新任务冲突，或者某些启发式规则被验证为错误。现有框架把'删除某条规则'和'修改某句话'等同处理，缺乏一个专门面向知识整合的清理机制。作者认为，技能演化既需要添加（consolidation），也需要降权（demotion）和删除（removal），且这些操作应当由验证信号门控，而不是由生成模型自由发挥。

3. 技能演化的目标函数不完整。现有方法只优化任务表现，忽略了技能本身的可维护性和复杂度。这会导致技能文本无限膨胀、冗余规则堆积、过拟合到历史任务，进而损害分布外泛化。作者因此提出一个复合目标，同时考虑任务损失与技能复杂度，并用类似近端梯度的方式约束每次更新的'步长'。

4. 更广义地，问题属于'如何让文本制品拥有类似参数空间的优化动力学'。技能作为上下文中的可执行知识，无法使用反向传播，只能通过离散文本编辑进行演化。这要求设计一个同时具备梯度方向估计、步长控制、正则化与收敛判断的框架。目前该方向尚缺乏系统性工作，尤其缺少把'诊断'与'结果验证'结合成闭环的设计。

Q2: 有哪些相关研究？

由于本次生成仅能依赖摘要与少量检索片段，相关工作部分的信息主要来自 References 命中线索和问题背景推断，具体如下：

1. 文本空间梯度下降类方法：References 片段中出现 'Optimizing Agent Skills Like Gradient Descent'，表明已有工作把技能优化类比为梯度下降，在文本空间中通过自然语言'梯度'更新技能。这类方法是 SKILLPROX 的直接竞争基线（摘要称其平均准确率超过'最强基于梯度的基线'）。合理推断：这类方法可能只做前向的梯度式编辑，缺少显式的执行结果校验和单元级审计。

2. 递归技能增强方法：References 片段中出现 'SkillRL: Evolving Agents via Recursive Skill-Augmented'，推测是一种通过递归地将技能增强到智能体决策回路中的强化学习式方法。可能强调'增强'而非'删减'，因此与 SKILLPROX 的'删除也是一种专门机制'形成对比。

3. 更广泛的技能库/技能学习研究：LLM 代理中已有很多工作把技能作为可复用文本制品（例如通过示范轨迹提炼技能、用技能库支持规划等）。这些方法大多只做增量积累，缺乏系统的知识整合与淘汰机制。SKILLPROX 的定位正是在这一谱系中补充了'前向-后向'的优化结构。

4. 关于近端优化的启发：作者明确说灵感来自近端梯度下降（proximal gradient descent）。在经典优化中，近端梯度用于处理复合目标（光滑损失 + 非光滑正则），对应到文本空间即'用复杂度惩罚限制每次编辑的幅度'。这一视角在技能学习领域较为新颖。

需要指出：检索证据中 References 片段只是残片，无法确认这些工作的具体方法、数据集和作者。完整的 Related Work 对比论证需要回原文阅读。

Q3: 论文如何解决这个问题？

SKILLPROX 的整体框架是一个受近端梯度下降启发的前向-后向（forward-backward）优化过程，目标是解决'如何用离散文本编辑实现受控的技能更新'。其方法要点如下：

1. 复合目标函数。将技能演化建模为最小化 L(skill) = TaskLoss(skill) + λ·Complexity(skill)。TaskLoss 衡量技能在任务批次上的执行结果（如准确率），Complexity 惩罚技能文本的冗余度、长度或知识单元数量。λ 控制两者权衡。这个复合目标直接呼应近端梯度中'数据项 + 正则项'的结构。

2. 前向阶段：闭环诊断演化（closed-loop diagnostic evolution）。其流程是：
 - 先在当前技能下执行同一任务批次，得到基线结果；
 - 由诊断模块（LLM）分析失败案例，生成针对性的编辑候选（修改、增补或删除某段技能文本）；
 - 将编辑后的技能在同一任务批次上重新执行，逐任务对比结果；
 - 如果某个编辑导致整体回归（regression），则回滚该编辑；
 - 把实际测得的'帮助/有害'信号作为显式反馈，喂给下一轮诊断模块，使诊断不再是单程的，而是被真实结果校准的闭环。

3. 后向阶段：效用感知近端精炼（utility-aware proximal refinement）。
 - 将技能文本分解为可审计的知识单元（knowledge units），每个单元近似一个可独立评估的规则或步骤；
 - 对每个单元执行冻结的留一效用审计（frozen leave-one-out utility audit）：临时移除该单元，在验证集上重新执行技能，测量性能变化，从而估计该单元的边际贡献；
 - 基于验证门控（validation gating）对单元执行三类操作：巩固（consolidation，高正贡献单元被强化/保留）、降级（demotion，低贡献单元被弱化或降低优先级）、移除（removal，负贡献单元被删除）。

4. 'Proximal' 的含义。框架名称中的 Proximal 至少体现在两个层面：(a) 复合目标中的复杂度惩罚限制了单次更新的文本变化量，类比近端梯度中的近端算子；(b) 前向阶段的回滚机制保证了更新不会偏离当前技能太远，相当于在离散空间中实现'Trust Region'。这两个机制共同防止技能演化发散。

5. 诊断与结果的耦合。作者强调现有方法缺乏 diagnosis–outcome feedback，因此 SKILLPROX 特意把每一步编辑的实测结果纳入后续诊断输入，使系统知道哪些诊断是有效的，从而在迭代中自我修正诊断策略。

需要说明：以上是对摘要和检索片段的忠实梳理；具体 prompt 设计、回滚阈值、知识单元切分粒度、λ 调节方式等实现细节在摘要中未给出，需查阅正文。

Q4: 论文做了哪些实验？

根据摘要、主结果片段和消融信息，SKILLPROX 的实验设置与内容如下：

1. 骨干模型：实验使用多个骨干 LLM，明确提到的有 Qwen3.5-27B，其余模型名称未在检索片段中出现。推测可能包含不同规模或不同系列的模型以验证泛化性。

2. 基线：与六个基线方法对比，其中至少包含一个'最强基于梯度的基线'（likely 文本梯度下降类方法，如 'Optimizing Agent Skills Like Gradient Descent'）。其他五个基线的具体名称未知。

3. 基准数据：包含一个 in-distribution（ID）和两个 out-of-distribution（OOD）基准。具体任务类型、数据集名称、评估指标，摘要未给出；从'准确率'和'pp'（百分点）来看，主要指标是任务准确率。

4. 初始化：实验从 Human Skill 初始化开始，即先用人类编写的技能作为起点，再让 SKILLPROX 对其进行优化。这个设计使'提升'具有明确的参照系。

5. 消融实验：组件消融验证前向闭环诊断与后向近端精炼的互补效应，即分别去掉其中一个模块后性能下降，两者联合效果最佳。此外，检索片段还显示一个单元级审计的消融结果：删除负效用内容使准确率从 46% 提升到 54%，直接证明了后向阶段中'移除'机制的价值。

6. 实验覆盖范围：摘要说'在大多数评估设置中观察到性能增益'，暗示并非所有设置都提升，但未给出失败的具体案例。

由于论文正文未被完整检索到，以下细节属于当前证据缺口：六个基线的具体名称、三个骨干模型的完整名单、ID/OOD 基准的具体任务与数据集、统计显著性检验、计算开销报告、超参数（λ、批次大小、迭代轮数）等。

Q5: 发现了什么实验现象？

从检索到的片段可以提炼出以下实验现象：

1. 一致的正向提升：SkillProx 在所有骨干模型上都把基础技能（Human Skill 初始化）转变为净正信号。最具体的数字是：在 Qwen3.5-27B 上，基础技能从 38.3% 准确率提升到 51.3%，即 +13.0 个百分点。这是一个相当显著的增益，说明框架在单个模型上的效果非常强。

2. 平均领先最强梯度基线 3.0 个百分点：这是一个全局结论，说明即使与专门为技能优化设计的文本梯度基线相比，SKILLPROX 也有明显优势。合理推断：优势主要来自闭环反馈和单元级清理，因为纯梯度式编辑容易出现'加一条规则破坏另一条'的问题。

3. 删除负效用内容带来大幅提升：在后向阶段的单元审计中，移除负效用知识单元使准确率从 46% 提升到 54%（+8pp）。这是一个反直觉但很有价值的现象——技能中积累的'知识'并非越多越好，部分规则在新任务分布下反而造成干扰。这也解释了为什么作者要把'删除'从通用编辑中独立出来：它不是一个边界情况，而是主要的收益来源之一。

4. 闭环诊断与近端精炼互补：消融实验显示两者缺一不可。推测单用闭环诊断可能让技能快速过拟合任务批次，而单用近端精炼可能缺少对失败原因的深度修正；两者结合平衡了探索与保守。

5. '大多数评估设置'的措辞说明存在负结果或持平情况：这提醒我们该方法不是在所有 backbone/benchmark 组合上都有效。由于论文未给出具体失败细节，这是值得关注的边界条件。

6. 从复杂度惩罚视角看，技能文本在优化后可能会变得更简洁。虽然摘要没有直接报告技能长度变化，但'技能复杂度'进入复合目标意味着优化后技能应更短或更结构化。这属于合理推断。

Q6: 有什么可以进一步探索的点？

受当前证据限制，以下方向多为基于框架特性的合理推断，而非原文明确列出的 Future Work：

1. 更精细的知识单元表示：当前需要把技能分解为可审计的单元，这种分解如果由规则启发式完成，会限制适用范围。可以探索学习式分解、语义聚类或层次化知识单元，使后向审计能捕获跨单元的交互效应。

2. 自适应复杂度系数 λ：复合目标中的 λ 如何设定、是否随演化轮次变化，目前未知。一个自然延伸是让 λ 根据验证集性能动态调整，类似于正则化强度的在线调度。

3. 诊断-结果闭环的跨任务泛化：当前闭环是在同一任务批次上验证编辑，未来可以把'验证批次'与'诊断批次'分离，测试闭环信号能否预测不同分布下的长期收益，从而提升 OOD 泛化。

4. 与其他记忆机制结合：技能库通常不是唯一记忆形式，还可以与参数化记忆、向量检索、工作流图等结合。SKILLPROX 的效用审计本身不依赖具体技能表示，因此可以扩展到更多知识载体。

5. 扩展到更多应用场景：作者没有提到任何 AI-for-Science 或生物方向。但技能演化机制对'实验方案优化'、'数据分析流程复用'等科学智能体任务同样适用——科学任务中经常需要积累可复用的协议知识，并且需要淘汰过时的操作步骤。这是对当前用户画像（ai-for-science 偏好）最有对接潜力的方向。

6. 理论分析：近端梯度在凸优化中有收敛保证，但文本空间的编辑算子显然非凸。研究类似框架在离散空间中的收敛条件、步长与回滚阈值之间的关系，会是一个有趣的理论方向。

7. 失败模式分析：既然存在'大多数设置提升'，识别哪些设置不提升（例如任务类型、技能初始化质量、骨干模型能力）并给出应对策略，是实际部署前必须补齐的工作。

Q7: 总结一下论文的主要内容

SKILLPROX 是一种面向 LLM agent 的、受近端梯度下降启发的技能自我演化框架，核心贡献是把'技能'这个文本制品的优化过程系统化为一个前向-后向结构。

论证主线：作者从三个观察出发。第一，LLM agent 越来越依赖以技能形式累积的程序性知识，这些技能是轻量级文本，加载到上下文中即可使用，不更新权重。第二，已有技能精炼方法虽然能做到'执行→诊断→编辑'，但缺少诊断-结果反馈：编辑后是否真的变好没有被显式测量并回收，导致演化缺乏可靠的梯度信号。第三，删除被当作通用编辑操作，而不是专门的知识整合机制，导致负效用知识在技能中堆积。基于这三个观察，作者提出把技能演化看成一个复合优化问题，目标同时包含任务损失与技能复杂度，并借鉴近端梯度下降的思路设计更新规则。

技术主线：框架分为前向和后向两个阶段。前向阶段执行'诊断驱动编辑 → 同批重执行 → 回滚回归 → 结果反馈'的闭环，确保每次修改都经过现实检验。后向阶段把技能分解为知识单元，用冻结的留一效用审计估计每个单元的边际贡献，然后通过验证门控对单元进行巩固、降级或移除。前向阶段负责探索和修正，后向阶段负责整合与清理；两者共用同一个验证信号，形成一个完整的自进化循环。复合目标中的复杂度项扮演近端正则的角色，约束单次更新不要偏离太远，使演化过程稳定。

实验主线：论文在多个骨干 LLM（明确包含 Qwen3.5-27B）上，与六个基线对比，在一个人工编写的初始技能（Human Skill）基础上进行优化，使用了 1 个 in-distribution 和 2 个 out-of-distribution 基准。主要结果是：平均准确率比最强基于梯度的基线高 3.0 个百分点；在 Qwen3.5-27B 上从 38.3% 提升到 51.3%（+13.0pp）。消融实验显示闭环诊断和近端精炼互补；单元审计方面，移除负效用内容使准确率从 46% 提升到 54%，说明删除机制是高收益来源。

总体评价：SKILLPROX 把离散文本技能演化提升到了'有显式验证、有复杂度控制、有单元级审计'的层次，填补了现有框架在反馈闭环和删除机制上的空缺。其方法命名与近端梯度下降的类比是否严格成立，取决于后向精炼是否真的等价于近端算子，这需要细读正文。另外，由于检索证据有限，目前无法核实实验细节、baseline 设置和失败案例，建议读者以原文为准。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：论文主题是 LLM agent 如何通过文本技能实现自我演化，属于 agent 学习与自我改进的前沿问题。

## 基本信息

- 作者：Mingxuan Zheng, Yujin Zhou, Chuxue Cao, Boqin Yin, Yuyao Zhang, Jiapeng Sun, Shuaishuai Gong, Sirui Han, Yike Guo
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.07449`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的片段（Abstract、Introduction、Conclusion、Main Results 与 References 残片），并结合 heuristic_draft 进行了润色与补全；由于全文信息有限，除摘要明确支持的内容外，多处细节（如具体 baseline 名称、benchmark 名单、消融数字的适用范围）属于合理推断或推测，并已在相关字段中标注。另需注意：该论文的 arXiv 编号与发表日期（2608.07449 / 2026-08-10）在当前时间下属于未来时间戳，建议核实来源真实性后再作为引用依据。
