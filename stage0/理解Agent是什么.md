#  区分 chatbot、workflow、agent、multi-agent

这几个概念代表了 AI 应用能力的不同层级，可以理解成**从「聊天」→「自动执行」→「自主完成任务」→「团队协作」** 的演进。

| 类型          | 核心特点   | 是否自主决策      | 是否调用工具 | 是否规划任务  | 典型产品                       |
|-------------|--------|-------------|--------|---------|----------------------------|
| Chatbot     | 对话问答   | ❌           | 少量/可选  | ❌       | ChatGPT（纯聊天）、Claude        |
| Workflow    | 固定流程   | ❌（按规则）      | ✅      | ❌       | Dify Workflow、n8n、Zapier   |
| Agent       | 自主完成目标 | ✅           | ✅      | ✅       | OpenAI Agent、AutoGPT、Manus |
| Multi-Agent | 多智能体协作 | ✅（多个 Agent） | ✅      | ✅（协作规划） | CrewAI、AutoGen、LangGraph   |

下面分别说明。

---

# 1. Chatbot（聊天机器人）

最简单的一层。

特点：

* 用户输入
* LLM 回复
* 一问一答
* 不会主动完成任务

流程：

```text
用户
  │
  ▼
LLM
  │
  ▼
回答
```

例如：

> 用户：
> 帮我解释 Transformer。

AI：

> Transformer 是一种……

结束。

整个过程没有调用数据库、API、浏览器，也没有执行动作。

适合：

* QA
* 客服
* 知识问答
* 翻译
* 写作

---

# 2. Workflow（工作流）

Workflow 本质上是：

> **人提前把流程设计好，AI 只是其中一个节点。**

例如：

```text
用户输入
    │
    ▼
意图分类
    │
 ┌──┴────┐
 │        │
客服     SQL查询
 │        │
 ▼        ▼
LLM总结
 │
 ▼
输出
```

特点：

* 每一步固定
* 没有自主思考
* 没有动态规划
* 路径由开发者决定

例如：
```text
收到发票

↓

OCR

↓

提取金额

↓

数据库查询

↓

LLM 总结

↓

邮件发送
```
整个流程已经写死。

所以 Workflow 更像：

> **AI + 自动化流水线**

代表：

* Dify Workflow
* n8n
* Zapier AI
* Make

---

# 3. Agent（智能体）

Agent 的目标不是回答问题，而是：

> **完成一个目标（Goal）。**

例如：

用户说：

> 帮我找一台 2 万以内的游戏本，并整理成 Excel。

Agent 会自己决定：

```text
目标
 │
 ▼
需要搜索
 │
 ▼
调用浏览器
 │
 ▼
找到商品
 │
 ▼
比较配置
 │
 ▼
生成Excel
 │
 ▼
结束
```

整个过程中：

Agent 会不断问自己：

> 下一步应该干什么？

所以 Agent 会：

* 思考（Reason）
* 规划（Plan）
* 行动（Act）
* 观察（Observe）
* 再规划

经典循环：

```text
Goal

↓

Think

↓

Action

↓

Observation

↓

Think

↓

Action

↓

...
```

即：

Reason → Act → Observe（常见于 ReAct 思想）。

Agent 通常具备：

✅ Memory

✅ Tool Calling

✅ Browser

✅ API

✅ Code Interpreter

✅ 长任务

例如：

> 帮我写一篇关于 AI 的报告，并引用最新论文。

Agent 会：

* 搜论文
* 阅读论文
* 提炼
* 写报告
* 引用文献

整个过程用户不用一步一步指挥。

---

# 4. Multi-Agent（多智能体）

如果任务太复杂，

一个 Agent 不够。

于是多个 Agent 分工合作。

例如：

开发一个网站。

可能有：

```text
             Manager
                 │
      ┌──────────┼──────────┐
      │          │          │
 Product      Backend     Frontend
      │          │          │
      └──────────┼──────────┘
                 │
               Tester
                 │
              Reviewer
```

每个 Agent：

都有自己的：

* Prompt
* Memory
* Tool
* 专业能力

例如：

Research Agent

只负责：

> 搜资料

Writer Agent

只负责：

> 写文章

Reviewer Agent

只负责：

> 修改

最终：

Manager Agent 汇总。

---

## 举例

用户：

> 写一份新能源行业分析。

Multi-Agent：

Research Agent：

> 找数据。

↓

Finance Agent：

> 分析财报。

↓

Writer Agent：

> 写报告。

↓

Reviewer Agent：

> 检查逻辑。

↓

Manager：

> 输出最终版本。

这种模式与公司团队协作类似。

---

# 四者的关系

```text
Chatbot
    │
    ▼
Workflow
    │
    ▼
Agent
    │
    ▼
Multi-Agent
```

能力逐步增强：

* **Chatbot**：回答问题。
* **Workflow**：按照预设流程自动执行。
* **Agent**：围绕目标自主规划、调用工具并完成任务。
* **Multi-Agent**：多个 Agent 分工协作，处理更复杂、跨领域的目标。

---

# 一个实际例子

假设用户说：

> **帮我规划一趟东京五日游。**

### Chatbot

直接生成一份行程。

```
Day1...
Day2...
```

---

### Workflow

固定流程：

```
输入目的地
↓

查询天气

↓

查询景点

↓

生成路线

↓

输出
```

所有步骤都由开发者预先定义。

---

### Agent

Agent 会自主决定需要哪些步骤，例如：

```
思考：
需要预算、出行时间、住宿偏好

↓

询问用户缺失信息

↓

搜索机票

↓

搜索酒店

↓

比较价格

↓

优化路线

↓

生成 PDF
```

步骤可以根据情况动态调整，而不是固定模板。

---

### Multi-Agent

多个 Agent 并行协作：

```
旅行规划 Agent
        │
 ┌──────┼──────┐
 │      │      │
机票    酒店   景点
Agent  Agent  Agent
 │      │      │
 └──────┼──────┘
        │
预算优化 Agent
        │
最终整合 Agent
```

不同 Agent 可以并行搜索和分析，再由协调者整合结果，因此更适合大型、复杂、需要多领域知识和大量工具调用的任务。
