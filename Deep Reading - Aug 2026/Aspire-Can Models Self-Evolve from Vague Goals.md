---
user_id: "cheng tan"
paper_id: 10019
arxiv_id: "2608.31111v1"
title: "Aspire: Can Models Self-Evolve from Vague Goals?"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.31111v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.31111v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:46:50"
---
# Aspire: Can Models Self-Evolve from Vague Goals?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolution · vague goal · llm agent · benchmark

## 一句话总结

本文提出ASPIRE基准，将LLM自我进化从显式任务优化推广到模糊目标驱动：智能体在只有自然语言能力目标、下游评估任务隐藏的条件下，自行解释目标、选择数据与更新方法、构建训练与验证信号并决定何时评估；实验发现模糊目标将搜索精力引向目标解释，权重级收益稀疏且不稳定，最强进化框架仍低于工程化的Qwen-Agent参考。

## 摘要

> Many important forms of human learning begin with a vague goal, such as “become a better physicist” or “improve at research.” Learners must interpret the goal, identify capability gaps, decide how to learn, and determine whether they have actually improved. In contrast, existing work on LLM self-evolution typically begins with tasks and evaluation metrics specified by humans, reducing self-evolution to optimizing an explicit objective rather than deciding what and how to learn. We introduce ASPIRE, a benchmark for vague-goal-driven self-evolution. ASPIRE provides only a natural-language capability goal while downstream evaluation tasks remain hidden. The agent must operationalize the goal by choosing data and update methods, constructing training and validation signals, and deciding when to evaluate. ASPIRE supports both model-weight and agent-harness evolution in a unified interactive environment and evaluates the resulting systems on a hidden, expert-authored set of 520 items spanning six goals. Our experiments show that vague goals redirect search effort toward goal interpretation. Current agents routinely complete training and harness-editing loops, but weight-level gains remain sparse and unstable, and the strongest evolved harness remains below the engineered Qwen-Agent reference. Agents often train on mismatched data and trust narrow self-evaluations, so local gains fail to transfer to hidden evaluation and continued search and training can erase earlier improvements.
> Date: September 1, 2026
> Project Page: https://self-developing-agents.github.io/

Q1: 这篇论文试图解决什么问题？

核心问题：LLM能否真正实现从模糊目标出发的自我进化？

论文指出，现有自我进化研究存在一个根本性简化：任务和评估指标由人类预先给定，模型只需优化显式目标，这相当于把“进化”降维成“在固定目标上搜索”。但人类学习的重要形式是从模糊的能力目标开始（如“成为更好的物理学家”），学习者必须先解释目标、判断自己缺什么、决定学什么、怎么学，以及如何知道是否变好了。论文认为，这个“决定学什么”的环节正是下一代LLM自我进化需要攻克的核心。

具体子问题包括：
1. 如何形式化“模糊目标驱动的自我进化”？——模糊目标不能等同于含糊不清，而应定义为尚未被操作化为固定任务或可分解奖励的广泛能力方向（合理推断，依据方法片段）。
2. 如何设计研究环境，使进化过程保持外部可测量但又不让智能体直接看到评估？——ASPIRE保留评估器作为控制器端科学仪器，但隐藏其基准定义、条目与可分解奖励，从而在“可测量”和“不可直接优化”之间取得平衡。
3. 智能体在只有模糊目标时，能否自主构造训练/验证信号？——需要解决数据选择、更新方法选择、验证集构造和评估时机决策等问题。
4. 权重级进化与智能体框架级进化各自的成效与局限是什么？——论文统一研究两种层级，并对比显式任务设定。
5. 局部收益能否迁移到隐藏评估？持续搜索和训练是否会导致回退？——摘要明确显示当前智能体存在迁移失败和进步被抹除的问题。

论文的立场不是提出一个新的训练算法，而是建立一个可复现的研究基准和交互环境，使“决定学什么”的问题成为可系统研究的对象。

Q2: 有哪些相关研究？

论文没有在摘要中列出具体引用，但根据摘要和检索片段可以梳理出以下相关研究脉络（其中部分为合理推断）：

