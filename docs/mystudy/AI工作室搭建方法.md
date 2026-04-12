# AI工作室搭建方法

## 1. 先理解这个模板的总纲

`Claude Code Game Studios` 的本质，不是“很多 Prompt”，也不是“固定技术栈脚手架”，而是一个可复制、可改造、可约束的 AI 研发组织模板。

它想解决的问题是：

- 单个 AI 会话没有岗位分工
- 没有明确的升级链和审查链
- 没有目录边界和质量门槛
- 没有标准工作流
- 没有共享技术共识

所以这个仓库真正提供的是一套“工作室操作系统”：

- `CLAUDE.md`
  - 工作室总章程
- `.claude/agents/`
  - 岗位定义
- `.claude/skills/`
  - 标准工作流
- `.claude/rules/`
  - 路径约束和编码边界
- `.claude/docs/`
  - 共享知识和治理手册
- `.claude/hooks/` 与 `.claude/settings.json`
  - 自动检查和守门机制
- 项目目录
  - 真实代码疆域和交付物落点

如果你想基于它为一个新项目创建专用 AI 工作室，你要搭建的不是一个“大 Prompt”，而是下面 6 样东西：

1. 组织
2. 制度
3. 流程
4. 共享认知
5. 代码疆域
6. 项目内容

在真正开始定制之前，建议先读：

- `docs/mystudy/工作室母版索引台账.md`

它可以帮你先确认当前母模板的 agents、skills、rules、templates、hooks 基线，避免你在旧数字或旧索引上继续改造。

---

## 2. 不要只用“人”和“项目内容”来理解

“人”和“项目内容”是好的出发点，但还不够。更完整的理解方式是 6 层。

## 2.1 人

对应文件和目录：

- `.claude/agents/*.md`
- `.claude/docs/agent-roster.md`
- `.claude/docs/agent-coordination-map.md`
- `.claude/docs/coordination-rules.md`

这一层回答：

- 谁负责什么
- 谁向谁汇报
- 谁能升级问题
- 谁可以委托谁

---

## 2.2 制度

对应文件和目录：

- `.claude/rules/*.md`
- `.claude/docs/coding-standards.md`
- `.claude/docs/rules-reference.md`
- `.claude/settings.json`
- `.claude/hooks/*`

这一层回答：

- 哪些实现允许
- 哪些模式禁止
- 哪些目录不能越权改
- 哪些质量门槛必须过

---

## 2.3 流程

对应文件和目录：

- `.claude/skills/*`
- `.claude/docs/quick-start.md`
- `docs/WORKFLOW-GUIDE.md`

这一层回答：

- 新项目怎么开始
- 一个系统怎么设计
- 多 Agent 怎么协作
- 发布怎么走

---

## 2.4 共享认知

对应文件和目录：

- `CLAUDE.md`
- `.claude/docs/technical-preferences.md`
- `.claude/docs/context-management.md`
- `docs/COLLABORATIVE-DESIGN-PRINCIPLE.md`

这一层回答：

- 项目是什么
- 技术栈是什么
- 默认命名、测试、性能目标是什么
- 会话如何收束和交接

---

## 2.5 代码疆域

对应文件和目录：

- `.claude/docs/directory-structure.md`
- `.claude/docs/rules-reference.md`
- `src/`
- `assets/`
- `design/`
- `docs/`
- `tests/`
- `tools/`
- `production/`

这一层回答：

- 哪些目录是正式领地
- 哪些人负责哪些地盘
- 哪些规则绑定哪些路径

---

## 2.6 项目内容

对应文件和目录：

- `CLAUDE.md`
- `.claude/docs/technical-preferences.md`
- `design/`
- `docs/`
- `production/`

这一层回答：

- 你到底在研发什么
- 面向哪些平台
- 用哪些引擎、语言和框架
- 现在处于什么阶段

---

## 3. 彻底分清几个核心目录

## 3.1 `CLAUDE.md` 是总章程

位置：

- `CLAUDE.md`

作用：

- 定义项目总身份
- 声明技术栈
- 指向关键治理文档

它不适合堆很多细则。它像公司章程，而不是员工手册。

---

## 3.2 `.claude/agents/` 是岗位定义

位置：

- `.claude/agents/`

作用：

- 定义这个 Agent 是谁
- 定义它负责什么
- 定义它不能做什么
- 定义它向谁汇报、能委托谁

