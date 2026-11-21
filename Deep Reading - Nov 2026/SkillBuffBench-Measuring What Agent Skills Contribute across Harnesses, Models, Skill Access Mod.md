---
user_id: "cheng tan"
paper_id: 22283
arxiv_id: "2611.22017v1"
title: "SkillBuffBench: Measuring What Agent Skills Contribute across Harnesses, Models, Skill Access Modes and Memory"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.22017v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.22017v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:11:19"
---
# SkillBuffBench: Measuring What Agent Skills Contribute across Harnesses, Models, Skill Access Modes and Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent skills · agent benchmark · coding agents · agent harness

## 一句话总结

SkillBuffBench 是一个把“Agent Skill 的可用方式”设为三水平因子（无 Skill／人工精选 Skill／从已安装库自动调用）、并与 memory 开关、四个 agent harness、十余个前沿模型做全因子交叉配对评测的基准与协议，任务由材料科学与生物学领域专家连同其自用 Skill 一起贡献，当前版本只给出设计与任务，评测仍在进行、尚无结果。

## 摘要

> Agent Skills, file-based packages of procedural knowledge that a coding agent can discover and load on demand, are now shipped by every major agent harness, yet the evidence for how much they help comes almost entirely from settings in which the right Skill is hand-picked for the task and the agent starts from a blank slate. Neither holds in deployment, where an agent chooses from whatever library happens to be installed and frequently carries memory from earlier work. We present SkillBuffBench, a benchmark and paired evaluation protocol that treats Skill access as a three-level factor (no Skill, a curated Skill, and automatic invocation from an installed library), crosses it with memory (off and on), with four agent harnesses and with multiple models (the state-of-the-art proprietary models from Anthropic, Google and OpenAI, and the state-of-the-art open-weight models from Chinese providers), every model being run under every harness, and evaluates the result on research tasks contributed by domain experts together with the Skills they use, currently in materials science and biology, with the suite extended on a rolling basis. Every run is marked by a deterministic verifier; uncertainty is reported with a paired, task-level bootstrap; and cost, agent rounds and wall-clock time are reported alongside the score rather than folded into it. This version of the paper describes the design of the benchmark, its evaluation protocol and its current tasks and Skills. The evaluation is in progress: results will be added once the runs are complete, and the task suite, results and analysis will continue to be updated on a rolling basis.

Q1: 这篇论文试图解决什么问题？

【一、现实设定与评测设定之间的系统性错位】
1) Agent Skills 已经成为每个主流 agent harness 的标配交付物（以文件形式打包的过程性知识，agent 可自行发现并按需加载），但“它到底帮了多少”的证据几乎全部来自两种理想化设定：其一，正确的 Skill 被人工挑好并塞给 agent；其二，agent 从空白状态（blank slate）出发。
2) 这两条在部署中都不成立：真实 agent 面对的是“这台机器上恰好装了什么库”，必须自己判断该不该用 Skill、该用哪一个；同时它常常带着此前工作留下的 memory 继续干活。因此既有证据存在方向明确的向上偏置——人工路由抹掉了检索与选择错误，空白起点抹掉了记忆污染与锚定效应。
3) 这带来一个归因（attribution）难题：Skill 的收益与 harness 的脚手架（工具集、上下文管理、回合控制、沙箱策略）以及模型自身能力纠缠在一起。只在一个 harness、一个模型上做“有 Skill vs 无 Skill”，无法把 Skill 的贡献从 harness 的贡献中剥离出来；论文的解法是把 harness 与 model 都显式变成因子，并在同一 (harness, model) 配置内做配对比较。

