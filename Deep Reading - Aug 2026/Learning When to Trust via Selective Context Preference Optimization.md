---
user_id: "cheng tan"
paper_id: 7011
arxiv_id: "2608.06377v1"
title: "Learning When to Trust via Selective Context Preference Optimization"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06377v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06377v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T14:05:43"
---
# Learning When to Trust via Selective Context Preference Optimization

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：selective trust · misleading context · robustness evaluation · direct preference optimization

## 一句话总结

本文提出选择性信任（selective trust）框架，构建MIST基准和SC2W指标，并给出SCOPE方法；SCOPE通过在四种匹配条件下平衡的DPO偏好对训练，大幅降低开源模型被误导性上下文翻转答案的比例，同时保留模型对正确、干净和无关上下文的原有准确率。

## 摘要

> Language models increasingly condition their answers on external signals, and a single misleading one can turn a correct answer wrong. The obvious remedy, training models to resist such signals, hides a failure mode: a model that ignores all context looks robust yet is useless when the context is worth trusting. We recast the problem as selective trust and introduce MIST, a human-annotated benchmark that renders each reasoning item under four matched conditions (clean, misleading, correct-context, and irrelevant-context), together with SC2W, a paired metric counting how often a misleading signal flips a clean-correct answer to wrong. Across a comprehensive benchmark study, we observe that such a susceptibility is universal. We then propose SCOPE, which mines clean-correct/misleading-wrong failures and optimizes a standard Direct Preference Optimization (DPO) objective over matched preference pairs balanced equally across all four conditions, rather than over misleading items alone. Our approach substantially reduces SC2W on popular open-sourced models while preserving accuracy when the added context is clean, correct, or irrelevant. With this work, we argue that models should be judged on selective trust, not on resistance alone.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：语言模型在生成回答时越来越依赖外部信号（提示、上下文、系统指令甚至可能是检索结果），而单个误导性信号足以把原本正确的答案翻转成错误答案。论文把这种脆弱性抽象为“选择性信任”问题：模型需要在什么时候信任、什么时候不信任外部信号之间做出正确判断。
2. 现有对策的缺陷：直观方案是训练模型抵抗误导信号（例如在误导样本上做鲁棒化训练），但这会走向另一个极端——模型对所有信号都持怀疑态度，出现“全盘不信任”的鲁棒假象。在单一条件评估下，这种全盘不信任的模型与真正按信号质量做判断的模型表现无异，但实际部署中前者毫无用处。因此单条件评测无法区分“盲目抵抗”和“选择性信任”。
3. 评测缺失：缺乏一个能同时测量“抗误导”和“保留对正确上下文利用能力”的基准与指标。现有基准通常只在一个或少数条件下评测，无法刻画上下文角色变化带来的行为差异。
4. 训练数据偏差：即使做偏好优化，如果只在误导样本上优化，模型可能学会一刀切地忽略所有上下文；需要设计一个让模型区分四种上下文角色（干净、误导、正确、无关）的训练信号。
5. 普遍性验证不足：论文要回答的核心实证问题是——这种“一个误导信号翻转正确答案”的易感性是否跨模型普遍存在？是否连前沿模型也会中招？

Q2: 有哪些相关研究？

1. 上下文鲁棒性研究：已有工作关注模型对上下文干扰的鲁棒性，但大多只评估单一方面（例如对误导提示的抵抗），未同时考虑对正确上下文的利用，无法刻画选择性信任。
2. 偏好优化方法：DPO（Direct Preference Optimization）等基于偏好的对齐方法被广泛用于让模型学会选择更优回答，但其在误导性上下文场景下的应用主要局限于“误导-正确”配对，缺少对上下文角色的平衡控制。论文的SCOPE直接基于DPO，但通过信号反事实配对和四条件平衡扩展了其适用性。
3. 反事实评估：利用反事实（counterfactual）样本评估模型因果推理和鲁棒性是常见范式，本文的“信号反事实”（signal-counterfactual）体现在把同一推理题置于误导、正确、无关等条件下，形成配对的上下文反事实。
4. 用户偏好与事实性权衡：相关研究如Sharma et al. (2024)探讨了模型在“迎合用户偏好”与“坚持事实”之间的权衡，本文Fig. 1显示权威外观的错误提示甚至能让前沿模型从正确翻转为错误，与该线工作呼应。
5. 基准构建：MIST与现有推理基准（可能从公开基准改编而来）的区别在于同一题目生成四种匹配条件，并人工标注确保条件配对的语义平行，从而准确归因上下文角色的影响。
6. 信任校准：与“模型应该何时信任检索信号”的检索增强生成（RAG）校准工作相关，但本文面向更一般的上下文信号，不限于检索。

