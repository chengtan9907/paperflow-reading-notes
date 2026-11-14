---
user_id: "cheng tan"
paper_id: 21574
arxiv_id: "2611.15766v1"
title: "Prior-as-Skill: Remember Outside and Read Inside with On-Policy Self-Distillation for Self-Evolving VLM Agents"
publish_date: "2026-11-11"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.15766v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.15766v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:49:35"
---
# Prior-as-Skill: Remember Outside and Read Inside with On-Policy Self-Distillation for Self-Evolving VLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-evolving agents · on-policy self-distillation · vision-language models · visual skills

## 一句话总结

论文提出 PriorSplit：把自演化 VLM agent 的技能步骤顺序保存在模型外的计数先验中，并用可见性检查在推理时决定读帧还是查先验，从而在 OPSD 训练成本不变的情况下同时应对缺帧和异常顺序。

## 摘要

> sive experiments on synthetic skills with five models and on
> step-tagged order questions in new episodes of seen web,
> Self-evolving visual agents keep what they learn as skills: cooking and egocentric tasks demonstrate that, with frames
> key frames with step notes on a task’s steps and their order. missing, PriorSplit outperforms every compared method
> On-policy self-distillation (OPSD) is now a popular way to at OPSD’s training cost. With Qwen3-VL-4B on synthetic
> train such skills into a vision-language model that matches, skills, it also stays close to OPSD on unusual orders, where
> on its own answers, a teacher that also reads the written methods that gain most by storing the order fall behind.
> order. At deployment, frames often go missing or show an
> unusual order, as when a person or a website reorders the
> steps. Existing internalization methods keep any order they 1. IntroductionarXiv:2611.15766v1
> store in the frame-reading weights: trained on full frames,
> the model guesses when they are missing, and trained with Self-evolving agents turn past runs into reusable skills [49]
> frames withheld, it stores the order, so memory can override and increasingly internalize them, so a skill helps after leav-
> what it sees. We propose PriorSplit, which fills in miss- ing the context window [23, 43, 61]. The agent must then
> ing frames from an order kept outside the model, with no answer from what it remembers and what it sees. For a vi-
> training beyond OPSD. It counts which of two neighbor- sual agent, a skill is a keyframe sequence with step notes
> ing steps comes first in training episodes, and the model on how a task unfolds on a website [9, 59] or in egocen-
> checks if both asked steps are in view: if so, it answers tric videos of cooking and other tasks [27, 46]. On-policy
> from the frames, and otherwise the counts answer. Exten- self-distillation (OPSD) [57] suits this: a teacher given priv-
> ileged information supervises the student’s own outputs, so
> ∗Corresponding author.
> the student matches it without that information. Contact: y.lin64@ncl.ac.uk
> zhongliang.guo@abdn.ac.uk At test time, frames fail the student in two ways. They go

Q1: 这篇论文试图解决什么问题？

一、论文要解决的问题不是“如何把技能顺序记得更牢”，而是“如何让顺序记忆在缺帧时补位、在视觉证据出现时让位”。自演化视觉 agent 把过去回合压缩成技能：关键帧序列加步骤笔记，步骤的规范顺序是技能的一部分。OPSD 让持有特权顺序信息的教师监督学生自己的输出，把这种顺序蒸馏进 VLM 的 reader 权重。问题在于部署分布会偏移：帧可能缺失，网站或人可能重排步骤。若把顺序存进同一套帧阅读权重，模型就难以区分“我记得的顺序”和“我现在看到的顺序”；全帧训练时缺帧只能猜，去帧训练时顺序又会覆盖视觉。论文把这一耦合诊断为现有内化方法的根本缺陷。

二、形式化：技能 s 是规范序列 u1...uL，每个 unit 是关键帧加短步骤笔记；一个 episode 只展示 K 个 unit 的窗口，呈现顺序 S 受 per-skill 强度 δ 的顺序噪声影响；问题 q 点名两个在规范顺序中相邻的 unit X、Y，问在 S 中谁先出现。这个设定把技能顺序压缩成局部成对比较，便于计数，但也意味着更长的依赖、部分序和循环序不在建模范围内（合理推断）。

三、难点：1）缺帧时没有视觉证据，必须依赖先验；2）有帧但顺序异常时，先验可能是错的，必须让视觉优先；3）先验不能只存在于模型权重中，否则无法审计和切换；4）检查器本身会错，检查漏检会把先验错误地放到可见项上；5）训练回合有限，稀有步骤对的计数不可靠；6）真实 web/烹饪任务的顺序统计可能很弱，先验收益依赖数据中的稳定规律。

