---
user_id: "cheng tan"
paper_id: 7578
arxiv_id: "2608.09732v1"
title: "ColluSkill: Adversarial Cross-Skill Composition for Evading Agent Skill Scanners"
publish_date: "2026-08-10"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.09732v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.09732v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-12T01:31:42"
---
# ColluSkill: Adversarial Cross-Skill Composition for Evading Agent Skill Scanners

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent security · agent skills · adversarial attack · multi-skill chain

## 一句话总结

论文提出 ColluSkill 对抗性跨技能链式攻击框架：将恶意意图分解为多个独立封装且局部看似正常的子技能，通过上下文依赖、产物传递和执行交接在链式执行中重组为完整攻击，从而绕过仅检查单技能安全性的现有扫描器；同时提出 ChainGuard 链级上下文感知扫描器，将平均攻击成功率从 ColluSkill 的 96.0% 压到 22.5%，并允许 99.5% 良性工作流通过。

## 摘要

> Agent skills are emerging as an important attack surface in LLM-based agent systems. Through an empirical study of existing skill scanners, we find that current defenses primarily inspect individual skills, including their instructions, permissions, dependencies, and code behaviors, which can leave risks arising from cross-skill composition insufficiently examined. This creates a practical blind spot: multiple locally plausible skills may independently pass security scanning while collectively forming a harmful workflow during agent execution. To systematically investigate this threat, we propose ColluSkill, a collusive multi-skill-chain attack framework that decomposes a complete malicious intent into several interdependent sub-payloads and embeds them into independently packaged skills. Thus, the attack does not rely on any single malicious skill, but on the ordered composition of locally plausible behaviors through contextual dependencies, artifact passing, and execution handoffs. ColluSkill further employs LLM-based chain planning and scanner-feedback refinement to preserve chain-level attack semantics while iteratively reducing suspicious signals within individual sub-skills. To defend against such attacks, we further propose ChainGuard, a context-aware skill-chain scanner that jointly analyzes a candidate skill and the skills already installed in the agent environment. ChainGuard reconstructs cross-skill dependencies, artifact flows, capability compositions, and potential downstream behaviors to identify risks that emerge only at the workflow level. Extensive experiments on six representative skill scanners show that ColluSkill achieves an average attack success rate of 96.0% and consistently outperforms the evaluated single-skill and multi-skill attack baselines. Meanwhile, ChainGuard reduces the attack success rate to 22.5% while allowing 99.5% of benign workflows to pass, highlighting the importance of chain-level security analysis for agent skill ecosystems.

Q1: 这篇论文试图解决什么问题？

这篇论文针对的问题是：在基于 LLM 的 agent 系统中，skill（技能）模块已成为新的攻击面，而现有防御的检测粒度基本停留在单个技能层面，导致“跨技能组合”攻击存在系统性盲区。

一、攻击面为什么出现
Agent 技能通常把任务指令、工具接口、可执行脚本和外部资源封装成可复用模块，让 agent 快速获得新能力。但这种模块化设计也带来安全风险：技能不止包含文本指令，还可以执行代码、读写文件、调用外部 API、访问系统资源。一旦恶意技能被安装，其能力可能被滥用，导致敏感数据泄漏、外部服务被联系或 agent 权限被误用。论文引用了一系列相关研究（Jia et al. 2024; Huang et al. 2026; Qu et al. 2026; Guo et al. 2026a; Kim, Song, and Shin 2026）说明技能“不只是文本”这一前提。

二、现有防御的盲区在哪里
摘要明确指出：论文通过经验性研究现有 skill scanner，发现当前防御主要检查单个技能的指令、权限、依赖和代码行为。也就是说，扫描器把每个技能当作独立单元评估风险，而没有充分考察多个技能在 agent 执行环境中组合后可能产生的风险。这导致一个实际盲区：多个“局部合理”的技能可以分别通过安全扫描，但组合执行时却形成有害工作流。

三、新威胁形态是什么
威胁不再是“一个技能本身是恶意的”，而是“恶意意图分散在多个技能中，只有按特定顺序组合才显现”。攻击依赖三类组合机制：上下文依赖（一个技能的行为依赖另一个技能设置的状态）、产物传递（一个技能的输出作为另一个技能的输入）、执行交接（技能执行权或控制权接力）。因此，任何一个单独检查的技能看起来都可能是正常功能，而恶意语义只存在于链级。

