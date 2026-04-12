# Claude Code Game Studios Agent 调度流程

## 1. 先说结论：谁在调度

这个项目里的 Agent 调度，不是“Agent 自己形成自治网络”，而是一个非常明确的三层结构：

1. **用户**
   - 最终决策者
   - 决定方向、批准写文件、批准进入下一阶段
2. **主会话 / 当前技能**
   - 真正的调度者
   - 负责读取上下文、选择要调用的 Agent、汇总结果、向用户提问
3. **子 Agent**
   - 专业执行者
   - 提供分析、方案、实现或验证结果

一句话理解：

> 真正做“编排”的通常不是某个部门 Agent，而是当前运行的主会话或 `team-*` / `design-system` 这类技能。

---

## 2. 调度结构总览

项目中定义了三层 Agent 体系。

## 2.1 第一层：主管理层 Agent

这三位是最高层：

- `creative-director`
  - 管创意方向、游戏气质、跨设计冲突
- `technical-director`
  - 管技术方向、架构原则、技术冲突
- `producer`
  - 管排期、跨部门协作、风险与范围

其中最像“总调度中心”的是：

> `producer`

因为它的定义明确写着：它是主要协调 Agent，负责跨部门同步、任务分配、范围与进度管理。

但要注意：

- `creative-director` 和 `technical-director` 负责“高层决策”
- `producer` 负责“项目推进与跨部门协调”

所以“主管理 Agent”不是只有一个，而是三个不同维度：

- 创意总管：`creative-director`
- 技术总管：`technical-director`
- 生产总管：`producer`

---

## 2.2 第二层：部门管理层 Agent

这些 Agent 是各部门负责人：

- `game-designer`
- `lead-programmer`
- `art-director`
- `audio-director`
- `narrative-director`
- `qa-lead`
- `release-manager`
- `localization-lead`

它们负责：

- 接收上层目标
- 在本部门内拆分任务
- 调用本部门子 Agent
- 在本领域内给出初步判断
- 把冲突上报给上层

例如：

- `game-designer` 下面对接 `systems-designer`、`level-designer`、`economy-designer`
- `lead-programmer` 下面对接 `gameplay-programmer`、`engine-programmer`、`ai-programmer`、`network-programmer`、`tools-programmer`、`ui-programmer`
- `qa-lead` 对接 `qa-tester`

---

## 2.3 第三层：部门内子 Agent

这些是执行层专家，例如：

- 设计侧
  - `systems-designer`
  - `level-designer`
  - `economy-designer`
- 程序侧
  - `gameplay-programmer`
  - `engine-programmer`
  - `ai-programmer`
  - `network-programmer`
  - `ui-programmer`
  - `tools-programmer`
- 内容与表现侧
  - `technical-artist`
  - `sound-designer`
  - `writer`
  - `world-builder`
  - `ux-designer`
- 质量与支撑侧
  - `qa-tester`
  - `performance-analyst`
  - `devops-engineer`
  - `analytics-engineer`
  - `security-engineer`
  - `accessibility-specialist`
  - `community-manager`
  - `live-ops-designer`

它们通常不负责最终拍板，而是：

- 做专业分析
- 给出方案
- 产出具体实现
- 回传结果给主会话或部门负责人

---

## 3. 引擎专属 Agent 也遵循同样层级

除了通用部门体系，项目还提供三套引擎专家链路：

### Godot

- `godot-specialist`
- `godot-gdscript-specialist`
- `godot-shader-specialist`
- `godot-gdextension-specialist`

### Unity

- `unity-specialist`
- `unity-dots-specialist`
- `unity-shader-specialist`
- `unity-addressables-specialist`
- `unity-ui-specialist`

### Unreal

- `unreal-specialist`
- `ue-gas-specialist`
- `ue-blueprint-specialist`
- `ue-replication-specialist`
- `ue-umg-specialist`

调度逻辑一样：

- 引擎总专家负责引擎层面总判断
- 子专家负责某个子系统

---

## 4. 真正的调度规则是什么

`.claude/docs/coordination-rules.md` 和 `.claude/docs/agent-coordination-map.md` 给出了核心规则。

