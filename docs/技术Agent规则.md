# 技术 Agent 规则

## 1. 文档目的

这份文档专门说明 Claude Code Game Studios 中与“技术实现”直接相关的 Agent、规则文件、适用边界和协作方式。

它回答 4 个核心问题：

1. 哪些 Agent 负责编码实现
2. 这些 Agent 分别受哪些规则约束
3. 对一个具体项目，例如三消项目，客户端 / Web 前端 / 服务器端分别应该如何理解
4. 当前模板是否已经具备完整的服务器后端实现约束体系

---

## 2. 技术实现的总体分层

这个模板里的技术实现并不是由单一“程序员 Agent”负责，而是按职责分层。

可以把它理解成四层：

### 第 1 层：技术治理层

- `technical-director`
  - 负责技术方向、架构原则、关键技术决策、性能和系统边界

### 第 2 层：技术管理层

- `lead-programmer`
  - 负责代码架构、API 设计、代码审查、任务拆分

### 第 3 层：具体实现层

- `gameplay-programmer`
- `engine-programmer`
- `ai-programmer`
- `network-programmer`
- `tools-programmer`
- `ui-programmer`

### 第 4 层：技术支撑层

- `devops-engineer`
- `security-engineer`
- `performance-analyst`
- `analytics-engineer`
- 各引擎专属专家 Agent

一句话理解：

> `technical-director` 定方向，`lead-programmer` 定结构，具体实现 Agent 写各类代码，支撑类 Agent 负责部署、安全、性能与数据。

---

## 3. 技术 Agent 清单与职责

以下内容基于 `.claude/agents/` 中的定义整理。

## 3.1 `technical-director`

文件：

- `.claude/agents/technical-director.md`

定位：

- 技术治理最高层

职责：

- 技术架构方向
- 系统边界
- 技术选型
- 性能预算
- 当程序、网络、安全、部署产生冲突时做最终技术仲裁

不适合直接承担：

- 具体功能实现
- 具体 UI 编码
- 具体服务器代码细节落地

---

## 3.2 `lead-programmer`

文件：

- `.claude/agents/lead-programmer.md`

定位：

- 技术实现总协调人

职责：

- 把设计文档翻译成代码结构
- 设计模块边界和 API
- 进行代码评审
- 把任务分发给具体程序 Agent
- 识别重构需求

这是最核心的“程序管理层 Agent”。

如果你不知道一个编码任务该先交给谁，通常先从它开始最稳。

---

## 3.3 `gameplay-programmer`

文件：

- `.claude/agents/gameplay-programmer.md`

定位：

- 核心玩法实现 Agent

职责：

- 实现玩家可见、可操作的玩法逻辑
- 把 GDD 转成真正可运行的机制
- 保持数值数据驱动
- 建立玩法状态机
- 和 UI、AI、网络进行玩法层集成

对三消项目来说，它通常负责：

- 棋盘逻辑
- 交换规则
- 匹配检测
- 消除结算
- 掉落补位
- 连锁反应
- 步数和目标的玩法判定

---

## 3.4 `engine-programmer`

文件：

- `.claude/agents/engine-programmer.md`

定位：

- 核心系统和底层基础设施实现 Agent

职责：

- 核心框架
- 引擎层封装
- 性能敏感模块
- 线程、安全、资源管理
- 稳定公共接口

适合处理：

- 基础运行时架构
- 底层系统抽象
- 性能关键路径
- 公共工具接口

不适合处理：

- 具体玩法设计决策

---

## 3.5 `ui-programmer`

文件：

- `.claude/agents/ui-programmer.md`

定位：

- 游戏 UI 实现 Agent

职责：

- 屏幕、菜单、HUD、弹窗等游戏 UI
- 数据展示与绑定
- 本地化支持
- 输入方式适配
- 可访问性相关 UI 实现

对三消项目来说，它适合负责：

- 主界面
- 关卡开始页
- 游戏中 HUD
- 目标显示
- 步数显示
- 结算页
- 设置页

注意：

这个 Agent 更偏“游戏内 UI”，不是典型的 Web 前端工程 Agent。

---

## 3.6 `network-programmer`

文件：

- `.claude/agents/network-programmer.md`

定位：

- 多人联网与同步 Agent

职责：

- client-server / peer-to-peer 架构
- 状态同步
- 消息协议
- 延迟补偿
- 带宽优化
- 匹配和大厅生命周期

