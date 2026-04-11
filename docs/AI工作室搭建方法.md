# 技术Agent规则

## 1. 这份文档在整个项目中的作用

这份文档不是孤立的“程序员说明”，而是 Claude Code Game Studios 在技术实现维度上的治理文件。

要先明确这个项目的总纲：

> Claude Code Game Studios 的作用，是把一次 Claude Code 会话组织成一个有分工、有层级、有流程、有检查机制的 AI 游戏工作室。

这个模板不是单纯的 Prompt 集合，也不是某个固定技术栈的代码脚手架。它本质上是一个：

- 可协作的 AI 工作室模板
- 可按项目类型定制的研发组织框架
- 可通过 Agent、Rules、Skills、Hooks、Templates 共同约束研发质量的系统

因此，这份《技术Agent规则》要解决的不是“哪个 Agent 会写代码”这么简单，而是以下 5 个问题：

1. 技术实现相关的 Agent 在整个工作室体系中处于什么位置
2. 哪些 Agent 负责技术管理，哪些 Agent 负责编码落地
3. 这些 Agent 分别受哪些规则和文档约束
4. 对 APP 前端、Web 前端、服务器后端的研发需求，这套模板当前能覆盖到什么程度
5. 如果项目技术栈不同，应该修改哪里，才能把这个通用模板变成专用 AI 项目工作室

---

## 2. 这个模板的技术治理总结构

从技术视角看，这个模板不是“一个程序员 Agent 写所有代码”，而是分成四层。

## 2.1 技术治理层

- `technical-director`

作用：

- 负责技术总方向
- 负责技术架构原则
- 负责技术边界与系统分层
- 负责高风险技术决策
- 负责程序、网络、安全、部署之间的冲突仲裁

它更像 CTO / 首席技术负责人，而不是具体编码人员。

---

## 2.2 技术管理层

- `lead-programmer`

作用：

- 把设计转成实现结构
- 拆任务、定模块边界、定 API
- 做代码审查和重构策略
- 把工作分发给具体实现 Agent

它更像技术项目经理 + 技术负责人结合体，是最重要的“技术执行总协调者”。

---

## 2.3 技术实现层

主要包括：

- `gameplay-programmer`
- `engine-programmer`
- `ai-programmer`
- `network-programmer`
- `tools-programmer`
- `ui-programmer`

这些 Agent 负责写具体代码，但应在：

- 已有设计输入
- 已有架构方向
- 已有边界约束

的前提下进行实现。

---

## 2.4 技术支撑层

主要包括：

- `devops-engineer`
- `security-engineer`
- `performance-analyst`
- `analytics-engineer`
- 各引擎专属专家 Agent

这些 Agent 往往不是直接完成玩法功能，而是保证：

- 代码可构建
- 可部署
- 可测试
- 可分析
- 可控风险

---

## 3. 技术实现类 Agent 清单与职责边界

## 3.1 技术管理型 Agent

### `technical-director`

文件：

- `.claude/agents/technical-director.md`

适合负责：

- 技术选型
- 架构原则
- 性能预算
- 客户端 / 服务端 / 网络边界划分
- 高风险技术路线判断

不适合直接负责：

- 具体业务功能编码
- 普通页面实现
- 单个接口细节开发

### `lead-programmer`

文件：

- `.claude/agents/lead-programmer.md`

适合负责：

- 模块结构
- API 设计
- 编码任务拆分
- 代码评审
- 工程重构路线

这是整个模板里最关键的“技术项目经理型 Agent”。

如果你在一个项目里要新增：

- Android 客户端小组
- iOS 客户端小组
- Web 前端小组
- 服务端小组

那么 `lead-programmer` 通常要么继续承担总技术协调角色，要么被你扩展成更明确的“总研发技术经理 Agent”。

---

## 3.2 玩法与客户端实现 Agent

### `gameplay-programmer`

文件：

- `.claude/agents/gameplay-programmer.md`

适合负责：

- 核心玩法逻辑
- 状态机
- 规则实现
- 客户端交互背后的业务逻辑

对三消项目非常关键，通常负责：

- 棋盘状态
- 交换
- 匹配
- 消除
- 掉落
- 补位
- 连锁
- 回合与目标判定