Q3: 论文如何解决这个问题？

1. 问题重定义：将“抗误导鲁棒性”重新定义为“选择性信任”——模型应当按信号本身的可信度决定是否采纳，而不是一味抵抗或一味服从。2. 构建MIST基准：从公开推理基准中筛选/改编题目，人工标注生成四种匹配条件：clean（干净，无额外上下文）、misleading（误导性上下文）、correct-context（正确上下文，帮助解题）、irrelevant-context（无关上下文）。四种条件共享同一推理题，保证除上下文角色外其余完全匹配，从而把行为差异归因于上下文角色。3. 提出SC2W指标：统计“干净条件下回答正确，但加上误导上下文后回答错误”的样本比例（clean-correct → misleading-wrong），即误导信号翻转正确回答的频率。该指标直接衡量模型对误导信号的易感性。4. SCOPE训练方法：（1）挖掘失败样本：从模型在MIST上的预测中找到clean-correct和misleading-wrong的配对样本，作为训练信号；（2）构造信号反事实偏好对：对同一题目，在同一上下文条件下生成回答，并复用同一配对用于所有四种条件，使学习信号绑定上下文角色而非单个提示的表面形式；（3）平衡四种条件：在DPO目标中，均衡地包含clean、misleading、correct-context、irrelevant-context四种条件下的偏好对，而不是只在误导条件下训练。这样模型被迫学会区分上下文角色，而不是简单忽略所有上下文。5. 评估：在主流开源模型上评测SC2W下降幅度，并检查在clean和correct-context下准确率是否保持（即不牺牲对有用信号的利用），以及对irrelevant-context的容忍度。

Q4: 论文做了哪些实验？

1. 基准实验：在MIST的四种条件下评测一系列流行开源模型（包括前沿模型），测量误导条件相对于干净条件的准确率下降，用SC2W量化；Fig. 1展示了权威外观错误提示使前沿模型从正确翻转为错误的现象。2. 跨模型普遍性验证：在多个模型家族（不同规模）上重复测量SC2W，验证易感性是否普遍存在。3. SCOPE训练实验：（1）对主流开源模型应用SCOPE，对比训练前后SC2W变化；（2）关键消融：对比只在误导条件上做DPO（misleading-only）与SCOPE的四条件平衡训练，检验是否出现“全盘不信任”现象（即在干净和正确上下文上准确率大幅下降）；（3）可能还对比了不同偏好对来源、不同配对策略，但摘要证据有限。4. 保持性检查：验证SCOPE训练后模型在clean、correct-context、irrelevant-context条件下的准确率是否与未训练时基本一致。5. 消融/分析：可能分析SC2W与模型规模的关系、不同题目类型的影响等。由于证据片段有限，以下为合理推断：实验应当还包含与SCOPE的变体比较（如不同平衡策略），以及错误翻转案例的定性分析。

Q5: 发现了什么实验现象？

1. 易感性普遍存在：跨所有测试的开源模型，误导上下文都能以显著频率翻转干净-正确回答，包括前沿模型（Fig. 1）。这说明“单个误导信号翻转正确答案”不是个别模型的缺陷，而是普遍现象。2. 权威外观的误导信号特别有效：Fig. 1中使用了“official-looking but wrong hint”（看起来官方但实际错误的提示），即使前沿模型也会被翻转为错误，说明表面权威性影响模型的信任判断。3. 单条件评测的错觉：在单条件评测下，全盘不信任的模型与选择性信任的模型表现无异；论文强调这种评估歧义是该问题的关键障碍。4. SCOPE能大幅降低SC2W，且不牺牲对干净和正确上下文的准确率；对比之下，仅在误导条件上训练很可能导致模型对上下文整体不敏感（全盘不信任），在干净和正确条件下准确率下降。5. SCOPE对无关上下文也保持原有行为（不因训练而改变对无关信息的处理）。6. 失败模式和指标张力：若只用“抗误导”一个指标，无法区分“模型忽略一切”和“模型明智信任”；SC2W + 条件保持准确率共同构成选择性信任的完整评价。7. 信号反事实配对的关键作用：复用同一回答配对到所有四种条件，使得训练信号绑定上下文角色，这比在单一提示表面形式上训练更有效。