1. LLM自我进化/自我改进（self-evolution, self-improvement, self-training）：这类工作通常由人类给定任务和评估指标，模型通过自举数据、自我奖励或迭代训练来优化显式目标。论文明确批评这种设定将自我进化简化为显式目标优化，ASPIRE的贡献正是把隐藏目标引入。
2. LLM Agent研究：现代LLM agent能够针对给定目标搜索有效改进方式，例如调整工具、提示或流程（结论片段）。但agent通常依赖外部任务规范或可分解奖励，论文则要求agent在没有这些规范的情况下自行决定学什么。
3. 基准与评估设计：ASPIRE强调“外部可测量性”由控制器端评估器保证，同时隐藏具体条目，这与避免过拟合benchmark的设计思路一脉相承，类似hidden test set或动态benchmark的做法（合理推断）。
4. 自动机器学习与AI科学家：自动数据选择、自动课程学习、自动提示优化等方向与ASPIRE中的“选择数据、更新方法”存在交集；论文可能借鉴了这些领域的思想，但把它们统一在“模糊目标”框架之下（推测，需原文确认）。
5. 权重级训练与框架级优化：论文同时支持model-weight和agent-harness两种进化，涵盖了参数微调、模型合并、harness编辑等不同粒度的方法，与模型编辑、prompt optimization等研究方向相关（推测）。

ASPIRE与上述工作的关键差异：不提供任何可见任务定义或可分解奖励，智能体必须自行操作化目标；评估完全隐藏，使得“进步”只能通过外部专家条目衡量；且同时考察权重与框架两级进化，而不是只关注其中一种。

Q3: 论文如何解决这个问题？

论文通过构建一个名为ASPIRE的基准和交互环境来研究模糊目标驱动的自我进化，其方法要点如下：

1. 问题形式化：明确将研究问题定义为vague-goal-driven self-evolution。“vague”不是指模糊不清，而是指一个尚未被操作化为固定任务或可分解奖励的广泛能力方向（依据方法片段）。这界定了问题的难度层级。
2. 基准设定：ASPIRE只向智能体提供一个自然语言能力目标，不提供下游任务描述、评价指标或奖励函数。所有下游评估任务对智能体隐藏，由专家编写520个条目、覆盖六个能力目标。评估器作为控制器端的“科学仪器”保留，但智能体看不到基准定义、条目和可分解奖励，从而防止直接优化评估器。
3. 统一交互环境：环境同时支持两种进化层级：
 - 模型权重级进化：从指令微调后的checkpoint出发，由智能体决定如何在迭代中更新权重（如选择数据、损失函数、微调超参数等）。
 - 智能体框架级进化：允许智能体编辑自身harness（如提示词、工具调用方式、流程编排等），以改善能力。
 两种层级可分别或联合使用，使得研究和对比更加统一。
4. 智能体的自主决策范围：在环境中，智能体必须自行完成四个关键环节——(a) 选择训练数据；(b) 选择更新方法；(c) 构建训练信号和验证信号；(d) 决定何时评估。这模拟了人类学习者“决定学什么、怎么学、怎么验证”的过程。
5. 对比实验设计：将模糊目标设定与显式任务设定进行对比，以回答RQ1；通过在指令微调checkpoint上施加迭代权重更新来检验RQ2；通过对agent harness进行编辑来检验RQ3（合理推断）。并提供工程化的Qwen-Agent作为“专家设计”的参考基线，用于衡量进化后的系统是否真的达到人类工程水平。

整体而言，ASPIRE不是提出某种具体训练算法，而是把“从模糊目标出发自我进化”变成一个可操作、可复现、可外部验证的研究问题，并暴露当前agent在此问题上的能力边界。

Q4: 论文做了哪些实验？

论文围绕三个研究问题开展实验（依据实验结果片段，RQ1/RQ2明确出现，RQ3为合理推断）：

1. RQ1 —— 模糊目标如何改变后训练结果和搜索轨迹？
 - 对比模糊目标设定与显式任务设定，在相同交互环境下进行后训练和搜索。
 - 观测指标：后训练效果、搜索轨迹的特征（如精力分配、迭代路径）。
 - 目的：分析“目标模糊化”这一变量本身对进化行为的影响。

2. RQ2 —— 给定一个指令微调checkpoint，LLM能否仅凭模糊目标决定学什么，并通过迭代权重更新提升能力？
 - 起点：指令微调后的checkpoint，只有自然语言能力目标。
 - 智能体需要自主选择数据、更新方法、验证信号，并决定何时评估。
 - 评估：隐藏的专家测试条目。