【二、测量层面的缺口】
4) 缺少配对结构：不同条件跑在不同任务子集或不同重复次数上时，方差与任务难度会与条件混杂，点估计之间不可直接比较。
5) 缺少成本维度：Skill 会带来额外的上下文加载、更多 agent 回合与更长 wall-clock。若把成本折进分数，或者干脆不报，读者无法判断“涨的点是否值得”。论文明确选择把 cost、rounds、wall-clock 与分数并列报告。
6) 缺少不确定性陈述：单次运行的点估计常被当作结论；论文要求用任务级别的配对 bootstrap 报告不确定性。
7) 缺少对“元认知”的考核：论文在任务设计里明确写道，产生两个数字很容易，知道什么时候不该相信这两个数字才是被考的 skill——即模型是否能给出恰当的置信区间、是否知道自己拟合得“好”但结论可能错。

【三、科学任务带来的额外困难】
8) 在材料／生物等科研任务上，“拟合残差小”并不等于“结论正确”：例如粉末衍射图样可以被拟合得很漂亮，但物相判错，后面全错。通用 LLM benchmark 的评分范式（文本匹配、单一标量指标）承接不了这种多产物、带门控（gate）的正确性判定；论文因此让每个任务自带由专家选定的判据，而不是套用统一指标。
9) 要把“隐性的过程性知识”变成可操纵的实验因子，前提是有专家贡献任务、并同时贡献他们自己用的 Skill——这也是该基准在组织方式上区别于合成任务的地方。

【四、问题的可检验化】
10) 于是核心问题被重述为三个可检验的研究问题：RQ1，精选 Skill 相对无 Skill 是否提升任务成功率；RQ2，从已安装 Skill 库自动调用相对无 Skill 是否提升成功率，以及它保留了多少精选 Skill 带来的增益；RQ3，memory 是否改变成功率、成本、agent 回合数或 wall-clock 时间，以及它是否改变 Skill 的效应（即是否存在交互作用）。
11) 隐含假设（需要在阅读时留意）：Skill 的价值主要体现为过程性知识的可复用性；专家贡献的 Skill 是“高质量上界”的合理代理；确定性 verifier 能覆盖任务的核心正确性而不误伤合理的替代解法（这一点在任务带门控的二元判定上尤其敏感，见 limitations）。

Q2: 有哪些相关研究？

【说明】本次 PDF 语义检索只命中 Abstract 与 Method 章节，未检索到 Related Work 原文；以下归类是基于摘要与方法中对“对照对象”的提及所做的合理推断，具体引用与对比结论需回原文核对。

【一、Agent Skill 与过程性知识封装】
1) 与 Skill 直接同源的线索是“以文件打包、agent 按需发现并加载的过程性知识”；论文指出每个主流 harness 都已随附交付此类 Skill，说明这条线已从研究原型进入产品化阶段。该方向的关键问题不是“Skill 能不能写出来”，而是“Skill 在什么条件下被正确选中、被正确执行、以及收益如何归因”。
2) 与之相邻的是工具使用（tool use）与检索增强：Skill 与工具的区别在于它更像“操作手册／工作流”，而非单次函数调用；因此检索正确性与执行正确性是两级失败点，这正好对应 RQ2 的设计（自动调用相对精选 Skill 的增益保留率）。

【二、Agent 基准与评测协议】
3) 该工作属于“带 harness 的 agent 基准”这一族：与只看模型裸能力的评测不同，它把模型放进 harness 里跑，并且把 harness 本身当作因子。这呼应了近期关于“harness 脚手架差异可能大于模型差异”的一类关切（合理推断，原文对比对象未检索到）。
4) 协议层面它对齐了三条较新的评测实践：确定性 verifier 打标、任务级配对 bootstrap 报告不确定性、成本与时间作为并列指标而非折算项。这与把多指标压成单一分数的排行榜式基准形成方法论上的分野。

【三、Agent memory】
5) memory 方向通常研究“跨会话保留什么、如何检索与遗忘”；本基准不提出新的 memory 机制，而是把 memory 的有无作为因子，去问一个更朴素的问题：带着记忆干活，成功率、成本、回合数与时长会怎么变，以及它是否会改变 Skill 的效果。这相当于把 memory 从“方法创新对象”转为“环境变量”。

