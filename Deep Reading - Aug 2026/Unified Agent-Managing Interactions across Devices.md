---
user_id: "cheng tan"
paper_id: 6957
arxiv_id: "2608.05729v1"
title: "Unified Agent: Managing Interactions across Devices"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.05729v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.05729v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:01:41"
---
# Unified Agent: Managing Interactions across Devices

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：unified agent · state design · cross-device interaction · cross-time interaction

## 一句话总结

论文提出跨设备、跨时间的用户-智能体交互需要智能体维护一个紧凑、行动就绪的状态（state），并实例化为 Unified Agent；在自建基准上，其状态设计显著优于四种已发表设计的改编版本，且在多种 MLLM 配置下保持领先。

## 摘要

> As capabilities rapidly increase, AI agents can move from running inside one app to acting across a user's devices over time. Yet existing agent systems still fall short in this scenario. This is because observations are scattered across devices and moments, but mainstream systems are not designed around this fact: a single agent that treats devices as tools lacks effective state management for all devices across time, and multi-agent systems coordinate across agents but do not maintain the compact carried state a cross-device, cross-time request needs. We argue that the agent should maintain an effectively designed state that organizes engagement evidence, stated facts, and the standing request in a compact, action-ready form for deciding its action given the current observation. To compare state designs, we construct a benchmark of user-agent interaction across devices and time. We instantiate this principle in Unified Agent, a stateful agent that carries interaction evidence across devices and moments and uses it with the current observation to act. In the default setting, it significantly outperforms our adaptations of four published designs. Across changes in multimodal large language model (MLLM) family, capability, and reasoning effort, it remains ahead of all compared systems, demonstrating that the state-design advantage is robust across MLLM settings. Our code and data will be publicly available on GitHub.

Q1: 这篇论文试图解决什么问题？

1. **场景定义**：论文聚焦于 AI 智能体跨设备、跨时间执行用户任务的场景。例如，用户在多个设备上比较餐厅，智能体需要在后续某个时刻基于分散在不同设备上的信息做出行动决策。

2. **核心难点**：智能体在决定每个动作时，所依赖的证据可能来自较早的某时或另一个设备，并且在行动时刻可能已不可见（例如屏幕熄灭、设备未命名）。这种“观察分散”特性是现有系统设计时未考虑的。

3. **现有方案的两类缺陷**：
 - 单智能体把设备当作工具调用，但其状态管理局限于当前上下文，无法有效追踪所有设备上的历史交互证据，导致“它当时在哪个设备上？”这类问题无法回答。
 - 多智能体系统通过代理间协调来分担任务，但代理间传递的是消息而非一个紧凑的、跨设备跨时间的请求状态，因此无法维持请求所需的全局视图。

4. **问题本质**：作者将问题归结为状态设计（state design）问题，而非系统复杂度问题。关键在于如何在紧凑、行动就绪的形式下，组织和保留跨设备、跨时间的交互证据，使智能体在仅有当前观察时也能做出正确决策。

5. **基准缺失**：现有文献缺乏针对跨设备、跨时间交互的标准化评测，导致不同状态设计难以客观比较，论文因此构建了专用基准。

6. **隐含假设**：论文假设多模态大语言模型（MLLM）本身具备足够的基本推理能力，差距主要体现在状态设计上；实验通过跨 MLLM 设置验证这一假设的稳健性。

Q2: 有哪些相关研究？

由于检索证据未提供具体文献列表，以下基于摘要中的分类和上下文推断相关研究方向：

1. **单智能体工具使用（Tool-augmented Agents）**：
 - 主流智能体系统常将设备或外部工具视为可调用 API，依靠上下文窗口组织信息。这类方法在单设备、短时任务中有效，但缺乏跨时间的状态持久化，无法处理跨设备请求。
 - 典型代表：ReAct、Toolformer 等工具调用范式，以及各类 MLLM 智能体框架。

