---
user_id: "cheng tan"
paper_id: 9074
arxiv_id: "2608.19437"
title: "Are LLMs becoming similarly creative? Evidence from three years of models"
publish_date: "2026-08-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.19437.pdf"
pdf_url: "https://arxiv.org/pdf/2608.19437"
abs_url: "https://arxiv.org/abs/2608.19437"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-21T23:55:41"
---
# Are LLMs becoming similarly creative? Evidence from three years of models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm creativity · output diversity · semantic embedding similarity · alternate uses task

## 一句话总结

本文对 2023 年至今三年来各代 LLM 在开放型创意任务上的输出进行了初步分析，利用 Infinity-Chat100 真实用户查询和 Alternate Uses Task 心理测量学任务，通过句子嵌入相似度衡量输出多样性，发现模型输出多样性随时间显著下降，提示 LLM 创意输出可能正在跨模型趋同，进而可能削弱人机共创中的人类能动性。

## 摘要

> Many benchmarks track Large Language Model (LLM) performance on tasks with verifiable answers, but less is known about how LLM performance is evolving on open-ended tasks, where creativity, originality and diversity may matter as much as quality. As LLMs increasingly support human ideation and creative work, understanding trends in LLM performance on open-ended tasks is critical. This paper presents a preliminary analysis of LLM creative outputs spanning three years of model releases, examining model responses to Infinity-Chat100, a real-world collection of open-ended user queries, and the Alternate Uses Task, an established psychometric creativity assessment. Using sentence-embedding similarity, we examine trends in LLM responses to these prompts. Our findings show a statistically significant decrease in model output diversity over time, suggesting that LLM outputs may be converging in creative substance across models. If this trend persists, LLM-driven homogenization may progressively diminish human agency in human-AI co-creative work, demanding careful consideration of LLMs' role in the human creative process.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是：现有 LLM 基准大多聚焦于具有可验证答案的任务，而 LLM 在开放型、无唯一正确答案的创意任务上的表现如何随时间演变，尚缺乏系统认识。具体而言，论文关注三点：(1) 三年间发布的各代模型在开放型创意任务上的输出多样性是否发生变化；(2) 不同模型之间以及同一模型不同代际之间，创意输出是否趋于同质化；(3) 若输出多样性下降，这种 LLM 驱动的同质化对人类创意过程、人类能动性以及人机共创实践可能产生什么影响。作者认为，随着 LLM 被广泛用于支持人类构思和创意工作，模型输出的多样性直接决定了用户接触到的想法范围，从而影响下游人类创造力，因此这一问题是关键但尚未被充分研究。

Q2: 有哪些相关研究？

根据摘要和引言线索，相关研究背景包括：大量基准测试追踪 LLM 在可验证答案任务上的表现，例如推理、编码、数学等；但对开放型任务（如创意写作、发散性思维）的演变趋势关注较少。既有研究表明 LLM 输出可能影响人类创造力，例如模型生成的内容可能塑造用户接触的想法范围，从而影响下游人类创意。Alternate Uses Task (AUT) 是心理测量学中评估发散性思维的经典任务，被用于衡量创意的原创性和灵活性；Infinity-Chat100 是一个真实世界的开放型用户查询数据集，覆盖六类真实查询（证据提及六类，但具体类别未在摘要中列出，需原文确认）。此外，讨论部分提到人类更聪明地使用 AI 模型可能产生更好的创意结果（引文 41、42），暗示人机协作策略对创意产出有影响。总体而言，相关工作横跨 LLM 评估、创造力心理学、人机交互三个领域，但将时间维度和跨模型比较引入创意输出多样性的研究尚属起步。

Q3: 论文如何解决这个问题？

论文采用四步流程进行基于时间的 LLM 输出多样性分析（合理推断自方法论片段）：(1) 选择一组开放型提示词，涵盖 Infinity-Chat100 的六类真实世界查询以及 Alternate Uses Task；(2) 让 2023 年至今各模型提供商和发布时期的模型生成回答；(3) 将回答嵌入语义空间，计算句子嵌入相似度；(4) 比较不同模型和代际之间的输出距离，以衡量多样性随时间的变化。方法核心是利用语义嵌入相似度作为输出多样性的代理指标：距离越大表示多样性越高，距离缩小表示趋同。该设计结合了真实世界用户查询（生态效度高）和标准化心理测量任务（可对比性高），并横跨三年模型代际，构成一个初步的比较框架。需要注意的是，摘要指出这是‘初步分析’，方法细节（如抽样模型列表、温度设置、回答次数、相似度阈值）在现有证据中不完整，需回原文确认。

Q4: 论文做了哪些实验？