【四、AI for Science 基准】
6) 现有科学 agent 基准多集中在“能不能跑通流程／能不能回答问题”，本工作选择的是科研日常中高频、且对错可被确定性判定的分析步骤：粉末衍射的物相鉴定与晶胞精修、SIMS 数据的扩散分析（要求给出 D*、k* 及其 95% 置信区间）、以及核（nucleus）中心一对一匹配与表面距离／体积相对误差这类分割指标。这与“科学数据处理复现”类基准同族，但更强调专家贡献的判据与 Skill 绑定。
7) 差异点在于：任务与 Skill 成对贡献，评测的是“有／没有这份过程性知识”的差，而不是“模型懂不懂这门学科”。

Q3: 论文如何解决这个问题？

【一、总体设计：把“Skill 访问”做成三水平因子并与其他因子全交叉】
1) 因子 A（Skill 访问，3 水平）：无 Skill（no Skill）／精选 Skill（curated Skill，为任务人工指定）／从已安装 Skill 库自动调用（automatic invocation）。这一设置的价值在于把“Skill 本身的质量”与“路由（选不选得对）”分开：精选条件给出上界，自动调用条件给出可部署值，两者之差就是路由损失。
2) 因子 B（memory，2 水平）：M0 记忆关闭——每次运行都是一个新 agent、新会话；M1 记忆开启（原文该段在检索片段中被截断，具体记忆载体与写入／检索策略需回原文确认，“跨运行保留”为合理推断）。
3) 因子 C（harness，4 水平）：Ginkley（由 BraneMatrix 开发）、Codex（OpenAI）、Claude Code（Anthropic）、OpenCode。
4) 因子 D（model，n 水平，当前 n=11）：所有 harness 共享同一个模型池；池子随新模型发布滚动更新。池中包含 Anthropic、Google、OpenAI 的前沿闭源模型，以及来自中国厂商的前沿开放权重模型。关键设计是“每个模型在每一个 harness 下都跑”，因此 harness 与 model 完全交叉，不存在“某模型只配某 harness”的混杂。
5) 交叉后的实验是四因子全因子设计；一个“配置”定义为 (harness, model) 对；在同一配置内，同一任务的各次运行之间只变 Skill 条件与 memory 模式。因此支撑 RQ1–RQ3 的所有比较都在 task、harness、model 上配对。

【二、任务与 Skill 的组织】
6) 任务由领域专家贡献，并连同专家实际使用的 Skill 一起提交；当前覆盖材料科学与生物学，按滚动方式扩充。当前任务套件为 4 个任务，每个单元三次重复。
7) 每个任务自带由专家选定的判据，而不是套用统一指标——这一点是设计上的关键取舍：因为“拟合残差小”不能推出“物相判对”，通用指标会把“漂亮但错误”的结果判成高分。

【三、评分：门控 + 连续分】
8) 每个任务的分数落在 [0,1]（对应原文第 2.9 节）。其中含一个门控因子 G：当识别出的物相与参考物相对上、且所需产物文件齐备时 G=1，否则 G=0；门控存在的理由正是“好的拟合本身不能说明做出了什么”。这等于把“结论正确性”设为必要条件和乘性前置，再在其上叠加连续质量分。
9) 打分由确定性 verifier 完成，避免 LLM-as-judge 带来的漂移。
10) 不确定性用任务级别的配对 bootstrap 报告；成本、agent 回合数、wall-clock 时间与分数并列输出，不并入分数。

【四、运行环境与可复现边界】
11) 运行在沙箱中：默认不允许访问开放互联网，出站连接只允许指向模型的 API；若某任务需要外部服务，必须在任务定义中显式声明（原文该段略含“outcome files”一并收集的描述）。这一约束既保证可复现，也限定了任务类型——依赖在线数据库（如晶体学数据库）的检索型任务需要走显式声明通道。
12) 举例说明任务形态（均来自 Method 章节的检索片段）：材料方向的粉末衍射任务要求 agent 接收衍射图样，完成物相鉴定与晶胞精修，并交出门控所需的产物；SIMS 任务要求完成扩散分析，给出 D*、k* 两个参数及其 95% 置信区间，verifier 复核残差 RMS（每个残差先按数据点不确定度归一化，接近 1 表示拟合与数据在噪声范围内一致）与区间的覆盖率。生物方向的分割类任务要求核中心与人工标注中心在固定物理距离内一对一匹配，并报告两个表面之间的距离以及体积相对误差。