四、隐含假设：规范相邻关系已知；训练回合与部署回合的顺序分布大体同分布；检查器能判断点名步骤是否在画面中；问题只涉及两个点名步骤；“正确”由当前呈现顺序定义，而不是由规范顺序定义。论文用 Theorem 2 处理“独立但不同分布”的顺序生成，用 Proposition S3 说明正确检查时先验不进入可见项；这些假设在真实部署中是否成立，是阅读时应重点核对的地方。

Q2: 有哪些相关研究？

一、自演化 agent 与技能记忆：论文把技能视为可复用、可内化的经验，引用 self-evolving agents [49] 和上下文窗口限制 [23,43,61]。两条路线是外部记忆/检索与参数内化。PriorSplit 属于“外置记忆 + 推理时查询”，但记忆不是原始轨迹，而是成对顺序计数。

二、OPSD 与特权信息蒸馏：OPSD [57] 用教师持有特权信息（书面顺序）监督学生自己的 on-policy 输出，避免离线轨迹分布偏移。相关谱系包括 privileged information distillation、on-policy distillation、imitation/DAgger 式学生自身状态监督。论文的差异是：不把特权信息继续压进学生权重，而是把顺序计数留在模型外。

三、视觉 agent 的顺序问答：web agent [9,59] 与 egocentric/cooking [27,46]；数据集包括 Mind2Web、HoloAssist、CaptainCook4D；技能由关键帧加步骤笔记表示。顺序题问两个规范相邻步骤在呈现顺序中谁先。

四、记忆-感知冲突：认知上存在“记忆覆盖知觉”；工程上参数记忆、上下文记忆、检索记忆与当前观测冲突。PriorSplit 用检查器做路由，把冲突显式化；不同于简单 retrieval-augmented，它不检索整段轨迹，而是查成对计数。

五、计数先验与贝叶斯平滑：Laplace/Beta(c,c) 后验均值 (n_XY+1)/(n+2)，裁剪到 [0.02,0.98]；对称 Beta 的性质使后验均值落在计数较大一侧（Lemma S2）。这连接 Markov/顺序统计、稀疏计数校准、选择性预测。

六、路由与选择性预测：check-then-answer 类似 selective prediction、mixture-of-experts、abstention。区别是路由信号来自 VLM 自身的可见性判断，且不额外训练；错误分析聚焦 check miss。

七、与论文最接近的基线：OPSD、全帧内化、去帧/子序列变体、仅先验/仅 reader。论文主张这些基线在“缺帧收益”与“冲突准确率”之间存在结构性权衡。

Q3: 论文如何解决这个问题？

一、总体：PriorSplit 不改变 OPSD 的 reader 训练，只增加两个推理组件：外部计数先验 p_s 和可见性检查 g。训练阶段仍用 OPSD teacher（特权顺序）蒸馏 student；部署阶段 reader 先检查问题点名的 unit 是否在 V 中，g=1 时读帧，g=0 时查先验。

二、技能与问题：技能是规范序列，每个 unit 是关键帧加短步骤笔记；episode 展示窗口 K，呈现顺序 S 受顺序噪声 δ 影响；问题点名两个规范相邻 unit X、Y，问在 S 中谁先。PriorSplit 只针对这类成对顺序问题。

三、计数先验：对每个规范相邻对 (X,Y)，统计训练 episode 的呈现顺序 S 中 X 先于 Y 的次数 n_XY 和总次数 n；后验均值写作 p_s(X first) = (n_XY + 1)/(n + 2)，并裁剪到 [0.02,0.98]，使稀有对接近 1/2。对称 Beta(c,c) 先验使后验均值落在计数较大的一侧，只有这一侧会进入答案（Lemma S2）。

四、Check：对每个点名 unit u，reader E 回答“u 是否 shown?”，用 log-ratio ℓ_u = log p_E(y | q,V) 与阈值 0 判定；g = 1[ℓ_X>0] ∧ 1[ℓ_Y>0]；最多两次检查。检查本身是 reader 的可见性判断，不是额外训练的分类器（具体冻结关系需回原文核对）。

五、Switch：g=1 时用 reader 从帧中读出的答案；g=0 时用先验 argmax。这样正确检查时先验不进入可见项，视觉证据优先；只有缺帧或检查认为缺帧时才由计数补位。

