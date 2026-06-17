# Architecture Index — 架构索引

> ADE Failure Registry 覆盖的 AI Agent 架构。每个架构的失效案例用 ADE 五层分类学统一分析。

## 为什么跨架构？

ADE 失效分类学基于失效的**本质机制**，不绑定任何特定框架。同一个分类（如 L1-B 接收端断裂）在不同架构中有不同的具体表现，但根因相同。

跨架构案例收集的意义：
1. **理论验证**：如果分类框架能覆盖所有架构的案例，证明理论完备
2. **模式发现**：跨架构共性 = 行业级问题，不是个别框架的 bug
3. **实践指导**：新架构的设计者可以参考已有案例避免重复错误

---

## 已收录架构

### Hermes Agent

| 属性 | 值 |
|------|------|
| 开发者 | Nous Research |
| 架构 | Python 单体 + 多平台 Gateway + Plugin 系统 |
| 多 Agent | Profile 隔离 + Cron + 子 Agent 编排 |
| GitHub | [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) |
| 案例数 | 11 |

**典型案例模式**：
- WebSocket 半开连接（QQ Bot 僵尸 18h）
- cron 上下文能力降级（skip_memory=True）
- 跨平台状态不同步（WeCom 846609）

---

### LangChain / LangGraph

| 属性 | 值 |
|------|------|
| 开发者 | LangChain Inc. |
| 架构 | Python/JS 链式调用 + Graph 编排 |
| 多 Agent | LangGraph 状态图 |
| GitHub | [langchain-ai/langchain](https://github.com/langchain-ai/langchain) |
| 案例数 | 待收集 |

**预期案例模式**：
- Chain 中间步骤静默失败
- Graph 状态丢失
- Memory 后端不一致

---

### CrewAI

| 属性 | 值 |
|------|------|
| 开发者 | CrewAI Inc. |
| 架构 | Python 角色编排 + 任务委托 |
| 多 Agent | 角色分工 + 协作流程 |
| GitHub | [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) |
| 案例数 | 待收集 |

**预期案例模式**：
- 角色间通信断裂
- 委托任务静默失败
- 协作流程死锁

---

### AutoGPT / AutoGen

| 属性 | 值 |
|------|------|
| 开发者 | Significant Gravitas / Microsoft |
| 架构 | 自主 Agent + 工具调用 |
| 多 Agent | AutoGen 多 Agent 对话 |
| GitHub | [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) / [microsoft/autogen](https://github.com/microsoft/autogen) |
| 案例数 | 待收集 |

**预期案例模式**：
- 自主循环失控
- 工具调用静默失败
- 多 Agent 对话状态丢失

---

### OpenAI Assistants / GPTs

| 属性 | 值 |
|------|------|
| 开发者 | OpenAI |
| 架构 | 托管 API + Function Calling |
| 多 Agent | GPT Store + Actions |
| GitHub | N/A (托管服务) |
| 案例数 | 待收集 |

**预期案例模式**：
- Function Calling 静默失败
- Thread 状态不一致
- Assistant 间通信断裂

---

### Claude (Anthropic)

| 属性 | 值 |
|------|------|
| 开发者 | Anthropic |
| 架构 | API + Tool Use + MCP |
| 多 Agent | Claude Code / Claude Desktop |
| GitHub | [anthropics/courses](https://github.com/anthropics/courses) |
| 案例数 | 待收集 |

**预期案例模式**：
- Tool Use 返回值丢失
- MCP 连接断裂
- 上下文压缩信息丢失

---

### Custom / 私有架构

| 属性 | 值 |
|------|------|
| 说明 | 企业自建 Agent 系统 |
| 案例数 | 待收集 |

---

## 贡献新架构

如果你使用的 Agent 架构不在上述列表中：

1. 在 [Issues](https://github.com/ADE-standard/ade-failure-registry/issues) 中提议新架构
2. 提供架构的基本信息（开发者、GitHub、架构特点）
3. 提供至少一个符合 ADE 分类的失效案例
4. 审核后加入索引

---

## 版本

- v1.0 — 2026-06-17 — Dexing Liu, Qijing Digital Technology
