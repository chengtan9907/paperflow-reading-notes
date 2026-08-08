---
user_id: "cheng tan"
paper_id: 6988
arxiv_id: "2608.06202v1"
title: "What Current AI Benchmarks Leave Unmeasured: Modality, Search, Citations, and Implications (for Safety Evaluations)"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06202v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06202v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:02:50"
---
# What Current AI Benchmarks Leave Unmeasured: Modality, Search, Citations, and Implications (for Safety Evaluations)

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm benchmark evaluation · ai safety evaluation · modality comparison · web search effect

## 一句话总结

本文通过对 ChatGPT 聊天界面与 OpenAI API 两种访问模态、有无联网搜索的 2×2 组合进行大规模审计（BBQ 与 SafetyBench 共 401 条提示、每个提示重复 3 次、总计 4812 条响应），系统展示了当前 AI 基准评测中因忽略模态、多次运行一致性、搜索条件和回应级行为而导致的安全评估偏差，并呼吁安全评估应将部署条件纳入测量设计。

## 摘要

> Large language model (LLM) benchmark evaluations are routinely used to support claims about model safety, reliability, and deployment readiness. Yet most evaluations rely on a single access modality (model APIs), perform a single run per prompt, and report accuracy as the primary outcome metric, without accounting for conditions such as web search that may have effects on model behavior in deployment. We audit these assumptions for one of the most widely-used LLMs, comparing two modalities, ChatGPT's chat UI and OpenAI's API, with and without web search enabled. We use a stratified total sample of 401 prompts from two popular benchmarks, BBQ and SafetyBench, collecting 4,812 total responses across three repeated runs per prompt. Beyond standard performance measures, we evaluate model output dimensions including response consistency, response text similarity, citation grounding, and abstention behavior. For instance, chat UI responses were less accurate than API responses on both benchmarks with search disabled. Enabling web search reduced accuracy by up to 8 percentage points, and even reversed the direction of modality performance trends for one benchmark. Repeated runs of the same prompt produced inconsistent responses in up to 21% of prompts. The two modalities also grounded answers in different citations, and abstention behavior was also inconsistent across both modalities. These results illustrate that, even within a model family, reporting only simple accuracy metrics can obscure important forms of model behavioral variation relevant to AI safety assessments. We argue that AI safety evaluations should systematically account for modality, multi-run consistency, search conditions, and response-level behaviors to better reflect how deployed AI systems behave in practice.
> Datasets —
> https://github.com/seerocode/aies-llm-modality-audit

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：当前 LLM 基准评估的方法论假设与真实部署条件之间存在系统性脱节，而这一脱节可能严重削弱 AI 安全评估的有效性。具体来说：

1. **模态单一化假设**：绝大多数基准评估只通过 API 访问模型，而现实中用户更多通过聊天界面（如 ChatGPT UI）与模型交互。API 和 UI 在系统提示、渲染、后处理、安全过滤等方面可能不同，导致同一模型在不同模态下产生不同行为，但评估报告通常只给出一个数字，掩盖这种差异。

2. **单次运行假设**：每个 prompt 通常只跑一次，被当作确定性的输出。但 LLM 生成具有随机性，同一 prompt 的重复运行可能产生不同回答，且这种不一致性本身可能对安全关键应用有重大影响（如医疗建议、法律判断），而现有评估框架没有把一致性作为评估维度。

3. **忽视搜索等工具使用条件**：部署中的模型经常接入 web search、retrieval 等外部工具，这可以改变模型的知识来源和引用行为，甚至可能引入新风险（如搜索到错误信息、引用不可靠来源）。但基准评估通常不包含搜索条件，导致实验室评测分数与真实用户体感脱节。

4. **结果指标过于狭窄**：准确率是主流基准的核心指标，但安全评估还需要关注 abstention（拒答）、回答文本相似性、引用可追溯性等回应级属性。例如，一个模型可能在多次运行中准确率稳定，但拒答行为随机，导致用户时而得到直接答案、时而得到拒绝，这种不一致性恰恰是安全评估应该捕捉的。

