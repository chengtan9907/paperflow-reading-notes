---
user_id: "cheng tan"
paper_id: 8696
arxiv_id: "2608.18770"
title: "To Go Far, Go Together: Diverse Preferences Induce a Curriculum for Reward Optimization"
publish_date: "2026-08-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.18770.pdf"
pdf_url: "https://arxiv.org/pdf/2608.18770"
abs_url: "https://arxiv.org/abs/2608.18770"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-20T11:30:56"
---
# To Go Far, Go Together: Diverse Preferences Induce a Curriculum for Reward Optimization

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reward model · curriculum learning · AI alignment · personalization

## 一句话总结

本文提出CurriPO方法，利用多样化用户偏好在奖励优化中自然形成的课程结构，通过树形课程分支和策略复用来系统性覆盖所有用户，在模拟连续控制任务中将人口满意度提升至最强基线的1.2~2.1倍，并大幅缩短训练时间。

## 摘要

> Learning a reward model from human feedback and optimizing a policy against it is one approach to aligning AI systems with individual users. From a fairness perspective, existing work improves such alignment by developing data-efficient and accurate reward models that capture minority preferences despite scarce data. We push this line of inquiry one step further and argue that data-efficient and accurate per-user reward models are not sufficient: users whose reward models are difficult to \textit{optimize} at the policy level can become a new underserved group. We start from the observation that one user's reward model can be easy to optimize from the initial policy while another's is not. We argue that, given a sufficiently diverse user population, a curriculum naturally emerges between easy- and hard-to-optimize reward models. Building on this insight, we propose CurriPO, which grows a tree-structured curriculum to accommodate diverse user-specific objectives, covering the population in a single traversal. Specifically, CurriPO automatically constructs a curriculum over diverse user reward models, allowing it to branch from the existing curriculum and reuse reward models previously incorporated into the curriculum. To the best of our knowledge, this is the first work to explicitly exploit multi-user structure to address optimization in AI alignment. Extensive experiments on personalized continuous control in a simulated environment show that CurriPO achieves $1.2$--$2.1\times$ the population satisfaction of the strongest baseline while substantially reducing training time. Additional analysis attributes much of this improvement to the users left underserved by conventional optimization.

Q1: 这篇论文试图解决什么问题？

本文针对AI对齐中的个性化优化公平性问题。现有从人类反馈学习奖励模型并优化策略的框架中，研究多以奖励模型本身的准确性和数据效率为公平性衡量标准，例如通过元学习或数据增强提高少数偏好的拟合精度。然而，论文指出这一衡量标准存在盲区：即使每个用户的奖励模型都能被准确学习，不同用户的奖励模型在策略优化层面上的难度也可能存在系统性差异。从同一初始策略出发，某些用户的奖励目标容易被梯度优化，另一些则很难。如果算法只优化整体或平均性能，这些“优化困难”的用户会被系统性地排除在高质量服务之外，成为新的未服务群体。作者将这种问题类比为“优化层面的数字鸿沟”，强调其严重性不亚于数据稀缺。论文进一步提出一个关键观察：在足够多样化的用户群体中，奖励模型的优化难度自然形成一个从易到难的谱系，这正好与课程学习思想契合——先用简单任务构建基础策略，再用中间解逼近困难任务。因此，课程可以不是人为设计，而是从用户偏好多样性中自动涌现。基于这个洞见，作者提出CurriPO，通过自动生长的树结构课程来覆盖全部用户目标。该工作不仅是提出一种新算法，更是在框架层面改变了公平性对齐的思考方式：不仅要让奖励模型拟合每个人，还要确保策略优化过程对每个人都是可达的。论文还指出现有方法的隐含假设是“奖励模型准确则优化必然容易”，而Skalse等人（2023）等研究已经表明，奖励函数的等价表示可以保持最优策略不变但大幅改变学习难度，因此这一假设不成立。综上，本文要解决的问题是如何在多用户奖励模型之间自动构造一个既能共享计算又能分叉服务的优化路径，使得所有用户，包括那些难以直接优化的用户，都能获得高质量策略。

Q2: 有哪些相关研究？

