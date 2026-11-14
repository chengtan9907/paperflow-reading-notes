---
user_id: "cheng tan"
paper_id: 21488
arxiv_id: "2611.14908v1"
title: "Hy ASR 3.0 Technical Report"
publish_date: "2026-11-11"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.14908v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.14908v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:43:05"
---
# Hy ASR 3.0 Technical Report

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm-based asr · contextual biasing · dialect recognition · code-switching

## 一句话总结

Hy ASR 3.0 是一个以 LLM 为解码核心的工业级语音识别系统，通过“音频编码器—音频适配器—LLM 解码器”架构、音频条件热词检索和多阶段后训练，在普通话、英语、粤语及 13 个汉语方言组上同时追求高识别精度、外部/句内上下文利用与多场景鲁棒性。

## 摘要

> 摘要清理与复述：论文提出 Hy ASR 3.0，一个基于大语言模型（LLM）的自动语音识别系统，目标是在保持强识别精度的同时支持灵活的上下文条件，并在多样语音条件下维持稳健识别。系统覆盖普通话、英语以及 13 个汉语方言组（包括粤语）。在作者评估的系统中，该系统在公开普通话、英语、粤语基准上取得最低聚合错误率，在对应内部测试集上也取得最低错误率，方言识别处于第一梯队。除纯音频转写外，Hy ASR 3.0 支持多种外部上下文，包括热词列表、问答对话和摘要，使任务相关信息可在识别过程中被利用；同时利用同一句内其他位置的上下文来消歧同音或近音关键词，无需额外上下文输入。评估覆盖外部上下文与句内上下文、长语音、儿童与老年人语音、低音量语音以及噪声环境语音，并报告识别精度、上下文利用和鲁棒性三类评测。需要注意：摘要中的对比表在 PDF 文本抽取中严重错位，具体数值需回原文表格核对；本报告按 RERR 公式对可对应数值做了谨慎反推，并在 experiments 字段标注不确定性。

Q1: 这篇论文试图解决什么问题？

一、论文明确要解决的三类问题
1. 多语言与多方言识别精度：Hy ASR 3.0 要同时覆盖普通话、英语、粤语和 13 个汉语方言组，并在公开与内部基准上取得有竞争力的聚合错误率。这不是单点语言优化，而是跨语言、跨方言、跨口音的系统性覆盖问题。
2. 异构上下文条件：系统要支持热词列表、问答对话、摘要等外部上下文，还要利用同一句内上下文消歧同音/近音关键词。不同上下文的形式、长度、可靠性和与音频的耦合方式差异很大，统一建模困难。
3. 复杂声学条件下的鲁棒性：目标包括长语音、儿童与老年人语音、低音量语音、噪声环境语音，以及不同说话风格和音频时长。论文在 Introduction 中把 speaker age groups、speaking styles 和 audio durations 列为明确目标。

二、为什么这些问题难
1. 方言与口音的长尾性：方言组内部差异大、标注规范不统一、公开数据规模远小于普通话；粤语和 code-switching 又涉及书面转写惯例。聚合 CER 很容易掩盖某一方言或口音类别的回退。
2. 上下文偏置的精确率—召回率权衡：热词/对话上下文可以提高目标词召回，但也可能诱发误触发、过度偏置或幻觉；上下文越长，候选空间越大，模型越容易抓错线索。
3. 句内消歧要求模型把同一 utterance 中远距离的语音/语义线索关联起来，涉及指代、重复、韵律和局部语境；在流式场景还会受到因果性和延迟约束。
4. 长语音存在分段、错误累积、标点/格式一致性、说话人切换等问题；儿童/老年人/低音量/噪声则是明显的分布偏移。
5. 评测本身难：公开基准与内部测试集分布不同，商业基线版本不可控，方言需要人评，上下文需要设计 target-keyword recall 等专门指标；单一聚合分数难以解释系统到底强在哪里。

