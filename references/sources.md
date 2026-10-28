# design-review 外部参考基准（维护用，不被 SKILL.md 引用）

> 本文件记录本 skill 的设计来源与「采纳 / 不采纳」判定，供后续迭代时避免重复调研、也避免把已被否决的做法捡回来。审核流程本身以 `SKILL.md` 为准。

## 参考来源

| 来源 | 采纳点 | 不采纳 / 仅作反面参照 |
|---|---|---|
| `github/spec-kit` — `templates/commands/analyze.md`（跨产物一致性分析） | 发现项必须带稳定 ID + `file:line` 位置 + 建议列；按 CRITICAL/HIGH/MEDIUM/LOW 给分级启发式；覆盖率统计与「零问题也要出指标」；只读分析、可重跑结果稳定 | 其 spec/plan/tasks 三件套与 constitution 文件约定（本项目无此结构） |
| `github/spec-kit` — `templates/commands/checklist.md`（"requirements 的单元测试"） | 审核聚焦「要求写清楚了吗」：完整性/清晰性/一致性/可测性/覆盖/边界；模糊词与占位符必须量化；`[Gap]` 标记缺写位置；提问式清单产物 | 清单文件的多类型拆分与 append 机制（对单次审核过重） |
| `kubernetes/enhancements` — KEP 模板 | 「不实施的理由（Drawbacks）」与「被否决的备选」作为必查项；生产就绪（指标/告警/回滚/依赖配额）；毕业标准 = 可测验收口径 | KEP 的 GA/Alpha 阶段划分（不适用于本项目节奏） |
| `obra/superpowers` — `skills/writing-plans/SKILL.md` | 「计划 = 执行者无法独自决定的事」判据；落点必须精确到文件/签名；`TBD / 处理边界情况 / 补充必要校验` 属不合格；自审覆盖度（需求 → 步骤可指认） | 其 TDD 逐步模板与 subagent 执行编排（属执行侧，不是审核侧） |
| `github/spec-kit` — `.github/skills/code-review/SKILL.md` | 区分「未解释的设计违规」与「有理有据的有意变更」，后者审理由与证据；引用文档章节而非复述规则 | 其仓库专属路径（`design/cli.md` 等） |
| `VoltAgent/awesome-claude-code-subagents` — `architect-reviewer` | 仅作**反面参照**：30+ 维度关键词堆叠、无判据无证据 ⇒ 报告不可核销。本 skill 因此坚持「维度少而每条带证据 + 判据」 | 全部维度罗列与其通信协议 JSON |
| `plan-review` 子代理 system（本机） | 已知缺陷「优先级膨胀（把加固建议升格 P0/P1）」⇒ 步骤4 增加「真实缺陷 / 加固建议」性质判定 + 防膨胀清单 + 分级依据；输出须可逐条核销 | — |

## 维护约定

- 新增维度前先问：能否挂到现有 A-E 维度下并给出判据；不能则说明为何必须新增。
- 新增清单文件必须同时更新 `SKILL.md` 的类型表与 `mobile-delta` 触发说明，否则不会被读到。
- 改动后跑 `python3 ~/.config/opencode/scripts/verify-env-guard.py --gate --quiet`（skill description 属门禁断言域）。