六、为什么不是训练：顺序是离散、可计数、可更新的；训练进权重会与视觉特征纠缠；外部计数可审计；不增加 OPSD 训练预算。片段中的 optional probe 用 L2 logistic/ hidden state / LoRA 等轻量适配尝试学习计数，但 frame-free adapter fitted to the counts changes short-side accuracy by −0.7 points（Table S9），说明学习式适配没有超过简单计数，主方法不依赖它。

七、理论：Proposition S3 说明正确检查时先验不进入可见项；Theorem 2 给出 check miss 比例 α 下的误差分解/界；计数在独立但非同样分布的顺序生成下有界；对称 Beta 性质保证计数较大侧才可能改变答案。片段给出的误差分解形如 A*−A = v(1−R_vis) + vLα + (1−v)Lβ，符号与项义需回原文核对。

八、与“把顺序训进权重”的对比：内化方法把顺序存进 reader，缺帧收益与冲突风险绑定；PriorSplit 把顺序外置，用检查器显式路由，代价是检查器错误和两次额外查询。

Q4: 论文做了哪些实验？

一、实验问题：1）OPSD 是否真的把顺序存进 reader？2）用子序列/去帧训练能否分离记忆与感知？3）PriorSplit 在缺帧时是否优于对比方法且训练成本不变？4）在异常顺序/冲突可见时是否仍以视觉为准？5）检查与计数各自贡献多少？6）跨五个模型和真实任务是否成立？7）学习式 probe 是否必要？

二、数据与模型：合成技能（可控顺序噪声 δ、窗口 K、序列长度）；Mind2Web（web）；HoloAssist、CaptainCook4D（egocentric/cooking）；评估的是已见任务的新回合顺序题。摘要明确五个模型，片段明确 Qwen3-VL-4B；完整模型清单需回原文。

三、条件与基线：全帧训练、缺帧训练、异常顺序、冲突可见；对比族包括 OPSD、全帧内化、去帧/子序列变体、仅 reader、仅先验等。片段提到共享 LoRA 调度、OPSD 训练预算相同，说明公平性控制至少包括训练成本与适配器调度。

四、指标：缺帧顺序准确率、冲突准确率、可见准确率、短边/稀有对准确率、检查成本、训练成本。片段表格出现 9、1600/300/1400/3200/1800、97、154、173、74.1 等数字，但列名与行名因双栏错位无法可靠对齐，不能安全解释为某一具体指标。

五、主要结果：缺帧时 PriorSplit 在合成技能（五个模型）以及已见 web、烹饪、第一视角任务的新回合顺序题上优于所有对比方法，训练成本与 OPSD 相同；Qwen3-VL-4B 合成技能上，异常顺序上仍接近 OPSD，而靠存储顺序获得缺帧收益的方法落后；冲突准确率只在检查漏检时下降。

六、消融与负结果：OPSD 几乎不存顺序；所有测试的子序列变体都把顺序存进同一套权重并损失冲突准确率；frame-free adapter 相对计数先验在短边准确率上变化 −0.7 点，说明学习式适配没有带来稳定增益；检查漏检是主要失败模式。

七、证据缺口：摘要截断、两栏排版错位，Introduction/Related Work 为空，表格数字与列名对应关系不可靠；片段出现真实数据上的 chance-level 数字（53.8%、52.6%、52.0%），但上下文被截断，不能确认具体指标，需回原文核对。

Q5: 发现了什么实验现象？

1. OPSD 的顺序内化很弱：结论明确说在 Qwen3-VL-4B 合成技能上 OPSD stores almost no order。这与“教师持有特权顺序，学生应该能记住”的直觉相反，说明蒸馏目标并不自动把顺序压进 reader；缺帧时它更像在猜。

2. 存顺序的收益与代价强绑定：全帧训练时缺帧靠猜；去帧/子序列训练能把顺序存进权重，但同一套权重会在可见顺序异常时覆盖视觉证据，导致冲突准确率下降。结论说 every tested subsequence variant stores it in the same weights and loses conflict accuracy，这是一个结构性负结果：不是调参能轻易解决的。

3. PriorSplit 的路由现象：正确检查时先验完全不进入可见项，因此异常顺序下仍接近 OPSD；缺帧时先验补位，因而在同训练成本下领先。它把“记忆收益”和“视觉优先”拆成两个可切换的路径，而不是让一个权重同时承担两者。