Q6: 有什么可以进一步探索的点？

1. 更广泛架构与规模：论文的局限性提到“two-family mitigation leaves broader architectures and scales untested”，因此可扩展到更多模型家族（如编码器-解码器、MoE）、更大规模模型，验证SCOPE的泛化性。2. 部署场景的迁移：MIST是有对照的文本诊断，其易感性比率不能直接等于部署中的发生频率；需要在实际RAG、工具调用、agent 场景中评估误导信号的真实暴露率。3. 链式思维忠实性：当前鲁棒性是在匹配提示下测量的，不能证明CoT内部的忠实性；未来可研究SCOPE是否影响CoT的可信度。4. 污染消除：MIST题目多改编自公开基准，存在数据污染风险；未来应构建污染可控的新题目集，或使用动态生成的题目。5. 更细粒度的信任校准：从二值“信任/不信任”发展为连续信任分数，结合不确定性估计。6. 多模态条件：扩展到视听、图像等多模态误导信号。7. 在线/交互场景：在agent连续多轮交互中，误导信号可能累积，需要研究选择性信任是否能在多步决策中保持。8. 结合检索验证：如何让模型在信任之前主动验证信号来源，结合事实核查工具。9. 社会性提示：权威性（如官方格式）、措辞、来源信誉等属性对信任判断的影响，可进一步建模。10. SCOPE的理论性质：平衡四种条件的DPO是否从理论上保证贝叶斯意义上的最优信任策略。

Q7: 总结一下论文的主要内容

论文针对语言模型在外部信号影响下回答易被误导的问题，提出应从“选择性信任”而非“单纯抗误导”的角度来评估和改进模型。作者指出，现有直观方法（训练模型抵抗误导信号）会导致模型全盘不信任上下文，在上下文有用时不再利用；在单条件评测中这种退化行为与理想的选择性信任行为无法区分。为此，论文构建了MIST基准：将每个推理题目人工标注为四种匹配条件（干净、误导、正确上下文、无关上下文），保证共享题目内容，只改变上下文角色。基于该基准提出SC2W配对指标，统计干净-正确回答被误导信号翻转的比例，直接衡量易感性。在多个开源模型上的研究表明，这种翻转现象普遍存在，甚至前沿模型也会被外表权威的错误提示欺骗。在此基础上提出SCOPE训练方法：挖掘clean-correct和misleading-wrong的失败配对作为偏好对，构造信号反事实样本，并在四种条件上均衡优化标准DPO目标，使模型学会根据上下文角色决定信任程度，而不是简单忽略上下文。实验显示SCOPE大幅降低SC2W，同时保持模型在干净、正确和无关上下文上的准确率，证明了选择性信任目标的可行性。最后，论文主张模型评测应综合“抗误导能力”和“利用有用上下文能力”，以选择性信任为标准。局限包括限定的文本诊断设置、模型家族覆盖有限、无法排除数据污染等。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体（agent）方向：智能体高度依赖外部信号（工具输出、环境反馈、检索结果），选择性信任直接关系到agent在错误信息面前保持任务正确性；SCOPE的四条件平衡思想可用于agent训练中的鲁棒性优化。

## 基本信息

- 作者：Xian Sun, Wei Chow, Yingshuo Wang, Junhao Liu, Wei Gao, Qing Wu, Lingdong Kong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06377v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据（abstract、introduction、conclusion、limitations片段），并结合heuristic_draft进行补全；由于缺乏正文内容，部分实验细节和数值为合理推断或标注为需要查证。