3. RQ3 ——（合理推断）通过编辑agent harness能否从模糊目标中获得能力提升？
 - 摘要中提到“agent-harness evolution”被支持，且实验发现“the strongest evolved harness remains below the engineered Qwen-Agent reference”，所以第三个问题很可能围绕harness进化展开。具体协议需要阅读原文确认。

实验环境：ASPIRE统一交互环境，支持model-weight和agent-harness两种进化。

评估方式：使用隐藏的520个专家条目，覆盖六个能力目标；对照组包括显式任务设定和工程化的Qwen-Agent参考系统（作为人类工程基线）。

注意：摘要未提供具体的模型大小、训练数据量、迭代轮数、每个目标对应的条目数、参考系统的架构等细节，这些信息需查阅论文全文。

Q5: 发现了什么实验现象？

基于摘要和检索片段，实验观察到的主要现象包括：

1. 模糊目标重定向搜索努力：与显式任务设定相比，智能体在模糊目标下花费大量尝试去解释目标本身（goal interpretation），而非直接训练。搜索轨迹明显偏向“理解目标”而非“优化指标”。
2. 训练和harness编辑循环可被例行完成：智能体能够形成“选择数据→训练→评估→编辑harness”的循环，流程上没有崩溃，说明基本机制是可行的。
3. 权重级增益稀疏且不稳定：尽管循环能完成，但通过权重更新获得的实际能力提升非常稀少，且不同运行之间波动大，说明自主决定学什么和怎么学的成功率很低。
4. 最强的进化harness仍不及工程化参考：进化后的最佳harness在隐藏评估上仍低于人工设计的Qwen-Agent参考系统，表明当前自动化框架搜索尚未达到人类工程水平。
5. 数据选择的错配：智能体经常在与模糊目标不匹配的数据上进行训练，说明其对“该学什么”的判断常常出错。
6. 自我评估的狭窄性：智能体倾向于信任狭窄的自我评估信号（例如在很小或偏差的验证集上评估），导致其自以为在改进，实际上未覆盖真实能力。
7. 局部收益不迁移：在自建验证信号上看到的局部改进无法迁移到隐藏的专家评估，说明自评与真实目标之间存在系统性偏差。
8. 回退现象：持续搜索和训练可能抹掉早期已经取得的改进，进化轨迹不是单调上升的。

这些现象合在一起，说明当前agent虽然具备“做”自我进化的能力，但缺乏“判断”该进化什么和是否真进步的能力，而这正是模糊目标设定下最关键的挑战。

Q6: 有什么可以进一步探索的点？

基于论文的局限和当前实验结果，以下方向值得进一步探索（其中部分为合理推断或推测）：

1. 目标解释的自动化和校准：研究如何让模型更准确地将模糊目标转化为可执行的学习计划，例如引入显式的目标分解、能力缺口分析、多轮自我澄清等机制。
2. 自我评估与隐藏评估的一致性：开发更鲁棒的自建验证信号，使其与专家评估的分布更接近。可探索利用模型不确定性、集成评估、meta-evaluation等方式减少“狭窄自评”。
3. 数据选择与课程设计：解决“在错误数据上训练”的问题。可以借鉴主动学习、难度感知采样、基于模型差距的数据选择等方法。
4. 权重级进化与harness级进化的协同：目前两类进化是分开研究的，未来可以探索二者联合搜索，例如交替进行数据选择、参数更新和harness编辑。
5. 防止回退与保持进步：研究持续训练中如何保留已获得的技能，例如记忆回放、正则化、skill library等方式，使进化轨迹更单调。
6. 扩展目标与任务覆盖面：目前只有六个目标、520个条目，可扩展到科学发现、编程、数学、创意写作、多语言、工具使用等更广泛能力，并增加目标的模糊程度梯度。
7. 更细粒度的归因分析：在实验协议中更精细地分离“目标解释”“数据选择”“验证信号”“评估时机”各自的贡献，以定位瓶颈。
8. 与AI-for-Science等跨领域结合：论文明确指出“成为更好的物理学家”这类科学能力目标，可以扩展到真实科研任务，例如让模型自主决定读哪些论文、做哪些实验、如何更新模型。
9. 安全与对齐问题：模糊目标下，模型可能误解目标或优化出与人类意图不符的行为。需要研究外部评估器之外的护栏、价值观对齐和可控性。
10. 降低评估器的工程依赖：目前隐藏评估需要专家编写数百条目并保持更新，未来可探索如何自动化生成隐藏且不泄漏的评估集，以及如何让评估器适应新目标。