它定义的是“职员职责”，不是“目录编码规范”。

---

## 3.3 `.claude/rules/` 是硬边界

位置：

- `.claude/rules/`

作用：

- 约束某个目录下的代码必须怎么写
- 约束某类文档必须满足什么结构
- 禁止某些不合格模式进入对应路径

它回答的是“改这个目录时必须遵守什么”，而不是“由谁来改”。

---

## 3.4 `.claude/docs/` 是共享手册

位置：

- `.claude/docs/`

作用：

- 帮整个工作室建立共同理解
- 提供目录地图、规则总览、协作说明、技术偏好

它不是岗位，也不是规则本身，而是让岗位和规则能被统一理解。

---

## 3.5 `.claude/skills/` 是 SOP

位置：

- `.claude/skills/`

作用：

- 定义一类工作如何被标准化推进
- 定义什么时候该调度多个 Agent
- 定义产出物和确认点

它不是岗位手册，而是流程模板。

---

## 3.6 代码目录结构是工作室地图

关键文件：

- `.claude/docs/directory-structure.md`

关键现实目录：

- `src/`
- `assets/`
- `design/`
- `docs/`
- `tests/`
- `tools/`
- `production/`

没有地图，就没有清晰的职责边界；没有边界，Rules 和 Agents 都会失效。

---

## 4. 组建一个新 AI 工作室的正确顺序

不要先疯狂加 Agent。最稳的顺序是自上而下。

## 4.1 先改总章程

先改：

- `CLAUDE.md`

你要明确：

- 项目类型
- 平台范围
- 主要引擎或框架
- 主要语言
- 构建方式

---

## 4.2 再改技术共识

重点改：

- `.claude/docs/technical-preferences.md`

你要写清：

- 语言和框架
- 命名风格
- 测试框架
- 性能预算
- 禁止模式
- 允许依赖

---

## 4.3 再画代码疆域

重点改：

- `.claude/docs/directory-structure.md`

如果你是多端项目，不要只沿用默认游戏客户端结构。你应该明确加出真实地盘，例如：

- `src/mobile/android/`
- `src/mobile/ios/`
- `src/web/`
- `src/backend/api/`
- `src/backend/services/`
- `src/backend/db/`
- `db/migrations/`

---

## 4.4 再定义岗位体系

重点改：

- `.claude/agents/*.md`
- `.claude/docs/agent-coordination-map.md`
- `.claude/docs/coordination-rules.md`

至少要定清：

- 管理岗是谁
- 工程岗是谁
- 支撑岗是谁
- 谁负责跨端边界
- 谁负责代码评审

---

## 4.5 再为每块地盘配 Rules

重点改：

- `.claude/rules/*.md`
- `.claude/docs/rules-reference.md`

如果你新增了目录，却没新增对应 Rules，你得到的只是“新文件夹”，不是“新制度”。

例如新增这些目录：

- `src/mobile/android/`
- `src/mobile/ios/`
- `src/web/`
- `src/backend/api/`
- `src/backend/services/`
- `src/backend/db/`
- `db/migrations/`

那通常也应新增：

- `.claude/rules/android-code.md`
- `.claude/rules/ios-code.md`
- `.claude/rules/web-ui-code.md`
- `.claude/rules/backend-code.md`
- `.claude/rules/api-code.md`
- `.claude/rules/database-rules.md`

---

## 4.6 最后补流程和守门

重点看：

- `.claude/skills/*`
- `.claude/settings.json`
- `.claude/hooks/*`

这是把工作室从“有结构”推进到“能稳定运行”的关键一步。

---

## 5. 如何约束技术项目经理 Agent 和技术工程师 Agent

## 5.1 技术项目经理 Agent

主要文件：

- `.claude/agents/technical-director.md`
- `.claude/agents/lead-programmer.md`

复杂项目可以新增：

- `.claude/agents/mobile-client-lead.md`
- `.claude/agents/web-frontend-lead.md`
- `.claude/agents/backend-lead.md`

这类 Agent 应重点约束：

1. 先划边界，再拆任务
2. 先给接口草案，再允许进入实现
3. 先说明影响目录，再分派工作
4. 先定义验收标准，再开始交付
5. 跨端问题必须说明客户端、Web、后端的边界

它们的主要职责应是治理、设计、评审、拆解，而不是亲自吞掉大量实现。

---

## 5.2 技术工程师 Agent

现有主要文件：