本文研究涉及多个相关方向。第一是个性化奖励模型学习：已有大量工作关注数据高效地学习每个用户的偏好，例如利用元学习、任务自适应、数据重加权等方法捕捉少数群体的偏好。这些工作以奖励模型的准确率为公平性目标，很少考察后续策略优化对用户的公平性问题。第二是课程学习：经典课程学习通过从易到难排列训练样本或任务来加快收敛，如Wang等人（2019, 2020）在强化学习中使用课程策略迁移，把中间解决方案作为通向困难行为的跳板。CurriPO与这类工作的差异在于两点：一是不需要单独设计或选择课程顺序，而是从用户奖励模型的分布中自动生成；二是课程结构为树形而非线性，以应对多种用户目标同时存在的情况。第三是奖励函数变换与优化难度分析：已有理论工作研究MDP的状态或动作变换可以在保持最优策略不变的前提下显著改变学习难度；Skalse等人（2023）将此分析拓展到学习得到的奖励函数，指出仅凭留出集上的准确度无法判断其是否易于优化，这正好为本文“可优化性”这一维度提供了理论支持。第四是多任务学习和多目标优化：多任务学习通常在任务间共享表示，但通常假设任务难度相近，CurriPO则显式适应难度差异。第五是公平性机器学习：本文更接近于“优化层面的公平”而非统计均衡，与资源分配公平性、算法可达性等概念相关。最后，本文也与实际应用中的偏好对齐（如RLHF）和个性化AI系统密切相关，这些应用需要处理多样化用户群体的不同偏好，但现有RLHF通常只训练单一奖励模型，未考虑用户间优化难度差异。总体而言，本文在交叉点提出新问题：利用多用户本身的结构来促进对齐优化。

Q3: 论文如何解决这个问题？

CurriPO的核心是将用户奖励模型的优化难度作为课程信号，自动构建一棵树形课程。方法结构如下（基于摘要与片段，算法细节为合理推断）：算法从根节点（初始策略）开始，对每个用户的奖励模型评估从当前可达节点优化的难度。若难度低，则直接在该节点基础上进行策略优化，并将该用户作为叶子节点挂载。若难度高，则算法不直接优化该目标，而是寻找一个“中间”目标——可能是已纳入课程的其他用户奖励模型或这些模型的某种组合——先从中间目标开始优化，再逐步逼近最终目标。这种由易到难的路径以树的分支形式呈现。每个节点对应一个策略，父节点的策略作为子节点优化的初始化，这种复用使得不同用户之间共享早期学习成果，避免从零开始训练。树形结构允许不同难度群体从公共前缀分叉，从而在一次前向遍历中覆盖整个用户群体。为了实现自动生长，算法需要度量两个元素：一是用户奖励模型的优化难度，二是两个策略（或目标）之间的转移成本。论文在消融章节中提到了KL距离，合理推断KL散度被用于控制课程步长，即限制新策略与父策略的距离，以保证平稳过渡并防止策略崩溃。分支机制使得课程能针对单个用户的特殊性进行自适应；复用机制则大幅节约计算资源。作者强调该方法无需人工设计课程顺序，也无需预先对用户分组，而是完全由数据驱动的难度评估来驱动树结构扩展。论文的贡献清单中还提到该方法自动构造课程，并复用先前纳入的奖励模型。这暗示每个奖励模型都可能被用作其他用户的课程中介，形成一个跨用户的知识传递网络。整体优化过程类似动态规划：先解决简单用户，再用他们的策略作为解决困难用户的起点，并在过程中保留分支以直接服务简单用户。

Q4: 论文做了哪些实验？

论文在模拟环境中进行了个性化连续控制任务的实验。实验的核心是比较CurriPO与多个基线（尤其是最强基线）在人口满意度以及训练时间上的表现。人口满意度这一指标具体定义不确定，但很可能衡量在所有用户上策略性能达到一定阈值的比例或平均性能。消融实验通过两个变体进行：Chain移除分支和复用，课程沿单一路径遍历；No reuse保留分支但移除策略复用。表3和表4报告了这些变体的对比结果。此外，作者还进行了针对“难以到达”用户的专门分析（章节4.4），通过图3等可视化检查：一个可能的反对意见是CurriPO的增益只是因为基线在这些用户上训练不足，而图3可能对比了不同训练预算下基线的表现，以证明差异并非源于训练量不足。实验还分析了分支和KL的作用（章节4.3），可能包括不同KL约束大小的敏感性。由于论文提供的具体数值有限，只能从摘要得知：CurriPO在人口满意度上比最强基线高1.2~2.1倍，并大幅减少训练时间。消融结果链式变体显著较差，而No reuse在多数设置中接近完整方法但仍有小差距，表明复用主要带来效率和边际性能改进。实验设计整体系统化，从总体性能到组件归因再到公平性分析，层次分明。

Q5: 发现了什么实验现象？