2. **多智能体协作系统（Multi-Agent Systems）**：
 - 多智能体架构通过分工和消息传递协调多个代理，以分解复杂任务。但代理间传递的是离散消息，没有维护一个统一、紧凑的“请求状态”，因此跨设备证据难以聚合。
 - 代表方向：AutoGen、MetaGPT、AgentVerse 等。

3. **记忆增强智能体（Memory-Augmented Agents）**：
 - 有些系统引入长期记忆（如向量数据库、摘要缓存）来保存历史信息。但这类记忆通常以通用文本形式存储，缺乏针对“证据-事实-请求”的结构化组织，且未必按设备或时间索引。
 - 论文中的 “Full context” 基线可能对应把所有历史观察拼接进上下文的方法，而 “memory” 基线对应显式记忆模块。

4. **跨设备交互与 Ubiquitous Computing**：
 - 在人机交互领域，跨设备任务延续（task handoff）研究由来已久，但多聚焦于界面连续性，而非智能体内部状态。

5. **状态表示与推理（State Representation and Reasoning）**：
 - 相关工作还涉及机器人任务规划中的状态估计、部分可观察马尔可夫决策过程（POMDP）等，但这些多用于仿真环境，未结合 MLLM 的交互场景。

6. **注意**：上述分类为合理推断，具体引文需查阅论文原文的 Related Work 部分。

Q3: 论文如何解决这个问题？

论文的核心解决方案是**状态设计（State Design）**，具体要点如下：

1. **核心原则**：智能体应维护一个“紧凑携带状态”（compact carried state），该状态整合跨设备、跨时间的交互信息，并以行动就绪（action-ready）的形式存在，使智能体在给定当前观察时能直接据此决策。

2. **状态的三类内容**：
 - **交互证据（Engagement Evidence）**：记录用户在哪些设备、哪些时刻与智能体交互的证据，例如“用户周三在手机上打开过餐厅 A 的页面”。
 - **陈述事实（Stated Facts）**：用户明确提供的偏好、约束或信息，按主题（必要时按设备）组织，使得相同主题的事实跨设备可区分。
 - **长期请求（Standing Request）**：用户当前尚未完成的请求或目标，例如“帮我比较这几家餐厅的评分”。

3. **状态的组织形式**：状态必须以紧凑、可操作的方式编码，不能简单堆砌原始历史记录。其表示方式（representation）和使用方式（usage）是设计的关键，而非增加系统复杂度。

4. **决策流程**：Unified Agent 在每个时刻将“当前观察”与“携带状态”结合，产生下一步动作。状态随交互不断更新，并跨设备、跨时刻携带。

5. **与现有方法的区别**：
 - 相比单智能体工具调用，它显式维护跨时间状态，而非仅依赖上下文窗口；
 - 相比多智能体协作，它不需要多代理通信，而是由单一个体携带统一状态；
 - 相比简单记忆系统，状态有结构化和行动导向的归纳，而非原始信息的堆叠。

6. **实例化**：Unified Agent 是一个通用框架，不绑定特定 MLLM，可通过替换底层模型来适配不同能力。论文通过多组 MLLM 设置验证其普适性。

7. **未在摘要中提供的细节**：状态的具体编码方式（例如自然语言摘要、结构化 JSON 或嵌入向量）、更新机制（何时合并何时丢弃）以及如何应对状态冲突，均需查阅论文原文 Method 节。

Q4: 论文做了哪些实验？

1. **基准构建**：论文构建了一个跨设备、跨时间的用户-智能体交互基准（Benchmark），用于评估不同状态设计的性能。基准包含多设备、多时间点的交互任务，具体任务类型、数据规模和标注方式未在摘要中给出，需查看原文。

2. **对比系统**：将 Unified Agent 与四种已发表设计的改编版本（adaptations）进行对比。四种设计的具体名称和细节未在摘要中列出，可能涵盖单智能体全上下文、记忆增强、多智能体等代表性方法。

