# Contributing to ADE Failure Registry

## 欢迎贡献

ADE Failure Registry 是一个社区驱动的知识库。你的每一个案例都在帮助整个行业避免重复错误。

## 提交案例的步骤

### 1. 确认案例符合收录标准

- **真实发生**：来自生产环境、Issue 报告或可验证的实验
- **可分类**：能归入 [五层分类学](./TAXONOMY.md) 中的至少一层
- **有根因**：有明确的根因分析（不只是现象描述）
- **非重复**：与已有案例不重复

### 2. 使用模板

复制 [案例模板](./TEMPLATE.md) 并填写：

```bash
cp TEMPLATE.md L{层级}-{编号}-{简述}.md
```

### 3. 放置到正确目录

```
L1-channel/      → 通道层断裂
L2-observability/ → 可观测性缺失
L3-data/         → 数据衰变/丢失
L4-cognitive/    → 认知能力退化
L5-behavior/     → 行为偏离契约
```

**层级判定规则**：如果一个案例跨多层，归最底层（物理优先）。

### 4. 提交 PR

```bash
git checkout -b case/{案例ID}
git add L{层级}/{文件}
git commit -m "Add {案例ID}: {简述}"
git push origin case/{案例ID}
```

然后在 GitHub 上创建 PR。

## 案例编号规则

```
{层级代码}-{三位数编号}-{简短描述}.md

层级代码：
  L1A = L1 通道层-发送端断裂
  L1B = L1 通道层-接收端断裂
  L1C = L1 通道层-状态同步断裂
  L2A = L2 可观测层-错误静默
  L2B = L2 可观测层-成功幻觉
  L2C = L2 可观测层-恢复机制静默失效
  L3A = L3 数据层-数据丢失
  L3B = L3 数据层-数据衰变
  L3C = L3 数据层-数据不一致
  L4A = L4 认知层-知识断裂
  L4B = L4 认知层-认知框架滞后
  L4C = L4 认知层-社会常识缺失
  L5A = L5 行为层-行为越界
  L5B = L5 行为层-行为缺失
  L5C = L5 行为层-行为不一致

示例：
  L1B-001-qqbot-websocket-zombie-18h.md
  L2A-001-cron-approval-deadlock.md
  L3A-001-context-compression-data-loss.md
```

## 质量标准

### 必须包含

- [ ] 案例 ID 和日期
- [ ] 来源（GitHub Issue / 生产事故 / 论文引用）
- [ ] 架构标签（hermes/langchain/crewai/...）
- [ ] 分类归位（对应五层分类学的哪个子类）
- [ ] 根因分析（至少到代码/配置级别）
- [ ] ADE 理论关联（哪个 ADE 原则被违反）
- [ ] 修复建议或防御措施

### 建议包含

- [ ] 原始日志/截图证据
- [ ] 受影响代码的精确位置
- [ ] 复现步骤
- [ ] 与已有案例的关联

## 审核标准

1. **真实性**：来源可验证（Issue 链接、commit hash、日志片段）
2. **分类正确**：符合五层分类学的判定流程
3. **根因充分**：不止描述现象，要分析到机制层面
4. **无敏感信息**：不包含 API key、密码、个人信息
5. **英文或中文**：两者均可，保持一致

## 报告 Issue

如果你发现已有案例的信息不准确或分类有误，请在 [Issues](https://github.com/ADE-standard/ade-failure-registry/issues) 中报告。

---

## 许可证

提交的内容将遵循 CC-BY-4.0 许可证。
