---
title: "什么是 AI"
author:
  - "[[Hello-AI Team]]"
source: "https://unclecheng-li.github.io/Hello-AI/basics/what-is-ai/"
domain: "github.io"
created: 2026-08-27
description: "面向中文学习者的 AI / LLM 学习入口平台"
---
[原文链接](https://unclecheng-li.github.io/Hello-AI/basics/what-is-ai/)

> 来源：[AI 基础 · 第 1 站](https://unclecheng-li.github.io/Hello-AI/basics/what-is-ai/)｜作者：Unknown｜站点：Hello-AI｜日期：Unknown

## 目录

- 1.什么是 AI？
- 2.AI 发展里程碑
- 3.AI 能做什么
- 4.AI 有很多条技术路线
- 5.AI 的三个层次
- 6.AI 的能力边界
- 7.动手试试：在线体验 AI
- 8.八个常见误解
- 9.本章学完你应该能做什么
- 10.练习题 / 小实验
- 11.延伸阅读
- 12.下一步
- AI总结

## 1.什么是 AI？

AI（Artificial Intelligence，人工智能）可以理解为一组能力，让计算机在某些任务上表现出「类似智能」的行为。

- **目标**：让机器完成智能任务
- **能力**：识别、预测、生成、规划
- **定位**：机器学习和 LLM 都在其中

「人工智能」一词诞生于 1956 年。那年夏天，一群科学家在美国达特茅斯学院开了两个月的会，正式把「让机器像人一样思考」命名为 AI。参会者包括 John McCarthy、Marvin Minsky、Claude Shannon、Herbert Simon 等。

![1956 年达特茅斯会议的八位参会者](https://unclecheng-li.github.io/Hello-AI/assets/images/basics/what-is-ai/some-participants-of-the-dartmouth-conference.png)

图1：1956 年达特茅斯会议的八位参会者

AI 的定义从 1956 年一路争论至今。

## 2.AI 发展里程碑

从达特茅斯会议到现在，AI 经历了「三起两落」——三次浪潮之间夹着两次寒冬。每次低谷都是对技术路径的反思，每次爆发都离不开算法、数据、算力三要素的共振。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/what-is-ai_mermaid_0_8a3eecb7.svg)

| 年份 | 事件 | 为什么重要 |
| --- | --- | --- |
| 1950 | 图灵测试（Turing Test） | 第一次给出「机器能不能思考」的判据 |
| 1956 | 达特茅斯会议 | AI 正式成为一门学科 |
| 1966 | ELIZA 聊天机器人 | 世界上第一个聊天程序，用模式匹配模拟心理治疗对话。它的创建者 Weizenbaum 后来反而成了 AI 伦理的批评者——他发现用户居然会对一个这么简单的程序产生情感依赖 |
| 1969 | 感知机 XOR 困境 | Minsky 等人证明单层感知机无法解决异或问题，直接导致神经网络研究被冷落近 15 年 |
| 1997 | 深蓝击败卡斯帕罗夫 | AI 首次在正式比赛中战胜人类世界冠军。但深蓝靠的是暴力穷举，它并不理解棋局 |
| 2012 | AlexNet 夺冠 | 深度学习第一次在大规模竞赛中碾压传统方法，引爆了工业界的关注 |
| 2016 | AlphaGo 击败李世石 | 围棋被认为是最复杂的棋类，AlphaGo 用深度学习 + 强化学习攻破了这个「人类最后的堡垒」 |
| 2022 | ChatGPT 发布 | 2 个月月活破亿，成为历史上增长最快的消费者应用，AI 从学术圈正式走向大众 |

> 💡 **规律**：每一次低谷都是对技术路径的反思，每一次爆发都是算法、数据、算力三要素协同共振的结果。

## 3.AI 能做什么

别把 AI 绑定到某个具体产品上。只要能让计算机表现出「像智能一样」的能力，都可以放进 AI 这张大地图里。

常见的 AI 任务：

- **识别**：识别人脸、识别语音、识别图片中的物体
- **分类**：判断垃圾邮件、标记恶意样本、给用户打标签
- **预测**：预测天气、预测故障、预测用户可能点击什么
- **推荐**：视频推荐、商品推荐、内容推荐
- **生成**：写文字、画图、生成代码、合成语音
- **规划与对话**：路径规划、资源调度、任务拆解、问答、客服、翻译、总结

真实案例：

| AI 任务 | 你可能在用的产品 | 背后的技术 |
| --- | --- | --- |
| 识别 | 手机人脸解锁、支付宝刷脸支付 | 计算机视觉（Computer Vision） |
| 分类 | 邮箱自动过滤垃圾邮件 | 文本分类（Text Classification） |
| 预测 | 天气预报、高德地图预估到达时间 | 时序预测、回归模型 |
| 推荐 | 抖音推荐、淘宝「猜你喜欢」、B 站推荐页 | 协同过滤、深度推荐模型 |
| 生成 | ChatGPT 写文案、Midjourney 画图、Suno 生成音乐 | 大语言模型、扩散模型 |
| 规划与对话 | 小爱同学 / Siri 语音助手、高德路线规划 | 语音识别 + NLP、路径规划算法 |

## 4.AI 有很多条技术路线

很多人第一次接触 AI 从 ChatGPT 开始，容易把 AI 和聊天机器人画等号，其实 AI 有很多路线。

![ChatGPT 页面截图](https://unclecheng-li.github.io/Hello-AI/assets/images/basics/what-is-ai/ChatGPT.png)

图2：ChatGPT

### 规则系统

**关键词：** 人写条件，系统照着执行。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/what-is-ai_mermaid_1_3508a7c2.svg)

- 人写好条件 → 系统执行
- 例如：用户名为空时提示；金额超阈值时走人工审核。

优点：稳定、可控、好解释。缺点：复杂场景下规则会爆炸。

### 机器学习

**关键词：** 把大量样本交给模型，让它自己从数据里找规律。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/what-is-ai_mermaid_2_e13a748a.svg)

- 给大量带标签的样本，模型学会识别垃圾邮件、异常登录、可能被点击的商品等。
- 比规则系统灵活，但依赖数据质量，数据有偏见则模型也有偏见。

### 深度学习

**关键词：** 机器学习里的重要路线，用多层神经网络处理复杂模式。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/what-is-ai_mermaid_3_fef9c2b4.svg)