### `ui-programmer`

文件：

- `.claude/agents/ui-programmer.md`

适合负责：

- 游戏内 UI
- HUD
- 菜单页
- 设置页
- 结算页
- 本地化支持的界面层

重要边界：

- 它偏“游戏 UI 程序”
- 不等于传统 Web 前端工程 Agent

### `engine-programmer`

文件：

- `.claude/agents/engine-programmer.md`

适合负责：

- 公共运行时基础设施
- 核心工具层
- 性能关键系统
- 引擎层 API 封装

---

## 3.3 网络与服务相关 Agent

### `network-programmer`

文件：

- `.claude/agents/network-programmer.md`

适合负责：

- 联机架构
- 状态同步
- 客户端预测与服务端校正
- 匹配和大厅逻辑
- 网络协议

它覆盖的是“游戏网络层”。

它并不天然等于：

- 通用后端服务开发
- 账号系统后端
- 活动中心后端
- 管理后台后端

### `devops-engineer`

文件：

- `.claude/agents/devops-engineer.md`

适合负责：

- CI/CD
- 构建
- 环境
- 部署
- 发布流程

它更像平台支撑，不是业务后端开发。

### `security-engineer`

文件：

- `.claude/agents/security-engineer.md`

适合负责：

- 网络安全
- 鉴权与 session 安全
- 反作弊
- 数据安全
- 隐私合规

它可以约束服务端安全实现，但不是完整后端业务实现 Agent。

### `analytics-engineer`

文件：

- `.claude/agents/analytics-engineer.md`

适合负责：

- 事件埋点
- 数据管线
- 指标设计
- A/B 测试设计

---

## 3.4 引擎专属技术 Agent

当前模板原生支持三大游戏引擎：

- Godot
- Unity
- Unreal

相关文件位于：

- `.claude/agents/godot-*.md`
- `.claude/agents/unity-*.md`
- `.claude/agents/unreal-*.md`

这些 Agent 的作用是：

- 把通用技术 Agent 的实现约束，落到具体引擎生态
- 约束 API 用法
- 约束性能实践
- 约束 UI、渲染、构建等引擎相关实现细节

---

## 4. 技术规则文件在哪些地方

技术实现相关规则主要分布在三层：

1. 总规范文档
2. 路径规则文档
3. Agent 定义本身

---

## 4.1 总规范文档

### `CLAUDE.md`

位置：

- `CLAUDE.md`

作用：

- 整个项目的总入口
- 声明技术栈
- 引用技术偏好、上下文管理、协作规则等文档

它不适合写过多细则，但它决定“项目技术治理入口”。

### `coding-standards.md`

位置：

- `.claude/docs/coding-standards.md`

作用：

- 游戏代码的总约束

关键内容：

- 公共 API 注释
- 可测试性
- 数值数据驱动
- ADR / 技术文档要求
- Verification-driven development

### `rules-reference.md`

位置：

- `.claude/docs/rules-reference.md`

作用：

- 汇总所有路径规则

### `technical-preferences.md`

位置：

- `.claude/docs/technical-preferences.md`

作用：

- 项目级技术偏好
- 语言、框架、命名方式、性能预算、测试方式

这是你在不同项目间最应该修改的技术文件之一。

---

## 4.2 路径规则文档

### `gameplay-code.md`

位置：

- `.claude/rules/gameplay-code.md`

路径：

- `src/gameplay/**`

适合：

- 客户端玩法逻辑

### `engine-code.md`

位置：

- `.claude/rules/engine-code.md`

路径：

- `src/core/**`

适合：

- 核心系统、底层框架、性能敏感模块

### `ai-code.md`

位置：

- `.claude/rules/ai-code.md`

路径：

- `src/ai/**`

适合：

- AI 逻辑与求解系统

### `network-code.md`

位置：

- `.claude/rules/network-code.md`

路径：

- `src/networking/**`

适合：

- 联机、同步、游戏网络层

### `ui-code.md`

位置：

- `.claude/rules/ui-code.md`

路径：

- `src/ui/**`

适合：

- 游戏内 UI

### `data-files.md`

位置：

- `.claude/rules/data-files.md`

路径：

- `assets/data/**`

