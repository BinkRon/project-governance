# Workplace 讨论机制

> 本文件在初始化时复制到 `docs/workplace/README.md`，
> 供所有角色在参与 workplace 讨论时参考。

## 定位

`docs/workplace/` 是讨论文档区，用来承接：

- 重要但尚未形成共识的议题
- 需要多角色或多 Agent 来回讨论的问题
- 暂时不适合直接写进正式文档的探索性内容

## 文档结构

每篇 workplace 文档分两部分：

### 1. 问题定义区

- 背景
- 参考材料
- 问题定义
- 当前已知约束

### 2. 讨论区

按编号倒序排列，最新 comment 在最上面：

```md
## Discussion

### Turn 003 Architect
Type: 对比意见

观点：
- ...

依据：
- ...

待确认：
- ...

### Turn 002 PM
Type: 补充证据
...
```

评论类型建议集合：调研结论、补充证据、对比意见、反驳、收敛判断、归档说明

## 文件命名

`YYYY-MM-DD-主题.md`

## 状态

文档顶部维护状态字段：
- `Status: discussing` — 正在讨论
- `Status: converging` — 接近收敛
- `Status: agreed` — 已形成共识，待转正式文档
- `Status: archived` — 已归档

## 达成共识后

1. 文档顶部新增 Consensus Summary
2. 保留原始 Turn 讨论串不删
3. 共识内容转写到正式文档
4. 原文档移入 `docs/workplace/archive/`

正式文档去向：
- 架构原则 → `docs/L1/architecture.md`
- 产品方案 → `docs/prd/...`
- 执行排期 → `docs/L3/current-sprint.md`
- 需求池 → `docs/L3/backlog.md`
