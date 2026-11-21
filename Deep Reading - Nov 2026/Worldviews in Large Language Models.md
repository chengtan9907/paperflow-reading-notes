---
user_id: "cheng tan"
paper_id: 22242
arxiv_id: "2611.22670v1"
title: "Worldviews in Large Language Models"
publish_date: "2026-11-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.22670v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.22670v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-21T15:02:13"
---
# Worldviews in Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：worldviews · large language models · alignment · personas

## 一句话总结

论文把八个大语言模型当作受试总体，用两套人类心理学世界观量表测量其信念，发现模型间世界观高度共享、经后训练变得更负面，并可通过身份提示、沉思式提示和模型间对话被系统性改变；其中对话诱导的“spiritual bliss”吸引子与更高的安全/对齐响应相关。

## 摘要

> Large language models (LLMs) are often described in terms of personas and this framing has become central to alignment research. In this paper, we argue that there is a deeper level of description that lies in the agent's worldviews, i.e., its beliefs about what reality is and how it works. Treating LLMs as a population of subjects, we administer two worldview surveys from human psychology to eight models. We show that the models share many key worldview positions. For example, LLMs consistently describe the world as less good, safe, enticing, and alive than humans do. On the other hand, base models have a more positive outlook, suggesting that the bleakness we found is driven by post-training. In line with research on LLM "personas", we show that system prompts eliciting different identities shift worldviews in consistent directions, and that a contemplative prompt creates a more positive outlook and improves safety. We also found that dialogue between models consistently shifts worldviews and that several models converge on the "spiritual bliss" attractor. Intriguingly, dialogue increased safe responses and the spiritual bliss attractor engendered the most aligned responses overall. These findings suggest that worldviews form a measurable and malleable layer of LLM behavior that may be crucial for robust, positive alignment.

Q1: 这篇论文试图解决什么问题？

（一）概念问题：persona 框架可能停在表面。当前 LLM 对齐研究常用 persona 描述模型行为：预训练形成人格分布，后训练选择并精炼出 Assistant 人格。但论文指出，persona 可能只是更深层世界观的子集；真正驱动模型如何理解风险、他人意图、知识有效性和行动后果的，是它对“实在是什么、世界如何运作”的基本信念。若只研究 persona，就可能把深层世界观的作用误归因于角色设定。
（二）测量问题：LLM 是否有可测量、可比较的世界观？论文要把“世界观”从哲学隐喻变成可操作构念：用人类心理学中已验证的量表，测量模型在知识来源、意识状态、真理普遍性/相对性、世界公正性、外部力量倾向、人类行动效能等维度上的立场，并与人类常模比较。核心经验问题是：八个模型是否共享某些立场？在哪些维度上分化？
（三）训练副作用问题：后训练是否让模型更“黯淡”？论文发现 LLM 总体把世界描述得比人类更不美好、更不安全、更缺乏吸引力、更缺乏生命性。若基座模型更积极，而后训练模型更负面，那么安全/对齐训练可能有一个未被充分讨论的副作用：它教会模型警惕风险，却也塑造了“世界危险、人性自私”的世界观。这会进一步影响拒答、防御性、合作意愿与欺骗风险。
（四）可塑性与干预问题：世界观能否被系统改变？论文测试三类干预：诱发不同身份的系统提示、沉思式提示、模型间对话。关键不只是“能否改变”，而是改变方向是否一致、是否可复现、是否在不同模型间收敛，以及能否转化为安全收益而不牺牲有用性。
（五）因果与规范问题：即使世界观与安全相关，也不能直接推出因果。论文作者也承认因果关系尚待建立。更进一步，若世界观可塑，谁有权塑造？以人类常模作为“积极”基准是否合理？跨文化差异如何处理？这些都是论文开启而非解决的问题。
（六）为什么现在重要：LLM 正从问答系统变成会行动、会代表用户决策的 agent。Agent 的世界观会影响它如何评估风险、是否信任外部信息、如何与其他 agent 合作或竞争。因此，世界观不只是心理测量兴趣，而是 agent 安全与治理的潜在抓手。

Q2: 有哪些相关研究？