Q7: 总结一下论文的主要内容

论文的核心立足点是：人类学习中有大量重要形式开始于模糊目标，而当前LLM自我进化研究避开了这个最本质的环节。现有工作由人工指定任务和评估指标，模型只需要在一个已经定义好的目标上做优化，这实际上是“在明确问题下搜索解法”，而不是“决定问题是什么”。论文将这种被省略的能力称为“模糊目标驱动的自我进化”，并指出其关键步骤包括：解释目标、识别能力差距、决定学习内容与方式、构造训练和验证信号、判断是否真正进步。

为研究这一能力，论文提出ASPIRE基准。ASPIRE只向智能体提供一个自然语言能力目标，所有下游评估任务对智能体隐藏。这样设计的目的在于隔离“决定学什么”与“怎么学”的混杂因素：智能体无法直接看见测试集，只能依靠自己对目标的理解来生成数据、构造验证信号和决定何时更新。同时，为了保持进化过程的可测量性，ASPIRE把评估器保留在控制器端，但不暴露其具体内容。这种“外部可测量但内部不可见”的设定是该基准的核心创新。

ASPIRE在统一交互环境中支持两种进化层级：模型权重级进化和智能体框架级进化。权重级进化意味着智能体可以决定如何通过对模型参数进行迭代更新来提升能力；框架级进化则允许智能体修改自身的agent harness（包括提示、工具、流程编排等）。论文通过这两个层级覆盖了从底层参数到高层策略的完整进化空间。

实验围绕三个研究问题展开。RQ1考察模糊目标相对显式任务如何改变后训练结果和搜索轨迹；RQ2考察仅凭模糊目标，智能体能否通过权重更新带来能力提升；RQ3（合理推断）关注agent harness进化。评估在隐藏在专家编写的520个条目、覆盖六个能力目标上进行，并以工程化的Qwen-Agent作为人类工程基线。

实验发现：第一，模糊目标确实把智能体的搜索努力引向了目标解释，说明模型确实感知到了“目标不明确”这一挑战，但这种解释努力并未转化为可靠的能力提升。第二，智能体能够完成训练和harness编辑的循环，但权重级收益稀疏且不稳定，说明“做了”不等于“做好了”。第三，最强的进化harness仍低于Qwen-Agent参考，说明自动化框架搜索还未达到人类工程师的水平。第四，智能体经常在错误匹配的数据上训练并信任狭窄的自我评估，导致局部收益无法外推到底层真实能力；更严重的是，持续搜索和训练还可能抹掉早期改进，使进化过程出现回退。

这些结果共同揭示了一个关键结论：当前LLM agent在模糊目标下的主要瓶颈不是“能否执行训练或编辑”，而是“能否正确判断该学什么、是否真正学会”。论文因此认为，下一步需要研究目标解释、数据选择、验证信号构造和评估时机决策本身，而不仅仅是优化显式目标。

论文的主要贡献包括：首次将模糊目标驱动的自我进化形式化为可研究问题；构建ASPIRE基准及统一交互环境，支持权重级和框架级进化；提供隐藏的专家评估集以测量外部进步；通过实证刻画了当前agent在模糊目标下的行为模式与失败模式。局限在于只聚焦能力提升的第一层以及该过程发起的开端，未来还需考虑环境构建和长期演进（依据限制片段）。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与agent方向直接相关（用户画像权重0.10）：论文研究LLM agent如何自主决定学习目标、数据和方法，属于agent自我进化与自主决策的核心议题。

## 基本信息

- 作者：Yuhao Wu, Jingyuan Zhang, Jiajun Shi, Yuxuan Zhang, Xinping Lei, Junting Zhou, Zexuan Wang, Yuchen Wu, Huan Zhou, Duo Wang, Yinzhu Piao, Yongchang Peng, Yunfeng Shi, Jin Chen, Zuo Wang, Jinkai Liu, Jiaheng Liu, Wenxuan Zhang, Shen Yan, Wenhao Huang, Ge Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.31111v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段（n=57，主要来自Abstract、Conclusion、Experiments、Background）及论文元数据；未获取完整PDF正文，部分推断和缺失细节已在相应位置标注。