适合：

- 配置与数据驱动内容

### `test-standards.md`

位置：

- `.claude/rules/test-standards.md`

路径：

- `tests/**`

适合：

- 技术验证、自动化测试、回归测试

### `prototype-code.md`

位置：

- `.claude/rules/prototype-code.md`

路径：

- `prototypes/**`

适合：

- 快速原型验证

---

## 4.3 Agent 定义本身也是规则来源

这一点很重要。

除了 `.claude/rules/` 中的显式规则，Agent 本身的定义也是约束来源。

例如：

- `gameplay-programmer.md`
  - 要求玩法代码可测试、数值数据驱动、不能直接依赖 UI
- `network-programmer.md`
  - 要求 server authoritative、消息版本化、处理重连
- `devops-engineer.md`
  - 要求环境、构建、部署流程规范化
- `security-engineer.md`
  - 要求服务端验证输入、使用 TLS、处理 token/session

所以真正的约束来源其实是：

> Agent 行为规则 + 路径代码规则 + 项目总规范

三者共同生效。

---

## 5. 技术项目经理 Agent 和技术工程师 Agent 如何约束

你特别问到了：

- APP 前端（Android + iOS）
- Web 前端
- 服务器后端

的研发需求下，技术项目经理 Agent 和技术工程师 Agent 如何约束。

这部分建议你分成两类来约束。

## 5.1 技术项目经理类 Agent

这类 Agent 指的是：

- `technical-director`
- `lead-programmer`
- 在必要时扩展出来的移动端负责人 / Web 技术负责人 / 后端技术负责人

它们应该被约束在“管理、设计、评审、拆解”范围内，而不是越位写大量业务实现。

### 应增加的约束方向

建议它们必须明确：

1. 先定模块边界，再分派任务
2. 先定技术选型，再讨论实现细节
3. 先给 API / 数据结构草案，再进入代码编写
4. 先定义验收标准，再开始交付
5. 对跨端问题必须说明客户端 / Web / 服务端的边界

### 你应该修改哪些地方

#### A. `lead-programmer.md`

建议根据项目类型补充：

- 负责哪些平台
- 是否统管 Android/iOS/Web/Backend 全部技术线
- 是否允许它直接写代码，还是只做技术拆解和评审

#### B. `technical-director.md`

建议补充：

- 当前项目的架构治理范围
- 是否多端并行
- 是否有服务端
- 各端如何划边界

#### C. `agent-coordination-map.md`

建议在这里补出更细的技术协作图，尤其是：

- APP 客户端负责人
- Web 前端负责人
- 后端负责人
- DevOps / Security 的支撑路径

---

## 5.2 技术工程师类 Agent

这类 Agent 指的是：

- `gameplay-programmer`
- `ui-programmer`
- `network-programmer`
- `tools-programmer`
- `engine-programmer`
- 未来新增的 `backend-engineer`、`web-frontend-engineer`、`mobile-client-engineer`

它们应被约束在“自己负责的代码域”内。

### 应增加的约束方向

它们必须明确：

1. 只能在自己负责的目录工作
2. 不能擅自越过设计文档和架构约束
3. 不能跳过测试与验收标准
4. 需要对跨端依赖显式说明
5. 需要在实现前说明影响文件范围

### 你应该修改哪些地方

#### A. 具体 Agent 文件

例如：

- `ui-programmer.md`
- `network-programmer.md`
- `gameplay-programmer.md`

在这些文件中补：

- 负责目录
- 不负责目录
- 与其他端的交界点
- 数据流输入输出

#### B. 路径规则文件

如果新增 Web / Backend / Mobile 独立目录，一定要同步新增 rules。

#### C. `directory-structure.md`

如果项目分成：

- `src/mobile/`
- `src/web/`
- `src/backend/`

必须先更新目录结构文档，让整套模板知道这些目录是正式结构的一部分。

---

## 6. 针对 APP 前端、Web 前端、服务器后端，模板当前支持到什么程度

## 6.1 APP 前端（Android + iOS）

这里要分两种情况。

### 情况 A：APP 前端就是游戏客户端

如果你的 Android/iOS APP 本身就是游戏客户端，那么这个模板支持很强。

现有支持来源：