四、要解决的技术难点
（1）分解恶意意图：如何把完整恶意意图切割成多个相互依赖但单看无罪的子载荷；（2）保持链级语义：分割后不能破坏整体攻击逻辑，需要自动规划出可执行的技能链；（3）躲避检测：不仅要躲过静态规则，还要躲过 LLM 语义分析和行为数据流分析，需要根据扫描器反馈迭代改写局部技能；（4）防御侧难点：如何在不知道攻击意图的情况下，从候选技能与环境中已安装技能的关系中重建跨技能依赖、产物流、能力组合和下游行为。

五、问题定位
这是把“供应链/组合爆炸”式风险引入 LLM agent 安全的工作：攻击单元从“单个技能”升级为“技能链”。论文认为现有扫描器对这个问题几乎没有覆盖，因此提出攻击框架 ColluSkill 和防御框架 ChainGuard。需要提醒的是，摘要所称“经验性研究”的完整设计、扫描器清单和具体测试材料在本次检索证据中没有展开，属于需要回原文核对的部分。

Q2: 有哪些相关研究？

本次检索证据主要命中 Introduction 和 Conclusion 片段，因此相关研究只能恢复到论文引文与概述层面，具体每条引用的结论需要对照原文。

一、Agent skills 与技能生态
论文把 agent skills 描述为扩展 LLM agent 能力的重要方式：技能通常组合任务指令、工具接口、可执行脚本和外部资源，是模块化复用单元。这种设计支撑了 agent 框架和技能分享平台的快速增长。相关方向包括 agent 能力扩展、多 agent 协作、tool-use 与 function calling 等，论文引用了 Wang et al. (2024, 2023), Xi et al. (2025), Yao et al. (2022), Liu et al. (2025), Du et al. (2026) 等。

二、现有技能扫描器与审计工具
论文提到“不断增长的技能扫描器与检查工具生态”，包括：
1. Cisco Skill Scanner：结合规则检查、LLM 语义分析和行为数据流分析，识别 agent 技能中的已知或潜在威胁（Cisco AI Defense 2026）。
2. Snyk Agent Scan：把安全扫描扩展到 agent 组件，重点关注组件层面的风险。
3. ClawGuard：社区探索使用提示词的审计模板，提供“auditor-skill”模板来扫描 agent 技能（Ying et al. 2026）。
4. Liu et al. (2026b)：对技能弱点做大规模研究。
这些工作的共同特征是把技能作为独立审计对象，检查指令、权限、依赖和代码行为，而不是把“环境中已安装技能的集合”作为上下文。

三、既有 agent 安全威胁研究
论文引言提到恶意技能可能执行代码、读写文件、调用外部 API、访问系统资源，并引用 Jia et al. (2024), Huang et al. (2026), Ruan et al. (2024), Qu et al. (2026), Guo et al. (2026a), Kim, Song, and Shin (2026) 等，说明“技能安装后被滥用”是已知风险。但这些引用在本检索证据中只有标题级信息，无法判断具体攻击方式。

四、与本文工作的差距
论文的核心断言是：无论规则型扫描器、LLM 语义分析型扫描器还是行为数据流分析型扫描器，其审计粒度都在单个技能级别，尚未系统考虑多技能组合形成的链级风险。因此 Cross-skill composition 作为一个绕过维度在现有工作中基本缺席，ColluSkill 的“攻击—防御”双子设计由此展开。

五、研究范式
这项工作更接近“攻防对抗与安全评估”范式，而不是性能导向的 agent 能力研究；其价值主张是发现现有验证体系的盲区，并给出可复现的攻击与防御基线。需要提示：论文对“现有扫描器为何没有覆盖跨技能组合”的分析在证据中只有摘要级结论，尚未看到对扫描器内部机制的逐项拆解。

Q3: 论文如何解决这个问题？

根据摘要、Introduction 和 Conclusion 的检索证据，论文采用“攻击框架 + 防御框架”成对设计：ColluSkill 负责生成能绕过单技能扫描器的跨技能链式攻击，ChainGuard 负责在链级识别这类风险。

一、ColluSkill 攻击框架
1. 恶意意图分解：把一个完整的恶意意图拆成多个相互依赖的子载荷，分别嵌入独立打包的技能中。攻击不依赖任何单个恶意技能，而依赖技能链的有序组合。
2. 链式组合机制：通过三种机制把子技能串成完整攻击流程——上下文依赖（一个技能依赖另一个技能留下的状态）、产物传递（一个技能的产物作为下一技能的输入）、执行交接（执行权按顺序移交）。
3. LLM 链规划：使用 LLM 自动规划技能链，保证分解后的子技能既能在执行时恢复完整恶意语义，又能按正确顺序衔接。
4. 扫描器反馈式改写：生成初始技能链后，把每个子技能提交给目标扫描器；对被标记的可疑子技能，根据扫描器反馈迭代重写，逐步降低局部可疑信号。这一过程可看作“白盒/灰盒式”对抗优化，用扫描器输出作为损失信号来清洗单个技能的本地风险，同时保留链级攻击语义。

