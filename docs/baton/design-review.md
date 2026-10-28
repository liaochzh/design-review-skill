# 接力棒：design-review skill（方案审核）

> 更新：2026-10-28 11:1x｜状态：**已结项 + 已仓库化，转入被动观察期**
> 仓库：`~/Projects/design-review-skill` ↔ `https://github.com/liaochzh/design-review-skill`（private，main）

## 仓库与发布（2026-10-28 起）

- **本体在仓库，配置目录是目录级软链**：`~/.config/opencode/skills/design-review -> ~/Projects/design-review-skill`。改文件请改仓库内路径，软链会透传。
- **docs 随仓库走**：`docs/plan/`（v1→v7 全文 + r1–r6 评审）、`docs/baton/`、`docs/plan/_evidence/`（冻结包）都在仓库内；`/Users/lcr/docs/` 下**不再保留** design-review 文档。
- **`references/plan-review-reading.md`** 已移入仓库，全局位置（`~/.config/opencode/references/`）留**软链**回指 ⇒ `references/README.md` 索引与 `SKILL.md:126` 的相对路径同时有效。
- **提交身份是仓库局部**：`git config user.email liaochzh@gmail.com`（`user.name` 未设，沿用全局 `LCR`）；不要改成全局。
- **10 份 sample 不在本仓库**：它们属于被审方案，已分到 `skills/code-error-fixer/docs/plan/`（sample-01..05、07、08-09、10-glm）与 `skills/product-manager/docs/plan/`（sample-06）。本仓库只留冻结包副本（不可变，可安全复制）。
- 发布流程：`git add -A && git commit && git push`（`origin` 已配好）；改 `~/.config/opencode/**` 后再跑一次 `python3 scripts/verify-env-guard.py --gate --quiet` ⇒ rc=0。

## 现状（下次接手先核这些）

| 文件（仓库内相对路径） | 行数 | sha256（前 12） | 说明 |
|---|---|---|---|
| `SKILL.md` | 298 | `d101d4bdaad4` | 输出纪律 = 三组 8 条（说什么/怎么说/交什么），**接近膨胀临界，再遇缺口优先组内替换，不加条** |
| `checklists/plan.md` | 41 | `8149b1b63cf0` | 含核销表两列三态约定 |
| `references/sources.md` | — | — | 设计原则与开源同类来源（路由 `references/review-routing.md` 在**全局** references，不随本仓库） |
| `references/plan-review-reading.md` | 137 | — | 观察期落点：采样读数 + **真实使用记录** + 不可归因规则 |
| `docs/plan/_evidence/20261027-s1-v7/` | — | SKILL `d101d4bdaad4` | 冻结包（`a-w` + `META.txt`），**新样本只用这个** |

## 已完成

W1–W11 全落：两档「精简」、三方契约对齐、核销表格式、证据等级 E1–E4、结构自检、篇幅预算、输出纪律三组重组、跨模型采样（GLM-5.3-Flash）。计划全文与分诊记录：`docs/plan/20261027-design-review-next.md`（646 行）；评审报告 r1–r6 同目录。

## 未完成 / 明确不做

- **W6**（`references/ui-design-philosophy.md` 输出模板加「证据：」行、`20261027-design-skill-consistency.md:39` 改断言形态）→ **归 design-skill 会话**，本线不代做。
- **D 脚本**（核销表计数校验）→ **已关闭**：触发条件「观察期断链 ≥1」在真实路径上**不可观测**（报告由派发方转录落盘，三字段整体缺失）。重启前提：派发方原样落盘原始返回。见计划文档 `:63`。
- **registry 探针**（`scripts/check-skill-registry.py`）→ 阻塞于 `HTTP 401`，属运维侧。
- **第二异模型**（`qwen3.6-flash` 控制组）→ 不做：区分结果不改变任何决策。

## 关键约束（动手前必看）

1. 改 `~/.config/opencode/**` 后必须 `python3 scripts/verify-env-guard.py --gate --quiet` rc=0。该目录改动**不交 review 子代理**（易触发外部目录误报）。
2. **有并发会话在写 design skills**：动之前先比 mtime/sha（曾因 19:30 改 SKILL.md 致整对样本作废）。
3. plan-review 子代理**不可写文件、不可跑 shell**，报告必须由主会话转录落盘；其**优先级会膨胀**，逐条复核定级。
4. 采样必须指向冻结包，不得指向 repo 现文；换 skill 版本就要**重建**冻结包（否则技能版本与被审对象两变量混叠）。

## 踩过的坑

1. **「≥150 行全部截断」是假命题** —— 被自家 `sample-01`（206 行、完整）反证。截断是**概率性方差**，阈值只能当保险线，不能当承诺。
2. **「结论档稳定」也被推翻** —— 7 次里 6 次同档，但同一事实 qwen-A 判 P0、qwen-B/GLM 判 P1 就跳档。写「稳定」必须给分母与反例。
3. **真实使用的三字段不可观测** —— 转录落盘会丢「执行模型/自检回执/E1-E4」，不能记为 skill 失败（见 `plan-review-reading.md` 不可归因规则）。
4. **macOS BSD `cp` 会继承源文件只读权限** —— 从 `a-w` 冻结包复制再写会 `Permission denied`，重建时先 `chmod u+w`。
5. **别把 prompt 差异归因成模型差异** —— GLM 严守取证范围 vs qwen 越界，实为 prompt 加了「勿读 repo 现文」。
6. 真实报告只有 35–52 行 ⇒ 篇幅预算（W9）是为「自造 294 行大方案」优化的，真实使用极少触发。

## 下一步

**被动观察**，不主动开发：目标是攒「真实方案事后被证明当初审漏了重大问题」的样本。若真出现，优先改**检查清单**（`checklists/plan.md` / `references/`），而不是往输出纪律加第 9 条。
