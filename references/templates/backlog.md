# 需求池 (Backlog)

> Last Updated: {日期}
> 
> 本文件是所有需求的中央索引。每个需求对应 `docs/prd/` 下的一份 PRD 文档。
> 状态变更规则见末尾【状态流转规则】。

## 状态总览

| 状态 | 数量 | 说明 |
|------|------|------|
| draft | 0 | PRD 初稿，尚有未确认项 |
| ready | 0 | PRD 完整，可排入迭代 |
| planned | 0 | 已排入当前迭代 |
| in-progress | 0 | 开发中 |
| review | 0 | 代码完成，QA 审查中 |
| acceptance | 0 | 待用户验收 |
| done | 0 | 验收通过 |

## 需求列表

### 🔴 draft — 需要完善

| 编号 | 需求 | 优先级 | 创建日期 | 阻塞原因 |
|------|------|--------|---------|----------|
| — | — | — | — | — |

### 🟡 ready — 等待排期

| 编号 | 需求 | 优先级 | 就绪日期 |
|------|------|--------|---------|
| — | — | — | — |

### 🔵 planned — 已排入迭代

| 编号 | 需求 | 优先级 | 迭代 | 预估工作量 |
|------|------|--------|------|-----------|
| — | — | — | — | — |

### 🟢 in-progress — 开发中

| 编号 | 需求 | 开始日期 | 负责 session |
|------|------|---------|-------------|
| — | — | — | — |

### 🟣 review — 审查中

| 编号 | 需求 | 提交审查日期 |
|------|------|------------|
| — | — | — |

### ⚪ acceptance — 待验收

| 编号 | 需求 | 进入验收日期 |
|------|------|-------------|
| — | — | — |

---

## 状态流转规则

```
draft → ready       触发条件：PRD 通过完整性检查（/pm 模式四），所有开放问题已关闭
ready → planned     触发条件：迭代规划时被选中（/pm 模式三），已写入 current-sprint.md
planned → in-progress  触发条件：Dev 开始开发（/dev 启动时自动更新）
in-progress → review   触发条件：Dev 完成开发，提交 QA 审查（/qa 启动时更新）
review → acceptance     触发条件：QA 审查通过（/qa 输出无阻塞性问题）
review → in-progress    触发条件：QA 审查发现阻塞性问题，需返工
acceptance → done       触发条件：用户验收通过
acceptance → in-progress 触发条件：用户验收发现问题，需返工
done → archived         触发条件：迭代结束，PRD 移入 docs/archive/prd/
```

**逆向流转说明**：
- 任何状态都可以回退到 `draft`（如果发现需求本身有问题）
- `review → in-progress` 和 `acceptance → in-progress` 是正常的返工路径
- 返工时，在 PRD 的变更记录中注明返工原因

**状态更新责任**：
- `draft → ready`：/pm 视角负责
- `ready → planned`：/pm 视角负责（迭代规划时）
- `planned → in-progress`：/dev 视角负责（开始开发时）
- `in-progress → review`：/dev 视角负责（开发完成时）
- `review → acceptance` 或 `review → in-progress`：/qa 视角负责
- `acceptance → done` 或 `acceptance → in-progress`：用户自己判断
- `done → archived`：/conductor 视角在迭代回顾时处理