4. 检查器是单点故障：冲突准确率只在 check misses 时下降。也就是说，方法的主要误差不是计数先验本身，而是“把可见误判为不可见”的漏检；这提示后续工作应优先做检查器校准、弃权和多证据融合。

5. 简单计数优于学习式 probe：frame-free adapter fitted to the counts changes short-side accuracy by −0.7 points（Table S9）。这暗示在这个任务上，稀疏计数加平滑已经足够，学习式适配可能过拟合或引入额外方差；是一个值得注意的“less is more”负结果。

6. 稀有对接近 1/2：裁剪与对称 Beta 使稀有步骤对不做出过度自信的判断。好处是校准和保守，代价是长尾技能上先验几乎不提供信息；这可能是真实任务收益受限的原因之一。

7. 异常顺序上的张力：靠存储顺序获益最多的方法在 unusual orders 上落后，而 PriorSplit 仍接近 OPSD。这说明“缺帧准确率”单独看会奖励错误的内化方式；必须同时看冲突准确率，否则会得出相反结论。

8. 真实数据信号可能偏弱：片段出现 53.8%、52.6%、52.0% 的 chance-level 描述，若这些数字指真实数据顺序题的机会水平，则说明 web/烹饪/第一视角任务中顺序统计本身不稳定，PriorSplit 的收益更依赖合成技能或稳定子分布；该判断需回原文表格确认。

9. 训练成本现象：PriorSplit 不训练新模块，计数离线完成，因此“更好缺帧准确率”不是用更多梯度换来的；这与常见“加一个适配器/加一轮训练”的路线形成对比。

10. 检查成本现象：每个问题最多两次可见性查询；相对长视频理解本身成本不高，但在多问题、长horizon agent 循环中会累积，论文未给出大规模部署成本曲线。

Q6: 有什么可以进一步探索的点？

1. 从成对相邻顺序扩展到更长程依赖：高阶 Markov、部分序/DAG、层级子任务、循环步骤、并行步骤。当前只问两个规范相邻 unit 谁先，无法表达“整个流程被重排”或“多个步骤交换”。

2. 在线更新与漂移适应：把计数先验做成可增量更新的外部记忆，加入 recency weighting、change-point detection、per-website/per-user 分层先验。真实 web 会改版，烹饪任务会因厨房/工具不同而变序，静态计数会过时。

3. 检查器校准与弃权：检查漏检是主要失败模式，可研究 conformal prediction、温度校准、多帧证据、跨模态一致性、主动再看一帧、显式 abstain。也可以训练轻量 checker，但必须控制 false positive，否则先验会重新覆盖视觉。

4. 多证据融合：当前检查只回答两个点名 unit 是否 shown；可扩展到遮挡、相似关键帧、步骤笔记歧义、多个候选帧、视频时序证据。把“是否可见”从二分类升级为带不确定性的证据分数。

5. 理论扩展：在非独立、对抗、非平稳顺序下给出 finite-sample bound；分析 check error 与 prior error 的联合影响；研究计数先验的 regret 与样本复杂度；把 Theorem 2 的独立不同分布假设放宽。

6. 与检索/记忆系统结合：PriorSplit 只存成对计数，可与其他外部记忆（轨迹检索、技能库、程序性记忆）组合；研究何时查计数、何时检索完整轨迹、何时直接读帧。

7. 评价协议：把冲突准确率、缺帧准确率、可见准确率、检查成本联合成标准 benchmark；覆盖 unseen tasks、跨域迁移、长horizon、流式视频、多轮交互；避免只用缺帧准确率奖励“记忆覆盖视觉”。

8. 与其他 agent 模块耦合：把顺序先验用于规划、工具调用、步骤补全、错误恢复；研究先验不确定性如何进入 planner，而不是只在问答层做路由。

9. 跨领域迁移：AI-for-science 的实验协议顺序、操作步骤、传感器缺帧；工业流程、医疗护理、教学视频；这些场景的顺序统计可能比 web 更稳定，也可能因协议变更而漂移。

10. 安全与审计：外置计数可审计是优点，但顺序统计可能泄露任务结构或用户习惯；需要研究隐私、访问控制、恶意重排下的鲁棒性。

Q7: 总结一下论文的主要内容