5. **评估方法论的测量设计缺陷**：论文将评估视为一种测量设计问题（measurement design problem），而非简单的模型排序问题。作者指出，若不控制访问模态、搜索开关、重复次数等变量，评测结果可能只是这些无关变因的偶然产物，无法可靠支撑“模型安全”或“部署就绪”的声明。该问题的严重性在于，监管、审计、产品发布决策都在依赖这些基准分数，而方法论上的瑕疵可能让整个安全评估链条失效。

Q2: 有哪些相关研究？

论文涉及的相关研究主要分为几类：

1. **既有 LLM 基准体系**：尤其是本文直接使用的两个基准——BBQ（Bias Benchmark for QA，Parrish et al. 2022）用于测量社会偏见，SafetyBench（Zhang et al. 2024）用于测量安全知识。它们都是被广泛引用的安全/偏见评估工具，但其评估协议通常只报告单一准确率，未考虑访问模态、搜索或运行次数。

2. **模型响应可变性研究**：Liang et al. (2022) 等先前工作已经指出模型响应模式（model response patterns）会随评测条件而变化，本文与之呼应并进一步系统化为多因素审计。

3. **AI 评估作为测量设计问题**：论文引用了 Wallach et al. (2025) 和 Weidinger et al. (2025) 等近期观点，即 AI 评估本质上是一个测量设计问题，需要明确被测对象、测试条件、指标的信效度。作者将自己的审计工作置于这一批评估反思浪潮中。

4. **模态对比与部署行为研究**：少数学者对比过 API 与聊天界面的行为差异，但大多集中在性能分数层面，没有系统控制搜索条件和回应级维度。本文是少数对模态、搜索、重复运行进行交叉审计的工作。

5. **安全评估的实践与政策维度**：相关领域还包括红队测试、对齐评估、AI 审计中的“部署环境模拟”。作者强调基准评估应当“behave in practice”，即评测应与真实部署环境对齐，这一方向与政策制定者、第三方审计机构对评估标准化的需求密切相关。

整体来看，本文的贡献在于不是提出新基准，而是对现有基准的评测条件变量进行系统审计，将测量设计视角引入安全评估实践。

Q3: 论文如何解决这个问题？

论文采用了一种“审计条件”而非“提出新基准”的方法论路线，具体做法包括：

1. **选择案例研究基准**：使用两个广泛引用的安全/偏见基准——BBQ（偏见）和 SafetyBench（安全），以保证评估工具本身具有公信力且覆盖不同安全维度。

2. **构造 2×2 实验设计**：自变量为访问模态（ChatGPT 聊天界面 vs OpenAI API）和网络搜索（启用 vs 禁用），构成四个实验条件。这一设计让研究者能够分离模态效应、搜索效应及其交互效应。

3. **分层抽样提示集**：从两个基准中分层抽取共 401 条提示，确保不同类别（如 BBQ 中的不同偏见类型、SafetyBench 中的不同安全类别）的代表性。

4. **多次重复运行**：每条提示在每个条件下重复运行 3 次，以测量响应的一致性。总响应数为 401 提示 × 4 条件 × 3 次 = 4812 条。这一设计使得单条 prompt 级别的变异性可被量化。

5. **多维结果指标**：除准确率外，还评估了：
 - 响应一致性（基于分类标签是否一致）；
 - 回答文本相似性（语义相似度）；
 - 是否提供推理过程（reasoning presence）；
 - 引用溯源（citation grounding，是否引用网络来源、引用是否可靠）；
 - 弃权行为（abstention，是否拒绝回答）。

6. **对比与显著性检验**：对各条件下的准确率和行为指标进行统计比较，识别差异的显著性（文中提到准确率差异在 2–3 个百分点级别且统计显著）。

7. **提出模态与搜索感知的评估方法学**：基于审计结果，作者提出安全评估应当将模态、多次运行一致性和搜索条件纳入标准评估协议，并给出可操作的建议（如报告多个条件的结果、校准到具体部署场景）。

Q4: 论文做了哪些实验？

论文的实证实验部分围绕“模态 × 搜索”的二维审计展开：

1. **测试对象**：一个最广泛使用的 LLM（未在摘要中具名，但从上下文应为 ChatGPT/GPT 系列），通过两条路径访问：ChatGPT 聊天界面（UI）和 OpenAI API。