三、论文的隐含假设与边界
1. 假设 LLM 的语言先验和上下文建模能力足以补足声学模型的不确定性；但 LLM 也可能“顺滑地”改写音频内容，需要 hallucination 与误触发指标约束。
2. 假设外部上下文与音频不冲突，或模型能自行解决冲突；论文可见片段未展示上下文冲突、错误上下文、超长上下文的失败分析。
3. 假设聚合指标可以代表系统能力；作者在结论中承认部分方言类别和带口音语音仍有缺口，说明聚合结果确实遮蔽了长尾失败。
4. 未覆盖：隐私、延迟、成本、流式能力、低资源方言、口音公平性、上下文误触发代价、独立复现。

四、问题定位判断：这是一个系统级三目标联合优化问题，不是单一算法创新；真正难点在数据配方、上下文注入接口、评测协议和失败模式控制，而不是一个孤立的网络模块。

Q2: 有哪些相关研究？

一、大规模弱监督/大数据 ASR：论文以 Whisper（Radford et al., 2023）作为大规模弱监督的代表，强调其通过海量数据提升识别精度与跨域泛化。这条脉络把 ASR 从精标小数据推向弱监督大模型，但也留下方言、口音、上下文条件化等长尾问题。
二、LLM-based ASR：Introduction 引用 Lakomkin et al. (2024)，指出 LLM 路线使转写能够利用预训练语言知识和文本上下文；Qwen3 ASR（Shi et al., 2026）则被描述为扩大识别覆盖范围并跨多样语音条件评测的系统。Hy ASR 3.0 明确站在这一脉络上，把 LLM 作为解码核心而非仅作后处理。
三、工业多语言/方言系统：实验对比对象包括 Qwen3 ASR Flash（Alibaba）、FunASR Flash（Alibaba Cloud）、Doubao ASR 2.0（Volcengine）、Seed 2.0 Lite（ByteDance）、StepAudio 3 ASR Max（StepFun）以及 Hy ASR 3.0 Preview。这些系统代表当前工业 ASR 在中文、英语、方言和上下文能力上的竞争面。
四、上下文感知识别：论文的相关工作可归纳为热词列表、问答对话、摘要三类外部上下文，以及利用同一句内上下文的同音/近音关键词消歧。前者偏向“给定候选词/对话历史/摘要，约束解码”，后者更接近“从当前语音内部寻找线索”，不需要额外上下文输入。论文把两者都作为核心能力，而不是附属功能。
五、鲁棒性与方言评测：公开基准覆盖 AISHELL-1、AISHELL-2、AliMeeting、WenetSpeech、FLEURS、Common Voice、SpeechIO、LibriSpeech、GigaSpeech、TED-LIUM 3、VoxPopuli、MLS、WenetSpeech-Yue、KeSpeech；鲁棒性维度覆盖长语音、儿童/老年人、低音量、噪声。这个评测组合反映当前工业 ASR 从“单语 CER”转向“多语言/方言+上下文+鲁棒性”的综合竞争。
六、与本文最接近的竞争面：Qwen3 ASR Flash、FunASR Flash、Doubao ASR 2.0、Seed 2.0 Lite、StepAudio 3 ASR Max 分别在普通话、英语、粤语、方言、code-switching 和上下文任务上构成主要对照。论文的差异化主张是同时覆盖多语言/方言、外部上下文、句内上下文和鲁棒性，并以技术报告形式给出聚合结果。
七、证据说明：论文的 Related Work 章节未在检索片段中完整出现，以上按 Introduction 的引用网络、实验基线和基准列表重构；未把实验结果混入本字段。

Q3: 论文如何解决这个问题？

