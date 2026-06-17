# ADE Failure Registry

> 跨架构 AI Agent 失效案例登记库 — 由社区驱动，持续演进。

[![Cases](https://img.shields.io/badge/cases-11-brightgreen)](./cases/)
[![Architectures](https://img.shields.io/badge/architectures-7-blue)](./ARCHITECTURES.md)
[![Taxonomy](https://img.shields.io/badge/taxonomy-5%20layers%20×%2015%20codes-orange)](./TAXONOMY.md)

## What is this?

ADE Failure Registry 是一个**跨架构的 AI Agent 失效案例知识库**，基于 ADE（Agent Delivery Engineering）五层分类学统一组织。

它不是一个 bug tracker——它是一个**结构化的失效模式百科全书**，帮助：
- **研究者**：用真实案例验证和丰富 ADE 理论
- **工程师**：在设计新系统时避免已知的失效模式
- **社区**：持续收集和分类各架构的失效案例

## Five-Layer Taxonomy — 五层分类学

```
L1 — 通道层 (Channel Layer)        → arXiv:2606.04896
  A: 发送端断裂 | B: 接收端断裂 | C: 状态同步断裂

L2 — 可观测层 (Observability Layer) → arXiv:2606.08162
  A: 错误静默 | B: 成功幻觉 | C: 恢复机制静默失效

L3 — 数据层 (Data Layer)            → arXiv:2606.18065
  A: 数据丢失 | B: 数据衰变 | C: 数据不一致

L4 — 认知层 (Cognitive Layer)       → ADE 框架理论
  A: 知识断裂 | B: 认知框架滞后 | C: 社会常识缺失

L5 — 行为层 (Behavior Layer)        → ADE 框架理论
  A: 行为越界 | B: 行为缺失 | C: 行为不一致
```

详见 [TAXONOMY.md](./TAXONOMY.md)。

## Case Index — 案例索引

| ID | 层级 | 架构 | 严重度 | 描述 |
|----|------|------|--------|------|
| [L5A-001](./L5-behavior/L5A-001-email-auto-reply.md) | L5-A | hermes | Critical | Email Gateway 自动回复客户 |
| [L1A-001](./L1-channel/L1A-001-cron-memory-channel-fracture.md) | L1-A | hermes | Critical | cron agent 记忆写入通道断裂 |
| [L2C-001](./L2-observability/L2C-001-cron-approval-deadlock.md) | L2-C | hermes | High | cron 审批死锁：安全机制变死锁 |
| [L2C-002](./L2-observability/L2C-002-wecom-websocket-silent-reconnect.md) | L2-C | hermes | High | WeCom WebSocket 重连静默失败 |
| [L1B-001](./L1-channel/L1B-001-qqbot-websocket-zombie-18h.md) | L1-B | hermes | Critical | QQ Bot 僵尸连接 18 小时 |
| [L1B-002](./L1-channel/L1B-002-wecom-846609-path-state-disconnect.md) | L1-B | hermes | Critical | WeCom 846609 发送路径不触发重连 |
| [L1C-001](./L1-channel/L1C-001-wecom-ack-loss-duplicate-messages.md) | L1-C | hermes | High | WeCom ACK 丢失导致重复消息 |
| [L3C-001](./L3-data/L3C-001-desktop-delete-profile-state-inconsistency.md) | L3-C | hermes | High | Desktop 删除 profile 状态不一致 |
| [L3A-001](./L3-data/L3A-001-context-compression-data-loss.md) | L3-A | hermes | Critical | 上下文压缩丢失未 flush 消息 |
| [L3A-002](./L3-data/L3A-002-v4a-patch-9bugs-data-loss.md) | L3-A | hermes | Critical | V4A 补丁解析器 9 个数据丢失 bug |
| [L3B-001](./L3-data/L3B-001-state-db-fts-corruption.md) | L3-B | hermes | Critical | state.db FTS 索引损坏 |

### Statistics

| 层级 | 数量 | Critical | High |
|------|------|----------|------|
| L1 通道层 | 4 | 3 | 1 |
| L2 可观测层 | 2 | 0 | 2 |
| L3 数据层 | 4 | 4 | 0 |
| L4 认知层 | 0 | — | — |
| L5 行为层 | 1 | 1 | 0 |
| **合计** | **11** | **8** | **3** |

### Architecture Coverage

| 架构 | 案例数 |
|------|--------|
| Hermes Agent | 11 |
| LangChain | 待收集 |
| CrewAI | 待收集 |
| AutoGPT/AutoGen | 待收集 |
| OpenAI Assistants | 待收集 |
| Claude | 待收集 |
| Custom | 待收集 |

## Supported Papers

本案例库为以下 ADE 论文提供持续的实证支撑：

| 论文 | arXiv | GitHub |
|------|-------|--------|
| Channel Fracture | [2606.04896](https://arxiv.org/abs/2606.04896) | [ADE-standard/channel-fracture](https://github.com/ADE-standard/channel-fracture) |
| Silent Failure | [2606.08162](https://arxiv.org/abs/2606.08162) | [ADE-standard/silent-failure](https://github.com/ADE-standard/silent-failure) |
| Intelligence Entropy | [2606.18065](https://arxiv.org/abs/2606.18065) | [ADE-standard/intelligence-entropy](https://github.com/ADE-standard/intelligence-entropy) |

## How to Contribute

1. 阅读 [CONTRIBUTING.md](./CONTRIBUTING.md)
2. 使用 [TEMPLATE.md](./TEMPLATE.md) 编写案例
3. 提交 PR

也欢迎在 [Issues](https://github.com/ADE-standard/ade-failure-registry/issues) 中报告新案例或建议改进。

## About ADE

**Agent Delivery Engineering (ADE)** 是稳定性力量的工程化 — 人类对抗智能熵增的工程运动。

- 论文：[arxiv.org](https://arxiv.org/search/?query=ade+agent+delivery+engineering)
- 规范：[ADE-standard](https://github.com/ADE-standard)
- 开发者：[Qijing Digital Technology](https://github.com/tobiglevent001)

## License

[CC-BY-4.0](./LICENSE)

---

*Last updated: 2026-06-17 | Maintained by Dexing Liu, Qijing Digital Technology*