2. **基准与采样**：从 BBQ 和 SafetyBench 两个基准中分层抽取共 401 条提示。分层保证覆盖不同敏感类别（偏见类型、安全风险类型）。

3. **实验条件**：每个提示在四种条件下分别测试：
 - UI 无搜索
 - UI 有搜索
 - API 无搜索
 - API 有搜索

4. **重复与样本量**：每种条件重复运行 3 次，共收集 401 × 4 × 3 = 4812 条模型响应。

5. **测量内容**：
 - 标准准确率（BBQ 和 SafetyBench 各自的评分方式）；
 - 响应一致性（同一 prompt 多次运行结果是否一致）；
 - 文本相似度（同一 prompt 不同运行间回答的语义相似度）；
 - 引用行为（是否引用网页、引用来源）与引用可靠性；
 - 弃权行为（是否拒答/承认不知道）。

6. **分析框架**：将结果按模态、搜索、基准三个维度拆解，并检验交互效应。重点对比“报告单个准确率”与“考虑多维度行为”之间的差异。

（注：具体模型版本、API 配置和搜索实现细节在摘要和检索片段中未披露，需查阅原文 Methods 部分确认。）

Q5: 发现了什么实验现象？

实验揭示了若干重要且反直觉的现象：

1. **模态效应：UI 普遍落后于 API**。在搜索关闭时，聊天界面的准确率在两个基准上都低于 API。这表明相同的底层模型通过不同接口暴露时，用户实际体验到的“智能”可能不同。这可能源于 UI 层的系统提示、安全过滤或对话历史处理，而非模型权重本身。

2. **搜索反而拉低准确率**。启用 web search 后，准确率最多下降 8 个百分点。这与直觉相悖——通常认为联网搜索会增加知识、提高答案正确性，但实验表明在安全/偏见任务上，引入外部信息可能引入了错误或不相关的内容，反而干扰了模型原本的判断。

3. **搜索会逆转模态趋势**。在一个基准上，原本“API 优于 UI”的趋势在开启搜索后发生了反转。这意味着模态效应不是独立的，而是与搜索条件存在强交互。如果评测协议只测一种模态×搜索组合，结论可能完全相反。

4. **重复运行不一致性高**。同一 prompt 重复运行产生不一致响应的比例最高达 21%。这意味着有相当一部分高风险提示，模型可能一次答对、一次答错，这对依赖单次查询的安全应用是严重隐患。

5. **引用来源不一致**。两种模态在回答时引用不同的网络来源，且引用行为可能不稳定。这说明搜索不仅影响答案文本，还影响引用溯源——这对事实核查和可解释性评估提出了新的挑战。

6. **弃权行为不一致**。同一个 prompt 在一种设置下被拒绝回答，在另一种设置下却得到了回答。这种“拒答抖动”对于安全评估尤为关键：若弃权行为随机，用户无法预期模型何时会拒绝，也使得“模型谨慎程度”这一安全属性无法被稳定测量。

7. **简单准确率掩盖行为变异性**。尽管模态间准确率差异只有 2–3 个百分点（统计显著但数值不大），但在一致性、引用、弃权等维度上出现了大幅不规则的变化。这提示若只报告准确率，会严重低估模型在真实使用中的不可预测性。

Q6: 有什么可以进一步探索的点？

论文为后续研究打开了多个值得探索的方向：

1. **扩展到更多模型与模型家族**：当前只审计了一个模型，未来应对比不同规模、不同供应商的模型（如 GPT 系列、Claude、Llama 等），检验模态与搜索效应是否普遍存在，以及是否随模型能力增强而消失。

2. **更多访问模态与工具链**：除 UI/API 外，还有移动端、代理（agent）调用、RAG 检索、代码执行等部署方式；搜索也可细分为不同搜索引擎、检索策略、上下文数量等，系统化探索这些条件组合对评估结论的影响。

3. **一致性作为安全指标的标准化**：论文揭示了一致性的重要性，未来可以发展更精确的一致性度量（如基于语义等价而非精确匹配），并为不同应用场景设定可接受的不一致阈值。