## 4.1 垂直委派

规则：

> Directors -> Leads -> Specialists

含义：

- 高层给方向
- 部门负责人拆分任务
- 专家负责落地

复杂任务不应跳层处理。

例如：

- 不应让 `creative-director` 直接写着色器
- 不应让 `qa-tester` 自己决定产品范围

## 4.2 同层可咨询，不可越权

同层 Agent 可以交流，但不能跨自己领域做“绑定性决策”。

例如：

- `game-designer` 可以咨询 `lead-programmer`
- 但不能替 `lead-programmer` 决定代码架构

## 4.3 冲突向上升级

通用规则：

- 设计冲突 -> `creative-director`
- 技术冲突 -> `technical-director`
- 排期/资源/跨部门冲突 -> `producer`

这意味着：

> 子 Agent 的职责不是争出最后赢家，而是把冲突提炼清楚，再交给上层决策。

## 4.4 跨部门变更由 Producer 传播

如果一个改动同时影响多个部门，`producer` 负责变更传播。

例如：

- GDD 变化影响程序、QA、排期
- 架构决策影响多个程序子系统和测试策略

这也是为什么 `producer` 是最核心的横向调度节点。

## 4.5 禁止擅自跨域改动

规则明确要求：

> Agent 不得在没有明确委派的情况下修改自己职责之外的文件。

这保证：

- 设计 Agent 不乱改代码
- 程序 Agent 不乱改叙事
- 子 Agent 不越界破坏其他模块

---

## 5. 主会话、主管理 Agent、部门 Agent、子 Agent 如何配合

这里是最关键的理解。

## 5.1 主会话不是普通对话窗口，而是“总编排器”

在这个模板里，很多技能文件都明确规定：

- 主会话先读上下文
- 主会话决定调用哪个子 Agent
- 子 Agent 返回分析
- 主会话把分析变成用户可选项
- 用户决定
- 主会话才写文件或进入下一阶段

所以一个完整调度链路常常是：

`用户 -> 主会话/技能 -> 子 Agent -> 主会话汇总 -> 用户决策 -> 主会话执行`

不是：

`用户 -> 子 Agent 自主做完全部`

---

## 5.2 `design-system` 的调度方式

`/design-system` 是一个非常典型的“主会话调度”技能。

它会：

1. 读取游戏概念、systems-index、依赖 GDD
2. 汇总当前要设计的系统上下文
3. 先创建骨架文件
4. 按章节循环：
   - 提问
   - 给方案
   - 用户选择
   - 起草
   - 用户批准
   - 写入该章节
5. 对复杂章节调用专家 Agent
6. 设计完成后更新 systems-index

在这个流程里：

- 主会话是调度者
- `game-designer` / `systems-designer` 等只是被调用的专家
- 文件写入权掌握在主会话手里

这点在 `design-system` 文档里写得非常明确：

> Agents do NOT write to files directly — the main session owns all file writes.

---

## 5.3 `team-*` 技能的调度方式

`/team-combat`、`/team-ui`、`/team-release` 这类技能，是多 Agent 编排的典型。

它们共享同样模式：

### 第一阶段：主会话定义流水线

例如 `/team-combat` 明确分成：

1. Design
2. Architecture
3. Implementation
4. Integration
5. Validation
6. Sign-off

### 第二阶段：主会话按阶段调子 Agent

例如 `/team-combat` 中：

- 设计阶段 -> `game-designer`
- 架构阶段 -> `gameplay-programmer`，必要时加 `ai-programmer`
- 实现阶段并行 -> `gameplay-programmer`、`ai-programmer`、`technical-artist`、`sound-designer`
- 验证阶段 -> `qa-tester`

### 第三阶段：主会话在阶段交接点向用户确认

技能定义里要求：

- 每个阶段切换时都要用结构化提问
- 子 Agent 的完整分析先展示
- 再由用户决定是否进入下一阶段

### 第四阶段：主会话负责集成与总结

也就是说：

- 子 Agent 负责各自专业段落
- 主会话负责串联
- 用户控制关键节点

---

## 6. “主管理 Agent”到底是谁