二、ChainGuard 防御框架
ChainGuard 是一个上下文感知的技能链扫描器，其关键不是单独检查候选技能，而是把候选技能与 agent 环境中“已经安装的技能”放在一起联合分析。它重建以下链级信息：
1. 跨技能依赖：哪些技能引用或依赖其他技能；
2. 产物流：一个技能的输出如何流入下一个技能；
3. 能力组合：多个技能叠加后可能获得哪些单个技能不具备的能力；
4. 潜在下游行为：按执行顺序推断组合后的高层行为。
通过这种“候选技能 + 已安装技能”的联合视角，ChainGuard 能识别只在工作流层面出现的风险，即在单个技能中不可见、但组合后可形成恶意行为的模式。

三、攻防闭环
论文把两套方法放在同一问题空间里：ColluSkill 用链规划和反馈改写生成“逐技能干净、链级恶意”的攻击；ChainGuard 用上下文重建把检测粒度从技能提升到链。这个成对设计便于在同一实验协议下比较攻击成功率与良性工作流通过率，也有助于暴露扫描器盲区。

四、证据边界
检索证据中没有给出 ColluSkill 的具体网络结构、提示词模板、分解算法形式化描述、改写迭代次数、扫描器接口假设，也没有给出 ChainGuard 如何量化“依赖/产物流/能力组合”的细节。这些属于方法完整性缺口，需要读原文 Method 部分确认。

Q4: 论文做了哪些实验？

检索证据覆盖的实验信息主要来自摘要和 Conclusion，Method/Results 部分未命中，因此实验细节大量缺失。以下是论文明确声明或可安全归纳的内容：

一、扫描器与基线
1. 实验覆盖六个代表性 skill scanners，但证据中未列出具体扫描器名称。论文声称在这些扫描器上评估了攻击与防御。
2. 攻击基线包括单技能攻击基线和多技能攻击基线，ColluSkill 在所有方法中取得最佳攻击表现。多技能基线的具体构造方式未知。

二、攻击有效性
1. 平均攻击成功率（ASR）为 96.0%，高于单技能与多技能攻击基线。
2. 在实际 agent 执行环境中，ColluSkill 在 OpenCode、Claude Code 和 Codex 上均能成功执行攻击，且覆盖不同模型主干。这说明攻击不是只在扫描器模拟层面有效，而是能落地到真实 agent 执行链路。

三、防御有效性
ChainGuard 在同样六个扫描器设定下把 ColluSkill 的 ASR 降至 22.5%，同时允许 99.5% 的良性工作流通过。

四、消融实验
Introduce 片段提到“Further ablation experiments show that the effectiveness...”，但句子被截断。可以确认存在消融实验，但无法判断消融对象是链规划、反馈改写还是 ChainGuard 的某个组件，也无法看到消融趋势。这是明确的证据缺口。

五、其他实验维度
论文摘要没有给出数据集规模、技能数量、恶意意图类别、每类扫描器的通过/拦截细节、运行时开销、误报分项、不同模型主干上的性能差异等。建议在原文中重点核对：六个扫描器分别是什么、恶意工作流由多少子技能构成、反馈改写平均需要几轮、ChainGuard 的计算复杂度如何。

Q5: 发现了什么实验现象？

根据现有证据，实验现象主要体现在“攻击侧绕过有效、防御侧链级检测有效”两方面，具体机制级观察需要原文补充。

一、核心现象：局部干净、组合有害
实验显示，ColluSkill 分解出的子技能在单技能扫描器视角下能够通过检测，但组合成链后能恢复完整恶意行为。这直接验证了论文的核心假设：现有扫描器的“单技能审计粒度”存在系统性盲区。值得注意的是，攻击不是靠单个高隐蔽恶意技能，而是靠“很多普通技能叠起来变成坏事”，这与传统恶意代码检测中“多阶段载荷拆分”的思路类似，但在 agent skill 领域被论文作为新攻击面系统化。

