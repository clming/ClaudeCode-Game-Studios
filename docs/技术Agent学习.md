# 技术Agent学习

## 1. 这份文档为什么存在

这份文档的目标，不是告诉你“哪个 Agent 会写代码”，而是帮助你从技术组织的角度重新理解这个仓库。

如果理解得够深，你后面面对新项目时就不会只想着：

- 该叫哪个 Agent
- 该补几个 Prompt

你会开始从下面这些层面去思考：

- 技术治理
- 技术管理
- 实现分工
- 目录疆域
- 路径规则
- 工作流

这才是这个模板真正想训练你的能力。

---

## 2. 先重新理解项目总纲

这个项目的总纲不是“让一个 AI 更会写代码”，而是“让一组 AI 形成一个有分工、有升级链、有边界的工作室”。

所以技术 Agent 的意义不是数量，而是组织关系。

从技术角度看，这个模板至少包含 5 层。

## 2.1 技术治理层

代表：

- `.claude/agents/technical-director.md`

负责：

- 技术方向
- 架构边界
- 高风险决策
- 性能预算

---

## 2.2 技术管理层

代表：

- `.claude/agents/lead-programmer.md`

负责：

- 代码级架构
- 模块边界
- API 设计
- 任务拆解
- 代码评审

---

## 2.3 技术实现层

代表：

- `.claude/agents/gameplay-programmer.md`
- `.claude/agents/ui-programmer.md`
- `.claude/agents/engine-programmer.md`
- `.claude/agents/network-programmer.md`
- `.claude/agents/ai-programmer.md`
- `.claude/agents/tools-programmer.md`

负责：

- 在明确约束下写实现
- 把设计变成真正运行的系统

---

## 2.4 技术支撑层

代表：

- `.claude/agents/devops-engineer.md`
- `.claude/agents/security-engineer.md`
- `.claude/agents/performance-analyst.md`
- `.claude/agents/analytics-engineer.md`

负责：

- 环境
- 安全
- 性能
- 数据反馈

---

## 2.5 平台专精层

代表：

- `.claude/agents/godot-specialist.md`
- `.claude/agents/unity-specialist.md`
- `.claude/agents/unreal-specialist.md`

负责：

- 把通用原则落到具体引擎或平台上

---

## 3. 彻底分清 5 类东西

这是学习这个项目最关键的一步。

## 3.1 `.claude/agents/` 是“人”

它定义：

- 岗位身份
- 职责范围
- 禁止事项
- 汇报关系
- 委托关系

它不负责定义目录级编码规则。

---

## 3.2 `.claude/rules/` 是“制度”

它定义：

- 某类路径的硬性边界
- 某类代码的禁止模式
- 某类目录的质量要求

它不负责决定“谁来做”。

---

## 3.3 `.claude/docs/` 是“共享认知”

它定义：

- 技术偏好
- 目录结构
- 协调规则
- 规则总览
- 上下文管理

它不替代 Agent，也不替代 Rules。

---

## 3.4 `.claude/skills/` 是“工作流”

它定义：

- 某类任务怎么推进
- 哪些 Agent 介入
- 产出什么
- 何时停下来确认

它不是岗位定义，也不是路径规则。

---

## 3.5 目录结构是“疆域”

重点文件：

- `.claude/docs/directory-structure.md`
- `.claude/docs/rules-reference.md`

实际目录：

- `src/`
- `assets/`
- `design/`
- `docs/`
- `tests/`
- `tools/`
- `production/`

疆域清晰，职责才会清晰；职责清晰，Rules 才能真正落地。

---

## 4. “人”和“项目内容”还不够，还要加两层

你原本用“人”和“项目内容”来理解，这是对的，但不完整。

更实用的模型是 4 个问题：

1. 谁负责
2. 做什么项目
3. 按什么制度做
4. 按什么流程做

对应到仓库就是：

- 谁负责：`.claude/agents/*.md`
- 做什么项目：`CLAUDE.md`、`.claude/docs/technical-preferences.md`
- 按什么制度做：`.claude/rules/*.md`、`.claude/docs/coding-standards.md`
- 按什么流程做：`.claude/skills/*`

如果只改“人”和“项目内容”，但不改制度和流程，工作室仍然会用旧习惯工作。

---

## 5. 技术 Agent 的能力到底写在哪里

一个技术 Agent 的“完整能力”并不只写在一个文件里，而是分散在 4 层。

