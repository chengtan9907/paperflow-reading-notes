---
user_id: "cheng tan"
paper_id: 15916
arxiv_id: "2610.18616v2"
title: "Knowledge Graphs as Files: An Agent Harness Where Tool Creation and Tool Use Co-Evolve"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18616v2.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18616v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:01:27"
---
# Knowledge Graphs as Files: An Agent Harness Where Tool Creation and Tool Use Co-Evolve

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge graph reasoning · llm agent · tool creation · agentic code execution

## 一句话总结

论文提出 DKGI——一种让 LLM agent 把知识图谱当作完整节点/边文件、自己编写并执行 Python 图访问代码的 agent harness；在 STaRK 三个文本丰富知识图谱上，无需训练的 DKGI 以至少 6.7 个 Hit@1 点的优势超过最强已发表基线。

## 摘要

> 摘要首先用一张三设置对比表说明问题：①直接工具使用（ToG、Graph-CoT、ARK、GRASP 等）依赖预定义图接口；②在图接口之上写代码（KG-Agent、Code-on-Graph、AvaTaROct 等）仍受接口约束；③DKGI 让 agent 直接对完整节点/边文件编程，没有预定义图 API 或子图检索器。DKGI 中，agent 编写 Python 代码沿关系跳转、合并节点集、选择文本证据，并将程序执行与 shell 搜索、文件检查交错进行；大节点集可以批量处理，只有被选中的文本返回给模型评估。执行反馈引导工具创建与修订，演化中的工具又决定下一轮收集哪些证据。在横跨三个领域的 benchmark 上，无需训练的 DKGI 即使使用最小化 harness，也在每个图上超过最强已发表基线至少 6.7 个 Hit@1 点；受控消融显示多轮编程优于纯 shell 访问和预定义接口。摘要结尾部分在提供的 PDF 文本中截断，更多定量细节需回原文核对。

Q1: 这篇论文试图解决什么问题？

论文要解决的问题可拆成三层。任务层：在文本丰富的知识图谱上做搜索/问答时，agent 既要沿关系路径跳转，又必须检查支撑每个候选答案的原始文本证据；答案往往分散在多个记录中，需要把跨记录证据连接起来，同时阅读候选记录的文字内容。接口层：现有方法通常把图访问封装成预定义接口——检索器先抽出推理路径或子图再交给 LLM，或者给 agent 一组预定义图函数；论文指出，这种封装虽然控制了上下文规模，但也决定了 agent 能接触的证据范围。即使 agent 被允许在图接口之上写代码来组合工具调用或处理子图，它仍然无法越过预定义图 API 看到完整的节点/边文件。因此核心 gap 是缺少一种“直接知识图谱交互”设置：让 agent 自己创建图访问工具，直接面对完整的 node/edge 文件。工程层：直接暴露完整文件会带来上下文管理问题，不能让模型读全部文本；需要支持 agent 批量处理大节点集，同时只把选中的文本证据返回给模型。此外，工具不应是一次性写死的，执行反馈应当能引导工具创建与修订，而工具的变化又决定 agent 下一轮收集什么证据，形成 tool creation 与 tool use 的共演化。需要说明的是，本次可用的 heuristic_draft 与检索片段高度碎片化，上述 gap 分析主要依据 Table 1 的三设置对比、摘要表述和结论片段；更细的动机陈述需回原文 Introduction 核对。

Q2: 有哪些相关研究？

相关工作可按图访问方式分三条线。第一条是 retrieve-then-reason：先检索推理路径或子图再交给 LLM 推理，代表工作包括 Jiang et al. 2023b、Luo et al. 2024b、Kim et al. 2023、He et al. 2024、Mavromatis & Karypis 2025、Li et al. 2025b。第二条是预定义图工具上的 agent 导航：ToG（Sun et al. 2024；Ma et al. 2025）让 LLM 在预定义 beam search 上选择实体和关系；StructGPT（Jiang et al. 2023a）、Graph-CoT（Jin et al. 2024）、GoG（Xu et al. 2024）、Plan-on-Graph（Chen et al. 2024a）、GRA（Dragic et al.）提供不同的预定义图函数集合；摘要表格中还把 ToG、Graph-CoT、ARK、GRASP 归为“直接工具使用”。第三条是 graph tools + code：KG-Agent、Code-on-Graph、AvaTaROct 等允许 agent 用代码组合图工具调用，但仍依赖图接口，未直接访问完整节点/边文件。论文的定位是这三条线之外的“直接知识图谱交互”：没有预定义图 API 和子图检索器，agent 编程直接面对文件。此外，检索片段还提到有监督的 mFAR retriever 和用 LoRA 微调的 Qwen3-Reranker-0.6B 作为基线/重排序组件。由于 PDF 提取残缺，这些引文的具体方法细节和贡献无法逐一展开，需要对照原文献阅读。