它的核心原则非常明确：

- Server authoritative
- Client prediction + reconciliation
- 所有网络消息版本化
- 处理断线重连

注意：

它主要负责“游戏网络层”和“联机同步逻辑”，不是完整的后端业务服务开发 Agent。

---

## 3.7 `tools-programmer`

文件：

- `.claude/agents/tools-programmer.md`

定位：

- 开发工具 Agent

职责：

- 编辑器扩展
- 内容生产工具
- 调试工具
- 自动化脚本
- 数据处理工具

对三消项目来说，适合做：

- 关卡编辑工具
- 棋盘测试工具
- 配置校验工具
- 调试控制台

---

## 3.8 `ai-programmer`

文件：

- `.claude/agents/ai-programmer.md`

定位：

- AI 系统实现 Agent

三消项目通常不优先依赖它，但如果你未来做：

- 对局推荐
- 自动试玩
- 关卡解算器
- AI 对手

它会变得重要。

---

## 3.9 `devops-engineer`

文件：

- `.claude/agents/devops-engineer.md`

定位：

- 构建、CI/CD、环境和部署 Agent

职责：

- Build pipeline
- CI/CD
- Artifact management
- Environment management
- Branching strategy

对服务器相关工作，它目前覆盖的是：

- 环境配置
- 部署流程
- 测试流水线
- 构建与发布自动化

它并不等于后端业务开发。

---

## 3.10 `security-engineer`

文件：

- `.claude/agents/security-engineer.md`

定位：

- 安全治理 Agent

职责：

- 网络安全
- 反作弊
- 数据保护
- 鉴权和 session 安全
- 隐私合规

它对服务器端最接近的覆盖包括：

- 服务端验证客户端输入
- token / session 过期和刷新
- TLS
- replay attack 防护
- 服务端验证关键状态变更

但它仍然不是一个纯后端业务实现 Agent。

---

## 3.11 `performance-analyst`

文件：

- `.claude/agents/performance-analyst.md`

定位：

- 性能分析 Agent

适合用于：

- 客户端性能
- 网络性能
- 构建后回归
- 帧率 / 内存 / 带宽分析

---

## 3.12 `analytics-engineer`

文件：

- `.claude/agents/analytics-engineer.md`

定位：

- 数据埋点与分析 Agent

对三消项目很有用，尤其是：

- 关卡通过率
- 失败点
- 道具使用率
- 留存与行为分析

---

## 4. 技术实现的核心规则文件

规则集中在 `.claude/rules/` 和 `.claude/docs/`。

## 4.1 总体规范

### `coding-standards.md`

文件：

- `.claude/docs/coding-standards.md`

作用：

- 所有游戏代码的总编码规范

关键约束：

- 公共 API 需要文档注释
- 系统应可测试
- 数值应数据驱动，不要硬编码
- 重要系统应有架构文档
- 强调 verification-driven development

---

## 4.2 路径规则总览

### `rules-reference.md`

文件：

- `.claude/docs/rules-reference.md`

作用：

- 汇总所有路径规则和作用范围

这是理解“哪个目录受什么约束”的总入口。

---

## 4.3 玩法规则

### `gameplay-code.md`

文件：

- `.claude/rules/gameplay-code.md`

路径：

- `src/gameplay/**`

适用场景：

- 玩法逻辑实现

对三消项目意味着：

- 棋盘逻辑
- 匹配逻辑
- 消除逻辑
- 回合目标逻辑

通常都应该放在这一类规则下。

---

## 4.4 核心系统规则

### `engine-code.md`

文件：

- `.claude/rules/engine-code.md`

路径：

- `src/core/**`

关键约束：

- 热路径零分配
- 线程安全
- API 稳定性
- 公共接口需要示例
- 引擎代码不反向依赖 gameplay

适用场景：

- 棋盘底层框架
- 通用系统调度
- 高性能工具层

---

## 4.5 AI 规则

### `ai-code.md`

文件：

- `.claude/rules/ai-code.md`

路径：

- `src/ai/**`

适用场景：

- AI 对手
- 自动求解器
- 行为分析逻辑

---

## 4.6 网络规则

### `network-code.md`

文件：

- `.claude/rules/network-code.md`

路径：

- `src/networking/**`

关键约束：