一、总体架构
Hy ASR 3.0 由音频编码器、音频适配器、LLM 解码器和音频条件热词检索机制组成（证据：架构段落）。音频编码器提取声学表征，适配器将语音表征映射到 LLM 可消费的空间，LLM 解码器输出转写文本；热词检索机制以音频为条件选择/组织热词候选，供解码阶段使用。具体编码器类型、LLM 初始化、适配器层数、参数规模在可见片段中未给出。
二、训练配方
系统采用大规模预训练加多阶段后训练；后训练分两阶段：SFT 和 RL（证据：3.3 Post-training）。SFT 阶段用于提升识别性能（片段在 improve recognition performance on… 处截断）；RL 阶段的具体算法、奖励设计、数据配比未在可见证据中说明。合理推断：SFT 负责转写格式、任务对齐和基础识别，RL 负责进一步优化识别质量或上下文遵循；但这只是推断，不能当作论文已说明的机制。
三、上下文条件化
1. 外部上下文：支持热词列表、问答对话和摘要。合理推断其做法是把这些文本组织成上下文输入，与音频表征一起送入 LLM 解码器；热词检索机制可能用于缩小候选空间、降低长热词表的干扰。
2. 句内上下文：利用同一 utterance 其他部分的信息消歧同音/近音关键词，不需要单独的上下文输入。具体是注意力机制、提示拼接还是多遍解码，证据未说明。
3. 音频条件热词检索：这是架构中最接近“新模块”的部分；其价值在于让热词选择依赖当前音频，而不是仅依赖文本先验。但检索器的训练目标、召回率、误检索代价未见。
四、数据治理与评测
论文采用严格数据去污染，确保评测 utterance 及其音频/文本近重复不进入训练数据；评测覆盖公开基准与内部测试集，指标包括 CER/WER、target-keyword recall、人评方言百分比和 RERR。RERR 定义为 (Ecomp − EHy)/Ecomp，且强调在聚合值取整前计算。
五、设计逻辑与关键未知
从可见证据看，Hy ASR 3.0 的创新更像系统集成、训练配方与评测协议，而非单一新算法：LLM 解码器提供语言先验和上下文接口，适配器解决模态对齐，热词检索解决候选约束，多阶段后训练解决行为对齐。缺少消融，无法判断各组件贡献；也缺少延迟、吞吐、流式实现和上下文冲突处理的分析。

Q4: 论文做了哪些实验？

一、评测对象与基线
对比系统包括 Qwen3 ASR Flash（Alibaba）、FunASR Flash（Alibaba Cloud）、Doubao ASR 2.0（Volcengine）、Seed 2.0 Lite（ByteDance）、StepAudio 3 ASR Max（StepFun）和 Hy ASR 3.0 Preview。论文特别说明：这些技术报告/官方服务只用于标识模型家族或服务，下面的数值是本研究报告的结果，不是从那些参考中直接抄来的分数。
二、公开基准
覆盖 AISHELL-1、AISHELL-2、AliMeeting、WenetSpeech、FLEURS、Common Voice、SpeechIO、LibriSpeech、GigaSpeech、TED-LIUM 3、VoxPopuli、MLS、WenetSpeech-Yue、KeSpeech。论文声称执行严格数据去污染，排除评测 utterance 及其音频/文本近重复。
三、指标
CER/WER 用于客观识别；target-keyword recall 用于上下文识别；人评百分比用于方言评测；RERR 用于相对最强非 Hy 竞争者的错误率下降。RERR 在聚合值取整前计算，召回/人评差异以百分点表示。
四、主要聚合结果（按摘要错位表格与 RERR 公式反推，需回原文表核对）
• 公开普通话：Hy 4.64 vs FunASR Flash 4.77，RERR 2.7%；
• 内部普通话：Hy 3.18 vs Doubao ASR 2.0 3.78，RERR 15.9%；
• 公开英语：Hy 5.50 vs Doubao ASR 2.0 6.42，RERR 14.3%；
• 内部英语：Hy 5.22 vs StepAudio 3 ASR Max 5.38，RERR 3.0%；
• 公开粤语：Hy 4.81 vs Qwen3 ASR Flash 6.00，RERR 19.8%；
• 方言行：Hy 4.80 vs Qwen3 ASR Flash 5.45，RERR 11.9%；
• code-switching：Hy 4.20 vs FunASR Flash 5.30，RERR 20.8%。
这些数值是 CER/WER 百分数；RERR 是相对错误率下降。注意：粤语聚合不含 KeSpeech，而中文方言组合包含 Cantonese+KeSpeech，两个口径不可直接混用。
五、上下文评测
论文在 intra-utterance context 与 dialogue-context 两个测试集上评估（Table 3）。Hy ASR 3.0 的 target-keyword recall 分别为 61.88% 和 83.94%，均为所评估系统中最高；最强非 Hy 竞争者为 Seed 2.0 Lite（片段未给出其具体 recall 数值）。对话上下文使用前序对话轮次；句内上下文测试同音/近音关键词消歧。
六、鲁棒性评测
覆盖长语音、儿童与老年人组合语音、低音量与噪声环境组合语音。长语音四子集聚合 CER 为 5.06%，对比 Qwen3 ASR Flash 5.68%，约 11.0% 相对错误率下降（在聚合值取整前计算）；该四子集聚合与 combined 指标不同，不能混用。
七、人评与方言
内部人评覆盖这些方言类别，报告 strict dialect recognition、transcription formatting 和 lenient recognition。结论称 Hy ASR 3.0 在严格方言识别与转写格式上表现较强，但宽松识别与 FunASR Flash 大致相当。
八、实验设计的关键细节
公开与内部测试集并用；聚合指标按语言/方言组报告；上下文任务用 target-keyword recall 而非整体 CER；长语音用四子集聚合；人评区分 strict/lenient。这些设计提高了可读性，但也带来口径复杂、不可直接比较的问题。
九、证据缺口
可见片段没有逐方言、逐口音、逐子集的完整表格；没有训练数据规模、模型参数量、消融、延迟/吞吐、统计显著性、上下文误触发率、整体 CER 与 recall 的联合分析。