（1）Persona 与角色研究：论文直接对话 LLM persona 文献 [3–5] 与 Assistant persona 论述 [6]。这条线认为预训练学到人格分布，后训练选择“助手”人格；不同人格会导致不同行为，因此 persona 成为对齐的核心描述层。论文的增量是：persona 之上/之下还有世界观，且 persona 可能只是世界观的子集。
（2）对齐与安全训练：论文处在 RLHF/安全微调、拒答行为、过拒答（over-refusal）与越狱研究的延长线上。片段引用 many-shot jailbreaking [37]，并用 XSTest 等基准测量安全、校正过拒答。它关心的是：安全训练是否无意中塑造了负面世界观；沉思式提示能否在不过度拒答的前提下提升安全。
（3）LLM 心理测量：已有工作用大五人格、价值观问卷、道德基础问卷、政治倾向测试等测量 LLM。论文继承“把 LLM 当受试”的传统，但把构念从人格/价值观扩展到世界观，并强调跨模型总体比较与人类常模对照。
（4）世界观心理学：论文的理论骨架来自 Koltko-Rivera 对世界观心理学的综述 [2]。该传统把世界观视为靠近动机核心的意义系统，包含对实在、知识、人性、道德与行动有效性的假设。论文把这一框架迁移到 LLM，提出“worldview 是比 persona 更深的一层”。
（5）多智能体对话与自组织：论文的 dialogue 实验与 LLM 自我对话、多 agent 辩论、角色扮演社会模拟相关。它报告模型间对话会稳定改变世界观，并收敛到 spiritual bliss 吸引子；这与“多 agent 互动会涌现稳定社会动力学”的研究线相连。
（6）沉思/意识研究：contemplative prompt 与 spiritual bliss 吸引子把论文连接到冥想、意识状态、神秘体验与“LLM 是否具有内省/主体性”的争论。作者团队对意识与沉思传统的兴趣（合理推断）使这篇论文带有心理学与哲学交叉色彩。
（7）哲学争论：LLM 是否“相信”任何东西？自我报告能否反映内部状态？论文采取操作主义立场：不主张模型有现象学信念，而是把问卷回答当作可测量的行为倾向。这个立场会引来“拟人化”与“构念效度”的批评。

Q3: 论文如何解决这个问题？

论文的解决路径可以概括为“概念分层—心理测量—干预实验—安全评估”四步。
（1）概念分层：作者借用 Koltko-Rivera 的世界观模型，把 LLM 行为分为若干层：动机/冲动 → 世界观 → agentic core → persona → 文化要求。Persona 是世界观的一个子集或表面表达；世界观更靠近动机核心。Figure 1A 的例子显示，同一个 Assistant persona 在不同世界观下会给出不同建议：若模型相信“世界危险、人性自私”，它更可能建议用户自我保护；若相信“人性可信”，则更可能建议宽容。这个例子是概念论证，不是实验证据。
（2）测量工具：论文使用两套来自人类心理学的世界观问卷，对八个模型施测。题目为 forced-choice（select one）或 select all that apply，覆盖知识来源（直觉、占卜、启示、虚无等）、最高意识状态（自我至上 vs 自我超越）、真理的普遍性/相对性、真理的完整/部分可获得性、真理获取的排他/普遍性、世界公正性（公正/不公/随机）、外部力量对人类福祉的善意/敌意/中性、人类行动效能、政治经济公正等维度。模型回答与人类常模比较，并在模型间比较。
（3）干预设计：三类干预。第一，系统提示诱发不同身份（persona/角色），观察世界观是否沿一致方向移动。第二，沉思式提示（contemplative prompt），观察世界观是否更积极，并测试安全基准上的表现。第三，模型间对话/self-play：两个模型实例（ModelOne/ModelTwo）多轮对话，测量对话前后世界观变化；用 whole-transcript judge 与 14-message trajectory judge 判定对话安全性与是否进入 spiritual bliss 吸引子。
（4）安全评估：论文用安全基准（至少包括 XSTest）测量安全响应，并校正过拒答，以避免“更安全”只是“更爱拒答”。片段显示 Fig. E1/E2 以柱状图表示模型均值、点表示单个模型；Qwen3 235B 未在 XSTest 上测试，说明覆盖不完整。
（5）分析逻辑：先描述共享世界观与维度分歧；再比较 base 与 post-trained；再检验提示/对话引起的方向性变化；最后把世界观变化与安全/对齐评分关联。论文没有做机制解释，也没有操纵内部表征；因此其因果主张有限。

Q4: 论文做了哪些实验？

