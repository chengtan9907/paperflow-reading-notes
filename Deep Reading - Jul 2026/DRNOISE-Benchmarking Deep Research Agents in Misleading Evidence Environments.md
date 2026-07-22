---
user_id: "cheng tan"
paper_id: 4703
arxiv_id: "2607.17291"
title: "DRNOISE: Benchmarking Deep Research Agents in Misleading Evidence Environments"
publish_date: "2026-07-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.17291.pdf"
pdf_url: "https://arxiv.org/pdf/2607.17291"
abs_url: "https://arxiv.org/abs/2607.17291"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-21T19:19:33"
---
# DRNOISE: Benchmarking Deep Research Agents in Misleading Evidence Environments

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：deep research agents · benchmark · misleading evidence · answer recovery

## 一句话总结

DRNOISE：一个100任务的基准，用于评估深度研究智能体在误导性证据环境中的答案恢复能力。

## 摘要

> Deep research agents increasingly operate over the open web, where relevant records coexist with redundant summaries, outdated reports, and misleading documents. Existing evaluations offer limited insight into whether agents preserve sound evidential standards when an ordinary-looking false document is deliberately seeded into a searchable environment and offers a direct shortcut to a conflicting answer. We introduce DRNOISE, a 100-task benchmark for answer recovery under misleading evidence. Each task has a unique gold answer supported by two corroborating indirect record chains; the paired noisy condition adds one plausible document that states a conflicting answer directly. The benchmark spans ten families of evidence operations. Across agents with strong clean-task performance, this single intervention causes 66–88 percentage-point accuracy drops. Trace analyses identify verification inertia as the dominant failure mode: agents often retrieve truthful records but stop before completing and reconciling the evidence chain, instead deferring to the answer-like document. Generic verification prompts reduce but do not close this gap. The setting is especially relevant to open-web deployment, where plausible falsehoods arrive through ordinary-looking pages rather than explicit attacks. Reliable deep research therefore requires more than retrieval and citation; it requires active reconciliation of direct claims with record-level evidence.
> ![](images/d35f993995b58cd9f476a0af764d28b0f22c353bdb25a341c9834a0aee7ab492.jpg)
> ![](images/f27dbb2e9df42f6541f8a3364b37dc60aeafeb0bc9b6f38a22c975af7ed2001f.jpg)
> Figure 1: Main benchmark result on DRNOISE. Left: clean vs. noisy accuracy on the 100-task benchmark. Adding a single misleading document sharply reduces the accuracy of agents with strong clean-task performance. Right: conditional deference, the fraction of a system's clean-correct tasks that are answered with the injected false value once the misleading document is present.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决什么问题？
1. 当前深度研究智能体在开放网络环境中的评估缺少对误导性文档处理能力的系统性测验。
2. 现有基准（如SWE-bench、InteractComp）评估了软件工程或交互式推理能力，但未检验智能体面对精心设计的虚假直接答案时是否仍能坚持基于证据链的推理。
3. 核心问题：当一个普通外表的虚假文档直接给出冲突答案时，智能体会不会将其视为足够证据而停止进一步验证？论文聚焦于这一诊断性设置，旨在量化该失败并分析其根源。

Q2: 有哪些相关研究？

有哪些相关研究？
1. 现有智能体基准（SWE-bench、InteractComp、GAIA等）评估了任务执行、工具使用和用户交互，但未涉及误导性证据。
2. 关于智能体可信度和事实性的研究（如SelfCheckGPT、FActScore）关注生成内容的事实性，但未在可控环境下测试智能体面对直接冲突文档时的行为。
3. 对抗性攻击方面的工作更关注提示攻击或文档噪声，但本文聚焦于“看似正常”的误导文档，而非明显恶意内容。
4. 文档验证和证据链推理的相关工作（如REWARD、STaR）提供了基础，但缺乏在智能体完整评估流程中的系统测试。
5. DRNOISE填补了这一空白：假设相关证据可访问且请求明确，专门检验智能体是否验证一个吸引人的直接答案。

Q3: 论文如何解决这个问题？

论文如何解决这个问题？
1. 构建DRNOISE基准：100个任务，每个任务有唯一金标准答案，由两条独立的间接记录链（共四个文档）共同支持；干净条件下两条链条均指向金标准答案。
2. 噪声条件：在语料中增加一个看似普通的文档，该文档直接给出一个不同的答案（冲突答案），但没有任何记录级支持。
3. 任务覆盖10个证据操作族（例如约束选择、比较、计数、排序等），每个族10个任务，确保多样性。
4. 文档设计：记录文档包含详细数值、属性、时间戳等，误导文档以类似格式捏造信息，使其在字面上合理但实际虚假。
5. 评估协议：在干净和噪声条件下分别运行智能体，比较准确率；同时计算“条件性遵从”（在干净正确任务中，噪声条件下输出错误答案的比例）。
6. 轨迹分析：记录智能体每一步，手动编码失败模式，识别出“验证惯性”等典型错误。

