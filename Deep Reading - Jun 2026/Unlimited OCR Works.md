---
user_id: "cheng tan"
paper_id: 1232
arxiv_id: "2606.23050"
title: "Unlimited OCR Works"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23050.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23050"
abs_url: "https://arxiv.org/abs/2606.23050"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:09:03"
---
# Unlimited OCR Works

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reference sliding window attention · kv cache · long context decoding · multimodal large language model

## 一句话总结

本文提出Unlimited OCR模型，通过新颖的Reference Sliding Window Attention (R-SWA)机制在解码过程中保持恒定KV缓存，实现一次性转录数十页文档，解决了长序列OCR解码中内存和速度随输出长度增长而恶化的问题。

## 摘要

> Recently, end-to-end OCR models, exemplified by DeepSeek OCR, have once again thrust OCR into the spotlight. A widely held view is that employing a large language model (LLM) as the decoder allows the model to leverage the prior distribution of language, leading to improved OCR performance. However, the downside is equally evident: as the output sequence lengthens, the accumulated KV cache drives up memory consumption and progressively slows down generation. This stands in stark contrast to humans, who exhibit no such decline in efficiency during long-horizon copying tasks. In this technical report, we propose Unlimited OCR, a model designed to emulate human parsing working memory. Taking DeepSeek OCR as the baseline, we replace all attention layers in the decoder with our proposed Reference Sliding Window Attention (R-SWA), which reduces attention computation costs while maintaining a constant KV cache throughout the entire decoding process. By combining the high compression rate of DeepSeek OCR's encoder with our constant KV cache design, Unlimited OCR can transcribe dozens of pages of documents in a single forward pass under a standard maximum length of 32K. More importantly, R-SWA is a general-purpose parsing attention mechanism — beyond OCR, it is equally applicable to tasks such as ASR, translation, etc. Codes and model weights are publicly available at http://github.com/baidu/Unlimited-OCR.
> ![](images/ef0d8e9a22bb637934af3049b6b94291b2485a080c572f2769700c2da973a996.jpg)
> Figure 1 | Illustration of Reference Sliding Window Attention (R-SWA). Each generated token attends to all reference tokens (visual tokens in OCR) and the preceding n output tokens (128 by default). Compared to standard full attention, R-SWA maintains a constant KV cache throughout decoding. Compared to vanilla SWA, it preserves visual token fidelity by excluding them from state transitions, thereby avoiding progressive blurring.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决端到端光学字符识别（OCR）模型中一个关键但未充分研究的效率瓶颈：当使用大型语言模型（LLM）作为解码器时，随着输出序列增长，累积的KV（键值）缓存导致内存消耗线性上升和生成速度逐渐下降。这种退化与人类在长时间复制任务中的稳定表现形成鲜明对比。虽然已有工作通过压缩视觉token或使用滑动窗口注意力来缓解，但标准滑动窗口注意力（SWA）在每次token生成时仅考虑固定数量的最近token，会丢失对早期视觉token的全局访问，导致渐进式模糊。因此，需要一种既能保持对参考视觉信息的持续关注，又能避免KV缓存无限增长的注意力机制，以实现真正不限长度的OCR解析。

Q2: 有哪些相关研究？

1. **端到端OCR模型**：近期以DeepSeek OCR为代表的方法将视觉编码器与LLM解码器结合，利用语言先验提升准确率，但长序列解码效率低。2. **长序列语言模型**：Transformer的二次注意力复杂度催生了各种高效注意力变体，如稀疏注意力、局部窗口注意力、线性注意力等。3. **滑动窗口注意力（SWA）**：常用在长文档生成中，只关注最近n个token，但无法直接用于多模态任务，因为视觉信息会随窗口滑动丢失。4. **KV缓存优化**：包括缓存复用、压缩、分页等技术，但大多面向固定长度或预填充场景。5. **工作记忆模型**：心理学中工作记忆的概念被引入，本文直接类比人类在长时间解析任务中保持视觉信息的能力。6. **多模态大模型（MLLM）**：如LLaVA、Qwen-VL等，通常使用全注意力和长上下文扩展，但未专门针对解码效率优化。

Q3: 论文如何解决这个问题？