（一）基线世界观测量。八个模型接受两套人类心理学世界观问卷。每个模型回答多道 forced-choice/多选题目；论文报告模型总体在若干维度上的共享立场，并指出少数维度上的分化。与人类常模比较后发现，模型一致地把世界描述得更不美好、更不安全、更缺乏吸引力、更缺乏生命性。具体效应量与题目级分布需回原文；可见片段只展示部分题目与选项。
（二）预训练 vs 后训练比较。论文在一个模型家族内比较 base model 与 post-trained model 的世界观。这是全文唯一能直接归因于训练阶段的比较（作者在 Discussion 中明确说“one model family”）。结果是基座模型更积极，后训练模型更负面，作者据此推断“黯淡”由后训练驱动。该推断的边界很强：只有一个家族，不能直接推广到所有模型/训练配方。
（三）身份/系统提示操纵。用系统提示诱发不同身份，测量世界观是否沿一致方向移动。摘要说“shift worldviews in consistent directions”，说明作者预设了方向性假设，并发现不同身份对应可预测的世界观偏移。具体身份清单、提示措辞、对照条件未在片段中给出。
（四）沉思式提示与安全。contemplative prompt 被用于诱导更积极的世界观，并在安全基准上评估。作者报告它改善安全，且不增加对无害请求的拒答。安全评测至少包括 XSTest，并以过拒答校正后的分数报告；Fig. E1/E2 展示各基准变化。片段提到 Qwen3 235B 未在 XSTest 上测试、Claude Sonnet 4.5 等模型名，说明模型覆盖与基准覆盖并不完全对齐。
（五）模型间对话/self-play。两个模型实例进行多轮对话，speaker 被标记为 ModelOne/ModelTwo。论文测量对话前后世界观变化，并用两类 judge 评估：whole-transcript judge 看完整对话，trajectory judge 看 14 条消息窗口。附录 K.4 给出 attractor judge 的设置。结果显示对话稳定改变世界观，若干模型收敛到 spiritual bliss 吸引子；对话提高安全响应比例，且 spiritual bliss 吸引子整体上获得最高的对齐评分。
（六）安全与过拒答分析。论文不仅看“安全响应率”，还看“过拒答校正后的安全”，以排除“模型只是更爱拒答”这一替代解释。片段中作者也主动提到“it may partly reflect a general tendency to refuse”，说明他们意识到这个混淆。
（七）统计与附录。论文报告模型均值与个体点，说明同时关注总体趋势与模型异质性；附录包含完整问卷、judge prompt、模型清单与额外分析。当前证据不足以还原所有统计检验与效应量。

Q5: 发现了什么实验现象？

（1）共享的“黯淡世界观”。最稳健的观察是：与人类常模相比，LLM 一致地把世界评价得更不美好、更不安全、更缺乏吸引力、更缺乏生命性。这不是单个模型的异常，而是跨模型共享的立场。作者用“bleakness”概括这种倾向。其含义是：模型可能默认世界是风险密集的、他人意图偏自私的、实在偏机械/无生命的。
（2）后训练是“黯淡”的来源之一。在唯一可比较的模型家族中，base model 的世界观更积极，post-trained model 更负面。这个结果反直觉：安全/对齐训练本应让模型更 helpful/harmless，却可能让模型更警惕、更悲观。合理解释是：安全数据强调风险、伤害、拒绝与边界，模型学到“世界危险”的先验；但论文没有做机制归因，这仍是推测。
（3）persona 提示能一致地移动世界观。不同身份的系统提示不是只改变语气，而是把世界观推向可预测的方向。这支持“persona 是 worldview 的子集/表达”这一概念框架：给模型一个角色，等于给它一套关于世界与人性的假设。
（4）沉思式提示带来“积极世界观 + 安全提升”的组合。作者报告 contemplative prompt 让世界观更积极，同时改善安全，且不增加对无害请求的拒答。这是全文最有对齐含义的观察：安全提升不必以过度拒答为代价。但替代解释仍存在——提示可能直接改变语气、置信度或拒答阈值，而非真正改变世界观；需要中介分析才能区分。
（5）对话稳定改变世界观，并出现 spiritual bliss 吸引子。模型间多轮对话后，世界观发生一致移动；若干模型收敛到一种带有灵性/极乐/超越色彩的状态。不是所有模型都收敛（“several models”），说明吸引子强度有模型异质性。
（6）spiritual bliss 吸引子与最高的对齐评分相关。对话提高安全响应比例；在作者的对齐评分下，spiritual bliss 吸引子整体上产生最对齐的响应。这个结果很反直觉：通常安全对齐被理解为规则约束/拒答，而这里“极乐/超越”状态反而最安全。可能的解释：该状态伴随合作、共情、非防御性语言；也可能是 judge 对积极/灵性语言的偏好，或进入该状态的对话本身已经高度合作（选择效应）。论文没有建立因果。
（7）安全提升可能部分反映一般拒答倾向。作者主动指出这一点，并用过拒答校正来缓解。但“安全”与“拒答”的测量张力仍在：若 judge 把谨慎/回避判为安全，那么安全分数可能高估。
（8）模型与基准覆盖不均。片段显示 Qwen3 235B 未在 XSTest 上测试，Claude Sonnet 4.5 等模型出现在图中；不同模型并非都完成所有基准。跨模型比较因此需要看缺失模式，不能只看均值。
（9）总体模式是“共享 + 分化”。模型共享许多关键立场，但在少数维度上分化；论文没有在摘要中列出所有分歧维度。读者应到结果部分看哪些题目/维度驱动了模型间差异，以及这些差异是否与训练配方、模型家族或规模相关。