- 服务端权威
- 消息版本化
- 客户端预测与服务端校正
- 断线重连与迁移
- 带宽预算
- 日志限流
- 对包大小和字段范围做安全校验

这是当前模板里最接近“服务端/联机实现规则”的文件。

---

## 4.7 UI 规则

### `ui-code.md`

文件：

- `.claude/rules/ui-code.md`

路径：

- `src/ui/**`

关键约束方向：

- UI 不持有游戏状态
- 文本本地化
- 可访问性
- 输入支持

适合：

- 游戏客户端 UI
- 菜单层
- HUD 层

---

## 4.8 数据规则

### `data-files.md`

文件：

- `.claude/rules/data-files.md`

路径：

- `assets/data/**`

适合：

- 关卡数据
- 配置表
- 数值数据
- 道具定义

对三消项目尤其重要，因为它天然适合走数据驱动。

---

## 4.9 测试规则

### `test-standards.md`

文件：

- `.claude/rules/test-standards.md`

路径：

- `tests/**`

对服务器相关的一个重要信号是：

- 它要求测试尽量不要依赖外部 state
- 明确提到 unit tests 不应依赖 filesystem / network / database

这说明模板更偏向“可隔离测试”的工程方式。

---

## 4.10 原型规则

### `prototype-code.md`

文件：

- `.claude/rules/prototype-code.md`

路径：

- `prototypes/**`

适合三消项目第一阶段快速验证。

---

## 5. 技术 Agent 与规则的对应关系

下面是一个实用映射表。

| Agent | 主要工作目录 | 主要受哪些规则影响 |
|------|--------------|-------------------|
| `lead-programmer` | 跨目录架构协调 | `coding-standards.md` + 各路径规则 |
| `gameplay-programmer` | `src/gameplay/**` | `gameplay-code.md` |
| `engine-programmer` | `src/core/**` | `engine-code.md` |
| `ai-programmer` | `src/ai/**` | `ai-code.md` |
| `network-programmer` | `src/networking/**` | `network-code.md` |
| `ui-programmer` | `src/ui/**` | `ui-code.md` |
| `tools-programmer` | `tools/**` 或工具目录 | 受总规范影响，缺少单独 tools 规则 |
| `devops-engineer` | CI/CD / build / env | 主要靠 Agent 定义与流程，不靠单独 rules 文件 |
| `security-engineer` | 安全审查、网络与鉴权 | 主要靠 Agent 定义 + `network-code.md` |
| `analytics-engineer` | 埋点、事件、数据管线 | 主要靠 Agent 定义，缺少单独 analytics 规则 |

---

## 6. 以三消项目为例：客户端、Web、服务器如何落位

如果你的项目是一个现代三消游戏，技术上通常会有三块：

1. App 前端
2. Web 前端
3. 服务器后端

模板对这三块的覆盖程度并不完全相同。

---

## 6.1 App 前端

这里的“App 前端”主要指游戏客户端本体。

对三消项目来说，模板对这部分支持是最完整的。

### 相关 Agent

- `gameplay-programmer`
- `ui-programmer`
- `lead-programmer`
- `ux-designer`
- 引擎专属 Agent

### 相关规则

- `src/gameplay/**`
- `src/ui/**`
- `src/core/**`
- `assets/data/**`
- `tests/**`

### 适合承载的内容

- 棋盘和消除核心逻辑
- 客户端动画驱动逻辑
- HUD 和菜单
- 关卡内目标显示
- 客户端配置与本地数据

结论：

> 对 App 端三消实现，这套模板是足够强的。

---

## 6.2 Web 前端

这里的“Web 前端”可能有两种情况：

1. Web 版游戏前端
2. 官网 / 活动页 / 运营后台前端

当前模板对这部分支持是“可以借用”，但不是专门设计。

### 可借用的 Agent

- `ui-programmer`
- `ux-designer`
- `lead-programmer`
- `devops-engineer`

### 当前缺口

- 没有专门的 Web 前端 Agent
- 没有 `src/web/**` 的专门规则
- 没有 React/Vue/SSR 之类的专用约束

结论：

> 模板能支持游戏 UI 风格的前端实现，但对典型 Web 工程前端没有独立规则体系。

---

## 6.3 服务器后端

这里的“服务器后端”如果细分，通常又包含：

- 登录与账号
- 活动和配置分发
- 排行榜
- 存档同步
- 支付校验
- 运营后台 API
- 联机服务

当前模板只对其中一部分有明确支持。