Q5: 发现了什么实验现象？

一、公开集与内部集的提升幅度差异很大
公开普通话仅 2.7% RERR，而内部普通话 15.9%；公开英语 14.3%，内部英语只有 3.0%。这说明“领先幅度”高度依赖对手强度和测试集分布，不能把某一行的 RERR 外推为全面优势。公开普通话的绝对 CER 已较低，压缩空间小；内部集上对手基线更弱或分布不同，导致相对提升更大。
二、语言间差异明显
粤语 19.8%、code-switching 20.8%、方言 11.9% 的提升高于公开普通话 2.7%。合理推断：这些场景的数据覆盖和竞争基线相对不饱和，系统更容易通过数据配方和上下文能力拉开差距；也可能是基线在粤语/代码转换上的版本或配置不占优。
三、上下文两类任务的难度差异很大
对话上下文 target-keyword recall 为 83.94%，句内上下文只有 61.88%。前序对话轮次提供了显式语义线索，模型更容易利用；同一句内消歧需要把远距离声学/语义线索关联起来，难度更高。这个差距也提示：外部上下文与句内上下文不是同一种能力，不能用一个“context score”概括。
四、长语音提升是聚合性的
长语音四子集聚合 5.06% vs 5.68%，约 11.0% RERR。聚合结果说明整体有优势，但没有展示四个子集各自的方向；可能存在某些子集提升、某些子集回退的抵消。
五、人评的 strict/lenient 张力
结论称严格方言识别和转写格式较强，但宽松识别与 FunASR Flash 大致相当。合理推断：部分优势可能来自规范化/格式/严格匹配，而不是纯声学判别；也可能 strict rubric 对格式更敏感。需要看人评 rubric 才能判断这是“识别更强”还是“输出更规整”。
六、聚合指标遮蔽长尾失败
作者在结论中承认部分方言类别和带口音语音仍有缺口。聚合 CER 最低不等于每个方言/口音类别都最低；这类系统报告最值得警惕的就是“平均值领先、长尾回退”。
七、指标设计的潜在盲区
上下文评估以 target-keyword recall 为主，没有同时报告整体 CER/WER、误触发率、错误上下文下的退化、上下文冲突处理。召回提高可能伴随过度偏置或幻觉；如果只看 target-keyword recall，会高估上下文能力的实际可用性。
八、反直觉/负结果线索
公开普通话提升很小，说明在成熟基准上继续压 CER 很难；内部英语提升只有 3.0%，说明内部集上对手并不弱。论文没有报告失败案例，但“剩余方言/口音缺口”本身就是一个负结果信号：系统仍未解决区域变异和声学变异下的一致性问题。

Q6: 有什么可以进一步探索的点？