Q3: 论文如何解决这个问题？

DKGI 的核心做法是把知识图谱序列化为完整节点文件和边文件，并让 LLM agent 在沙箱中直接对这些文件编写、执行和修订 Python 代码。环境提供 get_schema() 这类辅助调用，返回 schema.json，帮助 agent 理解文件结构；除此之外，图内容不再通过预定义图 API 暴露。agent 的图访问原语是程序本身：它写代码沿关系跳转、对节点集做集合运算、按条件筛选文本证据，然后执行；执行结果只把被选中的文本返回给模型评估。DKGI 还允许 agent 把程序执行与 shell 搜索、文件检查交错进行，从而在写代码之外用轻量命令快速定位文件和片段。为控制上下文，工具输出采用 head-and-tail 截断：先返回前 9000 字节或 900 行，提示一行，再返回最后 9000 字节；这一策略是为了适配 Pi 的 bash 工具 50KB 输出限制和 DCI Agent-Lite 的 20,000 字符上下文管理限制，保证模型能看到程序两端的输出而不被中间内容淹没。执行反馈是方法的关键闭环：程序报错、空结果、截断提示都会进入下一轮程序修订；论文给出的示例中，agent 的成员测试因逐字符匹配失败而打印出两个文件名，在打印文件头（call 12）之后，于 call 13 修订程序。安全与公平性方面，运行环境只读，并阻止网络访问和标签/答案访问（附录 C）。受控变体区分三种设置：直接调用预定义图工具、在图接口上写代码、直接文件访问（DKGI），并通过消融隔离多轮编程、单轮编程和纯 shell 访问的贡献。需要指出，部分关于沙箱工具禁用（如 grep/awk/jq 是否可用）的片段较破碎，具体变体配置需回原文 §3.3 核对。

Q4: 论文做了哪些实验？

论文在 STaRK（Wu et al. 2024b）的三个文本丰富知识图谱上做评测：Amazon、MAG、Prime。这些图规模上限为 10 种实体类型、18 种关系类型、1.9M 实体和 39.8M 边；查询为英文，节点记录也几乎全英文。Amazon 数据包含用户评论，MAG 数据包含作者姓名和机构信息。实验是 training-free 设置，不训练检索器或重排序器。主结果使用 Hit@1 和 MRR 作为指标，报告点估计。主实验使用 GPT-6-Astra：DKGI 在每个 STaRK 数据集上都达到最高 H@1 和 MRR。受控消融使用 GPT-6-Luna，在更小/更受控的 harness 中对比三种设置：直接调用预定义图工具、在图接口上组合代码、直接文件访问；另外比较多轮编程与纯 shell 访问。基线和参照包括最强已发表基线、预定义图工具、有监督的 mFAR retriever，以及用 LoRA 微调的 Qwen3-Reranker-0.6B。论文还报告了 Table 5，将 DKGI 的错误/行为与预定义图工具进行对照，并包含逐任务轨迹分析。由于可用 PDF 文本截断严重，完整实验协议、每个数据集的精确分数、消融表格条目和统计显著性无法在此还原；需要阅读原文 Experiments 与附录。

Q5: 发现了什么实验现象？

