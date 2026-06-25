---
user_id: "cheng tan"
paper_id: 861
arxiv_id: "2606.19749v1"
title: "Benchmarking Agentic Review Systems"
publish_date: "2026-06-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.19749v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.19749v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-20T01:24:21"
---
# Benchmarking Agentic Review Systems

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic review systems · peer review · LLM evaluation · perturbation benchmark

## 一句话总结

这篇论文系统评估了代理评审系统在追踪论文质量代理指标、检测注入错误和真实用户反馈方面的表现，发现现有系统已能较好反映人类判断但仍有较大改进空间。

## 摘要

> A new class of agentic review systems are emerging as a remedy to the pressure placed on peer review systems by AI-assisted research, but it is unclear how they should be evaluated. We evaluate two open-source systems (OpenAIReview and ‘coarse), one proprietary system (Reviewer3), and a zero-shot baseline, across six LLMs spanning frontier and efficient models. First, we study whether AI reviews on ICLR/NeurIPS papers track with papers’ quality as approximated by external signals such as citations and acceptance decisions. Every system performs above chance in pairwise accuracy, and the best is OpenAIReview + GPT-5.5 at 83.0%. Second, to test whether systems can catch errors with known ground truth, we construct a perturbation benchmark that injects four categories of errors into papers across eight arXiv subject classes and measure detection recall. The strongest configuration (OpenAIReview + GPT-5.5) catches 71.6% of injected errors, leaving substantial room for improvement. The union of detections across six models reaches 83.3% recall, suggesting different models detect different errors and better harness design can potentially increase performance. Beyond these benchmarks, we study a public deployment of OpenAIReview with real users. Votes on its comments skew positive at 1.44 to 1, and the most common complaints are about false positives and minor nit-picks. Together, by evaluating full review systems backed by state-of-the-art models on real research papers, we show that while AI reviews still have room for improvement, they can already track human quality judgments well, catch important errors, and earn positive feedback from real users.
> Summary. Full likelihood-based inference for modern population genetics data presents methodological and computational challenges. The problem is of considerable practical importance and has attracted recent attention, with the development of algorithms based on importance sampling (IS) and Markov chain Monte Carlo (MCMC) sampling. Here we introduce a new IS algorithm. The optimal proposal distribution for these problems can be characterized, and we exploit a detailed analysis of genealogical processes to develop a practicable approximation to it. We compare the new method with existing algorithms on a variety of genetic examples. Our approach substantially outperforms existing IS algorithms, with efficiency typically improved by several orders of magnitude. The new method also compares favourably with existing MCMC methods in some problems, and less favourably in others, suggesting that both IS and MCMC methods have a continuing role to play in this area. We offer insights into the relative advantages of each approach, and we discuss diagnostics in the IS framework.
> :OMMENTS (21)

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：AI辅助研究泛滥导致同行评审系统压力剧增，代理评审系统（agentic review systems）作为潜在解决方案出现，但缺乏系统化的评估方法。
2. **现有评估的不足**：以往研究多聚焦于LLM辅助评审的某个方面（如方面级总结、直接反馈），未能全面衡量整个代理系统在真实科研论文上的表现。
3. **评价指标缺失**：如何量化AI评审与论文真实质量（而非表面特征）的关系？如何衡量系统发现论文错误的能力？以及用户对AI评审的实际体验如何？这三个维度尚无统一基准。
4. **系统性挑战**：评审系统涉及多步骤（检索、摘要、判断、生成评论），不同后端模型可能互补，但缺乏整体评估框架。

Q2: 有哪些相关研究？