一、论证主线：论文从一个部署矛盾出发。自演化视觉 agent 把过去的技能压缩成关键帧加步骤笔记，并用 OPSD 把教师看到的书面顺序蒸馏进 VLM 的 reader。训练时教师有特权顺序，学生只看帧；部署时帧可能缺失，人或网站也可能重排步骤。现有内化方法把顺序存进 reader 权重，于是“记得的顺序”和“看到的顺序”纠缠在一起：全帧训练时缺帧只能猜；去帧/子序列训练时顺序记得住，却会在可见顺序异常时压过视觉。论文因此提出 Prior-as-Skill：顺序应该保存在模型外，reader 只负责读帧，推理时用一个可见性检查决定“读里面”还是“查外面”。

二、技术主线：PriorSplit 由三部分组成。1）外部计数先验：对每个规范相邻对 (X,Y)，统计训练 episode 的呈现顺序中 X 先于 Y 的次数，用 Laplace/Beta(c,c) 后验均值 (n_XY+1)/(n+2) 作为 p_s，并裁剪到 [0.02,0.98]，使稀有对接近 1/2；对称 Beta 性质保证计数较大的一侧才可能改变答案。2）可见性检查：reader E 对问题点名的每个 unit 回答“是否 shown”，用 log-ratio ℓ_u = log p_E(y|q,V) 与阈值 0 判定；g = 1[ℓ_X>0] ∧ 1[ℓ_Y>0]，最多两次查询。3）Switch：g=1 时从帧回答，g=0 时由计数先验回答。除 OPSD 外不训练新模块；片段中的 optional probe 用 L2 logistic/hidden state/LoRA 尝试学习计数，但相对简单计数在短边准确率上变化 −0.7 点，说明学习式适配没有稳定增益。

三、理论主线：Proposition S3 说明正确检查时先验不进入可见项；Theorem 2 分析 check miss 比例 α 下的 switch 误差，并给出独立但非同样分布顺序生成下的计数界；Lemma S2 说明对称 Beta 先验下只有计数较大侧会进入答案。作者把误差拆成可见 reader 误差、检查漏检误差和先验误差三部分；片段给出的分解形如 A*−A = v(1−R_vis) + vLα + (1−v)Lβ，具体符号与权重需回原文核对。理论的核心不是证明 PriorSplit 全面更优，而是说明它把错误集中到 check miss，从而给出可解释的失败模式。

四、实验主线：实验覆盖合成技能（五个模型，明确 Qwen3-VL-4B）与 Mind2Web、HoloAssist、CaptainCook4D 的已见任务新回合顺序题。条件包括全帧、缺帧、异常顺序、冲突可见；对比族包括 OPSD、全帧内化、去帧/子序列变体、仅 reader、仅先验等。主要发现是：OPSD 几乎不存顺序；所有测试的子序列变体都把顺序存进同一套权重并损失冲突准确率；PriorSplit 在缺帧时以 OPSD 同等训练成本优于所有对比方法，在异常顺序上接近 OPSD，而靠存储顺序获益的方法落后；冲突准确率只在检查漏检时下降。

五、结论与边界：论文的结论是“技能顺序应当外置，视觉证据应当优先；检查器决定何时查记忆”。边界也很清楚：只处理两个规范相邻步骤的成对顺序；依赖训练回合与部署顺序分布大体一致；检查漏检是主要失败模式；评估是已见任务的新回合，不是未见任务/跨域迁移；真实数据上的顺序信号可能偏弱（片段出现 chance-level 数字，需核对）。因此它更像一个“记忆-感知分离”的系统性范式，而不是一个在所有真实场景都立即涨点的通用模块。

六、阅读判断：这篇论文的价值主要在问题诊断和路由设计，而不是复杂模型结构。它把“内化”与“外置”的权衡显式化，并用负结果说明继续把顺序训进权重不是免费收益。若关注 agent 记忆、技能内化、OPSD、视觉顺序问答，这篇值得读；若关注长程/层级/开放世界技能，需注意它只覆盖局部成对顺序。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：自演化 agent 的技能内化、外部记忆、记忆-感知分离、推理时路由，都是当前 agent 系统的核心问题。

## 基本信息

- 作者：Yan Lin, Jingyu Sun, Yifei Qian, Zhengtao Yao, Yuxia Wang, Daizong Liu, Chun Pong Lau, Zhongliang Guo, Stephen McGough
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-11-11
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.15766v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索命中的 Method 与 Conclusion 片段，而非受摘要截断影响的 heuristic_draft；摘要存在截断和双栏错位，具体表格数字、模型清单与作者机构未能可靠恢复，相关不确定性已在字段内标注。