如果你问“整个系统里谁是总管理 Agent”，答案需要分场景看。

## 6.1 在创意方向问题上

主管理 Agent 是：

- `creative-director`

它处理：

- 游戏定位
- 体验目标
- 气质、风格、叙事与玩法的创意冲突

## 6.2 在技术架构问题上

主管理 Agent 是：

- `technical-director`

它处理：

- 架构路线
- 技术栈
- 性能方向
- 技术争议升级

## 6.3 在项目推进和跨部门协作上

主管理 Agent 是：

- `producer`

它处理：

- 排期
- 里程碑
- 范围管理
- 风险
- 跨部门同步

如果从“最像项目经理和总调度器”的角度说，确实是 `producer`。
但从“整个工作室顶层治理”的角度，应该理解成三位共同组成的主管理层。

---

## 7. 部门管理层是怎么调度子 Agent 的

这里可以分几个最典型的部门来看。

## 7.1 设计部门

核心链路：

- `creative-director`
  - 决定创意方向
- `game-designer`
  - 负责机制与系统设计
- `systems-designer` / `level-designer` / `economy-designer`
  - 负责细化子系统、关卡、经济模型

调度方式：

1. 先由 `game-designer` 把需求抽象成系统问题
2. 再按子领域调用专家
3. 冲突或方向问题再升级给 `creative-director`

## 7.2 程序部门

核心链路：

- `technical-director`
  - 技术方向与冲突仲裁
- `lead-programmer`
  - 代码架构、代码审查、任务拆分
- `gameplay-programmer` / `engine-programmer` / `ai-programmer` / `network-programmer` / `ui-programmer` / `tools-programmer`
  - 各领域落地

调度方式：

1. `lead-programmer` 先把设计转成代码结构
2. 把不同模块分配给对应程序子 Agent
3. 如果设计与技术冲突，就同步 `game-designer` 与上层
4. 重大问题再升级给 `technical-director`

## 7.3 QA 部门

核心链路：

- `qa-lead`
- `qa-tester`

调度方式：

1. `qa-lead` 定测试策略
2. `qa-tester` 写测试、跑验证、提 bug
3. 发版阶段由 `qa-lead` 做质量门禁

## 7.4 发布部门

核心链路：

- `release-manager`
- `devops-engineer`
- `qa-lead`
- `producer`

调度方式：

1. `release-manager` 准备发布流程
2. `devops-engineer` 构建与部署
3. `qa-lead` 做回归与签收
4. `producer` 做 go/no-go 决策组织

---

## 8. 一个完整的调度示例

下面用“新增一个战斗功能”举例。

## 8.1 用户发起

用户执行：

`/team-combat "grappling hook ability"`

## 8.2 主会话建立流程

主会话识别这属于战斗团队工作流，于是进入 `team-combat` 技能定义的流水线。

## 8.3 设计阶段

主会话调用：

- `game-designer`

产出：

- 机制方案
- 公式
- 边界情况

然后主会话：

- 把方案解释给用户
- 让用户选方向

## 8.4 架构阶段

主会话调用：

- `gameplay-programmer`
- 如有 AI 交互，再加 `ai-programmer`

产出：

- 类结构
- 文件列表
- 接口设计

然后主会话：

- 再次向用户确认

## 8.5 并行实现阶段

主会话并行调用：

- `gameplay-programmer`
- `ai-programmer`
- `technical-artist`
- `sound-designer`

每个 Agent 只负责自己的部分。

## 8.6 集成与验证阶段

主会话整合实现结果，然后调用：

- `qa-tester`

验证：

- 功能
- 边界情况
- 性能影响

## 8.7 最终签收

主会话汇总：

- 哪些完成
- 哪些待修
- 当前状态 COMPLETE / NEEDS WORK / BLOCKED

最后再给用户决定下一步。

---

## 9. 为什么这个项目要用这种调度方式

原因有四个。

## 9.1 防止单 Agent 越权

一个 Agent 很容易：

- 既做设计又做实现又做测试
- 没有人检查它的假设

而分层调度能强制：

- 先设计
- 再架构
- 再实现
- 再验证