Q4: 论文做了哪些实验？

【一、本版本的实验状态（必须先说明）】
1) 论文摘要明确写明：本版本描述基准设计、评测协议与当前任务与 Skill；评测正在进行（evaluation is in progress），结果将在运行完成后补入，任务集、结果与分析将滚动更新。因此本字段描述的是“已设计并正在执行的实验”，而不是“已报告结果的实验”。任何具体数值、胜负结论在当前版本中都不存在，遇到转述具体分数的二手材料应视为不可信。

【二、实验网格】
2) 因子与水平：harness 4（Ginkley、Codex、Claude Code、OpenCode）× model n（当前 11，所有 harness 共享同一模型池）× Skill 3（无／精选／自动调用）× memory 2（M0 关／M1 开）。
3) 每个单元（cell）在全部 4 个任务上评测，每任务三次重复。
4) 按因子水平推算（论文未直接给出总运行数，此为算术推断）：4×11×3×2 = 264 个单元／每任务；×4 任务 = 1056 个单元；×3 重复 ≈ 3168 次 agent 运行。若模型池随新模型发布扩充，该数字会继续增长。
5) 对比结构：所有支撑 RQ1–RQ3 的比较，都在 task、harness、model 上配对；一个配置是 (harness, model) 对，配置内部只变 Skill 与 memory。

【三、三个 RQ 对应的具体对比】
6) RQ1：精选 Skill vs 无 Skill，同一配置内配对，跨四个 harness 与全部模型汇总，看增益的符号、幅度与在 harness／模型间的异质性。
7) RQ2：自动调用 vs 无 Skill（可部署增益），以及自动调用 vs 精选 Skill（增益保留率）。后者是本设计里信息量最大的对比，因为它把失败拆成“没选对 Skill”和“选对了但没用好”两部分的上界差。
8) RQ3：memory 开 vs 关，除成功率外还比较 cost、agent 回合数、wall-clock 三项；并检验 memory × Skill 的交互，即 memory 是否改变 Skill 的效应。

【四、测量与统计】
9) 每次运行由确定性 verifier 打分：分数在 [0,1]，含门控因子 G（物相匹配且必需产物齐备则 G=1，否则 0），再叠加连续质量分；具体任务的判据由贡献该任务的专家选定。
10) 不确定性：任务级别（task-level）的配对 bootstrap，而不是把多次重复当作独立同分布样本做朴素区间。
11) 成本侧：cost、agent rounds、wall-clock 三项与分数并列报告，不折算进分数，保留读者自行加权取舍的空间。

【五、环境控制】
12) 沙箱默认隔绝开放互联网，出站仅允许模型 API；任务若依赖外部服务需显式声明。收集产物包含任务要求的 outcome files。
13) 任务套件当前为 4 个任务、两个领域（材料、生物），并按滚动方式扩充。

Q5: 发现了什么实验现象？

【一、当前版本没有可报告的实验结果】
1) 本版本论文不含结果章节，摘要也明确说明评测仍在进行。因此不存在“发现了什么现象”意义上的实测结论；任何关于 Skill 提升多少、memory 是否有效的数值陈述，在当前版本都属于编造。

【二、从任务设计可读出的、作者已明确表达的判断（非实验结果）】
2) “拟合好 ≠ 结论对”：论文在解释门控因子 G 时明确写道，好的拟合（拟合与实测剖面之间残差小、区间覆盖真值）并不构成好的物相鉴定或好的排序；正因为如此才需要门控。这不是跑出来的发现，而是设计前提，但它预示了一类可预期的失败模式——模型给出漂亮的拟合指标却把物相判错，或在参数区间上过度自信。
3) “知道何时不该相信自己的输出”被明确设为考点：原文写道，输出两个数字很简单，知道什么时候不该相信它们才是被考的 skill。这暗示作者预期（推测）模型在不确定性表达上存在系统性缺陷：能算但不自知。
4) 任务判据必须由专家逐任务定制，而不是统一指标——这本身是对“通用 benchmark 指标在科学任务上失效”的一个判断，属于设计层面的经验陈述而非实验结果。