实验现象主要有五点。第一，训练无关的 DKGI 在 STaRK 的每个图上都超过最强已发表基线至少 6.7 个 Hit@1 点，即使使用最小化 harness 也成立。第二，使用 GPT-6-Astra 时，DKGI 在每个 STaRK 数据集上都取得最高 H@1 和 MRR。第三，使用 GPT-6-Luna 的受控对比中，DKGI 的整体 H@1 高于预定义图工具——无论这些工具是被直接调用还是在代码中被组合；这说明收益来自直接文件访问本身，而不只是“会写代码”。第四，多轮编写和修订程序带来的增益最大，即 agent 不是一次生成最终程序，而是在执行反馈中逐步修正工具；这直接支持“工具创建与工具使用共演化”的主张。第五，轨迹案例显示，一个成员测试程序因为逐字符匹配失败而打印出两个文件名；agent 在打印文件头之后修订程序，下一轮程序转向更合理的两分支处理。检索片段没有给出明显的负结果或指标间张力；唯一明确提到的边界是聚合类问题和随图规模扩展留待未来工作。另需注意，现有证据不足以判断 DKGI 在哪些查询类型上失效，以及 Hit@1 与 MRR 之间是否存在不一致。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：（1）聚合类问题（aggregation questions）——论文明确将其留待未来，这类问题可能要求跨大量节点统计，而不是沿路径找证据；（2）DKGI 随图规模扩展性——当图大到无法扫描时，需要新的索引、采样或分布式策略；（3）更智能的上下文管理——当前 head-and-tail 截断只保留两端输出，若关键证据在中间会被截断，可研究流式、分段或按需展开机制；（4）工具记忆与复用——agent 在同一任务中创建的工具是否可以跨任务保存、版本化或组合成工具库；（5）更强/更弱的模型泛化——DKGI 依赖模型写代码和修订代码的能力，较弱模型上的表现未知；（6）动态/更新型知识图谱——文件快照方式如何应对图更新；（7）跨语言和其他 benchmark——当前 STaRK 查询和记录几乎全英文，多语言及更多领域图待验证；（8）安全与公平性——只读环境阻止网络和标签访问，放宽限制后 agent 可能引入外部知识或在线检索，需要设计新的评估协议。以上第（3）至（8）条属于合理推断/推测，论文原文只明确提到聚合问题与规模扩展。

Q7: 总结一下论文的主要内容

论文的论证主线是：现有文本丰富知识图谱搜索系统把图访问封装为预定义接口（检索路径/子图或图 API），这种封装在控制上下文的同时也决定了 agent 能接触的证据范围；即使允许 agent 在图接口之上写代码，也无法访问完整节点和边文件。因此作者提出第三种设置 DKGI，把图当作文件直接暴露，让 agent 自己编写图访问代码，形成“工具创建与工具使用共演化”的闭环。技术主线上，DKGI 是一个最小化 harness：节点与边以文件形式存在，agent 通过 get_schema() 获得 schema.json，然后写 Python 程序沿关系跳转、合并节点集、筛选文本证据；程序执行与 shell 搜索、文件检查交错进行，执行反馈进入下一轮程序修订。为控制上下文，工具输出采用 head-and-tail 截断（先返回前 9000 字节或 900 行，再返回最后 9000 字节），并适配 bash 工具 50KB 与 DCI Agent-Lite 20,000 字符限制；运行环境只读，禁止网络和标签访问。实验主线上，论文在 STaRK 的 Amazon、MAG、Prime 三个文本丰富知识图谱上做训练无关评测，用 Hit@1 和 MRR 报告点估计；最强配置 GPT-6-Astra + DKGI 在每个数据集上都超过所有已发表基线，最小化 harness 也至少领先最强已发表基线 6.7 个 Hit@1 点。受控消融用 GPT-6-Luna 对比直接调用预定义图工具、在图接口上写代码、直接文件访问三种设置；结果是 DKGI 整体 H@1 更高，且多轮编写/修订程序带来的增益最大。轨迹分析显示工具创建与工具使用在同一任务内互相塑造。论文承认的边界包括：仅覆盖 STaRK 三图、查询和记录几乎全英文、聚合类问题和图规模扩展留待未来、只读环境阻止网络与标签访问。由于可用 PDF 提取残缺，上述描述中部分机制细节来自片段推断，原文 §3 与附录 C 需进一步核对。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体方向：论文属于 LLM agent harness 研究，核心是 agent 自主创建工具、用执行反馈修订工具，与 agent 方向高度相关。

## 基本信息

- 作者：Yuyang Bai, Yu Wang, Yu Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.IR
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18616v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 retrieved_evidence 的 PDF 语义检索片段；由于原文提取截断严重，部分方法细节与边界条件为合理推断，需以原文核对。