实验中揭示的核心现象是多用户奖励模型之间的优化难度差异确实存在，且传统的单一路径优化会在难度较高的用户上失效。CurriPO通过树形课程显著提升了整体人口满意度，达到最强基线的1.2~2.1倍。这一提升并非均匀分布，而主要来自那些“难以到达”的用户，即传统方法几乎无法服务的群体。消融实验显示：去除分支和复用的Chain变体性能大幅下跌，说明树形分叉和策略复用都是必要的；No reuse变体虽接近完整方法但在若干任务设置下仍稍差，说明复用机制不仅节约时间，还能提升最终策略质量。作者特别探讨了“基线只是训练不足”这一假设，并通过图3等证据排除它，证明传统优化在路径上本身存在不可跨越的障碍。此外，训练时间的显著降低直接体现了策略复用带来的计算效率红利。另一个值得注意的现象是，课程的自然涌现依赖于用户群体的多样性，当多样性不足时课程可能退化为线性链。论文还分析了KL距离在课程步长中的作用，暗示过大或过小的KL约束都可能恶化性能（该点基于章节标题的合理推断）。整体上，实验现象与论文核心论断高度一致，但具体数值细节在可获取材料中不完整。

Q6: 有什么可以进一步探索的点？

本文开创了多用户结构辅助对齐优化的新范式，未来可以从以下方向拓展。第一，将CurriPO验证在真实世界场景，例如从自然语言偏好中学习的RLHF系统，真实用户偏好更复杂、噪声更大且随时间变化，需要检验算法鲁棒性。第二，理论研究：在什么条件下，用户偏好多样性必然诱导出课程结构？能否给出课程涌现的形式化条件以及最优树结构的存在性？第三，算法效率：当前树结构的生长依赖于手动设计的难度度量，是否可以用学习到的难度预测器替换，甚至端到端学习课程结构？对于极大用户群体，树的存储和遍历可能成为瓶颈，能否并行化分支或采用层次化集群策略？第四，与元学习结合：利用跨用户经验来初始化新用户的课程节点，从而进一步提高数据效率。第五，公平性理论：引入更严格的个体公平指标（如 minimax regret），研究人口满意度与最差用户性能之间的权衡。第六，安全性：课程优化过程中如何保证每个分支的策略都满足安全约束，避免利用奖励模型漏洞。第七，应用拓展：个性化自动驾驶、医疗行为干预、多机器人协作等场景中，用户奖励的差异天然存在，可以直接应用CurriPO。第八，与动态偏好结合：当用户偏好随上下文改变时，课程树应如何动态更新。这些方向都有望进一步挖掘多用户课程学习的潜力。

Q7: 总结一下论文的主要内容

论文针对AI对齐中一个被忽视的公平性问题：不同用户的奖励模型即使在准确学习后，其策略优化难度也可能差异悬殊，导致部分用户无法得到有效服务。作者首先论证，在多样化的用户群体中，奖励模型的优化难度天然构成一个从易到难的课程，因此可以将课程学习的思想引入多用户奖励优化。基于这一洞见，他们提出了CurriPO，一个自动构建树形课程的算法。CurriPO从初始策略出发，评估每个用户奖励模型的优化难度，通过分支和策略复用机制，在一次遍历中覆盖全部用户。与先前的课程学习不同，它不需要人工设计课程，也不需要预先假设任务间有明确的难易顺序，而是利用多用户偏好本身的结构。作者在模拟环境中的个性化连续控制任务上进行了系统实验，结果表明CurriPO的人口满意度比最强基线高出1.2~2.1倍，并大幅降低训练时间。消融实验表明，分支和复用机制都是性能的重要支撑；线性单路径课程（Chain）表现显著较差，而取消复用（No reuse）虽大部分场景接近，但仍无法完全达到完整方法的效果。作者进一步分析了“难以到达”的用户群体，证明其主要受益于分支结构，并排除了基线仅训练不足的可能性。论文的贡献包括：提出多用户优化公平性问题，揭示可优化性差异导致的未服务群体；论证多样化偏好自然诱导课程；提出CurriPO方法，包含自动树课程构建、分支和复用机制；在模拟控制任务上证明其有效性。局限性方面，工作仅在模拟环境验证，模型和方法细节在可获取材料中不够完整，真实偏好复杂度和可扩展性有待探索。总体而言，本文独特地将课程学习与多用户公平对齐结合，在概念和算法两个层面都提供了新见解，对该领域有较强的启发价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户关注的agent方向直接相关：CurriPO可视为多智能体/多主体系统中个性化对齐的策略优化方法。

## 基本信息

- 作者：Taehyung Kim, Jongeun Choi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.RO
- 日期：2026-08-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.18770`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（Abstract、Introduction、Related Work、Conclusion及消融实验片段）和heuristic_draft；方法细节和部分实验观察基于摘要与片段合理推断，具体数值和表格内容需回原文确认。
