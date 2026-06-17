---
user_id: "cheng tan"
paper_id: 436
arxiv_id: "2606.16748v1"
title: "MyPCBench: A Benchmark for Personally Intelligent Computer-Use Agents"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16748v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16748v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:08:03"
---
# MyPCBench: A Benchmark for Personally Intelligent Computer-Use Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer-use agent · benchmark · personalization · desktop automation

## 一句话总结

Introduces MyPCBench, a benchmark for personally intelligent computer-use agents with a personalized Linux desktop environment and 184 tasks based on real user requests.

## 摘要

> Current benchmarks for computer-use agents evaluate models in impersonal environments. This leaves a gap between evaluation and deployment where personal assistants are expected to work across a user's whole digital life, including their context, historical data, and logged-in accounts. This gap is widest on web tasks, where live web evaluations cannot exercise sites that require logging in or personal information, the kind of site a real personal assistant has to drive. We introduce MyPCBench, which tests computer-use agents as personal assistants on a Linux desktop populated with 17 simulated real-world web applications and a full desktop stack, all seeded for one canonical persona, Michael Scott from The Office. We define 184 tasks in this environment, each inspired by a real request drawn from the OpenClaw community, and benchmark six closed and open-weight models with a uniform computer+bash tool surface. We find that the best model, Claude Opus 4.6, fully solves 55.4\% of the tasks, the only model above 50\%. Model failures cluster on tasks that span many applications and on long trajectories, where personalization stresses an assistant the most. We release the environment, task set, and agent harness at https://mypcbench.com.

Q1: 这篇论文试图解决什么问题？

Existing computer-use agent benchmarks evaluate models in impersonal, empty environments without user data or context. This creates a gap between evaluation and deployment, as personal assistants must operate across a user's digital life with historical data, logged-in accounts, and personal preferences. The gap is most evident on web tasks requiring login or personal information, which live web evaluations cannot exercise. There is no benchmark that tests agents as truly personal assistants.

Q2: 有哪些相关研究？

Prior agent benchmarks include web-focused ones (WebArena, Mind2Web, etc.), full desktop environments (OSWorld, etc.), enterprise platforms (WorkArena), and mobile benchmarks. Generative Agents and GAIA probe general assistant capabilities but lack personalized context. All these benchmarks are simulated for reproducibility but sacrifice personalization: each application carries only task-necessary data with no user history. MyPCBench is the first to explicitly incorporate a persistent persona and history across applications.

Q3: 论文如何解决这个问题？

MyPCBench provides a reproducible Linux desktop environment via Docker running a QEMU/KVM Ubuntu 24.04 VM with GNOME Shell. The VM hosts 17 pre-logged-in websites modeled on real services, all seeded with data for a fixed persona (Michael Scott). 184 tasks are defined, each inspired by real user requests from the OpenClaw Discord community. Tasks span 6 categories (Computers/Tech, Finance, Travel, Food, Ecommerce, Gambling) and 1 to 19 applications. Agents interact via a uniform computer (screenshot-based GUI actions) and bash tool surface. Evaluation uses LLM-as-a-judge with rubric-based grading.

Q4: 论文做了哪些实验？

The authors benchmark six models: closed-source (Claude Opus 4.6, GPT-4o, GPT-5) and open-weight (Qwen2.5-72B, Qwen2.5-35B, Qwen2.5-9B). Each model is given the same tool surface (computer+bash). Tasks are executed with a fixed budget of 30 steps per task. Success is determined by an LLM judge (likely using a rubric) checking task completion and side effects. They also analyze scaling trends with trajectory length and number of applications per task, and categorize failure modes.

Q5: 发现了什么实验现象？

Claude Opus 4.6 achieves the highest success rate at 55.4%, the only model above 50%. Failures cluster on tasks that span many applications and long trajectories. Three family-shaped failure patterns emerge: Claude shortcuts through bash instead of UI, GPT family premature 'DONE' before rubric-graded side effects, and Qwen models hallucinate persona values (35B) or collapse under dual-tool schema (9B). Cross-app and long-horizon slopes are steep for all models. Hardest categories: aggregation & reporting, multi-step orchestration, pattern inference.

Q6: 有什么可以进一步探索的点？

Future work can extend MyPCBench to more personas and real-world applications, improve models' ability to balance coding (bash) with computer-use actions, and address failure patterns. The benchmark can serve as a baseline for personal computer-use agent research, pushing towards personalized evaluation. More robust rubric grading and evaluation of side-effect awareness are also open.

Q7: 总结一下论文的主要内容

The paper identifies a gap in existing computer-use agent benchmarks: lack of personalization. To address this, the authors introduce MyPCBench, a benchmark comprising a reproducible Linux desktop environment with 17 simulated web applications, all seeded with data for a fixed persona (Michael Scott). 184 tasks are sourced from real user requests. They evaluate six models using a uniform computer+bash tool surface. Claude Opus 4.6 achieves the highest success rate (55.4%), with failures concentrated on cross-application and long-horizon tasks. Three family-shaped failure patterns are identified. The benchmark and associated resources are released to encourage research on personalized computer-use agents.

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：Directly relevant to AI agent research, especially computer-use agents and personal assistants.

## 基本信息

- 作者：Lawrence Keunho Jang, Andrew Keunwoo Jang, Jing Yu Koh, Ruslan Salakhutdinov
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16748v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 This response is generated primarily based on the paper abstract and retrieved evidence from the PDF (introduction, conclusion, and references). The heuristic draft was used as a guide but supplemented with evidence chunks.