本文提出Reference Sliding Window Attention (R-SWA)作为核心创新。R-SWA将注意力计算分为两部分：每个生成token始终关注所有参考token（即视觉token和提示token），同时只关注输出序列中前n个token（默认n=128）。这样，视觉信息通过参考路径保持全局可见，不会因窗口滑动而丢失；而输出部分的滑动窗口使得KV缓存大小在解码过程中恒定不变，复杂度从O(T²)降为O(T)。R-SWA可无缝替换标准解码器中的全注意力层。Unlimited OCR模型基于DeepSeek OCR基线，采用DeepEncoder（含Mixture-of-Experts架构，3B总参数量、500M激活参数）编码视觉token，解码器全部替换为R-SWA。在训练时，模型使用因果参考滑动窗口掩码，每个token的注意力定义为：Attention(Q, [K_ref; K_out_swa])，其中K_ref包含所有参考token，K_out_swa包含前n个输出token。推理时KV缓存仅保留n个输出token，参考token的KV可直接缓存不更新。

Q4: 论文做了哪些实验？

论文实验部分验证了R-SWA在OCR任务中的有效性，但具体实验设置和数据集未在检索片段中详细披露。从证据推断，实验包括：1）在DeepSeek OCR基线基础上，用R-SWA替换全注意力，在相同训练设置下比较性能；2）在长文档上测试一次性转录能力（标准32K长度，可转录数十页）；3）可能包括消融研究比较不同窗口大小n和不同压缩率的影响。证据提到R-SWA相比DeepSeek OCR基线提升6%，但未明确是何种指标（推测是识别准确率或字符错误率）。此外，论文进行了初步的MLLM架构验证，证明线性复杂度注意力在OCR长段任务中可行。

Q5: 发现了什么实验现象？

1. **性能提升**：R-SWA在OCR任务上相比DeepSeek OCR基线提升了6%（合理推断为准确率或降低错误率）。2. **记忆效率**：R-SWA在解码过程中保持恒定KV缓存大小，使得内存消耗不随输出增长而增加，从而能够在32K上下文中一次性处理数十页文档。3. **长序列稳定性**：与标准全注意力相比，R-SWA避免了长输出序列末尾的速度退化，生成速度几乎不变（推测：因为缓存恒定）。4. **视觉保真度**：通过保留对所有参考token的完整注意力，避免了标准SWA中因窗口滑动导致的视觉信息逐渐模糊的问题（本文声称）。5. **泛化潜力**：R-SWA作为通用解析注意力，除OCR外还可应用于ASR、翻译等任务，但论文仅以OCR为验证场景。

Q6: 有什么可以进一步探索的点？

1. **扩展到更多解析任务**：将R-SWA应用于ASR、机器翻译、语音合成等序列到序列任务，验证其通用性。2. **优化窗口大小自适应**：当前默认窗口n=128，可探索根据任务或序列长度动态调整窗口大小的策略。3. **结合其他缓存优化**：R-SWA的恒定KV缓存可与量化、剪枝、内存复用等技术结合，进一步降低部署成本。4. **与长上下文MLLM融合**：研究R-SWA在多轮对话、视频理解等超长上下文场景中的表现。5. **理论分析**：从信息流角度分析R-SWA如何保持参考token的长期依赖，以及滑动窗口对输出精度的影响边界。6. **训练策略改进**：R-SWA带来的注意力偏置可能影响训练稳定性，探索更优的初始化或正则化方法。7. **更大规模验证**：在更高分辨率、更多语种、更复杂版面文档上测试Unlimited OCR的鲁棒性。

Q7: 总结一下论文的主要内容

本文提出Unlimited OCR，一个专为长序列解析设计的端到端OCR模型，核心创新是Reference Sliding Window Attention (R-SWA)。R-SWA允许每个生成token同时关注所有参考token（视觉token和提示）以及固定数量的最近输出token（默认128），从而在解码过程中维持恒定大小的KV缓存，解决了传统全注意力随输出增长的内存和速度问题。模型以DeepSeek OCR为基线，保留其DeepEncoder和MoE架构，仅替换解码器注意力层。实验表明，R-SWA在OCR任务上相比基线提升6%，并能一次性转录数十页文档（32K上下文）。R-SWA是通用注意力机制，可应用于ASR、翻译等其他解析任务。论文公开了代码和模型权重。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文提出的R-SWA机制可作为高效注意力模块，适用于需要长序列解码的生成任务。

## 基本信息

- 作者：Youyang Yin, Huanhuan Liu, Qunyi Xie, Chaorun Liu, Shiqi Yang, Shaohua Wang, Zhanlong Liu, Hao Zou, Jinyue Chen, Shu Wei, Jingjing Wu, Mingxin Huang, Zhen Wu, Guibin Wang, Tengyu Du, Lei Jia
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23050`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 精读报告已参考PDF检索证据，主要基于Abstract、Introduction和Conclusion片段的语义嵌入结果生成，但论文实验细节和完整方法描述不足，部分内容根据摘要和引言推断。