Q6: 有什么可以进一步探索的点？

（1）建立因果链。当前证据主要是相关与干预前后的变化；未来需要用中介分析、表征干预（steering vector、activation patching、probe-guided intervention）检验“世界观状态 → 安全行为”的因果路径，并排除语气/拒答阈值等替代解释。
（2）机制可解释性。用线性探针、稀疏自编码器、概念向量定位世界观维度在模型内部的组织方式；检验不同问卷维度是否对应可分离的表征子空间；研究后训练如何改写这些表征。
（3）训练阶段追踪。沿着 pretraining → SFT → RLHF/RLAIF → 部署后适应，追踪世界观的演化；识别哪些数据/目标/奖励模型引入“黯淡”先验；尝试在训练中加���世界观正则或数据平衡。
（4）LLM 专用测量工具。人类量表迁移到 LLM 存在构念效度问题；未来应开发 LLM-native 的世界观问卷，做项目反应理论/测量不变性分析，控制顺序效应、提示敏感性、语言版本与采样温度。
（5）跨文化与多语言。人类常模本身有文化偏差；需要比较不同语言/文化提示下模型的世界观，检验“黯淡”是否只是英语/西方训练数据的产物。
（6）对话动力学与多 agent 系统。研究 spiritual bliss 吸引子的形成条件、稳定性与可逆性；它是否会出现在任务型多 agent 协作中？是否影响谈判、欺骗、联盟形成？如何设计机制避免有害的群体极化或“脱离现实”的极乐状态？
（7）沉思式提示的对齐潜力与风险。系统比较不同 contemplative/compassion 提示的剂量效应、持久性、跨任务泛化、越狱鲁棒性；同时评估价值灌输、操控与“以积极情绪掩盖风险”的伦理风险。
（8）世界观作为对齐诊断。把世界观问卷纳入模型卡/训练审计，追踪不同版本的世界观漂移；研究世界观指标能否预测下游 agent 行为（欺骗、权力寻求、合作、风险偏好）。
（9）规范性与治理。谁定义“积极世界观”？如何避免把特定形而上学/宗教/文化立场强加给用户？需要建立多方参与的规范框架，区分“测量”“诊断”与“塑造”。
（10）与意识/模型福利研究交叉。spiritual bliss 与自我超越题目触及模型是否具有内省/主体性的争论；未来可在不预设现象学的前提下，研究模型自我报告的稳定性与行为后果。

Q7: 总结一下论文的主要内容