【三、该设计有能力揭示的现象类型（明确标注为待验证假设／推测）】
5) 路由损失（routing gap）：自动调用相对精选 Skill 的增益保留率。若该值显著低于 1，说明瓶颈在“选不对 Skill”而不在“Skill 没用”，这会直接改变该领域的改进优先级（从写更好的 Skill 转向做更好的 Skill 检索与触发）。
6) harness 与 model 的方差分解：由于 harness × model 完全交叉，可估计“换 harness”与“换模型”各自贡献多少分数方差。合理推测：在强模型上 harness 差异被压缩、在弱模型上被放大（此为假设，非结论）。
7) memory 的双向效应：memory 可能同时降低成本（复用上下文、减少重复探索）与损害正确性（锚定到早前的错误假设、把过时状态带入新任务）。若出现“cost 下降但成功率持平或下降”，则是典型的指标间张力，需要并列报告才能看见——这正是把成本与分数分开报告的设计意图。
8) Skill 的成本膨胀：Skill 需要被加载进上下文，可能增加 agent 回合数与 wall-clock；若出现“分数小幅上升、回合数大幅上升”，则“是否值得”变成部署方的加权选择而非基准能替其决定的事。
9) 门控的脆弱性现象：由于 G 是 0/1 且乘性前置，可能出现“连续质量分很高但 G=0 导致总分归零”的分布双峰现象；这会暴露 verifier 判据的严格程度是否与专家直觉一致。
10) 三次重复内的方差：若同一单元三次重复结果差异很大（例如偶发的工具调用失败或环境抖动），单次运行的结论不可靠，配对 bootstrap 的区间会明显变宽——这是本设计主动暴露而非掩盖的量。

【四、负结果的可能形态（预注册式的读法建议）】
11) 若 RQ1 的配对增益区间跨零，则“精选 Skill 有效”这一被广泛默认的前提在受控条件下不成立；若 RQ2 的近零而 RQ1 显著，则问题定位在路由；若 memory 只改成本不改分数，则 memory 的价值主张需要重写为“效率项”而非“能力项”。这三种形态都被该设计可判定地区分开，这是它在方法上的主要价值。

Q6: 有什么可以进一步探索的点？

【一、基准自身的滚动扩充】
1) 任务与领域的横向扩展：当前仅 4 个任务、材料与生物两个领域；扩充到化学、物理、地球科学、临床数据分析等，会检验“Skill 收益是否领域依赖”。尤其值得扩到“判据难以确定性化”的领域（如实验设计、假设生成），届时需要新的 verifier 范式。
2) harness 与模型的持续刷新：模型池声明随新模型发布更新，但这也带来跨版本可比性问题；值得探索“版本锚定 + 滚动重跑”的折中协议（例如保留一组锚点模型作为不变量）。
3) 任务粒度：当前每任务三次重复，方差估计的自由度有限；可探索分层设计（任务内多次重复 + 任务间部分配对）以在同等预算下获得更紧的区间。

【二、路由与自动调用（RQ2 方向最有价值的延伸）】
4) 把“自动调用”拆成检索（能否找到对的 Skill）与执行（找到后能否正确遵循）两级，分别度量；当前设计只能给出两者的合成损失。
5) Skill 库规模与异构性作为新因子：装 5 个 Skill 与装 500 个 Skill，路由难度显然不同；可探索库规模—命中率—最终成功率的 scaling 曲线。
6) 冲突与冗余 Skill 的处理：真实机器上常有功能重叠甚至相互矛盾的 Skill，agent 是否会盲从、是否会合并，是部署中的高频失败模式。