3. **默认设置**：在默认配置下，Unified Agent 显著优于所有对比系统。

4. **鲁棒性实验**：
 - 更换 **MLLM 家族**（例如不同厂商或架构的多模态模型）；
 - 改变模型**能力**（例如不同参数规模或版本）；
 - 调整**推理努力**（reasoning effort，例如思考时间或采样步数）。
 在这些变化下，Unified Agent 始终领先所有对比系统，说明状态设计优势不依赖于特定底层模型。

5. **统计显著性**：论文使用了配对自助法（paired bootstrap）计算 95% 置信区间，表明性能增益在统计上显著。

6. **具体数值**：根据检索片段，整体增益范围在 0.194–0.408 之间，且所有配对自助法的 95% 置信区间上界都大于零。这些数值来自实验部分，具体指标（准确率、成功率等）未知。

7. **消融与分析**：摘要未提及消融实验，但可能包含对状态组件的消融（如去除事实或证据后的效果）。需查阅原文实验节。

Q5: 发现了什么实验现象？

1. **状态设计的压倒性优势**：在默认设置下，Unified Agent 的整体性能显著超过四种已发表设计的改编版本，支持了“紧凑状态是跨设备交互关键”的核心论点。

2. **增益幅度具体化**：整体性能增益范围在 0.194–0.408 之间（具体指标未知），且所有配对自助法的 95% 置信区间均不包含零，说明增益在统计上显著，而非噪声波动。

3. **跨 MLLM 设置的稳健性**：
 - 当更换 MLLM 家族时，Unified Agent 依然保持领先，表明状态设计效果与底层模型品牌无关；
 - 当改变模型能力（如更强的或更弱的模型）时优势不变，说明状态设计并非仅在最强模型下有效；
 - 当调整推理努力时仍领先，说明不需要额外推理成本就能获得优势。

4. **反直觉点**：一个简单的“有状态”智能体能胜过使用全上下文或复杂多智能体通信的方法，提示在长时跨设备任务中，结构化状态比上下文长度或代理数量更重要。

5. **可能的负结果**：对比系统在某些配置下可能接近或超过 Unified Agent 的某个子指标，但总体性能仍落后。摘要未提供分项失败案例。

6. **指标间张力**：未见摘要提及不同指标间的冲突，但可能存在任务类型上的差异（例如简单查询 vs 复杂推理），需要原文确认。

7. **注意**：上述实验现象均基于摘要和检索片段，具体任务细节、曲线和消融结果需阅读原文实验结果图。

Q6: 有什么可以进一步探索的点？

1. **状态设计的深化**：探索更丰富的状态表示方式，例如层级状态、概率状态或可微状态存储，以处理更复杂的跨设备依赖。

2. **状态压缩与遗忘**：随着长期使用，状态可能膨胀，如何设计遗忘策略或重要性采样以保持紧凑性，是实际部署的关键问题。

3. **多设备异构性推广**：当前基准可能仅涵盖手机、电脑等常见设备，可扩展至 IoT、可穿戴设备、车载系统等，验证状态设计在更异构环境的效用。

4. **隐私与安全**：携带跨设备状态可能涉及敏感个人信息，需要研究状态加密、差分隐私或本地化存储等隐私保护机制。

5. **状态与记忆的结合**：将长期语义记忆（如用户偏好画像）与短期交互证据整合进统一状态，可能进一步提升个性化能力。

6. **应用到更广的任务类型**：不仅限于信息查询，还可扩展到跨设备的工作流编排、自动化操作（如跨应用表单填写）等。

7. **与 Ai-for-Science 的结合**：在科学工作流中，研究者常在不同设备上收集数据、运行实验，Unified Agent 的状态设计可帮助跨设备追踪实验条件和结果，形成可落地的应用场景。

8. **多智能体与有状态智能体的混合架构**：在复杂团队任务中，让每个智能体携带自己的状态，同时通过共享状态区域协调，可能是既有鲁棒性又可扩展的方向。