- `.claude/agents/gameplay-programmer.md`
- `.claude/agents/ui-programmer.md`
- `.claude/agents/engine-programmer.md`
- `.claude/agents/network-programmer.md`
- `.claude/agents/tools-programmer.md`

多端项目建议新增：

- `.claude/agents/android-engineer.md`
- `.claude/agents/ios-engineer.md`
- `.claude/agents/web-frontend-engineer.md`
- `.claude/agents/backend-engineer.md`
- `.claude/agents/api-engineer.md`
- `.claude/agents/database-engineer.md`

这类 Agent 应重点约束：

1. 负责哪些目录
2. 不负责哪些目录
3. 接收哪些输入文档
4. 产出哪些代码和测试
5. 与其他端的交界在哪里

---

## 6. 针对 APP、Web、Backend 应该怎么扩

## 6.1 App 前端如果本质是游戏客户端

当前模板支持较强，主要依赖：

- `.claude/agents/gameplay-programmer.md`
- `.claude/agents/ui-programmer.md`
- `.claude/agents/lead-programmer.md`
- `.claude/agents/technical-director.md`
- `.claude/rules/gameplay-code.md`
- `.claude/rules/ui-code.md`
- `.claude/rules/engine-code.md`
- `.claude/rules/data-files.md`

---

## 6.2 App 前端如果是原生应用

建议新增：

- `.claude/agents/mobile-client-lead.md`
- `.claude/agents/android-engineer.md`
- `.claude/agents/ios-engineer.md`

建议新增目录：

- `src/mobile/android/`
- `src/mobile/ios/`

建议新增规则：

- `.claude/rules/android-code.md`
- `.claude/rules/ios-code.md`

---

## 6.3 Web 前端

建议新增：

- `.claude/agents/web-frontend-lead.md`
- `.claude/agents/web-frontend-engineer.md`

建议新增目录：

- `src/web/`

建议新增规则：

- `.claude/rules/web-ui-code.md`

并同步修改：

- `.claude/docs/technical-preferences.md`
- `.claude/docs/directory-structure.md`
- `.claude/docs/rules-reference.md`
- `.claude/docs/agent-coordination-map.md`

---

## 6.4 服务器后端

默认模板只有一部分服务端意识，主要来自：

- `.claude/agents/network-programmer.md`
- `.claude/agents/devops-engineer.md`
- `.claude/agents/security-engineer.md`
- `.claude/rules/network-code.md`

如果要形成完整后端体系，建议新增：

- `.claude/agents/backend-lead.md`
- `.claude/agents/backend-engineer.md`
- `.claude/agents/api-engineer.md`
- `.claude/agents/database-engineer.md`

建议新增目录：

- `src/backend/api/`
- `src/backend/services/`
- `src/backend/db/`
- `db/migrations/`

建议新增规则：

- `.claude/rules/backend-code.md`
- `.claude/rules/api-code.md`
- `.claude/rules/database-rules.md`

---

## 7. 你真正要学会的能力

当你能独立回答下面这些问题时，你就已经具备“组建 AI 工作室”的能力了：

1. 这个项目的总章程写在哪里
2. 这个项目的岗位职责写在哪里
3. 这个项目的共享技术共识写在哪里
4. 这个项目的代码地盘画在哪里
5. 这个项目的路径规则写在哪里
6. 这个项目的标准流程写在哪里
7. 多端扩展时，应该先改章程、还是先改 Agent、还是先改 Rules

正确答案通常是：

> 先改总章程和技术共识，再改目录地图，再改岗位，再改 Rules，再补流程和守门。

---

## 8. 一份可执行的建室清单

下次为新项目搭建专用 AI 工作室时，你可以按这个顺序执行：

1. 修改 `CLAUDE.md`
2. 修改 `.claude/docs/technical-preferences.md`
3. 修改 `.claude/docs/directory-structure.md`
4. 修改 `.claude/agents/*.md`
5. 修改 `.claude/docs/agent-coordination-map.md`
6. 修改 `.claude/docs/coordination-rules.md`
7. 新增或修改 `.claude/rules/*.md`
8. 修改 `.claude/docs/rules-reference.md`
9. 新增或修改 `.claude/skills/*`
10. 最后补 `.claude/settings.json` 与 `.claude/hooks/*`

这 10 步走完，才算真正从这个母模板里分化出了一个新项目专用的 AI 工作室。