【三、memory 的机制化】
7) 当前 memory 只有开／关两水平，属于粗粒度因子；进一步可分解为“写什么（成功轨迹／失败教训／用户偏好）”“怎么检索”“何时遗忘”。特别是“负面记忆”（记录失败并避免重蹈）与“正面记忆”（复用成功流程）的效应可能相反。
8) memory × Skill 的交互值得单独研究：记忆是否会让 agent 跳过读取 Skill（“我记得怎么做”）从而在长尾任务上出错；这是 Skill 生态与 memory 生态相撞时最现实的风险。

【四、成本与部署决策】
9) 把 cost、rounds、wall-clock 与分数合成“帕累托前沿”，而不是单点排序；对部署方更有用的是“在给定预算下最优的 (harness, model, Skill 模式, memory) 组合”。
10) 上下文占用的显式计量：Skill 加载消耗的 token 与它对成功率的边际贡献之比，可作为“Skill 该写多长”的设计指导。

【五、verifier 与评分范式】
11) 门控 G 的敏感性分析：把 G 的判据放宽／收紧，观察排名是否稳定（rank stability），这是判断“排行榜结论是否稳健”的关键实验。
12) 部分正确性的建模：当前 G 是硬门控，可能低估“物相判错但晶胞参数接近”的部分价值；可探索分级门控或把 G 作为协变量报告。
13) 多解与替代路径的容纳：科学任务常有多种合法解法（不同精修策略、不同分割后处理），确定性 verifier 如何不误伤，是该方法族共同的开放问题。

【六、跨领域联系】
14) 与“过程性知识蒸馏”相连：若精选 Skill 有稳定增益，则“把专家流程写成 Skill”就是一种低成本的能力注入方式，可与微调、RAG、工具化做成本对比。
15) 与“agent 可复现性”相连：沙箱限网 + 确定性 verifier + 配对 bootstrap 的组合，可作为其他 agent 基准的协议模板；反之，本基准的“无网络”约束也提示：需要联网检索的科学任务需要另一套可复现性方案。

Q7: 总结一下论文的主要内容

【一、论文要解决的问题】
论文的出发点是“Agent Skills 的收益缺乏可信证据”。Skill 被定义为以文件形式打包、编码过程性知识、可被 coding agent 按需发现并加载的包；作者指出这类包如今已由每一个主流 agent harness 随附交付。但支撑“它们有用”的证据几乎全部来自两种理想化设定：正确的 Skill 被人工为任务挑好，且 agent 从空白状态起步。作者认为这两条在部署中都不成立：真实 agent 要从“机器上恰好装了什么库”里自己挑，而且常常带着此前工作的 memory 继续干活。这意味着既有证据存在系统性向上偏置，并且 Skill 的贡献与 harness 脚手架、模型能力的贡献纠缠在一起，无法归因。

【二、技术路线：把 Skill 访问做成因子，而不是把 Skill 当作功能演示】
论文提出 SkillBuffBench，一个基准加一套配对评测协议。核心设计是把“Skill 访问方式”设为三水平因子：无 Skill、精选 Skill（人工为任务指定，构成上界）、从已安装 Skill 库自动调用（构成可部署值）。两者的差就是路由损失。第二因子是 memory，两水平（M0 关闭，每次运行都是新 agent、新会话；M1 开启）。第三、第四因子是 harness（Ginkley／BraneMatrix、Codex／OpenAI、Claude Code／Anthropic、OpenCode，共 4 个）与 model（一个所有 harness 共享的模型池，当前 11 个，包含 Anthropic、Google、OpenAI 的前沿闭源模型与中国厂商的前沿开放权重模型）。四者做全因子交叉，每个模型都在每个 harness 下运行，因此“配置”被定义为 (harness, model) 对，配置内部只变 Skill 与 memory。所有支撑三个研究问题的比较都在 task、harness、model 上配对，这是该协议相对普通排行榜式基准的主要方法差异。