## 5.1 岗位能力

写在：

- `.claude/agents/*.md`

例如它擅长什么、不能做什么、如何委托。

## 5.2 项目技术现实

写在：

- `CLAUDE.md`
- `.claude/docs/technical-preferences.md`

例如语言、框架、命名、预算、测试方式。

## 5.3 路径能力边界

写在：

- `.claude/rules/*.md`

例如 `src/ui/**` 下禁止持有游戏状态，`src/networking/**` 必须版本化协议。

## 5.4 工作方法能力

写在：

- `.claude/skills/*`

例如某个任务要不要多 Agent 协同，先产出什么，再进入什么环节。

所以一个 Agent 的真实行为，来自：

> 岗位定义 + 项目技术现实 + 路径规则 + 工作流。

---

## 6. 遇到新项目时，怎么判断该改哪一层

这是最实用的一张判断表。

## 6.1 如果你要改“谁负责”

改：

- `.claude/agents/*.md`
- `.claude/docs/agent-coordination-map.md`
- `.claude/docs/coordination-rules.md`

## 6.2 如果你要改“项目技术栈”

改：

- `CLAUDE.md`
- `.claude/docs/technical-preferences.md`

## 6.3 如果你要改“代码放哪里”

改：

- `.claude/docs/directory-structure.md`
- `.claude/docs/rules-reference.md`

## 6.4 如果你要改“这个目录里的代码必须怎么写”

改：

- `.claude/rules/*.md`

## 6.5 如果你要改“这类任务怎么推进”

改：

- `.claude/skills/*`

---

## 7. 以多端项目为例，怎样形成新的技术组织

如果你要基于这个模板搭一个包含 App、Web、Backend 的新工作室，你至少要这么做。

## 7.1 先改总章程

改：

- `CLAUDE.md`

## 7.2 再改技术共识

改：

- `.claude/docs/technical-preferences.md`

## 7.3 再改目录地图

改：

- `.claude/docs/directory-structure.md`

例如新增：

- `src/mobile/android/`
- `src/mobile/ios/`
- `src/web/`
- `src/backend/api/`
- `src/backend/services/`
- `src/backend/db/`
- `db/migrations/`

## 7.4 再改岗位体系

新增或修改：

- `.claude/agents/mobile-client-lead.md`
- `.claude/agents/android-engineer.md`
- `.claude/agents/ios-engineer.md`
- `.claude/agents/web-frontend-lead.md`
- `.claude/agents/web-frontend-engineer.md`
- `.claude/agents/backend-lead.md`
- `.claude/agents/backend-engineer.md`
- `.claude/agents/api-engineer.md`
- `.claude/agents/database-engineer.md`

并同步改：

- `.claude/docs/agent-coordination-map.md`

## 7.5 再改 Rules

新增：

- `.claude/rules/android-code.md`
- `.claude/rules/ios-code.md`
- `.claude/rules/web-ui-code.md`
- `.claude/rules/backend-code.md`
- `.claude/rules/api-code.md`
- `.claude/rules/database-rules.md`

并同步改：

- `.claude/docs/rules-reference.md`

---

## 8. 这份学习文档想让你形成什么新理解

最重要的结论有 5 条。

1. 这个项目不是“AI 写代码模板”，而是“AI 研发组织模板”。
2. Agent 是岗位，不是全部规则。
3. Rules 是制度，不是岗位描述。
4. Docs 是共享认知，不是执行者。
5. Skills 是流程，不是单纯命令集合。

如果你以后还能再多记住一条，那就是：

> 不要把所有问题都往某一个 Agent 文件里塞。真正稳定的工作室，永远是岗位、制度、流程、疆域和项目共识一起成立。

---

## 9. 建议你的阅读顺序

如果你想继续把这个项目读得更透，建议按这个顺序看：

1. `README.md`
2. `CLAUDE.md`
3. `.claude/docs/quick-start.md`
4. `.claude/docs/agent-coordination-map.md`
5. `.claude/docs/directory-structure.md`
6. `.claude/docs/rules-reference.md`
7. `.claude/docs/technical-preferences.md`
8. `.claude/agents/technical-director.md`
9. `.claude/agents/lead-programmer.md`
10. 你最常用的几个工程 Agent

按这个顺序，你会越来越清楚这个模板真正的结构，而不再只把它当成一个“会调很多 Agent 的工具箱”。