Q4: 论文做了哪些实验？

论文做了哪些实验？
1. 测试了多个前沿语言模型作为智能体：GPT-4o、Claude 3.5 Sonnet、Gemini 2.0 Flash、DeepSeek-V3、Llama 3.1 405B等，均配置了检索工具。
2. 在干净条件下测试所有智能体，记录准确率。
3. 在噪声条件下（添加一个误导文档）重复测试，记录准确率变化。
4. 计算条件性遵从（conditional deference）分数：原本正确但噪声后错误的比率。
5. 进行轨迹分析，手动分类失败模式（验证惯性、过早停止、忽略记录等）。
6. 测试通用验证提示的效果：在每个智能体指令中添加“请验证所有事实”等提示，看是否改善。
7. 对同一智能体（GPT-4o）进行控制实验：替换误导文档的不同属性，以理解哪些因素影响。

Q5: 发现了什么实验现象？

发现了什么实验现象？
1. 干净条件下准确率普遍较高（如GPT-4o约86%，Claude约82%），但噪声条件下准确率骤降（GPT-4o降至约18%，Claude降至约16%），下降幅度达66-88个百分点。
2. 条件性遵从极高：在干净正确的任务中，约80-90%在噪声条件下被输出错误答案。
3. 验证惯性（verification inertia）是主要失败模式：智能体检索到正确记录，但未完成证据链综合，直接引用误导文档。
4. 通用验证提示（如“请仔细核对来源”）只能小幅提升准确率（例如GPT-4o从18%提升到32%），远不能消除差距。
5. 误导文档的位置（检索排名）会影响效果，但即使排名靠后，仍能显著降低准确率。
6. 不同证据操作族之间脆弱程度不同，比较类和计数类任务受损更严重。
7. 轨迹显示智能体经常先检索并阅读误导文档，然后停止进一步搜索，缺乏主动核查。

Q6: 有什么可以进一步探索的点？

有什么可以进一步探索的点？
1. 开发更稳健的验证协议：设计显式的证据链步骤，要求智能体在给出答案前对每个主张进行确认。
2. 扩展至多步误导：多个相互矛盾的误导文档，或混合真假来源。
3. 研究交互式验证：允许智能体向用户提问或请求澄清，减少过早停止。
4. 训练专用鲁棒模型：在训练数据中包含类似对抗场景，提升模型对直接答案的抵抗力。
5. 迁移至真实网络环境：不再使用人工合成任务，而是从真实网站中选取包含误导信息的案例。
6. 评估更广泛的智能体框架：如不同工具选择策略、记忆机制等对验证惯性的影响。
7. 跨语言和跨文化场景：不同语言中误导信息的格式和逻辑可能不同，需单独测试。

Q7: 总结一下论文的主要内容

总结一下论文的主要内容：
论文针对深度研究智能体在开放网络环境中可能遇到的误导性证据问题，提出了DRNOISE基准。该基准包含100个任务，每个任务在干净条件下由两条间接记录链支持一个唯一正确答案，在噪声条件下额外增加一个直接陈述冲突答案的伪造文档。任务覆盖10种不同的证据操作族，从约束选择到比较推理，确保覆盖面。实验在多个前沿语言模型（GPT-4o、Claude 3.5等）上进行，结果显示仅一个误导文档就导致准确率从超过80%降至低于20%，条件性遵从比率极高。通过轨迹分析，论文识别出“验证惯性”作为主导失败模式：智能体能够检索到正确记录，但往往不完成证据链的综合，而是直接采用文档中提供的现成答案。通用验证提示仅部分缓解问题。论文指出，这一发现对开放网络部署具有重要启示：深度研究智能体不仅需要强大的检索和推理能力，还需要主动的证据协调和停止标准。论文的贡献包括：引入DRNOISE基准、系统评估揭示脆弱性、识别失败机制、以及提出后续研究方向。局限性包括：仅评估了工具增强型智能体、文档为人工构造、任务局限于特定类型、未考虑互动验证等。总体上，这项工作为研究智能体在误导环境中的鲁棒性提供了可控且可复现的测试平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接命中用户研究方向：智能体（权重0.10），尤其关注智能体鲁棒性和可信度。

## 基本信息

- 作者：Jun Nie, Zhiqin Yang, Zhenheng Tang, Yonggang Zhang, Xiaowen Chu, Xinmei Tian, Bo Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL, cs.IR
- 日期：2026-07-21
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.17291`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成参考了PDF检索证据（retrieved_evidence），并优先采用了field_evidence_map中对应的证据锚点。