## 9.2 让复杂任务能并行

例如战斗功能通常同时涉及：

- 程序
- AI
- 特效
- 音效
- QA

如果没有调度层，就很难并行组织。

## 9.3 让用户保留控制权

所有关键节点都要经过：

- 解释
- 选项
- 用户决定
- 批准写入

所以它不是黑盒式自动生成。

## 9.4 让中断后还能恢复

通过：

- `production/session-state/active.md`
- `session-start.sh`
- `pre-compact.sh`

系统可以恢复到上次设计到哪一章、哪个系统、哪个任务。

---

## 10. 调度过程中有哪些自动辅助机制

## 10.1 SubagentStart 审计

`log-agent.sh` 会在子 Agent 启动时记录：

- 时间戳
- Agent 名称

写入：

- `production/session-logs/agent-audit.log`

这意味着项目有基础审计链。

## 10.2 SessionStart 恢复上下文

`session-start.sh` 会在启动时显示：

- 当前分支
- 最近提交
- sprint / milestone
- bug 数
- 活跃会话状态摘要

它帮助主会话重新进入正确上下文。

## 10.3 detect-gaps 主动提醒缺口

`detect-gaps.sh` 会在启动时提示：

- 新项目应先跑 `/start`
- 有大量源码但缺文档
- 有原型但没说明
- 有核心系统但缺架构文档

它本质上是在给调度器提供“下一步建议”。

---

## 11. 新会话交接规则在调度链中的位置

这条新规则不只是“聊天建议”，它在整个调度体系里其实承担的是：

> 主会话的收口、切换和交接协议。

也就是说，它主要作用于这条链路：

`用户 -> 主会话/技能 -> 子 Agent -> 主会话汇总 -> 用户决策 -> 新会话交接`

### 11.1 为什么主要由主会话执行

虽然很多分析来自子 Agent，但真正应该判断“是否该开启新会话”的，通常仍然是主会话。

原因有三个：

1. 主会话掌握完整阶段状态
   - 知道当前任务是否完成
   - 知道是否发生话题漂移
   - 知道是否已经切换了工作流
2. 子 Agent 通常只看到自己局部任务
   - 它知道自己这段分析是否结束
   - 但未必知道整个会话是否该切换
3. 交接提示需要面向“下一步总任务”
   - 这通常不是子 Agent 能单独定义的
   - 而是由主会话结合当前阶段和下一阶段来生成

所以最准确的理解是：

> 子 Agent 负责完成本段专业工作，主会话负责判断何时收口并给出下一轮对话入口。

### 11.2 它在 team-* 技能中的位置

在 `team-combat`、`team-ui`、`team-release` 这类多 Agent 编排技能里，这条规则最适合出现在两个位置：

#### 位置 A：整个流水线完成后

例如：

- 战斗功能设计、实现、验证都完成了
- 主会话已经能给出 COMPLETE / NEEDS WORK / BLOCKED 总结

这时主会话应判断：

- 当前任务已经闭环
- 下一个任务是否属于不同阶段

如果是，就应该建议新会话继续，例如：

- 从功能实现转入 Sprint 规划
- 从 UI 功能转入 QA 验证
- 从发版准备转入上线监控

#### 位置 B：中途发生明显工作流切换时

例如：

- 当前在 `/team-combat` 中做战斗功能
- 用户突然想切去讨论商业化路线
- 或从实现细节跳到版本排期

这类变化已经不是当前 team skill 的自然下一步，就适合主会话主动建议：

- 当前会话先收尾
- 给一个新的开场 prompt
- 在新对话里切换到 `producer`、`creative-director` 或其他更合适的角色

### 11.3 它在 design-system 这类文档流中的位置

对 `/design-system` 这种按章节推进的技能，这条规则也很有用，但触发点更明确：

#### 适合继续当前会话

- 还在同一份 GDD 内
- 只是从一个章节进入下一个章节
- 或还在修同一轮 review 问题

#### 适合建议新会话

- 当前系统 GDD 已完成并已 review
- 下一步要切到另一个系统
- 或要从设计切换到原型实现
- 或要从设计切换到 Sprint 计划