9. **基准的扩充与社区化**：公开基准可加入更多真实用户数据、更细粒度的动作评估标准，以及对抗性场景（如设备切换错误、信息冲突）。

10. **生成任务延伸**：在拥有跨设备证据后，智能体后续可生成总结摘要、可视化报告等，结合生成能力提升用户体验——这为跨设备交互与生成模型的结合提供接口。

Q7: 总结一下论文的主要内容

本论文针对 AI 智能体在“跨设备、跨时间”执行用户任务时的核心问题展开研究。作者指出，随着模型能力的提升，智能体应用场景正从单应用扩展到用户的多个设备，但现有系统在此类场景下性能不佳。问题的根源在于：交互观察（例如用户在哪台设备上说了什么、做了什么）散落在不同设备和不同时刻，而主流智能体系统并非围绕这一事实而设计。单智能体方法把设备视为工具，却缺乏对所有设备跨时间的统一状态管理；多智能体系统虽能协调多个代理，却也没有维护一个紧凑的“携带状态”来表达请求的全局背景。因此，智能体在需要决策时常常面临“证据不可见”的困境。

针对这一困境，论文提出核心原则：智能体应当维护一个紧凑、行动就绪的状态，该状态需要整合三类信息——交互证据（engagement evidence）、用户陈述事实（stated facts）和长期请求（standing request）。状态设计本身是决定系统成功与否的关键变量，而非增加系统复杂度或依赖更大模型。作者将这个原则具体实现为 Unified Agent，一个有状态智能体，它会随交互更新状态，并在每个时间点将状态与当前观察结合，产生行动。

为了验证状态设计的重要性，作者构建了专门的基准（benchmark），模拟跨设备、跨时间的用户-智能体交互任务，并用该基准对比了 Unified Agent 与四种已发表设计的改编版本。实验结果表明，在默认设置下 Unified Agent 的整体性能显著优于所有对比系统。随后，作者进行了一系列鲁棒性测试，更换底层多模态大语言模型（MLLM）家族、调整模型能力配置和推理努力（reasoning effort），结果 Unified Agent 始终保持领先地位，说明状态设计的优势不依赖于特定底层模型，具有很强的泛化能力。

论文的主要贡献包括：第一，形式化了跨设备、跨时间的用户-智能体交互问题，明确了状态设计在其中的核心地位；第二，提出了一个统一的智能体框架，通过紧凑状态组织交互证据、事实和请求；第三，构建了富有挑战性的基准，为后续研究提供公共评测工具；第四，通过多组实验验证了状态设计在不同 MLLM 环境下的稳健优势。

从论证主线看，论文遵循“问题观察 → 缺陷分析 → 原则提出 → 实例化 → 实验验证”的逻辑。技术主线上，它不依赖复杂的系统架构或多代理通信，而是聚焦于状态的内容与组织方式——这一点对实际部署尤为重要，因为轻量级状态设计更容易适配不同模型和应用场景。实验主线则通过跨模型、跨设置的全面对比，试图说明“状态设计”是横向上更普适的改进维度。

当然，论文在摘要中未提供具体任务细节、数据集规模、基线名称和绝对性能数值，这些信息需要从全文获取。总体而言，这项工作为跨设备、跨时间的人机交互智能体提供了一个清晰的设计原则和实证基础，对智能体系统设计和状态表示研究均有参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接相关：论文核心主题是 AI 智能体（agent），与你的研究方向“agent”权重高度重合。

## 基本信息

- 作者：Xinshuang Liu, Runfa Blark Li, Shaoxiu Wei, Xin Lin, Truong Nguyen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.CV, cs.HC
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.05729v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答基于 heuristic_draft 与 retrieved_evidence 中的摘要和引言片段生成，未提供全文；部分细节（如具体实验配置）为合理推断，建议结合原文核对。