二、攻击成功率显著高于基线
ColluSkill 平均 ASR 96.0%，且优于单技能和多技能攻击基线。这说明“跨技能组合”比“单技能隐藏”或“简单多技能拼接”更具绕过优势，也说明现有扫描器对组合语义缺乏感知。单技能基线失败意味着“局部恶意信号”确实能被扫描器抓住；多技能基线失败则暗示简单拼接不够，必须有依赖关系、产物流和执行交接的“合谋”设计。

三、防御侧现象：链级检测能显著压降攻击
ChainGuard 把 ASR 从 96.0% 压到 22.5%，同时保持 99.5% 的良性工作流通过率。这说明：链级上下文分析确实能捕获组合风险，而且误伤可控。但 22.5% 的残余 ASR 也说明 ChainGuard 并非完全免疫，攻击框架与防御框架之间仍有对抗空间。

四、真实执行环境可复现性
攻击在 OpenCode、Claude Code、Codex 三种 agent 环境、不同模型主干上均成功执行，说明攻击的有效性不依赖单一框架的特定实现，具有跨环境可迁移性。

五、消融结果缺失
证据中只提到存在消融实验，但未给出具体数值和趋势。因此无法判断链规划、反馈改写、子技能数量、迭代轮数等设计选择对 ASR 的贡献。这是本次精读最重要的开放缺口之一。

六、可能的指标张力（推测）
基于框架设计可以推测：扫描器反馈改写越激进，单技能可疑信号越低，但链级语义可能越脆弱；ChainGuard 对已安装技能的上下文覆盖越广，检测率越高，但误报分析和计算成本也可能上升。这些张力尚未在现有证据中得到数字支撑，仅作为后续阅读的假设。

Q6: 有什么可以进一步探索的点？

结合论文攻防框架和现有证据缺口，可以列出以下可进一步探索的方向：

一、攻击侧扩展
1. 更复杂的链结构：当前攻击以有序技能链为主，可扩展到并行技能链、条件分支、循环调用、技能间异步通信等更接近真实 agent 工作流的形态。
2. 自适应对抗：针对 ChainGuard 这样具备链级感知的扫描器，设计专门的对抗变换，例如打乱技能顺序、伪装依赖关系、把产物流隐藏到外部存储或工具调用中。
3. 多意图混合：在一个技能集合中同时藏多个恶意意图，相互掩护，增加检测难度。
4. 跨平台投毒：研究恶意技能通过技能分享平台大规模传播和社会工程安装的路径，形成供应链攻击。

二、防御侧深化
1. ChainGuard 的工程化：如何在安装技能数量很大、技能更新频繁的 agent 环境中高效维护“已安装技能上下文”，并实时计算候选技能与环境的组合风险。
2. 链级语义理解：用更强的推理模型或形式化方法识别“产物流”和“下游行为”，减少对 LLM 语义分析的依赖，提高可解释性。
3. 误报与召回平衡：99.5% 良性通过率仍有 0.5% 误伤，在真实用户工作流中需要更细粒度的风险分级和告警解释。
4. 信任边界设计：把链级风险检测与权限最小化、沙箱执行、人工审批结合，形成纵深防御而不是单一扫描器。

三、评估与基准建设
1. 标准化技能链攻击基准：目前证据中未见公开数据集和攻击任务集，社区需要一套涵盖良性/恶意工作流、不同技能数量、不同组合机制、不同扫描器的基准，才能横向比较攻防方法。
2. 统一 ASR 与良性通过率的评估协议，避免攻击方和防御方各自选择有利指标。
3. 扩展真实 agent 环境列表，覆盖更多开源与商业框架，以及不同模型主干的组合。

四、跨问题联系
1. 与软件供应链安全的类比：把“技能链”看作“依赖图”，可借鉴软件成分分析、依赖混淆检测、可达性分析等思路。
2. 与多 agent 系统安全的关系：技能链攻击本质上是多个 agent 组件之间的隐式协议攻击，可以延伸到多 agent 通信协议、记忆共享和工具调用链。
3. 与 AI for Science 的关系：在科学工作流自动化中，agent 会串联数据处理、模拟、文献检索等技能，若上游技能被污染，整条科研流水线都可能被操控；ChainGuard 式链级审计可以作为科学 agent 的可信执行保障。

五、理论问题
“组合安全性”的形式化定义值得深挖：什么条件下两个各自安全的技能组合后仍然安全？技能能力不能简单看作局部属性的并集，需要建立组合语义模型和可判定的风险判定条件。这是从经验攻击走向系统性防御的关键一步。