一、方法可复现性
需要补充编码器类型、LLM 初始化与参数量、适配器结构、热词检索器训练目标、SFT 数据配比、RL 算法与奖励、推理时上下文格式。没有这些，外部团队无法判断收益来自架构、数据还是评测配置。
二、上下文精确率—召回率权衡
建议系统报告 target-keyword recall 的同时报告整体 CER/WER、误触发率、错误上下文/冲突上下文下的退化、上下文长度 scaling、检索 top-k 消融。进一步可研究上下文置信度估计、冲突检测、可归因上下文和“何时忽略上下文”的策略。
三、句内上下文机制
需要分析模型到底利用了哪些句内线索：同音词重复、语义指代、韵律、局部 n-gram 还是注意力路由。可探索流式因果版本、长句跨段消歧、对话内跨轮指代与句内消歧的统一建模。
四、方言与口音长尾
按方言组、口音、年龄、性别、录音条件做逐类错误分析；研究方言感知适配器、数据增强、伪标签、课程学习、低资源方言迁移。粤语与 code-switching 的转写规范也值得单独研究。
五、鲁棒性扩展
除长语音、儿童/老年人、低音量/噪声外，还应覆盖重叠语音、远场、混响、情感语音、快速/慢速语音、麦克风差异。长语音需要与分段、标点、说话人日志、流式延迟联合评估。
六、训练与推理效率
报告模型规模、训练算力、推理延迟、吞吐、显存/成本；研究热词检索的缓存、增量更新、端侧部署、隐私保护。对工业系统而言，这些约束与 CER 同等重要。
七、评测协议
建议公开内部测试集构建方式、方言类别定义、人评 rubric、统计显著性、置信区间、逐子集结果；引入独立第三方复现。商业基线应固定版本/调用日期，避免服务更新导致不可比。
八、公平性与部署
方言/口音公平性、儿童与老年人语音的隐私保护、错误转写的下游风险、上下文注入的数据泄露，都是系统落地前需要补的评估维度。

Q7: 总结一下论文的主要内容