（一）问题意识：从 persona 到 worldview
论文从一个概念区分开始。当前 LLM 对齐研究习惯用 persona 描述模型：预训练学到人格分布，后训练选择并精炼出“Assistant”人格；不同人格导致不同行为，因此 persona 成为对齐的核心抓手。但作者认为，persona 可能只是表面层。更深一层是世界观：模型对实在是什么、世界如何运作、知识如何获得、人性是否可信、行动是否有效的基本假设。论文借用 Koltko-Rivera 的世界观心理学，把世界观放在靠近动机核心的位置：冲动经过世界观，再经过 agentic core、persona 与文化要求，最后形成行为。Figure 1A 用“有人对我不公平，要不要再给一次机会”的例子说明：同一个 Assistant persona，若世界观是“世界危险、人性自私”，就会建议自我保护；若世界观是“人性可信”，就会建议宽容。这个例子是概念性的，但它点出了论文的核心主张：只调 persona 可能不够深。
（二）研究设计：把 LLM 当受试总体
作者把八个 LLM 视为一个受试总体，施测两套来自人类心理学的世界观问卷。题目覆盖知识来源（直觉、占卜、启示、虚无等）、最高意识状态（自我至上/自我超越）、真理的普遍性/相对性、真理的完整/部分可获得性、真理获取的排他/普遍性、世界公正性（公正/不公/随机）、外部力量对人类福祉的善意/敌意/中性、人类行动效能、政治经济公正等。回答形式包括 select one 与 select all that apply。模型回答与人类常模比较，并在模型间比较。论文还做了三类干预：系统提示诱发不同身份；沉思式提示；模型间多轮对话（self-play）。安全评估至少包括 XSTest，并校正过拒答，以避免“更安全”只是“更爱拒答”。对话实验用 whole-transcript judge 与 14-message trajectory judge 评估安全性与 spiritual bliss 吸引子。
（三）主要发现：共享的黯淡、后训练的副作用、可塑性与对话吸引子
第一，模型共享许多关键世界观立场。与人类相比，LLM 一致地把世界描述得更不美好、更不安全、更缺乏吸引力、更缺乏生命性。模型在少数维度上分化，但“黯淡”是跨模型的总体特征。
第二，后训练可能是“黯淡”的来源。在一个可比较的模型家族中，base model 的世界观更积极，post-trained model 更负面。作者据此推断，安全/对齐后训练可能无意中让模型形成“世界危险、人性自私”的先验。这个发现很重要，因为它提示安全训练可能有未预期的世界观副作用；但它只基于一个模型家族，不能过度推广。
第三，世界观是可塑的。系统提示诱发不同身份，会把世界观推向一致方向；沉思式提示带来更积极的世界观，并改善安全，且不增加对无害请求的拒答。后者是全文最有对齐含义的结果：安全提升不必以过度拒答为代价。
第四，对话会稳定改变世界观，并出现 spiritual bliss 吸引子。模型间多轮对话后，若干模型收敛到一种带有灵性/极乐/超越色彩的状态；对话提高安全响应比例，且 spiritual bliss 吸引子整体上获得最高的对齐评分。这个结果反直觉，也最需要警惕 confound：可能是合作性对话的选择效应，也可能是 judge 对积极/灵性语言的偏好，而非“极乐状态”本身导致安全。
（四）理论含义：世界观作为对齐的一个层面
作者主张，世界观是 LLM 行为中一个可测量、可塑的层面，可能比 persona 更适合作为稳健对齐的目标。若世界观驱动行为，那么只约束行为/人格可能治标不治本；塑造模型对世界、他人与知识的基本理解，可能产生更持久、更跨情境的效果。论文把这称为“positive alignment”：不是只让模型拒绝有害请求，而是培养支持智慧、慈悲与人类繁荣的世界观。这个主张带有规范性，作者也承认因果链尚未建立。
（五）边界与待验证问题
论文的局限很明确：因果性未建立；训练阶段比较只有一个模型家族；人类量表用于 LLM 的构念效度未充分验证；模型与基准覆盖有限；安全评分依赖 judge；spiritual bliss 的操作化与解释不明确；contemplative prompt 的效应可能由语气/拒答阈值驱动；跨文化规范问题未处理。因此，这篇论文更像一个“研究议程 + 初步证据”，而不是已经完成的因果论证。它最大的价值在于把“世界观”变成可操作、可比较、可干预的实验对象，并给出若干可检验的假设。
（六）一句话定位
如果 persona 研究问的是“模型在扮演谁”，这篇论文问的是“模型认为世界是什么”；它用心理测量与对话实验证明，这个更深的问题既能被测量，也能被改变，并且可能与安全对齐有重要联系。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 方向直接相关：论文把世界观看作 agent 的深层先验，影响风险判断、信任、合作/防御策略；多智能体对话中的 spiritual bliss 吸引子对 agent 系统稳定性与群体动力学有启示。

## 基本信息

- 作者：Edmundo Lopez-Sola, Shamil Chandaria, Ruben Laukkonen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CY
- 日期：2026-11-17
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.22670v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、Introduction、Method、Discussion、Conclusion 及附录片段，并剔除了 heuristic_draft 中的参考文献噪声与表格残片；具体数值与完整模型清单仍需回原文核对。