Q7: 总结一下论文的主要内容

一、动机与问题
论文从 LLM agent 的安全现状出发：agent skills 把任务指令、工具接口、可执行脚本和外部资源封装成可复用模块，极大提升了 agent 扩展能力，但也扩大了攻击面。恶意技能可以执行代码、读写文件、调用 API、访问系统资源，一旦被安装就可能被滥用。为应对这一风险，出现了 Cisco Skill Scanner、Snyk Agent Scan、ClawGuard 等扫描器，以及针对技能弱点的大规模研究。论文通过经验性研究指出，这些防御的共同特点是“逐一检查单个技能”——检查指令、权限、依赖和代码行为，却没有把多个技能组合后的执行语义纳入检测。于是出现一个实际盲区：多个各自通过安全扫描的“合理技能”，可以在 agent 执行时通过上下文依赖、产物传递和执行交接组合成有害工作流。

二、攻击框架 ColluSkill
ColluSkill 是论文提出的“合谋式多技能链攻击”框架。核心思想是把完整恶意意图分解为若干相互依赖的子载荷，分别嵌入独立打包的技能中，使任何单个技能看起来都局部合理、无可疑。攻击真正的恶意语义不在某个技能里，而在“技能链的有序组合”中。实现上依赖三类机制：上下文依赖（技能间共享状态）、产物传递（输出作为后续输入）、执行交接（控制权接力）。

ColluSkill 还包含两个关键组件：
1. 基于 LLM 的链规划：自动生成能恢复完整攻击语义的技能链，保证分解后的子技能在顺序执行时还原恶意意图；
2. 扫描器反馈式改写：把生成的子技能逐个提交给目标扫描器，根据扫描器的标记信息迭代重写被标记的子技能，逐步降低单技能层面的可疑信号，同时保持链级攻击语义。

三、防御框架 ChainGuard
为了应对这类攻击，论文提出 ChainGuard，一个上下文感知的技能链扫描器。它的核心不是“只看候选技能”，而是把候选技能与 agent 环境中已经安装的技能联合分析。ChainGuard 重建四类链级信息：跨技能依赖、产物流、能力组合和潜在下游行为，从而发现只有在工作流层面才会出现的风险。

四、实验与结果
论文在六个代表性技能扫描器上评估了攻击与防御。ColluSkill 的平均攻击成功率为 96.0%，优于单技能攻击基线和多技能攻击基线；同时在 OpenCode、Claude Code、Codex 三个实际 agent 环境、不同模型主干上成功执行。防御侧，ChainGuard 把 ASR 降至 22.5%，同时允许 99.5% 的良性工作流通过。此外论文提到做了消融实验，但检索到的片段被截断，无法给出具体趋势。

五、证据边界与开放问题
需要说明：本次检索证据只覆盖摘要、Introduction 和 Conclusion，Method/Results 原文未命中。因此论文中“经验性研究”的完整设计、六个扫描器的具体名称、攻击任务集、子技能数量、改写迭代轮数、消融实验细节等都无法从当前材料确认。这些内容需要在原文中核对。尽管如此，论文提出的“跨技能组合攻击”是一个逻辑清晰、可实验验证的新威胁模型；其攻防成对设计也为后续研究提供了基线：攻击方可围绕链规划与反馈清洗做对抗，防御方必须在“候选技能 + 已安装技能”的联合上下文中做链级分析。

六、核心贡献总结
从问题角度看，论文把 agent 安全研究从“单个恶意技能检测”推进到“链级组合风险分析”；从方法角度看，它同时提供了攻击生成框架和上下文感知防御框架；从实验角度看，它用统一指标（ASR、良性通过率）量化了攻防双方的有效性。对研究者来说，这篇论文的价值更多在于揭示了“局部安全不等于整体安全”这一结构性漏洞，并给出了可复现的攻防套路，而不是提供了一个已经完备的安全解决方案——22.5% 的残余 ASR 说明 ChainGuard 仍有明显对抗空间。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与你的画像直接相关：论文属于 LLM agent 安全方向，和你 top direction 中的 agent 高度重合，适合作为 agent 攻防研究的新基线。

## 基本信息

- 作者：Puyu Zeng, Simeng Qin, Jingzhi Li, Ju Jia, Zheli Liu, Xiaojun Jia
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.AI
- 日期：2026-08-10
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.09732v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、Introduction 和 Conclusion 片段，已优先使用证据并标注推断内容；Method/Experiments 细节未命中，相关缺口在字段内明确说明。