突破包括：
- 图像识别：2012 年 AlexNet 在 ImageNet 把错误率从 26% 降到 15.3%
- 语音识别：端到端神经网络提升准确率
- 自然语言处理：从翻译到问答，深度学习逐步取代传统统计方法

今天说的大模型，底层基本走深度学习。

### 大模型 / LLM

**关键词：** 在海量文本上训练，擅长问答、总结、改写、抽取和生成草稿。

![Mermaid Diagram](https://unclecheng-li.github.io/Hello-AI/assets/mermaid/what-is-ai_mermaid_4_ff227905.svg)

- LLM 是 Large Language Model 的缩写，属于深度学习里偏语言方向的模型。
- 参数规模动辄几百亿到几千亿。ChatGPT、Claude、DeepSeek、通义千问等产品都是 LLM 加包装。
- 擅长文本类任务：问答、总结、改写、抽取、生成草稿、在上下文中推理。

LLM 有三个明显边界：
- 正确性没法自动保证：生成的是「最可能的回答」，不是「经核实的事实」
- 知识有截止线：训练结束后的事需要搜索或外部工具
- 长链条精确逻辑不稳定：算账、审计、严格因果推理需额外验证

### 一张图理清关系

```js
AI（人工智能总称）
└─ 机器学习（从数据里学规律）
   └─ 深度学习（用深层神经网络处理复杂模式）
      └─ LLM（大语言模型，偏语言方向）
         └─ ChatGPT 等产品（LLM + 产品包装）
```

LLM 只是 AI 地图里的一条路线。如果别人默认「AI = ChatGPT」，大概率被产品宣传带偏了。

## 5.AI 的三个层次

**ANI：弱人工智能 / 窄人工智能**
- 当前所有 AI 都在这一层。
- 在特定任务上非常强（如 AlphaGo、医学影像 AI、GPT-4 通过律师考试），但能力无法迁移。
- 当前最强的 LLM 仍在 ANI，离 AGI 还有距离。

**AGI：通用人工智能**
- 尚未实现的目标：让机器在任何认知任务上达到或超过人类水平。
- 需要跨领域迁移学习、因果推理、自主设定目标等能力。
- 实现时间未知，乐观者说十几年，悲观者说永远。

**ASI：超级人工智能**
- 比 AGI 更远的概念：在所有领域都远超人类。
- 理论上可能递归自我改进，但纯属推测，目前没有实现路径。

判断：今天接触的所有 AI 基本都在 ANI 层。

## 6.AI 的能力边界

**LLM 很擅长：**
- 总结长文本
- 改写润色
- 翻译
- 按模板生成内容
- 在已知知识范围内回答问题
- 代码补全和简单调试

**LLM 需谨慎使用：**
- 提供事实正确的答案（可能“幻觉”）
- 处理训练截止后的新信息
- 严格数学计算和精确逻辑推理
- 长链条任务保持一致
- 理解情感和语境中的微妙含义
- 需要常识判断的开放式场景

**经验法则：** 把 AI 当成读过很多书但没见过真实世界、偶尔会瞎编的助手，需要你监督。

## 7.动手试试：在线体验 AI

- 🎨 **Quick, Draw!** —— AI 猜你画什么。地址：https://quickdraw.withgoogle.com/
  - 技术：神经网络图像识别。试试画得潦草一点，看 AI 能否认出。
- 🧠 **Teachable Machine** —— 训练你自己的 AI 模型。地址：https://teachablemachine.withgoogle.com/
  - 技术：迁移学习。试试用“举手/不举手”照片控制网页播放/暂停。
- 📊 **TensorFlow Playground** —— 可视化神经网络学习。地址：http://playground.tensorflow.org/
  - 技术：前馈神经网络 + 反向传播。试试增加隐藏层观察训练和过拟合。
- 💬 **ELIZA** —— 1966 年聊天机器人。地址：https://www.masswerk.at/elizabot/eliza.html
  - 代码不到 200 行，用模式匹配模拟心理治疗师。体会“ELIZA 效应”。

## 8.八个常见误解

1. **AI 像人一样思考**：AI 没有意识、情绪、自我认知，只是找模式输出结果。
2. **AI 很快会有意识**：意识来自生物演化，AI 底层是数学函数。
3. **AI 会取代所有工作**：会淘汰机械重复岗位，但创造力、同理心、批判判断需要人兜底。
4. **AI 始终客观公正**：数据来自人，会带偏见。例：Amazon 招聘工具因历史数据中学到“女性简历降分”，删除性别字段也无法消除性别偏见，最终被放弃。
5. **AI 会毁灭人类**：风险来自人类误用（深度伪造、自主武器等），机器自己没有动机。
6. **AI 比人类聪明**：AI 是窄智能，人类智能更通用。
7. **AI 能自己学习进步**：模型需要人类设计、选择数据、标注、评估、调参，不能自主进化。
8. **AI 能解决所有问题**：AI 是工具，社会问题最终由人类处理。

## 9.本章学完你应该能做什么

- 分清 AI、机器学习、深度学习、LLM 的关系。
- 知道当前所有 AI 都是 ANI。
- 判断哪些问题适合交给 AI。
- 看穿“AI 觉醒”等标题党。

## 10.练习题 / 小实验

**练习 1：概念分类**
(a) 手机计算器算 123 × 456
(b) 微信语音转文字
(c) Excel 排序
(d) 高德地图预测到达时间
(e) 自动贩卖机投币出货

参考：属于 AI 的是 (b) 和 (d)，因为需要从数据中学习模式；(a)(c)(e) 是确定算法或机械逻辑。

**练习 2：技术路线判断**
(a) 电梯按楼层停靠 → 规则系统（简单确定）
(b) 判断垃圾邮件 → 机器学习（形式多变）
(c) ATM 检查密码 → 规则系统（确定性）
(d) 预测用户明天是否打开 App → 机器学习（行为复杂，需学习历史）

**练习 3：偏见识别**
训练数据：公司过去 20 年 95% CEO 是男性。
- (a) 可能学到“男性更适合当 CEO”。
- (b) 删除性别字段不能解决，其他字段可能携带性别信息。
- (c) 改进：审视数据代表性、检测偏见指标、人工审核、定期审计公平性。

**练习 4：动手实验**
打开 Quick, Draw!，完成 3 个任务。思考：识别能力来自训练数据；训练集中常见的物体更容易被认出，否则认不出。文化差异也影响表现。

## 11.延伸阅读

- [人工智能发展简史：从图灵测试到 GPT-5](https://cloud.tencent.com/developer/article/2632623)
- [微软 AI For Beginners 课程](https://microsoft.github.io/AI-For-Beginners/)
- [Google ML Crash Course](https://developers.google.com/machine-learning/crash-course/)
- [Why a 1960s Chatbot Left Its Creator Deeply Unsettled](https://www.history.com/articles/ai-first-chatbot-eliza-artificial-intelligence-precursor-llms)

## 12.下一步

[机器学习 →](https://unclecheng-li.github.io/Hello-AI/basics/machine-learning/) 了解 AI 如何从数据里学习规律，以及它与规则系统的区别。

## AI总结

本节介绍了 AI 的基本概念、发展历史、技术路线、能力边界和常见误解。核心要点：AI 是让机器完成智能任务的能力集合；当前所有 AI 都是弱人工智能（ANI）；AI 不等于聊天机器人，而是包含规则系统、机器学习、深度学习、LLM 等多元路径；使用 AI 时需要对其能力边界保持警惕。通过动手实验和练习题，你可以建立对 AI 的正确认知。