- `gameplay-programmer`
- `ui-programmer`
- 引擎专属 Agent
- `lead-programmer`
- `technical-director`
- `gameplay-code.md`
- `ui-code.md`
- `engine-code.md`
- `data-files.md`

结论：

> 对游戏客户端型 APP，这个模板原生支持较强。

### 情况 B：APP 前端是原生业务容器

如果你的 APP 更像：

- 原生壳
- 商城容器
- 活动页容器
- 社区 / 账号中心容器

那当前模板支持不够直接，因为它没有：

- Android 专属 Agent
- iOS 专属 Agent
- Kotlin / Swift / Flutter 专门规则

这时候你应该新增：

- `mobile-client-lead`
- `android-engineer`
- `ios-engineer`

并增加：

- `mobile-code.md`
- `android-code.md`
- `ios-code.md`

---

## 6.2 Web 前端

当前模板没有独立的 Web 工程前端体系。

现有可借用能力：

- `ui-programmer`
- `ux-designer`
- `lead-programmer`
- `devops-engineer`

但缺失：

- Web 前端 Agent
- Web 目录规则
- React / Vue / Next / SSR / 状态管理约束

如果你的项目需要 Web 前端，建议新增：

- `web-frontend-lead`
- `web-frontend-engineer`

并新增规则：

- `web-ui-code.md` 对应 `src/web/**`

建议规则至少包含：

- 组件边界
- 状态管理方式
- API 调用约束
- 路由约束
- 表单校验
- 可访问性
- 国际化
- 打包与性能约束

---

## 6.3 服务器后端

当前模板有“网络、安全、部署意识”，但没有完整后端工程体系。

### 当前已有的部分

- `network-programmer`
- `security-engineer`
- `devops-engineer`
- `network-code.md`

### 当前缺失的部分

- 通用业务服务 Agent
- API 规则
- 数据库规则
- 配置中心和 secret 管理规则
- 缓存 / MQ / 定时任务等服务端规范

如果你的项目需要完整后端，建议新增：

- `backend-lead`
- `backend-engineer`
- `api-engineer`
- `database-engineer`

并新增目录和规则：

- `src/backend/**`
- `src/backend/api/**`
- `src/backend/services/**`
- `src/backend/db/**`
- `db/migrations/**`

---

## 7. 如果技术语言、框架、约束点不同，应该修改哪些地方

这是最关键的项目定制问题。

不同 APP 项目或不同游戏项目，往往会在这几个方面发生变化：

- 编程语言
- 引擎或框架
- 架构形态
- 客户端 / Web / 后端的划分
- 团队组织方式
- 约束重点

要让模板真正适合你的项目，通常需要修改以下地方。

## 7.1 必改项

### A. `CLAUDE.md`

修改内容：

- Engine
- Language
- Build System
- Asset Pipeline

这是项目总入口，必须和你的技术现实一致。

### B. `.claude/docs/technical-preferences.md`

修改内容：

- 语言
- 命名风格
- 框架偏好
- 性能预算
- 测试框架
- 禁止模式

这是项目级技术偏好的主文件。

### C. `.claude/docs/directory-structure.md`

修改内容：

- 目录结构
- 新增 `src/mobile/`、`src/web/`、`src/backend/` 等目录

### D. `.claude/docs/rules-reference.md`

修改内容：

- 新增规则与路径的映射说明

### E. `.claude/rules/*.md`

修改内容：

- 为新目录增加新规则

### F. `.claude/agents/*.md`

修改内容：

- 修改现有 Agent 的职责范围
- 新增你项目专属 Agent

---

## 7.2 常见项目的具体改法

### 项目类型 A：Godot 三消手游

建议重点修改：

- `CLAUDE.md`
- `technical-preferences.md`
- `gameplay-programmer.md`
- `ui-programmer.md`
- `godot-specialist.md`
- `assets/data/**` 规则

### 项目类型 B：Unity 跨平台 APP 游戏

建议重点修改：

- `CLAUDE.md`
- `technical-preferences.md`
- `unity-specialist.md`
- `unity-ui-specialist.md`
- 可能新增移动端相关 Agent

### 项目类型 C：带 Web 官网和运营后台的游戏项目

