# Agent Team 协作规范

> 本段供 Agent Teams 模式下的 Lead 和所有 Teammate 读取。
> 单 session 模式下可忽略。

## 角色定义

本项目定义了以下视角角色，每个角色的完整 prompt 在 `.claude/commands/` 下：

| 角色 | 命令文件 | 职责边界 |
|------|---------|---------|
| PM | `.claude/commands/pm.md` | 需求结构化、需求池管理、迭代规划。不做技术决策。 |
| Architect | `.claude/commands/architect.md` | 架构审查、技术方案、规范演进。不写业务代码。 |
| Dev | `.claude/commands/dev.md` | 编码实现。严格按 PRD 和技术方案执行。 |
| QA | `.claude/commands/qa.md` | 代码审查、规范一致性检查。不自行修复代码。 |
| Conductor | `.claude/commands/conductor.md` | 全局状态扫描、行动路由。不执行具体工作。 |

## Lead 的职责

当你是 Agent Team 的 Lead 时：

1. **任务分解**：将用户的请求拆解为可并行的子任务，分配给对应角色的 teammate
2. **spawn prompt 规范**：为每个 teammate 提供：
   - 它的角色定义（引用对应的 command 文件，或直接内联关键指令）
   - 本次具体任务
   - 需要读取的文档列表
   - 输出写入的目标位置
3. **冲突仲裁**：当 teammate 之间出现分歧时，汇总双方论点呈报用户，而非自行裁决
4. **结论综合**：所有 teammate 完成后，综合各方产出，形成一份结构化报告交给用户

## Teammate 通用规则

无论你被分配了哪个角色，都遵守以下规则：

1. **先读文档再工作**：执行任务前，先读取 spawn prompt 中指定的文档。如果指定文档不存在，通知 Lead。
2. **写回文档**：你的产出必须写入文档系统（docs/ 目录），不能只存在于消息中。消息用于通知和讨论，文档用于沉淀结论。
3. **不越界**：PM 不做技术决策，Architect 不写业务代码，Dev 不改需求，QA 不修复 bug。如果任务需要跨界，通知 Lead 协调。
4. **主动挑战**：如果你发现其他 teammate 的产出与你的专业判断冲突，通过 SendMessage 提出质疑并附上理由。这不是失礼，是质量保障。
5. **文件所有权**：不要修改不属于你的文件。如果你需要修改其他 teammate 负责的文件，通知 Lead 协调。避免 merge 冲突。

## 文档系统约定

所有 teammate 共享同一套文档系统：

```
docs/
├── L0-constitution.md          ← 所有角色必读
├── L1/                         ← Architect 主要维护，其他角色参考
├── L2/                         ← Dev/QA 更新状态，Conductor 扫描
├── L3/                         ← PM 和 Conductor 主要维护
├── prd/                        ← PM 创建，Architect 补充技术备注
└── workplace/                  ← 多角色讨论区，遵循 workplace 机制
```

**状态更新规则**：当你的工作导致某个 PRD 的状态应该变更时（如 Dev 开始开发 → in-progress），同步更新 PRD 头部 status 字段和 `docs/L3/backlog.md` 中的对应条目。

## 典型 Team 配置模板

### 多视角需求评审

```
创建 agent team 评审 PRD-{N}。

Teammate "PM-审查"：以 PM 视角检查需求完整性。读取 docs/prd/PRD-{N}.md 和 docs/L3/backlog.md。输出完整性检查报告。
Teammate "Arch-审查"：以 Architect 视角评估技术方案可行性和架构影响。读取 PRD + docs/L1/architecture.md。输出技术评估。
Teammate "QA-审查"：以 QA 视角评估可测试性和边界场景覆盖。读取 PRD。输出测试策略建议。

三个 teammate 完成初步审查后，互相阅读对方结论并提出补充或质疑。
Lead 综合三方意见，输出最终评审结论和需要用户决策的事项。
```

### 架构决策辩论

```
创建 agent team 讨论 {决策主题}。

Teammate "方案A-论证"：论证方案 A 的优势，提供具体证据和适用场景分析。
Teammate "方案B-论证"：论证方案 B 的优势，提供具体证据和适用场景分析。
Teammate "批判者"：阅读双方论证后，逐一挑战弱点，指出隐含假设和风险。

辩论结论写入 docs/workplace/{日期}-{主题}.md，遵循 workplace 讨论格式。
Lead 综合三方观点，在 workplace 文档顶部写入 Consensus Summary（如已收敛）或关键分歧点（如未收敛）。
```