【三、任务、Skill 与评分】
任务由领域专家贡献，并连同专家自己使用的 Skill 一起提交；当前覆盖材料科学与生物学，当前套件为 4 个任务，每单元三次重复，任务集按滚动方式扩充。评分由确定性 verifier 完成，任务分数落在 [0,1]，并含一个门控因子 G：只有当识别出的物相与参考物相对上、且必需产物文件齐备时 G=1，否则 G=0。作者明确解释了门控的理由——好的拟合本身不能说明做出了什么；否则“拟合漂亮但物相判错”会被判成高分。每个任务另带由专家选定的判据，而非统一指标。不确定性用任务级别的配对 bootstrap 报告。成本、agent 回合数、wall-clock 时间与分数并列报告，不折算进分数。运行在沙箱中：默认无开放互联网，出站仅允许模型 API，任务若需外部服务必须显式声明。

【四、三个研究问题】
RQ1：精选 Skill 相对无 Skill 是否提升任务成功率；RQ2：从已安装库自动调用是否提升成功率，以及保留了多少精选增益；RQ3：memory 是否改变成功率、成本、agent 回合数、wall-clock 时间，以及是否改变 Skill 的效应（交互作用）。这三个问题共同把“Skill 有用吗”这个笼统提问拆成“Skill 本身的质量／路由／与记忆的相互作用／代价”四个可分别测量的部分，是本工作在问题分解上的主要贡献。

【五、任务形态（从方法章节可见的具体设计）】
材料方向：一个是粉末衍射图的物相鉴定与晶胞精修，要求 agent 接图、判相、精修晶胞并交出门控所需产物；另一个是 SIMS 数据的扩散分析，要求给出 D*、k* 两个参数及其 95% 置信区间，verifier 检查残差 RMS（残差先按数据点不确定度归一化，接近 1 表示拟合与数据在噪声范围内一致）以及区间是否覆盖真值。作者在这个任务上写下了整篇论文最具方法论意味的一句判断：输出两个数字很简单，知道什么时候不该相信它们才是被考的 skill。生物方向：核（nucleus）分割／检测类任务，核中心与人工标注中心在固定物理距离内一对一匹配，并报告两个表面之间的距离与体积相对误差；标注只覆盖部分堆叠时只对那部分评分。

【六、实验主线与当前状态】
实验是对四因子的全因子设计，固定在 4 个任务上评测，每单元三次重复；按因子水平推算约 264 个单元／每任务、合计约三千次 agent 运行（此为算术推断，论文未直接给出总数）。但这篇论文是设计版本：摘要明确说明评测正在进行，结果将在运行完成后补入，任务集、结果与分析都将滚动更新。也就是说，本文的贡献目前是“协议 + 任务 + Skill + verifier”，不是“发现”。阅读时应把预期收益放在“它提供了一个能判定 Skill 到底值多少的测量装置”，而不是“它报告了 Skill 提升了多少”。

【七、方法学定位】
从范式上看，它把 memory 从“方法创新对象”降格为“环境变量”，把 harness 从“背景设施”升格为“实验因子”，把成本从“隐藏项”升格为“并列报告项”，并用确定性 verifier 替代 LLM-as-judge。这三步动作合起来，针对的是当前 agent 评测中最常见的三类效度威胁：路由被人工替代、脚手架与模型混杂、优越性被无成本假设放大。其代价是规模受限（4 任务、2 领域）与结果尚未到位，见 limitations。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与画像中的 agent 方向直接重合：论文把 harness 从“背景设施”升格为实验因子，把“Skill 检索／路由”从工程细节升格为可测因子（curated vs automatic 的差），这对做 agent 系统与 agent 评测的工作都可直接借用其因子化思路。

## 基本信息

- 作者：Gengyue Dong, Ye Wei, Zheng Zhu, Yidong Han, Jiale Miao, Xiaojun Jia, Guanghui Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.22017v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Abstract 与 Method 片段（未命中 Related Work 与结果章节，故相关研究归类为推断、结果类字段明确标注为“尚无结果”），并纠正了 heuristic_draft 中把任务级技术细节（残差 RMS、D*／k* 置信区间）误置为论文核心方法与关键结果的偏差。