建议重点修改：

- `directory-structure.md`
- 新增 `src/web/**`
- 新增 Web 前端 Agent
- 新增 Web 前端规则

### 项目类型 D：带完整服务端的商业化游戏

建议重点修改：

- `directory-structure.md`
- 新增 `src/backend/**`
- 新增后端 Agent
- 新增后端规则
- 强化 `devops-engineer` 和 `security-engineer` 的职责边界

---

## 8. 如何让这个模板变成“专用 AI 项目工作室”

你希望的是：

> 针对不同 APP 项目或不同游戏项目，像公司立项一样，基于这个模板创建一个项目专属 AI 工作室。

这是完全符合这个模板设计方向的。

正确做法不是把所有能力都塞进一个通用模板，而是按项目复制一个新工作室，再做项目级定制。

## 8.1 推荐做法：每个新项目创建一个专用工作室副本

流程建议：

1. 复制这个模板为一个新项目仓库
2. 确认项目类型
3. 确认平台范围
4. 确认技术栈
5. 调整 Agent
6. 调整 Rules
7. 调整 Directory Structure
8. 调整 Skills 和 Templates

最后形成：

- 项目专属 Agent 组织
- 项目专属技术规则
- 项目专属工作流
- 项目专属上下文和知识库

这就相当于：

> 为该项目新建一个 AI 工作室。

## 8.2 一个专用 AI 项目工作室应该包含什么

至少要定制这 6 项：

1. **项目总纲**
   - `CLAUDE.md`
2. **技术偏好**
   - `.claude/docs/technical-preferences.md`
3. **目录结构**
   - `.claude/docs/directory-structure.md`
4. **Agent 组织**
   - `.claude/agents/*.md`
5. **规则体系**
   - `.claude/rules/*.md`
6. **技能工作流**
   - `.claude/skills/*.md`

## 8.3 什么情况下应该新建专用 Agent

当你的项目出现以下情况之一时，建议新建专用 Agent：

- 现有 Agent 无法准确覆盖平台职责
- 同一平台有独立工程要求
- 现有 Agent 的边界过于宽泛
- 某类工程在项目中是主战场

例如：

- Android / iOS 原生并行开发
- Web 前端是运营主阵地
- 后端是活动、账号、支付的核心系统

这时就不该只靠：

- `ui-programmer`
- `network-programmer`
- `devops-engineer`

去硬扛全部职责。

---

## 9. 对你当前需求的直接建议

如果你的目标是让这个模板适配：

- APP 前端（Android + iOS）
- Web 前端
- 服务器后端

我建议你按下面的思路扩展。

## 9.1 保留现有角色

保留：

- `technical-director`
- `lead-programmer`
- `gameplay-programmer`
- `ui-programmer`
- `network-programmer`
- `devops-engineer`
- `security-engineer`

## 9.2 新增项目专属角色

建议新增：

- `mobile-client-lead`
- `android-engineer`
- `ios-engineer`
- `web-frontend-lead`
- `web-frontend-engineer`
- `backend-lead`
- `backend-engineer`
- `api-engineer`
- `database-engineer`

## 9.3 新增项目专属规则

建议新增：

- `mobile-code.md`
- `android-code.md`
- `ios-code.md`
- `web-ui-code.md`
- `backend-code.md`
- `api-code.md`
- `database-rules.md`

## 9.4 新增项目专属目录

建议至少考虑：

- `src/mobile/android/`
- `src/mobile/ios/`
- `src/web/`
- `src/backend/api/`
- `src/backend/services/`
- `src/backend/db/`
- `db/migrations/`

---

## 10. 结论

这份文档最重要的结论有 4 条：

1. 这个模板本质上是一个可定制的 AI 游戏工作室框架，而不是单一技术栈脚手架
2. 当前模板对游戏客户端、网络、安全、部署支持较好，但对完整 Web 和后端工程支持不足
3. 技术项目经理类 Agent 和技术工程师类 Agent 都需要通过 Agent 定义、路径规则、目录结构和技术偏好文件共同约束
4. 如果你希望它适配不同 APP 项目或不同游戏项目，正确做法是基于模板创建“项目专用 AI 工作室”，而不是一直在一个通用模板里堆规则
