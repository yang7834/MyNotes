---
title: "Token、Embedding 与上下文窗口"
author:
  - "[[Hello-AI Team]]"
source: "https://unclecheng-li.github.io/Hello-AI/basics/token-embedding-context/"
domain: "github.io"
created: 2026-08-31
description: "面向中文学习者的 AI / LLM 学习入口平台"
---
[原文链接](https://unclecheng-li.github.io/Hello-AI/basics/token-embedding-context/)

> 来源：[AI 基础 · 第 7 站：Token、Embedding 和上下文窗口](https://unclecheng-li.github.io/Hello-AI/basics/token-embedding-context/)｜作者：Hello-AI｜站点：Hello-AI｜日期：未知

## 目录

- 1. 这章解决什么问题
- 2. Token：模型处理文本的颗粒
- 3. Embedding：把语义放进向量空间
- 4. 上下文窗口：模型一次能看的工作台
- 5. 三者关系：从文本到回答
- 6. 最小示例：做一个公司知识库问答
- 7. 使用时的经验规则
- 8. 常见误区
- 9. 使用和开发时的安全边界
- 10. 练习题 / 小实验
- 11. 下一步

## 1. 这章解决什么问题

将文本发送给模型后，模型实际处理流程包括：将文字切分为 token，再把 token 变成向量，接着在上下文窗口中与历史对话、系统提示词、工具结果一起计算，答案也按 token 逐个生成。

新手常见问题：

- 为什么一个中文提示词比想象中更费 token？
- 为什么模型能找出「退款」和「return policy」的关系？
- 为什么知识库问答要把文档切块再建向量索引？
- 为什么 100 万 token 上下文听起来很大，用起来还可能漏掉中间的信息？
- 为什么聊天记录越来越长，模型会变慢、变贵，甚至开始跑偏？

答案基本都藏在三个词里：**Token、Embedding、上下文窗口**。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/token-embedding-context_mermaid_0_fc5c7326.svg)

## 2. Token：模型处理文本的颗粒

**Token（词元）** 是模型处理文本时使用的「文本颗粒」。它可能是一个字、一个词、一个词根、一个标点，也可能是一段字节。OpenAI Cookbook 介绍 `tiktoken` 时指出，GPT 模型看到的是 token 形式的文本，计算 token 数有助于判断文本是否超长以及估算 API 成本。官方示例中，`tiktoken is great!` 会被切成类似 `t`、`ik`、`token`、`is`、`great`、`!` 的片段。 [OpenAI Cookbook](https://developers.openai.com/cookbook/examples/how_to_count_tokens_with_tiktoken)

可以把 tokenizer 想成切菜刀：同一句话进不同模型，token 数可能差异很大。

### 子词切分为什么会出现

早期 NLP 系统按词处理文本，但开放词表无法覆盖所有新词、人名、代码变量。2016 年 ACL 论文 [Neural Machine Translation of Rare Words with Subword Units](https://arxiv.org/abs/1508.07909) 提出用 subword units 解决罕见词和未知词问题。例如 `unbelievable` 可拆成 `un`、`bel`、`ievable`。后来 [SentencePiece](https://arxiv.org/abs/1808.06226) 可直接从原始句子训练 subword 模型，对中文、日文等没有空格的语言更友好。

**记法：** Token 是模型词表和 tokenizer 共同切出来的工程单位，不是自然语言里的「字」或「词」。

### 同一句话，tokenizer 会切出不同结果

`tiktoken` 是 OpenAI 的快速 BPE tokenizer，支持 `o200k_base`、`cl100k_base` 等编码，可通过 `encoding_for_model('gpt-4o')` 选择。 [tiktoken GitHub](https://github.com/openai/tiktoken)

示例：

| 文本 | encoding | token 数 |
| --- | --- | --- |
| `antidisestablishmentarianism` | `r50k_base` / `p50k_base` | 5 |
| `antidisestablishmentarianism` | `cl100k_base` / `o200k_base` | 6 |
| `お誕生日おめでとう` | `r50k_base` / `p50k_base` | 14 |
| `お誕生日おめでとう` | `cl100k_base` | 9 |
| `お誕生日おめでとう` | `o200k_base` | 8 |

新 tokenizer 对非英文文本可能更省 token，中文也有类似情况。

### Token 会影响三件事

1. **价格**：API 计费通常看输入和输出 token 数。
2. **长度**：上下文窗口按 token 算，不按字数算。
3. **模型看到的形状**：专业词、变量名、代码、URL 等常被切得很碎，影响模型处理和 token 消耗。

写 Prompt 时，应关注信息必要性、结构清晰度和 token 浪费。

## 3. Embedding：把语义放进向量空间

**Embedding（嵌入 / 向量表示）** 将文本、图片、音频等对象变成一串数字，使语义相近的内容在向量空间里距离更近。OpenAI Embeddings 文档说明 text embeddings 用于衡量文本字符串相关性，用途包括搜索、聚类、推荐、异常检测、分类等。 [OpenAI Embeddings 文档](https://developers.openai.com/api/docs/guides/embeddings)

如果说 token 是「切块」，embedding 就是「坐标」。

### 从 word2vec 到句向量

2013 年 [word2vec 论文](https://arxiv.org/abs/1301.3781) 提出从大规模文本中学习单词的连续向量表示，可在不到一天内从 16 亿词数据中训练出高质量词向量。经典示例：$king - man + woman \\approx queen$。词语关系会在向量空间中表现为方向。

[Sentence-BERT](https://arxiv.org/abs/1908.10084) 通过 Siamese / Triplet 网络生成句子 embedding，用 cosine similarity 比较相似度，将 10,000 句找最相似句的时间从约 65 小时降到约 5 秒。

### Embedding 能做什么

| 场景 | 怎么用 embedding |
| --- | --- |
| 语义搜索 | 把问题和文档转成向量，找距离最近的文档 |
| 聚类 | 把相似评论、工单、用户反馈自动归类 |
| 推荐 | 找和用户已读内容相近的文章或商品 |
| 去重 | 找语义重复但文字不同的内容 |
| 分类 | 用文本向量和标签向量的相似度判断类别 |
| RAG | 先检索相关片段，再交给大模型回答 |

RAG 基础论文 [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401) 将 Wikipedia 文档编码为 dense vector index，用户问题也编码为向量进行检索。典型套路是「先找，再答」。

### 向量库为什么能搜得快

- [HNSW 论文](https://arxiv.org/abs/1603.09320) 利用分层可导航小世界图，高层快速跳转、低层精细搜索。
- [FAISS 相关论文 Billion-scale similarity search with GPUs](https://arxiv.org/abs/1702.08734) 展示了 GPU 上十亿级向量搜索，比此前方法快 8.5 倍。

向量数据库能降低延迟、扩大规模、控制成本。

## 4. 上下文窗口：模型一次能看的工作台

**上下文窗口（Context Window）** 是模型生成答案时可参考的 token 总量，包括当前输入、历史对话和模型将要生成的响应。Anthropic Claude 文档明确输入和输出共享窗口。 [Anthropic Context Windows 文档](https://platform.claude.com/docs/en/build-with-claude/context-windows)

```text
上下文窗口 ≈ 系统提示词 + 历史对话 + 当前输入 + 工具结果 + 本轮输出
```

窗口满时，旧内容会被截断、压缩、清理，或请求失败。

### Transformer 为什么需要上下文窗口

[Attention Is All You Need](https://arxiv.org/abs/1706.03762) 提出完全基于 attention 的 Transformer，更易并行化，但注意力计算随窗口长度增长。需要位置编码来区分顺序，如 [RoPE](https://arxiv.org/abs/2104.09864) 和 [ALiBi](https://arxiv.org/abs/2108.12409)。这些技术决定了模型能处理多长材料。

### 长上下文很强，也有脾气

Google Gemini 1.5 Pro 标准上下文为 128K tokens，私有预览中开放 1M tokens，可处理约 1 小时视频、11 小时音频、超过 30,000 行代码或超过 700,000 个单词。 [Google Gemini 1.5 Pro 发布](https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/)

[Lost in the Middle](https://arxiv.org/abs/2307.03172) 发现信息位于长上下文中间时，模型性能明显下降。因此要克制：先筛选、再压缩、再排序，把关键信息放在开头或结尾。

## 5. 三者关系：从文本到回答

Token、Embedding、上下文窗口对应模型处理信息的三个层面。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/token-embedding-context_mermaid_1_b85aa282.svg)

| 概念 | 回答的问题 | 影响什么 |
| --- | --- | --- |
| Token | 模型把文本切成什么颗粒 | 价格、长度、切分效果 |
| Embedding | 文本怎样变成可计算的语义 | 搜索、聚类、推荐、RAG |
| 上下文窗口 | 模型一次能参考多少 token | 记忆、长文档、速度、成本 |

```text
想省钱，看 token。 想做搜索，看 embedding。 想处理长材料，看上下文窗口。
```

## 6. 最小示例：做一个公司知识库问答

假设有 500 份产品文档，流程如下：

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/token-embedding-context_mermaid_2_5a4ef74a.svg)

- 文档切分 chunk，每个 chunk 消耗 token；
- chunk 和问题计算 embedding 用于语义检索；
- 检索结果塞进上下文窗口供模型生成。

如果 chunk 太大，检索不准且浪费上下文；太小则语义被切碎；检索结果太多会填满窗口。RAG 工程细节很多。

## 7. 使用时的经验规则

1. **别用字数估 token**：用目标模型 tokenizer 计算。OpenAI 模型可用 `tiktoken`。
2. **长文档先切块**：按标题、段落、列表切，保留少量 overlap 和元数据。

| 维度 | 建议 |
| --- | --- |
| 章节边界 | 尽量按标题、段落、列表切，不要硬切断一句话 |
| chunk 大小 | 从几百到一两千 token 试起 |
| overlap | 相邻 chunk 保留少量重叠 |
| 元数据 | 保存标题、来源文件、页码、更新时间 |

3. **重要信息放前面或结尾复述**：关键规则、约束、输出格式可放在开头，并在结尾复述。
4. **压缩历史对话**：将老历史摘要为「当前任务状态」「已确认约束」「待解决问题」。
5. **Embedding 检索要保留来源**：返回引用来源（文件名、章节、页码、URL）以便核查。

## 8. 常见误区

误区 1：1 个汉字等于 1 个 token。
→ 不固定，不同 tokenizer 切法不同。

误区 2：Embedding 就是普通编号。
→ Embedding 编码语义关系，使近义表达向量距离更近。

误区 3：向量检索能保证答案正确。
→ 只能找相似内容，不能保证正确，受文档质量、chunk 切分、Top-K 等影响。

误区 4：上下文窗口越大，效果一定越好。
→ 大窗口更慢更贵，中间信息利用不稳，需配合筛选和排序。

误区 5：模型会永久记住当前对话。
→ 模型只参考当前上下文窗口，无持久记忆。

误区 6：把资料丢给模型就等于训练了模型。
→ 知识库问答只是推理时检索，模型参数未更新。

## 9. 使用和开发时的安全边界

1. 不要将敏感文档直接拿去试不明工具，看清服务条款。
2. Embedding 可能泄露语义特征，高敏数据要访问控制和隔离存储。
3. 知识库要保留权限，向量库做权限过滤。
4. 长上下文不要混入无关资料。
5. 高风险场景（法务、财务、医疗等）必须给出答案来源。

## 10. 练习题 / 小实验

练习 1：观察 token 切分

输入：

```text
你好世界 unbelievable 深度学习 user_id=12345&debug=true
```

观察中文、英文、代码参数分别切成多少 token。

练习 2：判断该用关键词搜索还是向量搜索

- 查找所有包含「退款」两个字的客服记录 → 关键词搜索
- 查找和「用户想取消订单并拿回钱」意思相近的客服记录 → 向量搜索

练习 3：设计一个小型 RAG 流程

100 页产品说明书，最小流程：按章节切块 → 每块计算 embedding → 存入向量库并保留标题和页码 → 用户问题计算 embedding → 检索 Top-K 片段 → 将片段和问题放入上下文 → 要求模型只基于片段回答并给出处。

练习 4：处理超长材料

20 万字手册装不进窗口：按章节切分、做摘要和索引，针对问题检索，必要时分批分析再合并结论。

## 11. 下一步

继续学习 [Prompt、上下文和记忆](https://unclecheng-li.github.io/Hello-AI/basics/prompt-context-memory/)。

## AI总结

本文系统讲解了 Token、Embedding 和上下文窗口三个核心概念。Token 是模型处理文本的颗粒，直接影响价格、长度和切分效果；Embedding 将文本映射为语义向量，支撑搜索、聚类和 RAG；上下文窗口决定模型一次能参考多少信息，有长上下文时需注意“中间丢失”问题。三者相互配合，理解它们有助于优化 Prompt、降低成本和构建可靠的知识库问答系统。