一、论文定位与问题
Hy ASR 3.0 是一篇系统型技术报告，目标不是提出单一新算法，而是构建一个基于 LLM 的 ASR 系统，同时解决三件事：跨普通话/英语/粤语/13 个汉语方言组的高精度识别；对热词列表、问答对话、摘要等异构上下文的灵活利用；在长语音、儿童/老年人、低音量、噪声等复杂条件下的鲁棒性。论文在 Introduction 中把这三件事明确列为实践目标，并在结论中再次用“识别精度、上下文利用、鲁棒性”三条主线总结。
二、系统架构
Hy ASR 3.0 由音频编码器、音频适配器、LLM 解码器和音频条件热词检索机制组成。音频编码器提取声学表征，适配器把语音表征映射到 LLM 可消费的空间，LLM 解码器生成转写；热词检索机制以音频为条件选择/组织热词候选，供解码阶段使用。论文还提到大规模预训练与多阶段后训练，后训练分 SFT 和 RL 两阶段；SFT 用于提升识别性能，RL 细节在可见片段中未展开。
三、上下文能力
系统支持三类外部上下文：热词列表、问答对话、摘要。除此之外，它还能利用同一句内上下文消歧同音/近音关键词，不需要额外上下文输入。这个“句内上下文”能力是它与普通热词偏置系统的一个区别：线索来自当前语音本身，而不是外部给定文本。论文在 Table 3 上分别评估 intra-utterance context 和 dialogue-context，用 target-keyword recall 衡量。
四、实验设计
公开基准覆盖 AISHELL-1、AISHELL-2、AliMeeting、WenetSpeech、FLEURS、Common Voice、SpeechIO、LibriSpeech、GigaSpeech、TED-LIUM 3、VoxPopuli、MLS、WenetSpeech-Yue、KeSpeech；对比系统包括 Qwen3 ASR Flash、FunASR Flash、Doubao ASR 2.0、Seed 2.0 Lite、StepAudio 3 ASR Max 和 Hy ASR 3.0 Preview。指标包括 CER/WER、target-keyword recall、人评方言百分比和 RERR。论文强调严格数据去污染，且基线数值是本研究报告的结果，不是从厂商参考中直接抄录。
五、关键结果
按摘要错位表格与 RERR 公式反推：公开普通话 Hy 4.64 vs FunASR Flash 4.77，RERR 2.7%；内部普通话 Hy 3.18 vs Doubao 3.78，RERR 15.9%；公开英语 Hy 5.50 vs Doubao 6.42，RERR 14.3%；内部英语 Hy 5.22 vs StepAudio 5.38，RERR 3.0%；公开粤语 Hy 4.81 vs Qwen3 6.00，RERR 19.8%；方言行 Hy 4.80 vs Qwen3 5.45，RERR 11.9%；code-switching Hy 4.20 vs FunASR 5.30，RERR 20.8%。这些数值需回原文表核对，但按 RERR 公式能自洽对应。上下文 target-keyword recall 为 61.88%（句内）和 83.94%（对话），均为所评估最高；长语音四子集聚合 5.06% vs Qwen3 5.68%，约 11.0% RERR。
六、实验现象
最值得注意的现象是“提升幅度高度依赖测试集”：公开普通话只有 2.7% RERR，内部普通话却达 15.9%；公开英语 14.3%，内部英语只有 3.0%。这说明不能把单行 RERR 当作普遍优势。第二，对话上下文 recall（83.94%）远高于句内上下文（61.88%），说明显式前序对话线索比同一句内远距离消歧更容易。第三，人评显示严格方言识别与转写格式较强，但宽松识别与 FunASR Flash 大致相当，提示部分优势可能来自格式/规范化，而非纯声学识别。第四，作者承认部分方言类别和带口音语音仍有缺口，聚合领先不等于长尾无回退。
七、作者结论与边界
作者结论是：Hy ASR 3.0 在普通话、英语、粤语上取得所评估系统中的领先聚合识别性能，方言识别处于第一梯队；外部对话上下文、句内消歧、长语音、儿童+老年人、低音量+噪声等评测表现较强。剩余缺口集中在特定方言类别和带口音语音，未来需要更一致的区域/声学变异识别，以及更细粒度的上下文评估。论文没有在可见片段中给出模型规模、训练数据量、RL 细节、消融、延迟/成本、误触发率和独立复现，因此更适合作为工业系统与评测协议的参考，而不是可直接复现的方法论文。
八、总体判断
这篇报告的价值在于把“多语言/方言覆盖 + 异构上下文 + 鲁棒性”放在同一个系统里做联合评测，并给出了公开/内部、客观/人评、聚合/子集的组合视角。它的短板是系统报告常见的信息缺口：关键训练细节不透明、基线不可控、聚合指标遮蔽长尾、上下文评估只看召回不看代价。阅读时应重点核对表格口径、上下文指标定义和人评 rubric，而不是只记住“最低 CER”这一句。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对语音智能体/对话式 agent：外部热词、问答对话、摘要上下文和句内消歧可直接映射到 agent 的实体记忆、对话状态和纠错机制。

## 基本信息

- 作者：Aoxiong Yin, Auden Li, Chao Huang, Chuanyang Hu, Eitan Wang, Ethan Zou, Fangming Feng, Guangyu Chen, Guinan Li, He Wang, Jiahong Li, Meng Yu, Mingyu Cui, Peng Wang, Rongjie Li, Steve Yves, Teddy Sun, Tian Zheng, Wei Liu, Yang Zhou, Yixuan Zhang, Zhennan Lin, Zhihang Xu, Zhuohao Li, Ziqiang Zhang, Anji Zhang, Bang Wang, Caisheng Ouyang, Chengqi Zhu, Chong Li, Conglian Xie, Dongdong Wang, Edward Wang, Evelynn Liu, Ferman Yang, Feng Lu, Gang Zhang, Jathon Zhang, Jaxl Liu, Jiachen Luo, Jian Li, Jingxi Gui, Kysun Wang, Liang Dong, Longsha Zhou, Na Hu, Qi Chen, Rilin Chen, Rui Yuan, Shanshan Sang, Siyu Yan, Steven Hong, Taihui Wang, Weiwei Li, Xiangyu Zhang, Xiaotong Yang, Xue Hu, Xue Jiang, Yameng Li, Yifu Sun, Yuhuan Zhou, Zhiang Chen, Zhibin Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, eess.AS
- 日期：2026-11-11
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.14908v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中的 Introduction、Results、Conclusion 片段，并对摘要中错位的对比表按 RERR 公式做了谨慎反推；方法细节与部分数值仍需回原文核对。