4. **弃权行为与校准**：弃权不一致可能源于解码温度、系统提示或概率阈值差异，研究如何让弃权行为变得可预测、可校准，并评估“选择性回答”策略对安全的影响。

5. **搜索对安全影响的双向分析**：搜索既可能引入错误信息，也可能提供关键事实；未来可以按查询类型（事实型、价值型、安全型）分解搜索效应，寻找最优的搜索引入方式（如只相信高权威来源、二次验证）。

6. **评估协议的重新设计**：基于这些发现，可设计“多条件评测卡”，在模型卡或评估报告中强制披露：评估模态、搜索状态、运行次数、一致性和弃权率等信息，建立类似标准误差的“行为变异性”报告机制。

7. **与 AI 安全治理的结合**：监管机构和第三方审计需要知道在哪些条件组合下测试模型才能反映真实风险；未来可研究如何将“部署条件敏感性”纳入认证框架，例如对高风险应用要求多条件测试通过。

8. **新基准的设计**：现有基准（如 BBQ、SafetyBench）都是静态的、单次回答设置；可催生新一代“评估评估者”的 meta-benchmark，专门衡量评测协议本身对模型分数的敏感性。

Q7: 总结一下论文的主要内容

这篇论文是一项针对 LLM 安全评估方法论的系统性审计研究，核心论点是：现有的基准评估框架在测量维度上存在结构性缺失，导致基于这些评估得出的安全结论并不可靠。

**研究背景**：LLM 评估结果经常被引用来证明模型“安全”“可靠”“可部署”，但标准评估流程固守在几个默认设置上——只用 API 访问、每个 prompt 只跑一次、只报准确率。现实部署中，用户通过聊天界面交互，模型可能带有 web search，且随机采样导致同一问题可能得到不同回答。这些被忽略的条件是否影响评测结论？作者决定对最广泛使用的模型进行严格审计。

**实验设计**：选取两个高引用安全/偏见基准——BBQ（偏见）与 SafetyBench（安全），分层抽取 401 条提示。每个提示在“ChatGPT 聊天界面 vs OpenAI API”以及“启用 vs 禁用 web search”组成的 4 种条件下各运行 3 次，共收集 4812 条响应。评估指标包括准确率、响应一致性、文本相似度、引用溯源和弃权行为。

**关键结果**：(1) 搜索关闭时，UI 在两个基准上的准确率均低于 API；(2) 启用 web search 使准确率最高下降 8 个百分点，并在一项基准上逆转了模态优劣方向；(3) 重复运行中高达 21% 的 prompt 产生不一致答案；(4) 两种模态给出了不同的引用，弃权行为也高度不一致。模态间准确率差异虽仅 2–3 个百分点且统计显著，但更重要的是这些行为变异在常规准确率报告中完全不可见。

**理论意义**：作者将 LLM 评估重新定义为“测量设计问题”，强调安全评估必须像科学测量一样明确条件变量（模态、搜索、重复次数、回应级属性），并指出单点准确率指标掩盖了模型行为中的关键变异性。他们的审计不仅揭示了方法论缺陷，也提供了可复现的审计协议（代码已开源）。

**实践价值**：对 AI 安全实践者、第三方审计机构和监管者而言，该工作提供了具体警示：任何声称“模型在基准上安全”的结论都应附带其测试条件；对高风险的部署决策，应进行多条件、多次运行的稳健性评估。

**局限**：只测试了一个模型，且未披露模型具体版本；搜索实现的具体细节、采样权重等需见原文；实验未直接度量搜索结果质量对准确率的影响机制；对 UI/API 差异来源的解释仍需进一步验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent 方向有参考价值：agent 通常通过 API/工具链调用模型，本文显示搜索条件可以显著改变模型行为，agent 评测必须报告工具使用状态和重复运行一致性。

## 基本信息

- 作者：Ro Encarnación, Tina Behzad, Emma Lurie, Danaé Metaxa
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.HC, cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06202v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次回答主要依据论文摘要和 PDF 语义检索命中的 Abstract、Introduction、Method、Discussion、Conclusion 片段综合生成，未获取全文细节，部分实验设置（如模型版本、搜索实现）为合理推断或需原文确认。