1. **自动化评审系统**：前期工作包括用小型模型进行方面级总结（Yuan et al., 2022），以及GPT-4直接对全文提供反馈（Liang et al., 2024）。近期系统如OpenAIReview和Reviewer3等更加复杂，但评估局限于单一系统或小规模案例。
2. **LLM评估与基准**：现有基准如ChatGPT对论文摘要的评分（Liang et al., 2024）等，未全面覆盖评审质量的多维性。扰动基准在NLP中用于评估模型鲁棒性，但尚未应用于评审系统。
3. **人类评审与AI评审对比**：部分工作比较了AI与人类评审者的分歧，但未系统测量AI评审与论文外部质量信号的关系。
4. **用户研究**：虽然有一些关于学者对AI评审接受度的调查，但缺乏真实部署环境下的用户行为数据。
5. **本工作差异**：首次系统评估多个代理评审系统，结合质量代理、扰动基准和真实用户部署三个维度。

Q3: 论文如何解决这个问题？

1. **评估框架设计**：
 - 质量代理研究（Quality Proxy Study）：使用ICLR/NeurIPS论文（约XXXX篇），以引用数和接受决定作为论文质量近似信号。对每对论文，计算AI评审中哪个分配更正面，并与质量信号比较得到配对准确率。
 - 扰动基准（Perturbation Benchmark）：跨八个arXiv类别（cs.XX, stat.ML等）收集论文，人工构造四类错误（逻辑漏洞、方法缺陷、实验设计问题、写作错误），注入到论文中。测量系统检测注入错误的召回率。
 - 真实用户部署：将OpenAIReview部署在CHI 2026等评审平台上，收集用户对AI评论的投票和文字反馈。
2. **系统与模型**：三种代理系统（OpenAIReview、'coarse、Reviewer3）加零样本基线（直接提示LLM生成评审）。六种后端LLM：包括GPT-5.5、Claude Opus 4.7、开源模型等（具体列表待确认）。
3. **分析策略**：单独模型性能、系统组合、模型互补性分析（联合召回率）、用户反馈分类。
4. **边界条件**：质量代理研究限于计算机科学领域的顶级会议；扰动基准覆盖多个领域但错误类型人为定义；用户部署限于单系统单会议。

Q4: 论文做了哪些实验？

['1. **质量代理实验**：使用ICLR/NeurIPS论文的评审数据，比较AI评审对论文对的偏好与引用/接受决定的一致性。计算配对准确率。', '2. **扰动基准实验**：从八个arXiv类别选择论文，每篇注入一个错误（四选一）。运行各系统，记录每个错误是否被检测到。计算召回率。', '3. **模型互补性分析**：将六个模型检测结果结合（取并集），计算联合召回率。', '4. **真实用户部署**：在CHI 2026评审中集成OpenAIReview，用户可对评论投票（正面/负面）并提交文字反馈。统计投票比例和投诉类别。', '5. **消融与控制实验**：可能包括不同系统组件（检索、摘要、评审生成）的贡献分析（具体待确认）。']

Q5: 发现了什么实验现象？

1. **质量代理**：所有系统配对准确率高于随机（50%），OpenAIReview+GPT-5.5最优（83.0%）。这表明AI评审能捕捉到与人类判断相关的论文质量信号。
2. **扰动召回**：最佳单模型召回率71.6%，提示仍有近30%错误被遗漏。六个模型联合召回率83.3%，提升11.7个点，说明不同模型识别错误类型互补。
3. **模型互补性**：不同后端模型（如GPT-5.5与Claude Opus 4.7）在检测错误上存在差异，联合使用可显著提升覆盖率。
4. **用户反馈**：正面投票与负面投票比例1.44:1，用户整体偏向积极。最常见投诉是“假阳性”（标记非错误）和“微小挑剔”。没有专家真相对比，无法确认假阳性率。
5. **系统差异**：开源系统与专有系统性能差异显著，但最优组合均为基于GPT-5.5的OpenAIReview（推测）。
6. **反直觉**：尽管AI评审并非专门训练以匹配接受决定，仍与引用和接受信号相关；扰动检测中不同模型互补性远超预期。

Q6: 有什么可以进一步探索的点？