### 已覆盖的部分

#### A. 联机 / 网络同步类

由以下内容覆盖：

- `network-programmer`
- `network-code.md`
- `security-engineer`

这部分强调：

- Server authoritative
- 协议版本化
- 预测与回滚
- 安全校验

#### B. 部署与环境类

由以下内容覆盖：

- `devops-engineer`

这部分强调：

- CI/CD
- 环境
- 部署
- 构建

#### C. 安全与认证类

由以下内容覆盖：

- `security-engineer`

这部分强调：

- 输入验证
- token / session
- TLS
- replay 防护
- 服务端验证关键状态

---

## 7. 当前模板对服务器后端“没有形成完整规则体系”

这是最重要的判断。

虽然有网络、安全、部署相关 Agent 和规则，但当前模板仍然**没有一套独立的、完整的服务器后端开发规则**。

### 具体缺失项

当前模板没有这些专门内容：

- `backend-engineer` 之类的专用 Agent
- `api-engineer` 之类的 API 设计 Agent
- `database-engineer` 之类的数据库 Agent
- `src/backend/**` 的路径规则
- `src/backend/api/**` 的 API 规则
- `db/**` 的 migration / schema 规则
- 缓存、消息队列、异步任务系统规范
- 服务配置、secret 管理、服务间边界的专门编码规范

也就是说：

> 它有“联机和服务端安全意识”，但还没有“通用后端工程规则体系”。

---

## 8. 对三消项目来说，现有模板够不够

这要看你的三消项目做多深。

## 8.1 如果你做的是单机或轻服务三消

例如：

- 本地关卡
- 本地存档
- 远程拉取配置
- 简单排行榜
- 基础埋点

那当前模板基本够用。

你主要依赖：

- `gameplay-programmer`
- `ui-programmer`
- `lead-programmer`
- `devops-engineer`
- `security-engineer`
- `analytics-engineer`

---

## 8.2 如果你做的是商业化服务型三消

例如：

- 登录
- 活动系统
- 支付校验
- 云存档
- 排行榜
- A/B 测试
- 配置中心
- 运营后台

那当前模板就不够完整了。

你需要额外补充一层“服务端规则体系”。

---

## 9. 如果要补服务器端体系，建议补什么

如果你未来要把这个模板扩展到完整三消服务端，建议至少补以下内容。

## 9.1 新 Agent

建议新增：

- `backend-engineer`
  - 通用服务实现
- `api-engineer`
  - API 设计、鉴权、错误码、版本控制
- `database-engineer`
  - schema、索引、事务、迁移

---

## 9.2 新规则文件

建议新增：

- `backend-code.md`
  - 对应 `src/backend/**`
- `api-code.md`
  - 对应 `src/backend/api/**`
- `database-rules.md`
  - 对应 `db/**` 或 `src/backend/db/**`

---

## 9.3 建议的后端规则内容

这些规则至少应该包含：

- 输入校验
- 鉴权与授权
- session/token 生命周期
- API versioning
- 幂等性
- 错误码规范
- 数据库 migration 和回滚
- 事务边界
- 索引约束
- 缓存一致性
- 日志、trace、监控
- rate limiting
- secret 管理
- 配置隔离

---

## 10. 对当前模板的最终判断

如果从“技术 Agent 和规则是否存在”来看，答案是：

- 有，而且结构清楚

如果从“是否覆盖游戏客户端实现”来看，答案是：

- 覆盖较完整

如果从“是否覆盖联机、部署和安全”来看，答案是：

- 有一部分明确支持

如果从“是否已经具备完整服务器后端工程规则体系”来看，答案是：

- 还没有

最准确的一句话总结是：

> 这套模板本质上是面向游戏客户端开发的工作室协作框架，已经具备网络、安全、部署方面的局部服务端意识，但尚未扩展成一套完整的后端工程模板。

---

## 11. 建议你现在怎么用

如果你当前主要目标是三消项目，建议这样使用：

### 当前阶段

把重点放在：

- 客户端玩法实现
- 客户端 UI
- 数据驱动关卡
- 原型验证

### 如果后面需要轻服务能力

再逐步引入：

- `analytics-engineer`
- `devops-engineer`
- `security-engineer`

### 如果后面明确要走重服务架构

再考虑扩展本模板的服务端 Agent 和规则，而不是现在就一次性把整套后端体系塞进去。