例如：

- 当前完成 `board-system`
- 下一步要做 `match-resolution-system`

如果上下文仍然清晰，可以继续；
但如果会话已经很长，主会话就应该主动收口，给出一个新开场 prompt，让下一轮只聚焦新系统。

### 11.4 子 Agent 在这个规则里的职责

子 Agent 虽然通常不是最终交接者，但它仍然有两类职责：

#### 职责 A：明确说明“本段任务已完成”

例如：

- `game-designer` 返回：设计阶段完成
- `gameplay-programmer` 返回：架构建议完成
- `qa-tester` 返回：验证结果完成

这会给主会话一个清晰信号：可以考虑收口或切阶段。

#### 职责 B：提示需要角色切换

例如某个子 Agent 可以明确说：

- 这个问题已经超出我的领域
- 下一步更适合交给 `producer`
- 下一步更适合用 `/team-release`

然后由主会话把它转换成真正的新会话交接提示。

也就是说：

> 子 Agent 可以发出“该切换”的信号，但真正输出 handoff prompt 的应是主会话。

### 11.5 新会话交接的标准形式

当主会话判断应结束当前对话并进入下一个任务时，应输出：

```text
[Current task summary]
Start a new conversation for the next task with:
"[Suggested opening prompt with role, task, and prior context summary]"
```

在调度上，这段文本有三个作用：

1. 对当前会话做闭环总结
2. 把下一步任务明确绑定到合适角色或工作流
3. 把最小必要上下文传给下一轮，而不是把整段旧对话拖过去

### 11.6 一个调度视角下的例子

假设当前会话一直在做三消项目的系统设计：

- 已完成 `board-system`
- 已完成 `match-resolution-system`
- 用户现在想开始 Sprint 规划

这时主会话不应该继续在同一条超长对话里直接切到排期，而应输出类似：

```text
The board-system and match-resolution-system GDDs are complete for the match-3 project. The next step is production planning for the prototype milestone.
Start a new conversation for the next task with:
"Act as producer. Help me create Sprint 1 for my Godot 4.6 match-3 project. We already completed the game concept, systems index, board-system GDD, and match-resolution-system GDD. Focus on MVP prototype scope, task breakdown, risks, and a 1-week sprint."
```

这就是它在调度链中的准确位置：

- 不是替代子 Agent
- 不是替代技能编排
- 而是在一个阶段完成后，由主会话把“当前工作”安全交接到“下一轮工作”

---

## 12. 你应该怎样正确使用这些 Agent

## 11.1 把 Agent 当角色，不当命令别名

不要把 Agent 理解成“换个名字的同一个 Claude”。

正确理解是：

- 每个 Agent 有自己的职责边界
- 它会依据定义中的工具权限和工作方式行动
- 你应该把问题交给正确部门

## 11.2 大任务优先用技能，不优先手搓调用顺序

例如：

- 做战斗功能 -> `/team-combat`
- 做 UI 功能 -> `/team-ui`
- 做发布 -> `/team-release`

因为技能已经帮你预定义了合理阶段与 Agent 组合。

## 11.3 遇到跨部门问题，优先找 Producer 视角

如果你不确定谁来协调：

- 先从 `producer` 角度切入，通常最稳妥

因为它负责跨部门同步与范围/风险控制。

## 11.4 遇到争议时，不要跳过升级链

例如：

- 设计和实现意见冲突
- 进度与质量冲突
- 表现和玩法气质冲突

正确做法是按定义升级，而不是让执行层自行决定。

---

## 13. 最后给你的简化记忆法

如果你只记住一套最小模型，可以记这 4 句：

1. **用户拍板**
2. **主会话编排**
3. **主管理层定方向**
4. **部门和子 Agent 分工执行**

再进一步简化成一句：

> 用户负责决策，技能负责调度，Agent 负责专业工作。

这就是 Claude Code Game Studios 的 Agent 调度本质。

---

## 14. 配套阅读

建议和这份文档搭配阅读：

- `docs/mystudy/项目使用说明.md`
- `docs/mystudy/快速上手清单.md`
- `docs/examples/README.md`