根据现有证据，论文的实验设计包括：使用 Infinity-Chat100 数据集（真实世界开放型用户查询，覆盖六类）和 Alternate Uses Task（经典心理测量创造力任务）作为提示来源；选择 2023 年至今的多个模型提供商和发布时期的模型作为对象；让这些模型对两组提示生成回答；对回答进行语义嵌入并计算相似度。证据中提及‘统计分析显著’但未给出具体数值、样本量或效应量。讨论部分提到作者‘轶事性地观察到 InfinityChat 数据集中有几个提示请求相似输出（例如 2 个提示询问电影 Zootopia）’，这暗示数据集中可能存在提示重复或相似性问题。具体的实验协议（如每个模型生成的响应数、温度参数、随机种子等）在检索片段中缺失，需要原文补充。

Q5: 发现了什么实验现象？

论文的核心实验观察是：在三年时间跨度内，LLM 对开放型创意提示的响应多样性在统计上显著下降。具体现象：(1) 输出多样性随时间递减，即不同代际模型生成的回答在语义空间上越来越接近；(2) 这种趋同可能跨模型存在，即不仅同一模型的后续版本，甚至不同提供商模型之间的输出也可能趋于相似（合理推断，摘要明确说‘across models’）；(3) 数据集本身存在提示相似性问题，例如 InfinityChat 中有两个提示都询问电影 Zootopia，但作者认为这不影响其多样性测量的有效性（证据原文：‘While this does not affect our measurement of response...’被截断）；(4) 讨论部分承认即使模型在创意任务上表现良好，收敛的输出也可能限制用户接触的可能性范围，这暗示‘质量’与‘多样性’之间可能存在张力——模型可能变得更‘好’但更‘同质’。值得注意的是，论文标注为 preliminary，因此这些观察需要谨慎解读；负结果或非显著结果未在检索证据中出现，但可能存在。

Q6: 有什么可以进一步探索的点？

论文在讨论中明确表示‘许多有趣的未来工作方向仍然开放’（原文 Future work 片段），并指出‘我们的聚合分析并未确定是否…’（句子被截断，但表明当前分析是聚合层面的，未深入到个体或更细粒度）。可进一步探索的方向包括：(1) 将聚合分析分解到具体提示类别、模型家族或时间窗口，检验多样性下降是否在某些子群体中更显著；(2) 建立因果链条：输出多样性下降是否由共同训练技术（如 RLHF、蒸馏、共享数据）导致，而非偶然；(3) 测量模型多样性与下游人类创造力的实际关系，例如通过受控人类实验验证趋同输出是否真的削弱人类发散性思维；(4) 开发更好的多样性度量，超越句子嵌入相似度（例如结构、概念、叙事层面的多样性）；(5) 考察提示工程或人机协作策略（如多次采样、角色扮演）是否能缓解趋同；(6) 扩展时间范围和模型覆盖面，纳入更多开源与闭源模型；(7) 从描述性分析转向预测性模型，预警哪些应用场景最先出现同质化风险。

Q7: 总结一下论文的主要内容

该论文针对当前 LLM 评测过度聚焦可验证答案任务、忽视开放型创意任务多样性的缺口，提出了一个跨三年模型发布时间轴的初步比较研究。作者选取两类互补的创意提示源：Infinity-Chat100（真实世界开放型用户查询，覆盖六个类别）和 Alternate Uses Task（经典心理测量学发散性思维任务），让 2023 年至今的多个模型提供商及其不同代际模型生成回答，通过句子嵌入相似度计算输出之间的语义距离，以此作为多样性的代理指标。研究结果显示，模型输出多样性在统计上显著下降，表明 LLM 的创意实质正在跨模型趋同。作者认为这一趋势若持续，将威胁人类在 AI 辅助创意工作中的能动性，因为用户接触到的想法范围将被压缩。研究还注意到数据集本身存在相似提示的琐碎问题（如两个关于 Zootopia 的提示），但不影响其测量。讨论部分承认工作为初步性质，并提及人类更熟练地使用 AI 模型可能产生更好的创意结果，暗示解决趋同问题的一个方向是提升人机协作策略。总体而言，论文贡献在于首次从时间维度系统刻画 LLM 创意输出多样性的下降趋势，并提出了创意同质化这一值得关注的风险，但方法细节、统计量和因果解释仍有待后续工作充实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于大模型开放型任务评估与输出多样性，与智能体、生成任务方向相关，可作为评测设计的参考案例。

## 基本信息

- 作者：Nirav Patel, Josiah Crossman, Eva Aggarwal, Emily Wenger
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CY
- 日期：2026-08-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.19437`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要、引言、方法论和讨论片段，但检索证据中缺少完整的实验结果数据和统计量，部分细节（如具体模型列表、统计方法）为合理推断或需回原文确认。