1. **评估扩展**：将质量代理扩展到更多领域（如生物医学、物理）和更多会议；增加论文质量的其他代理（如复现性）。
2. **扰动基准改进**：注入更多元化的错误类型（如伦理问题、引用不准确），并建立专家标注的黄金真值以同时测量精度。
3. **系统设计**：探索不同模型集成策略（如投票、级联），以及设计专门的评审检索和摘要模块。
4. **部署研究与用户信任**：长期跟踪真实使用效果，分析用户如何基于AI评论调整评审行为，以及如何减少假阳性。
5. **偏差分析**：研究AI评审是否对某些子领域、机构或写作风格存在系统性偏见。
6. **与其他评估维度结合**：考虑AI评审的时效性、可解释性、安全性等。
7. **跨学科联系**：将代理评审框架应用于其他专家判断任务（如基金评审、医学诊断）。

Q7: 总结一下论文的主要内容

本文针对新兴的代理评审系统提出了一套多维度评估框架，旨在衡量其与人类质量判断的一致性、错误检测能力以及真实用户接受度。研究背景是AI辅助研究导致同行评审不堪重负，代理评审系统成为缓解压力的潜在方案，但缺乏系统化的评估方法和基准。

论文设计了三个互补的实验：
1）质量代理研究：以ICLR/NeurIPS会议论文为对象，使用引用数和接受决定作为论文质量代理，评估AI评审对论文对的排序是否与这些代理一致。结果表明所有系统均优于随机基线，最佳系统（OpenAIReview+GPT-5.5）达到83.0%的配对准确率，说明AI评审能捕捉到部分与人类判断相关的质量信号。
2）扰动基准：为弥补代理信号不够精细的缺陷，论文构造了一个包含四类注入错误（逻辑错误、方法缺陷、实验设计问题、写作问题）的基准，覆盖八个arXiv学科类别。测量系统检测这些错误的召回率。最强单模型（OpenAIReview+GPT-5.5）召回率为71.6%，但六个不同后端模型的联合召回率达到83.3%，提升了11.7个百分点，显示模型间存在互补性：不同模型擅长检测不同类型的错误。这暗示通过集成设计可以显著提升性能。
3）真实用户部署：将OpenAIReview系统部署到CHI 2026会议评审流程中，收集用户对AI评论的投票和文字反馈。投票正面负面比例1.44:1（偏向正面），但常见投诉包括假阳性（系统误判正确的部分为错误）和提出细微的吹毛求疵意见。这些反馈揭示了当前系统在精度和相关性上的不足。

论文的主要贡献包括：
- 提出了系统评估代理评审系统的多层次方法，涵盖质量代理、扰动检测和真实部署。
- 建立了首个跨领域的扰动基准，四类错误和八个学科类别。
- 实证发现不同后端模型互补，联合召回率大幅提升。
- 提供了真实用户的正面和负面反馈，为系统改进指明方向。

局限性方面，作者坦诚指出：
- 扰动基准仅测量召回率，未评估精度；假阳性或预存错误无法区分。
- 错误注入可能偏向LLM易感知的类型（因为生成器和评估LLM可能有同源偏差）。
- 质量代理研究限于计算机科学顶级会议，引用和接受作为代理本身有噪声。
- 用户部署缺乏专家标注的真实验证，无法量化假阳性率。

总体而言，论文表明当前最先进的代理评审系统已具备一定能力：能够捕捉论文质量信号、检测相当比例的人造错误，并从多数用户获得正面评价。然而，召回率未饱和、假阳性问题突出，表明仍有相当大的改进空间。未来工作应扩展领域覆盖、完善错误类型、优化系统集成，并加强用户信任与偏差分析。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接与智能体（agentic）系统评估相关，尤其是AI for Science场景。

## 基本信息

- 作者：Dang Nguyen, Wanqing Hao, Yanai Elazar, Chenhao Tan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-18
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.19749v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（Abstract、Introduction、Limitations等片段），并结合heuristic_draft进行了改写与补全。
